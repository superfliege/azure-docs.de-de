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
ms.date: 12/04/2018
ms.openlocfilehash: f16d39238b46a811f34fdfe291d6c09bee662b9a
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52870066"
---
# <a name="quickstart-use-python-to-get-started-with-azure-machine-learning"></a>Schnellstart: Verwenden von Python für die ersten Schritte mit Azure Machine Learning

In dieser Schnellstartanleitung verwenden Sie das Azure Machine Learning SDK für Python, um einen [Arbeitsbereich](concept-azure-machine-learning-architecture.md) für den Machine Learning-Dienst zu erstellen und zu verwenden. Dieser Arbeitsbereich bildet die Grundlage in der Cloud zum Experimentieren, Trainieren und Bereitstellen von Machine Learning-Modellen mit Machine Learning. In dieser Schnellstartanleitung beginnen Sie mit dem Konfigurieren Ihrer eigenen Python-Umgebung und eines eigenen Jupyter Notebook-Servers. Für die Ausführung ohne Installation lesen Sie die Informationen unter [Schnellstart: Verwenden des Azure-Portals zum Ausführen der ersten Schritte für Azure Machine Learning](quickstart-get-started.md).

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2G9N6]

In diesem Tutorial installieren Sie das Python SDK und führen Folgendes durch:

* Erstellen eines Arbeitsbereichs in Ihrem Azure-Abonnement.
* Erstellen einer Konfigurationsdatei für diesen Arbeitsbereich zur späteren Verwendung in anderen Notebooks und Skripts
* Schreiben von Code, der Werte im Arbeitsbereich protokolliert
* Anzeigen der protokollierten Werte in Ihrem Arbeitsbereich

Anhand dieser Schnellstartanleitung erstellen Sie einen Arbeitsbereich und eine Konfigurationsdatei. Diese können in anderen Machine Learning-Tutorials und -Anleitungen verwendet werden. Genau wie bei anderen Azure-Diensten gelten auch für Machine Learning bestimmte Grenzwerte und Kontingente. [Erfahren Sie mehr zu Kontingenten und ihrer Erhöhung.](how-to-manage-quotas.md)

Folgende Azure-Ressourcen werden Ihrem Arbeitsbereich automatisch hinzugefügt, sofern sie regional verfügbar sind:
 
