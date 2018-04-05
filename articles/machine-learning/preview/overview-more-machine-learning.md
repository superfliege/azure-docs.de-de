---
title: Was ist Azure Machine Learning? | Microsoft-Dokumentation
description: Enthält eine Übersicht über Machine Learning-Experimentieren und die Modellverwaltung. Hierbei handelt es sich um eine integrierte End-to-End-Data Science-Lösung für Data Science-Profis, mit der erweiterte Analyseanwendungen für die Cloud entwickelt und für Experimente genutzt und bereitgestellt werden.
services: machine-learning
author: haining
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: overview
ms.date: 03/28/2018
ms.openlocfilehash: 1c4826959bb92e2e515867261094b1154a3c805c
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="machine-learning-solutions-from-microsoft"></a>Machine Learning-Lösungen von Microsoft

Neben [Azure Machine Learning](overview-what-is-azure-ml.md) bietet Microsoft eine Vielzahl von Optionen für die Erstellung, Bereitstellung und Verwaltung Ihrer Machine Learning-Modelle. 
* SQL Server Machine Learning Services
* Microsoft Machine Learning Server
* Azure Machine Learning Services
* Azure Machine Learning Studio
* Virtueller Computer für Data Science
* Spark MLLib in HDInsight
* Batch AI Training-Dienst
* Microsoft Cognitive Toolkit
* Microsoft Cognitive Services


