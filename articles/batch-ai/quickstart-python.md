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
ms.date: 06/18/2018
ms.author: danlep
ms.openlocfilehash: 2ae0220b5240dc4a6e6d70056956140feb8153c4
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "43108526"
---
# <a name="run-a-cntk-training-job-using-the-azure-python-sdk"></a>Ausführen eines CNTK-Trainingsauftrags über das Azure Python SDK

In dieser Schnellstartanleitung wird beschrieben, wie Sie das Python SDK zum Ausführen eines CNTK-Trainingsauftrags (Microsoft Cognitive Toolkit) über den Batch AI-Dienst verwenden.

In diesem Beispiel verwenden Sie die MNIST-Datenbank mit handschriftlichen Bildern, um ein künstliches neuronales Netz (Convolutional Neural Network, CNN) in einem GPU-Cluster mit nur einem Knoten zu trainieren.

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement: Falls Sie nicht über ein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

* Azure Python SDK: [Installationsanleitung](/python/azure/python-sdk-azure-install). Für diesen Artikel ist mindestens Paketversion 2.0.0 von „azure-mgmt-batchai“ erforderlich.

* Azure-Speicherkonto: [Erstellen eines Azure-Speicherkontos](../storage/common/storage-create-storage-account.md)

* Anmeldeinformationen für Azure Active Directory-Dienstprinzipal: [Erstellen eines Dienstprinzipals mit der CLI](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md)

* Registrieren Sie den Batch AI-Ressourcenanbieter einmal für Ihr Abonnement. Verwenden Sie hierzu entweder Azure Cloud Shell oder die Azure-Befehlszeilenschnittstelle. Eine Anbieterregistrierung kann bis zu 15 Minuten dauern.

```azurecli
az provider register -n Microsoft.BatchAI
```

## <a name="configure-credentials"></a>Konfigurieren von Anmeldeinformationen
Fügen Sie Ihrer Skriptdatei den folgenden Code hinzu, und ersetzen Sie `FILL-IN-HERE` durch geeignete Werte:

```Python
# credentials used for authentication
aad_client_id = 'FILL-IN-HERE'
aad_secret = 'FILL-IN-HERE'
aad_tenant = 'FILL-IN-HERE'
subscription_id = 'FILL-IN-HERE'

# credentials used for storage
storage_account_name = 'FILL-IN-HERE'
storage_account_key = 'FILL-IN-HERE'

# specify the credentials used to remote login your GPU node
admin_user_name = 'FILL-IN-HERE'
admin_user_password = 'FILL-IN-HERE'

# specify the location in which to create Batch AI resources
mylocation = 'eastus'
```

Anmeldeinformationen sollten normalerweise nicht in den Quellcode eingefügt werden. Diese Vorgehensweise wird hier lediglich zur Vereinfachung der Schnellstartanleitung verwendet.
Verwenden Sie stattdessen Umgebungsvariablen oder eine separate Konfigurationsdatei.

## <a name="create-batch-ai-client"></a>Erstellen des Batch AI-Clients

Der folgende Code erstellt ein Dienstprinzipal-Anmeldeinformationsobjekt sowie einen Batch AI-Client:

```Python
from azure.common.credentials import ServicePrincipalCredentials
import azure.mgmt.batchai as batchai
import azure.mgmt.batchai.models as models

creds = ServicePrincipalCredentials(
        client_id=aad_client_id, secret=aad_secret, tenant=aad_tenant)

batchai_client = batchai.BatchAIManagementClient(
    credentials=creds, subscription_id=subscription_id)
```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Batch AI-Cluster und -Aufträge sind Azure-Ressourcen, die in einer Azure-Ressourcengruppe angeordnet werden müssen. Mit dem folgenden Codeausschnitt wird eine Ressourcengruppe erstellt:

```Python
from azure.mgmt.resource import ResourceManagementClient

resource_group_name = 'myresourcegroup'
resource_management_client = ResourceManagementClient(
        credentials=creds, subscription_id=subscription_id)
resource = resource_management_client.resource_groups.create_or_update(
        resource_group_name, {'location': mylocation})
```


## <a name="prepare-azure-file-share"></a>Vorbereiten einer Azure-Dateifreigabe
Zur Veranschaulichung werden die Trainingsdaten und die Skripts für den Trainingsauftrag in dieser Schnellstartanleitung mithilfe einer Azure-Dateifreigabe gehostet.

Erstellen Sie eine Dateifreigabe namens `batchaiquickstart`.

