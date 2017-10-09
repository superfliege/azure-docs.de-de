---
title: Was ist Azure Machine Learning? | Microsoft-Dokumentation
description: "Enthält eine Übersicht über Machine Learning-Experimentieren und die Modellverwaltung. Hierbei handelt es sich um eine integrierte End-to-End-Data Science-Lösung für Data Science-Profis, mit der erweiterte Analyseanwendungen für die Cloud entwickelt und für Experimente genutzt und bereitgestellt werden."
services: machine-learning
author: haining
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: get-started-article
ms.date: 09/21/2017
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: c4919fb679eeb4d25eb0066b9bf617b057d44354
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="what-is-azure-machine-learning"></a>Was ist Azure Machine Learning?

Azure Machine Learning ist eine integrierte End-to-End-Lösung für Data Science und Advanced Analytics. Data Scientists können sie nutzen, um Daten vorzubereiten, Experimente zu entwickeln und Modelle für die Cloud bereitzustellen.

Die wichtigsten Komponenten von Azure Machine Learning sind:
- Azure Machine Learning Workbench
- Azure Machine Learning-Experimentieren-Dienst
- Azure Machine Learning-Modellverwaltungsdienst
- Microsoft Machine Learning-Bibliotheken für Apache Spark (MMLSpark Library)
- Visual Studio Code-Tools für KI

Zusammen stellen diese Anwendungen und Dienste eine Hilfe dar, mit der Sie die Entwicklung und Bereitstellung Ihrer Data Science-Projekte erheblich beschleunigen können. 

![Azure Machine Learning-Konzepte](media/overview-what-is-azure-ml/aml-concepts.png)

## <a name="open-source-compatible"></a>Open-Source-Kompatibilität

Azure Machine Learning verfügt über eine umfassende Unterstützung von Open-Source-Technologie. Sie können mehrere zehntausend Open-Source-Python-Pakete nutzen, z.B. die folgenden Machine Learning-Frameworks:

