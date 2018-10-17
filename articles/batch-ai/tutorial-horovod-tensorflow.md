---
title: Tutorial – Verteiltes Training mit Azure Batch AI und Horovod | Microsoft-Dokumentation
description: Tutorial – So trainieren Sie ein verteiltes Modell mit Horovod mithilfe des Azure Batch AI-Dienstes und der Azure CLI.
services: batch-ai
author: johnwu10
manager: jeconnoc
ms.service: batch-ai
ms.topic: tutorial
ms.date: 09/03/2018
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: de19b20865127fd37cd7bc1ac854288a95a68091
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44058099"
---
# <a name="tutorial-train-a-distributed-model-with-horovod"></a>Tutorial: Trainieren eines verteilten Modells mit Horovod

In diesem Tutorial trainieren Sie ein verteiltes Deep Learning-Modell, indem Sie es parallel über mehrere Knoten in einem Batch AI-Cluster ausführen. Batch AI ist ein verwalteter Dienst für das Trainieren von Machine Learning- und KI-Modellen von Azure GPU-Clustern. 

Dieses Tutorial stellt einen gängigen Batch AI-Workflow vor und zeigt, wie Sie mit Batch AI-Ressourcen über die Azure CLI interagieren können. Folgende Themen werden behandelt:

> [!div class="checklist"]
> * Einrichten eines Batch AI-Arbeitsbereichs, eines Experiments und eines Clusters
> * Einrichten einer Azure-Dateifreigabe für die Eingabe und Ausgabe
> * Parallelisieren eines Deep Learning-Modells mit Horovod
> * Übermitteln eines Trainingsauftrags
> * Überwachen des Auftrags
> * Abrufen der Trainingsergebnisse

