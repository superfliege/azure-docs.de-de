---
title: Erstellen und Registrieren von Datasets in Ihrem Arbeitsbereich
titleSuffix: Azure Machine Learning service
description: Erfahren Sie, wie Sie Datasets aus verschiedenen Quellen erstellen und Datasets in Ihrem Arbeitsbereich registrieren.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 05/02/19
ms.openlocfilehash: 938f13524b22f34f4becc936885d1611cb854df1
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2019
ms.locfileid: "65510505"
---
# <a name="create-and-register-azure-machine-learning-datasets-preview"></a>Erstellen und Registrieren von Azure Machine Learning-Datasets (Vorschau)

In diesem Artikel lernen Sie die Azure Machine Learning-Workflows zum Erstellen und Registrieren von Datasets kennen, und Sie erfahren, wie Sie zur Wiederverwendung über lokale und Remoteexperimente auf sie zugreifen.

Azure Machine Learning-Datasets (Vorschau) erleichtern Ihnen den Zugriff auf Ihre Daten und die Arbeit mit ihnen. Datasets verwalten Daten in verschiedenen Szenarien, z.B. Modelltraining und Pipelineerstellung. Mithilfe des [Azure Machine Learning-SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) können Sie mit Daten in gängigen Formaten arbeiten, auf zugrunde liegenden Speicher zugreifen, Daten untersuchen und vorbereiten, den Lebenszyklus unterschiedlicher Datasetdefinitionen verwalten und zwischen im Training und in der Produktion verwendeten Datasets vergleichen.

## <a name="prerequisites"></a>Voraussetzungen

So erstellen und registrieren Sie benötigte Datasets:

* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie noch heute die [kostenlose oder kostenpflichtige Version von Azure Machine Learning Service](https://aka.ms/AMLFree) aus.

* Ein Azure Machine Learning-Dienstbereich. Siehe [Erstellen eines Azure Machine Learning Service-Arbeitsbereichs](https://docs.microsoft.com/azure/machine-learning/service/setup-create-workspace).

* Das Azure Machine Learning SDK für Python. Wie Sie die neueste Version des SDK installieren oder auf diese aktualisieren, erfahren Sie unter [Installieren des Azure Machine Learning SDK für Python](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).

## <a name="create-datasets-from-local-files"></a>Erstellen von Datasets aus lokalen Dateien

Um Dateien von Ihrem lokalen Computer zu laden, geben Sie den Pfad von Datei oder Ordner mit der [`auto_read_files()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset(class)?view=azure-ml-py#auto-read-files-path--include-path-false-)-Methode aus der `Dataset`-Klasse an.  Diese Methode führt die folgenden Schritte aus, ohne dass Sie den Dateityp angeben oder Argumente analysieren müssen:

* Ableiten und Festlegen von Trennzeichen.
* Überspringen leerer Datensätze am Anfang der Datei.
* Ableiten und Festlegen der Kopfzeile.
* Ableiten und Konvertieren von Spaltendatentypen.

```Python
from azureml.core.dataset import Dataset

dataset = Dataset.auto_read_files('./data/crime.csv')
```

Verwenden Sie alternativ die dateispezifischen Funktionen, um explizit die Analyse der Datei zu steuern. Das Datasets-SDK unterstützt derzeit [durch Trennzeichen getrennte](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-delimited-files-path--separator------header--promoteheadersbehavior-allfileshavesameheaders--3---encoding--fileencoding-utf8--0---quoting-false--infer-column-types-true--skip-rows-0--skip-mode--skiplinesbehavior-norows--0---comment-none--include-path-false--archive-options-none-), [Excel](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-excel-files-path--sheet-name-none--use-column-headers-false--skip-rows-0--include-path-false--infer-column-types-true-)-, [Parquet](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-parquet-files-path--include-path-false-)-, [binäre](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-binary-files-path-) und [JSON](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-json-files-path--encoding--fileencoding-utf8--0---flatten-nested-arrays-false--include-path-false-)-Dateiformate.

## <a name="create-datasets-from-azure-datastores"></a>Erstellen von Datasets aus Azure-Datenspeichern

Um Datasets aus einem Azure-Datenspeicher zu erstellen, gehen Sie folgendermaßen vor:

* Stellen Sie sicher, dass Sie als Mitwirkender oder Besitzer über Zugriff auf den registrierten Azure-Datenspeicher verfügen.

* Importieren Sie die [`Workspace`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py)-, [`Datastore`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#definition)- und `Dataset`-Pakete aus dem SDK.

```Python
from azureml.core.workspace import Workspace
from azureml.core.datastore import Datastore
from azureml.core.dataset import Dataset

datastore_name = 'your datastore name'

# get existing workspace
workspace = Workspace.from_config()
```

 Die `get()`-Methode ruft einen vorhandenen Datenspeicher im Arbeitsbereich ab.

```
dstore = Datastore.get(workspace, datastore_name)
```

Verwenden Sie die `from_delimited_files()`-Methode, um in durch Trennzeichen getrennten Dateien zu lesen und ein nicht registriertes Dataset zu erstellen.

```Python
# create an in-memory Dataset on your local machine
datapath = dstore.path('data/src/crime.csv')
dataset = Dataset.from_delimited_files(datapath)

# returns the first 5 rows of the Dataset as a pandas Dataframe.
dataset.head(5)
```

||ID|Fallnummer|Datum|Block|IUCR|Primärer Typ|BESCHREIBUNG|Standortbeschreibung|Verhaftung|Inland|...|Bezirk|Gemeindebezirk|FBI-Code|x-Koordinate|y-Koordinate|Jahr|Aktualisiert am|Breitengrad|Längengrad|Location|
|--|--|---|---|---|---|----|------|-------|------|-----|---|----|----|-----|-----|------|----|-----|----|----|-----
|0|10498554|HZ239907|4/4/2016 23:56|007XX E 111TH ST|1153|DECEPTIVE PRACTICE|FINANCIAL IDENTITY THEFT OVER $ 300|OTHER|FALSE|FALSE|...|9|50|11|1183356|1831503|2016|5/11/2016 15:48|41.69283384|-87.60431945|(41.692833841, -87.60431945)|
1|10516598|HZ258664|4/15/2016 17:00|082XX S MARSHFIELD AVE|890|THEFT| FROM BUILDING|RESIDENCE|FALSE|FALSE|...|21|71|6|1166776|1850053|2016|5/12/2016 15:48|41.74410697|-87.66449429|(41.744106973, -87.664494285)
2|10519196|HZ261252|4/15/2016 10:00|104XX S SACRAMENTO AVE|1154|DECEPTIVE PRACTICE|FINANCIAL IDENTITY THEFT $300 AND UNDER|RESIDENCE|FALSE|FALSE|...|19|74|11|||2016|5/12/2016 15:50
3|10519591|HZ261534|4/15/2016 9:00|113XX S PRAIRIE AVE|1120|DECEPTIVE PRACTICE|FORGERY|RESIDENCE|FALSE|FALSE|...|9|49|10|||2016|5/13/2016 15:51
4|10534446|HZ277630|4/15/2016 10:00|055XX N KEDZIE AVE|890|THEFT|FROM BUILDING|SCHOOL, PUBLIC, BUILDING|FALSE|FALSE|...|40|13|6|||2016|5/25/2016 15:59|

## <a name="register-your-datasets-with-workspace"></a>Registrieren Ihrer Datasets mit dem Arbeitsbereich

Verwenden Sie die [`register()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#register-workspace--name--description-none--tags-none--visible-true--exist-ok-false--update-if-exist-false-)-Methode, um in Ihrem Arbeitsbereich Datasets für die Freigabe und Wiederverwendung in Ihrer Organisation und verschiedene Experimente übergreifend zu registrieren.

```Python
dataset = dataset.register(workspace = workspace,
                           name = 'dataset_crime',
                           description = 'Training data',
                           exist_ok = False
                           )
```

>[!NOTE]
> Die Standardparametereinstellung für `register()` ist `exist_ok = False`. Wenn Sie versuchen, ein Dataset mit dem gleichen Namen zu registrieren, ohne diese Einstellung zu ändern, tritt ein Fehler auf.

Die `register()`-Methode gibt das bereits registrierte Dataset mit der Parametereinstellung `exist_ok = True` zurück.

```Python
dataset = dataset.register(workspace = workspace,
                           name = 'dataset_crime',
                           description = 'Training data',
                           exist_ok = True
                           )
```

Verwenden Sie `list()`, um alle registrierten Datasets in Ihrem Arbeitsbereich anzuzeigen.

```Python
Dataset.list(workspace_name)
```

Aus dem oben stehenden Code resultiert Folgendes:

```Python
[Dataset(Name: dataset_crime,
         Workspace: workspace_name)]
```

## <a name="access-datasets-in-workspace"></a>Zugriff auf Datasets im Arbeitsbereich

Registrierte Datasets sind lokal, remote und auf Computeclustern wie der Azure Machine Learning-Computeressource verfügbar und nutzbar. Um Ihre registrierten Datasets Experimente und Computeumgebungen übergreifend wiederzuverwenden, rufen Sie mit folgendem Code Ihren Arbeitsbereich und das registrierte Dataset mit Namen ab.

```Python
workspace = Workspace.from_config()

dataset = workspace.datasets['dataset_crime']
```

## <a name="next-steps"></a>Nächste Schritte

* [Untersuchen und Aufbereiten von Datasets](how-to-explore-prepare-data.md).
* [Verwalten des Lebenszyklus von Dataset-Definitionen](how-to-manage-dataset-definitions.md).
* Ein Beispiel für die Verwendung von Datasets finden Sie in den [Beispielnotebooks](https://aka.ms/dataset-tutorial).