- [Azure Container Registry](https://azure.microsoft.com/services/container-registry/)
- [Azure Storage (in englischer Sprache)](https://azure.microsoft.com/services/storage/)
- [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) 
- [Azure Key Vault](https://azure.microsoft.com/services/key-vault/)

>[!NOTE]
> Der Code in diesem Artikel wurde mit Version 1.0.2 des Azure Machine Learning SDK getestet. 


Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://aka.ms/AMLfree) erstellen, bevor Sie beginnen.


## <a name="install-the-sdk"></a>Installieren des SDKs

*Überspringen Sie diesen Abschnitt, wenn Sie einen virtuellen Data Science-Computer verwenden, der nach dem 27. September 2018 erstellt wurde.* Auf diesen virtuellen Data Science-Computern ist das Python SDK bereits vorinstalliert.

Es empfiehlt sich, vor der Installation des SDK eine isolierte Python-Umgebung zu erstellen. In dieser Schnellstartanleitung wird zwar [Miniconda](https://conda.io/docs/user-guide/install/index.html) verwendet, Sie aber können auch eine vollständige [Anaconda](https://www.anaconda.com/)-Installation oder [Python virtualenv](https://virtualenv.pypa.io/en/stable/) nutzen.

### <a name="install-miniconda"></a>Installieren von Miniconda


Führen Sie den [Download](https://conda.io/miniconda.html) für Miniconda durch, und installieren Sie die Anwendung. Wählen Sie mindestens die Python-Version 3.7 aus. Verwenden Sie nicht die Python-Version 2.x.

### <a name="create-an-isolated-python-environment"></a>Erstellen einer isolierten Python-Umgebung 

Öffnen Sie ein Befehlszeilenfenster. Erstellen Sie mit Python 3.6 eine neue Conda-Umgebung mit dem Namen `myenv`.

```shell
conda create -n myenv -y Python=3.6
```

Aktivieren Sie die Umgebung.

  ```shell
  conda activate myenv
  ```

### <a name="install-the-sdk"></a>Installieren des SDKs

Installieren Sie in der aktivierten Conda-Umgebung das SDK. Dieser Code installiert die Hauptkomponenten des Machine Learning SDK. Darüber hinaus installiert er einen Jupyter Notebook-Server in der Conda-Umgebung. Der Installationsvorgang dauert abhängig von der Konfiguration Ihres Computers einige Minuten.

```sh
# install Jupyter
conda install nb_conda

# install the base SDK and Jupyter Notebook
pip install azureml-sdk[notebooks]

```

Sie können auch andere zusätzliche Schlüsselwörter zur Installation weiterer Komponenten des SDK verwenden.

```sh
# install the base SDK and auto ml components
pip install azureml-sdk[automl]

# install the base SDK and model explainability component
pip install azureml-sdk[explain]

# install the base SDK and experimental components
pip install azureml-sdk[contrib]
```

Verwenden Sie diese Installation stattdessen in einer Databricks-Umgebung.

```
# install the base SDK and automl components in Azure Databricks environment
# read more at: https://github.com/Azure/MachineLearningNotebooks/tree/master/databricks
pip install azureml-sdk[databricks]
```


## <a name="create-a-workspace"></a>Erstellen eines Arbeitsbereichs

Geben Sie den folgenden Befehl ein, um das Jupyter Notebook zu starten:
```shell
jupyter notebook
```

Erstellen Sie im Browserfenster ein neues Notebook unter Verwendung des Standardkernels `Python 3`. 

Geben Sie zum Anzeigen der SDK-Version den folgenden Python-Code in eine Zelle des Notebooks ein, und führen Sie ihn aus:

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=import)]

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

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=getDetails)]


## <a name="write-a-configuration-file"></a>Schreiben einer Konfigurationsdatei

Speichern Sie die Details Ihres Arbeitsbereichs in einer Konfigurationsdatei im aktuellen Verzeichnis. Diese Datei hat den Namen „aml_config\config.json“.  

Diese Konfigurationsdatei für den Arbeitsbereich erleichtert das spätere Laden dieses Arbeitsbereichs. Sie können ihn mit anderen Notebooks und Skripts im gleichen Verzeichnis oder in einem Unterverzeichnis laden. 

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=writeConfig)]


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

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=useWs)]


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

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=delete)]


## <a name="next-steps"></a>Nächste Schritte

Sie haben die Ressourcen erstellt, die Sie zum Experimentieren und zum Bereitstellen von Modellen benötigen. Sie haben Code in einem Notebook ausgeführt. Und Sie haben den Ausführungsverlauf dieses Codes in Ihrem Arbeitsbereich in der Cloud untersucht.

Sie benötigen noch einige weitere Pakete in Ihrer Umgebung, um sie in Machine Learning-Tutorials verwenden zu können.

1. Schließen Sie Ihr Notebook im Browser.
1. Verwenden Sie im Befehlszeilenfenster die Tastenkombination `Ctrl`+`C`, um den Notebookserver zu beenden.
1. Installieren Sie zusätzliche Pakete.

    ```shell
    conda install -y cython matplotlib scikit-learn pandas numpy
    pip install azureml-sdk[automl]
    ```


Absolvieren Sie nach der Installation dieser Pakete die Tutorials zum Trainieren und Bereitstellen eines Modells. 

> [!div class="nextstepaction"]
> [Tutorial: Trainieren eines Imageklassifizierungsmodells](tutorial-train-models-with-aml.md)

Sie können sich auch [anspruchsvollere Beispiele auf GitHub](https://aka.ms/aml-notebooks) ansehen.
