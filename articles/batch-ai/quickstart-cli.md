---
title: Azure-Schnellstart – CNTK-Training mit Batch AI – Azure CLI | Microsoft-Dokumentation
description: Es wird in kompakter Form beschrieben, wie Sie einen CNTK-Trainingsauftrag mit Batch AI über die Azure CLI ausführen.
services: batch-ai
documentationcenter: na
author: AlexanderYukhanov
manager: Vaman.Bedekar
editor: tysonn
ms.assetid: ''
ms.custom: ''
ms.service: batch-ai
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: CLI
ms.topic: quickstart
ms.date: 10/06/2017
ms.author: Alexander.Yukhanov
ms.openlocfilehash: 82e3885021a2f2309dfed456d472e7027b8d6cf2
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="run-a-cntk-training-job-using-the-azure-cli"></a>Ausführen eines CNTK-Trainingsauftrags über die Azure CLI

In dieser Schnellstartanleitung wird beschrieben, wie Sie die Azure-Befehlszeilenschnittstelle (CLI) zum Ausführen eines CNTK-Trainingsauftrags (Microsoft Cognitive Toolkit) über den Batch AI-Dienst verwenden. Die Azure CLI dient zum Erstellen und Verwalten von Azure-Ressourcen über die Befehlszeile oder mit Skripts.

In diesem Beispiel verwenden Sie die MNIST-Datenbank mit handschriftlichen Bildern, um ein künstliches neuronales Netz (Convolutional Neural Network, CNN) in einem GPU-Cluster mit nur einem Knoten zu trainieren, der mit Batch AI verwaltet wird. 

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

Für diese Schnellstartanleitung müssen Sie die aktuellste Version der Azure CLI ausführen. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu.

Außerdem müssen die Batch AI-Ressourcenanbieter über die Azure Cloud Shell oder Azure CLI einmal für Ihr Abonnement registriert werden. Eine Anbieterregistrierung kann bis zu 15 Minuten dauern.

```azurecli
az provider register -n Microsoft.BatchAI
az provider register -n Microsoft.Batch
```


## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Batch AI-Cluster und -Aufträge sind Azure-Ressourcen, die in einer Azure-Ressourcengruppe angeordnet werden müssen.

