---
title: Azure-Schnellstart – CNTK-Training mit Batch AI – Python | Microsoft-Dokumentation
description: Es wird in kompakter Form beschrieben, wie Sie einen CNTK-Trainingsauftrag mit Batch AI über das Python SDK ausführen.
services: batch-ai
documentationcenter: na
author: lliimsft
manager: Vaman.Bedekar
editor: tysonn
ms.assetid: ''
ms.custom: ''
ms.service: batch-ai
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: Python
ms.topic: quickstart
ms.date: 10/06/2017
ms.author: lili
ms.openlocfilehash: f535c9adf4926f29ae9cade6382debedab73937d
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="run-a-cntk-training-job-using-the-azure-python-sdk"></a>Ausführen eines CNTK-Trainingsauftrags über das Azure Python SDK

In dieser Schnellstartanleitung wird beschrieben, wie Sie das Python SDK zum Ausführen eines CNTK-Trainingsauftrags (Microsoft Cognitive Toolkit) über den Batch AI-Dienst verwenden. 

In diesem Beispiel verwenden Sie die MNIST-Datenbank mit handschriftlichen Bildern, um ein künstliches neuronales Netz (Convolutional Neural Network, CNN) in einem GPU-Cluster mit nur einem Knoten zu trainieren. 

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement: Falls Sie nicht über ein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen. 

* Azure Python SDK: [Installationsanleitung](/python/azure/python-sdk-azure-install)

* Azure-Speicherkonto: [Erstellen eines Azure-Speicherkontos](../storage/common/storage-create-storage-account.md)

* Anmeldeinformationen für Azure Active Directory-Dienstprinzipal: [Erstellen eines Dienstprinzipals mit der CLI](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md)

* Registrieren Sie die Batch AI-Ressourcenanbieter einmal für Ihr Abonnement, indem Sie Azure Cloud Shell oder die Azure CLI verwenden. Eine Anbieterregistrierung kann bis zu 15 Minuten dauern.

  ```azurecli
  az provider register -n Microsoft.BatchAI
  az provider register -n Microsoft.Batch
  ```


## <a name="configure-credentials"></a>Konfigurieren von Anmeldeinformationen
Erstellen Sie diese Parameter in Ihrem Python-Skript, und fügen Sie Ihre eigenen Werte ein:

```Python
# credentials used for authentication
client_id = 'my_aad_client_id'
secret = 'my_aad_secret_key'
token_uri = 'my_aad_token_uri'
subscription_id = 'my_subscription_id'

# credentials used for storage
storage_account_name = 'my_storage_account_name'
storage_account_key = 'my_storage_account_key'

# specify the credentials used to remote login your GPU node
admin_user_name = 'my_admin_user_name'
admin_user_password = 'my_admin_user_password'
```

