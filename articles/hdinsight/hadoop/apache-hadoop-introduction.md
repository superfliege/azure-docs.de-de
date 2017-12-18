---
title: "Einführung in HDInsight, den Hadoop- und Spark-Technologiestapel – Azure | Microsoft-Dokumentation"
description: "Eine Einführung in HDInsight, den Hadoop- und Spark-Technologiestapel und die dazugehörigen Komponenten wie Kafka, Hive und HBase für Big Data-Analysen."
keywords: "Azure Hadoop, Hadoop Azure, Hadoop-Intro, Hadoop-Einführung, Hadoop-Technologiestapel, Einführung in Hadoop, Einführung für Hadoop, was ist ein Hadoop-Cluster, was ist ein Hadoop-Cluster, wozu dient Hadoop"
services: hdinsight
documentationcenter: 
author: cjgronlund
manager: jhubbard
editor: cgronlun
ms.assetid: e56a396a-1b39-43e0-b543-f2dee5b8dd3a
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/13/2017
ms.author: cgronlun
ms.openlocfilehash: ba2a14e3f327fd8ab4f1ebfe329a64b4aa1a25a1
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2017
---
# <a name="introduction-to-azure-hdinsight-the-hadoop-and-spark-technology-stack"></a>Einführung in HDInsight, den Hadoop- und Spark-Technologiestapel
Dieser Artikel enthält eine Einführung in Azure HDInsight. Hierbei handelt es sich um einen umfassenden, vollständig verwalteten Open Source-Analysedienst für Unternehmen. Sie können Open Source-Frameworks wie Hadoop, Spark, Hive, LLAP, Kafka, Storm, R und andere verwenden. 

[!INCLUDE [hdinsight-price-change](../../../includes/hdinsight-enhancements.md)]

