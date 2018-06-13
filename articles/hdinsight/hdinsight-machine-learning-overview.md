---
title: Übersicht über Machine Learning – Azure HDInsight | Microsoft-Dokumentation
description: In diesem Artikel werden die Optionen für das maschinelle Lernen in HDInsight beschrieben.
services: hdinsight
documentationcenter: ''
tags: azure-portal
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 01/19/2018
ms.author: nitinme
ms.openlocfilehash: b80c49b67c7cca835ae08346561984e168e3df4e
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
ms.locfileid: "31392169"
---
# <a name="machine-learning-on-hdinsight"></a>Machine Learning in HDInsight

HDInsight ermöglicht Machine Learning mit Big Data und bietet so die Möglichkeit, wertvolle Erkenntnisse aus großen Mengen (Petabytes, ja sogar Exabytes) strukturierter, unstrukturierter und sich schnell verändernder Daten zu gewinnen. HDInsight umfasst mehrere Optionen für maschinelles Lernen: SparkML und MLlib, R, Hive und das Microsoft Cognitive Toolkit.

## <a name="sparkml-and-mllib"></a>SparkML und MLlib

[HDInsight Spark](spark/apache-spark-overview.md) ist ein in Azure gehostetes Angebot von [Spark](http://spark.apache.org/), einem einheitlichen Open-Source-Framework für die Parallelverarbeitung von Daten, das die arbeitsspeicherinterne Verarbeitung unterstützt, um die Leistung von Big Data-Analysen zu steigern. Die Spark-Verarbeitungs-Engine ist auf Geschwindigkeit, einfache Nutzung und anspruchsvolle Analysen ausgelegt. Dank seiner verteilten In-Memory-Datenverarbeitungsfunktionen eignet sich Spark besonders für iterative Algorithmen beim maschinellen Lernen und für Graphberechnungen. Es gibt zwei skalierbare Bibliotheken für maschinelles Lernen, die algorithmische Modellierungsfunktionen in dieser verteilten Umgebung bereitstellen: MLlib und SparkML. MLlib enthält die Original-API, die auf Grundlage von RDDs erstellt wurde. SparkML ist ein neueres Paket, das eine API auf höherer Ebene bereitstellt, die auf Dataframes basiert, um ML-Pipelines zu erstellen. SparkML unterstützt noch nicht alle Funktionen von MLlib, ersetzt jedoch MLlib als Machine Learning-Standardbibliothek in Spark.

Die Microsoft Machine Learning-Bibliothek für Apache Spark ist [MMLSpark](https://github.com/Azure/mmlspark). Diese Bibliothek ist darauf ausgelegt, die Produktivität von Datenanalysten mit Spark zu steigern, die Experimentierrate zu erhöhen und innovative Verfahren für das maschinelle Lernen, einschließlich Deep Learning, in sehr großen Datasets zu nutzen. MMLSpark bietet zusätzlich zu den SparkML-APIs auf niedriger Ebene eine weitere Ebene beim Erstellen skalierbarer ML-Modelle, z.B. zum Indizieren von Zeichenfolgen, Umwandeln von Daten in ein von Algorithmen für maschinelles Lernen erwartetes Layout und Zusammenstellen von Featurevektoren. Die MMLSpark-Bibliothek vereinfacht diese und andere gängige Aufgaben zum Erstellen von Modellen in PySpark.

## <a name="r"></a>R

[R](https://www.r-project.org/) ist derzeit die weltweit am häufigsten verwendete Programmiersprache für statistische Berechnungen. Es handelt sich um ein Open-Source-Datenvisualisierungstool mit einer Community von mehr als 2,5 Millionen Benutzern, die stetig wächst. Mit der stark wachsenden Benutzerbasis und mehr als 8.000 verteilten Paketen ist R die wahrscheinlichste Wahl für viele Unternehmen, die maschinelles Lernen nutzen möchten. Sie können einen HDInsight-Cluster mit R Server erstellen, der bereit ist für den Einsatz mit sehr großen Datasets und Modellen. Diese Funktion bietet Datenanalysten und Statistikern eine vertraute R-Schnittstelle, die bei Bedarf durch HDInsight skaliert werden kann – ohne den zusätzlichen Aufwand der Einrichtung und Wartung eines Clusters.

![Training für die Vorhersage mit R Server](./media/hdinsight-machine-learning-overview/r-training.png)

Der Edgeknoten eines Clusters ist ein praktischer Ort für die Verbindungsherstellung mit dem Cluster und die Ausführung Ihrer R-Skripts.  Außerdem können Sie R-Skripts über die Knoten des Clusters hinweg ausführen, indem Sie Hadoop MapReduce von ScaleR oder Spark-Computekontexte verwenden.

Mit R Server auf HDInsight mit Spark können Sie das Training auf den Knoten eines Clusters durch Verwenden eines Spark-Computekontexts parallel ausführen. Sie können R-Skripts direkt auf dem Edgeknoten ausführen und bei Bedarf alle verfügbaren Kerne parallel verwenden. Alternativ können Sie den Code auch über den Edgeknoten ausführen, um die Verarbeitung zu starten, die auf alle Knoten im Cluster verteilt wird. R Server auf HDInsight mit Spark ermöglicht zudem bei Bedarf parallele Funktionen von Open-Source-R-Paketen.

## <a name="azure-machine-learning-and-hive"></a>Azure Machine Learning und Hive

Azure Machine Learning verfügt über Tools zur Modellierung von Predictive Analytics-Lösungen und einen vollständig verwalteten Dienst, über den Sie Ihre Vorhersagemodelle als sofort nutzbare Webdienste bereitstellen können. Azure Machine Learning ist eine vollständige Predictive Analytics-Lösung in der Cloud, die Sie zum Erstellen, Testen, Operationalisieren und Verwalten von Vorhersagemodelle verwenden können. Wählen Sie aus einer großen Algorithmusbibliothek aus, verwenden Sie ein webbasiertes Studio zum Erstellen von Modellen, und stellen Sie Ihr Modell ganz einfach als Webdienst bereit.

![Verfügbarkeit erweiterter Analysen für Hadoop mit Microsoft Azure Machine Learning](./media/hdinsight-machine-learning-overview/hadoop-azure-ml.png)

Sie können Features für Daten in einem HDInsight Hadoop-Cluster mit [Hive-Abfragen](../machine-learning/team-data-science-process/create-features-hive.md) erstellen. Bei der *Featureentwicklung* wird versucht, die Vorhersageleistung der Lernalgorithmen durch Erstellen von Merkmalen aus Rohdaten zu verbessern und mit diesen den Lernprozess zu vereinfachen. Mithilfe des [Import Data-Moduls](../machine-learning/studio/import-data.md) können Sie HiveQL-Abfragen von Azure ML ausführen und auf in Hive verarbeitete und in Blob Storage gespeicherte Daten zugreifen.

## <a name="microsoft-cognitive-toolkit"></a>Microsoft Cognitive Toolkit

[Deep Learning](https://www.microsoft.com/en-us/research/group/dltc/) ist ein Nebenzweig des maschinellen Lernens, der neuronale Netze verwendet, die an die biologischen Prozesse des menschlichen Gehirns angelehnt sind. Viele Forscher sehen Deep Learning als vielversprechenden Ansatz zur Weiterentwicklung der künstlichen Intelligenz. Beispiele für Deep Learning sind Übersetzungssysteme für gesprochene Sprache, Bilderkennungssysteme und Machine Reasoning (maschinelles logisches Schlussfolgern).

Um seine eigenen Bestrebungen im Deep Learning voranzutreiben, hat Microsoft das kostenlose und bedienfreundliche Open-Source-[Microsoft Cognitive Toolkit](https://www.microsoft.com/en-us/cognitive-toolkit/) entwickelt. Dieses Toolkit wird von einer Vielzahl verschiedener Microsoft-Produkte sowie von Firmen weltweit verwendet, die Deep Learning skaliert bereitstellen müssen, sowie von Studenten, die an den neuesten Algorithmen und Methoden interessiert sind. 

## <a name="see-also"></a>Weitere Informationen

### <a name="scenarios"></a>Szenarien

* [Spark mit Machine Learning: Analysieren von Gebäudetemperaturen mithilfe von Spark in HDInsight und HVAC-Daten](spark/apache-spark-ipython-notebook-machine-learning.md)
* [Spark mit Machine Learning: Vorhersage von Lebensmittelkontrollergebnissen mithilfe von Spark in HDInsight](spark/apache-spark-machine-learning-mllib-ipython.md)
* [Erstellen von Filmempfehlungen mit Mahout](hadoop/apache-hadoop-mahout-linux-mac.md)
* [Hive und Azure Machine Learning](../machine-learning/team-data-science-process/create-features-hive.md)
* [Hive und Azure Machine Learning End-to-End](../machine-learning/team-data-science-process/hive-walkthrough.md)
* [Machine Learning mit Spark in HDInsight](../machine-learning/team-data-science-process/spark-overview.md)

### <a name="deep-learning-resources"></a>Deep Learning-Ressourcen

* [Deep Learning Toolkit with Spark](https://blogs.technet.microsoft.com/machinelearning/2017/04/25/using-microsofts-deep-learning-toolkit-with-spark-on-azure-hdinsight-clusters/) (Verwenden des Deep Learning-Toolkits mit Spark)
* [Embarrassingly Parallel Image Classification, Using Cognitive Toolkit and TensorFlow on Azure HDInsight Spark](https://blogs.technet.microsoft.com/machinelearning/2017/04/12/embarrassingly-parallel-image-classification-using-cognitive-toolkit-tensorflow-on-azure-hdinsight-spark/) (Verblüffend parallele Bildklassifikation mit Cognitive Toolkit und TensorFlow in Azure HDInsight Spark)
