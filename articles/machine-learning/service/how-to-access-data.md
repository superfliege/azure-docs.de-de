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
ms.openlocfilehash: 615ab592c040eedf7d31e3a3036f558ea6c09740
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46990806"
---
# <a name="how-to-access-data-during-training"></a>Zugreifen auf Daten während des Trainings
In Azure Machine Learning Services ist ein Datenspeicher eine Abstraktion von [Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-introduction). Der Datenspeicher kann entweder auf einen [Azure-Blobcontainer](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction) oder eine [Azure-Dateifreigabe](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) als zugrunde liegenden Speicher verweisen. Mit Datenspeichern können Sie während Ihrer Azure Machine Learning-Workflows auf einfache Weise über das Python SDK oder die Befehlszeilenschnittstelle auf Ihren gespeicherten Daten zugreifen und damit interagieren.

## <a name="create-a-datastore"></a>Erstellen eines Datenspeichers
Um Datenspeicher verwenden zu können, brauchen Sie zuerst einen [Arbeitsbereich](concept-azure-machine-learning-architecture.md#workspace). Sie können entweder einen neuen Arbeitsbereich erstellen oder einen vorhandenen Arbeitsbereich abrufen:

```Python
import azureml.core
from azureml.core import Workspace

ws = Workspace.from_config()
```

### <a name="use-the-default-datastore"></a>Verwenden des Standarddatenspeichers
Jeder Arbeitsbereich verfügt über einen Standarddatenspeicher, den Sie sofort verwenden können, sodass Sie keine eigenen Speicherkonten erstellen und konfigurieren müssen.

So rufen Sie den Standarddatenspeicher des Arbeitsbereichs ab:
```Python
ds = ws.get_default_datastore()
```

### <a name="register-a-datastore"></a>Registrieren eines Datenspeichers
Wenn Sie Azure Storage bereits verwenden, können Sie diesen Speicher als Datenspeicher in Ihrem Arbeitsbereich registrieren. Azure Machine Learning bietet die Funktionalität zum Registrieren eines Azure-Blobcontainers oder einer Azure-Dateifreigabe als Datenspeicher. Alle Registriermethoden befinden sich in der `Datastore`-Klasse und weisen das Format `register_azure_*` auf.

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

Wenn Sie einen Ihrer registrierten Datenspeicher der Einfachheit halber als Standarddatenspeicher für Ihren Arbeitsbereich festlegen möchten, gehen Sie wie folgt vor:
```Python
ws.set_default_datastore('your datastore name')
```

## <a name="upload-and-download-data"></a>Hochladen und Herunterladen von Daten
### <a name="upload"></a>Hochladen
Sie können entweder ein Verzeichnis oder einzelne Dateien über das Python SDK in den Datenspeicher hochladen.

So laden Sie ein Verzeichnis in einen Datenspeicher `ds` hoch:
```Python
ds.upload(src_dir='your source directory',
          target_path='your target path',
          overwrite=True,
          show_progress=True)
```
`target_path` gibt den Speicherort in der Dateifreigabe (oder im Blobcontainer) für den Upload an. Standardmäßig lautet er `None`. In diesem Fall werden die Daten in den Stamm hochgeladen. `overwrite=True` überschreibt alle vorhandenen Daten in `target_path`.

Über die `upload_files()`-Methode des Datenspeichers können Sie auch eine Liste einzelner Dateien in den Datenspeicher hochladen.

### <a name="download"></a>Download
Auf ähnliche Weise können Sie Daten aus einem Datenspeicher in Ihr lokales Dateisystem herunterladen.

```Python
ds.download(target_path='your target path',
            prefix='your prefix',
            show_progress=True)
```
`target_path` ist der Speicherort des lokalen Verzeichnisses, in das die Daten heruntergeladen werden sollen. Um einen Pfad zu dem Ordner in der Dateifreigabe (oder im Blobcontainer) für den Download anzugeben, geben Sie diesen Pfad mit `prefix` an. Wenn `prefix` gleich `None` ist, werden alle Inhalte Ihrer Dateifreigabe (oder Ihres Blobcontainers) heruntergeladen.

## <a name="access-datastores-for-training"></a>Zugreifen auf Datenspeicher für das Training
Sie können während einer Trainingsausführung über das Python SDK auf einen Datenspeicher auf einem Remotecomputeziel zugreifen (z.B. zum Abrufen von Trainings- oder Überprüfungsdaten). 

Es gibt zwei unterstützte Methoden, um Ihren Datenspeicher auf der Remotecomputeressource zur Verfügung zu stellen:
* **Einbinden**  
`ds.as_mount()`: Durch Angabe dieses Einbindungsmodus wird der Datenspeicher für Sie in die Remotecomputeressource eingebunden. 
* **Herunterladen/Hochladen**  
    * `ds.as_download(path_on_compute='your path on compute')`: Mit diesem Downloadmodus werden die Daten von Ihrem Datenspeicher auf die Remotecomputeressource in den durch `path_on_compute` angegebenen Speicherort heruntergeladen.
    * Umgekehrt können Sie Daten, die in Ihrer Trainingsausführung erstellt wurden, auch in einen Datenspeicher hochladen. Wenn Ihr Trainingsskript beispielsweise eine Datei `foo.pkl` im aktuellen Arbeitsverzeichnis auf der Remotecomputeressource erstellt, können Sie angeben, dass diese nach Ausführung des Skripts in Ihren Datenspeicher hochgeladen wird: `ds.as_upload(path_on_compute='./foo.pkl')`. Dadurch wird die Datei in den Stamm Ihres Datenspeichers hochgeladen.
    
Wenn Sie auf einen bestimmten Ordner oder eine Datei in Ihrem Datenspeicher verweisen möchten, können Sie die Funktion **`path`** des Datenspeichers verwenden. Wenn Ihr Datenspeicher beispielsweise ein Verzeichnis mit dem relativen Pfad `./bar` enthält und Sie nur den Inhalt dieses Ordners in das Computeziel herunterladen möchten, gehen Sie wie folgt vor: `ds.path('./bar').as_download()`

Jedes `ds`- oder `ds.path`-Objekt wird in den Namen einer Umgebungsvariablen des Formats `"$AZUREML_DATAREFERENCE_XXXX"` aufgelöst, deren Wert für den Einbindungs-/Downloadpfad auf der Remotecomputeressource steht. Der Datenspeicherpfad auf der Remotecomputeressource stimmt möglicherweise nicht mit dem Ausführungspfad für das Skript überein.

Um während des Trainings auf Ihren Datenspeicher zuzugreifen, können Sie ihn mit `script_params` als Befehlszeilenargument an das Trainingsskript übergeben:

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

Alternativ dazu können Sie eine Liste von Datenspeichern an den Parameter `inputs` des Estimator-Konstruktors übergeben, um Einbindungs- oder Kopiervorgänge in oder aus Ihren Datenspeichern durchzuführen:

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
