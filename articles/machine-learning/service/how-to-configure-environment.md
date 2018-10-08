---
title: Konfigurieren einer Entwicklungsumgebung für Azure Machine Learning | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Entwicklungsumgebung für die Arbeit mit dem Azure Machine Learning Service konfigurieren. In diesem Dokument erfahren Sie, wie Sie Conda-Umgebungen verwenden, Konfigurationsdateien erstellen und Jupyter-Notebooks, Azure-Notebooks, IDEs, Code-Editoren und die Data Science Virtual Machine konfigurieren.
services: machine-learning
author: rastala
ms.author: roastala
ms.service: machine-learning
ms.reviewer: larryfr
manager: cgronlun
ms.topic: conceptual
ms.date: 8/6/2018
ms.openlocfilehash: 675dae022376fc62292f3b079bd735939b9199c2
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47220294"
---
# <a name="configure-a-development-environment-for-the-azure-machine-learning-service"></a>Konfigurieren einer Entwicklungsumgebung für den Azure Machine Learning Service

Erfahren Sie, wie Sie Ihre Entwicklungsumgebung für die Arbeit mit dem Azure Machine Learning Service konfigurieren. Sie lernen, wie eine Konfigurationsdatei erstellt wird, die Ihre Umgebung einem Azure Machine Learning-Arbeitsbereich zuordnet. Sie erfahren darüber hinaus, wie die folgenden Entwicklungsumgebungen konfiguriert werden:

* Jupyter-Notebooks auf Ihrem eigenen Computer
* Visual Studio Code
* Code-Editor Ihrer Wahl

