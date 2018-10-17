---
title: 'Schnellstart: Verwenden des Python SDK zum Erstellen eines Machine Learning-Dienst-Arbeitsbereichs – Azure Machine Learning'
description: Enthält eine Beschreibung der ersten Schritte mit Azure Machine Learning.  Installieren Sie das Python SDK, und verwenden Sie es zum Erstellen eines Arbeitsbereichs. Dieser Arbeitsbereich bildet die Grundlage in der Cloud für das Experimentieren, Trainieren und Bereitstellen von Modellen für maschinelles Lernen mit dem Azure Machine Learning-Dienst.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: quickstart
ms.reviewer: sgilley
author: hning86
ms.author: haining
ms.date: 09/24/2018
ms.openlocfilehash: ee24c1797d0f52d2529ed583a0cfe90cc9e27035
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "49067759"
---
# <a name="quickstart-use-python-to-get-started-with-azure-machine-learning"></a>Schnellstart: Verwenden von Python für die ersten Schritte mit Azure Machine Learning

In diesem Schnellstart verwenden Sie das Azure Machine Learning SDK für Python, um einen [Arbeitsbereich](concept-azure-machine-learning-architecture.md) für den Machine Learning-Dienst zu erstellen und dann zu verwenden. Dieser Arbeitsbereich bildet die Grundlage für das Experimentieren, Trainieren und Bereitstellen von Modellen für maschinelles Lernen in der Cloud mit dem Azure Machine Learning-Dienst.

In diesem Tutorial installieren Sie das Python SDK und führen Folgendes durch:
* Erstellen eines Arbeitsbereichs in Ihrem Azure-Abonnement
* Erstellen einer Konfigurationsdatei für diesen Arbeitsbereich, der später in anderen Notebooks und Skripts verwendet werden kann
* Schreiben von Code, mit dem Werte im Arbeitsbereich protokolliert werden
* Anzeigen der protokollierten Werte in Ihrem Arbeitsbereich

Der Arbeitsbereich und die zugehörige Konfigurationsdatei, die Sie in dieser Schnellstartanleitung erstellen, können als Voraussetzungen für andere Azure Machine Learning-Tutorials und -Anleitungen genutzt werden. Ebenso wie bei anderen Azure-Diensten gelten für den Azure Machine Learning-Dienst Grenzwerte und Kontingente. [Erfahren Sie mehr zu Kontingenten und ihrer Erhöhung.](how-to-manage-quotas.md)

