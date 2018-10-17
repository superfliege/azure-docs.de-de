---
title: Azure-Schnellstart – Deep Learning-Training – Azure CLI | Microsoft-Dokumentation
description: 'Schnellstart: Lernen Sie schnell, wie Sie ein neuronales TensorFlow-Deep Learning-Netzwerk in einer einzelnen GPU mit Batch AI mit der Azure CLI trainieren können.'
services: batch-ai
documentationcenter: na
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.custom: ''
ms.service: batch-ai
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: CLI
ms.topic: quickstart
ms.date: 09/03/2018
ms.author: danlep
ms.openlocfilehash: 99d864a5d519ce56a559bea4db7fe89a113e47b9
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44157921"
---
# <a name="quickstart-train-a-deep-learning-model-with-batch-ai"></a>Schnellstart: Trainieren eines Deep Learning-Modells mit Batch AI

Dieser Schnellstart veranschaulicht das Trainieren eines exemplarischen Deep Learning-Modells auf einem GPU-fähigen virtuellen Computer, der mit Batch AI verwaltet wird. Batch AI ist ein verwalteter Dienst, mit dem Data Scientists und KI-Forscher KI- und Machine Learning-Modelle in Clustern mit virtuellen Azure-Computern trainieren können. 

In diesem Beispiel verwenden Sie die Azure CLI zum Einrichten von Batch AI zum Trainieren eines exemplarischen neuronalen [TensorFlow](https://www.tensorflow.org/)-Netzwerks in der [MNIST-Datenbank](http://yann.lecun.com/exdb/mnist/) der handschriftlichen Ziffern. Nach Abschluss dieses Schnellstarts kennen Sie die wichtigsten Konzepte, wie Sie mit Batch AI ein KI- oder Machine Learning-Modell trainieren und verschiedene große Modelle trainieren können.

[!INCLUDE [quickstarts-free-trial-note.md](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für diesen Schnellstart die Azure CLI-Version 2.0.38 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sei bei Bedarf unter [Installieren der Azure CLI](/cli/azure/install-azure-cli). 

Dieser Schnellstart setzt voraus, dass Sie Befehle in einer Bash-Shell ausführen, entweder in der Cloud Shell oder auf Ihrem lokalen Computer. Wenn Sie bereits den Schnellstartphase für [Erstellen eines Batch AI-Clusters mit der Azure CLI](quickstart-create-cluster-cli.md) abgeschlossen haben, überspringen Sie die ersten beiden Schritte zum Erstellen einer Ressourcengruppe und eines Batch AI-Clusters.

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit dem Befehl `az group create` eine Ressourcengruppe. Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. 

Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *eastus2* erstellt. Stellen Sie sicher, dass Sie den Standort USA, Osten 2 oder einen anderen Standort wählen, an dem der Batch AI-Dienst verfügbar ist. 

```azurecli-interactive 
az group create \
    --name myResourceGroup \
    --location eastus2
```

## <a name="create-a-batch-ai-cluster"></a>Erstellen eines Batch AI-Clusters

Verwenden Sie zunächst den Befehl `az batchai workspace create`, um einen Batch AI-*Arbeitsbereich* zu erstellen. Sie benötigen einen Arbeitsbereich, um Ihre Batch AI-Cluster und andere Ressourcen zu organisieren.

```azurecli-interactive
az batchai workspace create \
    --workspace myworkspace \
    --resource-group myResourceGroup
```

Verwenden Sie zum Erstellen eines Batch AI-Clusters den Befehl `az batchai cluster create`. Im folgenden Beispiel wird ein Cluster mit einem Knoten mit den folgenden Eigenschaften erstellt:

* Verwendet die NC6-VM-Größe, die über eine NVIDIA Tesla K80-GPU verfügt. Azure bietet verschiedene VM-Größen mit verschiedenen NVIDIA-GPUs.
* Führt ein Ubuntu-Serverimage aus, das für das Hosting von containerbasierten Anwendungen entwickelt wurde. Sie können die meisten Trainingsworkloads an diesem Verteilungspunkt ausführen. 
* Fügt ein Benutzerkonto mit dem Namen *myusername* hinzu und generiert SSH-Schlüssel, wenn sie nicht bereits am Standardspeicherort für Schlüssel (*~/.ssh*) in Ihrer lokalen Umgebung vorhanden sind.

```azurecli-interactive
az batchai cluster create \
    --name mycluster \
    --workspace myworkspace \
    --resource-group myResourceGroup \
    --vm-size Standard_NC6 \
    --target 1 \
    --user-name myusername \
    --generate-ssh-keys
```

Die Befehlsausgabe zeigt die Clustereigenschaften an. Es dauert einige Minuten, um den Knoten zu erstellen und zu starten. Führen Sie den Befehl `az batchai cluster show`aus, um den Status des Clusters anzuzeigen.

```azurecli-interactive
az batchai cluster show \
    --name mycluster \
    --workspace myworkspace \
    --resource-group myResourceGroup \
    --output table
```

Zu Beginn der Clustererstellung ähnelt die Ausgabe der folgenden Ausgabe, die zeigt, dass sich der Cluster in `resizing` befindet:

```bash
Name       Resource Group    Workspace    VM Size       State      Idle    Running    Preparing    Leaving    Unusable
---------  ----------------  -----------  ------------  -------  ------  ---------  -----------  ---------  ----------
mycluster  myResourceGroup   myworkspace  STANDARD_NC6  resizing      0          0            0          0           0

```

Führen Sie die folgenden Schritte aus, um das Trainingsskript hochzuladen und den Trainingsauftrag zu erstellen, während sich der Clusterstatus ändert. Der Cluster ist bereit für die Auftragsausführung, wenn der Status `steady` lautet und sich der Einzelknoten im `Idle` befinden.

## <a name="upload-training-script"></a>Hochladen des Trainingsskripts

Verwenden Sie den Befehl `az storage account create`, um ein Speicherkonto zum Speichern Ihres Trainingsskripts und der Trainingsausgabe zu erstellen.

```azurecli-interactive
az storage account create \
    --resource-group myResourceGroup \
    --name mystorageaccount \
    --location eastus2 \
    --sku Standard_LRS
```

Erstellen Sie eine Azure-Dateifreigabe namens `myshare` im-Konto mithilfe des Befehls `az storage share create`:

```azurecli-interactive
az storage share create \
    --name myshare \
    --account-name mystorageaccount
```

Verwenden Sie den Befehl `az storage directory create`, um Verzeichnissen in der Azure-Dateifreigabe zu erstellen. Erstellen Sie das `scripts`-Verzeichnis für das Trainingsskript und `logs` für die Trainingsausgabe:

```azurecli-interactive
# Create /scripts directory in file share
az storage directory create \
    --name scripts \
    --share-name myshare \
    --account-name mystorageaccount

# Create /logs directory in file share 
az storage directory create \
    --name logs \
    --share-name myshare \
    --account-name mystorageaccount
```

Erstellen Sie in Ihrer Bash-Shell ein lokales Arbeitsverzeichnis und laden Sie das TensorFlow [convolutional.py](https://raw.githubusercontent.com/tensorflow/models/master/tutorials/image/mnist/convolutional.py)-Beispiel herunter. Das Python-Skript trainiert ein künstliches neuronales Netz im MNIST-Imagesatz aus 60.000 handschriftlichen Ziffern von 0 bis 9. Anschließend wird das Modell anhand verschiedener Testbeispiele getestet.

```bash
wget https://raw.githubusercontent.com/tensorflow/models/master/tutorials/image/mnist/convolutional.py
```

Laden Sie das Skript mit dem Befehl `az storage file upload` in das `scripts`-Verzeichnis der Freigabe.

```azurecli-interactive
az storage file upload \
    --share-name myshare \
    --path scripts \
    --source convolutional.py \
    --account-name mystorageaccount
```

## <a name="submit-training-job"></a>Übermitteln des Trainingsauftrags

Erstellen Sie zunächst einen Batch AI-*Experiment* in Ihrem Arbeitsbereich mithilfe des Befehls `az batchai experiment create`. Ein Experiment ist ein logischer Container für verwandte Batch AI-Aufträge.

```azurecli-interactive
az batchai experiment create \
    --name myexperiment \
    --workspace myworkspace \
    --resource-group myResourceGroup
```

Erstellen Sie in Ihrem Arbeitsverzeichnis eine Trainingsauftrags-Konfigurationsdatei `job.json` mit folgendem Inhalt. Sie übergeben diese Konfigurationsdatei beim Übermitteln des Trainingsauftrags.

Diese `job.json`-Datei enthält Einstellungen zum Suchen der Python-Skriptdatei und zum Ausführen in einem TensorFlow-Container im GPU-Knoten. Außerdem wird angegeben, wo die Ausgabedateien des Auftrags im Azure-Speicher gespeichert werden soll. `<AZURE_BATCHAI_STORAGE_ACCOUNT>` gibt an, dass der Name des Speicherkontos bei der Auftragsübermittlung angegeben wird.

```json
{
    "$schema": "https://raw.githubusercontent.com/Azure/BatchAI/master/schemas/2018-05-01/job.json",
    "properties": {
        "nodeCount": 1,
        "tensorFlowSettings": {
            "pythonScriptFilePath": "$AZ_BATCHAI_JOB_MOUNT_ROOT/myshare/scripts/convolutional.py"
        },
        "stdOutErrPathPrefix": "$AZ_BATCHAI_JOB_MOUNT_ROOT/myshare/logs",
        "mountVolumes": {
            "azureFileShares": [
                {
                    "azureFileUrl": "https://<AZURE_BATCHAI_STORAGE_ACCOUNT>.file.core.windows.net/myshare",
                    "relativeMountPath": "myshare"
                }
            ]
        },
        "containerSettings": {
            "imageSourceRegistry": {
                "image": "tensorflow/tensorflow:1.8.0-gpu"
            }
        }
    }
}
```

Verwenden Sie den Befehl `az batchai job create`, um den Auftrag auf dem Knoten zu übertragen, indem Sie die Konfigurationsdatei `job.json` und den Namen Ihres Speicherkonto übergeben:

```azurecli-interactive
az batchai job create \
    --name myjob \
    --cluster mycluster \
    --experiment myexperiment \
    --workspace myworkspace \
    --resource-group myResourceGroup \
    --config-file job.json \
    --storage-account-name mystorageaccount
```

Der Befehl gibt die Auftragseigenschaften zurück. Anschließend dauert es einige Minuten, bis er abgeschlossen ist. Um den Fortschritt dieses Auftrags zu überwachen, verwenden Sie den Befehl `az batchai job file stream`, um die Datei `stdout-wk-0.txt` aus dem Standardausgabeverzeichnis auf dem Knoten zu streamen. Das Trainingsskript generiert diese Datei, nachdem der Auftrag gestartet wurde.  

```azurecli-interactive
az batchai job file stream \
    --job myjob \
    --experiment myexperiment \
    --workspace myworkspace \
    --resource-group myResourceGroup \
    --file-name stdout-wk-0.txt
```

Beispielausgabe:

```
File found with URL "https://mystorageaccount.file.core.windows.net/logs/00000000-0000-0000-0000-000000000000/myResourceGroup/workspaces/myworkspace/experiments/myexperiment/jobs/myjob/<JOB_ID>/stdouterr/stdout-wk-0.txt?sv=2016-05-31&sr=f&sig=Kih9baozMao8Ugos%2FVG%2BcsVsSeY1O%2FTocCNvLQhwtx4%3D&se=2018-06-20T22%3A07%3A30Z&sp=rl". Start streaming
Successfully downloaded train-images-idx3-ubyte.gz 9912422 bytes.
Successfully downloaded train-labels-idx1-ubyte.gz 28881 bytes.
Successfully downloaded t10k-images-idx3-ubyte.gz 1648877 bytes.
Successfully downloaded t10k-labels-idx1-ubyte.gz 4542 bytes.
Extracting data/train-images-idx3-ubyte.gz
Extracting data/train-labels-idx1-ubyte.gz
Extracting data/t10k-images-idx3-ubyte.gz
Extracting data/t10k-labels-idx1-ubyte.gz
Initialized!
Step 0 (epoch 0.00), 14.9 ms
Minibatch loss: 8.334, learning rate: 0.010000
Minibatch error: 85.9%
Validation error: 84.6%
Step 100 (epoch 0.12), 9.7 ms
Minibatch loss: 3.240, learning rate: 0.010000
Minibatch error: 6.2%
Validation error: 7.7%
Step 200 (epoch 0.23), 8.3 ms
Minibatch loss: 3.335, learning rate: 0.010000
Minibatch error: 7.8%
Validation error: 4.5%
Step 300 (epoch 0.35), 8.3 ms
Minibatch loss: 3.157, learning rate: 0.010000
Minibatch error: 3.1%
...
Step 8500 (epoch 9.89), 8.3 ms
Minibatch loss: 1.605, learning rate: 0.006302
Minibatch error: 0.0%
Validation error: 0.9%
Test error: 0.8%
```

Wenn der Auftrag abgeschlossen ist, wird das Streamen beendet. Das Beispielskript trainiert über 10 *Epochen* oder durchläuft das Trainingsdataset. In diesem Beispiel weist das trainierte Modell nach 10 Epochen, einen Testfehler von nur 0,8 % auf.

## <a name="get-job-output"></a>Abrufen der Auftragsausgabe

Batch AI erstellt für jede Auftragsausgabe eine eindeutige Ordnerstruktur im Speicherkonto. Legen Sie mit diesem Pfad die JOB_OUTPUT_PATH-Umgebungsvariable fest. Listen Sie dann die gespeicherten Ausgabedateien mit dem Befehl `az storage file list` auf:

```azurecli-interactive
export JOB_OUTPUT_PATH=$(az batchai job show --name myjob --experiment myexperiment --workspace myworkspace --resource-group myResourceGroup --query jobOutputDirectoryPathSegment --output tsv)

az storage file list \
    --share-name myshare/logs \
    --account-name mystorageaccount \
    --path $JOB_OUTPUT_PATH/stdouterr \
    --output table
```

Die Ausgabe Die Ausgabe lautet in etwa wie folgt:

```
Name               Content Length  Type    Last Modified
---------------  ----------------  ------  ---------------
execution.log               14866  file
stderr-wk-0.txt              1527  file
stdout-wk-0.txt             11027  file
```

Verwenden Sie den Befehl `az storage file download`, um eine oder mehrere Dateien in Ihr lokales Arbeitsverzeichnis herunterzuladen. Beispiel: 

```azurecli-interactive
az storage file download \
    --share-name myshare/logs \
    --account-name mystorageaccount \
    --path $JOB_OUTPUT_PATH/stdouterr/stdout-wk-0.txt
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie mit den Batchtutorials und -beispielen fortfahren möchten, können Sie den mithilfe dieser Schnellstartanleitung erstellten Batch AI-Arbeitsbereich, Cluster und das Speicherkonto verwenden. 

Es entstehen Kosten für den Batch AI-Cluster, wenn die Knoten ausgeführt werden. Wenn Sie die Clusterkonfiguration beibehalten möchten, wenn Sie keine Aufträge ausführen müssen, passen Sie die Größe des Clusters auf 0 Knoten an. 

```azurecli-interactive
az batchai cluster resize \
    --name mycluster \
    --workspace myworkspace \
    --resource-group myResourceGroup \
    --target 0
```

Ändern Sie die Größe später auf 1 oder mehr Knoten, um Ihre Aufträge auszuführen. Wenn Sie einen Cluster nicht mehr benötigen, löschen Sie ihn mit dem Befehl `az batchai cluster delete`:

```azurecli-interactive
az batchai cluster delete \
    --name mycluster \
    --workspace myworkspace \
    --resource-group myResourceGroup
```

Wenn Ressourcengruppe für die Batch AI- und Speicherressourcen nicht mehr benötigt werden, können Sie sie mit dem Befehl `az group delete` entfernen. 

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte
In diesem Schnellstart haben Sie gelernt, wie Sie mit Batch AI ein exemplarisches TensorFlow Deep Learning-Modell auf einer einzelnen GPU-VM mit der Azure CLI trainieren können. Um zu erfahren, wie Sie Modelltrainings auf einen größeren GPU-Cluster verteilen können, lesen Sie bitte das Batch AI-Tutorial.

> [!div class="nextstepaction"]
> [Tutorial zum verteilten Training](./tutorial-horovod-tensorflow.md)