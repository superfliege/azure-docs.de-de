---
title: 'Schnellstart: Verwenden des Python SDK zum Erstellen eines Machine Learning-Dienst-Arbeitsbereichs – Azure Machine Learning'
description: Enthält eine Beschreibung der ersten Schritte mit Azure Machine Learning. Installieren Sie das Python SDK, und verwenden Sie es zum Erstellen eines Arbeitsbereichs. Dieser Arbeitsbereich bildet die Grundlage in der Cloud zum Experimentieren, Trainieren und Bereitstellen von Machine Learning-Modellen mit Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: quickstart
ms.reviewer: sgilley
author: hning86
ms.author: haining
ms.date: 09/24/2018
ms.openlocfilehash: cc348ca50b942b6b8b1474ed4dac4067d107a4af
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49377998"
---
# <a name="quickstart-use-python-to-get-started-with-azure-machine-learning"></a>Schnellstart: Verwenden von Python für die ersten Schritte mit Azure Machine Learning

In dieser Schnellstartanleitung verwenden Sie das Azure Machine Learning SDK für Python, um einen [Arbeitsbereich](concept-azure-machine-learning-architecture.md) für den Machine Learning-Dienst zu erstellen und zu verwenden. Dieser Arbeitsbereich bildet die Grundlage in der Cloud zum Experimentieren, Trainieren und Bereitstellen von Machine Learning-Modellen mit Machine Learning.

In diesem Tutorial installieren Sie das Python SDK und führen Folgendes durch:

* Erstellen eines Arbeitsbereichs in Ihrem Azure-Abonnement
* Erstellen einer Konfigurationsdatei für diesen Arbeitsbereich zur späteren Verwendung in anderen Notebooks und Skripts
* Schreiben von Code, der Werte im Arbeitsbereich protokolliert
* Anzeigen der protokollierten Werte in Ihrem Arbeitsbereich

Anhand dieser Schnellstartanleitung erstellen Sie einen Arbeitsbereich und eine Konfigurationsdatei. Diese können in anderen Machine Learning-Tutorials und -Anleitungen verwendet werden. Genau wie bei anderen Azure-Diensten gelten auch für Machine Learning bestimmte Grenzwerte und Kontingente. [Erfahren Sie mehr zu Kontingenten und ihrer Erhöhung.](how-to-manage-quotas.md)

Folgende Azure-Ressourcen werden Ihrem Arbeitsbereich automatisch hinzugefügt, sofern sie regional verfügbar sind:
 
