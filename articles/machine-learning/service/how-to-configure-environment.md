---
title: Konfigurieren einer Entwicklungsumgebung für Azure Machine Learning | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Entwicklungsumgebung für die Arbeit mit dem Azure Machine Learning Service konfigurieren. In diesem Dokument erfahren Sie, wie Sie Conda-Umgebungen verwenden, Konfigurationsdateien erstellen und Jupyter Notebooks, Azure Notebooks, IDEs, Code-Editoren und die Data Science Virtual Machine konfigurieren.
services: machine-learning
author: rastala
ms.author: roastala
ms.service: machine-learning
ms.component: core
ms.reviewer: larryfr
manager: cgronlun
ms.topic: conceptual
ms.date: 8/6/2018
ms.openlocfilehash: 657a762874f7c2fb40553552ef6c17d9b5b6da0f
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2018
ms.locfileid: "49958617"
---
# <a name="configure-a-development-environment-for-azure-machine-learning"></a>Konfigurieren einer Entwicklungsumgebung für Azure Machine Learning

In diesem Artikel wird beschrieben, wie Sie eine Entwicklungsumgebung für die Arbeit mit dem Azure Machine Learning-Dienst konfigurieren, z.B.:

- Erstellen einer Konfigurationsdatei, die Ihre Umgebung einem Azure Machine Learning-Dienstarbeitsbereich zuordnet.
- Konfigurieren der folgenden Entwicklungsumgebungen:
  - Jupyter Notebooks auf Ihrem Computer
  - Visual Studio Code
  - Benutzerdefinierter Code-Editor
