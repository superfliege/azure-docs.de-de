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
ms.date: 06/14/2018
ms.author: Alexander.Yukhanov
ms.openlocfilehash: eb00c1d4ec74b5268a1497b11087030ab6a86e5a
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36294072"
---
# <a name="run-a-cntk-training-job-using-the-azure-cli"></a>Ausführen eines CNTK-Trainingsauftrags über die Azure CLI

Azure CLI 2.0 ermöglicht Ihnen das Erstellen und Verwalten von Batch AI-Ressourcen: Erstellen/Löschen von Batch AI-Dateiservern und -Clustern und Übermitteln/Beenden/Löschen/Überwachen von Trainingsaufträgen.

In dieser Schnellstartanleitung wird erläutert, wie Sie mit dem Microsoft Cognitive Toolkit (CNTK) einen GPU-Cluster erstellen und einen Trainingsauftrag ausführen.

Das Trainingsskript [ConvNet_MNIST.py](https://raw.githubusercontent.com/Azure/BatchAI/master/recipes/CNTK/CNTK-GPU-Python/ConvNet_MNIST.py) steht auf der GitHub-Seite für Batch AI zur Verfügung. Dieses Skript trainiert ein künstliches neuronales Netz in der MNIST-Datenbank handschriftlicher Ziffern.

Das offizielle CNTK-Beispiel wurde angepasst, um den Speicherort des Trainingsdatasets und den Speicherort des Ausgabeverzeichnisses über Befehlszeilenargumente zu akzeptieren.

## <a name="quickstart-overview"></a>Schnellstartübersicht

* Erstellen eines GPU-Clusters mit einem Knoten (VM-Größe: `Standard_NC6`) mit dem Namen `nc6`
* Erstellen eines Speicherkontos zum Speichern von Auftragseingabe und -ausgabe
* Erstellen einer Azure-Dateifreigabe mit den zwei Ordnern `logs` und `scripts` zum Speichern von Auftragsausgaben und Trainingsskripts
* Erstellen des Azure-Blobcontainers `data` zum Speichern von Trainingsdaten
* Bereitstellen des Trainingsskripts und der Trainingsdaten in der erstellten Dateifreigabe und im Container
* Konfigurieren des Auftrags, um die Azure-Dateifreigabe und den Azure-Blobcontainer in den Knoten des Clusters einzubinden und sie als reguläres Dateisystem unter `$AZ_BATCHAI_JOB_MOUNT_ROOT/logs`, `$AZ_BATCHAI_JOB_MOUNT_ROOT/scripts` und `$AZ_BATCHAI_JOB_MOUNT_ROOT/data` verfügbar zu machen.
`AZ_BATCHAI_JOB_MOUNT_ROOT` ist eine Umgebungsvariable, die von Batch AI für den Auftrag festgelegt wird.
* Überwachen der Auftragsausführung durch Streamen der Standardausgabe
* Überprüfen der Ausgabe und generierten Modelle nach Auftragsabschluss
* Abschließendes Löschen aller zugeordneten Ressourcen

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement: Falls Sie nicht über ein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.
* Zugriff auf Azure CLI 2.0 mit Version 0.3 oder höher des Batch AI-Moduls. Sie können entweder die in [Azure Cloud Shell](../cloud-shell/overview.md) verfügbare Azure CLI 2.0 verwenden oder die CLI anhand [dieser Anweisungen](/cli/azure/install-azure-cli?view=azure-cli-latest) lokal installieren.

  Ändern Sie bei Verwendung von Cloud Shell das Arbeitsverzeichnis in `/usr/$USER/clouddrive`, da im Basisverzeichnis kein Platz frei ist:

  ```azurecli
  cd /usr/$USER/clouddrive
  ```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Eine Azure-Ressourcengruppe ist ein logischer Container für die Bereitstellung und Verwaltung von Azure-Ressourcen. Der folgende Befehl erstellt die neue Ressourcengruppe `batchai.quickstart` am Standort „USA, Osten“:

```azurecli
az group create -n batchai.quickstart -l eastus
```
## <a name="create-batch-ai-workspace"></a>Erstellen eines Batch AI-Arbeitsbereichs

Der folgende Befehl erstellt einen Batch AI-Arbeitsbereich in der Ressourcengruppe. Ein Batch AI-Arbeitsbereich ist eine allgemeine Sammlung aller Typen von Batch AI-Ressourcen:

```azurecli
az batchai workspace create -g batchai.quickstart -n quickstart
```

## <a name="create-gpu-cluster"></a>Erstellen des GPU-Clusters

Der folgende Befehl erstellt einen GPU-Cluster mit einem Knoten (VM-Größe: Standard_NC6) im Arbeitsbereich mit Ubuntu Data Science Virtual Machine (DSVM) als Betriebssystemimage:

```azurecli
az batchai cluster create -n nc6 -g batchai.quickstart -w quickstart -s Standard_NC6 -i UbuntuDSVM -t 1 --generate-ssh-keys
```

Ubuntu DSVM ermöglicht die Ausführung beliebiger Trainingsaufträge in Docker-Containern sowie die direkte Ausführung der meisten gängigen Deep Learning-Frameworks auf dem virtuellen Computer.

Die Option `--generate-ssh-keys` weist die Azure CLI an, private und öffentliche SSH-Schlüssel zu generieren, sofern noch keine vorhanden sind. Sie können mit dem aktuellen Benutzernamen und dem generierten SSH-Schlüssel auf die Clusterknoten zugreifen.

Bei Verwendung von Cloud Shell sollten Sie den Ordner „~/.ssh“ in einem dauerhaften Speicher sichern.

Beispielausgabe:
```json
{
  "allocationState": "steady",
  "allocationStateTransitionTime": "2018-04-11T21:17:26.345000+00:00",
  "creationTime": "2018-04-11T20:12:10.758000+00:00",
  "currentNodeCount": 0,
  "errors": null,
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/batchai.quickstart/providers/Microsoft.BatchAI/workspaces/quickstart/clusters/nc6",
  "location": "eastus",
  "name": "nc6",
  "nodeSetup": null,
  "nodeStateCounts": {
    "additionalProperties": {},
    "idleNodeCount": 0,
    "leavingNodeCount": 0,
    "preparingNodeCount": 0,
    "runningNodeCount": 0,
    "unusableNodeCount": 0
  },
  "provisioningState": "succeeded",
  "provisioningStateTransitionTime": "2018-04-11T20:12:11.445000+00:00",
  "resourceGroup": "batchai.quickstart",
  "scaleSettings": {
    "additionalProperties": {},
    "autoScale": null,
    "manual": {
      "nodeDeallocationOption": "requeue",
      "targetNodeCount": 1
    }
  },
  "subnet": null,
  "tags": null,
  "type": "Microsoft.BatchAI/Clusters",
  "userAccountSettings": {
    "additionalProperties": {},
    "adminUserName": "myuser",
    "adminUserPassword": null,
    "adminUserSshPublicKey": "<YOUR SSH PUBLIC KEY HERE>"
  },
  "virtualMachineConfiguration": {
    "additionalProperties": {},
    "imageReference": {
      "additionalProperties": {},
      "offer": "linux-data-science-vm-ubuntu",
      "publisher": "microsoft-ads",
      "sku": "linuxdsvmubuntu",
      "version": "latest",
      "virtualMachineImageId": null
    }
  },
  "vmPriority": "dedicated",
  "vmSize": "STANDARD_NC6"
}
```

## <a name="create-a-storage-account"></a>Speicherkonto erstellen

Der folgende Befehl erstellt ein Speicherkonto in derselben Ressourcengruppe, die zum Erstellen des Batch AI-Clusters verwendet wurde. Sie verwenden das Speicherkonto zum Speichern von Auftragseingabe und -ausgabe. Aktualisieren Sie den Befehl mit einem eindeutigen Speicherkontonamen.

```azurecli
az storage account create -n <storage account name> --sku Standard_LRS -g batchai.quickstart
```


## <a name="deploy-data"></a>Bereitstellen von Daten

### <a name="download-the-training-script-and-training-data"></a>Herunterladen des Trainingsskripts und der Trainingsdaten

* Laden Sie die vorverarbeitete MNIST-Datenbank [hier](https://batchaisamples.blob.core.windows.net/samples/mnist_dataset.zip?st=2017-09-29T18%3A29%3A00Z&se=2099-12-31T08%3A00%3A00Z&sp=rl&sv=2016-05-31&sr=c&sig=PmhL%2BYnYAyNTZr1DM2JySvrI12e%2F4wZNIwCtf7TRI%2BM%3D) herunter, und extrahieren Sie sie im aktuellen Ordner.

Für GNU/Linux oder Cloud Shell:

```azurecli
wget "https://batchaisamples.blob.core.windows.net/samples/mnist_dataset.zip?st=2017-09-29T18%3A29%3A00Z&se=2099-12-31T08%3A00%3A00Z&sp=rl&sv=2016-05-31&sr=c&sig=PmhL%2BYnYAyNTZr1DM2JySvrI12e%2F4wZNIwCtf7TRI%2BM%3D" -O mnist_dataset.zip
unzip mnist_dataset.zip
```

Beachten Sie, dass Sie unter Umständen `unzip` installieren müssen, sofern noch nicht in Ihrer GNU-/Linux-Distribution enthalten.

* Laden Sie das Beispielskript [ConvNet_MNIST.py](https://raw.githubusercontent.com/Azure/BatchAI/master/recipes/CNTK/CNTK-GPU-Python/ConvNet_MNIST.py) in den aktuellen Ordner herunter:

Für GNU/Linux oder Cloud Shell:

```azurecli
wget https://raw.githubusercontent.com/Azure/BatchAI/master/recipes/CNTK/CNTK-GPU-Python/ConvNet_MNIST.py
```

### <a name="create-azure-file-share-and-deploy-the-training-script"></a>Erstellen einer Azure-Dateifreigabe und Bereitstellen des Trainingsskripts

Die folgenden Befehle erstellen die Azure Dateifreigaben `scripts` und `logs` und kopieren das Trainingsskript in den Ordner `cntk` in der Freigabe `scripts`:

```azurecli
az storage share create -n scripts --account-name <storage account name>
az storage share create -n logs --account-name <storage account name>
az storage directory create -n cntk -s scripts --account-name <storage account name>
az storage file upload -s scripts --source ConvNet_MNIST.py --path cntk --account-name <storage account name> 
```

### <a name="create-a-blob-container-and-deploy-training-data"></a>Erstellen eines Blobcontainers und Bereitstellen der Trainingsdaten

Die folgenden Befehle erstellen den Azure-Blobcontainer `data` und kopieren Trainingsdaten in den Ordner `mnist_cntk`:

```azurecli
az storage container create -n data --account-name <storage account name>
az storage blob upload-batch -s . --pattern '*28x28_cntk*' --destination data --destination-path mnist_cntk --account-name <storage account name>
```

## <a name="submit-training-job"></a>Übermitteln des Trainingsauftrags

### <a name="create-a-batch-ai-experiment"></a>Erstellen eines Batch AI-Experiments

Ein Experiment ist ein logischer Container für verwandte Batch AI-Aufträge. Verwenden Sie den folgenden Befehl, um ein Experiment in Ihrem Arbeitsbereich zu erstellen:

```azurecli
az batchai experiment create -g batchai.quickstart -w quickstart -n quickstart
```

### <a name="prepare-job-configuration-file"></a>Vorbereiten der Auftragskonfigurationsdatei

Erstellen Sie die Trainingsauftrags-Konfigurationsdatei `job.json` mit folgendem Inhalt. Aktualisieren Sie sie mit dem Namen Ihres Speicherkontos.

```json
{
    "$schema": "https://raw.githubusercontent.com/Azure/BatchAI/master/schemas/2018-03-01/cntk.json",
    "properties": {
        "nodeCount": 1,
        "cntkSettings": {
            "pythonScriptFilePath": "$AZ_BATCHAI_JOB_MOUNT_ROOT/scripts/cntk/ConvNet_MNIST.py",
            "commandLineArgs": "$AZ_BATCHAI_JOB_MOUNT_ROOT/data/mnist_cntk $AZ_BATCHAI_OUTPUT_MODEL"
        },
        "stdOutErrPathPrefix": "$AZ_BATCHAI_JOB_MOUNT_ROOT/logs",
        "outputDirectories": [{
            "id": "MODEL",
            "pathPrefix": "$AZ_BATCHAI_JOB_MOUNT_ROOT/logs"
        }],
        "mountVolumes": {
            "azureFileShares": [
                {
                    "azureFileUrl": "https://<YOUR_STORAGE_ACCOUNT>.file.core.windows.net/logs",
                    "relativeMountPath": "logs"
                },
                {
                    "azureFileUrl": "https://<YOUR_STORAGE_ACCOUNT>.file.core.windows.net/scripts",
                    "relativeMountPath": "scripts"
                }
            ],
            "azureBlobFileSystems": [
                {
                    "accountName": "<YOUR_STORAGE_ACCOUNT>",
                    "containerName": "data",
                    "relativeMountPath": "data"
                }
            ]
        }
    }
}
```

Diese Konfigurationsdatei enthält Folgendes:

* `nodeCount`: Anzahl der vom Auftrag benötigten Knoten (ein Knoten in dieser Schnellstartanleitung)
* `cntkSettings`: Gibt den Pfad des Trainingsskripts und der Befehlszeilenargumente an. Befehlszeilenargumente enthalten den Pfad zu Trainingsdaten und den Zielpfad zum Speichern der generierten Modelle. `AZ_BATCHAI_OUTPUT_MODEL` ist eine Umgebungsvariable, die von Batch AI auf der Grundlage der Ausgabeverzeichniskonfiguration festgelegt wird (siehe unten).
* `stdOutErrPathPrefix`: Pfad, an dem Batch AI Verzeichnisse mit der Ausgabe und den Protokollen des Auftrags erstellt
* `outputDirectories`: Auflistung der Ausgabeverzeichnisse, die von Batch AI erstellt werden sollen. Für jedes Verzeichnis erstellt Batch AI eine Umgebungsvariable mit dem Namen `AZ_BATCHAI_OUTPUT_<id>`. `<id>` ist die Verzeichnis-ID.
* `mountVolumes`: Liste der Dateisysteme, die während der Auftragsausführung eingebunden werden sollen. Die Dateisysteme werden unter `AZ_BATCHAI_JOB_MOUNT_ROOT/<relativeMountPath>` eingebunden. `AZ_BATCHAI_JOB_MOUNT_ROOT` ist eine Umgebungsvariable, die von Batch AI festgelegt wird.
* `<AZURE_BATCHAI_STORAGE_ACCOUNT>`: Der Speicherkontoname, der während der Auftragsübermittlung über die Umgebungsvariable `--storage-account-name parameter` oder `AZURE_BATCHAI_STORAGE_ACCOUNT` auf Ihrem Computer angegeben wird

### <a name="submit-the-job"></a>Übermitteln des Auftrags

```azurecli
az batchai job create -n cntk_python_1 -c nc6 -g batchai.quickstart -w quickstart -e quickstart  -f job.json --storage-account-name <storage account name>
```

Beispielausgabe:
```
{
  "caffe2Settings": null,
  "caffeSettings": null,
  "chainerSettings": null,
  "cluster": {
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/batchai.quickstart/providers/Microsoft.BatchAI/workspaces/quickstart/clusters/nc6",
    "resourceGroup": "batchai.quickstart"
  },
  "cntkSettings": {
    "commandLineArgs": "$AZ_BATCHAI_JOB_MOUNT_ROOT/data/mnist_cntk $AZ_BATCHAI_OUTPUT_MODEL",
    "configFilePath": null,
    "languageType": "Python",
    "processCount": 1,
    "pythonInterpreterPath": null,
    "pythonScriptFilePath": "$AZ_BATCHAI_JOB_MOUNT_ROOT/scripts/cntk/ConvNet_MNIST.py"
  },
  "constraints": {
    "maxWallClockTime": "7 days, 0:00:00"
  },
  "containerSettings": null,
  "creationTime": "2018-06-14T22:22:57.543000+00:00",
  "customMpiSettings": null,
  "customToolkitSettings": null,
  "environmentVariables": null,
  "executionInfo": {
    "endTime": null,
    "errors": null,
    "exitCode": null,
    "startTime": "2018-06-14T22:22:59.838000+00:00"
  },
  "executionState": "running",
  "executionStateTransitionTime": "2018-06-14T22:22:59.838000+00:00",
  "horovodSettings": null,
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/batchai.quickstart/providers/Microsoft.BatchAI/workspaces/quickstart/experiments/quickstart/jobs/cntk_python_1",
  "inputDirectories": null,
  "jobOutputDirectoryPathSegment": "00000000-0000-0000-0000-000000000000/batchai.quickstart/workspaces/quickstart/experiments/quickstart/jobs/cntk_python_1/f2d6ff09-7549-4e1a-8cd8-ec839f042a61",
  "jobPreparation": null,
  "mountVolumes": {
    "azureBlobFileSystems": [
      {
        "accountName": "<YOUR STORAGE ACCOUNT NAME>",
        "containerName": "data",
        "credentials": {
          "accountKey": null,
          "accountKeySecretReference": null
        },
        "mountOptions": null,
        "relativeMountPath": "data"
      }
    ],
    "azureFileShares": [
      {
        "accountName": "<YOUR STORAGE ACCOUNT NAME>",
        "azureFileUrl": "https://<YOUR STORAGE ACCOUNT NAME>.file.core.windows.net/logs",
        "credentials": {
          "accountKey": null,
          "accountKeySecretReference": null
        },
        "directoryMode": "0777",
        "fileMode": "0777",
        "relativeMountPath": "logs"
      },
      {
        "accountName": "<YOUR STORAGE ACCOUNT NAME>",
        "azureFileUrl": "https://<YOUR STORAGE ACCOUNT NAME>.file.core.windows.net/scripts",
        "credentials": {
          "accountKey": null,
          "accountKeySecretReference": null
        },
        "directoryMode": "0777",
        "fileMode": "0777",
        "relativeMountPath": "scripts"
      }
    ],
    "fileServers": null,
    "unmanagedFileSystems": null
  },
  "name": "cntk_python_1",
  "nodeCount": 1,
  "outputDirectories": [
    {
      "id": "MODEL",
      "pathPrefix": "$AZ_BATCHAI_JOB_MOUNT_ROOT/logs",
      "pathSuffix": null
    }
  ],
  "provisioningState": "succeeded",
  "provisioningStateTransitionTime": "2018-06-14T22:22:58.625000+00:00",
  "pyTorchSettings": null,
  "resourceGroup": "danlep0614b",
  "schedulingPriority": "normal",
  "secrets": null,
  "stdOutErrPathPrefix": "$AZ_BATCHAI_JOB_MOUNT_ROOT/logs",
  "tensorFlowSettings": null,
  "toolType": "cntk",
  "type": "Microsoft.BatchAI/workspaces/experiments/jobs"
}

```

## <a name="monitor-job-execution"></a>Überwachen der Auftragsausführung

Das Trainingsskript meldet den Trainingsstatus in der Datei `stderr.txt` im Standardausgabeverzeichnis. Überwachen Sie den Fortschritt mithilfe des folgenden Befehls:

```azurecli
az batchai job file stream -j cntk_python_1 -g batchai.quickstart -w quickstart -e quickstart -f stderr.txt
```

Beispielausgabe:
```
File found with URL "https://<YOUR STORAGE ACCOUNT>.file.core.windows.net/logs/00000000-0000-0000-0000-000000000000/batchai.quickstart/jobs/cntk_python_1/<JOB's UUID>/stdouterr/stderr.txt?sv=2016-05-31&sr=f&sig=n86JK9YowV%2BPQ%2BkBzmqr0eud%2FlpRB%2FVu%2FFlcKZx192k%3D&se=2018-04-11T23%3A05%3A54Z&sp=rl". Start streaming
Selected GPU[0] Tesla K80 as the process wide default device.
-------------------------------------------------------------------
Build info:

        Built time: Jan 31 2018 15:03:41
        Last modified date: Tue Jan 30 03:26:13 2018
        Build type: release
        Build target: GPU
        With 1bit-SGD: no
        With ASGD: yes
        Math lib: mkl
        CUDA version: 9.0.0
        CUDNN version: 7.0.4
        Build Branch: HEAD
        Build SHA1: a70455c7abe76596853f8e6a77a4d6de1e3ba76e
        MPI distribution: Open MPI
        MPI version: 1.10.7
-------------------------------------------------------------------
Training 98778 parameters in 10 parameter tensors.

Learning rate per 1 samples: 0.001
Momentum per 1 samples: 0.0
Finished Epoch[1 of 40]: [Training] loss = 0.405960 * 60000, metric = 13.01% * 60000 21.741s (2759.8 samples/s);
Finished Epoch[2 of 40]: [Training] loss = 0.106030 * 60000, metric = 3.09% * 60000 3.638s (16492.6 samples/s);
Finished Epoch[3 of 40]: [Training] loss = 0.078542 * 60000, metric = 2.32% * 60000 3.477s (17256.3 samples/s);
...
Final Results: Minibatch[1-11]: errs = 0.62% * 10000
```

Das Streaming wird beendet, wenn der Auftrag abgeschlossen ist (erfolgreich oder mit Fehlern).

## <a name="inspect-generated-model-files"></a>Überprüfen der generierten Modelldateien

Der Auftrag speichert die generierten Modelldateien im Ausgabeverzeichnis. Dabei ist das Attribut `id` gleich `MODEL`. Mit dem folgenden Befehl listen Sie Modelldateien auf und rufen Download-URLs ab:

```azurecli
az batchai job file list -j cntk_python_1 -w quickstart -e quickstart -g batchai.quickstart -d MODEL
```

Beispielausgabe:
```
[
  {
    "additionalProperties": {},
    "contentLength": 409456,
    "downloadUrl": "https://<YOUR STORAGE ACCOUNT>.file.core.windows.net/...",
    "isDirectory": false,
    "lastModified": "2018-04-11T22:05:51+00:00",
    "name": "ConvNet_MNIST_0.dnn"
  },
  {
    "additionalProperties": {},
    "contentLength": 409456,
    "downloadUrl": "https://<YOUR STORAGE ACCOUNT>.file.core.windows.net/...",
    "isDirectory": false,
    "lastModified": "2018-04-11T22:05:55+00:00",
    "name": "ConvNet_MNIST_1.dnn"
  },
...

```

Alternativ können Sie das Azure-Portal oder den Azure Storage-Explorer zum Überprüfen der generierten Dateien verwenden. Damit die Ausgaben der verschiedenen Aufträge besser unterschieden werden können, erstellt Batch AI eine eindeutige Ordnerstruktur für sie. Den Pfad zum Ordner mit der Ausgabe können Sie anhand des `jobOutputDirectoryPathSegment`-Attributs des übermittelten Auftrags ermitteln:

```azurecli
az batchai job show -n cntk_python_1 -g batchai.quickstart -w quickstart -e quickstart --query jobOutputDirectoryPathSegment
```

Beispielausgabe:
```
"00000000-0000-0000-0000-000000000000/batchai.quickstart/workspaces/quickstart/experiments/quickstart/jobs/cntk_python_1/<JOB's UUID>"
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe und alle zugewiesenen Ressourcen mit dem folgenden Befehl, wenn Sie sie nicht mehr benötigen:

```azurecli
az group delete -n batchai.quickstart -y
```
