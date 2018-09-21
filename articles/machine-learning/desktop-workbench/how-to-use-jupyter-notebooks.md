---
title: Verwenden von Jupyter-Notebooks in Azure Machine Learning Workbench | Microsoft-Dokumentation
description: Handbuch für die Verwendung des Jupyter Notebook-Features von Azure Machine Learning Workbench
services: machine-learning
author: rastala
ms.author: roastala
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 11/09/2017
ms.openlocfilehash: 712cdaa65487620b2f8af4a0ad57c01c24b9a965
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2018
ms.locfileid: "35637496"
---
# <a name="use-jupyter-notebooks-in-azure-machine-learning-workbench"></a>Verwenden von Jupyter-Notebooks in Azure Machine Learning Workbench

Azure Machine Learning Workbench unterstützt interaktives Data Science-Experimentieren über die Integration in Jupyter-Notebooks. Dieser Artikel beschreibt die effektive Nutzung dieses Features, um Rate und Qualität Ihres interaktiven Data Science-Experimentierens zu erhöhen.

## <a name="prerequisites"></a>Voraussetzungen
- [Erstellen von Azure Machine Learning-Konten und Installieren von Azure Machine Learning Workbench](../service/quickstart-installation.md)
- Kenntnisse von [Jupyter Notebook](http://jupyter.org/). Dieser Artikel ist nicht dazu gedacht, sich mit der Verwendung von Jupyter vertraut zu machen.

## <a name="jupyter-notebook-architecture"></a>Jupyter Notebook-Architektur
Ganz allgemein umfasst die Jupyter Notebook-Architektur drei Komponenten. Jede kann in einer anderen Computeumgebung ausgeführt werden:

- **Client**: Empfängt eine Benutzereingabe und zeigt die gerenderte Ausgabe an.
- **Server**: Webserver, der die Notebook-Dateien (IPYNB-Dateien) hostet
- **Kernel**: die Laufzeitumgebung, in der die Ausführung der Notebook-Zellen stattfindet

Weitere Informationen finden Sie in der offiziellen [Jupyter-Dokumentation](http://jupyter.readthedocs.io/en/latest/architecture/how_jupyter_ipython_work.html). Das folgende Diagramm zeigt, wie diese Client-, Server- und Kernelarchitektur den Komponenten in Azure Machine Learning zugeordnet wird:

![Jupyter Notebook-Architektur](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-architecture.png)

## <a name="kernels-in-azure-machine-learning-workbench-notebooks"></a>Kernel in Azure Machine Learning Workbench-Notebooks
Sie können auf verschiedene Kernel in Azure Machine Learning Workbench zugreifen, indem Sie im `aml_config`-Ordner des Projekts Ausführungskonfigurationen und Computeziele konfigurieren. Das Hinzufügen eines neuen Computeziels mithilfe des `az ml computetarget attach`-Befehls entspricht dem Hinzufügen eines neuen Kernels.

>[!NOTE]
>Ausführlichere Informationen zu Ausführungskonfigurationen und Computezielen finden Sie unter [Configuring Azure Machine Learning Experimentation Service](experimentation-service-configuration.md) (Konfigurieren des Azure Machine Learning-Experimentieren-Diensts).

### <a name="kernel-naming-convention"></a>Kernelbenennungskonvention
Azure Machine Learning Workbench generiert benutzerdefinierte Jupyter-Kernel. Diese werden mit *\<Projektname> \<Name der Ausführungskonfiguration>* benannt. Wenn Sie beispielsweise eine Ausführungskonfiguration mit dem Namen _docker-python_ in einem Projekt mit dem Namen _myIris_ haben, stellt Azure Machine Learning einen Kernel mit dem Namen *myIris docker-python* zur Verfügung. Sie legen den ausgeführten Kernel im Menü **Kernel** von Jupyter Notebook im Untermenü **Change kernel** (Kernel ändern) fest. Der Name des ausgeführten Kernels wird auf der Menüleiste ganz rechts angezeigt.
 
Azure Machine Learning Workbench unterstützt derzeit die folgenden Kerneltypen.

### <a name="local-python-kernel"></a>Lokaler Python-Kernel
Dieser Python-Kernel unterstützt die Ausführung auf lokalen Computern. Er ist in die Unterstützung des Ausführungsverlaufs von Azure Machine Learning integriert. Der Name des Kernels ist in der Regel *my_project_name local*.

>[!NOTE]
>Verwenden Sie nicht den Kernel „Python 3“. Er ist ein eigenständiger Kernel, der von Jupyter standardmäßig bereitgestellt wird und nicht in die Azure Machine Learning-Funktionen integriert ist. Die magischen `%azureml`-Funktionen von Jupyter geben beispielsweise Fehler vom Typ „nicht gefunden“ zurück. 

### <a name="python-kernel-in-docker-local-or-remote"></a>Python-Kernel in Docker (lokal oder remote)
Dieser Python-Kernel wird entweder auf dem lokalen Computer oder einem virtuellen Linux-Remotecomputer in einem Docker-Container ausgeführt. Der Name des Kernels ist in der Regel *my_project docker*. In der zugeordneten `docker.runconfig`-Datei ist das `Framework`-Feld auf `Python` festgelegt.

### <a name="pyspark-kernel-in-docker-local-or-remote"></a>PySpark-Kernel in Docker (lokal oder remote)
Dieser entweder auf dem lokalen Computer oder einem virtuellen Linux-Remotecomputer im Docker-Container ausgeführte PySpark-Kernel führt Skripts in einem Spark-Kontext aus. Der Name des Kernels ist in der Regel *my_project docker*. In der zugeordneten `docker.runconfig`-Datei ist das `Framework`-Feld auf `PySpark` festgelegt.

### <a name="pyspark-kernel-in-an-azure-hdinsight-cluster"></a>PySpark-Kernel in einem Azure HDInsight-Cluster
Dieser Kernel wird auf dem Azure HDInsight-Remotecluster ausgeführt, den Sie als Computeziel für das Projekt angefügt haben. Der Name des Kernels ist in der Regel *my_project my_hdi*. 

>[!IMPORTANT]
>In der `.compute`-Datei für das HDI-Computeziel müssen Sie das Feld `yarnDeployMode` in `client` ändern (der Standardwert ist `cluster`), um diesen Kernel zu verwenden. 

## <a name="start-a-jupyter-server-from-azure-machine-learning-workbench"></a>Starten eines Jupyter-Servers über Azure Machine Learning Workbench
In Azure Machine Learning Workbench können Sie über die Registerkarte **Notebooks** auf Notebooks zugreifen. Das Beispielprojekt _Classifying Iris_ enthält ein `iris.ipynb`-Beispiel-Notebook.

![Registerkarte „Notebooks“](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-01.png)

Wenn Sie ein Notebook in Azure Machine Learning Workbench öffnen, wird es auf seiner eigenen Dokumentregisterkarte im **Vorschaumodus** angezeigt. Dies ist eine schreibgeschützte Ansicht, für die kein laufender Jupyter-Server und Kernel erforderlich ist.

![Notebook-Vorschau](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-02.png)

Durch Klicken auf **Start Notebook Server** (Notebook-Server starten) wird der Jupyter-Server gestartet und das Notebook in den **Bearbeitungsmodus** geschaltet. Die vertraute Jupyter Notebook-Benutzeroberfläche wird in die Workbench eingebettet angezeigt. Nun können Sie im Menü **Kernel** einen Kernel festlegen und die interaktive Notebook-Sitzung starten. 

>[!NOTE]
>Der Start eines nicht lokalen Kernels kann bei der ersten Verwendung ein bis zwei Minuten dauern. Sie können den `az ml experiment prepare`-Befehl im CLI-Fenster ausführen, um das Computeziel vorzubereiten, damit der Kernel anschließend viel schneller gestartet wird.

![Bearbeitungsmodus](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-04.png)

Dies ist eine vollständig interaktive Jupyter Notebook-Benutzeroberfläche. Alle regulären Notebook-Vorgänge und Tastenkombinationen werden in diesem Fenster mit Ausnahme einiger Dateivorgänge unterstützt, die über die Registerkarten **Notebooks** und **File** der Workbench ausgeführt werden können.

## <a name="start-a-jupyter-server-from-the-command-line"></a>Starten eines Jupyter-Servers über die Befehlszeile
Sie können eine Notebook-Sitzung auch starten, indem Sie den Befehl `az ml notebook start` im Befehlszeilenfenster eingeben:
```
$ az ml notebook start
[I 10:14:25.455 NotebookApp] The port 8888 is already in use, trying another port.
[I 10:14:25.464 NotebookApp] Serving notebooks from local directory: /Users/johnpelak/Desktop/IrisDemo
[I 10:14:25.465 NotebookApp] 0 active kernels 
[I 10:14:25.465 NotebookApp] The Jupyter Notebook is running at: http://localhost:8889/?token=1f0161ab88b22fc83f2083a93879ec5e8d0ec18490f0b953
[I 10:14:25.465 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[C 10:14:25.466 NotebookApp] 
    
Copy and paste this URL into your browser when you connect for the first time, to login with a token: http://localhost:8889/?token=1f0161ab88b22fc83f2083a93879ec5e8d0ec18490f0b953
[I 10:14:25.759 NotebookApp] Accepting one-time-token-authenticated connection from ::1
[I 10:16:52.970 NotebookApp] Kernel started: 7f8932e0-89b9-48b4-b5d0-e8f48d1da159
[I 10:16:53.854 NotebookApp] Adapting to protocol v5.1 for kernel 7f8932e0-89b9-48b4-b5d0-e8f48d1da159
```
Ihr Standardbrowser wird automatisch gestartet, und der Jupyter-Server verweist auf das Stammverzeichnis des Projekts. Sie können die URL und das Token, die im CLI-Fenster angezeigt werden, auch zum lokalen Öffnen anderer Browserfenster verwenden. 

![Projektdashboard](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-07.png)

Sie können jetzt eine `.ipynb`-Notebook-Datei auswählen und öffnen, den Kernel festlegen (falls er noch nicht festgelegt wurde) und die interaktive Sitzung starten.

![Projektdashboard](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-08.png)

## <a name="use-magic-commands-to-manage-experiments"></a>Verwenden von Magic-Befehlen zum Verwalten von Experimenten

Sie können [Magic-Befehle](http://ipython.readthedocs.io/en/stable/interactive/magics.html) innerhalb Ihrer Notebookzellen verwenden, um Ihren Ausführungsverlauf nachzuverfolgen und Ausgaben wie etwa Modelle oder Datasets zu speichern.

Um die Ausführung einzelner Notebookzellen nachzuverfolgen, verwenden Sie den Magic-Befehl `%azureml history on`. Nachdem Sie den Verlauf aktiviert haben, wird jede Zellausführung im Ausführungsverlauf als Eintrag angezeigt:

```
%azureml history on
from azureml.logging import get_azureml_logger
logger = get_azureml_logger()
logger.log("Cell","Load Data")
```

Verwenden Sie den magischen Befehl `%azureml history off`, um die Nachverfolgung der Zellausführungen zu deaktivieren.

Mithilfe des Magic-Befehls `%azureml upload` können Sie Modell- und Datendateien aus der Ausführung speichern. Die gespeicherten Objekte werden in der Ausführungsverlaufsansicht als Ausgaben angezeigt:

```
modelpath = os.path.join("outputs","model.pkl")
with open(modelpath,"wb") as f:
    pickle.dump(model,f)
%azureml upload outputs/model.pkl
```

>[!NOTE]
>Die Ausgaben müssen in einem Ordner mit dem Namen *outputs* gespeichert werden.

## <a name="next-steps"></a>Nächste Schritte
- Informationen zum Verwenden von Jupyter Notebook finden Sie in der [offiziellen Jupyter-Dokumentation](http://jupyter-notebook.readthedocs.io/en/latest/).    
- Weitere Informationen zur Ausführungsumgebung von Azure Machine Learning-Experimentieren finden Sie unter [Konfigurieren des Azure Machine Learning-Experimentieren-Diensts](experimentation-service-configuration.md).

