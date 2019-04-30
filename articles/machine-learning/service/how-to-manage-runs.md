---
title: Starten, Überwachen und Abbrechen von Trainingsausführungen in Python
titleSuffix: Azure Machine Learning service
description: Erfahren Sie, wie Sie Machine Learning-Experimente starten, deren Status festlegen, sie markieren und sie organisieren.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: roastala
author: rastala
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 04/05/2019
ms.openlocfilehash: c0c1c1353b12944fa913dfb0789192917b99f234
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2019
ms.locfileid: "59543592"
---
# <a name="start-monitor-and-cancel-training-runs-in-python"></a>Starten, Überwachen und Abbrechen von Trainingsausführungen in Python

Das [Azure Machine Learning-SDK für Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) bietet verschiedene Methoden zum Überwachen, Organisieren und Verwalten Ihrer Ausführungen für Training und Experimentieren.

In diesem Artikel finden Sie Beispiele für die folgenden Aufgaben:

* Überwachen der Ausführungsleistung
* Abbrechen oder Fehler von Ausführungen
* Erstellen untergeordneter Ausführungen
* Markieren und Suchen von Ausführungen

## <a name="prerequisites"></a>Voraussetzungen

Sie benötige folgende Elemente:

* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie noch heute die [kostenlose oder kostenpflichtige Version von Azure Machine Learning Service](https://aka.ms/AMLFree) aus.

* Ein [Azure Machine Learning Service-Arbeitsbereich](setup-create-workspace.md).

* Das Azure Machine Learning SDK für Python (Version 1.0.21 oder höher). Wie Sie die neueste Version des SDK installieren oder auf diese aktualisieren, erfahren Sie unter [Installieren des Azure Machine Learning SDK für Python](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).

    Um Ihre Version des Azure Machine Learning SDK zu überprüfen, verwenden Sie den folgenden Code:

    ```Python
    print(azureml.core.VERSION)
    ```

## <a name="start-a-run-and-its-logging-process"></a>Starten einer Ausführung und deren Protokollierungsprozess

Richten Sie Ihr Experiment durch Importieren der Klassen [Workspace](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py), [Experiment](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment?view=azure-ml-py), [Run](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py) und [ScriptRunConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrunconfig?view=azure-ml-py) aus dem [azureml.core](https://docs.microsoft.com/python/api/azureml-core/azureml.core?view=azure-ml-py)-Paket ein.

```Python
import azureml.core
from azureml.core import Workspace, Experiment, Run
from azureml.core import ScriptRunConfig

ws = Workspace.from_config()
exp = Experiment(workspace=ws, name="explore-runs")
```

Starten Sie eine Ausführung und ihren Protokollierungsprozess mit der [`start_logging()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment(class)?view=azure-ml-py#start-logging--args----kwargs-)-Methode.

```Python
notebook_run = exp.start_logging()

notebook_run.log(name="message", value="Hello from run!")
```

## <a name="monitor-the-status-of-a-run"></a>Überwachen des Status einer Ausführung

Rufen Sie den Status einer Ausführung mit der [ `get_status()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#get-status--)-Methode ab.

```Python
print(notebook_run.get_status())
```

Um weitere Details über die Ausführung abzurufen, verwenden Sie die [`get_details()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py#get-details--)-Methode.

```Python
notebook_run.get_details()
```

Wenn die Ausführung erfolgreich abgeschlossen wurde, verwenden Sie die [`complete()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#complete--set-status-true-)-Methode, um sie als abgeschlossen zu markieren.

```Python
notebook_run.complete()
print(notebook_run.get_status())
```

Wenn Sie das Python-Muster `with...as` verwenden, markiert sich die Ausführung automatisch selbst als abgeschlossen, wenn sie außerhalb des Bereichs liegt. Sie müssen die Ausführung nicht manuell als abgeschlossen markieren.

```Python
with exp.start_logging() as notebook_run:
    notebook_run.log(name="message", value="Hello from run!")
    print("Is it still running?",notebook_run.get_status())

print("Has it completed?",notebook_run.get_status())
```

## <a name="cancel-or-fail-runs"></a>Abbrechen oder Fehler von Ausführungen

 Wenn Sie einen Fehler feststellen, oder wenn Ihre Ausführung zu lange bis zum Abschluss benötigt, beenden Sie die Ausführung mit der [`cancel()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#cancel--)-Methode, bevor sie abgeschlossen ist, und markieren Sie sie als abgebrochen.

```Python
run_config = ScriptRunConfig(source_directory='.', script='hello_with_delay.py')

local_script_run = exp.submit(run_config)
print("Did the run start?",local_script_run.get_status())

local_script_run.cancel()
print("Did the run cancel?",local_script_run.get_status())
```

Wenn Ihre Ausführung abgeschlossen ist, aber Fehler enthält (etwa weil ein falsches Trainingsskript verwendet wurde), können Sie sie mit der [`fail()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#fail-error-details-none---set-status-true-)-Methode als fehlerhaft markieren.

```Python
local_script_run = exp.submit(run_config)
local_script_run.fail()

print(local_script_run.get_status())
```

## <a name="create-child-runs"></a>Erstellen untergeordneter Ausführungen

Erstellen Sie untergeordnete Ausführungen, um verwandte Ausführungen zu gruppieren, wie etwa für unterschiedliche Iterationen zum Optimieren von Hyperparametern.

Dieses Codebeispiel verwendet das `hello_with_children.py`-Skript, um einen Batch von fünf untergeordneten Ausführungen aus einer übermittelten Ausführung mithilfe der [`child_run()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#child-run-name-none--run-id-none--outputs-none-)-Methode zu erstellen:

```Python
!more hello_with_children.py
run_config = ScriptRunConfig(source_directory='.', script='hello_with_children.py')

local_script_run = exp.submit(run_config)
local_script_run.wait_for_completion(show_output=True)
print(local_script_run.get_status())

with exp.start_logging() as parent_run:
    for c,count in enumerate(range(5)):
        with parent_run.child_run() as child:
            child.log(name="Hello from child run", value=c)
```

> [!NOTE]
> Sobald sie den gültigen Bereich verlassen haben, werden untergeordnete Ausführungen automatisch als abgeschlossen markiert.

Sie können untergeordnete Ausführungen auch einzeln nacheinander starten, aber da jede Erstellung in einem Netzwerkaufruf resultiert, ist es weniger effizient als das Senden eines Batches von Ausführungen.

Um die untergeordneten Ausführungen eines bestimmten übergeordneten Elements abzufragen, verwenden Sie die [`get_children()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#get-children-recursive-false--tags-none--properties-none--type-none--status-none---rehydrate-runs-true-)-Methode.

```Python
list(parent_run.get_children())
```

## <a name="tag-and-find-runs"></a>Markieren und Suchen von Ausführungen

In Azure Machine Learning Service können Sie Eigenschaften und Tags zum Organisieren und Abfragen wichtiger Informationen Ihrer Ausführungen verwenden.

### <a name="add-properties-and-tags"></a>Hinzufügen von Eigenschaften und Tags

Um Ihren Ausführungen durchsuchbare Metadaten hinzuzufügen, verwenden Sie die [`add_properties()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#add-properties-properties-)-Methode. Der folgende Code fügt der Ausführung z. B. die `"author"`-Eigenschaft hinzu:

```Python
local_script_run.add_properties({"author":"azureml-user"})
print(local_script_run.get_properties())
```

Eigenschaften sind unveränderlich, sodass sie eine permanente Aufzeichnung zu Überwachungszwecken erstellen. Das folgende Codebeispiel führt zu einem Fehler, weil `"azureml-user"` im vorstehenden Code bereits als Wert der `"author"`-Eigenschaft hinzugefügt wurde:

```Python
try:
    local_script_run.add_properties({"author":"different-user"})
except Exception as e:
    print(e)
```

Im Gegensatz zu Eigenschaften können Tags geändert werden. Um durchsuchbare und aussagekräftige Informationen für Consumer Ihres Experiments hinzuzufügen, verwenden Sie die [`tag()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#tag-key--value-none-)-Methode.

```Python
local_script_run.tag("quality", "great run")
print(local_script_run.get_tags())

local_script_run.tag("quality", "fantastic run")
print(local_script_run.get_tags())
```

Sie können auch einfache Zeichenfolgentags hinzufügen. Wenn diese Tags in das Tag-Wörterbuch aufgenommen wurden, haben sie den Wert `None`.

```Python
local_script_run.tag("worth another look")
print(local_script_run.get_tags())
```

### <a name="query-properties-and-tags"></a>Abfragen von Eigenschaften und Tags

Sie können Ausführungen in einem Experiment abfragen, um eine Liste der Ausführungen zurückzugeben, die mit bestimmten Eigenschaften und Tags übereinstimmen.

```Python
list(exp.get_runs(properties={"author":"azureml-user"},tags={"quality":"fantastic run"}))
list(exp.get_runs(properties={"author":"azureml-user"},tags="worth another look"))
```

## <a name="example-notebooks"></a>Beispielnotebooks

Die folgenden Notebooks veranschaulichen die Konzepte in diesem Artikel:

* Weitere Informationen zu den Protokollierungs-APIs finden Sie im [Protokollierungs-API-Notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/logging-api/logging-api.ipynb).

* Weitere Informationen zum Verwalten von Ausführungen mit dem Azure Machine Learning-SDK finden Sie in dem Beitrag zum [Verwalten von Notebooks von Ausführungen](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training/manage-runs).

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Protokollieren von Metriken für Ihre Experimente finden Sie unter [Protokollieren von Metriken während Trainingsausführungen](how-to-track-experiments.md).