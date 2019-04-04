---
title: Zugreifen auf Daten in Datenspeichern/Blobs für das Training
titleSuffix: Azure Machine Learning service
description: Erfahren Sie, wie Sie mithilfe von Datenspeichern während des Trainings mit dem Azure Machine Learning Service auf den Blobdatenspeicher zugreifen können.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: minxia
author: mx-iao
ms.reviewer: sgilley
ms.date: 02/25/2019
ms.custom: seodec18
ms.openlocfilehash: e6e1b304b90b37c93bed22bcb720a646680ee083
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58223613"
---
# <a name="access-data-from-your-datastores"></a>Zugreifen auf Daten aus Ihren Datenspeichern

Wenn Sie Datenspeicher verwenden, können Sie mit Ihren Daten interagieren und auf diese zugreifen, unabhängig davon, ob Sie Ihren Code lokal auf einem Computecluster oder auf einem virtuellen Computer ausführen. In diesem Artikel werden die Azure Machine Learning-Workflows erläutert, die den Zugriff auf Ihre Datenspeicher und deren Verfügbarkeit für Ihren Rechenkontext sicherstellen.

In dieser Vorgehensweise finden Sie Beispiele für die folgenden Aufgaben:
* [Auswählen eines Datenspeichers](#access)
* [Abrufen von Daten](#get)
* [Hochladen und herunterladen von Daten in Datenspeicher](#up-and-down)
* [Zugreifen auf Datenspeicher während des Trainings](#train)

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie Datenspeicher verwenden möchten, benötigen Sie zunächst einen [Arbeitsbereich](concept-azure-machine-learning-architecture.md#workspace). 

Sie können entweder einen [neuen Arbeitsbereich erstellen](quickstart-create-workspace-with-python.md) oder einen vorhandenen Arbeitsbereich abrufen:

```Python
import azureml.core
from azureml.core import Workspace, Datastore

ws = Workspace.from_config()
```

Alternativ können Sie [dieser Schnellstartanleitung für Python folgen](quickstart-create-workspace-with-python.md), um das SDK zur Erstellung Ihres Arbeitsbereichs und für erste Schritte zu verwenden.

<a name="access"></a>

## <a name="choose-a-datastore"></a>Auswählen eines Datenspeichers

Sie können einen Standarddatenspeicher oder Ihren eigenen verwenden.

### <a name="use-the-default-datastore-in-your-workspace"></a>Verwenden des Standarddatenspeichers in Ihrem Arbeitsbereich

Sie müssen kein Speicherkonto erstellen oder konfigurieren, da jeder Arbeitsbereich über einen Standarddatenspeicher verfügt. Sie können diesen Datenspeicher sofort verwenden, da er bereits im Arbeitsbereich registriert ist. 

So rufen Sie den Standarddatenspeicher des Arbeitsbereichs ab:
```Python
ds = ws.get_default_datastore()
```

### <a name="register-your-own-datastore-with-the-workspace"></a>Registrieren Ihres eigenen Datenspeichers im Arbeitsbereich
Wenn Sie bereits über eine Azure Storage-Instanz verfügen, können Sie diesen Speicher als Datenspeicher in Ihrem Arbeitsbereich registrieren.   Alle Registriermethoden befinden sich in der [`Datastore`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py)-Klasse und weisen das Format „register_azure_*“ auf. 

In den folgenden Beispielen wird gezeigt, wie Sie einen Azure-Blobcontainer oder eine Azure-Dateifreigabe als Datenspeicher registrieren können.

+ Verwenden Sie [`register_azure_blob-container()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py) für einen **Azure-Blobcontainer-Datenspeicher**.

  ```Python
  ds = Datastore.register_azure_blob_container(workspace=ws, 
                                               datastore_name='your datastore name', 
                                               container_name='your azure blob container name',
                                               account_name='your storage account name', 
                                               account_key='your storage account key',
                                               create_if_not_exists=True)
  ```

+ Verwenden Sie [`register_azure_file_share()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#register-azure-file-share-workspace--datastore-name--file-share-name--account-name--sas-token-none--account-key-none--protocol-none--endpoint-none--overwrite-false--create-if-not-exists-false--skip-validation-false-) für einen **Azure-Dateifreigabe-Datenspeicher**. Beispiel:  
  ```Python
  ds = Datastore.register_azure_file_share(workspace=ws, 
                                           datastore_name='your datastore name', 
                                           container_name='your file share name',
                                           account_name='your storage account name', 
                                           account_key='your storage account key',
                                           create_if_not_exists=True)
  ```

<a name="get"></a>

## <a name="find--define-datastores"></a>Suchen und definieren von Datenspeichern

Verwenden Sie [`get()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#get-workspace--datastore-name-), um einen bestimmten Datenspeicher im aktuellen Arbeitsbereich zu registrieren:

```Python
#get named datastore from current workspace
ds = Datastore.get(ws, datastore_name='your datastore name')
```

Verwenden Sie diesen Code, um eine Liste aller Datenspeicher in einem bestimmten Arbeitsbereich anzuzeigen:

```Python
#list all datastores registered in current workspace
datastores = ws.datastores
for name, ds in datastores.items():
    print(name, ds.datastore_type)
```

Verwenden Sie [`set_default_datastore()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py#set-default-datastore-name-), um einen anderen Standarddatenspeicher für den aktuellen Arbeitsbereich zu definieren:

```Python
#define default datastore for current workspace
ws.set_default_datastore('your datastore name')
```

<a name="up-and-down"></a>
## <a name="upload--download-data"></a>Hoch- und herunterladen von Daten
Die in den folgenden Beispielen beschriebenen [`upload()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py#download-target-path--prefix-none--overwrite-false--show-progress-true-)- und [`download()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py#download-target-path--prefix-none--overwrite-false--show-progress-true-)-Methoden sind für die [AzureBlobDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py)- und [AzureFileDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azurefiledatastore?view=azure-ml-py)-Klassen bestimmt und werden identisch ausgeführt.

### <a name="upload"></a>Hochladen

 Laden Sie entweder ein Verzeichnis oder einzelne Dateien mit dem Python SDK in den Datenspeicher hoch.

So laden Sie ein Verzeichnis in einen Datenspeicher `ds` hoch:

```Python
import azureml.data
from azureml.data import AzureFileDatastore, AzureBlobDatastore

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

<a name="train"></a>
## <a name="access-datastores-during-training"></a>Zugreifen auf Datenspeicher während des Trainings

Wenn Sie Ihren Datenspeicher auf der Remotecomputeressource bereitgestellt haben, können Sie während des Trainings auf diesen zugreifen (z. B. für Trainings- oder Überprüfungsdaten), indem Sie einfach den Pfad als Parameter in Ihrem Trainingsskript übergeben.

In der folgenden Liste sind die allgemeinen [`DataReference`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py)-Methoden aufgelistet, durch die Datenspeicher auf der Remotecomputeressource bereitgestellt werden.

# #

Weg|Methode|BESCHREIBUNG
----|-----|--------
Einbinden| [`as_mount()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-mount--)| Stellt bei Verwendung einen Datenspeicher auf der Remotecomputeressource bereit. Standardmodus für Datenspeicher.
Download|[`as_download()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-)|Lädt bei Verwendung Daten aus dem von `path_on_compute` angegebenen Speicherort auf Ihrem Datenspeicher auf die Remotecomputeressource herunter.
Hochladen|[`as_upload()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)| Lädt bei Verwendung Daten aus dem von `path_on_compute` angegebenen Speicherort in das Stammverzeichnis Ihres Datenspeichers hoch.

```Python
import azureml.data
from azureml.data import DataReference

ds.as_mount()
ds.as_download(path_on_compute='your path on compute')
ds.as_upload(path_on_compute='yourfilename')
```  

Um auf einen bestimmten Ordner oder eine bestimmte Datei in Ihrem Datenspeicher zu verweisen, verwenden Sie die Datenspeicherfunktion [`path()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#path-path-none--data-reference-name-none-).

```Python
#download the contents of the `./bar` directory from the datastore to the remote compute
ds.path('./bar').as_download()
```



> [!NOTE]
> Jedes `ds`- oder `ds.path`-Objekt wird in den Namen einer Umgebungsvariablen des Formats `"$AZUREML_DATAREFERENCE_XXXX"` aufgelöst, deren Wert für den Einbindungs-/Downloadpfad auf der Remotecomputeressource steht. Der Datenspeicherpfad auf der Remotecomputeressource stimmt möglicherweise nicht mit dem Ausführungspfad für das Trainingsskript überein.

### <a name="examples"></a>Beispiele 

Im Folgenden sind für die [`Estimator`](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py)-Klasse spezifische Beispiele dargestellt, wie Sie während des Trainings auf Ihren Datenspeicher zugreifen können.


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

Da `as_mount()` der Standardmodus für einen Datenspeicher ist, haben Sie auch die Möglichkeit, `ds` direkt an das `'--data_dir'`-Argument zu übergeben.

Alternativ können Sie eine Liste von Datenspeichern an den Parameter `inputs` des Estimatorkonstruktors übergeben, um Einbindungs- oder Kopiervorgänge in oder aus Ihrem/n Datenspeicher(n) auszuführen. Dieses Codebeispiel:
* lädt alle Inhalte im `ds1`-Datenspeicher auf die Remotecomputeressource herunter, bevor das Trainingsskript `train.py` ausgeführt wird.
* lädt den Ordner `'./foo'` im `ds2`-Datenspeicher auf die Remotecomputeressource herunter, bevor `train.py` ausgeführt wird.
* lädt die Datei `'./bar.pkl'` aus der Remotecomputeressource in den Datenspeicher `ds3` hoch, nachdem Ihr Skript ausgeführt wurde.

```Python
est = Estimator(source_directory='your code directory',
                compute_target=compute_target,
                entry_script='train.py',
                inputs=[ds1.as_download(), ds2.path('./foo').as_download(), ds3.as_upload(path_on_compute='./bar.pkl')])
```


## <a name="next-steps"></a>Nächste Schritte

* [Trainieren eines Modells](how-to-train-ml-models.md)

* [Bereitstellen eines Modells](how-to-deploy-and-where.md)
