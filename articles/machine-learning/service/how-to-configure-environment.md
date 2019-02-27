---
title: Einrichten einer Python-Entwicklungsumgebung
titleSuffix: Azure Machine Learning service
description: Erfahren Sie, wie Sie eine Entwicklungsumgebung für die Arbeit mit dem Azure Machine Learning Service konfigurieren. In diesem Artikel erfahren Sie, wie Sie Conda-Umgebungen verwenden, Konfigurationsdateien erstellen und Jupyter Notebooks, Azure Notebooks, Azure Databricks, IDEs, Code-Editoren und die Data Science Virtual Machine konfigurieren.
services: machine-learning
author: rastala
ms.author: roastala
ms.service: machine-learning
ms.subservice: core
ms.reviewer: larryfr
manager: cgronlun
ms.topic: conceptual
ms.date: 01/18/2019
ms.custom: seodec18
ms.openlocfilehash: 136a83c586b2f797269beff3cdd0afb9973cb7c8
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/18/2019
ms.locfileid: "56340517"
---
# <a name="configure-a-development-environment-for-azure-machine-learning"></a>Konfigurieren einer Entwicklungsumgebung für Azure Machine Learning

In diesem Artikel erfahren Sie, wie Sie eine Entwicklungsumgebung für die Arbeit mit dem Azure Machine Learning Service konfigurieren. Der Machine Learning Service ist plattformunabhängig. 

Die einzigen Voraussetzungen für die Entwicklungsumgebung sind Python 3, Conda (für isolierte Umgebungen) und eine Konfigurationsdatei, die Informationen zu Ihrem Azure Machine Learning-Arbeitsbereich enthält.

In diesem Artikel werden in erster Linie die folgenden Umgebungen und Tools behandelt:

* Azure Notebooks: Ein in der Azure-Cloud gehosteter Jupyter Notebooks-Dienst. Dies ist die einfachste Methode für den Einstieg, da das Azure Machine Learning SDK bereits installiert ist.