Der Einfachheit halber werden die folgenden Azure-Ressourcen bei regionaler Verfügbarkeit Ihrem Arbeitsbereich automatisch hinzugefügt: [Containerregistrierung](https://azure.microsoft.com/services/container-registry/), [Storage](https://azure.microsoft.com/services/storage/), [Application Insights](https://azure.microsoft.com/services/application-insights/) und [Key Vault](https://azure.microsoft.com/services/key-vault/).

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.


##  <a name="install-the-sdk"></a>Installieren des SDKs

**Überspringen Sie diesen Abschnitt, wenn** Sie eine DSVM (Data Science Virtual Machine) nutzen, die nach dem 27. September 2018 erstellt wurde, da das Python SDK auf diesen DSVMs vorinstalliert ist.

Bevor Sie das SDK installieren, ist es ratsam, zunächst eine isolierte Python-Umgebung zu erstellen. In dieser Schnellstartanleitung wird zwar [Miniconda](https://conda.io/docs/user-guide/install/index.html) verwendet, aber Sie können auch eine vollständige [Anaconda](https://www.anaconda.com/)-Installation oder [Python virtualenv](https://virtualenv.pypa.io/en/stable/) nutzen.

### <a name="install-miniconda"></a>Installieren von Miniconda


Führen Sie den [Download](https://conda.io/miniconda.html) für Miniconda durch, und installieren Sie die Anwendung. Wählen Sie die Version Python 3.7 oder höher. Wählen Sie nicht Version Python 2.x.

### <a name="create-an-isolated-python-environment"></a>Erstellen einer isolierten Python-Umgebung 

Starten Sie ein Befehlszeilenfenster, und erstellen Sie mit Python 3.6 eine neue Conda-Umgebung mit dem Namen `myenv`.

```sh
conda create -n myenv -y Python=3.6
```

Aktivieren Sie die Umgebung.

  ```sh
  conda activate myenv
  ```

### <a name="install-the-sdk"></a>Installieren des SDKs

Installieren Sie in der aktivierten Conda-Umgebung das SDK. Mit diesem Code werden die Kernkomponenten des Azure Machine Learning SDK und der Jupyter Notebook-Server in der Conda-Umgebung `myenv` installiert.  Diese Installation dauert **ca. 4 Minuten**.

```sh
pip install azureml-sdk[notebooks]
```

## <a name="create-a-workspace"></a>Erstellen eines Arbeitsbereichs

Starten Sie Jupyter Notebook, indem Sie diesen Befehl eingeben.
```sh
jupyter notebook
```

Erstellen Sie im Browserfenster ein neues Notebook, indem Sie den Standardkernel `Python 3` verwenden. 

Zeigen Sie die SDK-Version an, indem Sie den folgenden Python-Code in eine Zelle des Notebooks eingeben und ausführen.

```python
import azureml.core
print(azureml.core.VERSION)
```

Erstellen Sie eine neue Azure-Ressourcengruppe und einen neuen Arbeitsbereich.

Suchen Sie in der [Liste mit den Abonnements im Azure-Portal](https://ms.portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) nach einem Wert für `<azure-subscription-id>`. Verwenden Sie ein beliebiges Abonnement, für das Sie über die Rolle „Besitzer“ oder „Mitwirkender“ verfügen.

```python
from azureml.core import Workspace
ws = Workspace.create(name='myworkspace',
                      subscription_id='<azure-subscription-id>',
                      resource_group='myresourcegroup',
                      create_resource_group=True,
                      location='eastus2' # or other supported Azure region
                     )
```

Beim Ausführen des obigen Codes wird ggf. ein neues Browserfenster ausgelöst, in dem Sie sich an Ihrem Azure-Konto anmelden können. Nachdem Sie sich angemeldet haben, wird das Authentifizierungstoken lokal zwischengespeichert.

Geben Sie Folgendes ein, um die Details des Arbeitsbereichs anzuzeigen, z.B. den zugeordneten Speicher, die Containerregistrierung und den Schlüsselwert:

```python
ws.get_details()
```

## <a name="write-a-configuration-file"></a>Schreiben einer Konfigurationsdatei

Speichern Sie die Details Ihres Arbeitsbereichs in einer Konfigurationsdatei im aktuellen Verzeichnis. Diese Datei hat den Namen „aml_config\config.json“.  

Diese Konfigurationsdatei für den Arbeitsbereich erleichtert das spätere Laden dieses Arbeitsbereichs mit anderen Notebooks und Skripts in demselben Verzeichnis oder einem Unterverzeichnis. 

```python
# Create the configuration file.
ws.write_config()

# Use this code to load the workspace from 
# other scripts and notebooks in this directory.
# ws = Workspace.from_config()
```

Mit dem API-Aufruf `write_config()` wird die Konfigurationsdatei im aktuellen Verzeichnis erstellt. Die Datei `config.json` enthält Folgendes:

```json
{
    "subscription_id": "<azure-subscription-id>",
    "resource_group": "myresourcegroup",
    "workspace_name": "myworkspace"
}
```

## <a name="use-the-workspace"></a>Verwenden des Arbeitsbereichs

Schreiben Sie Code, für den die grundlegenden APIs des SDK genutzt werden, um Experimentausführungen nachzuverfolgen.

```python
from azureml.core import Experiment

# create a new experiemnt
exp = Experiment(workspace=ws, name='myexp')

# start a run
run = exp.start_logging()

# log a number
run.log('my magic number', 42)

# log a list (Fibonacci numbers)
run.log_list('my list', [1, 1, 2, 3, 5, 8, 13, 21, 34, 55]) 

# finish the run
run.complete()
```

## <a name="view-logged-results"></a>Anzeigen der protokollierten Ergebnisse
Nachdem die Ausführung abgeschlossen ist, können Sie die Experimentausführung im Azure-Portal anzeigen. Verwenden Sie den folgenden Code, um eine URL in den Ergebnissen für die letzte Ausführung auszugeben.

```python
print(run.get_portal_url())
```

Verwenden Sie den Link, um die protokollierten Werte im Azure-Portal in Ihrem Browser anzuzeigen.

![Protokollierte Werte im Portal](./media/quickstart-create-workspace-with-python/logged-values.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen 
>[!IMPORTANT]
>Die von Ihnen erstellten Ressourcen können ggf. auch in anderen Azure Machine Learning-Tutorials und -Anleitungen verwendet werden.

Wenn Sie die hier erstellten Komponenten nicht weiter verwenden, sollten Sie die in dieser Schnellstartanleitung erstellten Ressourcen löschen, um das Anfallen von Kosten zu vermeiden.

```python
ws.delete(delete_dependent_resources=True)
```

## <a name="next-steps"></a>Nächste Schritte

Sie haben damit die erforderlichen Ressourcen erstellt, um mit dem Experimentieren und Bereitstellen von Modellen zu beginnen. Außerdem haben Sie Code in einem Notebook ausgeführt und den Ausführungsverlauf dieses Codes in Ihrem Arbeitsbereich in der Cloud untersucht.

Sie benötigen einige weitere Pakete in Ihrer Umgebung, um diese für die Azure Machine Learning-Tutorials zu verwenden:

1. Schließen Sie Ihr Notebook im Browser.
1. Verwenden Sie im Befehlszeilenfenster die Tastenkombination `Ctrl`+`C`, um den Notebookserver zu beenden.
1. Installieren Sie zusätzliche Pakete.

    ```sh
    conda install -y cython matplotlib scikit-learn pandas numpy
    pip install azureml-sdk[automl]
    ```

Arbeiten Sie nach der Installation dieser Pakete die Tutorials zum Trainieren und Bereitstellen eines Modells durch.  

> [!div class="nextstepaction"]
> [Tutorial: Trainieren eines Imageklassifizierungsmodells](tutorial-train-models-with-aml.md)

Sie können auch [anspruchsvollere Beispiele auf GitHub](https://aka.ms/aml-notebooks) erkunden.
