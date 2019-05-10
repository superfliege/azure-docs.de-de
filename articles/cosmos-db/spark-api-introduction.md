---
title: Einführung in die Spark-API von Azure Cosmos DB
description: Erfahren Sie, wie Sie die Azure Cosmos DB-Spark-API zum Ausführen von operativer Analyse und KI verwenden können
ms.service: cosmos-db
ms.topic: overview
ms.date: 05/06/2019
author: rimman
ms.author: rimman
ms.openlocfilehash: de920f40f2968942b7ac66414170b43bd9317cfb
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65080018"
---
# <a name="introduction-to-the-azure-cosmos-db-spark-api-preview"></a>Einführung in die Azure Cosmos DB-Spark-API (Vorschau) 

Die Spark-API in Azure Cosmos DB ermöglicht Ihnen das Ausführen von Analysen für Ihre in einem Azure Cosmos-Konto gespeicherten Daten aus Apache Spark.

Die Spark-API in Azure Cosmos DB stellt die native Unterstützung für die direkte Ausführung von Apache Spark-Aufträgen in Ihren global verteilten Cosmos-Datenbanken zur Verfügung. Mit diesen Funktionen können Entwickler, Data Engineers und Data Scientists Azure Cosmos DB als flexible, skalierbare und leistungsstarke Datenplattform zum Ausführen von **OLTP- und OLAP/HTAP**-Workloads verwenden. 