## <a name="sql-server-machine-learning-services"></a>SQL Server Machine Learning Services
Mit [Microsoft Machine Learning-Diensten](https://docs.microsoft.com/sql/advanced-analytics/r/r-services) können Sie Machine Learning-Modelle mit R oder Python ausführen, trainieren und bereitstellen. Sie können Daten verwenden, die lokal und in SQL Server-Datenbanken gespeichert sind. 

Verwenden Sie Microsoft Machine Learning-Dienste, wenn Sie Modelle lokal oder unter Microsoft SQL Server trainieren oder bereitstellen möchten. Modelle, die mit Machine Learning-Diensten erstellt werden, können per Azure Machine Learning-Modellverwaltung bereitgestellt werden. 

## <a name="microsoft-machine-learning-server"></a>Microsoft Machine Learning Server 
[Microsoft Machine Learning Server](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone) ist ein Unternehmensserver zum Hosten und Verwalten von parallelen und verteilten Workloads für R- und Python-Prozesse. Microsoft Machine Learning Server kann unter Linux, Windows, Hadoop und Apache Spark ausgeführt werden. Außerdem ist die Anwendung unter [HDInsight](https://azure.microsoft.com/services/hdinsight/r-server/) verfügbar. Sie enthält eine Ausführungs-Engine für Lösungen, die mit [Microsoft Machine Learning-Paketen](https://docs.microsoft.com/r-server/r/concept-what-is-the-microsoftml-package) erstellt wurden, und erweitert R- und Python-Open-Source-Lösungen um Unterstützung für die folgenden Szenarien:

- Hochleistungsanalysen
- Statistische Analysen
- Machine Learning
- Sehr große Datasets

Mehrwertfunktionen werden anhand von eigenen Paketen bereitgestellt, die mit dem Server installiert werden. Für die Entwicklung können Sie IDEs wie beispielsweise [R Tools für Visual Studio](https://www.visualstudio.com/vs/rtvs/) und [Python Tools für Visual Studio](https://www.visualstudio.com/vs/python/) verwenden.

Nutzen Sie Microsoft Machine Learning Server für folgende Zwecke:

- Erstellen und Bereitstellen von Modellen, die mit R und Python auf einem Server erstellt werden
- Bedarfsabhängiges Verteilen von R- und Python-Training in einem Hadoop- oder Spark-Cluster

## <a name="data-science-virtual-machine"></a>Virtueller Computer für Data Science
Der [virtuelle Computer für Data Science (DSVM)](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/overview) ist ein benutzerdefiniertes VM-Image in der Microsoft Azure-Cloud, das speziell für Data Science konfiguriert wurde. Es hat viele beliebte Data Science und andere Tools vorinstalliert und vorkonfiguriert, damit Sie sofort intelligente Anwendungen für die erweiterte Analyse erstellen können. Er ist unter Windows Server und unter Linux verfügbar. Wir bieten eine Windows-Edition von DSVM für Windows Server 2016 und 2012. Wir bieten eine Linux-Edition des DSVM in Linux-Distributionen auf der Basis von Ubuntu 16.04 LTS und OpenLogic 7.2 CentOS an. 

Verwenden Sie den virtuellen Computer für Data Science, wenn Sie Ihre Aufträge auf einem einzelnen Knoten ausführen oder hosten müssen. Eine anderer Grund für die Nutzung kann das zentrale Hochskalieren eines einzelnen Computers per Remotezugriff sein. Der virtuelle Data Science-Computer wird als Ziel für Azure Machine Learning-Experimentieren und für die Azure Machine Learning-Modellverwaltung unterstützt. 

## <a name="spark-mllib-in-hdinsight"></a>Spark MLLib in HDInsight
Mit [Spark MLLib in HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-ipython-notebook-machine-learning) können Sie Modelle als Teil von Spark-Aufträgen erstellen, die für Big Data-Szenarien ausgeführt werden. Spark ermöglicht Ihnen das einfache Transformieren und Vorbereiten von Daten und die anschließende Erstellung eines Modells für das horizontale Hochskalieren innerhalb eines einzelnen Auftrags. Modelle, die per Spark MLLib erstellt werden, können über die Azure Machine Learning-Modellverwaltung bereitgestellt, verwaltet und überwacht werden. Trainingsläufe können mit Azure Machine Learning-Experimentieren weitergeleitet und verwaltet werden. Außerdem kann Spark zum horizontalen Hochskalieren von Datenvorbereitungsaufträgen verwendet werden, die in Machine Learning Workbench erstellt werden. 

Nutzen Sie Spark, wenn Sie im Rahmen einer Datenpipeline Ihre Datenverarbeitung horizontal hochskalieren und Modelle erstellen müssen. Sie können Spark-Aufträge in Scala, Java, Python oder R erstellen. 

## <a name="batch-ai-training"></a>Batch AI Training 
Mit [Azure Batch AI Training](https://aka.ms/batchaitraining) können Sie mit Ihren KI-Modellen parallel experimentieren, indem Sie ein beliebiges Framework verwenden, und diese dann bedarfsabhängig über GPU-Cluster hinweg trainieren. Beschreiben Sie Ihre Auftragsanforderungen und die auszuführende Konfiguration, und wir erledigen den Rest. 

Batch AI Training ermöglicht Ihnen das horizontale Hochskalieren von Deep Learning-Aufträgen über GPU-Cluster hinweg, indem Sie beispielsweise die folgenden Frameworks verwenden:

- Cognitive Toolkit
- Caffe
- Chainer
- TensorFlow

Sie können die Azure Machine Learning-Modellverwaltung verwenden, um Modelle aus Batch AI Training bereitzustellen, zu verwalten und zu überwachen.  Es ist geplant, Batch AI Training in Azure Machine Learning-Experimentieren zu integrieren. 

## <a name="microsoft-cognitive-toolkit"></a>Microsoft Cognitive Toolkit
Das [Microsoft Cognitive Toolkit](https://www.microsoft.com/en-us/cognitive-toolkit/) ist ein vereinheitlichtes Deep Learning-Toolkit, mit dem neuronale Netzwerke als Berechnungsschritte in einem gerichteten Graphen beschrieben werden. In diesem gerichteten Graphen stehen Blattknoten für Eingabewerte oder Netzwerkparameter, während andere Knoten Matrixvorgänge für die Eingaben darstellen. Mit dem Cognitive Toolkit können Sie beliebte Modelltypen leicht realisieren und kombinieren, z.B. DNNs für die Feedweiterleitung, Convolutional Nets (CNNs) und Recurrent Networks (RNNs/LSTMs). Es wird Learning mit stochastischen Gradientenverfahren (Stochastic Gradient Descent (SGD), Zurückverteilung von Fehlern) und automatischer Differenzierung und Parallelisierung über mehrere GPUs und Server hinweg implementiert.

Verwenden Sie das Cognitive Toolkit, wenn Sie ein Modell per Deep Learning erstellen möchten.  Das Cognitive Toolkit kann in allen obigen Diensten genutzt werden.

## <a name="microsoft-cognitive-services"></a>Microsoft Cognitive Services
Bei Microsoft Cognitive Services handelt es sich um eine Gruppe von 30 APIs, mit denen Sie Apps erstellen können, für die natürliche Kommunikationsmethoden genutzt werden. Mit diesen APIs können Ihre Apps mit nur wenigen Codezeilen sehen, hören, sprechen, verstehen und unsere Anforderungen interpretieren. Fügen Sie Ihren Apps auf einfache Weise intelligente Features hinzu, z.B.: 

- Emotions- und Stimmungserkennung
- Maschinelles Sehen und Spracherkennung
- Sprachverständnis
- Wissen und Suche

Microsoft Cognitive Services kann verwendet werden, um Apps übergreifend für Geräte und Plattformen zu entwickeln. Die APIs werden ständig verbessert und lassen sich einfach einrichten. 

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die Übersicht für [Azure Machine Learning](overview-what-is-azure-ml.md).