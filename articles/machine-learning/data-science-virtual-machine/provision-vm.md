---
title: Bereitstellen der Data Science Virtual Machine für Windows in Azure | Microsoft-Dokumentation
description: Konfigurieren und erstellen Sie für Analysen und Machine Learning einen virtuellen Computer für Data Science in Azure.
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: e1467c0f-497b-48f7-96a0-7f806a7bec0b
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.devlang: na
ms.topic: article
ms.date: 08/30/2018
ms.author: gokuma
ms.openlocfilehash: 37a30055e567b27f4e215dac162642d4447a8e49
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2018
ms.locfileid: "44298204"
---
# <a name="provision-the-windows-data-science-virtual-machine-on-azure"></a>Bereitstellen der Data Science Virtual Machine für Windows in Azure
Die Microsoft Data Science Virtual Machine (DSVM) ist ein Image des virtuellen Windows Azure-Computers (VM). Sie ist vorinstalliert und mit verschiedenen Tools konfiguriert, die für Datenanalysen und Machine Learning verwendet werden. Die folgenden Tools sind enthalten:

* [Azure Machine Learning](../service/index.yml) Workbench.
* [Microsoft Machine Learning Server](https://docs.microsoft.com/machine-learning-server/index) Developer Edition.
* Anaconda Python Distribution.
* Jupyter Notebook mit R, Python und PySpark-Kernels.
* Microsoft Visual Studio Community.
* Microsoft Power BI Desktop.
* Microsoft SQL Server 2017 Developer Edition.
* Eine eigenständige Apache Spark-Instanz für lokale Entwicklung und Tests.
* [JuliaPro](https://juliacomputing.com/products/juliapro.html).
* Machine Learning und Datenanalysetools:
  * Deep Learning-Frameworks. Ein umfassender Satz von KI-Frameworks, z. B. [Microsoft Cognitive Toolkit](https://www.microsoft.com/en-us/cognitive-toolkit/), [TensorFlow](https://www.tensorflow.org/), [Chainer](https://chainer.org/), mxNet und Keras.
  * [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit). Ein schnelles Machine Learning-System, das verschiedene Verfahren unterstützt, wie z. B. Online, Hashing, Allreduce, Reductions, Learning2Search sowie Active und Interactive Learning.
  * [XGBoost](https://xgboost.readthedocs.org/en/latest/). Ein Tool, das eine schnelle und präzise Boosted Tree-Implementierung ermöglicht.
  * [Rattle](https://togaware.com/rattle/), R Analytical Tool To Learn Easily (R-Analysetool zum einfachen Lernen). Ein Tool, das die ersten Schritte von Datenanalysen und Machine Learning in R vereinfacht. Es umfasst eine GUI-basierte Datenuntersuchung und -modellierung mit automatischer R-Codegenerierung.
  * [Weka](http://www.cs.waikato.ac.nz/ml/weka/). Eine Software für visuelles Data Mining und Machine Learning in Java.
  * [Apache Drill](https://drill.apache.org/). Eine schemalose SQL-Abfrage-Engine für Apache Hadoop, NoSQL und Cloud Storage.  Sie unterstützt ODBC- und JDBC-Schnittstellen für NoSQL- und Dateiabfragen mit BI-Standardtools wie Power BI, Microsoft Excel und Tableau.
* Bibliotheken in R und Python zur Verwendung in Azure Machine Learning und anderen Azure-Diensten.
* Git, einschließlich Git Bash, für die Arbeit mit Quellcoderepositorys einschließlich GitHub und Azure DevOps. Git stellt mehrere gängige Linux-Befehlszeilenprogramme bereit, auf die sowohl in Git Bash als auch über eine Eingabeaufforderung zugegriffen werden kann. Beispiele sind awk, sed, perl, grep, find, wget und curl.

Der Data Science-Vorgang umfasst eine Aufgabensequenz:

1. Suchen, Laden und Vorverarbeiten von Daten.
1. Kompilieren und testen von Modellen.
1. Bereitstellen der Modelle für die Nutzung in intelligenten Anwendungen.

Datenanalysten verwenden verschiedene Tools für diese Aufgaben. Es kann zeitaufwändig sein, die richtigen Softwareversionen zu finden und dann herunterzuladen und zu installieren. Die Microsoft Data Science Virtual Machine erspart Zeit, indem sie ein einsatzbereites Image bietet, das mit vorinstallierten und konfigurierten gängigen Tools auf Azure bereitgestellt werden kann. 

Mit der Microsoft Data Science Virtual Machine können Sie gleich mit Ihrem Analyseprojekt loslegen. Sie können in verschiedenen Sprachen arbeiten, z. B. R, Python, SQL und C#. Visual Studio bietet eine einfach zu bedienende integrierte Entwicklungsumgebung (IDE) zum Entwickeln und Testen des Codes. Das Azure-SDK ist in der VM enthalten. Daher können Sie Ihre Anwendungen unter Verwendung verschiedener Dienste auf der Microsoft-Cloudplattform erstellen. 

Für dieses Data Science VM-Image fallen keine Softwaregebühren an. Sie zahlen nur die Azure-Nutzungsgebühren. Diese hängen von der Größe des virtuellen Computers ab, den Sie bereitstellen. Weitere Informationen zu Compute-Gebühren finden Sie auf der Seite [Data Science Virtual Machine](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.windows-data-science-vm?tab=PlansAndPrice) im Abschnitt **Preisübersicht**. 

## <a name="other-versions-of-the-data-science-virtual-machine"></a>Andere Versionen der Data Science Virtual Machine
* Ein [Ubuntu](dsvm-ubuntu-intro.md)-Image. Er verfügt über viele Tools, die denen der DSVM gleichen, sowie einige zusätzliche Deep Learning-Frameworks. 
* Ein [Linux CentOS](linux-dsvm-intro.md)-Image.
* Die [Windows Server 2012-Edition](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.standard-data-science-vm) der Data Science VM. Einige Tools sind nur in der Windows Server 2016-Edition verfügbar. Ansonsten gilt dieser Artikel auch für die Windows Server 2012-Edition.

## <a name="prerequisite"></a>Voraussetzung
Um eine Microsoft Data Science Virtual Machine zu erstellen, benötigen Sie ein Azure-Abonnement. Siehe [Kostenlose Azure-Testversion](http://azure.com/free).


## <a name="create-your-microsoft-data-science-virtual-machine"></a>Erstellen Ihrer Microsoft Data Science Virtual Machine
Führen Sie die folgenden Schritte aus, um eine Instanz der Microsoft Data Science Virtual Machine zu erstellen:

1. Wechseln Sie im [Azure-Portal](https://portal.azure.com/#create/microsoft-dsvm.dsvm-windowsserver-2016)zur Auflistung der virtuellen Computer. Wenn Sie sich noch nicht bei Ihrem Azure-Konto angemeldet haben, erhalten Sie eine entsprechende Aufforderung.
1. Wählen Sie unten auf der Seite die Schaltfläche **Erstellen** aus, um einen Assistenten aufzurufen.

  ![configure-data-science-vm](./media/provision-vm/configure-data-science-virtual-machine.png) 

1. Der Assistent für die Microsoft Data Science Virtual Machine erfordert eine **Eingabe**. Die folgende Eingabe ist zum Konfigurieren der einzelnen, auf der rechten Seite der Abbildung dargestellten Schritte erforderlich:

  a. **Grundlagen**:

    i. **Name**: Der Name des Data Science-Servers, den Sie erstellen.  

    ii. **VM-Datenträgertyp**. Wählen Sie **SSD** oder **HDD** aus. Wählen Sie für eine GPU-Instanz „NC_v1“, wie NVidia Tesla K80-basiert, **HDD** als Datenträgertyp aus.   

    iii. **Benutzername**. Die Administratorkonto-ID für die Anmeldung.   

    iv. **Kennwort**. Das Kennwort des Administratorkontos.  

    v. **Abonnement**. Wenn Sie über mehrere Abonnements verfügen, wählen Sie eines aus, über das der Computer erstellt und abgerechnet wird.   

    vi. **Ressourcengruppe**. Sie können eine neue Gruppe erstellen oder eine vorhandene Gruppe verwenden.   

    vii. **Standort**. Wählen Sie das Rechenzentrum aus, das am besten geeignet ist. Um den schnellsten Netzwerkzugriff zu erreichen, handelt es sich dabei um das Rechenzentrum, in dem der größte Teil Ihrer Daten gespeichert ist oder das Ihrem physischen Standort am nächsten ist.   

  b. **Größe**. Wählen Sie einen Servertyp aus, der die funktionalen Anforderungen und den Kostenrahmen erfüllt. Wählen Sie **Alle anzeigen** aus, um weitere Optionen für VM-Größen anzuzeigen.  

  c. **Einstellungen**:  

    i. **Managed Disks verwenden**. Wählen Sie **Verwaltet**, wenn Sie möchten, dass Azure die Datenträger für die VM verwaltet. Andernfalls müssen Sie ein neues oder vorhandenes Speicherkonto angeben.  

    ii. **Weitere Parameter**. Sie können die Standardwerte verwenden. Zeigen Sie mit der Maus auf den jeweiligen Informationslink, um Hilfe zu bestimmten Feldern anzuzeigen, falls Sie auch andere als die Standardwerte verwenden möchten.  

  d. **Zusammenfassung**. Stellen Sie sicher, dass alle eingegebenen Informationen richtig sind. Klicken Sie auf **Erstellen**. 

> [!NOTE]
> * Für die VM gelten keine über die Computekosten für die Servergröße, die Sie unter **Größe** ausgewählt haben, hinausgehenden Kosten. 
> * Die Bereitstellung dauert ungefähr 10 bis 20 Minuten. Der Status wird im Azure-Portal anzeigt.
> 
> 

## <a name="how-to-access-the-microsoft-data-science-virtual-machine"></a>Zugreifen auf die Microsoft Data Science Virtual Machine
Nachdem die VM erstellt und bereitgestellt wurde, können Sie sich mithilfe von Remotedesktop mit den Anmeldeinformationen des Administratorkontos anmelden, die Sie zuvor im Abschnitt **Grundlagen** erstellt haben. Sie können nun beginnen, die auf der VM installierten und konfigurierten Tools zu verwenden. Für viele der Tools gibt es Desktopsymbole und Kacheln im Startmenü. 


## <a name="tools-installed-on-the-microsoft-data-science-virtual-machine"></a>In der Microsoft Data Science Virtual Machine installierte Tools

### <a name="microsoft-machine-learning-server-developer-edition"></a>Microsoft Machine Learning Server Developer Edition
Sie können Microsoft Enterprise Library für skalierbares R oder Python für Ihre Analysen verwenden, da die Machine Learning Server Developer Edition auf der VM installiert ist. Machine Learning Server, zuvor als Microsoft R Server bekannt, ist eine in großem Maße bereitstellbare Analyseplattform für Unternehmen. Sie ist für R und Python verfügbar und skalierbar, kommerziell unterstützt und sicher. 

Machine Learning Server unterstützt verschiedene Aufgaben für Big Data-Statistiken, Vorhersagemodellierung und Machine Learning. Die gesamte Bandbreite der Analytik wird unterstützt: Durchsuchen, Analyse, Visualisierung und Modellierung. Durch die Verwendung und Erweiterung der Open-Source-Sprachen R und Python ist Machine Learning Server kompatibel mit R- und Python-Skripts und -Funktionen. Darüber hinaus ist es auch mit CRAN-, Pip- und Conda-Paketen kompatibel, um Daten auf Unternehmensebene zu analysieren. 

Mit der Möglichkeit, Daten parallel und in großen Stücken verarbeiten zu können, hebt Machine Learning Server die Speicherbeschränkungen der Open-Source-Sprache R auf. Somit können Sie Analysen von Daten, deren Größe die des Hauptspeichers bei weitem überschreitet, ausführen. Das auf dem virtuellen Computer befindliche Visual Studio Community enthält die R-Tools für Visual Studio und die Python-Tools für die Visual Studio-Erweiterung (PTVS), die eine vollständige IDE für das Arbeiten mit R oder Python bieten. Auf der VM werden außerdem andere IDEs bereitgestellt, z. B. [RStudio](http://www.rstudio.com) und die [PyCharm Community Edition](https://www.jetbrains.com/pycharm/). 

### <a name="python"></a>Python
Für die Entwicklung mit Python wurden die Anaconda Python-Distributionen 2.7 und 3.6 installiert. Diese Distributionen enthalten die Python-Basisversion sowie etwa 300 der beliebtesten Pakete für Mathematik, Entwicklung und Datenanalysen. Sie können PTVS verwenden, das in Visual Studio Community 2017 installiert ist. Oder Sie können eine der mit Anaconda gebündelten IDEs verwenden, z. B. IDLE oder Spyder. Suchen und starten Sie eines dieser Pakete (Windows-Taste+S).

> [!NOTE]
> Um unter Anaconda Python 2.7 auf Python Tools für Visual Studio zu verweisen, müssen Sie für jede Version eine angepasste Umgebung erstellen. Um diese Umgebungspfade in Visual Studio 2017 Community zu erstellen, navigieren Sie zu **Tools**   > **Python Tools** > **Python Environments**. Wählen Sie dann **+ Benutzerdefiniert** aus. 
> 
> 

Anaconda Python 3.6 wird unter **C:\Anaconda** installiert. Anaconda Python 2.7wird unter **c:\Anaconda\envs\python2** installiert. Ausführliche Schritte hierzu finden Sie in der [PTVS-Dokumentation](https://docs.microsoft.com/visualstudio/python/installing-python-interpreters). 

### <a name="the-jupyter-notebook"></a>Das Jupyter Notebook
Zur Anaconda-Distribution gehört außerdem das Jupyter Notebook, eine Umgebung zum Freigeben von Code und Analysen. Der Jupyter Notebook-Server wurde bereits mit Python 2.7, Python 3.x, PySpark, Julia und R-Kernels konfiguriert. Es ist ein Desktopsymbol mit dem Namen **Jupyter Notebook** vorhanden, um den Jupyter-Server und den Browser für den Zugriff auf den Notebook-Server zu starten. 

In den Python- und R-Paketen sind mehrere Beispiele für Notebooks enthalten. Nach dem Zugriff auf Jupyter zeigen die Notebooks, wie man mit den folgenden Technologien arbeitet:

* Machine Learning Server.
* SQL Server Machine Learning Services, datenbankinterne Analysen. 
* Python.
* Microsoft Cognitive ToolKit.
* Tensorflow.
* Andere Azure-Technologien. 

Der Link zu den Beispielen wird Ihnen auf der Startseite für die Notebooks angezeigt, nachdem Sie sich mit dem im vorherigen Schritt festgelegten Kennwort am Jupyter Notebook authentifiziert haben. 

### <a name="visual-studio-community-2017"></a>Visual Studio Community 2017
Visual Studio Community ist auf der VM installiert. Dies ist eine kostenlose Version der beliebten IDE von Microsoft, die Sie zur Evaluierung und für kleine Teams verwenden können. Weitere Informationen finden Sie in den [Lizenzierungsbedingungen](https://www.visualstudio.com/support/legal/mt171547). 

Öffnen Sie Visual Studio durch Doppelklicken auf das Desktopsymbol oder über das **Startmenü** . Suchen Sie nach Programmen (Windows-Taste+S), gefolgt von **Visual Studio**. Von dort aus können Sie Projekte in Sprachen wie C#, Python, R und node.js erstellen. Installierte Plug-Ins erleichtert die Arbeit mit den folgenden Azure-Diensten:
* Azure Data Catalog
* Azure HDInsight Hadoop und Spark
* Azure Data Lake 

Es gibt auch ein Plug-In namens ```Visual Studio Tools for AI```, das sich nahtlos in Azure Machine Learning integrieren lässt und Sie bei der schnellen Erstellung von KI-Anwendungen unterstützt. 

> [!NOTE]
> Es kann eine Meldung angezeigt werden, die besagt, dass der Evaluierungszeitraum abgelaufen ist. Geben Sie die Anmeldeinformationen Ihres Microsoft-Kontos ein. Oder erstellen Sie ein neues kostenloses Konto für den Zugriff auf Visual Studio Community. 
> 
> 

### <a name="sql-server-2017-developer-edition"></a>SQL Server 2017 Developer Edition
Eine Developer-Version von SQL Server 2017 mit Machine Learning Services für Datenbankanalysen wird auf dem virtuellen Computer in R oder Python bereitgestellt. Machine Learning Services bietet eine Plattform zum Entwickeln und Bereitstellen von intelligenten Anwendungen. Sie können diese Sprachen sowie viele Pakete aus der Community zum Erstellen von Modellen und Generieren von Vorhersagen mithilfe Ihrer SQL Server-Daten nutzen. So ist die Analytics-Komponente nah an den Daten angeordnet, weil Machine Learning Services (datenbankintern) die Sprachen R und Python in SQL Server integriert. Durch diese Integration vermeiden Sie die Kosten und Sicherheitsrisiken, die mit einer Datenverschiebung verbunden sind.

> [!NOTE]
> Die Developer Edition von SQL Server ist nur für Entwicklungs- und Testzwecke bestimmt. Sie benötigen eine Lizenz für die Ausführung in der Produktion. 
> 
> 

Sie können auf SQL Server zugreifen, indem Sie Microsoft SQL Server Management Studio starten. Ihr VM-Name wird als **Servername** angegeben. Verwenden Sie die Windows-Authentifizierung, wenn Sie sich als Administrator unter Windows anmelden. Wenn SQL Server Management Studio gestartet wurde, können Sie andere Benutzer erstellen, Datenbanken erstellen, Daten importieren und SQL-Abfragen ausführen. 

Um datenbankinterne Analysen mit SQL Machine Learning Services zu ermöglichen, führen Sie nach der Anmeldung als Serveradministrator in SQL Server Management Studio einmal den folgenden Befehl aus: 

        CREATE LOGIN [%COMPUTERNAME%\SQLRUserGroup] FROM WINDOWS 

        (Please replace the %COMPUTERNAME% with your VM name)


### <a name="azure"></a>Azure
Mehrere Azure-Tools werden auf dem virtuellen Computer installiert:

* Über eine Desktopverknüpfung gelangen Sie zur Azure SDK-Dokumentation. 
* **AzCopy** wird zum Verschieben von Daten in Ihr und aus Ihrem Azure Storage-Konto verwendet. Um die Nutzung anzuzeigen, geben Sie an einer Eingabeaufforderung **Azcopy** ein. 
* Verwenden Sie **Azure Storage Explorer**, um die in Ihrem Azure Storage-Konto gespeicherten Objekte zu durchsuchen. Außerdem können Sie Daten in den und aus dem Azure Storage übertragen. Um auf dieses Tool zuzugreifen, können Sie **Storage-Explorer** in das Feld **Suche** eingeben. Oder Sie suchen das Tool im Windows-Startmenü. 
* **Adlcopy** verschiebt Daten in Azure Data Lake. Geben Sie **adlcopy** in einer Befehlszeile ein, um die Nutzung anzuzeigen. 
* **dtui** verschiebt Daten in und aus Azure Cosmos DB, einer NoSQL-Datenbank in der Cloud. Geben Sie an einer Eingabeaufforderung **dtui** ein. 
* **Azure Data Factory Integration Runtime** verschiebt Daten zwischen lokalen Datenquellen und der Cloud. Es wird in Tools wie Azure Data Factory verwendet. 
* **Microsoft Azure PowerShell** ist ein Tool zur Verwaltung Ihrer Azure-Ressourcen in der PowerShell-Skriptsprache. Es ist auch auf Ihrem virtuellen Computer installiert. 

### <a name="power-bi"></a>Power BI
Das **Power BI Desktop** wurde installiert, um Sie bei der Erstellung von Dashboards und Visualisierungen zu unterstützen. Verwenden Sie dieses Tool, um Daten aus verschiedenen Quellen abzurufen, Dashboards und Berichte zu erstellen und diese in der Cloud zu veröffentlichen. Weitere Informationen finden Sie auf der [Power BI](http://powerbi.microsoft.com)-Website. Sie finden den Power BI-Desktop im Startmenü. 

> [!NOTE]
> Für den Zugriff auf Power BI benötigen Sie ein Microsoft Office 365-Konto. 
> 
> 

### <a name="azure-machine-learning-workbench"></a>Azure Machine Learning Workbench

Azure Machine Learning Workbench ist eine Desktopanwendung und Befehlszeilenschnittstelle. Die Workbench verfügt über eine integrierte Datenvorbereitung, bei der Ihre Schritte zur Datenvorbereitung erlernt werden, während Sie sie ausführen. Außerdem wird die Produktivität durch Features wie Projektmanagement, Ausführungsverlauf und Notebook-Integration gesteigert. 

Sie können Open-Source-Frameworks für die Entwicklung Ihrer Modelle nutzen, z. B. TensorFlow, Cognitive Toolkit, Spark ML und scikit-learn. Die DSVM enthält ein Desktopsymbol zum Installieren von Azure Machine Learning Workbench im Verzeichnis **%LOCALAPPDATA%** der einzelnen Benutzer. 

Jeder Benutzer, der Workbench muss eine einmalige Aktion ausführen. Doppelklicken Sie auf das ```AzureML Workbench Setup```-Desktopsymbol, um die Workbenchinstanz zu installieren. Azure Machine Learning erstellt und nutzt auch eine Python-Umgebung pro Benutzer, die in das Verzeichnis **%LOCALAPPDATA%\amlworkbench\python**“ extrahiert wird.

## <a name="more-microsoft-development-tools"></a>Weitere Microsoft-Entwicklungstools
Der [Microsoft-Webplattform-Installer](https://www.microsoft.com/web/downloads/platform.aspx) wird verwendet, um weitere Microsoft-Entwicklungstools zu suchen und herunterzuladen. Es gibt auch eine Verknüpfung mit dem Tool auf dem Desktop der Microsoft Data Science Virtual Machine.  

## <a name="important-directories-on-the-vm"></a>Wichtige Verzeichnisse auf dem virtuellen Computer
| Item | Verzeichnis |
| --- | --- |
| Jupyter Notebook-Serverkonfigurationen | C:\ProgramData\jupyter |
| Basisverzeichnis für Jupyter Notebook-Beispiele | c:\dsvm\notebooks und c:\benutzer\<benutzername>\notebooks |
| Weitere Beispiele | c:\dsvm\samples |
| Anaconda (Standard: Python 3.6) | c:\Anaconda |
| Anaconda Python 2.7-Umgebung | c:\Anaconda\envs\python2 |
| Microsoft Machine Learning Server (eigenständig) Python | C:\Programme\Microsoft\ML Server\PYTHON_SERVER |
| R-Standardinstanz, Machine Learning Server (eigenständig) | C:\Programme\Microsoft\ML Server\R_SERVER |
| SQL Machine Learning Services-Instanzverzeichnis (datenbankintern) | C:\Programme\Microsoft SQL Server\MSSQL14.MSSQLSERVER |
| Azure Machine Learning Workbench (pro Benutzer) | %localappdata%\amlworkbench | 
| Verschiedene Tools | c:\dsvm\tools |

> [!NOTE]
> Unter der Windows Server 2012-Edition der DSVM und der Windows Server 2016-Edition vor März 2018 ist Python 2.7 die Anaconda-Standardumgebung. Die sekundäre Umgebung ist Python 3.5 unter **c:\Anaconda\envs\py35**. 
> 
> 

## <a name="next-steps"></a>Nächste Schritte

* Schauen Sie sich die Tools für die Data Science-VM an, indem Sie das Startmenü auswählen.
* Informieren Sie sich über Azure Machine Learning Services und Workbench, indem Sie die [Seite mit Schnellstarts und Tutorials](../service/index.yml) für das Produkt besuchen. 
* Navigieren Sie zu **C:\Programme\Microsoft\ML Server\R_SERVER\library\RevoScaleR\demoScripts**, um Beispiele für die RevoScaleR-Bibliothek in R zu finden, die Datenanalysen auf Unternehmensebene unterstützt.  
* Lesen Sie den Artikel [Zehn Dinge, die Sie mit der Data Science Virtual Machine machen können](http://aka.ms/dsvmtenthings).
* Erfahren Sie, wie Sie mithilfe des [Team Data Science-Prozesses](../team-data-science-process/index.yml)systematisch vollständige Analyselösungen erstellen.
* Besuchen Sie die [Azure AI Gallery](http://gallery.cortanaintelligence.com), um Beispiele zu Machine Learning und zur Datenanalyse zu erhalten, in denen Azure Machine Learning und dazugehörige Datendienste in Azure verwendet werden. Wir haben auch ein Symbol für diese Galerie im Startmenü und auf dem Desktop der virtuellen Maschine bereitgestellt.

