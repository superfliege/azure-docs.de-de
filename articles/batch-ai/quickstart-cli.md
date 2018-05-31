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
ms.openlocfilehash: 3601ea412790c991892a0c05210d2551810287b8
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2018
ms.locfileid: "33869018"
---
# <a name="run-a-cntk-training-job-using-the-azure-cli"></a>Ausführen eines CNTK-Trainingsauftrags über die Azure CLI

Azure CLI 2.0 ermöglicht Ihnen das Erstellen und Verwalten von Batch AI-Ressourcen: Erstellen/Löschen von Batch AI-Dateiservern und -Clustern und Übermitteln/Beenden/Löschen/Überwachen von Trainingsaufträgen.

In dieser Schnellstartanleitung wird erläutert, wie Sie mit dem Microsoft Cognitive Toolkit einen GPU-Cluster erstellen und einen Trainingsauftrag ausführen.

Das Trainingsskript [ConvNet_MNIST.py](https://github.com/Azure/BatchAI/blob/master/recipes/CNTK/CNTK-GPU-Python/CNTK-GPU-Python.ipynb) steht auf der GitHub-Seite für Batch AI zur Verfügung. Dieses Skript trainiert ein künstliches neuronales Netz in der MNIST-Datenbank handschriftlicher Ziffern.

Das offizielle CNTK-Beispiel wurde angepasst, um den Speicherort des Trainingsdatasets und den Speicherort des Ausgabeverzeichnisses über Befehlszeilenargumente zu akzeptieren.

## <a name="quickstart-overview"></a>Schnellstartübersicht

* Erstellen eines GPU-Clusters mit einem Knoten (VM-Größe: `Standard_NC6`) mit dem Namen `nc6`
* Erstellen eines neuen Speicherkontos zum Speichern von Auftragseingabe und -ausgabe
* Erstellen einer Azure-Dateifreigabe mit den zwei Ordnern `logs` und `scripts` zum Speichern von Auftragsausgaben und Trainingsskripts
* Erstellen des Azure-Blobcontainers `data` zum Speichern von Trainingsdaten
* Bereitstellen des Trainingsskripts und der Trainingsdaten in der erstellten Dateifreigabe und im Container
* Konfigurieren des Auftrags, um die Azure-Dateifreigabe und den Azure-Blobcontainer in den Knoten des Clusters einzubinden und sie als reguläres Dateisystem unter `$AZ_BATCHAI_JOB_MOUNT_ROOT/logs`, `$AZ_BATCHAI_JOB_MOUNT_ROOT/scripts` und `$AZ_BATCHAI_JOB_MOUNT_ROOT/data` verfügbar zu machen.
`AZ_BATCHAI_JOB_MOUNT_ROOT` ist eine Umgebungsvariable, die von Batch AI für den Auftrag festgelegt wird.
* Überwachen der Auftragsausführung durch Streamen der Standardausgabe
* Überprüfen der Ausgabe und generierten Modelle nach Auftragsabschluss
* Abschließendes Löschen aller zugeordneten Ressourcen

# <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement: Falls Sie nicht über ein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.
* Zugriff auf Azure CLI 2.0 mit Version 2.0.31 oder höher. Sie können entweder die in [Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview) verfügbare Version Azure CLI 2.0 verwenden oder die CLI anhand [dieser Anweisungen](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest) lokal installieren.

# <a name="cloud-shell-only"></a>Nur Cloud Shell

Ändern Sie bei Verwendung von Cloud Shell das Arbeitsverzeichnis in `/usr/$USER/clouddrive`, da im Basisverzeichnis kein Platz frei ist:

```azurecli
cd /usr/$USER/clouddrive
```

# <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Eine Azure-Ressourcengruppe ist ein logischer Container für die Bereitstellung und Verwaltung von Azure-Ressourcen. Der folgende Befehl erstellt die neue Ressourcengruppe ```batchai.quickstart``` am Standort „USA, Osten“:

```azurecli
az group create -n batchai.quickstart -l eastus
```

# <a name="create-gpu-cluster"></a>Erstellen des GPU-Clusters

Der folgende Befehl erstellt einen GPU-Cluster mit einem Knoten (VM-Größe: Standard_NC6) mit einer Ubuntu-DSVM als Betriebssystemimage:

```azurecli
az batchai cluster create -n nc6 -g batchai.quickstart -s Standard_NC6 -i UbuntuDSVM -t 1 --generate-ssh-keys
```

Die Ubuntu-DSVM ermöglicht die Ausführung beliebiger Trainingsaufträge in Docker-Containern sowie die direkte Ausführung der meisten gängigen Deep Learning-Frameworks auf einem virtuellen Computer.

Die Option `--generate-ssh-keys` weist die Azure CLI an, private und öffentliche SSH-Schlüssel zu generieren, sofern noch keine vorhanden sind. Sie können mit dem aktuellen Benutzernamen und dem generierten SSH-Schlüssel auf die Clusterknoten zugreifen.

Hinweis: Sichern Sie bei Verwendung von Cloud Shell den Ordner „~/.ssh“ in einem dauerhaften Speicher.

Beispielausgabe:
```json
{
  "allocationState": "steady",
  "allocationStateTransitionTime": "2018-04-11T21:17:26.345000+00:00",
  "creationTime": "2018-04-11T20:12:10.758000+00:00",
  "currentNodeCount": 0,
  "errors": null,
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/batchai.quickstart/providers/Microsoft.BatchAI/clusters/nc6",
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
    "adminUserName": "alex",
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

# <a name="create-a-storage-account"></a>Erstellen eines Speicherkontos

Der folgende Befehl erstellt ein neues Speicherkonto in derselben Region wie die Ressourcengruppe „batchai.repices“. Aktualisieren Sie den Befehl mit einem eindeutigen Speicherkontonamen.

```azurecli
az storage account create -n <storage account name> --sku Standard_LRS -g batchai.quickstart
```

Ist der ausgewählte Speicherkontoname nicht verfügbar, meldet der obige Befehl einen entsprechenden Fehler. Wählen Sie in diesem Fall einen anderen Namen aus, und wiederholen Sie den Vorgang.

# <a name="data-deployment"></a>Datenbereitstellung

## <a name="download-the-training-script-and-training-data"></a>Herunterladen des Trainingsskripts und der Trainingsdaten

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

## <a name="create-azure-file-share-and-deploy-the-training-script"></a>Erstellen einer Azure-Dateifreigabe und Bereitstellen des Trainingsskripts

Die folgenden Befehle erstellen die Azure Dateifreigaben `scripts` und `logs` und kopieren das Trainingsskript in den Ordner `cntk` in der Freigabe `scripts`:

```azurecli
az storage share create -n scripts --account-name <storage account name>
az storage share create -n logs --account-name <storage account name>
az storage directory create -n cntk -s scripts --account-name <storage account name>
az storage file upload -s scripts --source ConvNet_MNIST.py --path cntk --account-name <storage account name> 
```

## <a name="create-a-blob-container-and-deploy-training-data"></a>Erstellen eines Blobcontainers und Bereitstellen der Trainingsdaten

Die folgenden Befehle erstellen den Azure-Blobcontainer `data` und kopieren Trainingsdaten in den Ordner `mnist_cntk`:
```azurecli
az storage container create -n data --account-name <storage account name>
az storage blob upload-batch -s . --pattern '*28x28_cntk*' --destination data --destination-path mnist_cntk --account-name <storage account name>
```

# <a name="submit-training-job"></a>Übermitteln des Trainingsauftrags

## <a name="prepare-job-configuration-file"></a>Vorbereiten der Auftragskonfigurationsdatei

Erstellen Sie die Trainingsauftrags-Konfigurationsdatei `job.json` mit folgendem Inhalt:
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
                    "azureFileUrl": "https://<AZURE_BATCHAI_STORAGE_ACCOUNT>.file.core.windows.net/logs",
                    "relativeMountPath": "logs"
                },
                {
                    "azureFileUrl": "https://<AZURE_BATCHAI_STORAGE_ACCOUNT>.file.core.windows.net/scripts",
                    "relativeMountPath": "scripts"
                }
            ],
            "azureBlobFileSystems": [
                {
                    "accountName": "<AZURE_BATCHAI_STORAGE_ACCOUNT>",
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
* `cntkSettings`: Gibt den Pfad des Trainingsskripts und der Befehlszeilenargumente an. Befehlszeilenargumente enthalten den Pfad zu Trainingsdaten und den Zielpfad zum Speichern der generierten Modelle. `AZ_BATCHAI_OUTPUT_MODEL` ist eine Umgebungsvariable, die von Batch AI auf der Grundlage der Ausgabeverzeichniskonfiguration festgelegt wird (siehe unten)
* `stdOutErrPathPrefix`: Pfad, an dem Batch AI Verzeichnisse mit der Ausgabe und den Protokollen des Auftrags erstellt
* `outputDirectories`: Auflistung der Ausgabeverzeichnisse, die von Batch AI erstellt werden sollen. Für jedes Verzeichnis erstellt Batch AI eine Umgebungsvariable mit dem Namen `AZ_BATCHAI_OUTPUT_<id>`. `<id>` ist die Verzeichnis-ID.
* `mountVolumes`: Liste der Dateisysteme, die während der Auftragsausführung eingebunden werden sollen. Die Dateisysteme werden unter `AZ_BATCHAI_JOB_MOUNT_ROOT/<relativeMountPath>` eingebunden. `AZ_BATCHAI_JOB_MOUNT_ROOT` ist eine Umgebungsvariable, die von Batch AI festgelegt wird.
* `<AZURE_BATCHAI_STORAGE_ACCOUNT>` gibt an, dass der Speicherkontoname während der Auftragsübermittlung über den Parameter „storage-account-name“ oder die Umgebungsvariable `AZURE_BATCHAI_STORAGE_ACCOUNT` auf Ihrem Computer bereitgestellt wird.

## <a name="submit-the-job"></a>Übermitteln des Auftrags

Verwenden Sie den folgenden Befehl, um den Auftrag im Cluster zu übermitteln:

```azurecli
az batchai job create -n cntk_python_1 -r nc6 -g batchai.quickstart -c job.json --storage-account-name <storage account name>
```

Beispielausgabe:
```
{
  "additionalProperties": {},
  "caffeSettings": null,
  "chainerSettings": null,
  "cluster": {
    "additionalProperties": {},
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/batchai.quickstart/providers/Microsoft.BatchAI/clusters/nc6",
    "resourceGroup": "batchai.quickstart"
  },
  "cntkSettings": {
    "additionalProperties": {},
    "commandLineArgs": "$AZ_BATCHAI_JOB_MOUNT_ROOT/data/mnist_cntk $AZ_BATCHAI_OUTPUT_MODEL",
    "configFilePath": null,
    "languageType": "Python",
    "processCount": 1,
    "pythonInterpreterPath": null,
    "pythonScriptFilePath": "$AZ_BATCHAI_JOB_MOUNT_ROOT/scripts/cntk/ConvNet_MNIST.py"
  },
  "constraints": {
    "additionalProperties": {},
    "maxWallClockTime": "7 days, 0:00:00"
  },
  "containerSettings": null,
  "creationTime": "2018-04-11T21:48:10.303000+00:00",
  "customToolkitSettings": null,
  "environmentVariables": null,
  "executionInfo": null,
  "executionState": "queued",
  "executionStateTransitionTime": "2018-04-11T21:48:10.303000+00:00",
  "experimentName": null,
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/batchai.quickstart/providers/Microsoft.BatchAI/jobs/cntk_python_1",
  "inputDirectories": null,
  "jobOutputDirectoryPathSegment": "00000000-0000-0000-0000-000000000000/batchai.quickstart/jobs/cntk_python_1/b9576bae-e878-4fb2-9390-2e962356b5b1",
  "jobPreparation": null,
  "location": null,
  "mountVolumes": {
    "additionalProperties": {},
    "azureBlobFileSystems": [
      {
        "accountName": "<YOU STORAGE ACCOUNT NAME>",
        "additionalProperties": {},
        "containerName": "data",
        "credentials": {
          "accountKey": null,
          "accountKeySecretReference": null,
          "additionalProperties": {}
        },
        "mountOptions": null,
        "relativeMountPath": "data"
      }
    ],
    "azureFileShares": [
      {
        "accountName": "<YOU STORAGE ACCOUNT NAME>,
        "additionalProperties": {},
        "azureFileUrl": "https://<YOU STORAGE ACCOUNT NAME>.file.core.windows.net/logs",
        "credentials": {
          "accountKey": null,
          "accountKeySecretReference": null,
          "additionalProperties": {}
        },
        "directoryMode": "0777",
        "fileMode": "0777",
        "relativeMountPath": "logs"
      },
      {
        "accountName": "<YOU STORAGE ACCOUNT NAME>",
        "additionalProperties": {},
        "azureFileUrl": "https://<YOU STORAGE ACCOUNT NAME>.file.core.windows.net/scripts",
        "credentials": {
          "accountKey": null,
          "accountKeySecretReference": null,
          "additionalProperties": {}
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
      "additionalProperties": {},
      "createNew": true,
      "id": "MODEL",
      "pathPrefix": "$AZ_BATCHAI_JOB_MOUNT_ROOT/logs",
      "pathSuffix": null,
      "type": "custom"
    }
  ],
  "priority": 0,
  "provisioningState": "succeeded",
  "provisioningStateTransitionTime": "2018-04-11T21:48:11.577000+00:00",
  "pyTorchSettings": null,
  "resourceGroup": "batchai.quickstart",
  "secrets": null,
  "stdOutErrPathPrefix": "$AZ_BATCHAI_JOB_MOUNT_ROOT/logs",
  "tags": null,
  "tensorFlowSettings": null,
  "toolType": "cntk",
  "type": "Microsoft.BatchAI/Jobs"
}
```

# <a name="monitor-job-execution"></a>Überwachen der Auftragsausführung

Das Trainingsskript meldet den Trainingsstatus in der Datei `stderr.txt` im Standardausgabeverzeichnis. Sie können den Fortschritt mithilfe des folgenden Befehls überwachen:

```azurecli
az batchai job file stream -n cntk_python_1 -g batchai.quickstart -f stderr.txt
```

Beispielausgabe:
```
File found with URL "https://<YOU STORAGE ACCOUNT>.file.core.windows.net/logs/00000000-0000-0000-0000-000000000000/batchai.quickstart/jobs/cntk_python_1/<JOB's UUID>/stdouterr/stderr.txt?sv=2016-05-31&sr=f&sig=n86JK9YowV%2BPQ%2BkBzmqr0eud%2FlpRB%2FVu%2FFlcKZx192k%3D&se=2018-04-11T23%3A05%3A54Z&sp=rl". Start streaming
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
Final Results: Minibatch[1-11]: errs = 0.54% * 10000
```

Das Streaming wird beendet, wenn der Auftrag abgeschlossen ist (erfolgreich oder mit Fehlern).

# <a name="inspect-generated-model-files"></a>Überprüfen der generierten Modelldateien

Der Auftrag speichert die generierten Modelldateien im Ausgabeverzeichnis. Dabei entspricht das `id`-Attribut `MODEL`. Sie können mit dem folgenden Befehl Modelldateien auflisten und Download-URLs abrufen:

```azurecli
az batchai job file list -n cntk_python_1 -g batchai.quickstart -d MODEL
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

Alternativ können Sie das Portal oder Azure Storage-Explorer zum Überprüfen der generierten Dateien verwenden. Damit die Ausgaben der verschiedenen Aufträge besser unterschieden werden können, erstellt Batch AI eine eindeutige Ordnerstruktur für sie. Den Pfad zum Ordner mit der Ausgabe können Sie anhand des `jobOutputDirectoryPathSegment`-Attributs des übermittelten Auftrags ermitteln:

```azurecli
az batchai job show -n cntk_python_1 -g batchai.quickstart --query jobOutputDirectoryPathSegment
```

Beispielausgabe:
```
"00000000-0000-0000-0000-000000000000/batchai.quickstart/jobs/cntk_python_1/<JOB's UUID>"
```

# <a name="delete-resources"></a>Löschen von Ressourcen

Löschen Sie die Ressourcengruppe und alle zugewiesenen Ressourcen mit dem folgenden Befehl:

```azurecli
az batchai group delete -n batchai.quickstart -y
```