- [Azure Container Registry](https://azure.microsoft.com/services/container-registry/)
- [Azure Storage (in englischer Sprache)](https://azure.microsoft.com/services/storage/)
- [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) 
- [Azure Key Vault](https://azure.microsoft.com/services/key-vault/)

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.


## <a name="install-the-sdk"></a>Installieren des SDKs

*Überspringen Sie diesen Abschnitt, wenn Sie einen virtuellen Data Science-Computer verwenden, der nach dem 27. September 2018 erstellt wurde.* Auf diesen virtuellen Data Science-Computern ist das Python SDK bereits vorinstalliert.

Es empfiehlt sich, vor der Installation des SDK eine isolierte Python-Umgebung zu erstellen. In dieser Schnellstartanleitung wird zwar [Miniconda](https://conda.io/docs/user-guide/install/index.html) verwendet, Sie aber können auch eine vollständige [Anaconda](https://www.anaconda.com/)-Installation oder [Python virtualenv](https://virtualenv.pypa.io/en/stable/) nutzen.

### <a name="install-miniconda"></a>Installieren von Miniconda


Führen Sie den [Download](https://conda.io/miniconda.html) für Miniconda durch, und installieren Sie die Anwendung. Wählen Sie mindestens die Python-Version 3.7 aus. Verwenden Sie nicht die Python-Version 2.x.

### <a name="create-an-isolated-python-environment"></a>Erstellen einer isolierten Python-Umgebung 

Öffnen Sie ein Befehlszeilenfenster. Erstellen Sie mit Python 3.6 eine neue Conda-Umgebung mit dem Namen `myenv`.

```sh
conda create -n myenv -y Python=3.6
```

Aktivieren Sie die Umgebung.

  ```sh
  conda activate myenv
  ```

### <a name="install-the-sdk"></a>Installieren des SDKs

Installieren Sie in der aktivierten Conda-Umgebung das SDK. Dieser Code installiert die Hauptkomponenten des Machine Learning SDK. Darüber hinaus installiert er einen Jupyter Notebook-Server in der Conda-Umgebung `myenv`. Die Installation dauert **ca. vier Minuten**.

```sh
pip install azureml-sdk[notebooks]
```

## <a name="create-a-workspace"></a>Erstellen eines Arbeitsbereichs

Geben Sie den folgenden Befehl ein, um das Jupyter Notebook zu starten:
```sh
jupyter notebook
```

Erstellen Sie im Browserfenster ein neues Notebook unter Verwendung des Standardkernels `Python 3`. 

Geben Sie zum Anzeigen der SDK-Version den folgenden Python-Code in eine Zelle des Notebooks ein, und führen Sie ihn aus:

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

Beim Ausführen des obigen Codes wird ggf. ein neues Browserfenster für die Anmeldung bei Ihrem Azure-Konto geöffnet. Nachdem Sie sich angemeldet haben, wird das Authentifizierungstoken lokal zwischengespeichert.

Geben Sie den folgenden Code ein, um Details des Arbeitsbereichs wie etwa den zugeordneten Speicher, die Containerregistrierung und den Schlüsseltresor anzuzeigen:

```python
ws.get_details()
```

## <a name="write-a-configuration-file"></a>Schreiben einer Konfigurationsdatei

Speichern Sie die Details Ihres Arbeitsbereichs in einer Konfigurationsdatei im aktuellen Verzeichnis. Diese Datei hat den Namen „aml_config\config.json“.  

Diese Konfigurationsdatei für den Arbeitsbereich erleichtert das spätere Laden dieses Arbeitsbereichs. Sie können ihn mit anderen Notebooks und Skripts im gleichen Verzeichnis oder in einem Unterverzeichnis laden. 

```python
# Create the configuration file.
ws.write_config()

# Use this code to load the workspace from 
# other scripts and notebooks in this directory.
# ws = Workspace.from_config()
```

Mit dem API-Aufruf `write_config()` wird die Konfigurationsdatei im aktuellen Verzeichnis erstellt. Die Datei `config.json` enthält das folgende Skript:

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

# create a new experiment
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
Nach Abschluss der Ausführung können Sie die Experimentausführung im Azure-Portal anzeigen. Verwenden Sie den folgenden Code, um eine URL in den Ergebnissen für die letzte Ausführung auszugeben.

```python
print(run.get_portal_url())
```

Verwenden Sie den Link, um die protokollierten Werte im Azure-Portal in Ihrem Browser anzuzeigen.

![Protokollierte Werte im Portal](./media/quickstart-create-workspace-with-python/logged-values.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen 
>[!IMPORTANT]
>Die von Ihnen erstellten Ressourcen können auch in anderen Machine Learning-Tutorials und -Anleitungen verwendet werden.

Wenn Sie die hier erstellten Ressourcen nicht mehr benötigen, löschen Sie sie, damit Ihnen keine Kosten entstehen.

```python
ws.delete(delete_dependent_resources=True)
```

## <a name="next-steps"></a>Nächste Schritte

Sie haben die Ressourcen erstellt, die Sie zum Experimentieren und zum Bereitstellen von Modellen benötigen. Sie haben Code in einem Notebook ausgeführt. Und Sie haben den Ausführungsverlauf dieses Codes in Ihrem Arbeitsbereich in der Cloud untersucht.

Sie benötigen noch einige weitere Pakete in Ihrer Umgebung, um sie in Machine Learning-Tutorials verwenden zu können.

1. Schließen Sie Ihr Notebook im Browser.
1. Verwenden Sie im Befehlszeilenfenster die Tastenkombination `Ctrl`+`C`, um den Notebookserver zu beenden.
1. Installieren Sie zusätzliche Pakete.

    ```sh
    conda install -y cython matplotlib scikit-learn pandas numpy
    pip install azureml-sdk[automl]
    ```

Absolvieren Sie nach der Installation dieser Pakete die Tutorials zum Trainieren und Bereitstellen eines Modells. 

> [!div class="nextstepaction"]
> [Tutorial: Trainieren eines Imageklassifizierungsmodells](tutorial-train-models-with-aml.md)

Sie können sich auch [anspruchsvollere Beispiele auf GitHub](https://aka.ms/aml-notebooks) ansehen.