[Apache Hadoop](http://hadoop.apache.org/) war ursprünglich ein Open Source-Framework für die verteilte Verarbeitung und Analyse umfangreicher Datasets in Clustern. Der Hadoop-Technologiestapel umfasst verwandte Software und Hilfsprogramme, einschließlich Apache Hive, HBase, Spark, Kafka und viele andere. Informationen zu verfügbare Komponenten des Hadoop-Technologiestapels für HDInsight finden Sie unter [Welche Hadoop-Komponenten und -Versionen sind in HDInsight verfügbar?][component-versioning]. Weitere Informationen zu Hadoop in HDInsight finden Sie auf der Seite mit [Azure-Features für HDInsight](https://azure.microsoft.com/services/hdinsight/).

[Apache Spark](http://spark.apache.org) ist ein Open-Source-Framework für die Parallelverarbeitung, das die arbeitsspeicherinterne Verarbeitung unterstützt, um die Leistung von Anwendungen zur Analyse von großen Datenmengen zu steigern. Weitere Informationen zu Spark in HDInsight finden Sie unter [Einführung in Spark in HDInsight](../spark/apache-spark-overview.md). 

<a href="https://ms.portal.azure.com/#create/Microsoft.HDInsightCluster" target="_blank"><img src="./media/apache-hadoop-introduction/deploy-to-azure.png" alt="Deploy an Azure HDInsight cluster"></a>

## <a name="what-is-hdinsight-and-the-hadoop-technology-stack"></a>Was sind HDInsight und der Hadoop-Technologiestapel? 
Azure HDInsight ist eine Clouddistribution der Hadoop-Komponenten von [Hortonworks Data Platform (HDP)](https://hortonworks.com/products/data-center/hdp/).  Azure HDInsight ermöglicht die einfache, schnelle und kostengünstige Verarbeitung umfangreicher Datenmengen. Dank der Möglichkeit zur Verwendung gängiger Open Source-Frameworks wie Hadoop, Spark, Hive, LLAP, Kafka, Storm und R lassen sich verschiedenste Szenarien realisieren – vom Extrahieren, Transformieren und Laden (ETL) über Data Warehousing und Machine Learning bis hin zu IoT.

## <a name="what-is-big-data"></a>Was versteht man unter "Big Data"?

Big Data werden in immer größeren Mengen, mit immer höherer Geschwindigkeit und in immer mehr Formaten erfasst. Dabei kann es sich um Verlaufsdaten (also gespeicherte Daten) oder um Echtzeitdaten (von der Quelle gestreamt) handeln. Informationen zu den gängigsten Anwendungsfällen für Big Data finden Sie unter [Verwendungsszenarien für HDInsight](#scenarios-for-using-hdinsight).

## <a name="why-should-i-use-hdinsight"></a>Argumente für die Verwendung von HDInsight

In diesem Abschnitt werden die Funktionen von Azure HDInsight aufgeführt.


|Funktion  |Beschreibung  |
|---------|---------|
|Cloudbasiert     |     Mit Azure HDInsight können Sie optimierte Cluster für [Hadoop](apache-hadoop-linux-tutorial-get-started.md),  [Spark](../spark/apache-spark-jupyter-spark-sql.md),  [Interactive Query (LLAP)](../interactive-query/apache-interactive-query-get-started.md),  [Kafka](../kafka/apache-kafka-get-started.md),  [Storm](../storm/apache-storm-tutorial-get-started-linux.md),  [HBase](../hbase/apache-hbase-tutorial-get-started-linux.md) und  [R Server](../r-server/r-server-get-started.md) in Azure erstellen. Darüber hinaus bietet HDInsight eine End-to-End-SLA für alle Ihre Produktionsworkloads.  |
|Kostengünstig und skalierbar     | Mit HDInsight können Sie Workloads [zentral hoch- und herunterskalieren](../hdinsight-administer-use-portal-linux.md) . Durch  [Erstellen bedarfsgesteuerter Cluster](../hdinsight-hadoop-create-linux-clusters-adf.md)  können Sie Ihre Kosten senken, indem Sie nur für das bezahlen, was Sie tatsächlich nutzen. Außerdem können Sie Datenpipelines für die Operationalisierung Ihrer Aufträge erstellen. Die Entkoppelung von Compute und Speicher sorgt für bessere Leistung und mehr Flexibilität. |
|Sicher und konform    | Mit HDInsight können Sie die Datenressourcen Ihres Unternehmens durch die Verwendung von [Azure Virtual Network](../hdinsight-extend-hadoop-virtual-network.md), [Verschlüsselung](../hdinsight-hadoop-create-linux-clusters-with-secure-transfer-storage.md) und Integration von [Azure Active Directory](../domain-joined/apache-domain-joined-introduction.md) schützen. Darüber hinaus erfüllt HDInsight die gängigsten branchen- und behördenspezifischen [Compliancestandards](https://azure.microsoft.com/overview/trusted-cloud).        |
|Überwachung    | Dank [Azure Log Analytics](../hdinsight-hadoop-oms-log-analytics-tutorial.md)-Integration bietet Azure HDInsight eine zentrale Oberfläche für die Überwachung sämtlicher Cluster.        |
|Globale Verfügbarkeit | HDInsight ist in mehr  [Regionen](https://azure.microsoft.com/regions/services/)  verfügbar als jede andere Big Data-Analyselösung. Zudem steht Azure HDInsight in Azure Government, China und Deutschland zur Verfügung, was die Erfüllung geschäftlicher Anforderungen in zentralen unabhängigen Bereichen ermöglicht. |  
|Produktivität     |  Mit Azure HDInsight können Sie umfangreiche Produktivitätstools für Hadoop und Spark in Ihrer bevorzugten Entwicklungsumgebung nutzen – etwa in [Visual Studio](apache-hadoop-visual-studio-tools-get-started.md), [Eclipse](../spark/apache-spark-eclipse-tool-plugin.md) und [IntelliJ](../spark/apache-spark-intellij-tool-plugin.md) für Scala, Python, R, Java, und .NET. Datenanalysten können zudem über gängige Notebooks wie [Jupyter](../spark/apache-spark-jupyter-notebook-kernels.md) und [Zeppelin](../spark/apache-spark-zeppelin-notebook.md) zusammenarbeiten.    |
|Erweiterbarkeit     |  Die HDInsight-Cluster können durch Installieren von Komponenten (beispielsweise Hue und Presto), durch Verwenden von [Skriptaktionen](../hdinsight-hadoop-customize-cluster-linux.md), durch [Hinzufügen von Edgeknoten](../hdinsight-apps-use-edge-node.md) sowie durch [Integrieren anderer, für Big Data zertifizierter Anwendungen](../hdinsight-apps-install-applications.md) erweitert werden. HDInsight lässt sich mittels [One-Click-Bereitstellung](https://azure.microsoft.com/services/hdinsight/partner-ecosystem/) nahtlos in die gängigsten Big Data-Lösungen integrieren.|

## <a name="scenarios-for-using-hdinsight"></a>Verwendungsszenarien für HDInsight

Azure HDInsight kann im Rahmen verschiedenster Anwendungsfälle für die Big Data-Verarbeitung verwendet werden. Big Data werden in immer größeren Mengen, mit immer höherer Geschwindigkeit und in immer mehr Formaten erfasst. Dabei kann es sich um historische Daten (Daten, die bereits erfasst und gespeichert wurden) oder um Echtzeitdaten (Daten, die direkt von der Quelle gestreamt werden) handeln. Die Anwendungsfälle für die Verarbeitung dieser Daten lassen sich in folgende Kategorien unterteilen: 

### <a name="batch-processing-etl"></a>Batchverarbeitung (ETL)

Beim Extrahieren, Transformieren und Laden (ETL) werden unstrukturierte oder strukturierte Daten aus heterogenen Datenquellen extrahiert, in ein strukturiertes Format transformiert und in einen Datenspeicher geladen. Die transformierten Daten können für Data Science oder Data Warehousing verwendet werden.

### <a name="internet-of-things-iot"></a>Internet der Dinge (IoT, Internet of Things)

Mit HDInsight können Sie in Echtzeit eingehende Streamingdaten verschiedenster Geräte verarbeiten. Weitere Informationen finden Sie in [diesem Blog](https://azure.microsoft.com/blog/announcing-public-preview-of-apache-kafka-on-hdinsight-with-azure-managed-disks/).

![HDInsight-Architektur: Internet der Dinge](./media/apache-hadoop-introduction/hdinsight-architecture-iot.png) 

### <a name="data-science"></a>Data Science

Mit HDInsight können Sie Anwendungen erstellen, die wichtige Erkenntnisse aus Daten extrahieren. Zusätzlich können Sie mithilfe von Azure Machine Learning zukünftige Trends für Ihr Unternehmen prognostizieren. Weitere Informationen finden Sie in [diesem Kundenbericht](https://customers.microsoft.com/story/pros).

![HDInsight-Architektur: Data Science](./media/apache-hadoop-introduction/hdinsight-architecture-data-science.png)

### <a name="data-warehousing"></a>Data Warehousing

Mit HDInsight können Sie interaktive Abfragen für Petabytes von strukturierten oder unstrukturierten Daten in einem beliebigen Format durchführen. Darüber hinaus können Sie Modelle für die Verknüpfung mit BI-Tools erstellen. Weitere Informationen finden Sie in [diesem Kundenbericht](https://customers.microsoft.com/story/milliman). 

![HDInsight-Architektur: Data Warehousing](./media/apache-hadoop-introduction/hdinsight-architecture-data-warehouse.png)

### <a name="hybrid"></a>Hybrid

Mit HDInsight können Sie Ihre vorhandene lokale Big Data-Infrastruktur auf Azure ausdehnen und von den erweiterten Analysefunktionen der Cloud profitieren.

![HDInsight-Architektur: Hybrid](./media/apache-hadoop-introduction/hdinsight-architecture-hybrid.png)

## <a name="overview"></a>Clustertypen in HDInsight
HDInsight umfasst bestimmte Clustertypen und Clusteranpassungsfunktionen (etwa das Hinzufügen von Komponenten, Hilfsprogrammen und Sprachen).

### <a name="spark-kafka-interactive-query-hbase-customized-and-other-cluster-types"></a>Clustertypen Spark, Kafka, Interactive Query, HBase, angepasste Cluster und andere
HDInsight bietet die folgenden Clustertypen:

* **[Apache Hadoop](https://wiki.apache.org/hadoop)**: Nutzt [HDFS](#hdfs), die [YARN](#yarn)-Ressourcenverwaltung und ein einfaches [MapReduce](#mapreduce)-Programmiermodell zum parallelen Verarbeiten und Analysieren von Batchdaten.
* **[Apache Spark](http://spark.apache.org/)**: Apache Spark ist ein Open-Source-Framework für die Parallelverarbeitung, das die arbeitsspeicherinterne Verarbeitung unterstützt, um die Leistung von Anwendungen zur Analyse von großen Datenmengen zu steigern. Spark funktioniert mit SQL, Datenströmen und Machine Learning. Siehe [Was ist Apache Spark in HDInsight?](../spark/apache-spark-overview.md)
* **[Apache HBase](http://hbase.apache.org/)**: Eine auf Hadoop basierende NoSQL-Datenbank, die wahlfreien Zugriff und starke Konsistenz für große Mengen unstrukturierter und teilstrukturierter Daten bietet – in einer potenziellen Dimension von Milliarden von Zeilen multipliziert mit Milliarden von Spalten. Siehe [Was ist HBase in HDInsight?](../hbase/apache-hbase-overview.md)
* **[Microsoft R Server](https://msdn.microsoft.com/microsoft-r/rserver)**: Ein Server zum Hosten und Verwalten von parallelen, verteilten R-Prozessen. Diese Funktion ermöglicht Datenanalysten, Statistikern und R-Programmierern bei Bedarf den Zugriff auf skalierbare, verteilte Analysemethoden in HDInsight. Siehe [Übersicht über R Server in HDInsight](../r-server/r-server-overview.md).
* **[Apache Storm](https://storm.incubator.apache.org/)**: Ein verteiltes Echtzeitberechnungssystem für die schnelle Verarbeitung großer Datenströme. Storm wird als verwalteter Cluster in HDInsight angeboten. Siehe [Analysieren von Echtzeit-Sensordaten mit Storm und Hadoop](../storm/apache-storm-sensor-data-analysis.md).
* **[Apache Interactive Query-Vorschau (auch bekannt als „Live Long and Process“, LLAP)](https://cwiki.apache.org/confluence/display/Hive/LLAP)**: In-Memory-Zwischenspeicherung für interaktive und schnellere Hive-Abfragen. Siehe [Use Interactive Query in HDInsight](../interactive-query/apache-interactive-query-get-started.md) (Verwenden von Interactive Query in HDInsight).
* **[Apache Kafka](https://kafka.apache.org/)**: Eine Open Source-Plattform zum Erstellen von Streamingdatenpipelines und -anwendungen. Kafka bietet auch eine Nachrichtenwarteschlangenfunktion, die Ihnen das Veröffentlichen und Abonnieren von Datenströmen ermöglicht. Siehe [Introduction to Apache Kafka on HDInsight](../kafka/apache-kafka-introduction.md) (Einführung in Apache Kafka in HDInsight).

## <a name="open-source-components-in-hdinsight"></a>Open Source-Komponenten in HDInsight

Azure HDInsight ermöglicht die Erstellung von Clustern mit Open Source-Frameworks wie Hadoop, Spark, Hive, LLAP, Kafka, Storm, HBase und R. Diese Cluster verfügen standardmäßig über weitere Open Source-Komponenten wie [Ambari](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md), [Avro](http://avro.apache.org/docs/current/spec.html), [Hive](http://hive.apache.org), [HCatalog](https://cwiki.apache.org/confluence/display/Hive/HCatalog/), [Mahout](https://mahout.apache.org/), [MapReduce](http://wiki.apache.org/hadoop/MapReduce), [YARN](http://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html), [Phoenix](http://phoenix.apache.org/), [Pig](http://pig.apache.org/), [Sqoop](http://sqoop.apache.org/), [Tez](http://tez.apache.org/), [Oozie](http://oozie.apache.org/) und [ZooKeeper](http://zookeeper.apache.org/).  


## <a name="programming-languages-on-hdinsight"></a>Programmiersprachen in HDInsight
HDInsight-Cluster – Spark, HBase Kafka, Hadoop und andere Cluster – unterstützen viele Programmiersprache, aber einige werden nicht standardmäßig installiert. [Verwenden Sie eine Skriptaktion](../hdinsight-hadoop-script-actions-linux.md), um Bibliotheken, Module oder Pakete zu installieren, die standardmäßig nicht installiert sind. 

### <a name="default-programming-language-support"></a>Standardmäßige Unterstützung für Programmiersprachen
Standardmäßig unterstützen HDInsight-Cluster folgende Sprachen:

* Java
* Python

Zusätzliche Sprachen können mithilfe von [Skriptaktionen](../hdinsight-hadoop-script-actions-linux.md) installiert werden.

### <a name="java-virtual-machine-jvm-languages"></a>JVM-Sprachen (Java Virtual Machine)
Neben Java können viele weitere Sprachen mithilfe einer JVM (Java Virtual Machine) ausgeführt werden. Für einige dieser Sprachen müssen jedoch möglicherweise zusätzliche Komponenten im Cluster installiert werden.

Diese JVM-basierten Sprachen werden in HDInsight-Clustern unterstützt:

* Clojure
* Jython (Python für Java)
* Scala

### <a name="hadoop-specific-languages"></a>Hadoop-spezifische Sprachen
HDInsight-Cluster bieten Unterstützung für die folgenden Sprachen, die für den Hadoop-Technologiestapel spezifisch sind:

* Pig Latin für Pig-Aufträge
* HiveQL für Hive-Aufträge und SparkSQL

## <a name="business-intelligence-on-hdinsight"></a>Business Intelligence in HDInsight
Bekannte Business Intelligence-Tools (BI) rufen Daten, die in HDInsight integriert sind, entweder über das Add-In Power Query oder den Microsoft Hive-ODBC-Treiber ab, analysieren sie und erstellen Berichte:

* [Apache Spark BI mit Datenvisualisierungstools unter Azure HDInsight](../spark/apache-spark-use-bi-tools.md)
* [Visualisieren von Hive-Daten mit Microsoft Power BI in Azure HDInsight](apache-hadoop-connect-hive-power-bi.md) 
* [Verbinden von Excel mit Hadoop über Power Query](apache-hadoop-connect-excel-power-query.md): Erfahren Sie, wie Sie Excel mithilfe von Microsoft Power Query für Excel mit dem Azure-Speicherkonto verbinden, in dem die Daten aus Ihrem HDInsight-Cluster gespeichert werden. Windows-Arbeitsstation ist erforderlich. 
* [Verbinden von Excel über den Microsoft Hive-ODBC-Treiber mit Hadoop](apache-hadoop-connect-excel-hive-odbc-driver.md): Informationen zum Importieren von Daten aus HDInsight mit dem Microsoft Hive-ODBC-Treiber. Windows-Arbeitsstation ist erforderlich. 
* [Microsoft-Cloudplattform](http://www.microsoft.com/server-cloud/solutions/business-intelligence/default.aspx): Entdecken Sie Power BI für Office 365, laden Sie die Testversion von SQL Server herunter, und richten Sie SharePoint Server 2013 und SQL Server BI ein.
* [SQL Server Analysis Services](http://msdn.microsoft.com/library/hh231701.aspx)
* [SQL Server Reporting Services](http://msdn.microsoft.com/library/ms159106.aspx)


## <a name="next-steps"></a>Nächste Schritte

* [Hadoop-Tutorial: Erste Schritte bei der Verwendung von Hadoop in HDInsight](apache-hadoop-linux-tutorial-get-started.md)
* [Erstellen eines Apache Spark-Clusters in Azure HDInsight](../spark/apache-spark-jupyter-spark-sql.md)
* [Erste Schritte mit Apache Kafka (Vorschau) in HDInsight](../kafka/apache-kafka-get-started.md)
* [Erste Schritte mit Apache Storm in HDInsight mithilfe der Storm-Starter-Beispiele](../storm/apache-storm-tutorial-get-started-linux.md)
* [Erste Schritte mit HBase in HDInsight](../hbase/apache-hbase-tutorial-get-started-linux.md)
* [Verwenden von Interactive Query mit HDInsight](../interactive-query/apache-interactive-query-get-started.md)
* [Erste Schritte mit R Server in HDInsight](../r-server/r-server-get-started.md)
* [Verwalten von Hadoop-Clustern in HDInsight mit dem Azure-Portal](../hdinsight-administer-use-portal-linux.md)
* [Einführung in die Hadoop-Sicherheit mit in die Domäne eingebundenen HDInsight-Clustern](../domain-joined/apache-domain-joined-introduction.md)
* [Verwenden von Azure Log Analytics zum Überwachen von HDInsight-Clustern](../hdinsight-hadoop-oms-log-analytics-tutorial.md)


[component-versioning]: ../hdinsight-component-versioning.md
[zookeeper]: http://zookeeper.apache.org/