> [!NOTE]
> Azure Cosmos DB Spark-API ist derzeit als eingeschränkte Vorschauversion verfügbar. Um sich für die Vorschauversion zu registrieren, navigieren Sie zur Seite [Für die Vorschau registrieren](https://aka.ms/cosmos-spark-preview). 

Die Spark-API in Azure Cosmos DB bietet die folgenden Vorteile:

* Sie können die kürzeste Zeitspanne bis zur Erkenntnis für die geografisch verteilten Benutzer und Daten erreichen.

* Sie können die Architektur Ihrer Lösung vereinfachen und die [Gesamtbetriebskosten](total-cost-ownership.md) (Total Cost of Ownership, TCO) senken. Das System weist die kleinste Anzahl an Datenverarbeitungskomponenten auf und vermeidet jede unnötige Verschiebung von Daten zwischen ihnen.

* Es erstellt eine [Sicherheits-](secure-access-to-data.md), [Compliance-](compliance.md) und Überwachungsgrenze, die sämtliche unter Verwaltung stehenden Daten umfasst.

* Es bietet "Always-On"- oder [hoch verfügbare](high-availability.md) Endbenutzeranalysen, die von strengen SLAs gestützt werden.

![Azure Cosmos DB-Spark-API-Visualisierung](./media/spark-api-introduction/spark-api-visualization.png)
 
Mithilfe der Azure Cosmos DB-Spark-API können Sie Lösungen wie KI- und Deep Learning-Modelle, Prognosen und Analysen, Empfehlungen, IoT, Kunden 360, Betrugserkennung, Textstimmungs- und Clickstreamanalyse erstellen und bereitstellen. Diese arbeiten direkt mit Ihren Azure Cosmos DB-Daten.

Sie können Batch- und Streaming-ETL-Aufträge in Azure Cosmos DB einrichten, ohne den Datenbankdienst verlassen oder zusätzliche Computedienste hinzufügen zu müssen. Sie können die Compute-Umgebung flexibel skalieren, wenn Sie einen ETL-Auftrag ausführen müssen, und die Skalierung wieder zurückfahren, wenn der Auftrag erledigt ist.

Die Spark-API in Azure Cosmos DB unterstützt die integrierte Machine Learning-Unterstützung in den Apache Spark-Runtimes. Die Runtime enthält Spark MLLib, Microsoft Machine Learning für Spark, Azure Machine Learning und Cognitive Services. Mit diesen Funktionen können Data Scientists, Data Engineers und Datenanalysten Modelle für maschinelles Lernen direkt innerhalb von Cosmos DB erstellen und betreiben, in einem Bruchteil der üblichen Zeit und zu geringen Kosten.


## <a name="key-benefits"></a>Hauptvorteile

### <a name="globally-distributed-low-latency-operational-analytics-and-ai"></a>Global verteilte operative Analyse und KI mit geringer Latenz

Mit Apache Spark in der global verteilten Azure Cosmos-Datenbank erhalten Sie jetzt weltweit mit geringer Wartezeit Erkenntnisse. Azure Cosmos DB ermöglicht **global verteilte operative Analyse mit geringer Latenz** und flexibler Skalierung dank dreier Schlüsseltechnologien:

* Da Ihre Azure Cosmos-Datenbank global verteilt ist, werden alle Daten lokal am Standort der Produzenten der Daten (beispielsweise Benutzer) erfasst. Die Abfragen werden anhand der lokalen Replikate verarbeitet, die sowohl für die Produzenten als auch für die Konsumenten der Daten am nächsten gelegen sind, unabhängig von ihrem Speicherort auf der Welt. 

* Ihre sämtlichen Analyseabfragen werden direkt auf den indizierten Daten ausgeführt, die innerhalb der Datenpartitionen gespeichert sind, ohne unnötige Datenverschiebungen. 

* Da Spark am gleichen Ort wie Azure Cosmos DB ausgeführt wird, sind weniger Zwischenübersetzungen und Datenverschiebungen erforderlich, was zu besserer Leistung und Skalierbarkeit führt.

### <a name="unified-serverless-experience-for-apache-spark"></a>Einheitliche serverlose Benutzererfahrung für Apache Spark

Als mehrere Modelle unterstützende Datenbank dehnt Azure Cosmos DB jetzt seine Unterstützung für OSS-APIs aus, indem es eine **einheitliche serverlose Benutzererfahrung für Apache Spark** mit Datenmodellen der Familien Schlüssel-Wert, Dokument, Diagramm und Spalte zur Verfügung stellt. Mithilfe von APIs für MongoDB, Cassandra, Gremlin, Etcd und SQL werden verschiedene Datenmodelle unterstützt – die alle mit den gleichen zugrundeliegenden Daten arbeiten. 

Mit der Spark-API können Sie Anwendungen, die in Scala, Python und Java geschrieben wurden, nativ unterstützen und verschiedene hoch integrierte Bibliotheken für SQL verwenden. Zu diesen Bibliotheken gehören ([Spark SQL](https://spark.apache.org/sql/)), maschinelles Lernen (Spark [MLlib](https://spark.apache.org/mllib/)), Datenstromverarbeitung ([Spark Structured Streaming](https://spark.apache.org/streaming/)) und Graphverarbeitung (Spark [GraphFrames]( https://docs.databricks.com/spark/latest/graph-analysis/graphframes/user-guide-python.html)). Diese Tools erleichtern die Nutzung der Spark-API für eine Vielzahl von Anwendungsfällen. Sie brauchen sich nicht mit der Verwaltung von Spark oder Spark-Clustern auseinanderzusetzen. Sie können die vertrauten Apache Spark-APIs und **Jupyter-Notebooks** für die Analyse und die SQL-API oder eine der OSS NoSQL-APIs wie Cassandra zur gleichzeitigen Transaktionsverarbeitung mit den gleichen zugrundeliegenden Daten verwenden.

### <a name="no-schema-or-index-management"></a>Keine Schema- oder Indexverwaltung

Im Gegensatz zu herkömmlichen Analysedatenbanken brauchen sich bei Azure Cosmos DB Data Engineers und Data Scientists nicht mehr mit der lästigen Schema- und Indexverwaltung abzugeben. Die Datenbank-Engine in Azure Cosmos DB erfordert keine explizite Schema- oder Indexverwaltung und kann automatisch alle von ihr erfassten Daten indizieren, um die Apache Spark-Abfragen schnell zu bedienen. 

### <a name="consistency-choices"></a>Konsistenzoptionen

Da die Apache Spark-Aufträge in den Datenpartitionen Ihrer Azure Cosmos-Datenbank ausgeführt werden, stehen für die Abfragen die [fünf klar definierte Konsistenzoptionen](consistency-levels.md) zur Verfügung. Diese Konsistenzmodelle verleihen die Flexibilität, strenge Konsistenz für Algorithmen für maschinelles Lernen auszuwählen, um die genauesten Ergebnisse zu erzielen, ohne Latenz und Hochverfügbarkeit zu beeinträchtigen. 

### <a name="slas"></a>SLAs

Die Apache Spark-Aufträge profitieren von den Azure Cosmos DB-Vorteilen, wie den branchenführenden umfassenden [SLAs](https://azure.microsoft.com/support/legal/sla/documentdb/v1_1/) (99,999) ohne jeglichen Mehraufwand für die Verwaltung separater Apache Spark-Cluster. Diese SLAs umfassen Durchsatz, Latenz im 99. Perzentil, Konsistenz und Hochverfügbarkeit. 

### <a name="mixed-workloads"></a>Gemischte Workloads

Durch die Integration von Apache Spark in Azure Cosmos DB wird die Trennung zwischen Transaktion und Analyse überbrückt, die für Kunden einen der größten neuralgischen Punkte beim Erstellen cloudnativer Anwendungen im globalen Maßstab darstellte. 

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu den Vorzügen von Azure Cosmos DB finden Sie im [Übersichtsartikel](introduction.md).
* [Erste Schritte mit der Azure Cosmos DB-API für MongoDB](mongodb-introduction.md)
* [Erste Schritte mit der Cassandra-API von Azure Cosmos DB](cassandra-introduction.md)
* [Erste Schritte mit der Gremlin-API von Azure Cosmos DB](graph-introduction.md)
* [Erste Schritte mit der Tabellen-API von Azure Cosmos DB](table-introduction.md)