```Python
from azure.storage.file import FileService
azure_file_share_name = 'batchaiquickstart'
service = FileService(storage_account_name, storage_account_key)
service.create_share(azure_file_share_name, fail_on_exist=False)
```

Erstellen Sie in der Freigabe ein Verzeichnis namens `mnistcntksample`.

```Python
mnist_dataset_directory = 'mnistcntksample'
service.create_directory(azure_file_share_name, mnist_dataset_directory, fail_on_exist=False)
```
Laden Sie das [Beispielpaket](https://batchaisamples.blob.core.windows.net/samples/BatchAIQuickStart.zip?st=2017-09-29T18%3A29%3A00Z&se=2099-12-31T08%3A00%3A00Z&sp=rl&sv=2016-05-31&sr=b&sig=hrAZfbZC%2BQ%2FKccFQZ7OC4b%2FXSzCF5Myi4Cj%2BW3sVZDo%3D) herunter, und entzippen Sie es im aktuellen Verzeichnis. Der folgende Code lädt die erforderlichen Dateien in die Azure-Dateifreigabe hoch:

```Python
for f in ['Train-28x28_cntk_text.txt', 'Test-28x28_cntk_text.txt',
          'ConvNet_MNIST.py']:
     service.create_file_from_path(
             azure_file_share_name, mnist_dataset_directory, f, f)
```

## <a name="create-batch-ai-workspace"></a>Erstellen eines Batch AI-Arbeitsbereichs

Ein Arbeitsbereich ist eine allgemeine Sammlung aller Typen von Batch AI-Ressourcen. Sie erstellen Batch AI-Cluster und -Experimente in einem Arbeitsbereich.

```Python
workspace_name='myworkspace'
batchai_client.workspaces.create(resource_group_name, workspace_name, mylocation)
```

## <a name="create-gpu-cluster"></a>Erstellen des GPU-Clusters

Erstellen Sie einen Batch AI-Cluster. In diesem Beispiel besteht der Cluster aus einem einzelnen STANDARD_NC6-VM-Knoten. Diese VM verfügt über eine NVIDIA K80 GPU. Binden Sie die Dateifreigabe unter einem Ordner namens `azurefileshare` ein. Der vollständige Pfad dieses Ordners auf dem GPU-Computeknoten lautet `$AZ_BATCHAI_MOUNT_ROOT/azurefileshare`.

```Python
cluster_name = 'mycluster'

relative_mount_point = 'azurefileshare'

parameters = models.ClusterCreateParameters(
    # VM size. Use N-series for GPU
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
                    azure_file_url='https://{0}/{1}'.format(
                        service.primary_endpoint, azure_file_share_name),
                    relative_mount_path=relative_mount_point)],
        ),
    ),
)
batchai_client.clusters.create(resource_group_name, workspace_name, cluster_name,
                               parameters).result()
```

## <a name="get-cluster-status"></a>Abrufen des Clusterstatus

Überwachen Sie den Clusterstatus mit dem folgenden Befehl:

```Python
cluster = batchai_client.clusters.get(resource_group_name, workspace_name, cluster_name)
print('Cluster state: {0} Target: {1}; Allocated: {2}; Idle: {3}; '
      'Unusable: {4}; Running: {5}; Preparing: {6}; Leaving: {7}'.format(
    cluster.allocation_state,
    cluster.scale_settings.manual.target_node_count,
    cluster.current_node_count,
    cluster.node_state_counts.idle_node_count,
    cluster.node_state_counts.unusable_node_count,
    cluster.node_state_counts.running_node_count,
    cluster.node_state_counts.preparing_node_count,
    cluster.node_state_counts.leaving_node_count))
```

Mit dem obigen Code werden grundlegende Informationen zur Clusterzuordnung ausgegeben, wie im folgenden Beispiel zu sehen:

```
Cluster state: AllocationState.steady Target: 1; Allocated: 1; Idle: 0; Unusable: 0; Running: 0; Preparing: 1; Leaving: 0
```

Der Cluster ist bereit, wenn die Knoten zugeordnet wurden und die Vorbereitung (siehe `nodeStateCounts`-Attribut) abgeschlossen ist. Wenn ein Fehler aufgetreten ist, enthält das `errors`-Attribut die Fehlerbeschreibung.

## <a name="create-experiment-and-training-job"></a>Erstellen eines Experiments und eines Trainingsauftrags

Erstellen Sie nach der Clustererstellung ein Experiment (ein logischer Container für eine Gruppe verwandter Aufträge). Anschließend konfigurieren und übermitteln Sie einen Lernauftrag im Experiment:

```Python
experiment_name='myexperiment'

batchai_client.experiments.create(resource_group_name, workspace_name, experiment_name)

job_name = 'myjob'

parameters = models.JobCreateParameters(
    # The cluster this job will run on
    cluster=models.ResourceId(id=cluster.id),
    # The number of VMs in the cluster to use
    node_count=1,
    # Write job's standard output and execution log to Azure File Share
    std_out_err_path_prefix='$AZ_BATCHAI_MOUNT_ROOT/{0}'.format(
        relative_mount_point),
    # Configure location of the training script and MNIST dataset
    input_directories=[models.InputDirectory(
        id='SAMPLE',
        path='$AZ_BATCHAI_MOUNT_ROOT/{0}/{1}'.format(
            relative_mount_point, mnist_dataset_directory))],
    # Specify location where generated model will be stored
    output_directories=[models.OutputDirectory(
        id='MODEL',
        path_prefix='$AZ_BATCHAI_MOUNT_ROOT/{0}'.format(relative_mount_point),
        path_suffix="Models")],
    # Container configuration
    container_settings=models.ContainerSettings(
        image_source_registry=models.ImageSourceRegistry(
            image='microsoft/cntk:2.1-gpu-python3.5-cuda8.0-cudnn6.0')),
    # Toolkit specific settings
    cntk_settings=models.CNTKsettings(
        python_script_file_path='$AZ_BATCHAI_INPUT_SAMPLE/ConvNet_MNIST.py',
        command_line_args='$AZ_BATCHAI_INPUT_SAMPLE $AZ_BATCHAI_OUTPUT_MODEL')
)

# Create the job
batchai_client.jobs.create(resource_group_name, workspace_name, experiment_name, job_name, parameters).result()
```

## <a name="monitor-job"></a>Überwachen des Auftrags
Den Status des Auftrags können Sie mithilfe des folgenden Codes ermitteln:

```Python
job = batchai_client.jobs.get(resource_group_name, workspace_name, experiment_name, job_name)

print('Job state: {0} '.format(job.execution_state))
```

Die Ausgabe lautet in etwa wie folgt: `Job state: running`.

`executionState` enthält den aktuellen Ausführungsstatus des Auftrags:
* `queued`: Der Auftrag wartet darauf, dass die Clusterknoten verfügbar sind.
* `running`: Der Auftrag wird ausgeführt.
* `succeeded` (oder `failed`): Der Auftrag ist abgeschlossen. `executionInfo` enthält Details zum Ergebnis.

## <a name="list-stdout-and-stderr-output"></a>Auflisten der stdout- und stderr-Ausgabe
Verwenden Sie den folgenden Code, um die generierten STDOUT-, STDERR- und LOG-Dateien aufzulisten:

```Python
files = batchai_client.jobs.list_output_files(
    resource_group_name, workspace_name, experiment_name, job_name,
    models.JobsListOutputFilesOptions(outputdirectoryid="stdouterr"))

for file in (f for f in files if f.download_url):
    print('file: {0}, download url: {1}'.format(file.name, file.download_url))
```

## <a name="list-generated-model-files"></a>Auflisten der generierten Modelldateien
Verwenden Sie den folgenden Code, um die generierten Modelldateien aufzulisten:
```Python
files = batchai_client.jobs.list_output_files(
    resource_group_name, workspace_name, experiment_name,job_name,
    models.JobsListOutputFilesOptions(outputdirectoryid="MODEL"))

for file in (f for f in files if f.download_url):
    print('file: {0}, download url: {1}'.format(file.name, file.download_url))
```

## <a name="delete-resources"></a>Löschen von Ressourcen

Verwenden Sie den folgenden Code, um den Auftrag zu löschen:
```Python
batchai_client.jobs.delete(resource_group_name, workspace_name, experiment_name, job_name)
```

Verwenden Sie den folgenden Code, um den Cluster zu löschen:
```Python
batchai_client.clusters.delete(resource_group_name, workspace_name, cluster_name)
```

Verwenden Sie den folgenden Code, um alle zugeordneten Ressourcen zu löschen:
```Python
resource_management_client.resource_groups.delete('myresourcegroup')
```
## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung wurde beschrieben, wie Sie einen CNTK-Trainingsauftrag in einem Batch AI-Cluster mit dem Azure Python SDK ausführen. Weitere Informationen zur Verwendung von Batch AI mit verschiedenen Toolkits finden Sie in den [Trainingsrezepten](https://github.com/Azure/BatchAI).