- Einrichten einer [virtuellen Conda-Umgebung](https://conda.io/docs/user-guide/tasks/manage-environments.html) und Verwenden der Umgebung für Azure Machine Learning. Wir empfehlen Ihnen, Continuum Anaconda zu nutzen, um Ihre Arbeitsumgebung zu isolieren und so Abhängigkeitskonflikte zwischen Paketen zu vermeiden.

## <a name="prerequisites"></a>Voraussetzungen

- Richten Sie einen Azure Machine Learning-Dienstarbeitsbereich ein. Führen Sie die Schritte unter [Schnellstart: Erste Schritte mit dem Azure Machine Learning-Dienst](quickstart-get-started.md) aus.
- Installieren Sie als Paket-Manager entweder [Continuum Anaconda](https://www.anaconda.com/download/) oder [Miniconda](https://conda.io/miniconda.html).
- Nutzen Sie bei Verwendung von Visual Studio Code die [Python-Erweiterung](https://code.visualstudio.com/docs/python/python-tutorial).

> [!NOTE]
> Sie können die in diesem Artikel veranschaulichten Shellbefehle testen, indem Sie Bash (unter Linux und Mac OS) oder die Eingabeaufforderung (unter Windows) nutzen.

## <a name="create-a-workspace-configuration-file"></a>Erstellen einer Konfigurationsdatei für den Arbeitsbereich

Die Konfigurationsdatei für den Arbeitsbereich wird vom Azure Machine Learning SDK zum Kommunizieren mit Ihrem Azure Machine Learning-Dienstarbeitsbereich verwendet.

- Arbeiten Sie die [Schnellstartanleitung für Azure Machine Learning](quickstart-get-started.md) durch, um die Konfigurationsdatei zu erstellen.
  - Mit dem Schnellstartprozess wird die Datei `config.json` in Azure Notebooks erstellt. Diese Datei enthält die Konfigurationsinformationen für Ihren Arbeitsbereich.
  - Laden Sie die Datei `config.json` in das Verzeichnis mit den Skripts oder Notebooks herunter, die darauf verweisen, oder kopieren Sie sie dorthin.

- Alternativ können Sie die Datei auch manuell erstellen, indem Sie die folgenden Schritte ausführen:

    1. Öffnen Sie Ihren Arbeitsbereich im [Azure-Portal](https://portal.azure.com). Kopieren Sie den __Arbeitsbereichsnamen__, die __Ressourcengruppe__ und die __Abonnement-ID__. Diese Werte werden verwendet, um die Konfigurationsdatei zu erstellen.
        ![Azure-Portal](./media/how-to-configure-environment/configure.png)

    1. Erstellen Sie die Datei mit dem folgenden Python-Code, und stellen Sie sicher, dass Sie den Code in demselben Verzeichnis wie die Skripts oder Notebooks ausführen, in denen auf den Arbeitsbereich verwiesen wird:

        ```python
        from azureml.core import Workspace

        subscription_id ='<subscription-id>'
        resource_group ='<resource-group>'
        workspace_name = '<workspace-name>'

        try:
           ws = Workspace(subscription_id = subscription_id, resource_group = resource_group, workspace_name = workspace_name)
           ws.write_config()
           print('Library configuration succeeded')
        except:
           print('Workspace not found')
        ```
        Mit diesem Code wird die folgende Datei `aml_config/config.json` geschrieben:

        ```json
        {
        "subscription_id": "<subscription-id>",
        "resource_group": "<resource-group>",
        "workspace_name": "<workspace-name>"
        }
        ```
        Sie können das `aml_config`-Verzeichnis oder nur die `config.json`-Datei in ein beliebiges anderes Verzeichnis kopieren, das auf den Arbeitsbereich verweist.

       > [!NOTE]
       > Andere Skripts oder Notebooks in demselben Verzeichnis laden den Arbeitsbereich mithilfe von `ws=Workspace.from_config()`.

## <a name="azure-notebooks-and-data-science-virtual-machines"></a>Azure Notebooks und Data Science Virtual Machines

Azure Notebooks und Azure Data Science Virtual Machines (DSVMs) sind für den Azure Machine Learning-Dienst vorkonfiguriert. Diese Umgebungen enthalten erforderliche Komponenten, z.B. das Azure Machine Learning SDK.

- Azure Notebooks ist ein Jupyter Notebook-Dienst in der Azure-Cloud.
- Die Data Science Virtual Machine ist ein benutzerdefiniertes VM-Image, das für Data Science-Zwecke konzipiert wurde. Sie enthält Folgendes:
  - Beliebte Tools
  - Integrierte Entwicklungsumgebungen (IDEs)
  - Pakete, z.B. Jupyter Notebooks, PyCharm und Tensorflow
- Für die Verwendung dieser Umgebungen benötigen Sie trotzdem eine Konfigurationsdatei für den Arbeitsbereich.

Ein Beispiel für die Verwendung von Azure Notebooks zusammen mit dem Azure Machine Learning-Dienst finden Sie unter [Schnellstart: Erste Schritte mit dem Azure Machine Learning-Dienst](quickstart-get-started.md).

Weitere Informationen zu Data Science Virtual Machines finden Sie unter [Data Science Virtual Machines](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/).

## <a name="configure-jupyter-notebooks-on-your-computer"></a>Konfigurieren von Jupyter Notebooks auf Ihrem Computer

1. Öffnen Sie eine Eingabeaufforderung oder Shell.

1. Verwenden Sie die folgenden Befehle, um eine Conda-Umgebung zu erstellen:

    ```shell
    # create a new conda environment with Python 3.6, numpy, and cython
    conda create -n myenv Python=3.6 cython numpy

    # activate the conda environment
    conda activate myenv

    # On Mac OS run
    source activate myenv
    ```

    Das Erstellen der Umgebung kann mehrere Minuten in Anspruch nehmen, falls Python 3.6 und andere Komponenten heruntergeladen werden müssen.

1. Installieren Sie das Azure Machine Learning SDK mit Notebook-Zugaben und das SDK für die Datenaufbereitung, indem Sie den folgenden Befehl verwenden:

     ```shell
    pip install --upgrade azureml-sdk[notebooks,automl] azureml-dataprep
    ```

    > [!NOTE]
    > Wenn Sie eine Meldung mit dem Hinweis erhalten, dass `PyYAML` nicht deinstalliert werden kann, können Sie stattdessen den folgenden Befehl verwenden:
    >
    > `pip install --upgrade azureml-sdk[notebooks,automl] azureml-dataprep --ignore-installed PyYAML`

    Die Installation des SDK kann mehrere Minuten dauern.

1. Installieren Sie Pakete für Ihre Machine Learning-Experimente. Verwenden Sie den folgenden Befehl, und ersetzen Sie `<new package>` durch das zu installierende Paket:

    ```shell
    conda install <new package>
    ```

1. Installieren Sie einen Conda-fähigen Jupyter Notebook-Server, und aktivieren Sie Experiment-Widgets (für die Anzeige von Ausführungsinformationen). Verwenden Sie die folgenden Befehle:

    ```shell
    # install Jupyter
    conda install nb_conda

    # install experiment widget
    jupyter nbextension install --py --user azureml.train.widgets

    # enable experiment widget
    jupyter nbextension enable --py --user azureml.train.widgets
    ```

1. Öffnen Sie Jupyter Notebook mit dem folgenden Befehl:

    ```shell
    jupyter notebook
    ```

1. Öffnen Sie ein neues Notebook, wählen Sie „myenv“ als Kernel aus, und vergewissern Sie sich dann, dass das Azure Machine Learning SDK installiert ist. Führen Sie den folgenden Befehl in einer Notebookzelle aus:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

## <a name="configure-visual-studio-code"></a>Konfigurieren von Visual Studio Code

1. Öffnen Sie eine Eingabeaufforderung oder Shell.

1. Verwenden Sie die folgenden Befehle, um eine Conda-Umgebung zu erstellen:

    ```shell
    # create a new conda environment with Python 3.6, numpy and cython
    conda create -n myenv Python=3.6 cython numpy

    # activate the conda environment
    conda activate myenv

    # If you are running Mac OS you should run
    source activate myenv
    ```

1. Installieren Sie das Azure Machine Learning SDK und das SDK für die Datenaufbereitung mit dem folgenden Befehl:

    ```shell
    pip install --upgrade azureml-sdk[automl] azureml-dataprep
    ```

1. Installieren Sie die Erweiterung „Visual Studio Code Tools for AI“. Weitere Informationen finden Sie unter [Tools for AI](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-ai).

1. Installieren Sie Pakete für Ihre Machine Learning-Experimente. Verwenden Sie den folgenden Befehl, und ersetzen Sie `<new package>` durch das zu installierende Paket:

    ```shell
    conda install <new package>
    ```

1. Öffnen Sie Visual Studio Code, und verwenden Sie dann **STRG+UMSCHALT+P** (unter Windows) bzw. **BEFEHLSTASTE+UMSCHALT+P** (unter Mac OS), um die **Befehlspalette** anzuzeigen. Geben Sie _Python: Select Interpreter_ ein, und wählen Sie die erstellte Conda-Umgebung aus.

   > [!NOTE]
   > Visual Studio Code erkennt Conda-Umgebungen auf Ihrem Computer automatisch. Weitere Informationen finden Sie in der [Visual Studio Code-Dokumentation](https://code.visualstudio.com/docs/python/environments#_conda-environments).

1. Überprüfen Sie die Konfiguration, indem Sie mit Visual Studio Code eine neue Python-Skriptdatei mit dem folgenden Code erstellen und diese dann ausführen:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

## <a name="configure-a-custom-code-editor"></a>Konfigurieren eines benutzerdefinierten Code-Editors

Sie können einen Code-Editor Ihrer Wahl für das Azure Machine Learning SDK verwenden.

1. Erstellen Sie Ihre Conda-Umgebung, wie in Schritt 2 oben unter [Konfigurieren von Visual Studio Code](#configure-visual-studio-code) beschrieben.
1. Befolgen Sie jeweils die Anweisungen für den Editor, um die Conda-Umgebung zu verwenden. Beispielsweise können Sie die [PyCharm-Anleitung](https://www.jetbrains.com/help/pycharm/2018.2/conda-support-creating-conda-virtual-environment.html) befolgen.

## <a name="next-steps"></a>Nächste Schritte

- [Trainieren eines Modells in Azure Machine Learning mit dem MNIST-Dataset](tutorial-train-models-with-aml.md)
