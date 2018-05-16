---
title: 'Tutorial: Verwenden der Apache Kafka Streams-API – Azure HDInsight | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie die Apache Kafka Streams-API mit Kafka in HDInsight verwenden. Mit dieser API können Sie eine Streamverarbeitung zwischen Themen in Kafka ausführen.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: tutorial
ms.date: 04/17/2018
ms.author: larryfr
ms.openlocfilehash: 8aff28079a0aaa7c02d8a187cb379ecdbedcd854
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="tutorial-apache-kafka-streams-api"></a>Tutorial: Apache Kafka Streams-API

Erfahren Sie, wie Sie eine Anwendung erstellen, die die Apache Kafka Streams-API verwendet, und diese mit Kafka in HDInsight ausführen. 

Die in diesem Tutorial verwendete Anwendung ist eine Streamingwörterzählung. Sie liest Textdaten aus einem Kafka-Thema, extrahiert einzelne Wörter, und speichert dann Word und Anzahl in einem anderen Kafka-Thema.

> [!NOTE]
> Die Kafka-Streamverarbeitung erfolgt häufig über Apache Spark oder Storm. Die Kafka Streams-API wurde in Kafka Version 0.10.0 (in HDInsight 3.5 und 3.6) eingeführt. Mit dieser API können Sie Datenstreams zwischen Eingabe- und Ausgabethemen transformieren. In einigen Fällen kann dies eine Alternative zum Erstellen einer Spark- oder Storm-Streaminglösung sein. 
>
> Weitere Informationen zu Kafka Streams finden Sie in der Dokumentation [Intro to Streams](https://kafka.apache.org/10/documentation/streams/) (Einführung in Streams) auf Apache.org.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Einrichten der Entwicklungsumgebung
> * Grundlegendes zum Code
> * Erstellen und Bereitstellen der Anwendung
> * Konfigurieren von Kafka-Themen
> * Ausführen des Codes

## <a name="prerequisites"></a>Voraussetzungen

* Ein Kafka-auf-HDInsight-3.6-Cluster. Informationen zum Erstellen eines Clusters für Kafka in HDInsight finden Sie im Dokument [Einstieg in Apache Kafka in HDInsight](apache-kafka-get-started.md).

* Führen Sie die im Dokument [Apache Kafka Producer- und Consumer-APIs](apache-kafka-producer-consumer-api.md) beschriebenen Schritte aus. Die Schritte in diesem Dokument verwenden die Beispielanwendung und die Themen, die in diesem Tutorial erstellt werden.

## <a name="set-up-your-development-environment"></a>Einrichten der Entwicklungsumgebung

Die folgenden Komponenten müssen in der Entwicklungsumgebung installiert sein:

* [Java JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) oder ein gleichwertiges Kit, z.B. OpenJDK.

* [Apache Maven](http://maven.apache.org/)

* Ein SSH-Client und der Befehl `scp`. Weitere Informationen finden Sie im Dokument [Herstellen einer Verbindung mit HDInsight (Hadoop) per SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="understand-the-code"></a>Grundlegendes zum Code

Die exemplarische Anwendung befindet sich unter [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started) im `Streaming`-Unterverzeichnis. Die Anwendung besteht aus zwei Dateien:

* `pom.xml`: Diese Datei definiert die Projektabhängigkeiten, Java-Version und Verpackungsmethoden.
* `Stream.java`: Diese Datei implementiert die Streaminglogik.

### <a name="pomxml"></a>Pom.Xml

Wichtige Informationen zur `pom.xml`-Datei:

* Abhängigkeiten: Dieses Projekt hängt von der Kafka Streams-API ab, die im `kafka-clients`-Paket bereitgestellt wird. Der folgende XML-Code definiert diese Abhängigkeit:

    ```xml
    <!-- Kafka client for producer/consumer operations -->
    <dependency>
      <groupId>org.apache.kafka</groupId>
      <artifactId>kafka-clients</artifactId>
      <version>${kafka.version}</version>
    </dependency>
    ```

    > [!NOTE]
    > Der `${kafka.version}`-Eintrag wird im `<properties>..</properties>`-Abschnitt von `pom.xml` deklariert und ist für die Kafka-Version des HDInsight-Clusters konfiguriert.

* Plug-Ins: Maven-Plug-Ins bieten verschiedene Funktionen. In diesem Projekt werden die folgenden Plug-Ins verwendet:

    * `maven-compiler-plugin`: Wird verwendet, um bis 8 die vom Projekt verwendete Java-Version festzulegen. Java 8 ist für HDInsight 3.6 erforderlich.
    * `maven-shade-plugin`: Wird zum Generieren einer Uber JAR-Datei verwendet, die diese Anwendung sowie alle Abhängigkeiten enthält. Es wird auch zum Festlegen des Einstiegspunkts der Anwendung verwendet, damit Sie die Jar-Datei direkt ausführen können, ohne die Hauptklasse angeben zu müssen.

### <a name="streamjava"></a>Stream.Java

Die `Stream.java`-Datei implementiert mit der Streams-API eine Wörterzählanwendung. Sie liest Daten aus einem Kafka-Thema mit dem Namen `test` und schreibt die Wörterzahlen in ein Thema namens `wordcounts`.

Der folgende Code definiert die Wörterzählanwendung:

```java
package com.microsoft.example;

import org.apache.kafka.common.serialization.Serde;
import org.apache.kafka.common.serialization.Serdes;
import org.apache.kafka.streams.KafkaStreams;
import org.apache.kafka.streams.KeyValue;
import org.apache.kafka.streams.StreamsConfig;
import org.apache.kafka.streams.kstream.KStream;
import org.apache.kafka.streams.kstream.KStreamBuilder;

import java.util.Arrays;
import java.util.Properties;

public class Stream
{
    public static void main( String[] args ) {
        Properties streamsConfig = new Properties();
        // The name must be unique on the Kafka cluster
        streamsConfig.put(StreamsConfig.APPLICATION_ID_CONFIG, "wordcount-example");
        // Brokers
        streamsConfig.put(StreamsConfig.BOOTSTRAP_SERVERS_CONFIG, args[0]);
        // SerDes for key and values
        streamsConfig.put(StreamsConfig.KEY_SERDE_CLASS_CONFIG, Serdes.String().getClass().getName());
        streamsConfig.put(StreamsConfig.VALUE_SERDE_CLASS_CONFIG, Serdes.String().getClass().getName());

        // Serdes for the word and count
        Serde<String> stringSerde = Serdes.String();
        Serde<Long> longSerde = Serdes.Long();

        KStreamBuilder builder = new KStreamBuilder();
        KStream<String, String> sentences = builder.stream(stringSerde, stringSerde, "test");
        KStream<String, Long> wordCounts = sentences
                .flatMapValues(value -> Arrays.asList(value.toLowerCase().split("\\W+")))
                .map((key, word) -> new KeyValue<>(word, word))
                .countByKey("Counts")
                .toStream();
        wordCounts.to(stringSerde, longSerde, "wordcounts");

        KafkaStreams streams = new KafkaStreams(builder, streamsConfig);
        streams.start();

        Runtime.getRuntime().addShutdownHook(new Thread(streams::close));
    }
}
```


## <a name="build-and-deploy-the-example"></a>Erstellen und Bereitstellen des Beispiels

Führen Sie die folgenden Schritte aus, um das Projekt in Ihrem Cluster für Kafka in HDInsight zu erstellen und bereitzustellen:

1. Laden Sie die Beispiele von [ https://github.com/Azure-Samples/hdinsight-kafka-java-get-started ](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started) herunter.

2. Wechseln Sie zum Verzeichnis `Streaming`, und erstellen Sie dann mit dem folgenden Befehl ein JAR-Paket:

    ```bash
    mvn clean package
    ```

    Dieser Befehl erstellt das Paket unter `target/kafka-streaming-1.0-SNAPSHOT.jar`.

3. Verwenden Sie den folgenden Befehl, um die Datei `kafka-streaming-1.0-SNAPSHOT.jar` in Ihren HDInsight-Cluster zu kopieren:
   
    ```bash
    scp ./target/kafka-streaming-1.0-SNAPSHOT.jar sshuser@clustername-ssh.azurehdinsight.net:kafka-streaming.jar
    ```
   
    Ersetzen Sie `sshuser` durch den SSH-Benutzer für Ihren Cluster und `clustername` durch den Namen Ihres Clusters. Geben Sie bei entsprechender Aufforderung das Kennwort für das SSH-Benutzerkonto ein. Weitere Informationen zum Verwenden von `scp` mit HDInsight finden Sie unter [Verwenden von SSH mit HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="create-kafka-topics"></a>Erstellen von Kafka-Themen

1. Um eine SSH-Verbindung mit dem Cluster zu öffnen, führen Sie folgenden Befehl aus:

    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```

    Ersetzen Sie `sshuser` durch den SSH-Benutzer für Ihren Cluster und `clustername` durch den Namen Ihres Clusters. Geben Sie bei entsprechender Aufforderung das Kennwort für das SSH-Benutzerkonto ein. Weitere Informationen zum Verwenden von `scp` mit HDInsight finden Sie unter [Verwenden von SSH mit HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. Um den Clusternamen in einer Variablen zu speichern und ein JSON-Analysehilfsprogramm (`jq`) zu installieren, verwenden Sie die folgenden Befehle. Geben Sie bei entsprechender Aufforderung den Namen des Kafka-Clusters ein:

    ```bash
    sudo apt -y install jq
    read -p 'Enter your Kafka cluster name:' CLUSTERNAME
    ```

3. Um die Kafka-Brokerhosts und die Zookeeper-Hosts abzurufen, verwenden Sie die folgenden Befehle. Geben Sie bei der entsprechenden Aufforderung das Kennwort des Anmeldekontos (Administrator) für den Cluster ein. Sie werden zweimal aufgefordert, das Kennwort einzugeben.

    ```bash
    export KAFKAZKHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`; \
    export KAFKABROKERS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`; \
    ```

4. Um die vom Streamingvorgang verwendeten Themen zu erstellen, verwenden Sie die folgenden Befehle:

    > [!NOTE]
    > Sie erhalten möglicherweise eine Fehlermeldung, dass das `test`-Thema bereits vorhanden ist. Dies ist in Ordnung, da es im Producer- und Consumer-API-Tutorial erstellt worden sein könnte.

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic test --zookeeper $KAFKAZKHOSTS

    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic wordcounts --zookeeper $KAFKAZKHOSTS

    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic RekeyedIntermediateTopic --zookeeper $KAFKAZKHOSTS

    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic wordcount-example-Counts-changelog --zookeeper $KAFKAZKHOSTS
    ```

    Die Themen werden für folgende Zwecke verwendet:

    * `test`: Dieses Thema befindet sich dort, wo Datensätze empfangen werden. Die Streaminganwendung liest von dort.
    * `wordcounts`: Dieses Thema befindet sich dort, wo die Streaminganwendung ihre Ausgabe speichert.
    * `RekeyedIntermediateTopic`: Mit diesem Thema werden Daten neu partitioniert, wenn die Anzahl vom `countByKey`-Operator aktualisiert wird.
    * `wordcount-example-Counts-changelog`: Dieses Thema ist ein Zustandsspeicher, den der `countByKey`-Vorgang verwendet.

    > [!IMPORTANT]
    > Kafka in HDInsight kann auch für das automatische Erstellen von Themen konfiguriert werden. Weitere Informationen finden Sie im Dokument [Konfigurieren von Apache Kafka in HDInsight zum automatischen Erstellen von Themen](apache-kafka-auto-create-topics.md).

## <a name="run-the-code"></a>Ausführen des Codes

1. Um die Streaminganwendung als Hintergrundprozess zu starten, verwenden Sie den folgenden Befehl:

    ```bash
    java -jar kafka-streaming.jar $KAFKABROKERS $KAFKAZKHOSTS &
    ```

    > [!NOTE]
    > Möglicherweise erhalten Sie eine Warnung über log4j. Sie können dies ignorieren.

2. Um Datensätze an das `test`-Thema zu senden, starten Sie die Producer-Anwendung mit folgendem Befehl:

    ```bash
    java -jar kafka-producer-consumer.jar producer $KAFKABROKERS
    ```

3. Nachdem der Producer abgeschlossen ist, zeigen Sie die im `wordcounts`-Thema gespeicherten Informationen mit folgendem Befehl an:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKABROKERS --topic wordcounts --formatter kafka.tools.DefaultMessageFormatter --property print.key=true --property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer --from-beginning
    ```

    > [!NOTE]
    > Die `--property`-Parameter teilen dem Konsolenconsumer mit, dass der Schlüssel (Wort) zusammen mit der Anzahl (Wert) ausgegeben werden soll. Diese Parameter konfigurieren auch das Deserialisierungsprogramm, das beim Lesen dieser Werte aus Kafka verwendet werden soll.

    Die Ausgabe sieht in etwa wie folgender Text aus:
   
        dwarfs  13635
        ago     13664
        snow    13636
        dwarfs  13636
        ago     13665
        a       13803
        ago     13666
        a       13804
        ago     13667
        ago     13668
        jumped  13640
        jumped  13641
   
    > [!NOTE]
    > Der Parameter `--from-beggining` konfiguriert den Consumer so, dass er am Anfang der im Thema gespeicherten Datensätze beginnt. Die Anzahl erhöht sich jedes Mal, wenn ein Wort angetroffen wird, sodass das Thema mehrere Einträge für jedes Wort enthält – mit zunehmender Anzahl.

7. Drücken Sie __STRG+C__, um den Producer zu beenden. Drücken Sie __STRG+C__ erneut, um die Anwendung und den Consumer zu beenden.

## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument haben Sie erfahren, wie Sie die Kafka Streams-API mit Kafka in HDInsight verwenden. Verwenden Sie Folgendes, um weitere Informationen zur Verwendung von Kafka zu erhalten:

* [Analysieren von Kafka-Protokollen](apache-kafka-log-analytics-operations-management.md)
* [Replizieren von Daten zwischen Kafka-Clustern](apache-kafka-mirroring.md)
