---
title: Log Analytics für Apache Kafka – Azure HDInsight
description: Erfahren Sie, wie Sie mithilfe von Log Analytics Protokolle aus Apache Kafka-Clustern in Azure HDInsight analysieren.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/15/2018
ms.openlocfilehash: 809196411df56c7448a9d0f983c17c60d5d3b4ae
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2018
ms.locfileid: "51015416"
---
# <a name="analyze-logs-for-apache-kafka-on-hdinsight"></a>Analysieren von Protokollen für Apache Kafka in HDInsight

Erfahren Sie, wie Sie mithilfe von Log Analytics in Apache Kafka generierte Protokolle in HDInsight analysieren.

## <a name="enable-log-analytics-for-kafka"></a>Aktivieren von Log Analytics für Kafka

Die Schritte zum Aktivieren von Log Analytics für HDInsight sind für alle HDInsight-Cluster identisch. Verwenden Sie die folgenden Links, um zu verstehen, wie die erforderlichen Dienste erstellt und konfiguriert werden:

1. Erstellen Sie einen Log Analytics-Arbeitsbereich. Weitere Informationen finden Sie im Dokument [Erste Schritte mit einem Log Analytics-Arbeitsbereich](https://docs.microsoft.com/azure/log-analytics).

2. Erstellen Sie ein Kafka-Cluster im HDInsight-Cluster. Weitere Informationen finden Sie unter [Einstieg in Apache Kafka in HDInsight](apache-kafka-get-started.md).

3. Konfigurieren Sie den Kafka-Cluster für die Verwendung von Log Analytics. Weitere Informationen finden Sie im Dokument [Überwachen von HDInsight mit Log Analytics](../hdinsight-hadoop-oms-log-analytics-tutorial.md).

    > [!NOTE]
    > Sie können auch den Cluster konfigurieren, um Log Analytics mit dem Cmdlet `Enable-AzureRmHDInsightOperationsManagementSuite` zu verwenden. Für dieses Cmdlet benötigen Sie die folgenden Informationen:
    >
    > * Den HDInsight-Clusternamen
    > * Die Arbeitsbereichs-ID für Log Analytics. Die Arbeitsbereichs-ID finden Sie in Ihrem Log Analytics-Arbeitsbereich.
    > * Den Primärschlüssel für die Log Analytics-Verbindung. Um den Primärschlüssel zu suchen, öffnen Sie den Arbeitsbereich im Azure-Portal, und wählen Sie im Menü auf der linken Seite __Erweiterte Einstellungen__ aus. Wählen Sie aus den erweiterten Einstellungen __Verbundene Quellen__>__Linux-Server__ aus.


> [!IMPORTANT]
> Es dauert ca. 20 Minuten, bis die Daten für Log Analytics zur Verfügung stehen.

## <a name="query-logs"></a>Abfrageprotokolle

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) Ihren Log Analytics-Arbeitsbereich aus.

2. Wählen Sie __Protokollsuche__ aus. In dieser Ansicht können Sie die von Kafka gesammelten Daten suchen. Nachfolgend sehen Sie einige Beispielsuchen:

    * Datenträgerauslastung: `Perf | where ObjectName == "Logical Disk" and CounterName == "Free Megabytes" and InstanceName == "_Total" and ((Computer startswith_cs "hn" and Computer contains_cs "-") or (Computer startswith_cs "wn" and Computer contains_cs "-")) | summarize AggregatedValue = avg(CounterValue) by Computer, bin(TimeGenerated, 1h)`

    * CPU-Auslastung: `Perf | where CounterName == "% Processor Time" and InstanceName == "_Total" and ((Computer startswith_cs "hn" and Computer contains_cs "-") or (Computer startswith_cs "wn" and Computer contains_cs "-")) | summarize AggregatedValue = avg(CounterValue) by Computer, bin(TimeGenerated, 1h)`

    * Eingehende Nachrichten pro Sekunde: `metrics_kafka_CL | where ClusterName_s == "your_kafka_cluster_name" and InstanceName_s == "kafka-BrokerTopicMetrics-MessagesInPerSec-Count" | summarize AggregatedValue = avg(kafka_BrokerTopicMetrics_MessagesInPerSec_Count_value_d) by HostName_s, bin(TimeGenerated, 1h)`

    * Eingehende Bytes pro Sekunde: `metrics_kafka_CL | where HostName_s == "wn0-kafka" and InstanceName_s == "kafka-BrokerTopicMetrics-BytesInPerSec-Count" | summarize AggregatedValue = avg(kafka_BrokerTopicMetrics_BytesInPerSec_Count_value_d) by bin(TimeGenerated, 1h)`

    * Ausgehende Bytes pro Sekunde: `metrics_kafka_CL | where ClusterName_s == "your_kafka_cluster_name" and InstanceName_s == "kafka-BrokerTopicMetrics-BytesOutPerSec-Count" | summarize AggregatedValue = avg(kafka_BrokerTopicMetrics_BytesOutPerSec_Count_value_d) by bin(TimeGenerated, 1h)`

    > [!IMPORTANT]
    > Ersetzen Sie die Abfragewerte durch die jeweiligen Informationen Ihres Clusters. Beispielsweise muss `ClusterName_s` auf den Namen Ihres Clusters festgelegt werden. `HostName_s` muss auf den Domänennamen eines Workerknotens im Cluster festgelegt werden.

    Sie können auch `*` eingeben, um alle protokollierte Typen zu suchen. Derzeit sind folgende Protokolle für Abfragen verfügbar:

    | Protokolltyp | BESCHREIBUNG |
    | ---- | ---- |
    | log\_kafkaserver\_CL | server.log des Kafka-Brokers |
    | log\_kafkacontroller\_CL | controller.log des Kafka-Brokers |
    | metrics\_kafka\_CL | Kafka-JMX-Metriken |

    ![Abbildung der Suche nach der CPU-Auslastung](./media/apache-kafka-log-analytics-operations-management/kafka-cpu-usage.png)
 
 ## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Log Analytics finden Sie im Dokument [Erste Schritte mit einem Log Analytics-Arbeitsbereich](../../log-analytics/log-analytics-get-started.md).

Weitere Informationen zum Arbeiten mit Kafka finden Sie in den folgenden Dokumenten:

 * [Spiegeln von Kafka zwischen HDInsight-Clustern](apache-kafka-mirroring.md)
 * [Erhöhen der Skalierbarkeit von Kafka in HDInsight](apache-kafka-scalability.md)
 * [Verwenden von Spark-Streaming (DStreams) mit Kafka](../hdinsight-apache-spark-with-kafka.md)
 * [Verwenden von strukturiertem Spark-Streaming mit Kafka](../hdinsight-apache-kafka-spark-structured-streaming.md)