Die bewährte Methode besteht darin, alle Anmeldeinformationen in einer separaten Konfigurationsdatei zu speichern. Dies wird in diesem Beispiel nicht veranschaulicht. Informationen zum Implementieren einer Konfigurationsdatei finden Sie in den entsprechenden [Rezepten](https://github.com/Azure/BatchAI/tree/master/recipes). 

## <a name="authenticate-with-batch-ai"></a>Authentifizieren mit Batch AI

Um auf Azure Batch AI zugreifen zu können, müssen Sie die Authentifizierung mit Azure Active Directory durchführen. Unten ist Code für die Authentifizierung beim Dienst (`BatchAIManagementClient`-Objekt erstellen) mit Ihren Dienstprinzipal-Anmeldeinformationen und der Abonnement-ID angegeben:

```Python
from azure.common.credentials import ServicePrincipalCredentials
import azure.mgmt.batchai as batchai
import azure.mgmt.batchai.models as models

creds = ServicePrincipalCredentials(
        client_id=client_id, secret=secret, token_uri=token_uri)

batchai_client = batchai.BatchAIManagementClient(credentials=creds,
                                         subscription_id=subscription_id
)
```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Batch AI-Cluster und -Aufträge sind Azure-Ressourcen, die in einer Azure-Ressourcengruppe angeordnet werden müssen. Mit dem folgenden Codeausschnitt wird eine Ressourcengruppe erstellt:

```Python
from azure.mgmt.resource import ResourceManagementClient

resource_group_name = 'myresourcegroup'

resource_management_client = ResourceManagementClient(
        credentials=creds, subscription_id=subscription_id)

resource = resource_management_client.resource_groups.create_or_update(
        resource_group_name, {'location': 'eastus'})
```


## <a name="prepare-azure-file-share"></a>Vorbereiten einer Azure-Dateifreigabe
Zu Darstellungszwecken wird in dieser Schnellstartanleitung eine Azure-Dateifreigabe verwendet, um die Trainingsdaten und Skripts für den Lernauftrag zu hosten. 

1. Erstellen Sie eine Dateifreigabe mit dem Namen *batchaiquickstart*.

  ```Python
  from azure.storage.file import FileService 
 
  azure_file_share_name = 'batchaiquickstart' 
 
  service = FileService(storage_account_name, storage_account_key) 
 
  service.create_share(azure_file_share_name, fail_on_exist=False)
  ``` 
 
2. Erstellen Sie auf der Freigabe ein Verzeichnis mit dem Namen *mnistcntksample*. 

  ```Python
  mnist_dataset_directory = 'mnistcntksample' 
 
  service.create_directory(azure_file_share_name, mnist_dataset_directory, fail_on_exist=False) 
  ```
3. Laden Sie das [Beispielpaket](https://batchaisamples.blob.core.windows.net/samples/BatchAIQuickStart.zip?st=2017-09-29T18%3A29%3A00Z&se=2099-12-31T08%3A00%3A00Z&sp=rl&sv=2016-05-31&sr=b&sig=hrAZfbZC%2BQ%2FKccFQZ7OC4b%2FXSzCF5Myi4Cj%2BW3sVZDo%3D) herunter, und entzippen Sie es. Laden Sie den Inhalt in das Verzeichnis hoch, in dem Sie das Python-Skript ausführen möchten.

  ```Python
  for f in ['Train-28x28_cntk_text.txt', 'Test-28x28_cntk_text.txt', 'ConvNet_MNIST.py']:     
     service.create_file_from_path(
             azure_file_share_name, mnist_dataset_directory, f, f) 
  ```

## <a name="create-gpu-cluster"></a>Erstellen des GPU-Clusters

Erstellen Sie einen Batch AI-Cluster. In diesem Beispiel besteht der Cluster aus einem einzelnen STANDARD_NC6-VM-Knoten. Diese VM verfügt über eine NVIDIA K80 GPU. Stellen Sie die Dateifreigabe in einem Ordner mit dem Namen *azurefileshare* bereit. Der vollständige Pfad dieses Ordners auf dem GPU-Computeknoten lautet „$AZ_BATCHAI_MOUNT_ROOT/azurefileshare“.

```Python
cluster_name = 'mycluster'

relative_mount_point = 'azurefileshare' 
 
parameters = models.ClusterCreateParameters(
    # Location where the cluster will physically be deployed
    location='eastus', 
 
    # VM size. Use NC or NV series for GPU
    vm_size='STANDARD_NC6', 
 
    # Configure the ssh users
    user_account_settings=models.UserAccountSettings(
         admin_user_name=admin_user_name,
         admin_user_password=admin_user_password), 
 
    # Number of VMs in the cluster
    scale_settings=models.ScaleSettings(
         manual=models.ManualScaleSettings(target_node_count=1)
     ), 
 
    # Configure each node in the cluster
    node_setup=models.NodeSetup( 
 
        # Mount shared volumes to the host
         mount_volumes=models.MountVolumes(
             azure_file_shares=[
                 models.AzureFileShareReference(
                     account_name=storage_account_name,
                     credentials=models.AzureStorageCredentialsInfo(
         account_key=storage_account_key),
         azure_file_url='https://{0}.file.core.windows.net/{1}'.format(
               storage_account_name, azure_file_share_name),
                  relative_mount_path = relative_mount_point)],
         ), 
    ), 
) 
batchai_client.clusters.create(resource_group_name, cluster_name, parameters).result() 
```

## <a name="get-cluster-status"></a>Abrufen des Clusterstatus

Überwachen Sie den Clusterstatus mit dem folgenden Befehl: 

```Python
cluster = batchai_client.clusters.get(resource_group_name, cluster_name)
print('Cluster state: {0} Target: {1}; Allocated: {2}; Idle: {3}; '
      'Unusable: {4}; Running: {5}; Preparing: {6}; leaving: {7}'.format(
    cluster.allocation_state,
    cluster.scale_settings.manual.target_node_count,
    cluster.current_node_count,
    cluster.node_state_counts.idle_node_count,
    cluster.node_state_counts.unusable_node_count,
    cluster.node_state_counts.running_node_count,
    cluster.node_state_counts.preparing_node_count,
    cluster.node_state_counts.leaving_node_count)) 
```

Mit dem obigen Code werden grundlegende Informationen zur Clusterzuordnung ausgegeben:

```Shell
Cluster state: AllocationState.steady Target: 1; Allocated: 1; Idle: 0; Unusable: 0; Running: 0; Preparing: 1; Leaving 0
 
```  

Der Cluster ist bereit, wenn die Knoten zugeordnet wurden und die Vorbereitung (siehe `nodeStateCounts`-Attribut) abgeschlossen ist. Wenn ein Fehler aufgetreten ist, enthält das `errors`-Attribut die Fehlerbeschreibung.

## <a name="create-training-job"></a>Erstellen des Trainingsauftrags

Wenn der Cluster bereit ist, können Sie den Lernauftrag konfigurieren und übermitteln. 

```Python
job_name = 'myjob' 
 
parameters = models.job_create_parameters.JobCreateParameters( 
 
     # Location where the job will run
     # Ideally this should be co-located with the cluster.
     location='eastus', 
 
     # The cluster this job will run on
     cluster=models.ResourceId(cluster.id), 
 
     # The number of VMs in the cluster to use
     node_count=1, 
 
     # Override the path where the std out and std err files will be written to.
     # In this case we will write these out to an Azure Files share
     std_out_err_path_prefix='$AZ_BATCHAI_MOUNT_ROOT/{0}'.format(relative_mount_point), 
 
     input_directories=[models.InputDirectory(
         id='SAMPLE',
         path='$AZ_BATCHAI_MOUNT_ROOT/{0}/{1}'.format(relative_mount_point, mnist_dataset_directory))], 
 
     # Specify directories where files will get written to 
     output_directories=[models.OutputDirectory(
        id='MODEL',
        path_prefix='$AZ_BATCHAI_MOUNT_ROOT/{0}'.format(relative_mount_point),
        path_suffix="Models")], 
 
     # Container configuration
     container_settings=models.ContainerSettings(
        models.ImageSourceRegistry(image='microsoft/cntk:2.1-gpu-python3.5-cuda8.0-cudnn6.0')), 
 
     # Toolkit specific settings
     cntk_settings = models.CNTKsettings(
        python_script_file_path='$AZ_BATCHAI_INPUT_SAMPLE/ConvNet_MNIST.py',
        command_line_args='$AZ_BATCHAI_INPUT_SAMPLE $AZ_BATCHAI_OUTPUT_MODEL')
 ) 
 
# Create the job 
batchai_client.jobs.create(resource_group_name, job_name, parameters).result() 
```

## <a name="monitor-job"></a>Überwachen des Auftrags
Sie können den Status des Auftrags mit dem folgenden Befehl untersuchen: 
 
```Python
job = batchai_client.jobs.get(resource_group_name, job_name) 
 
print('Job state: {0} '.format(job.execution_state.name))
```

Die Ausgabe lautet in etwa wie folgt: `Job state: running`.

`executionState` enthält den aktuellen Ausführungsstatus des Auftrags:
* `queued`: Der Auftrag wartet darauf, dass die Clusterknoten verfügbar sind.
* `running`: Der Auftrag wird ausgeführt.
* `succeeded` (oder `failed`): Der Auftrag ist abgeschlossen, und `executionInfo` enthält Details zum Ergebnis.
 
## <a name="list-stdout-and-stderr-output"></a>Auflisten der stdout- und stderr-Ausgabe
Verwenden Sie den folgenden Befehl, um die Links zu den stdout- und stderr-Protokolldateien aufzulisten:

```Python
files = batchai_client.jobs.list_output_files(resource_group_name, job_name, models.JobsListOutputFilesOptions("stdouterr")) 
 
for file in list(files):
     print('file: {0}, download url: {1}'.format(file.name, file.download_url)) 
```
## <a name="delete-resources"></a>Löschen von Ressourcen

Verwenden Sie den folgenden Befehl zum Löschen des Auftrags:
```Python
batchai_client.jobs.delete(resource_group_name, job_name) 
```

Verwenden Sie den folgenden Befehl zum Löschen des Clusters:
```Python
batchai_client.clusters.delete(resource_group_name, cluster_name)
```
## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung wurde beschrieben, wie Sie einen CNTK-Trainingsauftrag in einem Batch AI-Cluster mit dem Azure Python SDK ausführen. Weitere Informationen zur Verwendung von Batch AI mit verschiedenen Toolkits finden Sie in den [Trainingsrezepten](https://github.com/Azure/BatchAI).
