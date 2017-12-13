---
title: Verwenden Apache Spark Structured Streaming mit Event Hubs in Azure HDInsight | Microsoft-Dokumentation
description: Erstellen eines Apache Spark-Streamingbeispiels zum Senden eines Datenstroms an Azure Event Hub und zum Empfangen dieser Ereignisse in einem HDInsight Spark-Cluster mithilfe einer Scala-Anwendung
keywords: Apache Spark-Streaming, Spark-Streaming, Spark-Beispiel, Apache Spark-Streamingbeispiel, Event Hub-Azure-Beispiel, Spark-Beispiel
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: nitinme
ms.openlocfilehash: a542295e91a641289fa4261920a08eddbad6a217
ms.sourcegitcommit: be0d1aaed5c0bbd9224e2011165c5515bfa8306c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/01/2017
---
# <a name="apache-spark-structured-streaming-on-hdinsight-to-process-events-from-event-hubs"></a>Apache Spark Structured Streaming unter HDInsight zum Verarbeiten von Ereignissen von Event Hubs

In diesem Artikel erfahren Sie, wie Sie Echtzeittelemetrie mithilfe von Spark Structured Streaming verarbeiten. Hierzu müssen Sie die folgenden allgemeinen Schritte ausführen:

1. Kompilieren Sie auf Ihrer lokalen Arbeitsstation eine Beispielanwendung, die Ereignisse zum Senden an Event Hubs generiert (Ereignisherausgeber), und führen Sie sie aus.
2. Definieren Sie mit der [Spark-Shell](apache-spark-shell.md) eine einfache Spark Structured Streaming-Anwendung, und führen Sie sie aus.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Siehe [How to get Azure Free trial for testing Hadoop in HDInsight](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)(in englischer Sprache).

* Ein Apache Spark-Cluster unter HDInsight. Eine Anleitung finden Sie unter [Erstellen von Apache Spark-Clustern in Azure HDInsight](apache-spark-jupyter-spark-sql.md).

