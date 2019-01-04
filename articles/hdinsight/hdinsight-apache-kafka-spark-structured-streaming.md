---
title: 'Tutorial: Strukturiertes Apache Spark-Streaming mit Apache Kafka: Azure HDInsight'
description: Erfahren Sie, wie Sie Apache Spark-Streaming verwenden, um Daten in oder aus Apache Kafka zu streamen. In diesem Tutorial streamen Sie Daten mithilfe eines Jupyter Notebooks aus Spark in HDInsight.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,seodec18
ms.topic: tutorial
ms.date: 11/06/2018
ms.author: hrasheed
ms.openlocfilehash: 4dcf482e02d38e18b7b86fc57565a47510627d44
ms.sourcegitcommit: e68df5b9c04b11c8f24d616f4e687fe4e773253c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/20/2018
ms.locfileid: "53652863"
---
# <a name="tutorial-use-apache-spark-structured-streaming-with-apache-kafka-on-hdinsight"></a>Tutorial: Verwenden von strukturiertem Apache Spark-Streaming mit Apache Kafka in HDInsight

Dieses Tutorial veranschaulicht die Verwendung des [strukturierten Apache Spark-Streamings](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html) zum Lesen und Schreiben von Daten mit [Apache Kafka](https://kafka.apache.org/) in Azure HDInsight.

Strukturiertes Spark-Streaming ist eine auf Spark SQL basierende Stream-Verarbeitungs-Engine. Sie können damit Streamingberechnungen genauso wie Batchberechnung auf statischen Daten ausdrücken.  

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Strukturiertes Streaming mit Kafka
> * Erstellen von Kafka- und Spark-Clustern
> * Hochladen des Notebooks in Spark
> * Verwenden des Notebooks
> * Bereinigen von Ressourcen

Denken Sie nach dem Ausführen der Schritte in diesem Dokument daran, die Cluster zu löschen, um das Anfallen von Gebühren zu verhindern.

## <a name="prerequisites"></a>Voraussetzungen

* Kenntnisse im Umgang mit [Jupyter Notebooks](https://jupyter.org/) mit Spark in HDInsight. Weitere Informationen finden Sie im Dokument [Tutorial: Laden von Daten und Ausführen von Abfragen auf einem Apache Spark-Cluster in Azure HDInsight](spark/apache-spark-load-data-run-query.md).

* Kenntnisse in der Programmiersprache [Scala](https://www.scala-lang.org/). Der in diesem Tutorial verwendete Code ist in Scala geschrieben.

* Kenntnisse im Erstellen von Kafka-Themen. Weitere Informationen finden Sie im Dokument [Schnellstart: Erstellen eines Apache Kafka-Clusters in HDInsight](kafka/apache-kafka-get-started.md).

> [!IMPORTANT]  
> Für die in diesem Dokument beschriebenen Schritte wird eine Azure-Ressourcengruppe benötigt, die sowohl einen Spark- und einen Kafka-Cluster in HDInsight beinhaltet. Die Cluster befinden sich innerhalb eines virtuellen Azure-Netzwerks, wodurch Spark- und Kafka-Cluster direkt miteinander kommunizieren können.
> 
> Der Einfachheit halber enthält dieses Dokument Links zu einer Vorlage, mit der die erforderlichen Azure-Ressourcen erstellt werden können. 
>
> Weitere Informationen zur Verwendung von HDInsight in einem virtuellen Netzwerk finden Sie im Dokument [Erweitern von HDInsight mit einem virtuellen Netzwerk](hdinsight-extend-hadoop-virtual-network.md).

## <a name="structured-streaming-with-apache-kafka"></a>Strukturiertes Streaming mit Apache Kafka

Das strukturierte Spark-Streaming ist eine auf Spark SQL basierende Streamverarbeitungs-Engine. Wenn Sie das strukturierte Streaming verwenden, können Sie Streamingabfragen auf dieselbe Weise wie Batchabfragen schreiben.

Die folgenden Codeausschnitte veranschaulichen das Lesen aus Kafka und das Speichern in einer Datei. Der erste ist ein Batchvorgang, und der zweite ist ein Streamingvorgang:

```scala
// Read a batch from Kafka
val kafkaDF = spark.read.format("kafka")
                .option("kafka.bootstrap.servers", kafkaBrokers)
                .option("subscribe", kafkaTopic)
                .option("startingOffsets", "earliest")
                .load()
// Select data and write to file
kafkaDF.select(from_json(col("value").cast("string"), schema) as "trip")
                .write
                .format("parquet")
                .option("path","/example/batchtripdata")
                .option("checkpointLocation", "/batchcheckpoint")
                .save()
```

```scala
// Stream from Kafka
val kafkaStreamDF = spark.readStream.format("kafka")
                .option("kafka.bootstrap.servers", kafkaBrokers)
                .option("subscribe", kafkaTopic)
                .option("startingOffsets", "earliest")
                .load()
// Select data from the stream and write to file
kafkaStreamDF.select(from_json(col("value").cast("string"), schema) as "trip")
                .writeStream
                .format("parquet")
                .option("path","/example/streamingtripdata")
                .option("checkpointLocation", "/streamcheckpoint")
                .start.awaitTermination(30000)
```

In beiden Ausschnitten werden die Daten aus Kafka gelesen und in eine Datei geschrieben. Folgende Unterschiede weisen die Beispiele auf:

| Batch | Streaming |
| --- | --- |
| `read` | `readStream` |
| `write` | `writeStream` |
| `save` | `start` |

Der Streamingvorgang verwendet außerdem `awaitTermination(30000)`, sodass das Streaming nach 30.000 ms beendet wird. 

Um das strukturierte Streaming mit Kafka zu verwenden, benötigt das Projekt eine Abhängigkeit im Paket `org.apache.spark : spark-sql-kafka-0-10_2.11`. Die Version dieses Pakets sollte der Version von Spark in HDInsight entsprechen. Für Spark 2.2.0 (verfügbar in HDInsight 3.6) finden Sie die Abhängigkeitsinformationen für verschiedene Projekttypen unter [https://search.maven.org/#artifactdetails%7Corg.apache.spark%7Cspark-sql-kafka-0-10_2.11%7C2.2.0%7Cjar](https://search.maven.org/#artifactdetails%7Corg.apache.spark%7Cspark-sql-kafka-0-10_2.11%7C2.2.0%7Cjar).

Für das in diesem Tutorial bereitgestellte Jupyter Notebook lädt die folgende Zelle diese Paketabhängigkeit:

```
%%configure -f
{
    "conf": {
        "spark.jars.packages": "org.apache.spark:spark-sql-kafka-0-10_2.11:2.2.0",
        "spark.jars.excludes": "org.scala-lang:scala-reflect,org.apache.spark:spark-tags_2.11"
    }
}
```

## <a name="create-the-clusters"></a>Erstellen von Clustern

Apache Kafka in HDInsight ermöglicht keinen Zugriff auf die Kafka-Broker über das öffentliche Internet. Alle von Kafka verwendeten Elemente müssen sich im gleichen virtuellen Azure-Netzwerk befinden. In diesem Tutorial befinden sich der Kafka- und der Spark-Cluster im gleichen virtuellen Azure-Netzwerk. 

Das folgende Diagramm zeigt den Kommunikationsfluss zwischen Spark und Kafka:

![Diagramm der Spark- und Kafka-Cluster in einem virtuellen Azure-Netzwerk](./media/hdinsight-apache-spark-with-kafka/spark-kafka-vnet.png)

> [!NOTE]  
> Der Kafka-Dienst ist auf die Kommunikation innerhalb des virtuellen Netzwerks beschränkt. Auf andere Dienste auf dem Cluster, wie z.B. SSH und Ambari, kann über das Internet zugegriffen werden. Weitere Informationen zu den öffentlichen Ports, die für HDInsight verfügbar sind, finden Sie unter [Von HDInsight verwendete Ports und URIs](hdinsight-hadoop-port-settings-for-services.md).

Führen Sie zum Erstellen eines virtuellen Azure-Netzwerks und zum anschließenden Erstellen der Kafka- und Spark-Cluster in diesem die folgenden Schritte aus:

1. Verwenden Sie die folgende Schaltfläche, um sich bei Azure anzumelden, und öffnen Sie die Vorlage im Azure-Portal.
    
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fhdinsight-spark-kafka-structured-streaming%2Fmaster%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apache-spark-with-kafka/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    Die Azure Resource Manager-Vorlage finden Sie unter **https://raw.githubusercontent.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming/master/azuredeploy.json**.

    Diese Vorlage erstellt die folgenden Ressourcen:

    * Ein Kafka-auf-HDInsight-3.6-Cluster.
    * Einen Spark 2.2.0-Cluster in HDInsight 3.6.
    * Ein Azure Virtual Network, das die HDInsight-Cluster enthält.

    > [!IMPORTANT]  
    > Das in diesem Tutorial für strukturiertes Streaming verwendete Notebook benötigt Spark 2.2.0 in HDInsight 3.6. Bei Verwendung einer früheren Version von Spark auf HDInsight erhalten Sie Fehlermeldungen, wenn Sie das Notebook verwenden.

2. Geben Sie im Abschnitt **Benutzerdefinierte Vorlage** folgende Informationen an:

    | Einstellung | Wert |
    | --- | --- |
    | Abonnement | Ihr Azure-Abonnement |
    | Ressourcengruppe | Die Ressourcengruppe mit den Ressourcen. |
    | Standort | Die Azure-Region, in der die Ressourcen erstellt werden. |
    | Spark Cluster Name (Spark-Clustername) | Der Name des Spark-Clusters. Die ersten sechs Zeichen müssen sich vom Kafka-Clusternamen unterscheiden. |
    | Kafka Cluster Name (Kafka-Clustername) | Der Name des Kafka-Clusters. Die ersten sechs Zeichen müssen sich vom Spark-Clusternamen unterscheiden. |
    | Benutzername für Clusteranmeldung | Der Administratorbenutzername für die Cluster. |
    | Kennwort für Clusteranmeldung | Das Administratorbenutzerkennwort für die Cluster. |
    | SSH-Benutzername | Der SSH-Benutzer, der für die Cluster erstellt werden soll. |
    | SSH-Kennwort | Das Kennwort für den SSH-Benutzer. |
   
    ![Screenshot der angepassten Vorlage](./media/hdinsight-apache-kafka-spark-structured-streaming/spark-kafka-template.png)

3. Lesen Sie die **Geschäftsbedingungen**, und wählen Sie anschließend die Option **Ich stimme den oben genannten Geschäftsbedingungen zu** aus.

4. Aktivieren Sie zum Schluss **An Dashboard anheften**, und wählen Sie dann **Kaufen** aus. 

> [!NOTE]  
> Die Clustererstellung kann bis zu 20 Minuten dauern.

## <a name="upload-the-notebook"></a>Hochladen des Notebooks

Gehen Sie wie folgt vor, um das Notebook aus dem Projekt in Ihren Spark-Cluster in HDInsight hochzuladen:

1. Laden Sie das Projekt unter [https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming](https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming) herunter.

1. Verbinden Sie in Ihrem Webbrowser das Jupyter Notebook mit Ihrem Spark-Cluster. Ersetzen Sie in der folgenden URL `CLUSTERNAME` durch den Namen Ihres __Spark__-Clusters:

        https://CLUSTERNAME.azurehdinsight.net/jupyter

    Geben Sie bei Aufforderung den Clusterbenutzernamen (Administrator) und das Kennwort ein, den bzw. das Sie beim Erstellen des Clusters verwendet haben.

2. Klicken Sie rechts oben auf der Seite auf die Schaltfläche __Hochladen__, um die Datei __spark-structured-streaming-kafka.ipynb__ in den Cluster hochzuladen. Wählen Sie __Öffnen__, um das Hochladen zu starten.

    ![Verwenden der Schaltfläche „Hochladen“ für die Auswahl und das Hochladen eines Notebooks](./media/hdinsight-apache-kafka-spark-structured-streaming/upload-button.png)

    ![Auswahl der KafkaStreaming.ipynb-Datei](./media/hdinsight-apache-kafka-spark-structured-streaming/select-notebook.png)

3. Suchen Sie in der Liste mit den Notebooks nach dem Eintrag __spark-structured-streaming-kafka.ipynb__, und klicken Sie neben dem Eintrag auf die Schaltfläche __Hochladen__.

    ![Verwenden Sie zum Hochladen des Notebooks die Schaltfläche „Hochladen“ neben dem Eintrag „KafkaStreaming.ipynb“.](./media/hdinsight-apache-kafka-spark-structured-streaming/upload-notebook.png)


## <a name="use-the-notebook"></a>Verwenden des Notebooks

Klicken Sie nach dem Hochladen der Dateien auf den Eintrag __spark-structured-streaming-kafka.ipynb__, um das Notebook zu öffnen. Folgen Sie den Anweisungen im Notebook, um sich mit der Verwendung von Spark Structured Streaming mit Kafka in HDInsight vertraut zu machen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Zum Bereinigen der im Rahmen dieses Tutorials erstellten Ressourcen können Sie die Ressourcengruppe löschen. Dadurch werden auch der zugeordnete HDInsight-Cluster sowie alle anderen Ressourcen gelöscht, die der Ressourcengruppe zugeordnet sind.

So entfernen Sie die Ressourcengruppe über das Azure-Portal:

1. Erweitern Sie im Azure-Portal das Menü auf der linken Seite, um das Menü mit den Diensten zu öffnen, und klicken Sie auf __Ressourcengruppen__, um die Liste mit Ihren Ressourcengruppen anzuzeigen.
2. Suchen Sie die zu löschende Ressourcengruppe, und klicken Sie mit der rechten Maustaste rechts neben dem Eintrag auf die Schaltfläche __Mehr__ (...).
3. Klicken Sie auf __Ressourcengruppe löschen__, und bestätigen Sie den Vorgang.

> [!WARNING]  
> Die Abrechnung für einen HDInsight-Cluster beginnt, sobald der Cluster erstellt wurde, und endet mit dem Löschen des Clusters. Die Gebühren werden anteilig nach Minuten erhoben. Daher sollten Sie Ihren Cluster immer löschen, wenn Sie ihn nicht mehr verwenden.
> 
> Wenn Sie einen Kafka-Cluster in HDInsight löschen, werden auch alle in Kafka gespeicherten Daten gelöscht.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie das [strukturierte Apache Spark-Streaming](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html) zum Schreiben und Lesen von Daten aus [Apache Kafka](https://kafka.apache.org/) in HDInsight verwenden. Unter dem folgenden Link erfahren Sie, wie Sie [Apache Storm](https://storm.apache.org/) mit Kafka verwenden:

> [!div class="nextstepaction"]
> [Tutorial: Verwenden von Apache Storm mit Apache Kafka in HDInsight](hdinsight-apache-storm-with-kafka.md)