* [Data Science Virtual Machine (DSVM)](#dsvm): Eine vorkonfigurierte Entwicklungs- bzw. Experimentierumgebung in der Azure-Cloud, die für Data Science-Zwecke konzipiert ist und entweder auf ausschließlich CPU-basierten VM-Instanzen oder GPU-basierten Instanzen bereitgestellt werden kann. Python 3, Conda, Jupyter Notebooks und das Azure Machine Learning-SDK sind bereits installiert. Der virtuelle Computer ist mit gängigen Machine Learning- bzw. Deep Learning-Frameworks, -Tools und -Editoren zum Entwickeln von Lösungen für maschinelles Lernen ausgestattet. Dies ist wahrscheinlich die umfassendste Entwicklungsumgebung für maschinelles Lernen auf der Azure-Plattform.

* [Jupyter Notebook](#jupyter): Wenn Sie bereits ein Jupyter Notebook verwenden, enthält das SDK einige zusätzliche Elemente, die Sie installieren sollten.

* [Visual Studio Code](#vscode): Wenn Sie Visual Studio Code verwenden, bietet es einige nützliche Erweiterungen, die Sie installieren können.

* [Azure Databricks](#aml-databricks): Eine beliebte Datenanalyseplattform, die auf Apache Spark basiert. Erfahren Sie, wie Sie das Azure Machine Learning SDK in Ihren Cluster einbinden, um Modelle bereitzustellen.

Wenn Sie bereits über eine Python 3-Umgebung verfügen oder nur die grundlegenden Schritte zum Installieren des SDK erhalten möchten, finden Sie weitere Informationen im Abschnitt [Lokaler Computer](#local).

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure Machine Learning-Dienstbereich. Informationen zum Erstellen des Arbeitsbereichs finden Sie unter [Erste Schritte mit Azure Machine Learning](quickstart-get-started.md).

- Einen der Paket-Manager [Continuum Anaconda](https://www.anaconda.com/download/) oder [Miniconda](https://conda.io/miniconda.html).

    > [!IMPORTANT]
    > Anaconda und Miniconda sind bei Verwendung von Azure Notebooks nicht erforderlich.

- Unter Linux oder macOS benötigen Sie die Bash-Shell.

    > [!TIP]
    > Wenn Sie unter Linux oder macOS eine andere Shell als Bash verwenden (z.B. zsh), werden beim Ausführen einiger Befehle möglicherweise Fehler ausgegeben. Um dieses Problem zu umgehen, starten Sie mit dem Befehl `bash` eine neue Bash-Shell starten, und führen Sie die Befehle in dieser aus.

- Unter Windows benötigen Sie die Eingabeaufforderung oder die Anaconda-Eingabeaufforderung (installiert mit Anaconda und Miniconda).

## <a id="aznotebooks"></a>Azure Notebooks

[Azure Notebooks](https://notebooks.azure.com) (Vorschauversion) ist eine interaktive Umgebung in der Azure-Cloud. Dies ist die einfachste Methode für den Einstieg in die Azure Machine Learning-Entwicklung.

* Das Azure Machine Learning SDK ist bereits installiert.
* Nachdem Sie im Azure-Portal einen Arbeitsbereich für den Azure Machine Learning Service erstellt haben, können Sie über eine Schaltfläche Ihre Azure Notebook-Umgebung automatisch für die Arbeit mit dem Arbeitsbereich konfigurieren.

Informationen zum Einstieg in die Entwicklung mit Azure Notebooks finden Sie unter [Erste Schritte mit Azure Machine Learning](quickstart-get-started.md).

Standardmäßig verwendet Azure Notebooks einen kostenlosen Diensttarif, der auf 4GB Arbeitsspeicher und 1GB Daten beschränkt ist. Sie können diese Grenzwerte allerdings entfernen, indem Sie dem Azure Notebooks-Projekt eine Data Science Virtual Machine-Instanz anfügen. Weitere Informationen finden Sie unter [Verwalten und Konfigurieren von Projekten – Compute-Tarif](/azure/notebooks/configure-manage-azure-notebooks-projects#compute-tier).

## <a id="dsvm"></a>Data Science Virtual Machine

Die DSVM ist ein benutzerdefiniertes VM-Image. Sie ist für Data Science-Aufgaben konzipiert, die mit Folgendem vorab konfiguriert wurden:

  - Pakete wie TensorFlow, PyTorch, Scikit-learn, XGBoost und das Azure Machine Learning SDK.
  - Beliebte Data Science-Tools wie Spark (eigenständig) und Drill.
  - Azure-Tools wie die Azure CLI, AzCopy und Storage-Explorer.
  - Integrierte Entwicklungsumgebungen (IDEs), z.B. Visual Studio Code und PyCharm.
  - Jupyter Notebook-Server

Das Azure Machine Learning SDK funktioniert mit der Ubuntu- und der Windows-Version der DSVM. Wenn Sie die DSVM jedoch als Computeziel verwenden möchten, wird nur Ubuntu unterstützt.

Um die DSVM als Entwicklungsumgebung zu verwenden, gehen Sie folgendermaßen vor:

1. Erstellen Sie in einer der folgenden Umgebungen eine DSVM:

    * Azure-Portal:

        * [Erstellen einer Ubuntu-Data Science Virtual Machine](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro)

        * [Erstellen einer Windows-Data Science Virtual Machine](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/provision-vm)

    * Azure CLI:

        > [!IMPORTANT]
        > * Wenn Sie die Azure CLI verwenden, müssen Sie sich zuerst mit dem Befehl `az login` bei Ihrem Azure-Abonnement anmelden.
        >
        > * Wenn Sie die Befehle in diesem Schritt ausführen, müssen Sie einen Ressourcengruppennamen, einen Namen für die VM, einen Benutzernamen und ein Kennwort angeben.

        * Verwenden Sie zum Erstellen einer Data Science Virtual Machine-Instanz unter Ubuntu den folgenden Befehl:

            ```azurecli
            # create a Ubuntu DSVM in your resource group
            # note you need to be at least a contributor to the resource group in order to execute this command successfully
            # If you need to create a new resource group use: "az group create --name YOUR-RESOURCE-GROUP-NAME --location YOUR-REGION (For example: westus2)"
            az vm create --resource-group YOUR-RESOURCE-GROUP-NAME --name YOUR-VM-NAME --image microsoft-dsvm:linux-data-science-vm-ubuntu:linuxdsvmubuntu:latest --admin-username YOUR-USERNAME --admin-password YOUR-PASSWORD --generate-ssh-keys --authentication-type password
            ```

        * Verwenden Sie zum Erstellen einer Data Science Virtual Machine-Instanz unter Windows den folgenden Befehl:

            ```azurecli
            # create a Windows Server 2016 DSVM in your resource group
            # note you need to be at least a contributor to the resource group in order to execute this command successfully
            az vm create --resource-group YOUR-RESOURCE-GROUP-NAME --name YOUR-VM-NAME --image microsoft-dsvm:dsvm-windows:server-2016:latest --admin-username YOUR-USERNAME --admin-password YOUR-PASSWORD --authentication-type password
            ```    

2. Das Azure Machine Learning SDK ist auf der DSVM bereits installiert. Um die Conda-Umgebung mit dem SDK zu verwenden, führen Sie einen der folgenden Befehle aus:

    * Für die DSVM unter Ubuntu:

        ```shell
        conda activate py36
        ```

    * Für die DSVM unter Windows:

        ```shell
        conda activate AzureML
        ```

1. Um sicherzustellen, dass Sie auf das SDK zugreifen und die Version überprüfen können, verwenden Sie den folgenden Python-Code:

    ```python
    import azureml.core
    print(azureml.core.VERSION)
    ```

1. Informationen zum Konfigurieren der DSVM für Ihren Azure Machine Learning Service-Arbeitsbereich finden Sie im Abschnitt [Erstellen einer Konfigurationsdatei für den Arbeitsbereich](#workspace).

Weitere Informationen finden Sie unter [Data Science Virtual Machines](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/).

## <a id="local"></a>Lokaler Computer

Wenn Sie einen lokalen Computer verwenden (dabei kann es sich auch um einen virtuellen Remotecomputer handeln), gehen Sie folgendermaßen vor, um eine Conda-Umgebung zu erstellen und das SDK zu installieren:

1. Öffnen Sie eine Eingabeaufforderung oder Shell.

1. Erstellen Sie mithilfe der folgenden Befehle eine Conda-Umgebung:

    ```shell
    # create a new Conda environment with Python 3.6, NumPy, and Cython
    conda create -n myenv Python=3.6 cython numpy

    # activate the Conda environment
    conda activate myenv

    # On macOS run
    source activate myenv
    ```

    Das Erstellen der Umgebung kann mehrere Minuten in Anspruch nehmen, falls Python 3.6 und andere Komponenten heruntergeladen werden müssen.

1. Installieren Sie das Azure Machine Learning SDK mit Notebook-Zugaben und das SDK für die Datenaufbereitung, indem Sie den folgenden Befehl verwenden:

     ```shell
    pip install --upgrade azureml-sdk[notebooks,automl] azureml-dataprep
    ```

   > [!NOTE]
   > Wenn Sie eine Meldung mit dem Hinweis erhalten, dass PyYAML nicht deinstalliert werden kann, verwenden Sie stattdessen den folgenden Befehl:
   >
   > `pip install --upgrade azureml-sdk[notebooks,automl] azureml-dataprep --ignore-installed PyYAML`

   Die Installation des SDK kann mehrere Minuten dauern.

1. Installieren Sie Pakete für Ihre Machine Learning-Experimente. Verwenden Sie den folgenden Befehl, und ersetzen Sie *\<new package>* durch das zu installierende Paket:

    ```shell
    conda install <new package>
    ```

1. Um zu überprüfen, ob das SDK installiert wurde, verwenden Sie folgenden Python-Code:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

### <a id="jupyter"></a>Jupyter Notebooks

Jupyter Notebooks sind Teil des [Jupyter-Projekts](https://jupyter.org/). Sie stellen eine interaktive Programmierumgebung bereit, mit der Sie Dokumente erstellen können, in denen Livecode mit beschreibendem Text und Grafiken gemischt ist. Jupyter Notebooks sind auch eine hervorragende Möglichkeit, Ihre Ergebnisse mit anderen Personen zu teilen, da Sie die Ausgabe Ihrer Codeabschnitte im Dokument speichern können. Sie können Jupyter Notebooks auf vielen unterschiedlichen Plattformen installieren.

Mit dem Verfahren im Abschnitt [Lokaler Computer](#local) werden optionale Komponenten für Jupyter Notebooks installiert. Um diese Komponenten in Ihrer Jupyter Notebook-Umgebung zu aktivieren, gehen Sie folgendermaßen vor:

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

1. Um zu überprüfen, ob Jupyter Notebook das SDK verwenden kann, öffnen Sie ein neues Notebook, wählen Sie **myenv** als Kernel aus, und führen Sie dann folgenden Befehl in einer Notebookzelle aus:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

1. Informationen zum Konfigurieren des Jupyter Notebooks für Ihren Azure Machine Learning Service-Arbeitsbereich finden Sie im Abschnitt [Erstellen einer Konfigurationsdatei für den Arbeitsbereich](#workspace).

### <a id="vscode"></a>Visual Studio Code

Visual Studio Code ist ein Code-Editor für alle Plattformen. Für Python-Unterstützung benötigt er eine lokale Python 3- und Conda-Installation, aber er bietet zusätzliche Tools für die Arbeit mit künstlicher Intelligenz. Darüber hinaus bietet er Unterstützung für die Auswahl der Conda-Umgebung aus dem Code-Editor heraus.

Um Visual Studio Code für die Entwicklung zu verwenden, gehen Sie folgendermaßen vor:

1. Informationen zur Verwendung von Visual Studio Code für die Python-Entwicklung finden Sie unter [Erste Schritte mit Python in VS Code](https://code.visualstudio.com/docs/python/python-tutorial).

1. Um die Conda-Umgebung auszuwählen, öffnen Sie VS Code, und drücken Sie die Tastenkombination STRG+UMSCHALT+P (Linux und Windows) bzw. BEFEHL+UMSCHALT+P (Mac).  
    Die __Befehlspalette__ wird geöffnet. 

1. Geben Sie __Python: Select Interpreter__ ein, und wählen Sie dann die Conda-Umgebung aus.

1. Um sicherzustellen, dass Sie das SDK verwenden können, erstellen Sie eine neue Python-Datei (.py), die folgenden Code enthält, und führen Sie sie aus:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

1. Installationsanweisungen für die Azure Machine Learning-Erweiterung für Visual Studio Code finden Sie unter [Tools for AI](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-ai).

    Weitere Informationen finden Sie unter [Verwenden von Azure Machine Learning für Visual Studio Code](how-to-vscode-tools.md).

<a name="aml-databricks"></a>

## <a name="azure-databricks"></a>Azure Databricks

Sie können eine angepasste Version des Azure Machine Learning SDK für Azure Databricks für ein umfassendes, benutzerdefiniertes maschinelles Lernen verwenden. Alternativ dazu können Sie Ihr Modell in Databricks trainieren und mithilfe von [Visual Studio Code](how-to-vscode-train-deploy.md#deploy-your-service-from-vs-code) bereitstellen.

So bereiten Sie Ihren Databricks-Cluster vor und erhalten Beispiele für Notebooks

1. Erstellen Sie einen [Databricks-Cluster](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal) mit den folgenden Einstellungen:

    | Einstellung | Wert |
    |----|---|
    | Clustername | IhrClustername |
    | Databricks-Laufzeit | Beliebige Nicht-ML-Runtime (nicht ML 4.x, 5.x) |
    | Python-Version | 3 |
    | Worker | 2 oder mehr |

    Verwenden Sie diese Einstellungen nur, wenn Sie automatisiertes maschinelles Lernen mit Databricks verwenden:
    
    |   Einstellung | Wert |
    |----|---|
    | VM-Typen des Workerknotens | Arbeitsspeicheroptimierte VM bevorzugt |
    | Automatische Skalierung aktivieren | Deaktivieren |
    
    Die Anzahl der Workerknoten in Ihrem Databricks-Cluster bestimmt die maximale Anzahl gleichzeitiger Iterationen in den Einstellungen für automatisiertes ML.  

    Die Erstellung des Clusters dauert einige Minuten. Warten Sie, bis der Cluster ausgeführt wird, bevor Sie fortfahren.

1. Installieren Sie das Azure Machine Learning SDK-Paket, und fügen Sie es an Ihren Cluster an.  

    * [Erstellen Sie eine Bibliothek](https://docs.databricks.com/user-guide/libraries.html#create-a-library) mit einer dieser Einstellungen (_wählen Sie nur eine dieser Optionen aus_):
    
        * So installieren Sie das Azure Machine Learning SDK _ohne_ die Funktion für automatisiertes maschinelles Lernen
            | Einstellung | Wert |
            |----|---|
            |Quelle | Python Egg oder PyPI hochladen
            |PyPi-Name | azureml-sdk[databricks]
    
        * So installieren Sie das Azure Machine Learning SDK _mit_ automatisiertem maschinellen Lernen
            | Einstellung | Wert |
            |----|---|
            |Quelle | Python Egg oder PyPI hochladen
            |PyPi-Name | azureml-sdk[automl_databricks]
    
    * Aktivieren Sie nicht **Automatisch an alle Cluster anfügen**.

    * Aktivieren Sie **Anfügen** neben dem Namen Ihres Clusters.

    * Stellen Sie sicher, dass keine Fehler vorliegen, bis der Status zu **Angefügt** wechselt. Dies kann einige Minuten dauern.

    Wenn Sie eine alte SDK-Version haben, deaktivieren Sie diese in den installierten Bibliotheken des Clusters, und verschieben Sie sie in den Papierkorb. Installieren Sie die neue SDK-Version, und starten Sie den Cluster neu. Wenn danach ein Problem vorliegt, trennen Sie Ihren Cluster, und fügen Sie ihn wieder an.

    Wenn Sie fertig sind, wird die Bibliothek wie in den folgenden Abbildungen gezeigt angefügt. Beachten Sie diese [häufigen Probleme mit Databricks](resource-known-issues.md#databricks).

    * Wenn Sie das Azure Machine Learning SDK _ohne_ automatisiertes maschinelles Lernen installiert haben: ![SDK ohne installiertes automatisiertes maschinelles Lernen in Databricks](./media/how-to-configure-environment/amlsdk-withoutautoml.jpg)

    * Wenn Sie das Azure Machine Learning SDK _mit_ automatisiertes maschinelles Lernen installiert haben: ![SDK mit installiertem automatisierten maschinellen Lernen in Databricks](./media/how-to-configure-environment/automlonadb.jpg)

   Wenn dieser Schritt nicht ausgeführt werden kann, starten Sie Ihren Cluster folgendermaßen neu:

   a. Wählen Sie im linken Bereich die Option **Cluster** aus. 
   
   b. Wählen Sie in der Tabelle den Namen Ihres Clusters aus. 

   c. Klicken Sie auf der Registerkarte **Bibliotheken** auf **Neu starten**.

1. Laden Sie die [Notebookarchivdatei für Azure Databricks/Azure Machine Learning SDK](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/azure-databricks/Databricks_AMLSDK_1-4_6.dbc) herunter.

   >[!Warning]
   > Für die Verwendung mit Azure Machine Learning Service stehen zahlreiche Beispiel-Notebooks zur Verfügung. Mit Azure Databricks funktionieren nur [diese Beispiel-Notebooks](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/azure-databricks).

1.  [Importieren Sie die Archivdatei](https://docs.azuredatabricks.net/user-guide/notebooks/notebook-manage.html#import-an-archive) in Ihren Databricks-Cluster, und sehen Sie sich diese genauer an, wie auf der Seite [Machine Learning Notebooks](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/azure-databricks) beschrieben.


## <a id="workspace"></a>Erstellen einer Konfigurationsdatei für den Arbeitsbereich

Die Arbeitsbereichs-Konfigurationsdatei ist eine JSON-Datei, die das SDK zur Kommunikation mit Ihrem Azure Machine Learning-Dienstarbeitsbereich instruiert. Die Datei weist den Namen *config.json* und das folgende Format auf:

```json
{
    "subscription_id": "<subscription-id>",
    "resource_group": "<resource-group>",
    "workspace_name": "<workspace-name>"
}
```

Diese JSON-Datei muss sich in der Verzeichnisstruktur befinden, die Ihre Python-Skripts oder Jupyter Notebooks enthält. Sie kann sich im selben Verzeichnis, in einem Unterverzeichnis mit dem Namen *aml_config* oder in einem übergeordneten Verzeichnis befinden.

Um in Ihrem Code auf diese Datei zuzugreifen, verwenden Sie `ws=Workspace.from_config()`. Dieser Code lädt die Informationen aus der Datei und stellt eine Verbindung mit Ihrem Arbeitsbereich her.

Sie können die Konfigurationsdatei auf drei Arten erstellen:

* **Befolgen Sie die Anweisungen im [Schnellstart zu Azure Machine Learning](quickstart-get-started.md)**: In Ihrer Azure Notebooks-Bibliothek wird eine *config.json*-Datei erstellt. Diese Datei enthält die Konfigurationsinformationen für Ihren Arbeitsbereich. Sie können die Datei *config.json* in andere Entwicklungsumgebungen herunterladen oder kopieren.

* **Erstellen Sie die Datei manuell**: Bei dieser Methode verwenden Sie einen Text-Editor. Die Werte für die Konfigurationsdatei finden Sie in Ihrem Arbeitsbereich im [Azure-Portal](https://portal.azure.com). Kopieren Sie die Werte für den Arbeitsbereichsnamen, die Ressourcengruppe und die Abonnement-ID, und verwenden Sie sie in der Konfigurationsdatei.

     ![Azure-Portal](./media/how-to-configure-environment/configure.png)

* **Erstellen Sie die Datei programmgesteuert**: Im folgenden Codeausschnitt stellen Sie eine Verbindung mit einem Arbeitsbereich her, indem Sie die Abonnement-ID, die Ressourcengruppe und den Arbeitsbereichsnamen angeben. Anschließend speichert der Code die Arbeitsbereichskonfiguration in dieser Datei:

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

    Dieser Code schreibt die Konfigurationsdatei in die Datei *aml_config/config.json*.

## <a name="next-steps"></a>Nächste Schritte

- [Trainieren eines Modells](tutorial-train-models-with-aml.md) in Azure Machine Learning mit der MNIST-Datenbank
- Anzeigen der Referenz [Azure Machine Learning SDK für Python](https://aka.ms/aml-sdk)
- Informationen zum [Azure Machine Learning Data Prep SDK](https://aka.ms/data-prep-sdk)