Erstellen Sie mit dem Befehl [az group create](/cli/azure/group#az_group_create) eine Ressourcengruppe.

Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *eastus*. Anschließend wird der Befehl [az configure](/cli/azure/reference-index#az_configure) verwendet, um diese Ressourcengruppe und den Standort als Standardeinstellung festzulegen.

```azurecli
az group create --name myResourceGroup --location eastus

az configure --defaults group=myResourceGroup

az configure --defaults location=eastus
```

>[!NOTE]
>Das Festlegen der Standardwerte für den Befehl `az` ist ein optionaler Schritt. Sie können sich auch dafür entscheiden, die Standardwerte nicht festzulegen. Falls Sie Standardwerte festlegen, sollten Sie diese nach Abschluss des Tutorials wieder entfernen. Entfernen Sie die Standardeinstellungen mit den folgenden Befehlen:
>
>```azurecli
>az configure --defaults group=''
>
>az configure --defaults location=''
>```
>

## <a name="create-a-storage-account"></a>Speicherkonto erstellen

In dieser Schnellstartanleitung werden ein Azure-Speicherkonto zum Hosten der Daten und Skripts für den Trainingsauftrag verwendet. Sie erstellen ein Speicherkonto mit dem Befehl [az storage account create](/cli/azure/storage/account#az_storage_account_create).

```azurecli
az storage account create --name mystorageaccount --sku Standard_LRS
```

>[!NOTE]
>Jedes Speicherkonto muss einen eindeutigen Namen haben. Ersetzen Sie im obigen `az`-Befehl und ähnlichen Befehlen in diesem Tutorial den Wert für die Einstellung `mystorageaccount` jeweils durch den Namen Ihres Speicherkontos.

## <a name="prepare-azure-file-share"></a>Vorbereiten einer Azure-Dateifreigabe

Zu Darstellungszwecken wird in dieser Schnellstartanleitung eine Azure-Dateifreigabe verwendet, um die Trainingsdaten und Skripts für den Lernauftrag zu hosten.

1. Erstellen Sie eine Dateifreigabe mit dem Namen *batchaiquickstart*, indem Sie den Befehl [az storage share create](/cli/azure/storage/share#az_storage_share_create) verwenden.

  ```azurecli
  az storage share create --account-name mystorageaccount --name batchaiquickstart
  ```
2. Erstellen Sie auf der Freigabe ein Verzeichnis mit dem Namen *mnistcntksample*, indem Sie den Befehl [az storage directory create](/cli/azure/storage/directory#az_storage_directory_create) verwenden.

  ```azurecli
  az storage directory create --share-name batchaiquickstart  --name mnistcntksample
  ```

3. Laden Sie das [Beispielpaket](https://batchaisamples.blob.core.windows.net/samples/BatchAIQuickStart.zip?st=2017-09-29T18%3A29%3A00Z&se=2099-12-31T08%3A00%3A00Z&sp=rl&sv=2016-05-31&sr=b&sig=hrAZfbZC%2BQ%2FKccFQZ7OC4b%2FXSzCF5Myi4Cj%2BW3sVZDo%3D) herunter, und entzippen Sie es. Laden Sie den Inhalt in das Verzeichnis hoch, indem Sie den Befehl [az storage file upload](/cli/azure/storage/file#az_storage_file_upload) verwenden:

  ```azurecli
  az storage file upload --share-name batchaiquickstart --source Train-28x28_cntk_text.txt --path mnistcntksample

  az storage file upload --share-name batchaiquickstart --source Test-28x28_cntk_text.txt --path mnistcntksample

  az storage file upload --share-name batchaiquickstart --source ConvNet_MNIST.py --path mnistcntksample
  ```


## <a name="create-gpu-cluster"></a>Erstellen des GPU-Clusters
Verwenden Sie den Befehl [az batchai cluster create](/cli/azure/batchai/cluster#az_batchai_cluster_create), um einen Batch AI-Cluster mit einem einzelnen GPU-VM-Knoten zu erstellen. In diesem Beispiel wird auf dem virtuellen Computer das Ubuntu LTS-Standardimage ausgeführt. Geben Sie stattdessen `image UbuntuDSVM` an, um die Microsoft Deep Learning Virtual Machine auszuführen, die zusätzliche Trainingsframeworks unterstützt. Die Größe NC6 verfügt über eine NVIDIA K80 GPU. Stellen Sie die Dateifreigabe in einem Ordner mit dem Namen *azurefileshare* bereit. Der vollständige Pfad dieses Ordners auf dem GPU-Computeknoten lautet „$AZ_BATCHAI_MOUNT_ROOT/azurefileshare“.


```azurecli
az batchai cluster create --name mycluster --vm-size STANDARD_NC6 --image UbuntuLTS --min 1 --max 1 --storage-account-name mystorageaccount --afs-name batchaiquickstart --afs-mount-path azurefileshare --user-name <admin_username> --password <admin_password>
```


Nachdem der Cluster erstellt wurde, wird in etwa folgende Ausgabe angezeigt:

```azurecli
{
  "allocationState": "resizing",
  "allocationStateTransitionTime": "2017-10-05T02:09:03.194000+00:00",
  "creationTime": "2017-10-05T02:09:01.998000+00:00",
  "currentNodeCount": 0,
  "errors": null,
  "id": "/subscriptions/10d0b7c6-9243-4713-xxxx-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.BatchAI/clusters/mycluster",
  "location": "eastus",
  "name": "mycluster",
  "nodeSetup": {
    "mountVolumes": {
      "azureBlobFileSystems": null,
      "azureFileShares": [
        {
          "accountName": "batchaisamples",
          "azureFileUrl": "https://batchaisamples.file.core.windows.net/batchaiquickstart",
          "credentialsInfo": {
            "accountKey": null,
            "accountKeySecretUrl": null
          },
          "directoryMode": "0777",
          "fileMode": "0777",
          "relativeMountPath": "azurefileshare"
        }
      ],
      "fileServers": null,
      "unmanagedFileSystems": null
    },
    "setupTask": null
  },
  "nodeStateCounts": {
    "idleNodeCount": 0,
    "leavingNodeCount": 0,
    "preparingNodeCount": 0,
    "runningNodeCount": 0,
    "unusableNodeCount": 0
  },
  "provisioningState": "succeeded",
  "provisioningStateTransitionTime": "2017-10-05T02:09:02.857000+00:00",
  "resourceGroup": "myresourcegroup",
  "scaleSettings": {
    "autoScale": null,
    "manual": {
      "nodeDeallocationOption": "requeue",
      "targetNodeCount": 1
    }
  },
  "subnet": {
    "id": null
  },
  "tags": null,
  "type": "Microsoft.BatchAI/Clusters",
  "userAccountSettings": {
    "adminUserName": "demoUser",
    "adminUserPassword": null,
    "adminUserSshPublicKey": null
  },
  "virtualMachineConfiguration": {
    "imageReference": {
      "offer": "UbuntuServer",
      "publisher": "Canonical",
      "sku": "16.04-LTS",
      "version": "latest"
    }
  },
  "vmPriority": "dedicated",
  "vmSize": "STANDARD_NC6"
```
## <a name="get-cluster-status"></a>Abrufen des Clusterstatus

Führen Sie den Befehl [az batchai cluster list](/cli/azure/batchai/cluster#az_batchai_cluster_list) aus, um eine Übersicht über den Clusterstatus zu erhalten:

```azurecli
az batchai cluster list -o table
```

Die Ausgabe sieht in etwa wie folgt aus:

```azurecli
Name        Resource Group    VM Size        State     Idle    Running    Preparing    Unusable    Leaving
---------   ----------------  -------------  -------   ------  ---------  -----------  ----------  --------
mycluster   myresourcegroup   STANDARD_NC6   steady    1       0          0            0            0
```

Weitere Details erhalten Sie, wenn Sie den Befehl [az batchai cluster show](/cli/azure/batchai/cluster#az_batchai_cluster_show) ausführen. Es werden alle Clustereigenschaften zurückgegeben, die nach der Erstellung des Clusters angezeigt werden.

Der Cluster ist bereit, wenn die Knoten zugeordnet wurden und die Vorbereitung (siehe `nodeStateCounts`-Attribut) abgeschlossen ist. Wenn ein Fehler aufgetreten ist, enthält das `errors`-Attribut die Fehlerbeschreibung.

## <a name="create-training-job"></a>Erstellen des Trainingsauftrags

Wenn der Cluster bereit ist, können Sie den Lernauftrag konfigurieren und übermitteln.

1. Erstellen Sie für die Auftragserstellung eine JSON-Vorlagendatei mit dem Namen „job.json“:

  ```JSON
  {
    "properties": {
        "stdOutErrPathPrefix": "$AZ_BATCHAI_MOUNT_ROOT/azurefileshare",
       "inputDirectories": [{
            "id": "SAMPLE",
            "path": "$AZ_BATCHAI_MOUNT_ROOT/azurefileshare/mnistcntksample"
        }],
        "outputDirectories": [{
            "id": "MODEL",
            "pathPrefix": "$AZ_BATCHAI_MOUNT_ROOT/azurefileshare",
            "pathSuffix": "model",
            "type": "custom"
        }],
        "containerSettings": {
            "imageSourceRegistry": {
                "image": "microsoft/cntk:2.1-gpu-python3.5-cuda8.0-cudnn6.0"
            }
        },
        "nodeCount": 1,
        "cntkSettings": {
            "pythonScriptFilePath": "$AZ_BATCHAI_INPUT_SAMPLE/ConvNet_MNIST.py",
            "commandLineArgs": "$AZ_BATCHAI_INPUT_SAMPLE $AZ_BATCHAI_OUTPUT_MODEL"
        }
    }
  }
  ```
2. Erstellen Sie einen Auftrag mit dem Namen *myjob* zum Ausführen des Clusters, indem Sie den Befehl [az batchai job create](/cli/azure/batchai/job#az_batchai_job_create) verwenden:

  ```azurecli
  az batchai job create --name myjob --cluster-name mycluster --config job.json
  ```

Die Ausgabe sieht in etwa wie folgt aus:

```azurecli
{
  "caffeSettings": null,
  "chainerSettings": null,
  "cluster": {
    "id": "/subscriptions/10d0b7c6-9243-4713-xxxx-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.BatchAI/clusters/mycluster",
    "resourceGroup": "myresourcegroup"
  },
  "cntkSettings": {
    "commandLineArgs": "$AZ_BATCHAI_INPUT_SAMPLE $AZ_BATCHAI_OUTPUT_MODEL",
    "configFilePath": null,
    "languageType": "Python",
    "processCount": 1,
    "pythonInterpreterPath": null,
    "pythonScriptFilePath": "$AZ_BATCHAI_INPUT_SAMPLE/ConvNet_MNIST.py"
  },
  "constraints": {
    "maxTaskRetryCount": null,
    "maxWallClockTime": "7 days, 0:00:00"
  },
  "containerSettings": {
    "imageSourceRegistry": {
      "credentials": null,
      "image": "microsoft/cntk:2.1-gpu-python3.5-cuda8.0-cudnn6.0",
      "serverUrl": null
    }
  },
  "creationTime": "2017-10-05T06:41:42.163000+00:00",
  "customToolkitSettings": null,
  "environmentVariables": null,
  "executionInfo": {
    "endTime": null,
    "errors": null,
    "exitCode": null,
    "lastRetryTime": null,
    "retryCount": null,
    "startTime": "2017-10-05T06:41:44.392000+00:00"
  },
  "executionState": "running",
  "executionStateTransitionTime": "2017-10-05T06:41:44.953000+00:00",
  "experimentName": null,
  "id": "/subscriptions/10d0b7c6-9243-4713-xxxx-xxxxxxxxxxxx/resourceGroups/demo/providers/Microsoft.BatchAI/jobs/myjob",
  "inputDirectories": [
    {
      "id": "SAMPLE",
      "path": "$AZ_BATCHAI_MOUNT_ROOT/azurefileshare/mnistcntksample"
    }
  ],
  "jobPreparation": null,
  "location": null,
  "name": "cntk_job",
  "nodeCount": 1,
  "outputDirectories": [
    {
      "createNew": true,
      "id": "MODEL",
      "pathPrefix": "$AZ_BATCHAI_MOUNT_ROOT/azurefileshare",
      "pathSuffix": "model",
      "type": "Custom"
    }
  ],
  "priority": 0,
  "provisioningState": "succeeded",
  "provisioningStateTransitionTime": "2017-10-05T06:41:44.238000+00:00",
  "resourceGroup": "demo",
  "stdOutErrPathPrefix": "$AZ_BATCHAI_MOUNT_ROOT/azurefileshare",
  "tags": null,
  "tensorFlowSettings": null,
  "toolType": "CNTK",
  "type": "Microsoft.BatchAI/Jobs"
}
```

## <a name="monitor-job"></a>Überwachen des Auftrags

Verwenden Sie den Befehl [az batchai job list](/cli/azure/batchai/job#az_batchai_job_list), um eine Übersicht über den Auftragsstatus anzuzeigen:

```azurecli
az batchai job list -o table
```

Die Ausgabe sieht in etwa wie folgt aus:

```azurecli
Name        Resource Group    Cluster    Cluster RG      Nodes  State    Exit code
----------  ----------------  ---------  --------------- -----  -------  -----------
myjob       myresourcegroup   mycluster  myresourcegroup 1      running

```

Weitere Details erhalten Sie, wenn Sie den Befehl [az batchai job show](/cli/azure/batchai/job#az_batchai_job_show) ausführen.

`executionState` enthält den aktuellen Ausführungsstatus des Auftrags:

* `queued`: Der Auftrag wartet darauf, dass die Clusterknoten verfügbar sind.
* `running`: Der Auftrag wird ausgeführt.
*   `succeeded` (oder `failed`): Der Auftrag ist abgeschlossen, und `executionInfo` enthält Details zum Ergebnis.


## <a name="list-stdout-and-stderr-output"></a>Auflisten der stdout- und stderr-Ausgabe
Verwenden Sie den Befehl [az batchai job list-files](/cli/azure/batchai/job#az_batchai_job_list_files), um die Links zu den stdout- und stderr-Protokolldateien aufzulisten:

```azurecli
az batchai job list-files --name myjob --output-directory-id stdouterr
```

Die Ausgabe sieht in etwa wie folgt aus:

```azurecli
[
  {
    "contentLength": 733,
    "downloadUrl": "https://batchaisamples.file.core.windows.net/batchaiquickstart/10d0b7c6-9243-4713-91a9-2730375d3a1b/demo/jobs/cntk_job/stderr.txt?sv=2016-05-31&sr=f&sig=Rh%2BuTg9C1yQxm7NfA9YWiKb%2B5FRKqWmEXiGNRDeFMd8%3D&se=2017-10-05T07%3A44%3A38Z&sp=rl",
    "lastModified": "2017-10-05T06:44:38+00:00",
    "name": "stderr.txt"
  },
  {
    "contentLength": 300,
    "downloadUrl": "https://batchaisamples.file.core.windows.net/batchaiquickstart/10d0b7c6-9243-4713-91a9-2730375d3a1b/demo/jobs/cntk_job/stdout.txt?sv=2016-05-31&sr=f&sig=jMhJfQOGry9jr4Hh3YyUFpW5Uaxnp38bhVWNrTTWMtk%3D&se=2017-10-05T07%3A44%3A38Z&sp=rl",
    "lastModified": "2017-10-05T06:44:29+00:00",
    "name": "stdout.txt"
  }
]
```


## <a name="observe-output"></a>Anzeigen der Ausgabe

Sie können die Ausgabedateien eines Auftrags streamen oder „tailen“, während der Auftrag ausgeführt wird. Im folgenden Beispiel wird der Befehl [az batchai job stream-file](/cli/azure/batchai/job#az_batchai_job_stream_file) verwendet, um das Protokoll „stderr.txt“ zu streamen:

```azurecli
az batchai job stream-file --job-name myjob --output-directory-id stdouterr --name stderr.txt
```

Die Ausgabe sieht in etwa wie folgt aus. Unterbrechen Sie die Ausgabe, indem Sie [STRG]+[C] drücken.

```azurecli
…
Finished Epoch[2 of 40]: [Training] loss = 0.104846 * 60000, metric = 3.00% * 60000 3.849s (15588.5 samples/s);
Finished Epoch[3 of 40]: [Training] loss = 0.077043 * 60000, metric = 2.23% * 60000 3.902s (15376.7 samples/s);
Finished Epoch[4 of 40]: [Training] loss = 0.063050 * 60000, metric = 1.82% * 60000 3.811s (15743.9 samples/s);
…

```

## <a name="delete-resources"></a>Löschen von Ressourcen

Verwenden Sie den Befehl [az batchai job delete](/cli/azure/batchai/job#az_batchai_job_delete), um den Auftrag zu löschen:

```azurecli
az batchai job delete --name myjob
```
Verwenden Sie den Befehl [az batchai cluster delete](/cli/azure/batchai/cluster#az_batchai_cluster_delete), um den Cluster zu löschen:

```azurecli
az batchai cluster delete --name mycluster
```

Verwenden Sie den Befehl `az group delete`, um die Ressourcengruppe zu löschen, die Sie für diese Schnellstartanleitung erstellt haben:

```azurecli
az group delete --name myResourceGroup
```

## <a name="restore-azure-cli-20-default-settings"></a>Wiederherstellen der Azure CLI 2.0-Standardeinstellungen

Entfernen Sie die zuvor konfigurierten Standardeinstellungen für den Standort und die Ressourcengruppe:

```azurecli
az configure --defaults group=''

az configure --defaults location=''
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung wurde beschrieben, wie Sie einen CNTK-Trainingsauftrag in einem Batch AI-Cluster mit der Azure CLI ausführen. Weitere Informationen zur Verwendung von Batch AI mit verschiedenen Toolkits finden Sie in den [Trainingsrezepten](https://github.com/Azure/BatchAI).

Weitere Informationen zur Verwendung von Azure CLI 2.0 zum Verwalten von Batch AI finden Sie in der [GitHub-Dokumentation](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md).
