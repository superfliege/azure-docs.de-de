---
title: Verwenden von externen Metadatenspeichern – Azure HDInsight
description: Verwenden von externen Metadatenspeichern für HDInsight-Cluster
services: hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.author: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/14/2018
ms.openlocfilehash: a2c992a47e40a4f8764f5950c65bb90f1cd9e066
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "43045142"
---
# <a name="use-external-metadata-stores-in-azure-hdinsight"></a>Verwenden von externen Metadatenspeichern in Azure HDInsight

Der Hive-Metastore in HDInsight ist ein wesentlicher Bestandteil der Hadoop-Architektur. Ein Metastore ist das zentrale Schemarepository, das von anderen Tools für den Zugriff auf Big Data wie Spark, Interactive Query (LLAP), Presto oder Pig verwendet werden kann. HDInsight verwendet eine Azure SQL-Datenbank als Hive-Metastore.

![Architektur des Hive-Metadatenspeichers in HDInsight](./media/hdinsight-use-external-metadata-stores/metadata-store-architecture.png)

Sie können einen Metastore für Ihre HDInsight-Cluster auf zwei Arten einrichten:

* [Standardmetastore](#default-metastore)
* [Benutzerdefinierter Metastore](#custom-metastore)

## <a name="default-metastore"></a>Standardmetastore

HDInsight stellt standardmäßig einen Metastore für jeden Clustertyp bereit. Sie können stattdessen einen benutzerdefinierten Metastore angeben. Beim Standardmetastore sind folgende Aspekte zu berücksichtigen:
- Keine zusätzlichen Kosten. HDInsight stellt für jeden Clustertyp einen Metastore ohne zusätzliche Kosten bereit.
- Jeder Standardmetastore ist Teil des Clusterlebenszyklus. Wenn Sie einen Cluster löschen, werden der jeweilige Metastore und die Metadaten ebenfalls gelöscht.
- Der Standardmetastore kann nicht für andere Cluster freigegeben werden.
- Der Standardmetastore verwendet die Azure SQL-Basisdatenbank, die auf 5 DTUs (Datenbankübertragungseinheiten) begrenzt ist.
Dieser Standardmetastore wird normalerweise für relativ einfache Arbeitslasten verwendet, für die weder mehrere Cluster noch eine Beibehaltung von Metadaten über den Lebenszyklus des Clusters hinaus erforderlich sind.


## <a name="custom-metastore"></a>Benutzerdefinierter Metastore

HDInsight unterstützt auch benutzerdefinierte Metastores, der für Produktionscluster empfohlen werden:
- Sie geben Ihre eigene Azure SQL-Datenbank als Metastore an.
- Der Lebenszyklus des Metastore ist nicht an den Lebenszyklus eines Clusters gebunden, sodass Sie Cluster ohne Verlust von Metadaten erstellen und löschen können. Metadaten, wie z. B. Ihre Hive-Schemas, bleiben auch nach dem Löschen und erneuten Erstellen des HDInsight-Clusters erhalten.
- Ein benutzerdefinierter Metastore ermöglicht das Anfügen mehrerer Cluster und Clustertypen an diesen Metastore. Beispielsweise kann ein einzelner Metastore für Interactive Query-, Hive- und Spark-Cluster in HDInsight freigegeben werden.
- Die Kosten für einen Metastore (Azure SQL-Datenbank) richten sich nach der von Ihnen ausgewählten Leistungsstufe.
- Sie können den Metastore nach Bedarf zentral hochskalieren.


![Anwendungsfall für den Hive-Metadatenspeicher in HDInsight](./media/hdinsight-use-external-metadata-stores/metadata-store-use-case.png)

<!-- Image – Typical shared custom Metastore scenario in HDInsight (?) -->



### <a name="select-a-custom-metastore-during-cluster-creation"></a>Auswählen eines benutzerdefinierten Metastore während der Clustererstellung

Sie können den Cluster während der Erstellung auf eine zuvor erstellte Azure SQL-Datenbank verweisen, oder Sie können die SQL-Datenbank nach der Erstellung des Clusters konfigurieren. Diese Option wird unter „Speicher“ > „Metastore-Einstellungen“ beim Erstellen eines neuen Hadoop-, Spark- oder Interactive Hive-Clusters über das Azure-Portal angegeben.

![Azure-Portal für Hive-Metadatenspeicher in HDInsight](./media/hdinsight-use-external-metadata-stores/metadata-store-azure-portal.png)

Sie können über das Azure-Portal oder die Ambari-Konfigurationen („Hive“ > „Erweitert“) auch zusätzliche Cluster zu einem benutzerdefinierten Metastore hinzufügen.

![Ambari für Hive-Metadatenspeicher in HDInsight](./media/hdinsight-use-external-metadata-stores/metadata-store-ambari.png)

## <a name="hive-metastore-best-practices"></a>Bewährte Methoden für den Hive-Metastore

Es folgen einige allgemeine bewährte Methoden für den Hive-Metastore in HDInsight:

- Verwenden Sie wann immer möglich einen benutzerdefinierten Metastore, da dieser das Trennen von Computeressourcen (Ihre ausgeführten Cluster) und Metadaten (im Metastore gespeichert) erleichtert.
- Beginnen Sie mit einem S2-Tarif, der 50 DTUs und 250 GB Speicher bietet. Wenn Sie einen Engpass feststellen, können Sie die Datenbank zentral hochskalieren.
- Stellen Sie sicher, dass der Metastore, der für eine HDInsight-Clusterversion erstellt wurde, nicht über verschiedene HDInsight-Clusterversionen freigegeben wird. Verschiedene Hive-Versionen verwenden unterschiedliche Schemas. Beispielsweise können Sie einen Metastore nicht für Hive 1.2- und Hive 2.1-Cluster freigeben.
- Sichern Sie Ihren benutzerdefinierten Metastore regelmäßig.
- Belassen Sie den Metastore und den HDInsight-Cluster in derselben Region.
- Überwachen Sie den Metastore in Hinsicht auf Leistung und Verfügbarkeit. Verwenden Sie dazu Überwachungstools für die Azure SQL-Datenbank, wie z. B. das Azure-Portal oder Azure Log Analytics.

## <a name="oozie-metastore"></a>Oozie-Metastore

Apache Oozie ist ein Koordinationssystem für Workflows zur Verwaltung von Hadoop-Aufträgen.  Oozie unterstützt Hadoop-Aufträge für Apache MapReduce, Pig, Hive und andere.  Oozie verwendet einen Metastore zum Speichern von Details zu aktuellen und abgeschlossenen Workflows. Zur Leistungssteigerung bei Verwendung von Oozie können Sie eine Azure SQL-Datenbank als benutzerdefinierten Metastore verwenden. Der Metastore kann auch Zugriff auf Oozie-Auftragsdaten bieten, nachdem Sie Ihren Cluster gelöscht haben.

Anleitungen zum Erstellen eines Oozie-Metastore mit einer Azure SQL-Datenbank finden Sie unter [Verwenden von Oozie für Workflows](hdinsight-use-oozie-linux-mac.md).

## <a name="next-steps"></a>Nächste Schritte

- [Einrichten von Clustern in HDInsight mit Hadoop, Spark, Kafka usw.](./hdinsight-hadoop-provision-linux-clusters.md)