* Ein Azure Event Hubs-Namespace Weitere Informationen finden Sie unter [Erstellen eines Azure Event Hubs-Namespaces](apache-spark-eventhub-streaming.md#create-an-azure-event-hub).

* Oracle Java Development Kit. Das Installationspaket finden Sie [hier](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).

* Apache Maven. Sie können es [hier](https://maven.apache.org/download.cgi)herunterladen. Anweisungen zur Installation von Maven stehen [hier](https://maven.apache.org/install.html).

## <a name="build-configure-and-run-the-event-producer"></a>Erstellen, Konfigurieren und Ausführen des Ereignisherausgebers
In dieser Aufgabe klonen Sie eine Beispielanwendung, die zufällige Ereignisse erstellt und an einen konfigurierten Event Hub sendet. Diese Beispielanwendung steht bei GitHub unter [https://github.com/hdinsight/eventhubs-sample-event-producer](https://github.com/hdinsight/eventhubs-sample-event-producer) zur Verfügung.

1. Stellen Sie sicher, dass Sie die Git-Tools installiert haben. Sie können sie bei den [Git-Downloads](http://www.git-scm.com/downloads) herunterladen. 
2. Öffnen Sie ein Eingabeaufforderungsfenster oder eine Terminal-Shell, und führen Sie den folgenden Befehl im Verzeichnis Ihrer Wahl aus, um das Projekt zu klonen.
        
        git clone https://github.com/hdinsight/eventhubs-sample-event-producer.git eventhubs-client-sample

3. Dadurch wird ein neuer Ordner mit dem Namen „eventhubs-client-sample“ erstellt. Navigieren Sie in der Shell oder der Eingabeaufforderung zu diesem Ordner.
4. Führen Sie über den folgenden Befehl Maven aus, um die Anwendung zu erstellen:

          mvn package

5. Navigieren Sie in der Shell oder Eingabeaufforderung in das erstellte Zielverzeichnis, das die Datei ``com-microsoft-azure-eventhubs-client-example-0.2.0.jar`` enthält.
6. Als Nächstes müssen Sie den Ereignisherausgeber in der Befehlszeile für Ihren Event Hub starten. Ersetzen Sie dazu die Werte im Befehl wie folgt:

        java -cp com-microsoft-azure-eventhubs-client-example-0.2.0.jar com.microsoft.azure.eventhubs.client.example.EventhubsClientDriver --eventhubs-namespace "<namespace>" --eventhubs-name "hub1" --policy-name "RootManageSharedAccessKey" --policy-key "<policyKey>" --message-length 32 --thread-count 1 --message-count -1

7. Beispielsweise würde ein vollständiger Befehl etwa dem folgenden ähneln:

        java -cp com-microsoft-azure-eventhubs-client-example-0.2.0.jar com.microsoft.azure.eventhubs.client.example.EventhubsClientDriver --eventhubs-namespace "sparkstreaming" --eventhubs-name "hub1" --policy-name "RootManageSharedAccessKey" --policy-key "2P1Q17Wd1rdLP1OZQYn6dD2S13Bb3nF3h2XZD9hvyyU=" --message-length 32 --thread-count 1 --message-count -1

8. Der Befehl wird gestartet, und wenn Ihre Konfiguration fehlerfrei ist, werden nach kurzer Zeit Ausgaben für die an den Event Hub gesendeten Ereignisse angezeigt, die den folgenden ähneln:

        Map('policyKey -> 2P1Q17Wd1rdLP1OZQYn6dD2S13Bb3nF3h2XZD9hvyyU, 'eventhubsName -> hub1, 'policyName -> RootManageSharedAccessKey, 'eventhubsNamespace -> sparkstreaming, 'messageCount -> -1, 'messageLength -> 32, 'threadCount -> 1)
        Events per thread: -1 (-1 for unlimited)
        10 > Sun Jun 18 11:32:58 PDT 2017 > 1024 > 1024 > Sending event: ZZ93958saG5BUKbvUI9wHVmpuA2TrebS
        10 > Sun Jun 18 11:33:46 PDT 2017 > 2048 > 2048 > Sending event: RQorGRbTPp6U2wYzRSnZUlWEltRvTZ7R
        10 > Sun Jun 18 11:34:33 PDT 2017 > 3072 > 3072 > Sending event: 36Eoy2r8ptqibdlfCYSGgXe6ct4AyOX3
        10 > Sun Jun 18 11:35:19 PDT 2017 > 4096 > 4096 > Sending event: bPZma9V0CqOn6Hj9bhrrJT0bX2rbPSn3
        10 > Sun Jun 18 11:36:06 PDT 2017 > 5120 > 5120 > Sending event: H2TVD77HNTVyGsVcj76g0daVnYxN4Sqs

9. Unterbrechen Sie die Ausführung des Ereignisherausgebers nicht, während Sie mit den weiteren Schritten fortfahren.

## <a name="run-spark-shell-on-your-hdinsight-cluster"></a>Ausführen der Spark-Shell in Ihrem HDInsight-Cluster
In dieser Aufgabe melden Sie sich per SSH beim Hauptknoten Ihres HDInsight-Clusters an, starten die Spark-Shell und führen eine Spark Streaming-Anwendung aus, die die Ereignisse von Event Hubs abruft und verarbeitet. 

Zu diesem Zeitpunkt sollte Ihr HDInsight-Cluster bereit sein. Wenn dies nicht der Fall ist, müssen Sie warten, bis die Bereitstellung abgeschlossen ist. Wenn er bereit ist, führen Sie die folgenden Schritte aus:

1. Stellen Sie mithilfe von SSH eine Verbindung mit dem HDInsight-Cluster her. Anweisungen hierzu finden Sie unter [Herstellen einer Verbindung mit HDInsight über SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).

5. Die Anwendung, die Sie erstellen, erfordert das Event Hubs-Paket Spark Streaming. Um die Spark-Shell so auszuführen, dass diese Abhängigkeit automatisch von [Maven Central](https://search.maven.org) abgerufen wird, achten Sie darauf, den Schalter „packages“ wie folgt mit den Maven-Koordinaten anzugeben:

        spark-shell --packages "com.microsoft.azure:spark-streaming-eventhubs_2.11:2.1.0"

6. Nachdem die Spark-Shell vollständig geladen wurde, sollte Folgendes angezeigt werden:

        Welcome to
            ____              __
            / __/__  ___ _____/ /__
            _\ \/ _ \/ _ `/ __/  '_/
        /___/ .__/\_,_/_/ /_/\_\   version 2.1.0.2.6.0.10-29
            /_/
                
        Using Scala version 2.11.8 (OpenJDK 64-Bit Server VM, Java 1.8.0_131)
        Type in expressions to have them evaluated.
        Type :help for more information.

        scala> 

7. Kopieren Sie den folgenden Codeausschnitt in einen Text-Editor, und ändern Sie ihn so, dass die Werte „Policy Key“ (Richtlinienschlüssel) und „Namespace“ entsprechend Ihrem Event Hub festgelegt werden.

        val eventhubParameters = Map[String, String] (
            "eventhubs.policyname" -> "RootManageSharedAccessKey",
            "eventhubs.policykey" -> "<policyKey>",
            "eventhubs.namespace" -> "<namespace>",
            "eventhubs.name" -> "hub1",
            "eventhubs.partition.count" -> "2",
            "eventhubs.consumergroup" -> "$Default",
            "eventhubs.progressTrackingDir" -> "/eventhubs/progress",
            "eventhubs.sql.containsProperties" -> "true"
            )

8. Fügen Sie den angepassten Ausschnitt an der bereitstehenden Eingabeaufforderung „scala>“ ein, und drücken Sie die EINGABETASTE. Eine ähnliche Ausgabe wie die folgende sollte angezeigt werden:

        scala> val eventhubParameters = Map[String, String] (
            |       "eventhubs.policyname" -> "RootManageSharedAccessKey",
            |       "eventhubs.policykey" -> "2P1Q17Wd1rdLP1OZQYn6dD2S13Bb3nF3h2XZD9hvyyU",
            |       "eventhubs.namespace" -> "sparkstreaming",
            |       "eventhubs.name" -> "hub1",
            |       "eventhubs.partition.count" -> "2",
            |       "eventhubs.consumergroup" -> "$Default",
            |       "eventhubs.progressTrackingDir" -> "/eventhubs/progress",
            |       "eventhubs.sql.containsProperties" -> "true"
            |     )
        eventhubParameters: scala.collection.immutable.Map[String,String] = Map(eventhubs.sql.containsProperties -> true, eventhubs.name -> hub1, eventhubs.consumergroup -> $Default, eventhubs.partition.count -> 2, eventhubs.progressTrackingDir -> /eventhubs/progress, eventhubs.policykey -> 2P1Q17Wd1rdLP1OZQYn6dD2S13Bb3nF3h2XZD9hvyyU, eventhubs.namespace -> hdiz-docs-eventhubs, eventhubs.policyname -> RootManageSharedAccessKey)

9. Als Nächstes beginnen Sie mit der Formulierung einer Spark Structured Streaming-Abfrage, indem Sie die Quelle angeben. Fügen Sie Folgendes in die Spark-Shell ein, und drücken Sie die EINGABETASTE.

        val inputStream = spark.readStream.
        format("eventhubs").
        options(eventhubParameters).
        load()

10. Eine ähnliche Ausgabe wie die folgende sollte angezeigt werden:

        inputStream: org.apache.spark.sql.DataFrame = [body: binary, offset: bigint ... 5 more fields]

11. Formulieren Sie anschließend die Abfrage so, dass ihre Ausgabe in die Konsole geschrieben wird. Fügen Sie dazu Folgendes in die Spark-Shell ein, und drücken Sie die EINGABETASTE.

        val streamingQuery1 = inputStream.writeStream.
        outputMode("append").
        format("console").start().awaitTermination()

12. Es sollten einige Batches angezeigt werden, die Ausgaben ähnlich der folgenden aufweisen.

        -------------------------------------------
        Batch: 0
        -------------------------------------------
        [Stage 0:>                                                          (0 + 2) / 2]

13. Darauf folgen die Ausgabeergebnisse der Verarbeitung der einzelnen Microbatches von Ereignissen. 

        -------------------------------------------
        Batch: 0
        -------------------------------------------
        17/06/18 18:57:39 WARN TaskSetManager: Stage 1 contains a task of very large size (419 KB). The maximum recommended task size is 100 KB.
        +--------------------+------+---------+------------+---------+------------+----------+
        |                body|offset|seqNumber|enqueuedTime|publisher|partitionKey|properties|
        +--------------------+------+---------+------------+---------+------------+----------+
        |[7B 22 74 65 6D 7...|     0|        0|  1497734887|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   112|        1|  1497734887|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   224|        2|  1497734887|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   336|        3|  1497734887|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   448|        4|  1497734887|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   560|        5|  1497734887|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   672|        6|  1497734887|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   784|        7|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   896|        8|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1008|        9|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1120|       10|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1232|       11|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1344|       12|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1456|       13|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1568|       14|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1680|       15|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1792|       16|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1904|       17|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  2016|       18|  1497734889|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  2128|       19|  1497734889|     null|        null|     Map()|
        +--------------------+------+---------+------------+---------+------------+----------+
        only showing top 20 rows

14. Wenn neue Ereignisse vom Ereignisherausgeber eingehen, werden sie von dieser Structured Streaming-Abfrage verarbeitet.
15. Achten Sie darauf, Ihren HDInsight-Cluster zu löschen, wenn Sie mit dem Ausführen dieses Beispiels fertig sind.



## <a name="see-also"></a>Weitere Informationen

* [Übersicht über Spark-Streaming](apache-spark-streaming-overview.md)













