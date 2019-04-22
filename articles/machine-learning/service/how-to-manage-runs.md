---
title: Starten, Überwachen und Abbrechen von Trainingsausführungen in Python
titleSuffix: Azure Machine Learning service
description: Erfahren Sie, wie Sie Machine Learning-Experimente starten, ihren Status festlegen, sie markieren und organisieren.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: roastala
author: rastala
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 04/05/2019
ms.openlocfilehash: 82df2258116ce55fa440b67ec0a66b106d0d72c7
ms.sourcegitcommit: 6e32f493eb32f93f71d425497752e84763070fad
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/10/2019
ms.locfileid: "59471493"
---
# <a name="start-monitor-and-cancel-training-runs-in-python"></a>Starten, Überwachen und Abbrechen von Trainingsausführungen in Python

Das [Azure Machine Learning-SDK für Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) bietet verschiedene Methoden zum Überwachen, Organisieren und Verwalten Ihrer Ausführungen für Training und Experimentieren.

In dieser Vorgehensweise finden Sie Beispiele für die folgenden Aufgaben:

* [Überwachen der Ausführungsleistung](#monitor)
* [Abbrechen oder Fehler von Ausführungen](#cancel)
* [Erstellen untergeordneter Ausführungen](#children)
* [Markieren und Suchen von Ausführungen](#tag)

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen Folgendes:

* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie noch heute die [kostenlose oder kostenpflichtige Version von Azure Machine Learning Service](https://aka.ms/AMLFree) aus.

* Ein Azure Machine Learning-Dienstbereich. Siehe [Erstellen eines Azure Machine Learning Service-Arbeitsbereichs](setup-create-workspace.md).

* Installation des Azure Machine Learning SDK für Python (Version 1.0.21 oder höher). Wie Sie die neueste Version des SDK installieren oder darauf aktualisieren, erfahren Sie auf der Seite [Installieren des Azure Machine Learning SDK für Python](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).

    Um Ihre Version des Azure Machine Learning SDK zu überprüfen, verwenden Sie den folgenden Code.

    ```Python
    print(azureml.core.VERSION)
    ```

<a name="monitor"></a>

## <a name="start-a-run-and-set-its-status"></a>Starten einer Ausführung und Festlegen ihres Status

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

Rufen Sie mithilfe von [`get_status()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#get-status--) den Status der Ausführung ab.

```Python
print(notebook_run.get_status())
```

Um weitere Details zur Ausführung zu erfahren, verwenden Sie [`get_details()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py#get-details--).

```Python
notebook_run.get_details()
```

Wenn die Ausführung erfolgreich abgeschlossen wurde, verwenden Sie die [`complete()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#complete--set-status-true-)-Methode, um sie als abgeschlossen zu markieren.

```Python
notebook_run.complete()
print(notebook_run.get_status())
```

Sie können auch das `with...as`-Muster von Python verwenden. In diesem Kontext markiert die Ausführung sich automatisch selbst als abgeschlossen, wenn die Ausführung außerhalb des Bereichs liegt. Auf diese Weise müssen Sie die Ausführung nicht manuell als abgeschlossen markieren.

```Python
with exp.start_logging() as notebook_run:
    notebook_run.log(name="message", value="Hello from run!")
    print("Is it still running?",notebook_run.get_status())

print("Has it completed?",notebook_run.get_status())
```

<a name="cancel"></a>

## <a name="cancel-or-fail-runs"></a>Abbrechen oder Fehler von Ausführungen

 Wenn Sie einen Fehler feststellen, oder es lange zu dauern scheint, bis die Ausführung abgeschlossen ist, beenden Sie die Ausführung mit der [`cancel()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#cancel--)-Methode, bevor sie abgeschlossen ist, und markieren Sie sie als abgebrochen.

```Python
run_config = ScriptRunConfig(source_directory='.', script='hello_with_delay.py')

local_script_run = exp.submit(run_config)
print("Did the run start?",local_script_run.get_status())

local_script_run.cancel()
print("Did the run cancel?",local_script_run.get_status())
```

Wenn Ihre Ausführung abgeschlossen ist, aber Fehler enthält, als ob das falsche Trainingsskript verwendet worden sei, können Sie sie mit der [`fail()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#fail-error-details-none---set-status-true-)-Methode als fehlerhaft markieren.

```Python
local_script_run = exp.submit(run_config)
local_script_run.fail()

print(local_script_run.get_status())
```

<a name="children"></a>

## <a name="create-child-runs"></a>Erstellen untergeordneter Ausführungen

Erstellen Sie untergeordnete Ausführungen, um verwandte Ausführungen zu gruppieren, wie für andere Iterationen zum Optimieren von Hyperparametern.

Dieses Codebeispiel verwendet das hello_with_children.py-Skript zum Erstellen eines Batches von fünf untergeordneten Ausführungen in einer übermittelten Ausführung mithilfe der [`child_run()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#child-run-name-none--run-id-none--outputs-none-)-Methode.

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
> Untergeordnete Ausführungen werden automatisch abgeschlossen, da sie sich aus dem gültigen Bereich heraus bewegen.

Sie können untergeordnete Ausführungen auch einzeln nacheinander starten, aber da jede Erstellung in einem Netzwerkaufruf resultiert, ist es weniger effizient als das Senden eines Batches von Ausführungen.

Verwenden Sie die [`get_children()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#get-children-recursive-false--tags-none--properties-none--type-none--status-none---rehydrate-runs-true-)-Methode, um die untergeordneten Ausführungen eines bestimmten übergeordneten Elements abzufragen.

```Python
list(parent_run.get_children())
```

<a name="tag"></a>

## <a name="tag-and-find-runs"></a>Markieren und Suchen von Ausführungen

In Azure Machine Learning Service können Sie Eigenschaften und Tags zum Organisieren und Abfragen wichtiger Informationen Ihrer Ausführungen verwenden.

### <a name="add-properties-and-tags"></a>Hinzufügen von Eigenschaften und Tags

Fügen Sie mit [`add_properties()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#add-properties-properties-) Ihren Ausführungen durchsuchbare Metadaten hinzu. Der folgende Code fügt z.B. die „author“-Eigenschaft der Ausführung hinzu.

```Python
local_script_run.add_properties({"author":"azureml-user"})
print(local_script_run.get_properties())
```

Eigenschaften sind unveränderlich, was als eine permanente Aufzeichnung zu Überwachungszwecken hilfreich ist. Das folgende Codebeispiel führt zu einem Fehler, da wir „azureml-user“ bereits als „author“-Eigenschaft im vorangehenden Code hinzugefügt haben.

```Python
try:
    local_script_run.add_properties({"author":"different-user"})
except Exception as e:
    print(e)
```

Tags können jedoch geändert werden. Fügen Sie mit [`tag()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#tag-key--value-none-) durchsuchbare und aussagekräftige Informationen für Consumer Ihres Experiments hinzu.

```Python
local_script_run.tag("quality", "great run")
print(local_script_run.get_tags())

local_script_run.tag("quality", "fantastic run")
print(local_script_run.get_tags())
```

Sie können auch ein einfaches Zeichenfolgentag hinzufügen. Es wird im Tagwörterbuch mit dem Wert `None` angezeigt.

```Python
local_script_run.tag("worth another look")
print(local_script_run.get_tags())
```

### <a name="query-properties-and-tags"></a>Abfragen von Eigenschaften und Tags

Sie können Abfragen nach bestimmten Eigenschaften und Tags in einem Experiment ausführen.

```Python
list(exp.get_runs(properties={"author":"azureml-user"},tags={"quality":"fantastic run"}))
list(exp.get_runs(properties={"author":"azureml-user"},tags="worth another look"))
```

## <a name="example-notebooks"></a>Beispielnotebooks

Die folgenden Notebooks verdeutlichen die Konzepte in diesem Artikel:

* Weitere Informationen zu Protokollierungs-APIs finden Sie im [Protokollierungs-API-Notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/logging-api/logging-api.ipynb).

* Weitere Informationen zum Verwalten von Ausführungen mit dem Azure Machine Learning-SDK finden Sie in dem Beitrag zum [Verwalten von Notebooks von Ausführungen](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training/manage-runs).

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Protokollieren von Metriken für Ihre Experimente finden Sie im Artikel [Protokollieren von Metriken während Trainingsausführungen in Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/service/how-to-track-experiment).