Das empfohlene Vorgehen besteht in der Verwendung von [virtuellen Conda-Umgebungen](https://conda.io/docs/user-guide/tasks/manage-environments.html) aus Continuum Anaconda, um Ihre Arbeitsumgebung zu isolieren, damit Abhängigkeitskonflikte zwischen Paketen vermieden werden. In diesem Artikel werden die Schritte zur Einrichtung einer Conda-Umgebung und ihre Verwendung für Azure Machine Learning dargestellt.


## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure Machine Learning-Dienstbereich. Führen Sie zum Erstellen eines Dienstbereichs die unter [Erste Schritte mit Azure Machine Learning-Diensten](quickstart-get-started.md) beschriebenen Schritte durch.

* Paket-Manger [Continuum Anaconda](https://www.anaconda.com/download/) oder [Miniconda](https://conda.io/miniconda.html).

 * [Python-Erweiterung](https://code.visualstudio.com/docs/python/python-tutorial) für Visual Studio Code.

## <a name="create-workspace-configuration-file"></a>Erstellen der Konfigurationsdatei für den Arbeitsbereich

Die Arbeitsbereichs-Konfigurationsdatei wird vom SDK zum Kommunizieren mit Ihrem Azure Machine Learning Service-Arbeitsbereich verwendet.  Sie können auf zwei Arten an diese Datei gelangen:

* Schließen Sie den [Schnellstart](quickstart-get-started.md) ab, um einen Arbeitsbereich und eine Konfigurationsdatei zu erstellen. Die Datei `config.json` wird für Sie in Azure-Notebooks erstellt.  Diese Datei enthält die Konfigurationsinformationen für Ihren Arbeitsbereich.  Laden Sie sie in das Verzeichnis herunter, das die Skripts oder Notebooks enthält, die auf sie verweisen, oder kopieren Sie sie dorthin.


* Erstellen Sie die Konfigurationsdatei selbst mit den folgenden Schritten:

    1. Öffnen Sie Ihren Arbeitsbereich im [Azure-Portal](https://portal.azure.com). Kopieren Sie den __Arbeitsbereichsnamen__, die __Ressourcengruppe__ und die __Abonnement-ID__. Diese Werte werden verwendet, um die Konfigurationsdatei zu erstellen.

        ![Azure-Portal](./media/how-to-configure-environment/configure.png) 
    
    1. Erstellen Sie die Datei mit diesem Python-Code. Führen Sie den Code in dem Verzeichnis aus, das die Skripts oder Notebooks enthält, die auf den Arbeitsbereich verweisen:
        ```
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
        Dadurch wird die folgende `aml_config/config.json`-Datei geschrieben: 
    
        ```json
        {
        "subscription_id": "<subscription-id>",
        "resource_group": "<resource-group>",
        "workspace_name": "<workspace-name>"
        }
        ```
        Sie können das `aml_config`-Verzeichnis oder nur die `config.json`-Datei in ein beliebiges anderes Verzeichnis kopieren, das auf den Arbeitsbereich verweist.

>[!NOTE] 
>Andere Skripts oder Notebooks im gleichen Verzeichnis laden den Arbeitsbereich mithilfe von `ws=Workspace.from_config()`

## <a name="azure-notebooks-and-data-science-virtual-machine"></a>Azure Notebooks und Data Science Virtual Machine

Azure Notebooks und Azure Data Science Virtual Machines (DSVM) sind vorkonfiguriert, um mit dem Azure Machine Learning Service zu funktionieren. Erforderliche Komponenten, wie etwa das Azure Machine Learning SDK, sind in diesen Umgebungen vorinstalliert.

Azure Notebooks ist ein Jupyter Notebook-Dienst in der Azure-Cloud. Die Data Science Virtual Machine ist ein VM-Image, das für Data Science-Arbeiten vorkonfiguriert ist. Die VM enthält beliebte Tools, IDEs und Pakete wie Jupyter Notebooks, PyCharm und Tensorflow.

Für die Verwendung dieser Umgebungen benötigen Sie trotzdem eine Arbeitsbereichs-Konfigurationsdatei.

Weitere Informationen zu Data Science Virtual Machines finden Sie in der Dokumentation zu [Data Science Virtual Machines](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/).

Ein Beispiel für die Verwendung von Azure Notebooks zusammen mit dem Azure Machine Learning Service finden Sie im Dokument [Erste Schritte mit Azure Machine Learning Service](quickstart-get-started.md).

## <a name="configure-jupyter-notebooks-on-your-own-computer"></a>Konfigurieren von Jupyter-Notebooks auf Ihrem eigenen Computer

1. Öffnen Sie eine Eingabeaufforderung oder Shell.

2. Um eine Conda-Umgebung zu erstellen, verwenden Sie die folgenden Befehle:

    ```shell
    # create a new conda environment with Python 3.6, numpy and cython
    conda create -n myenv Python=3.6 cython numpy

    # activate the conda environment
    conda activate myenv

    # If you are running Mac OS you should run
    source activate myenv
    ```

    Das Erstellen der Umgebung kann mehrere Minuten in Anspruch nehmen, da Komponenten wie Python 3.6 und andere möglicherweise erst heruntergeladen werden müssen.

3. Um das Azure Machine Learning SDK mit den Notebook-Zugaben zu installieren, verwenden Sie den folgenden Befehl:

     ```shell
    pip install --upgrade azureml-sdk[notebooks,automl]
    ```

    Die Installation des SDKs kann mehrere Minuten in Anspruch nehmen.

4. Um Pakete für Ihre Machine Learning-Experimente zu installieren, verwenden Sie den folgenden Befehl, und ersetzen Sie darin `<new package>` durch den Namen des Pakets, das Sie installieren möchten:

    ```shell
    conda install <new package>
    ```

5. Um einen Conda-fähigen Jupyter Notebook-Server zu installieren und Experiment-Widgets (für die Anzeige von Ausführungsinformationen) zu aktivieren, verwenden Sie die folgenden Befehle:

    ```shell
    # install Jupyter 
    conda install nb_conda

    # install experiment widget
    jupyter nbextension install --py --user azureml.train.widgets

    # enable experiment widget
    jupyter nbextension enable --py --user azureml.train.widgets
    ```

6. Verwenden Sie den folgenden Befehl, um das Jupyter Notebook zu starten:

    ```shell
    jupyter notebook
    ```

7. Öffnen Sie ein neues Notebook, und wählen Sie als Kernel "myenv" aus. Überprüfen Sie dann, ob Sie das Azure Machine Learning SDK installiert haben, indem Sie in einer Notebookzelle den folgenden Befehl ausführen:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

## <a name="configure-visual-studio-code"></a>Konfigurieren von Visual Studio Code

1. Öffnen Sie eine Eingabeaufforderung oder Shell.

2. Um eine Conda-Umgebung zu erstellen, verwenden Sie die folgenden Befehle:

    ```shell
    # create a new conda environment with Python 3.6, numpy and cython
    conda create -n myenv Python=3.6 cython numpy

    # activate the conda environment
    conda activate myenv

    # If you are running Mac OS you should run
    source activate myenv
    ```

2. Um das Azure Machine Learning SDK zu installieren, verwenden Sie den folgenden Befehl:
 
    ```shell
    pip install --upgrade azureml-sdk[automl]
    ```

4. Informationen zur Installation der Visual Studio Code Tools for AI finden Sie im Marketplace-Eintrag von Visual Studio für [Tools for AI](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-ai). 

5. Um Pakete für Ihre Machine Learning-Experimente zu installieren, verwenden Sie den folgenden Befehl, und ersetzen Sie darin `<new package>` durch den Namen des Pakets, das Sie installieren möchten:

    ```shell
    conda install <new package>
    ```

6. Starten Sie Visual Studio Code, und verwenden Sie dann __STRG-UMSCHALT-P__, um die __Befehlspalette__ zu öffnen. Geben Sie *Python: Select Interpreter* ein, und wählen Sie die erstellte Conda-Umgebung aus.

    > [!NOTE]
    > Visual Studio Code erkennt Conda-Umgebungen auf Ihrem Computer automatisch. Weitere Informationen finden Sie in der [Visual Studio Code-Dokumentation](https://code.visualstudio.com/docs/python/environments#_conda-environments).

7. Um die Konfiguration zu überprüfen, verwenden Sie Visual Studio Code, um eine neue Python-Skriptdatei mit folgendem Code zu erstellen, und führen Sie sie dann aus:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

## <a name="configure-code-editor-of-your-choice"></a>Konfigurieren eines Code-Editors Ihrer Wahl

Um einen benutzerdefinierten Code-Editor mit dem Azure Machine Learning SDK zu verwenden, erstellen Sie zunächst eine Conda-Umgebung, wie oben beschrieben. Befolgen Sie anschließend die Anweisungen für jeden Editor, um die Conda-Umgebung zu verwenden. Beispielsweise befinden sich die Anweisungen für PyCharm hier: [https://www.jetbrains.com/help/pycharm/2018.2/conda-support-creating-conda-virtual-environment.html](https://www.jetbrains.com/help/pycharm/2018.2/conda-support-creating-conda-virtual-environment.html).
 
## <a name="next-steps"></a>Nächste Schritte

* [Trainieren eines Modells in Azure Machine Learning mit dem MNIST-Dataset](tutorial-train-models-with-aml.md)
