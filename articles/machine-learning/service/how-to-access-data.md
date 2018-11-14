---
title: Verwenden von Datenspeichern in Azure Machine Learning für den Zugriff auf Daten
description: Verwenden von Datenspeichern für den Zugriff auf gespeicherte Daten während des Trainings
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: minxia
author: mx-iao
ms.reviewer: sgilley
ms.date: 09/24/2018
ms.openlocfilehash: 79e26d4d2cf5743abae6dc0f1fb58585e1b9b9db
ms.sourcegitcommit: 1fc949dab883453ac960e02d882e613806fabe6f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2018
ms.locfileid: "50977905"
---
# <a name="how-to-access-data-during-training"></a>Zugreifen auf Daten während des Trainings
Verwenden Sie einen Datenspeicher, um auf Ihre Daten in den Azure Machine Learning-Workflows zuzugreifen und mit diesen zu arbeiten.

In Azure Machine Learning Services stellt ein Datenspeicher eine Abstraktion von [Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-introduction) dar. Der Datenspeicher kann entweder auf einen [Azure-Blobcontainer](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction) oder eine [Azure-Dateifreigabe](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) als zugrunde liegenden Speicher verweisen. 

## <a name="create-a-datastore"></a>Erstellen eines Datenspeichers
Wenn Sie einen Datenspeicher verwenden möchten, benötigen Sie zunächst einen [Arbeitsbereich](concept-azure-machine-learning-architecture.md#workspace). Sie können entweder einen [neuen Arbeitsbereich erstellen](quickstart-create-workspace-with-python.md) oder einen vorhandenen Arbeitsbereich abrufen:

```Python
import azureml.core
from azureml.core import Workspace

ws = Workspace.from_config()
```

### <a name="use-the-default-datastore"></a>Verwenden des Standarddatenspeichers
Sie müssen kein Speicherkonto erstellen oder konfigurieren.  Jeder Arbeitsbereich verfügt über einen Standarddatenspeicher, den Sie sofort verwenden können.

So rufen Sie den Standarddatenspeicher des Arbeitsbereichs ab:
```Python
ds = ws.get_default_datastore()
```

### <a name="register-a-datastore"></a>Registrieren eines Datenspeichers
Wenn Sie bereits über eine Azure Storage-Instanz verfügen, können Sie diesen Speicher als Datenspeicher in Ihrem Arbeitsbereich registrieren. Sie haben auch die Möglichkeit, einen Azure-Blobcontainer oder eine Azure-Dateifreigabe als Datenspeicher zu registrieren. Alle Registriermethoden befinden sich in der `Datastore`-Klasse und weisen das Format `register_azure_*` auf.

#### <a name="azure-blob-container-datastore"></a>Azure-Blobcontainer-Datenspeicher
So registrieren Sie einen Azure-Blobcontainer-Datenspeicher:

```Python
ds = Datastore.register_azure_blob_container(workspace=ws, 
                                             datastore_name='your datastore name', 
                                             container_name='your azure blob container name',
                                             account_name='your storage account name', 
                                             account_key='your storage account key',
                                             create_if_not_exists=True)
```

#### <a name="azure-file-share-datastore"></a>Azure-Dateifreigabe-Datenspeicher
So registrieren Sie einen Azure-Dateifreigabe-Datenspeicher:

```Python
ds = Datastore.register_azure_file_share(workspace=ws, 
                                         datastore_name='your datastore name', 
                                         container_name='your file share name',
                                         account_name='your storage account name', 
                                         account_key='your storage account key',
                                         create_if_not_exists=True)
```

### <a name="get-an-existing-datastore"></a>Abrufen eines vorhandenen Datenspeichers
So fragen Sie einen registrierten Datenspeicher anhand des Namens ab:
```Python
ds = Datastore.get(ws, datastore_name='your datastore name')
```

Sie können auch alle Datenspeicher für einen Arbeitsbereich abrufen:
```Python
datastores = ws.datastores()
for name, ds in datastores.items(),
    print(name, ds.datastore_type)"
```

Legen Sie der Einfachheit halber einen Ihrer registrierten Datenspeicher als Standarddatenspeicher für Ihren Arbeitsbereich fest:
```Python
ws.set_default_datastore('your datastore name')
```

## <a name="upload-and-download-data"></a>Hochladen und Herunterladen von Daten
### <a name="upload"></a>Hochladen
Laden Sie entweder ein Verzeichnis oder einzelne Dateien mit dem Python SDK in den Datenspeicher hoch.

So laden Sie ein Verzeichnis in einen Datenspeicher `ds` hoch:
```Python
ds.upload(src_dir='your source directory',
          target_path='your target path',
          overwrite=True,
          show_progress=True)
```
`target_path` gibt den Speicherort in der Dateifreigabe (oder im Blobcontainer) für den Upload an. Standardmäßig lautet er `None`. In diesem Fall werden die Daten in den Stamm hochgeladen. `overwrite=True` überschreibt alle vorhandenen Daten in `target_path`.

Alternativ können Sie eine Liste einzelner Dateien mithilfe der `upload_files()`-Methode des Datenspeichers in den Datenspeicher hochladen.

### <a name="download"></a>Download
Ebenso lassen sich Daten aus einem Datenspeicher in Ihr lokales Dateisystem herunterladen.

```Python
ds.download(target_path='your target path',
            prefix='your prefix',
            show_progress=True)
```
`target_path` ist der Speicherort des lokalen Verzeichnisses, in das die Daten heruntergeladen werden sollen. Um einen Pfad zu dem Ordner in der Dateifreigabe (oder im Blobcontainer) für den Download anzugeben, geben Sie diesen Pfad mit `prefix` an. Wenn `prefix` gleich `None` ist, werden alle Inhalte Ihrer Dateifreigabe (oder Ihres Blobcontainers) heruntergeladen.

## <a name="access-datastores-for-training"></a>Zugreifen auf Datenspeicher für das Training
Sie können während einer Trainingsausführung mit dem Python SDK auf einen Datenspeicher auf einem Remotecomputeziel zugreifen (z.B. zum Abrufen von Trainings- oder Überprüfungsdaten). 

Es gibt zwei unterstützte Methoden, um Ihren Datenspeicher auf der Remotecomputeressource zur Verfügung zu stellen:
* **Einbinden**  
`ds.as_mount()`: Durch Angabe dieses Einbindungsmodus wird der Datenspeicher für Sie in die Remotecomputeressource eingebunden. 
* **Herunterladen/Hochladen**  
    * `ds.as_download(path_on_compute='your path on compute')`: Durch diese Angabe werden Daten aus Ihrem Datenspeicher auf die Remotecomputeressource in den durch `path_on_compute` angegebenen Speicherort heruntergeladen.
    * `ds.as_upload(path_on_compute='yourfilename'`: Durch diese Angabe werden Daten in den Datenspeicher hochgeladen.  Angenommen, Ihr Trainingsskript erstellt eine `foo.pkl`-Datei im aktuellen Arbeitsverzeichnis auf der Remotecomputeressource. Wenn das Skript die Datei erstellt hat, laden Sie sie mit `ds.as_upload(path_on_compute='./foo.pkl')` in Ihren Datenspeicher hoch. Die Datei wird in den Stamm Ihres Datenspeichers hochgeladen.
    
Um auf einen bestimmten Ordner oder eine bestimmte Datei in Ihrem Datenspeicher zu verweisen, verwenden Sie die Datenspeicherfunktion **`path`**. Um beispielsweise den Inhalt des Verzeichnisses `./bar` aus dem Datenspeicher in Ihr Computeziel herunterzuladen, verwenden Sie `ds.path('./bar').as_download()`.

Jedes `ds`- oder `ds.path`-Objekt wird in den Namen einer Umgebungsvariablen des Formats `"$AZUREML_DATAREFERENCE_XXXX"` aufgelöst, deren Wert für den Einbindungs-/Downloadpfad auf der Remotecomputeressource steht. Der Datenspeicherpfad auf der Remotecomputeressource stimmt möglicherweise nicht mit dem Ausführungspfad für das Skript überein.

Um während des Trainings auf Ihren Datenspeicher zuzugreifen, übergeben Sie ihn mit `script_params` als Befehlszeilenargument an das Trainingsskript:

```Python
from azureml.train.estimator import Estimator

script_params = {
    '--data_dir': ds.as_mount()
}

est = Estimator(source_directory='your code directory',
                script_params=script_params,
                compute_target=compute_target,
                entry_script='train.py')
```
`as_mount()` ist der Standardmodus für einen Datenspeicher, daher können Sie `ds` auch direkt an das `'--data_dir'`-Argument übergeben.

Alternativ können Sie eine Liste von Datenspeichern an den Parameter `inputs` des Estimatorkonstruktors übergeben, um Einbindungs- oder Kopiervorgänge in oder aus Ihren Datenspeichern auszuführen:

```Python
est = Estimator(source_directory='your code directory',
                compute_target=compute_target,
                entry_script='train.py',
                inputs=[ds1.as_download(), ds2.path('./foo').as_download(), ds3.as_upload(path_on_compute='./bar.pkl')])
```
Der obige Code führt Folgendes aus:
* Er lädt den gesamten Inhalt des Datenspeichers `ds1` auf die Remotecomputeressource herunter, bevor Ihr Trainingsskript `train.py` ausgeführt wird.
* Er lädt den Ordner `'./foo'` im Datenspeicher `ds2` auf die Remotecomputeressource herunter, bevor `train.py` ausgeführt wird.
* Er lädt die Datei `'./bar.pkl'` aus der Remotecomputeressource in den Datenspeicher `d3` hoch, nachdem das Skript ausgeführt wurde.

## <a name="next-steps"></a>Nächste Schritte
* [Trainieren eines Modells](how-to-train-ml-models.md)
