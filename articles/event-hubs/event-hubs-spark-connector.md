---
title: Integrieren von Apache Spark in Azure Event Hubs | Microsoft-Dokumentation
description: Integrieren von Apache Spark für strukturiertes Streaming mit Event Hubs
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.service: event-hubs
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/05/2018
ms.author: shvija
ms.openlocfilehash: 2dcf390b80c119ab21948b982c0812395e45bc01
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="integrating-apache-spark-with-azure-event-hubs"></a>Integrieren von Apache Spark in Azure Event Hubs

Azure Event Hubs lässt sich nahtlos in [Apache Spark](https://spark.apache.org/) integrieren, um die Erstellung von _End-to-End-Anwendungen_ für verteiltes Streaming zu vereinfachen. Diese Integration unterstützt [Spark Core](http://spark.apache.org/docs/latest/rdd-programming-guide.html), [Spark Streaming](http://spark.apache.org/docs/latest/streaming-programming-guide.html) und [Structured Streaming](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html). Der Event Hubs-Connector für Apache Spark ist auf [GitHub](https://github.com/Azure/azure-event-hubs-spark) verfügbar. Diese Bibliothek steht zudem für die Verwendung in Maven-Projekten über das [zentrale Maven-Repository](http://search.maven.org/#artifactdetails%7Ccom.microsoft.azure%7Cazure-eventhubs-spark_2.11%7C2.1.6%7C) bereit.

In diesem Artikel erfahren Sie, wie Sie eine fortlaufende Anwendung in [Azure Databrick](https://azure.microsoft.com/services/databricks/) erstellen. Wenngleich in diesem Artikel auf [Azure Databricks](https://azure.microsoft.com/services/databricks/) eingegangen wird, so stehen auch Spark-Cluster in [HDInsight](https://docs.microsoft.com/azure/hdinsight/spark/apache-spark-overview) zur Verfügung.

Im folgenden Beispiel werden zwei Scala-Notebooks verwendet, eines für das Streaming von Ereignissen von einem Event Hub und eines für das Zurücksenden von Ereignissen.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie noch keins haben, [erstellen Sie ein kostenloses Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
* Eine Event Hubs-Instanz. Wenn Sie noch keine haben, [erstellen Sie eine](https://docs.microsoft.com/azure/event-hubs/event-hubs-create).
* Eine [Azure Databricks](https://azure.microsoft.com/services/databricks/)-Instanz. Wenn Sie noch keine haben, [erstellen Sie eine](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal).
* [Erstellen Sie eine Bibliothek mithilfe der Maven-Koordinate](https://docs.databricks.com/user-guide/libraries.html#upload-a-maven-package-or-spark-package): `com.microsoft.azure:azure‐eventhubs‐spark_2.11:2.3.0`

Sie können den folgenden Codeausschnitt in Ihrem Notebook verwenden, um Ereignisse aus der Event Hubs-Instanz zu streamen.

```scala
import org.apache.spark.eventhubs._

// To connect to an Event Hub, EntityPath is required as part of the connection string.
// Here, we assume that the connection string from the Azure portal does not have the EntityPath part.
val connectionString = ConnectionStringBuilder("{EVENT HUB CONNECTION STRING FROM AZURE PORTAL}")
  .setEventHubName("{EVENT HUB NAME}")
  .build 
val ehConf = EventHubsConf(connectionString)
  .setStartingPosition(EventPosition.fromEndOfStream)

// Create a stream that reads data from the specified Event Hub.
val reader = spark.readStream
  .format("eventhubs")
  .options(ehConf.toMap)
  .load()

// Select the body column and cast it to a string.
val eventhubs = reader
  .select("CAST (body AS STRING)")
  .as[String]
```
Der folgende Beispielcodeausschnitt sendet Ereignisse mit den Spark-Batch-APIs an Ihre Event Hubs-Instanz. Sie können auch eine Streamingabfrage schreiben, um Ereignisse an die Event Hubs-Instanz zu senden.

```scala
import org.apache.spark.eventhubs._
import org.apache.spark.sql.functions._

// To connect to an Event Hub, EntityPath is required as part of the connection string.
// Here, we assume that the connection string from the Azure portal does not have the EntityPath part.
val connectionString = ConnectionStringBuilder("{EVENT HUB CONNECTION STRING FROM AZURE PORTAL}")
  .setEventHubName("{EVENT HUB NAME}")
  .build

val eventHubsConf = EventHubsConf(connectionString)

// Create a column representing the partitionKey.
val partitionKeyColumn = (col("id") % 5).cast("string").as("partitionKey")
// Create random strings as the body of the message.
val bodyColumn = concat(lit("random nunmber: "), rand()).as("body")

// Write 200 rows to the specified Event Hub.
val df = spark.range(200).select(partitionKeyColumn, bodyColumn)
df.write
  .format("eventhubs")
  .options(eventHubsConf.toMap)
  .save() 

```

Dieser Artikel bietet Ihnen einen Überblick über die Funktionsweise des Event Hubs-Connectors beim Erstellen fehlertoleranter Echtzeit-Streaminglösungen. Weitere Informationen zum strukturierten Streaming und Event Hubs-Connector finden Sie in den nächsten Schritten.

## <a name="next-steps"></a>Nächste Schritte

* [Structured Streaming + Azure Event Hubs Integration Guide](https://github.com/Azure/azure-event-hubs-spark/blob/master/docs/structured-streaming-eventhubs-integration.md) (Integrationsleitfaden zu strukturiertem Streaming und Azure Event Hubs)
* [Spark Streaming + Event Hubs Integration Guide](https://github.com/Azure/azure-event-hubs-spark/blob/master/docs/spark-streaming-eventhubs-integration.md) (Integrationsleitfaden zu Spark-Streaming und Event Hubs)
