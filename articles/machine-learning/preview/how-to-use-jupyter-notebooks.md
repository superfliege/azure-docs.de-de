---
title: Verwenden von Jupyter-Notebooks in Azure Machine Learning Workbench | Microsoft-Dokumentation
description: "Handbuch für die Verwendung des Jupyter-Notebooks-Features von Azure Machine Learning Workbench"
services: machine-learning
author: rastala
ms.author: roastala
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 11/09/2017
ms.openlocfilehash: 80cdd07bff865776a68897a7b8c1b3fe66b76b18
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="how-to-use-jupyter-notebook-in-azure-machine-learning-workbench"></a>Verwenden des Jupyter-Notebooks in Azure Machine Learning Workbench

Azure Machine Learning Workbench unterstützt interaktives Data Science-Experimentieren über seine Integration des Jupyter-Notebooks. Dieser Artikel beschreibt die effektive Nutzung dieses Features, um Rate und Qualität Ihres interaktiven Data Science-Experimentierens zu erhöhen.

## <a name="prerequisites"></a>Voraussetzungen
- [Create Azure Machine Learning preview accounts and install Azure Machine Learning Workbench](/machine-learning/preview/quickstart-installation.md) (Erstellen von Azure Machine Learning-Vorschaukonten und Installieren von Azure Machine Learning Workbench).
- Machen Sie sich mit dem [Jupyter-Notebook](http://jupyter.org/) vertraut, da dessen Verwendung in diesem Artikel nicht gelehrt wird.

## <a name="jupyter-notebook-architecture"></a>Architektur des Jupyter-Notebooks
Auf dieser Ebene umfasst die Jupyter-Notebook-Architektur drei Komponenten, von denen jede in einer anderen Computeumgebung ausgeführt werden kann:

- **Client**: empfängt eine Benutzereingabe und zeigt die gerenderte Ausgabe an
- **Server**: Webserverhosten der Notebook-Dateien (IPYNB-Dateien)
- **Kernel**: die Laufzeitumgebung, in der die eigentliche Ausführung der Notebook-Zellen stattfindet

Weitere Informationen finden Sie in der offiziellen [Jupyter-Dokumentation](http://jupyter.readthedocs.io/en/latest/architecture/how_jupyter_ipython_work.html). Das folgende Diagramm zeigt, wie diese Client-, Server- und Kernelarchitektur den Komponenten in Azure ML zugeordnet wird.

![Notebook-Architektur](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-architecture.png)

## <a name="kernels-in-azure-ml-workbench-notebook"></a>Kernels im Azure ML Workbench-Notebook
Sie können in Azure ML Workbench auf viele verschiedene Kernels zugreifen, indem Sie im `aml_config`-Ordner des Projekts Ausführungskonfigurationen und Computeziele konfigurieren. Das Hinzufügen eines neuen Computeziels mithilfe des `az ml computetarget attach`-Befehls entspricht dem Hinzufügen eines neuen Kernels.

>[!NOTE]
>Unter [Overview of Azure Machine Learning experiment execution service](experimentation-service-configuration.md) (Übersicht über den Experimentausführungsdienst von Azure Machine Learning) finden Sie weitere Informationen zu Ausführungskonfigurationen und Computezielen.

### <a name="kernel-naming-convention"></a>Kernelbenennungskonvention
Der Kernel werden normalerweise im Format „\<Projektname > \<Name der Ausführungskonfiguration>“ benannt. Wenn Sie z.B. eine Ausführungskonfiguration mit dem Namen _docker-python_ in einem Projekt mit dem Namen _myIris_ haben, können Sie einen Kernel mit dem Namen „myIris docker-python“ in der Kernelliste finden, wenn Sie ein Jupyter-Notebook öffnen.

Die Workbench unterstützt derzeit die folgenden Kerneltypen.

### <a name="local-python-kernel"></a>Lokaler Python-Kernel
Dieser Python-Kernel unterstützt die Ausführung auf dem lokalen Computer. Er ist in die Unterstützung des Ausführungsverlaufs von Azure Machine Learning integriert. Der Name des Kernels ist in der Regel „my_project_name local“.

>[!NOTE]
>Verwenden Sie nicht den Kernel "Python 3". Dabei handelt es sich um einen eigenständigen Kernel, der von Jupyter standardmäßig bereitgestellt wird. Er ist nicht mit den Azure Machine Learning-Funktionen integriert.

### <a name="python-kernel-in-docker-local-or-remote"></a>Python-Kernel in Docker (lokal oder remote)
Dieser Python-Kernel wird entweder auf dem lokalen Computer oder einer Remote-Linux-VM in einem Docker-Container ausgeführt. Der Name des Kernels ist in der Regel „my_project docker“. In der zugeordneten `docker.runconfig`-Datei ist das `Framework`-Feld auf `Python` festgelegt.

### <a name="pyspark-kernel-in-docker-local-or-remote"></a>PySpark-Kernel in Docker (lokal oder remote)
Dieser entweder auf dem lokalen Computer oder einer Remote-Linux-VM im Docker-Container ausgeführte PySpark-Kernel führt Skripts in einem Spark-Kontext aus. Der Name des Kernels ist in der Regel „my_project docker“. In der zugeordneten `docker.runconfig`-Datei ist das `Framework`-Feld auf `PySpark` festgelegt.

### <a name="pyspark-kernel-on-hdinsight-cluster"></a>PySpark-Kernel auf HDInsight-Cluster
Dieser Kernel wird auf dem Remote-HDInsight-Cluster ausgeführt, den Sie als Computeziel für das Projekt angefügt haben. Der Name des Kernels ist in der Regel „my_project my_hdi“. 

>[!IMPORTANT]
>In der `.compute`-Datei für das HDI-Computeziel müssen Sie das `yarnDeployMode`-Feld in `client` ändern (der Standardwert ist `cluster`), um diesen Kernel zu verwenden. 

## <a name="start-jupyter-server-from-the-workbench"></a>Starten des Jupyter-Servers in der Workbench
In Azure Machine Learning Workbench können Sie über die Registerkarte **Notebooks** der Workbench auf Notebooks zugreifen. Das Beispielprojekt _Classifying Iris_ enthält ein `iris.ipynb`-Beispiel-Notebook.

![Registerkarte „Notebooks“](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-01.png)

Wenn ein Notebook in Azure Machine Learning Workbench geöffnet wird, wird es auf seiner eigenen Dokumentregisterkarte im **Vorschaumodus** angezeigt. Dies ist eine schreibgeschützte Ansicht, für die kein laufender Jupyter-Server und Kernel erforderlich ist.

![Notebook-Vorschau](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-02.png)

Durch Klicken auf **Start Notebook Server** wird der Jupyter-Server gestartet und das Notebook in den **Bearbeitungsmodus** geschaltet. Die vertraute Jupyter-Notebook-Benutzeroberfläche wird in die Workbench eingebettet angezeigt. Nun können Sie im Menü **Kernel** einen Kernel festlegen und die interaktive Notebook-Sitzung starten. 

>[!NOTE]
>Beachten Sie, dass der Start eines nicht lokalen Kernels bei der ersten Verwendung eine bis zwei Minuten dauern kann. Sie können den `az ml experiment prepare`-Befehl im CLI-Fenster ausführen, um das Computeziel vorzubereiten, damit der Kernel viel schneller starten kann.

![Bearbeitungsmodus](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-04.png)

Dies ist eine vollständig interaktive Jupyter-Notebook-Benutzeroberfläche. Alle regulären Notebook-Vorgänge und Tastenkombinationen mit Ausnahme einiger Dateivorgänge werden in diesem Fenster unterstützt, da sie über die Registerkarten **Notebooks** und **File** der Workbench ausgeführt werden können.

## <a name="start-jupyter-server-from-command-line"></a>Starten des Jupyter-Servers in der Befehlszeile
Sie können eine Notebook-Sitzung auch starten, indem Sie den Befehl `az ml notebook start` im Befehlszeilenfenster eingeben:
```
$ az ml notebook start
[I 10:14:25.455 NotebookApp] The port 8888 is already in use, trying another port.
[I 10:14:25.464 NotebookApp] Serving notebooks from local directory: /Users/johnpelak/Desktop/IrisDemo
[I 10:14:25.465 NotebookApp] 0 active kernels 
[I 10:14:25.465 NotebookApp] The Jupyter Notebook is running at: http://localhost:8889/?token=1f0161ab88b22fc83f2083a93879ec5e8d0ec18490f0b953
[I 10:14:25.465 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[C 10:14:25.466 NotebookApp] 
    
    Copy/paste this URL into your browser when you connect for the first time,
    to login with a token:
        http://localhost:8889/?token=1f0161ab88b22fc83f2083a93879ec5e8d0ec18490f0b953
[I 10:14:25.759 NotebookApp] Accepting one-time-token-authenticated connection from ::1
[I 10:16:52.970 NotebookApp] Kernel started: 7f8932e0-89b9-48b4-b5d0-e8f48d1da159
[I 10:16:53.854 NotebookApp] Adapting to protocol v5.1 for kernel 7f8932e0-89b9-48b4-b5d0-e8f48d1da159
```
Ihr Standardbrowser wird automatisch gestartet, und der Jupyter-Server verweist auf das Stammverzeichnis des Projekts. Sie können die URL und das Token, die im CLI-Fenster angezeigt werden, auch zum lokalen Starten anderer Browserfenster verwenden. 

![Projektdashboard](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-07.png)

Sie können jetzt auf eine `.ipynb`-Notebook-Datei klicken, sie öffnen und den Kernel festlegen (falls er noch nicht festgelegt wurde) und die interaktive Sitzung starten.

![Projektdashboard](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-08.png)

## <a name="use-magic-commands-to-manage-experiments"></a>Verwenden von Magic-Befehlen zum Verwalten von Experimenten

Sie können [Magic-Befehle](http://ipython.readthedocs.io/en/stable/interactive/magics.html) innerhalb Ihrer Notebookzellen verwenden, um Ihren Ausführungsverlauf nachzuverfolgen und Ausgaben zu speichern, wie etwa Modelle oder Datasets.

Um die Ausführung einzelner Notebookzellen nachzuverfolgen, verwenden Sie den Magic-Befehl „%azureml history on“. Nachdem Sie den Verlauf aktiviert haben, wird die Ausführung jeder Zelle im Verlauf als Eintrag angezeigt.

```
%azureml history on
from azureml.logging import get_azureml_logger
logger = get_azureml_logger()
logger.log("Cell","Load Data")
```

Verwenden Sie den Magic-Befehl „%azureml history off“, um die Nachverfolgung der Zellausführung zu deaktivieren.

Mithilfe des Magic-Befehls „%azureml upload“ können Sie Modell- und Datendateien aus der Ausführung speichern. Die gespeicherten Objekte werden in der Ausführungsverlaufsansicht für die betreffende Ausführung als Ausgaben angezeigt.

```
modelpath = os.path.join("outputs","model.pkl")
with open(modelpath,"wb") as f:
    pickle.dump(model,f)
%azureml upload outputs/model.pkl
```

>[!NOTE]
>Die Ausgaben müssen in einem Ordner mit dem Namen „outputs“ gespeichert werden.

## <a name="next-steps"></a>Nächste Schritte
- Informationen zum Verwenden des Jupyter-Notebooks finden Sie in der [offiziellen Jupyter-Dokumentation](http://jupyter-notebook.readthedocs.io/en/latest/).    
- Weitere Informationen zur Ausführungsumgebung von Azure ML-Experimenten finden Sie unter [Konfigurieren des Azure Machine Learning-Experimentieren-Diensts](experimentation-service-configuration.md).

