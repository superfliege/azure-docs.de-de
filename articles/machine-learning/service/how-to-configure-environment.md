---
title: Einrichten der Python-Entwicklungsumgebung
titleSuffix: Azure Machine Learning service
description: Erfahren Sie, wie Sie eine Entwicklungsumgebung für die Arbeit mit dem Azure Machine Learning Service konfigurieren. In diesem Dokument erfahren Sie, wie Sie Conda-Umgebungen verwenden, Konfigurationsdateien erstellen und Jupyter-Notebooks, Azure-Notebooks, IDEs, Code-Editoren und die Data Science Virtual Machine konfigurieren.
services: machine-learning
author: rastala
ms.author: roastala
ms.service: machine-learning
ms.component: core
ms.reviewer: larryfr
manager: cgronlun
ms.topic: conceptual
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 59f847dc38cddfd9185cfd169cf1ef34c744f8f7
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2018
ms.locfileid: "53192633"
---
# <a name="configure-a-development-environment-for-azure-machine-learning"></a>Konfigurieren einer Entwicklungsumgebung für Azure Machine Learning

In diesem Dokument erfahren Sie, wie Sie eine Entwicklungsumgebung für die Arbeit mit dem Azure Machine Learning-Dienst konfigurieren. Der Azure Machine Learning-Dienst ist frameworkunabhängig. Die einzigen Voraussetzungen für die Entwicklungsumgebung sind __Python 3__, __Conda__ (für isolierte Umgebungen) und eine Konfigurationsdatei, die die Informationen zu Ihrem Azure Machine Learning-Arbeitsbereich enthält.

In diesem Dokument werden in erster Linie die folgenden spezifischen Umgebungen und Tools behandelt:

* [Azure Notebooks](#aznotebooks): Ein in der Azure-Cloud gehosteter Jupyter Notebooks-Dienst. Dies ist __die einfachste__ Methode für die ersten Schritte, da das Azure Machine Learning-SDK bereits installiert ist.
* [Data Science Virtual Machine](#dsvm): Eine __vorkonfigurierte Entwicklungs- bzw. Experimentierumgebung__ in der Azure-Cloud, die __für Data Science-Zwecke konzipiert ist__ und entweder auf ausschließlich CPU-basierten VM-Instanzen oder GPU-basierten Instanzen bereitgestellt werden kann. Python 3, Conda, Jupyter Notebooks und das Azure Machine Learning-SDK sind bereits installiert. Der virtuelle Computer ist mit verbreiteten ML- bzw. Deep Learning-Frameworks, Tools und Editoren zum Entwickeln von ML-Lösungen ausgestattet. Dies ist wahrscheinlich __die vollständigste__ Entwicklungsumgebung für Machine Learning auf der Azure-Plattform.
* [Jupyter Notebooks](#jupyter): Wenn Sie Jupyter Notebooks bereits verwenden, enthält das SDK einige zusätzliche Elemente, die Sie installieren sollten.
* [Visual Studio Code](#vscode): Wenn Sie Visual Studio Code verwenden, gibt es einige nützliche Erweiterungen, die Sie installieren können.
* [Azure Databricks](#aml-databricks): Eine beliebte Datenanalyseplattform, die auf Apache Spark basiert. Erfahren Sie, wie Sie die Azure Machine Learning SDK in Ihren Cluster einbinden, um Modelle bereitzustellen.

Wenn Sie bereits über eine Python 3-Umgebung verfügen oder nur die grundlegenden Schritte zum Installieren des SDK erhalten möchten, finden Sie weitere Informationen im Abschnitt [Lokaler Computer](#local).

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure Machine Learning-Dienstbereich. Führen Sie die Schritte unter [Erste Schritte mit dem Azure Machine Learning-Dienst](quickstart-get-started.md) aus, um einen zu erstellen.

- Einen der Paket-Manager [Continuum Anaconda](https://www.anaconda.com/download/) oder [Miniconda](https://conda.io/miniconda.html).

    > [!IMPORTANT]
    > Anaconda und Miniconda sind bei Verwendung von Azure Notebooks nicht erforderlich.

- Unter Linux oder macOS benötigen Sie die Bash-Shell.

    > [!TIP]
    > Wenn Sie unter Linux oder macOS eine andere Shell als Bash verwenden (z.B. zsh), werden beim Ausführen einiger Befehle möglicherweise Fehler ausgegeben. Um dieses Problem zu umgehen, starten Sie mit dem Befehl `bash` eine neue Bash-Shell starten, und führen Sie die Befehle in dieser aus.

- Unter Windows benötigen Sie die Eingabeaufforderung oder die Anaconda-Eingabeaufforderung (installiert mit Anaconda und Miniconda).

## <a id="anotebooks"></a>Azure Notebooks

[Azure Notebooks](https://notebooks.azure.com) (Vorschauversion) ist eine interaktive Umgebung in der Azure-Cloud. Es handelt sich um die __einfachste__ Methode für die ersten Schritte bei der Azure Machine Learning-Entwicklung.

* Das Azure Machine Learning-SDK ist __bereits installiert__.
* Nachdem Sie im Azure-Portal einen Arbeitsbereich für den Azure Machine Learning-Dienst erstellt haben, können Sie über eine Schaltfläche Ihre Azure Notebook-Umgebung für die Arbeit mit dem Arbeitsbereich automatisch konfigurieren.

Informationen zum Einstieg in die Entwicklung mit Azure Notebooks finden Sie in den Anweisungen im Dokument [Erste Schritte mit dem Azure Machine Learning-Dienst](quickstart-get-started.md).

## <a id="dsvm"></a>Data Science Virtual Machine

Die Data Science Virtual Machine (DSVM) ist ein benutzerdefiniertes VM-Image, das für **Data Science-Zwecke konzipiert** wurde. Es ist mit Folgendem vorkonfiguriert:

  - Pakete wie Tensorflow, Pytorch, scikit-learn, Xgboost und Azure ML SDK
  - Beliebte Data Science-Tools wie Spark (eigenständig), Drill
  - Azure-Tools wie die CLI, Azcopy und Storage-Explorer
  - Integrierte Entwicklungsumgebungen (IDEs), z. B. Visual Studio Code, PyCharm und RStudio
  - Jupyter Notebook-Server 

Das Azure Machine Learning-SDK funktioniert mit der Ubuntu- und der Windows-Version der DSVM. Um die Data Science Virtual Machine-Instanz als Entwicklungsumgebung zu verwenden, gehen Sie wie folgt vor:

1. Verwenden Sie zum Erstellen einer Data Science Virtual Machine-Instanz die folgenden Methoden:

    * Bei Verwendung des Azure-Portals:

        * [Erstellen einer __Ubuntu__-Data Science Virtual Machine](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro)

        * [Erstellen einer __Windows__-Data Science Virtual Machine](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/provision-vm)

    * Bei Verwendung der Azure-Befehlszeilenschnittstelle:

        > [!IMPORTANT]
        > Melden Sie sich bei Verwendung der Azure-Befehlszeilenschnittstelle zunächst bei Ihrem Azure-Abonnement über den Befehl `az login` an.
        >
        > Wenn Sie die Befehle in diesem Schritt ausführen, müssen Sie einen Ressourcengruppennamen, einen Namen für die VM, einen Benutzernamen und ein Kennwort angeben.

        * Verwenden Sie zum Erstellen einer __Ubuntu__-Data Science Virtual Machine-Instanz den folgenden Befehl:

            ```azurecli
            # create a Ubuntu DSVM in your resource group
            # note you need to be at least a contributor to the resource group in order to execute this command successfully
            # If you need to create a new resource group use: "az group create --name YOUR-RESOURCE-GROUP-NAME --location YOUR-REGION (For example: westus2)"
            az vm create --resource-group YOUR-RESOURCE-GROUP-NAME --name YOUR-VM-NAME --image microsoft-dsvm:linux-data-science-vm-ubuntu:linuxdsvmubuntu:latest --admin-username YOUR-USERNAME --admin-password YOUR-PASSWORD --generate-ssh-keys --authentication-type password
            ```

        * Verwenden Sie zum Erstellen einer __Windows__-Data Science Virtual Machine-Instanz den folgenden Befehl:

            ```azurecli
            # create a Windows Server 2016 DSVM in your resource group
            # note you need to be at least a contributor to the resource group in order to execute this command successfully
            az vm create --resource-group YOUR-RESOURCE-GROUP-NAME --name YOUR-VM-NAME --image microsoft-dsvm:dsvm-windows:server-2016:latest --admin-username YOUR-USERNAME --admin-password YOUR-PASSWORD --authentication-type password
            ```    

2. Das Azure Machine Learning-SDK ist auf der DSVM **bereits installiert**. Um die Conda-Umgebung mit dem SDK zu verwenden, führen Sie einen der folgenden Befehle aus:

    * Verwenden Sie auf der DSVM unter __Ubuntu__ diesen Befehl:

        ```shell
        conda activate py36
        ```

    * Verwenden Sie auf der DSVM unter __Windows__ diesen Befehl:

        ```shell
        conda activate AzureML
        ```

1. Um sicherzustellen, dass Sie auf das SDK zugreifen und die Version überprüfen können, verwenden Sie den folgenden Python-Code:

    ```python
    import azureml.core
    print(azureml.core.VERSION)
    ```

1. Informationen zum Konfigurieren der DSVM für Ihren Azure Machine Learning-Arbeitsbereich finden Sie im Abschnitt [Konfigurieren des Arbeitsbereichs](#workspace).

Weitere Informationen zu Data Science Virtual Machines finden Sie unter [Data Science Virtual Machines](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/).

## <a id="local"></a>Lokaler Computer

Wenn Sie einen lokalen Computer verwenden (dabei kann es sich auch um einen virtuellen Remotecomputer handeln), gehen Sie folgendermaßen vor, um eine Conda-Umgebung zu erstellen und das SDK zu installieren:

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

1. Um zu überprüfen, ob das SDK installiert wurde, verwenden Sie folgenden Python-Code:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

### <a id="jupyter"></a>Jupyter Notebooks

Jupyter Notebooks sind Teil des [Jupyter-Projekts](https://jupyter.org/). Sie stellen eine interaktive Programmierumgebung bereit, mit der Sie Dokumente erstellen können, in denen Livecode mit beschreibendem Text und Grafiken gemischt ist. Jupyter Notebooks sind auch eine hervorragende Möglichkeit, Ihre Ergebnisse mit anderen zu teilen, da Sie die Ausgabe Ihrer Codeabschnitte im Dokument speichern können. Sie können Jupyter Notebooks auf vielen unterschiedlichen Plattformen installieren.

Über die Schritte im Abschnitt [Lokaler Computer](#local) werden optionale Komponenten für Jupyter Notebooks installiert. Um diese Komponenten in Ihrer Jupyter Notebook-Umgebung zu aktivieren, führen Sie die folgenden Schritte aus:

1. Öffnen Sie eine Eingabeaufforderung oder Shell.

1. Um einen Conda-fähigen Jupyter Notebook-Server zu installieren, führen Sie den folgenden Befehl aus:

    ```shell
    # install Jupyter
    conda install nb_conda
    ```

1. Öffnen Sie Jupyter Notebook mit dem folgenden Befehl:

    ```shell
    jupyter notebook
    ```

1. Um zu überprüfen, ob Jupyter Notebook das SDK verwenden kann, öffnen Sie ein neues Notebook, und wählen Sie als Kernel „myenv“ aus. Führen Sie dann den folgenden Befehl in einer Notebookzelle aus:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

1. Informationen zum Konfigurieren des Jupyter Notebook für Ihren Azure Machine Learning-Arbeitsbereich finden Sie im Abschnitt [Konfigurieren des Arbeitsbereichs](#workspace).

### <a id="vscode"></a>Visual Studio Code

Visual Studio Code ist ein Code-Editor für alle Plattformen. Für Python-Unterstützung benötigt er eine lokale Python 3- und Conda-Installation, aber er bietet zusätzliche Tools für die Arbeit mit künstlicher Intelligenz. Darüber hinaus bietet er Unterstützung für die Auswahl der Conda-Umgebung aus dem Code-Editor heraus.

Um Visual Studio Code für die Entwicklung zu verwenden, führen Sie die folgenden Schritte aus:

1. Informationen zur Verwendung von Visual Studio Code für die Python-Entwicklung finden Sie im Dokument [Erste Schritte mit Python in VS Code](https://code.visualstudio.com/docs/python/python-tutorial).

1. Um die Conda-Umgebung auszuwählen, öffnen Sie Visual Studio Code, und rufen Sie die __Befehlspalette__ mit __STRG+UMSCHALT+P__ (Linux und Windows) bzw. __BEFEHL+UMSCHALT+P__ (Mac) auf. Geben Sie __Python: Select Interpreter__ ein, und wählen Sie dann die Conda-Umgebung aus.

1. Um sicherzustellen dass Sie das SDK verwenden können, erstellen Sie eine neue Python-Datei (.py) mit dem folgenden Code. Führen Sie die Datei dann aus:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

1. Installationsanweisungen für die Azure Machine Learning-Erweiterung für Visual Studio Code finden Sie auf der Seite [Tools for AI](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-ai).

    Weitere Informationen finden Sie unter [Verwenden von Azure Machine Learning für Visual Studio Code](how-to-vscode-tools.md).

<a name="aml-databricks"></a>

## <a name="azure-databricks"></a>Azure Databricks

Sie können eine angepasste Version des Azure Machine Learning SDK für Azure Databricks für ein umfassendes, benutzerdefiniertes maschinelles Lernen verwenden. Alternativ können Sie Ihr Modell in Databricks trainieren und [Visual Studio Code](how-to-vscode-train-deploy.md#deploy-your-service-from-vs-code) verwenden, um das Modell bereitzustellen.

So bereiten Sie Ihren Databricks-Cluster vor und erhalten Beispiele für Notebooks

1. Erstellen Sie mit **Python 3** einen [Databricks-Cluster](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal) mit einer Databricks-Runtimeversion von 4.x (bevorzugt mit hoher Parallelität). 

1. Erstellen Sie eine Bibliothek, um das Azure Machine Learning SDK für das Python `azureml-sdk[databricks]` PyPi-Paket zu [installieren und an Ihren Cluster anzufügen](https://docs.databricks.com/user-guide/libraries.html#create-a-library). Wenn Sie fertig sind, sehen Sie die angefügte Bibliothek, wie in diesem Bild gezeigt. Beachten Sie diese [häufigen Probleme mit Databricks](resource-known-issues.md#databricks).

   ![Auf Databricks installiertes SDK ](./media/how-to-azure-machine-learning-on-databricks/sdk-installed-on-databricks.jpg)

   Wenn dieser Schritt fehlschlägt, starten Sie Ihren Cluster neu:
   1. Wählen Sie im linken Bereich die Option `Clusters` aus. Wählen Sie den Clusternamen in der Tabelle aus. 
   1. Wählen Sie auf der Registerkarte `Libraries` die Option `Restart` aus.

1. Laden Sie die [Notebookarchivdatei für Azure Databricks / Azure Machine Learning SDK](https://github.com/Azure/MachineLearningNotebooks/blob/master/databricks/Databricks_AMLSDK_github.dbc) herunter.

   >[!Warning]
   > Für die Verwendung mit Azure Machine Learning Service stehen zahlreiche Beispiel-Notebooks zur Verfügung. Mit Azure Databricks funktionieren nur diese Beispiel-Notebooks: https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/azure-databricks

1.  [Importieren Sie diese Archivdatei](https://docs.azuredatabricks.net/user-guide/notebooks/notebook-manage.html#import-an-archive) in Ihren Databricks-Cluster, und sehen Sie sich diese wie [hier beschrieben](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/azure-databricks) genauer an.


## <a id="workspace"></a>Erstellen einer Konfigurationsdatei für den Arbeitsbereich

Die Arbeitsbereichs-Konfigurationsdatei ist ein JSON-Dokument, das das SDK zur Kommunikation mit Ihrem Azure Machine Learning-Dienstarbeitsbereich instruiert. Die Datei hat den Namen `config.json` und das folgende Format:

```json
{
    "subscription_id": "<subscription-id>",
    "resource_group": "<resource-group>",
    "workspace_name": "<workspace-name>"
}
```

Diese Datei muss sich in der Verzeichnisstruktur befinden, die Ihre Python-Skripts oder Jupyter Notebooks enthält. Es kann sich im selben Verzeichnis, in einem Unterverzeichnis mit dem Namen `aml_config` oder in einem übergeordneten Verzeichnis befinden.

Um in Ihrem Code auf diese Datei zuzugreifen, verwenden Sie `ws=Workspace.from_config()`. Dieser Code lädt die Informationen aus der Datei und stellt eine Verbindung mit Ihrem Arbeitsbereich her.

Es gibt drei Möglichkeiten zum Erstellen der Konfigurationsdatei:

* Wenn Sie nach dem [Azure Machine Learning-Schnellstart ](quickstart-get-started.md) vorgehen, wird die Datei `config.json` in Ihrer Azure Notebooks-Bibliothek erstellt. Diese Datei enthält die Konfigurationsinformationen für Ihren Arbeitsbereich. Sie können diese Datei `config.json` in andere Entwicklungsumgebungen herunterladen oder kopieren.

* Sie können **die Datei mit einem Text-Editor manuell erstellen**. Sie finden die Werte dafür in der Konfigurationsdatei in Ihrem Arbeitsbereich im [Azure-Portal](https://portal.azure.com). Kopieren Sie die Werte für __Arbeitsbereichsname__, __Ressourcengruppe__ und __Abonnement-ID__, und verwenden Sie sie in der Konfigurationsdatei.
        ![Azure-Portal](./media/how-to-configure-environment/configure.png)

* Sie können **die Datei programmgesteuert erstellen**. Der folgende Codeausschnitt veranschaulicht, wie Sie mit der Abonnement-ID, der Ressourcengruppe und dem Arbeitsbereichsnamen eine Verbindung mit einem Arbeitsbereich herstellen. Anschließend wird die Arbeitsbereichskonfiguration in dieser Datei gespeichert:

    ```python
    from azureml.core import Workspace

    subscription_id = '<subscription-id>'
    resource_group  = '<resource-group>'
    workspace_name  = '<workspace-name>'

    try:
        ws = Workspace(subscription_id = subscription_id, resource_group = resource_group, workspace_name = workspace_name)
        ws.write_config()
        print('Library configuration succeeded')
    except:
        print('Workspace not found')
    ```

    Dieser Code schreibt die Konfigurationsdatei in die Datei `aml_config/config.json`.

## <a name="next-steps"></a>Nächste Schritte

- [Trainieren eines Modells in Azure Machine Learning mit dem MNIST-Dataset](tutorial-train-models-with-aml.md)
- [Azure Machine Learning SDK for Python](https://aka.ms/aml-sdk)
- [Azure Machine Learning – Datenaufbereitungs-SDK](https://aka.ms/data-prep-sdk)