- [scikit-learn](http://scikit-learn.org/)
- [TensorFlow](https://www.tensorflow.org/)
- [Microsoft Cognitive Toolkit](https://www.microsoft.com/en-us/cognitive-toolkit/)
- [Spark ML](https://spark.apache.org/docs/2.1.1/ml-pipeline.html)

Sie können Ihre Experimente in verwalteten Umgebungen durchführen, z.B. Docker-Containern und Spark-Clustern. Außerdem können Sie erweiterte Hardware, z.B. [GPU-fähige virtuelle Computer in Azure](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-gpu), verwenden, um die Ausführung zu beschleunigen.

Azure Machine Learning basiert auf der folgenden Open-Source-Technologie:

- [Jupyter Notebook](http://jupyter.org/)
- [Apache Spark](https://spark.apache.org/)
- [Docker](https://www.docker.com/)
- [Kubernetes](https://kubernetes.io/)
- [Python](https://www.python.org/)
- [Conda](https://conda.io/docs/)

Außerdem umfasst Azure Machine Learning die eigene Open-Source-Technologie von Microsoft, z.B. [Microsoft Machine Learning-Bibliothek für Apache Spark](https://github.com/Azure/mmlspark) und Cognitive Toolkit.

Zudem profitieren Sie von professioneller, altbewährter Lerntechnologie, die von Microsoft entworfen wurde, um Probleme größeren Umfangs zu lösen. Diese Komponenten haben sich bereits in vielen Microsoft-Produkten bewährt, z.B.:

- Windows
- Bing
- Xbox
- Office
- SQL Server

Hier sind einige Komponenten der Machine Learning-Technologie von Microsoft aufgeführt, die in Azure Machine Learning enthalten sind:

- [PROSE](https://microsoft.github.io/prose/) (PROgram Synthesis using Examples)
- [microsoftml](https://docs.microsoft.com/r-server/r/concept-what-is-the-microsoftml-package)
- [revoscalepy](https://docs.microsoft.com/r-server/python-reference/revoscalepy/revoscalepy-package)

## <a name="azure-machine-learning-workbench"></a>Azure Machine Learning Workbench
Azure Machine Learning Workbench ist eine Desktopanwendung mit Befehlszeilentools und Unterstützung für Windows und macOS. Mit dieser Anwendung können Sie für Machine Learning-Lösungen den gesamten Data Science-Lebenszyklus verwalten:

- Datenerfassung und -vorbereitung
- Modellentwicklung und Experimentverwaltung
- Modellbereitstellung in verschiedenen Zielumgebungen

Hier sind die wichtigsten Funktionen von Azure Machine Learning Workbench aufgeführt:

- Tool für die Datenvorbereitung, das die Datentransformation anhand von Beispielen erlernen kann.
- Auf die Datenquellenabstraktion kann per Benutzeroberfläche und Python-Code zugegriffen werden.
- Python SDK zum Aufrufen von visuell erstellten Paketen für die Datenvorbereitung.
- Integrierter Jupyter-Notebook-Dienst und Clientbenutzeroberfläche.
- Experimentüberwachung und -verwaltung über Ansichten von Ausführungsverläufen.
- Rollenbasierte Zugriffssteuerung, die die Freigabe und Kollaboration per Azure Active Directory ermöglicht.
- Automatische Projektmomentaufnahmen für jede Ausführung und explizite Versionskontrolle durch native Git-Integration. 
- Integration in beliebte Python-IDEs.

Weitere Informationen finden Sie in den folgenden Artikeln:
- [Anleitung zur Datenvorbereitung](data-prep-user-guide.md)
- [Using Git with Azure Machine Learning](using-git-ml-project.md) (Verwenden von Git mit Azure Machine Learning)
- [Using Jupyter Notebook in Azure Machine Learning](how-to-use-jupyter-notebooks.md) (Verwenden von Jupyter-Notebook in Azure Machine Learning)
- Roaming und Freigabe
- [Run History Guide](how-to-use-run-history-model-metrics.md) (Leitfaden zum Ausführungsverlauf)
- [IDE Integration](how-to-configure-your-IDE.md) (IDE-Integration)

## <a name="azure-machine-learning-experimentation-service"></a>Azure Machine Learning-Experimentieren-Dienst
Mit dem Experimentieren-Dienst wird die Ausführung von Machine Learning-Experimenten verarbeitet. Außerdem verfügt er über Unterstützung für die Workbench mit Funktionen für Projektmanagement, Git-Integration, Zugriffssteuerung, Roaming und Freigabe. 

Über eine einfache Konfiguration können Sie Ihre Experimente für viele verschiedene Computeumgebungsoptionen ausführen:

- Lokal nativ
- Lokaler Docker-Container
- Docker-Container auf einem virtuellen Remotecomputer
- Horizontales Hochskalieren eines Spark-Clusters in Azure

Mit dem Experimentieren-Dienst werden virtuelle Umgebungen erstellt, um sicherzustellen, dass Ihr Skript mit reproduzierbaren Ergebnissen isoliert ausgeführt werden kann. Es werden Informationen zum Ausführungsverlauf aufgezeichnet, und der Verlauf wird grafisch dargestellt. So können Sie aus Ihren Experimentausführungen leicht das beste Modell auswählen. 

Weitere Informationen finden Sie unter [Overview of Azure Machine Learning experiment execution service](experiment-execution-configuration.md) (Übersicht über den Azure Machine Learning-Experimentieren-Ausführungsdienst).

## <a name="azure-machine-learning-model-management-service"></a>Azure Machine Learning-Modellverwaltungsdienst

Mit dem Modellverwaltungsdienst können Data Scientists und Entwicklerteams Vorhersagemodelle für viele unterschiedliche Umgebungen bereitstellen. Die Modellversionen und -herkunft werden von den Trainingsläufen bis hin zu den Bereitstellungen nachverfolgt. Die Modelle werden in der Cloud gespeichert, registriert und verwaltet. 

Mit einfachen CLI-Befehlen können Sie Ihr Modell, Bewertungsskripts und Abhängigkeiten in Docker-Images in Containern verpacken. Diese Images werden in Ihrer eigenen Docker-Registrierung registriert, die in Azure (Azure Container Registry) gehostet wird. Sie können zuverlässig für die folgenden Ziele bereitgestellt werden:

- Lokale Computer
- Lokale Server
- Cloud
- IoT Edge-Geräte

Eine Kubernetes-Ausführung in Azure Container Service (ACS) wird für die Cloudbereitstellung für horizontales Skalieren verwendet. Die Telemetrie der Modellausführung wird in AppInsights für die visuelle Analyse erfasst. 

Weitere Informationen zum Modellverwaltungsdienst finden Sie unter [Übersicht über die Modellverwaltung](model-management-overview.md).


## <a name="microsoft-machine-learning-library-for-apache-spark"></a>Microsoft Machine Learning-Bibliothek für Apache Spark

[MMLSpark](https://github.com/Azure/mmlspark) (Microsoft Machine Learning-Bibliothek für Apache Spark) ist ein Open-Source-Spark-Paket mit Deep Learning- und Data Science-Tools für Apache Spark. Hiermit werden [Spark Machine Learning-Pipelines](https://spark.apache.org/docs/2.1.1/ml-pipeline.html) in das [Microsoft Cognitive Toolkit](https://www.microsoft.com/en-us/cognitive-toolkit/) und die [OpenCV](http://opencv.org/)-Bibliothek integriert. So können Sie schnell leistungsstarke, hoch skalierbare Vorhersage- und Analysemodelle für große Image- und Textdatasets erstellen. Einige Highlights:

- Einfaches Erfassen von Images aus HDFS in Spark DataFrame
- Vorverarbeiten von Imagedaten mit Transformationen aus OpenCV
- Ausstatten von Images mit Features anhand von vortrainierten Deep Neural Networks mit dem Microsoft Cognitive Toolkit 
- Verwenden von vortrainierten bidirektionalen LSTMs aus Keras zum Extrahieren von medizinischen Entitäten
- Trainieren von DNN-basierten Imageklassifizierungsmodellen auf VMs mit GPU der N-Serie in Azure
- Ausstatten von Freiform-Textdaten mit Features mit benutzerfreundlichen APIs zusätzlich zu Primitiven in SparkML über einen einzelnen Transformator
- Einfaches Trainieren von Klassifizierungs- und Regressionsmodellen durch die implizite Ausstattung von Daten mit Features
- Berechnen eines umfassenden Satzes von Auswertungsmetriken mit Metriken pro Instanz

Weitere Informationen finden Sie unter [Using MMLSpark in Azure Machine Learning](how-to-use-mmlspark.md) (Verwenden von MMLSpark in Azure Machine Learning).

## <a name="visual-studio-code-tools-for-ai"></a>Visual Studio Code-Tools für KI
Visual Studio Code-Tools für KI ist eine Erweiterung in Visual Studio Code zum Erstellen, Testen und Bereitstellen von Deep Learning- und KI-Lösungen. Die Tools enthalten viele Integrationspunkte für Azure Machine Learning, z.B.:
- Einen Ausführungsverlauf mit Anzeige der Leistung von Trainingsläufen und protokollierten Metriken
- Eine Katalogansicht, über die Benutzer neue Projekte mit dem Microsoft Cognitive Toolkit, TensorFlow und vielen anderen Deep Learning-Frameworks durchsuchen und das Bootstrapping dafür durchführen können 
- Eine Explorer-Ansicht zum Auswählen von Computezielen für Ihre auszuführenden Skripts
 

## <a name="what-are-the-machine-learning-options-from-microsoft"></a>Welche Machine Learning-Optionen gibt es bei Microsoft?
Neben Azure Machine Learning gibt es in Azure eine Vielzahl von Optionen zum Erstellen, Bereitstellen und Verwalten von Machine Learning-Modellen. 
* Microsoft Machine Learning-Dienste in SQL Server
* Microsoft Machine Learning Server
* Virtueller Data Science-Computer
* Spark MLLib in HDInsight
* Batch AI Training-Dienst
* Microsoft Cognitive Toolkit
* Microsoft Cognitive Services


### <a name="microsoft-machine-learning-services-in-sql-server"></a>Microsoft Machine Learning-Dienste in SQL Server
Mit [Microsoft Machine Learning-Diensten](https://docs.microsoft.com/sql/advanced-analytics/r/r-services) können Sie Machine Learning-Modelle mit R oder Python ausführen, trainieren und bereitstellen. Sie können Daten verwenden, die lokal und in SQL Server-Datenbanken gespeichert sind. 

Verwenden Sie Microsoft Machine Learning-Dienste, wenn Sie Modelle lokal oder unter Microsoft SQL Server trainieren oder bereitstellen möchten. Modelle, die mit Machine Learning-Diensten erstellt werden, können per Azure Machine Learning-Modellverwaltung bereitgestellt werden. 

### <a name="microsoft-machine-learning-server"></a>Microsoft Machine Learning Server 
[Microsoft Machine Learning Server](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone) ist ein Unternehmensserver zum Hosten und Verwalten von parallelen und verteilten Workloads für R- und Python-Prozesse. Microsoft Machine Learning Server kann unter Linux, Windows, Hadoop und Apache Spark ausgeführt werden. Außerdem ist die Anwendung unter [HDInsight](https://azure.microsoft.com/services/hdinsight/r-server/) verfügbar. Sie enthält ein Ausführungsmodul für Lösungen, die mit [Microsoft Machine Learning-Paketen](https://docs.microsoft.com/r-server/r/concept-what-is-the-microsoftml-package) erstellt wurden, und erweitert R- und Python-Open-Source-Lösungen um Unterstützung für die folgenden Szenarien:

- Hochleistungsanalysen
- Statistische Analysen
- Machine Learning
- Sehr große Datasets

Mehrwertfunktionen werden anhand von eigenen Paketen bereitgestellt, die mit dem Server installiert werden. Für die Entwicklung können Sie IDEs wie beispielsweise [R Tools für Visual Studio](https://www.visualstudio.com/vs/rtvs/) und [Python Tools für Visual Studio](https://www.visualstudio.com/vs/python/) verwenden.

Nutzen Sie Microsoft Machine Learning Server für folgende Zwecke:

- Erstellen und Bereitstellen von Modellen, die mit R und Python auf einem Server erstellt werden
- Bedarfsabhängiges Verteilen von R- und Python-Training in einem Hadoop- oder Spark-Cluster

### <a name="data-science-virtual-machine"></a>Virtueller Computer für Data Science
Der [virtuelle Computer für Data Science (DSVM)](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-virtual-machine-overview) ist ein benutzerdefiniertes VM-Image in der Microsoft Azure-Cloud, das speziell für Data Science konfiguriert wurde. Es hat viele beliebte Data Science und andere Tools vorinstalliert und vorkonfiguriert, damit Sie sofort intelligente Anwendungen für die erweiterte Analyse erstellen können. Er ist unter Windows Server und unter Linux verfügbar. Wir bieten eine Windows-Edition von DSVM für Windows Server 2016 und 2012. Wir bieten eine Linux-Edition des DSVM in Linux-Distributionen auf der Basis von Ubuntu 16.04 LTS und OpenLogic 7.2 CentOS an. 

Verwenden Sie den virtuellen Computer für Data Science, wenn Sie Ihre Aufträge auf einem einzelnen Knoten ausführen oder hosten müssen. Eine anderer Grund für die Nutzung kann das zentrale Hochskalieren eines einzelnen Computers per Remotezugriff sein. Der virtuelle Data Science-Computer wird als Ziel für Azure Machine Learning-Experimentieren und für die Azure Machine Learning-Modellverwaltung unterstützt. 

### <a name="spark-mllib-in-hdinsight"></a>Spark MLLib in HDInsight
Mit [Spark MLLib in HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-ipython-notebook-machine-learning) können Sie Modelle als Teil von Spark-Aufträgen erstellen, die für Big Data-Szenarien ausgeführt werden. Spark ermöglicht Ihnen das einfache Transformieren und Vorbereiten von Daten und die anschließende Erstellung eines Modells für das horizontale Hochskalieren innerhalb eines einzelnen Auftrags. Modelle, die per Spark MLLib erstellt werden, können über die Azure Machine Learning-Modellverwaltung bereitgestellt, verwaltet und überwacht werden. Trainingsläufe können mit Azure Machine Learning-Experimentieren weitergeleitet und verwaltet werden. Außerdem kann Spark zum horizontalen Hochskalieren von Datenvorbereitungsaufträgen verwendet werden, die in Machine Learning Workbench erstellt werden. 

Nutzen Sie Spark, wenn Sie im Rahmen einer Datenpipeline Ihre Datenverarbeitung horizontal hochskalieren und Modelle erstellen müssen. Sie können Spark-Aufträge in Scala, Java, Python oder R erstellen. 

### <a name="batch-ai-training"></a>Batch AI Training 
Mit [Azure Batch AI Training](https://aka.ms/batchaitraining) können Sie mit Ihren KI-Modellen parallel experimentieren, indem Sie ein beliebiges Framework verwenden, und diese dann bedarfsabhängig über GPU-Cluster hinweg trainieren. Beschreiben Sie Ihre Auftragsanforderungen und die auszuführende Konfiguration, und wir erledigen den Rest. 

Batch AI Training ermöglicht Ihnen das horizontale Hochskalieren von Deep Learning-Aufträgen über GPU-Cluster hinweg, indem Sie beispielsweise die folgenden Frameworks verwenden:

- Cognitive Toolkit
- Caffe
- Chainer
- TensorFlow

Sie können die Azure Machine Learning-Modellverwaltung verwenden, um Modelle aus Batch AI Training bereitzustellen, zu verwalten und zu überwachen.  Es ist geplant, Batch AI Training in Azure Machine Learning-Experimentieren zu integrieren. 

### <a name="microsoft-cognitive-toolkit"></a>Microsoft Cognitive Toolkit
Das [Microsoft Cognitive Toolkit](https://www.microsoft.com/en-us/cognitive-toolkit/) ist ein vereinheitlichtes Deep Learning-Toolkit, mit dem neuronale Netzwerke als Berechnungsschritte in einem gerichteten Graphen beschrieben werden. In diesem gerichteten Graphen stehen Blattknoten für Eingabewerte oder Netzwerkparameter, während andere Knoten Matrixvorgänge für die Eingaben darstellen. Mit dem Cognitive Toolkit können Sie beliebte Modelltypen leicht realisieren und kombinieren, z.B. DNNs für die Feedweiterleitung, Convolutional Nets (CNNs) und Recurrent Networks (RNNs/LSTMs). Es wird Learning mit stochastischen Gradientenverfahren (Stochastic Gradient Descent (SGD), Zurückverteilung von Fehlern) und automatischer Differenzierung und Parallelisierung über mehrere GPUs und Server hinweg implementiert.

Verwenden Sie das Cognitive Toolkit, wenn Sie ein Modell per Deep Learning erstellen möchten.  Das Cognitive Toolkit kann in allen obigen Diensten genutzt werden.

### <a name="microsoft-cognitive-services"></a>Microsoft Cognitive Services
Bei Microsoft Cognitive Services handelt es sich um eine Gruppe von 30 APIs, mit denen Sie Apps erstellen können, für die natürliche Kommunikationsmethoden genutzt werden. Mit diesen APIs können Ihre Apps mit nur wenigen Codezeilen sehen, hören, sprechen, verstehen und unsere Anforderungen interpretieren. Fügen Sie Ihren Apps auf einfache Weise intelligente Features hinzu, z.B.: 

- Emotions- und Stimmungserkennung
- Maschinelles Sehen und Spracherkennung
- Sprachverständnis
- Wissen und Suche

Microsoft Cognitive Services kann verwendet werden, um Apps übergreifend für Geräte und Plattformen zu entwickeln. Die APIs werden ständig verbessert und lassen sich einfach einrichten. 

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="next-steps"></a>Nächste Schritte
* [Install and create Azure Machine Learning](quickstart-installation.md) (Installieren und Erstellen von Azure Machine Learning)