In diesem Tutorial wird ein Objekterkennungsmodell so geändert, dass es parallel zu [Horovod](https://github.com/uber/horovod) ausgeführt wird. Das Modell trainiert auf dem [CIFAR 10-Dataset](https://www.cs.toronto.edu/~kriz/cifar.html) von Images. Der Trainingsauftrag läuft auf einem Cluster mit 24 vCPUs und 4 GPUs und dauert ca. 60 Minuten.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die Befehlszeilenschnittstelle lokal installieren und verwenden möchten, müssen Sie für dieses Tutorial die Azure CLI-Version 2.0.38 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sei bei Bedarf unter [Installieren der Azure CLI](/cli/azure/install-azure-cli). 

## <a name="why-use-horovod"></a>Gründe für die Verwendung von Horovod

Horovod ist ein verteiltes Trainingsframework für Tensorflow, Keras und PyTorch und wird für dieses Tutorial verwendet. Mit Horovod können Sie ein Trainingsskript, das für die Ausführung auf einer einzelnen GPU ausgelegt ist, in ein Skript umwandeln, das mit wenigen Zeilen Code effizient auf einem verteilten System läuft.

Zusätzlich zu Horovod unterstützt Batch AI verteilte Trainings mit mehreren anderen beliebten Open-Source-Frameworks. Achten Sie darauf, die Lizenzbedingungen eines jeden Frameworks zu lesen, das Sie zum Trainieren von Modellen in der Produktion verwenden.

## <a name="prepare-the-batch-ai-environment"></a>Vorbereiten der Batch AI-Umgebung

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Verwenden Sie den Befehl `az group create`, um eine Ressourcengruppe mit dem Namen `batchai.horovod` in der Region `eastus` zu erstellen. Sie verwenden die Ressourcengruppe aus, um Batch AI-Ressourcen bereitzustellen.

```azurecli-interactive
az group create --name batchai.horovod --location eastus
```

### <a name="create-a-workspace"></a>Erstellen eines Arbeitsbereichs

Verwenden Sie den Befehl `az batchai workspace create`, um einen Batch AI-Arbeitsbereich zu erstellen. Ein Arbeitsbereich ist eine allgemeine Sammlung anderer Batch AI-Ressourcen. Der folgende Befehl erstellt einen Arbeitsbereich mit der Bezeichnung `batchaidev` in Ihrer Ressourcengruppe.

```azurecli-interactive
az batchai workspace create --resource-group batchai.horovod --workspace batchaidev 
```

### <a name="create-an-experiment"></a>Erstellen eines Experiments

Ein Batch AI-Experiment gruppiert einen oder mehrere Aufträge, die Sie gemeinsam abfragen und verwalten. Der folgende Befehl `az batchai experiment create` erstellt einen Arbeitsbereich mit der Bezeichnung `cifar` im Arbeitsbereich und in der Ressourcengruppe.

```azurecli-interactive
az batchai experiment create --resource-group batchai.horovod --workspace batchaidev --name cifar 
```

## <a name="set-up-a-gpu-cluster"></a>Einrichten eines GPU-Clusters

Richten Sie als Nächstes einen GPU-Clusters ein, um das Experiment auszuführen. Batch AI bietet eine flexible Reihe von Optionen zum Anpassen von Clustern an spezifische Anforderungen.

Der folgende Befehl `az batchai cluster create` erstellt einen Cluster mit vier Knoten mit der Bezeichnung `nc6cluster` im Arbeitsbereich und in der Ressourcengruppe. Standardmäßig führen die VMs im Cluster ein Ubuntu-Serverimage aus, das für das Hosting von containerbasierten Anwendungen entwickelt wurde. Die Clusterknoten in diesem Beispiel wird die Größe `Standard_NC6`, die eine NVIDIA Tesla K80-GPU enthält.

```azurecli-interactive
az batchai cluster create --resource-group batchai.horovod --workspace batchaidev --name nc6cluster --vm-priority dedicated  --vm-size Standard_NC6 --target 4 --generate-ssh-keys
```

Führen Sie den Befehl `az batchai cluster show`aus, um den Status des Clusters anzuzeigen. Die vollständige Bereitstellung des Clusters dauert in der Regel einige Minuten.

```azurecli-interactive
az batchai cluster show --name nc6cluster --workspace batchaidev --resource-group batchai.horovod --output table
```

Zu einem frühen Zeitpunkt der Clustererstellung hat der Cluster den Zustand `resizing`. Führen Sie die folgenden Schritte aus, während sich der Clusterzustand ändert. Der Cluster ist bereit für die Auftragsausführung, wenn der Status `steady` lautet und sich die Knoten im `idle` befinden. Beispiel: 

```
Name        Resource Group    Workspace    VM Size       State      Idle    Running    Preparing    Leaving    Unusable
----------  ----------------  -----------  ------------  -------  ------  ---------  -----------  ---------  ----------
nc6cluster  batchai.horovod  batchaidev   STANDARD_NC6  steady        4          0            0          0           0
```

## <a name="set-up-storage"></a>Einrichten des Speichers

Verwenden Sie den Befehl `az storage account create`, um ein Speicherkonto zum Speichern Ihres Trainingsskripts und der Trainingsausgabe zu erstellen.

```azurecli-interactive
az storage account create --resource-group batchai.horovod --name mystorageaccount --location eastus --sku Standard_LRS
```

Erstellen Sie eine Azure-Dateifreigabe namens `myshare` im-Konto mithilfe des Befehls `az storage share create`:

```azurecli-interactive
az storage share create --name myshare --account-name mystorageaccount
```

In der Praxis kann dieser Speicher für mehrere Aufträge und Experimente verwendet werden. Um die Abläufe zu organisieren, erstellen Sie ein Verzeichnis innerhalb der Dateifreigabe, um Dateien zu speichern, die sich auf dieses spezielle Experiment beziehen. Mit dem folgenden Befehl `az storage directory create` wird ein Speicherkonto mit der Bezeichnung `cifar` erstellt.

```azurecli-interactive
az storage directory create --name cifar --share-name myshare --account-name mystorageaccount
```

Der nächste Schritt ist die Vorbereitung des eigentlichen Trainingsskripts, das Sie dann in das neu erstellte Verzeichnis hochladen.

## <a name="create-the-training-script"></a>Erstellen des Trainingsskripts

Für dieses Experiment führen Sie ein Python-Skript aus, das mit einigen Änderungen aktualisiert wird, um ein Objekterkennungsmodell parallel mit Horovod auszuführen. Das [Originalmodell](https://raw.githubusercontent.com/keras-team/keras/master/examples/cifar10_cnn.py) verwendet Keras mit einem TensorFlow-Back-End. 

Verwenden Sie in einem Arbeitsverzeichnis in Ihrer Shell Ihren bevorzugten Texteditor, um eine Datei namens `cifar_cnn_distributed.py` mit folgendem Inhalt zu erstellen. Änderungen am ursprünglichen Quellcode werden mit dem Präfix `HOROVOD` angegeben.

```python
from __future__ import print_function
import keras
from keras.datasets import cifar10
from keras.preprocessing.image import ImageDataGenerator
from keras.models import Sequential
from keras.layers import Dense, Dropout, Activation, Flatten
from keras.layers import Conv2D, MaxPooling2D
import tensorflow as tf
import horovod.keras as hvd
import os
from keras import backend as K
import math
import argparse 

# HOROVOD: initialize Horovod.
hvd.init()

# HOROVOD: pin GPU to be used to process local rank (one GPU per process)
config = tf.ConfigProto()
config.gpu_options.allow_growth = True
config.gpu_options.visible_device_list = str(hvd.local_rank())
K.set_session(tf.Session(config=config))

batch_size = 32
num_classes = 10
# HOROVOD: adjust number of epochs based on number of GPUs.
epochs = int(math.ceil(100.0 / hvd.size()))

data_augmentation = True
num_predictions = 20
# BATCH AI: change save directory to mounted storage path
parser = argparse.ArgumentParser()
parser.add_argument("-d", "--dir", help="directory to save model to")
args = parser.parse_args()
save_dir = os.path.join(args.dir, 'saved_models')
model_name = 'keras_cifar10_trained_model.h5'

# The data, split between train and test sets:
(x_train, y_train), (x_test, y_test) = cifar10.load_data()
print('x_train shape:', x_train.shape)
print(x_train.shape[0], 'train samples')
print(x_test.shape[0], 'test samples')

# Convert class vectors to binary class matrices.
y_train = keras.utils.to_categorical(y_train, num_classes)
y_test = keras.utils.to_categorical(y_test, num_classes)

model = Sequential()
model.add(Conv2D(32, (3, 3), padding='same',
                 input_shape=x_train.shape[1:]))
model.add(Activation('relu'))
model.add(Conv2D(32, (3, 3)))
model.add(Activation('relu'))
model.add(MaxPooling2D(pool_size=(2, 2)))
model.add(Dropout(0.25))

model.add(Conv2D(64, (3, 3), padding='same'))
model.add(Activation('relu'))
model.add(Conv2D(64, (3, 3)))
model.add(Activation('relu'))
model.add(MaxPooling2D(pool_size=(2, 2)))
model.add(Dropout(0.25))

model.add(Flatten())
model.add(Dense(512))
model.add(Activation('relu'))
model.add(Dropout(0.5))
model.add(Dense(num_classes))
model.add(Activation('softmax'))

# HOROVOD: adjust learning rate based on number of GPUs.
opt = keras.optimizers.rmsprop(lr=0.0001 * hvd.size(), decay=1e-6)

# HOROVOD: add Horovod Distributed Optimizer.
opt = hvd.DistributedOptimizer(opt)

# Let's train the model using RMSprop
model.compile(loss='categorical_crossentropy',
              optimizer=opt,
              metrics=['accuracy'])

callbacks = [
    # HOROVOD: broadcast initial variable states from rank 0 to all other processes.
    # This is necessary to ensure consistent initialization of all workers when
    # training is started with random weights or restored from a checkpoint.
    hvd.callbacks.BroadcastGlobalVariablesCallback(0),
]

# HOROVOD: save checkpoints only on worker 0 to prevent other workers from corrupting them.
if hvd.rank() == 0:
    callbacks.append(keras.callbacks.ModelCheckpoint('./checkpoint-{epoch}.h5'))

x_train = x_train.astype('float32')
x_test = x_test.astype('float32')
x_train /= 255
x_test /= 255

if not data_augmentation:
    print('Not using data augmentation.')
    model.fit(x_train, y_train,
              batch_size=batch_size,
              epochs=epochs,
              validation_data=(x_test, y_test),
              shuffle=True)
else:
    print('Using real-time data augmentation.')
    # This will do preprocessing and realtime data augmentation:
    datagen = ImageDataGenerator(
        featurewise_center=False,  # set input mean to 0 over the dataset
        samplewise_center=False,  # set each sample mean to 0
        featurewise_std_normalization=False,  # divide inputs by std of the dataset
        samplewise_std_normalization=False,  # divide each input by its std
        zca_whitening=False,  # apply ZCA whitening
        zca_epsilon=1e-06,  # epsilon for ZCA whitening
        rotation_range=0,  # randomly rotate images in the range (degrees, 0 to 180)
        width_shift_range=0.1,  # randomly shift images horizontally (fraction of total width)
        height_shift_range=0.1,  # randomly shift images vertically (fraction of total height)
        shear_range=0.,  # set range for random shear
        zoom_range=0.,  # set range for random zoom
        channel_shift_range=0.,  # set range for random channel shifts
        fill_mode='nearest',  # set mode for filling points outside the input boundaries
        cval=0.,  # value used for fill_mode = "constant"
        horizontal_flip=True,  # randomly flip images
        vertical_flip=False,  # randomly flip images
        rescale=None,  # set rescaling factor (applied before any other transformation)
        preprocessing_function=None,  # set function that will be applied on each input
        data_format=None,  # image data format, either "channels_first" or "channels_last"
        validation_split=0.0)  # fraction of images reserved for validation (strictly between 0 and 1)

    # Compute quantities required for feature-wise normalization
    # (std, mean, and principal components if ZCA whitening is applied).
    datagen.fit(x_train)

    # Fit the model on the batches generated by datagen.flow().
    model.fit_generator(datagen.flow(x_train, y_train,
                                     batch_size=batch_size),
                        epochs=epochs,
                        validation_data=(x_test, y_test),
                        workers=4)

# Save model and weights
if not os.path.isdir(save_dir):
    os.makedirs(save_dir)
model_path = os.path.join(save_dir, model_name)
model.save(model_path)
print('Saved trained model at %s ' % model_path)

# Score trained model.
scores = model.evaluate(x_test, y_test, verbose=1)
print('Test loss:', scores[0])
print('Test accuracy:', scores[1])
```

Wie in diesem Beispiel gezeigt, sind nur wenige Aktualisierungen des Modells erforderlich, um ein verteiltes Training mit dem Horovod-Framework zu ermöglichen. 

Beachten Sie, dass dieses Skript für Demozwecke ein relativ kleines Modell und Dataset verwendet, sodass dieses verteilte Modell nicht unbedingt eine wesentliche Leistungssteigerung aufweisen wird. Um zu erkennen, welche Leistung das verteilte Training wirklich erbringen kann, verwenden Sie ein wesentlich größeres Modell und Dataset.

## <a name="upload-the-training-script"></a>Hochladen des Trainingsskripts

Sobald das Skript fertig ist, laden Sie es im nächsten Schritt in das Dateifreigabeverzeichnis hoch, das Sie zuvor erstellt haben. Der folgende Befehl `az storage file upload` lädt es aus dem lokalen Arbeitsverzeichnis an den richtigen Ort hoch.

```azurecli-interactive
az storage file upload --path cifar --share-name myshare --source cifar_cnn_distributed.py --account-name mystorageaccount
```

## <a name="submit-the-training-job"></a>Übermitteln des Trainingsauftrags

Erstellen Sie nach Abschluss der vorherigen Schritte einen Trainingsauftrag. In Batch AI verwenden Sie eine `job.json`-Datei, um die Parameter für das Ausführen eines Auftrags zu definieren. Erstellen Sie mit Ihrem bevorzugten Texteditor eine Auftragskonfigurationsdatei mit der Bezeichnung `job.json` mit folgendem Inhalt.

```json
{
    "$schema": "https://raw.githubusercontent.com/Azure/BatchAI/master/schemas/2018-05-01/job.json",
    "properties": {
        "nodeCount": 4,
        "horovodSettings": {
            "pythonScriptFilePath": "$AZ_BATCHAI_JOB_MOUNT_ROOT/myshare/cifar/cifar_cnn_distributed.py",
            "commandLineArgs": "--dir=$AZ_BATCHAI_JOB_MOUNT_ROOT/myshare/cifar"
        },
        "stdOutErrPathPrefix": "$AZ_BATCHAI_JOB_MOUNT_ROOT/myshare/cifar",
        "mountVolumes": {
            "azureFileShares": [
                {
                    "azureFileUrl": "https://<AZURE_BATCHAI_STORAGE_ACCOUNT>.file.core.windows.net/myshare",
                    "relativeMountPath": "myshare"
                }
            ]
        },
        "jobPreparation": {
            "commandLine": "apt update; apt install mpi-default-dev mpi-default-bin -y; pip install keras horovod"
        },
        "containerSettings": {
            "imageSourceRegistry": {
                "image": "tensorflow/tensorflow:1.8.0-gpu"
            }
        }
    }
}
```

Erläuterung der Eigenschaften:

| Eigenschaft | BESCHREIBUNG |
| --------- | --------- |
| `nodeCount` | Die Anzahl der Knoten, die für den Auftrag zur Verfügung gestellt werden können. Hier wird der Auftrag parallel auf `4` Knoten ausgeführt. |
| `horovodSettings` | Das Feld `pythonScriptFilePath` definiert den Pfad zum Horovod-Skript, das sich im zuvor erstellten Verzeichnis `cifar` befindet. Das Feld `commandLineArgs` enthält die Befehlszeilenargumente für die Ausführung des Skripts. Für dieses Experiment ist das Verzeichnis, in dem das Modell gespeichert werden soll, das einzige erforderliche Argument. `$AZ_BATCHAI_JOB_MOUNT_ROOT/myshare` ist der Pfad, in dem die Dateifreigabe bereitgestellt wurde. | 
| `stdOutErrPathPrefix` | Der Pfad zum Speichern der Auftragsausgaben und Protokolle, der in diesem Beispiel das gleiche Verzeichnis `cifar` ist. |
| `jobPreparation` | Besondere Anweisungen zur Vorbereitung der Umgebung für die Ausführung des Auftrags. Dieses Skript erfordert die Installation der angegebenen MPI- und Horovod-Pakete. |
| `containerSettings` | Einstellungen für den Container, auf dem der Auftrag ausgeführt wird. Dieser Auftrag verwendet einen mit `tensorflow` erstellten Docker-Container.

Erstellen Sie mit der Konfiguration den Auftrag mithilfe des Befehls `az batchai job create`. Der folgende Befehl stellt einen neuen Auftrag mit dem Namen `cifar_distributed` in die Warteschlange, wobei alle Ressourcen verwendet werden, die bis zu diesem Zeitpunkt eingerichtet wurden. 

```azurecli-interactive
az batchai job create --cluster nc6cluster --name cifar_distributed --resource-group batchai.horovod --workspace batchaidev --experiment cifar --config-file job.json --storage-account-name mystorageaccount
```

Wenn die Knoten derzeit ausgelastet sind, dauert es möglicherweise eine Weile, bis die Ausführung des Auftrags startet. Verwenden Sie den Befehl `az batchai job show`, um den Ausführungsstatus des Auftrags anzuzeigen.

```azurecli-interactive
az batchai job show --experiment cifar --name cifar_distributed --resource-group batchai.horovod --workspace batchaidev --query "executionState"
```

### <a name="visualize-the-distributed-training"></a>Visualisieren des verteilten Trainings

Sobald der Auftrag ausgeführt wird, verwenden Sie den Befehl `az batchai cluster show` erneut, um den Status der Clusterknoten abzufragen. 

```azurecli-interactive
az batchai cluster show --name nc6cluster --workspace batchaidev --resource-group batchai.horovod --query "nodeStateCounts"
```

Die Ausgabe sollte ähnlich wie folgt aussehen. Es werden alle vier Knoten im laufenden Betrieb anzeigt. Dieses Ergebnis zeigt, dass alle vier Knoten derzeit im verteilten Training genutzt werden.

```
{
  "idleNodeCount": 0,
  "leavingNodeCount": 0,
  "preparingNodeCount": 0,
  "runningNodeCount": 4,
  "unusableNodeCount": 0
}
```

## <a name="monitor-the-job"></a>Überwachen des Auftrags

### <a name="list-output-files"></a>Auflisten der Ausgabedateien

Verwenden Sie während der Ausführung des Auftrags den Befehl `az batchai job file list`, um die Ausgabedateien aufzulisten, die der Auftrag generiert.

```azurecli-interactive
az batchai job file list --experiment cifar --job cifar_distributed --resource-group batchai.horovod --workspace batchaidev --output table
```

Für dieses spezifische Experiment sollte die Ausgabe in etwa wie folgt aussehen. Die Gesamtausgabe für den Auftrag wird in `stdout.txt` protokolliert, während `stderr.txt` alle Fehler ausgibt, die bei der Hauptausführung auftreten. Die anderen Dateien sind Ausgabe-, Fehler- und Auftragsvorbereitungsprotokolle für die einzelnen Knoten.

```
Name                                                    Type       Size  Modified
------------------------------------------------------  ------  -------  -------------------------
execution-tvm-676767296_1-20180718t174802z-p.log        file       8801  2018-07-18T22:41:28+00:00
execution-tvm-676767296_2-20180718t174802z-p.log        file      15094  2018-07-18T22:41:55+00:00
execution-tvm-676767296_3-20180718t174802z-p.log        file       8801  2018-07-18T22:41:28+00:00
execution-tvm-676767296_4-20180718t174802z-p.log        file       8801  2018-07-18T22:41:28+00:00
stderr-job_prep-tvm-676767296_1-20180718t174802z-p.txt  file        238  2018-07-18T22:41:50+00:00
stderr-job_prep-tvm-676767296_2-20180718t174802z-p.txt  file        238  2018-07-18T22:41:50+00:00
stderr-job_prep-tvm-676767296_3-20180718t174802z-p.txt  file        238  2018-07-18T22:41:50+00:00
stderr-job_prep-tvm-676767296_4-20180718t174802z-p.txt  file        238  2018-07-18T22:41:50+00:00
stderr.txt                                              file       7653  2018-07-18T22:46:32+00:00
stdout-job_prep-tvm-676767296_1-20180718t174802z-p.txt  file      13651  2018-07-18T22:41:55+00:00
stdout-job_prep-tvm-676767296_2-20180718t174802z-p.txt  file      13651  2018-07-18T22:41:54+00:00
stdout-job_prep-tvm-676767296_3-20180718t174802z-p.txt  file      13651  2018-07-18T22:41:54+00:00
stdout-job_prep-tvm-676767296_4-20180718t174802z-p.txt  file      13651  2018-07-18T22:41:55+00:00
stdout.txt                                              file    2316480  2018-07-18T22:46:32+00:00
```

### <a name="stream-an-output-file"></a>Streamen einer Ausgabedatei

Verwenden Sie den Befehl `az batchai job file stream`, um den Inhalt der Datei zu streamen. Im folgenden Beispiel wird das Hauptausgabeprotokoll gestreamt.

```azurecli-interactive
az batchai job file stream --experiment cifar --file-name stdout.txt --job cifar_distributed --resource-group batchai.horovod --workspace batchaidev
```

Während der Auftrag ausgeführt wird, streamt der Befehl die Standardausgabe des Trainingsauftrags und zeigt eine Ausgabe ähnlich der folgenden.

```
...
50000 train samples
10000 test samples
Using real-time data augmentation.
Epoch 1/25


   1/1563 [..............................] - ETA: 2:42:25 - loss: 2.3334 - acc: 0.0312   1/1563 [..............................] - ETA: 2:30:42 - loss: 2.2973 - acc: 0.0938
   1/1563 [..............................] - ETA: 30:36 - loss: 2.3175 - acc: 0.1250
   1/1563 [..............................] - ETA: 2:32:58 - loss: 2.3489 - acc: 0.0625
   2/1563 [..............................] - ETA: 1:21:59 - loss: 2.3230 - acc: 0.0625

   2/1563 [..............................]   2/1563 [..............................] - ETA: 1:16:09 - loss: 2.2913 - acc: 0.0938 - ETA: 1:17:15 - loss: 2.3147 - acc: 0.0781
   2/1563 [..............................] - ETA: 16:07 - loss: 2.3678 - acc: 0.0938
   3/1563 [..............................] - ETA: 55:05 - loss: 2.3232 - acc: 0.0938  
   3/1563 [..............................] - ETA: 51:57 - loss: 2.3185 - acc: 0.1146  
   3/1563 [..............................] - ETA: 51:12 - loss: 2.3179 - acc: 0.1042  
   3/1563 [..............................] - ETA: 11:13 - loss: 2.3504 - acc: 0.0833
   4/1563 [..............................] - ETA: 39:43 - loss: 2.3224 - acc: 0.1094
   4/1563 [..............................] - ETA: 42:09 - loss: 2.3049 - acc: 0.1250
   4/1563 [..............................] - ETA: 39:15 - loss: 2.3089 - acc: 0.1094
   4/1563 [..............................] - ETA: 9:16 - loss: 2.3316 - acc: 0.1016 
   5/1563 [..............................] - ETA: 39:51 - loss: 2.3153 - acc: 0.1125
   5/1563 [..............................] - ETA: 37:58 - loss: 2.3197 - acc: 0.1125
   5/1563 [..............................] - ETA: 37:35 - loss: 2.3148 - acc: 0.1062
   5/1563 [..............................] - ETA: 13:38 - loss: 2.3263 - acc: 0.1062
   6/1563 [..............................] - ETA: 35:48 - loss: 2.3168 - acc: 0.1198

   6/1563 [..............................]   6/1563 [..............................] - ETA: 34:13 - loss: 2.3142 - acc: 0.1198 - ETA: 33:51 - loss: 2.3162 - acc: 0.1042
   6/1563 [..............................] - ETA: 13:54 - loss: 2.3225 - acc: 0.1094
   7/1563 [..............................] - ETA: 30:53 - loss: 2.3181 - acc: 0.1071

   7/1563 [..............................]   7/1563 [..............................] - ETA: 29:32 - loss: 2.3149 - acc: 0.1161 - ETA: 29:13 - loss: 2.3140 - acc: 0.0938
   7/1563 [..............................] - ETA: 12:09 - loss: 2.3174 - acc: 0.1205
   8/1563 [..............................] - ETA: 26:04 - loss: 2.3113 - acc: 0.1133
   8/1563 [..............................] - ETA: 27:15 - loss: 2.3169 - acc: 0.1133
   8/1563 [..............................] - ETA: 10:51 - loss: 2.3152 - acc: 0.1172
...
```

Das Skript trainiert über 25 Epochen oder durchläuft das Trainingsdataset. Dieser Vorgang dauert ca. 60 Minuten. 

## <a name="retrieve-the-results"></a>Abrufen der Ergebnisse

Wenn das Skript abgeschlossen ist und alles funktioniert hat, sollte die Validierungsgenauigkeit etwa 70-75 % betragen und das trainierte Modell wird auf der Dateifreigabe bei `cifar/saved_models/keras_cifar10_trained_model.h5` gespeichert. 

Das Modelltraining ist in der Regel Teil eines größeren Workflows. Beispielsweise können Sie das trainierte Modell in einer anderen Anwendung verfügbar machen. Um das trainierte Modell lokal herunterzuladen, verwenden Sie den Befehl `az storage file download`. 

```azurecli-interactive
az storage file download --path cifar/saved_models/keras_cifar10_trained_model.h5 --share-name myshare --account-name mystorageaccount
```
## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Sobald die Ausführung von Aufträgen abgeschlossen ist, ist eine bewährte Methode zur Einsparung von Computekosten, alle Cluster auf `0 nodes` herunterzuskalieren, damit Ihnen keine Kosten für Leerlaufzeiten entstehen. Verwenden Sie den folgenden Befehl `az batchai cluster resize`. 

```azurecli-interactive
az batchai cluster resize --name nc6cluster --resource-group batchai.horovod --target 0 --workspace batchaidev
```

Ändern Sie die Größe später auf 1 oder mehr Knoten, um Ihre Aufträge auszuführen. 

Wenn Sie nicht planen, den Arbeitsbereich und das Speicherkonto in Zukunft zu verwenden, löschen Sie die Ressourcengruppe mit dem Befehl `az group delete`. Durch das Löschen einer Ressourcengruppe werden auch alle Ressourcen in dieser Ressourcengruppe gelöscht.

```azurecli-interactive
az group delete --name batchai.horovod
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Einrichten eines Batch AI-Arbeitsbereichs, eines Experiments und eines Clusters
> * Einrichten einer Azure-Dateifreigabe für die Eingabe und Ausgabe
> * Parallelisieren eines Modells mit Horovod
> * Übermitteln eines Trainingsauftrags
> * Überwachen des Auftrags
> * Abrufen der Trainingsergebnisse

Beispiele für die Verwendung von Batch AI mit unterschiedlichen Frameworks finden Sie in den Rezepten auf GitHub.

> [!div class="nextstepaction"]
> [Batch AI-Rezepte](https://github.com/Azure/BatchAI/tree/master/recipes)
