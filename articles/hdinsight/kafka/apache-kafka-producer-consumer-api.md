---
title: 'Tutorial: Verwenden der Apache Kafka Producer- und Consumer-APIs – Azure HDInsight '
description: Erfahren Sie, wie Sie die Apache Kafka Producer- und Consumer-APIs mit Kafka in HDInsight verwenden. In diesem Tutorial erfahren Sie, wie Sie diese APIs mit Kafka auf HDInsight aus einer Java-Anwendung verwenden.
services: hdinsight
author: jasonwhowell
ms.author: jasonh
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: tutorial
ms.date: 04/16/2018
ms.openlocfilehash: f757db47ff91537405b04dbc949797f5855b7952
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50416170"
---
# <a name="tutorial-use-the-apache-kafka-producer-and-consumer-apis"></a>Tutorial: Verwenden der Apache Kafka Producer- und Consumer-APIs

Erfahren Sie, wie Sie die Kafka Producer- und Consumer-APIs mit Kafka in HDInsight verwenden.

Mit der Kafka Producer-API können Anwendungen Datenströme an den Kafka-Cluster senden. Mit der Kafka Consumer-API können Anwendungen Datenströme aus dem Cluster lesen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Einrichten der Entwicklungsumgebung
> * Einrichten Ihrer Bereitstellungsumgebung
> * Grundlegendes zum Code
> * Erstellen und Bereitstellen der Anwendung
> * Ausführen der Anwendung im Cluster

Weitere Informationen zu den APIs finden Sie in der Apache-Dokumentation unter [Producer API](https://kafka.apache.org/documentation/#producerapi) und [Consumer API](https://kafka.apache.org/documentation/#consumerapi).

## <a name="set-up-your-development-environment"></a>Einrichten der Entwicklungsumgebung

Die folgenden Komponenten müssen in der Entwicklungsumgebung installiert sein:

* [Java JDK 8](https://aka.ms/azure-jdks) oder ein gleichwertiges Kit, z.B. OpenJDK.

* [Apache Maven](http://maven.apache.org/)

* Ein SSH-Client und der Befehl `scp`. Weitere Informationen finden Sie im Dokument [Herstellen einer Verbindung mit HDInsight (Hadoop) per SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).

* Ein Text-Editor oder eine Java-IDE.

Bei der Installation von Java und dem JDK auf Ihrer Entwicklungsworkstation können die folgenden Umgebungsvariablen festgelegt werden. Sie sollten dennoch prüfen, ob die Variablen vorhanden sind und korrekte Werte für Ihr System enthalten.

* `JAVA_HOME` – sollte auf das Verzeichnis verweisen, in dem das JDK installiert ist.
* `PATH` – sollte die folgenden Pfade enthalten:
  
    * `JAVA_HOME` (oder entsprechender Pfad)
    * `JAVA_HOME\bin` (oder entsprechender Pfad)
    * Das Verzeichnis, in dem Maven installiert ist

## <a name="set-up-your-deployment-environment"></a>Einrichten Ihrer Bereitstellungsumgebung

Dieses Tutorial erfordert Kafka in HDInsight 3.6. Informationen zum Erstellen eines Clusters für Kafka in HDInsight finden Sie im Dokument [Einstieg in Apache Kafka in HDInsight](apache-kafka-get-started.md).

## <a name="understand-the-code"></a>Grundlegendes zum Code

Die exemplarische Anwendung befindet sich unter [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started) im `Producer-Consumer`-Unterverzeichnis. Die Anwendung besteht im Wesentlichen aus vier Dateien:

* `pom.xml`: Diese Datei definiert die Projektabhängigkeiten, Java-Version und Verpackungsmethoden.
* `Producer.java`: Diese Datei sendet 1 Million (1.000.000) zufällige Sätze mithilfe der Producer-API an Kafka.
* `Consumer.java`: Diese Datei verwendet die Consumer-API zum Lesen von Daten aus Kafka und Ausgeben an STDOUT.
* `Run.java`: Über die Befehlszeilenschnittstelle werden Producer- und Consumer-Code ausgeführt.

### <a name="pomxml"></a>Pom.Xml

Wichtige Informationen zur `pom.xml`-Datei:

* Abhängigkeiten: Dieses Projekt hängt von den Kafka Producer- und Consumer-APIs ab, die im `kafka-clients`-Paket bereitgestellt werden. Der folgende XML-Code definiert diese Abhängigkeit:

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

    * `maven-compiler-plugin`: Wird verwendet, um bis 8 die vom Projekt verwendete Java-Version festzulegen. Dies ist die von HDInsight 3.6 verwendete Java-Version.
    * `maven-shade-plugin`: Wird zum Generieren einer Uber JAR-Datei verwendet, die diese Anwendung sowie alle Abhängigkeiten enthält. Es wird auch zum Festlegen des Einstiegspunkts der Anwendung verwendet, damit Sie die JAR-Datei direkt ausführen können, ohne die Hauptklasse angeben zu müssen.

### <a name="producerjava"></a>Producer.java

Der Producer kommuniziert mit den Kafka-Brokerhosts (Workerknoten), um Daten in einem Kafka-Thema zu speichern. Der folgende Codeausschnitt stammt aus der Datei `Producer.java`:

```java
package com.microsoft.example;

import org.apache.kafka.clients.producer.KafkaProducer;
import org.apache.kafka.clients.producer.ProducerRecord;
import java.util.Properties;
import java.util.Random;
import java.io.IOException;

public class Producer
{
    public static void produce(String brokers) throws IOException
    {

        // Set properties used to configure the producer
        Properties properties = new Properties();
        // Set the brokers (bootstrap servers)
        properties.setProperty("bootstrap.servers", brokers);
        // Set how to serialize key/value pairs
        properties.setProperty("key.serializer","org.apache.kafka.common.serialization.StringSerializer");
        properties.setProperty("value.serializer","org.apache.kafka.common.serialization.StringSerializer");
        KafkaProducer<String, String> producer = new KafkaProducer<>(properties);

        // So we can generate random sentences
        Random random = new Random();
        String[] sentences = new String[] {
                "the cow jumped over the moon",
                "an apple a day keeps the doctor away",
                "four score and seven years ago",
                "snow white and the seven dwarfs",
                "i am at two with nature"
        };

        String progressAnimation = "|/-\\";
        // Produce a bunch of records
        for(int i = 0; i < 1000000; i++) {
            // Pick a sentence at random
            String sentence = sentences[random.nextInt(sentences.length)];
            // Send the sentence to the test topic
            producer.send(new ProducerRecord<String, String>("test", sentence));
            String progressBar = "\r" + progressAnimation.charAt(i % progressAnimation.length()) + " " + i;
            System.out.write(progressBar.getBytes());
        }
    }
}
```

Dieser Code stellt die Verbindung mit den Kafka-Brokerhosts (Workerknoten) her und sendet dann 1.000.000 Sätze mithilfe der Producer-API an Kafka.

### <a name="consumerjava"></a>Consumer.java

Der Consumer kommuniziert mit den Kafka-Brokerhosts (Workerknoten) und liest Datensätze in eine Schleife ein. Der folgende Codeausschnitt ist ein Beispiel aus der Datei `Consumer.java`:

```java
package com.microsoft.example;

import org.apache.kafka.clients.consumer.KafkaConsumer;
import org.apache.kafka.clients.consumer.ConsumerRecords;
import org.apache.kafka.clients.consumer.ConsumerRecord;
import java.util.Properties;
import java.util.Arrays;

public class Consumer {
    public static void consume(String brokers, String groupId) {
        // Create a consumer
        KafkaConsumer<String, String> consumer;
        // Configure the consumer
        Properties properties = new Properties();
        // Point it to the brokers
        properties.setProperty("bootstrap.servers", brokers);
        // Set the consumer group (all consumers must belong to a group).
        properties.setProperty("group.id", groupId);
        // Set how to serialize key/value pairs
        properties.setProperty("key.deserializer","org.apache.kafka.common.serialization.StringDeserializer");
        properties.setProperty("value.deserializer","org.apache.kafka.common.serialization.StringDeserializer");
        // When a group is first created, it has no offset stored to start reading from. This tells it to start
        // with the earliest record in the stream.
        properties.setProperty("auto.offset.reset","earliest");
        consumer = new KafkaConsumer<>(properties);

        // Subscribe to the 'test' topic
        consumer.subscribe(Arrays.asList("test"));

        // Loop until ctrl + c
        int count = 0;
        while(true) {
            // Poll for records
            ConsumerRecords<String, String> records = consumer.poll(200);
            // Did we get any?
            if (records.count() == 0) {
                // timeout/nothing to read
            } else {
                // Yes, loop over records
                for(ConsumerRecord<String, String> record: records) {
                    // Display record and count
                    count += 1;
                    System.out.println( count + ": " + record.value());
                }
            }
        }
    }
}
```

In diesem Code ist der Consumer so konfiguriert, dass er vom Beginn des Themas an liest (`auto.offset.reset` ist auf `earliest` festgelegt.)

### <a name="runjava"></a>Run.java

Die Datei `Run.java` bietet eine Befehlszeilenschnittstelle, die entweder den Producer- oder Consumer-Code ausführt. Sie müssen die Kafka-Brokerhostinformationen als Parameter angeben. Sie können optional einen Gruppen-ID-Wert einschließen, der vom Consumer-Prozess verwendet wird. Wenn Sie mehrere Consumer-Instanzen mit derselben Gruppen-ID erstellen, wird beim Lesen aus dem Thema ein Lastenausgleich zwischen ihnen durchgeführt.

```java
package com.microsoft.example;

import java.io.IOException;
import java.util.UUID;

// Handle starting producer or consumer
public class Run {
    public static void main(String[] args) throws IOException {
        if(args.length < 2) {
            usage();
        }

        // Get the brokers
        String brokers = args[1];
        switch(args[0].toLowerCase()) {
            case "producer":
                Producer.produce(brokers);
                break;
            case "consumer":
                // Either a groupId was passed in, or we need a random one
                String groupId;
                if(args.length == 3) {
                    groupId = args[2];
                } else {
                    groupId = UUID.randomUUID().toString();
                }
                Consumer.consume(brokers, groupId);
                break;
            default:
                usage();
        }
        System.exit(0);
    }
    // Display usage
    public static void usage() {
        System.out.println("Usage:");
        System.out.println("kafka-example.jar <producer|consumer> brokerhosts [groupid]");
        System.exit(1);
    }
}
```

## <a name="build-and-deploy-the-example"></a>Erstellen und Bereitstellen des Beispiels

1. Laden Sie die Beispiele von [ https://github.com/Azure-Samples/hdinsight-kafka-java-get-started ](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started) herunter.

2. Wechseln Sie zum Verzeichnis `Producer-Consumer`, und führen Sie folgenden Befehl aus:

    ```
    mvn clean package
    ```

    Mit diesem Befehl wird ein Verzeichnis mit dem Namen `target` erstellt, das eine Datei namens `kafka-producer-consumer-1.0-SNAPSHOT.jar` enthält.

3. Verwenden Sie die folgenden Befehle, um die Datei `kafka-producer-consumer-1.0-SNAPSHOT.jar` in Ihren HDInsight-Cluster zu kopieren:
   
    ```bash
    scp ./target/kafka-producer-consumer-1.0-SNAPSHOT.jar SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net:kafka-producer-consumer.jar
    ```
   
    Ersetzen Sie **SSHUSER** durch den SSH-Benutzer für Ihren Cluster, und ersetzen Sie **CLUSTERNAME** durch den Namen Ihres Clusters. Geben Sie nach Aufforderung das Kennwort für den SSH-Benutzer ein.

## <a id="run"></a>Ausführen des Beispiels

1. Um eine SSH-Verbindung mit dem Cluster zu öffnen, führen Sie folgenden Befehl aus:

    ```bash
    ssh SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    Ersetzen Sie **SSHUSER** durch den SSH-Benutzer für Ihren Cluster, und ersetzen Sie **CLUSTERNAME** durch den Namen Ihres Clusters. Geben Sie bei entsprechender Aufforderung das Kennwort für das SSH-Benutzerkonto ein. Weitere Informationen zum Verwenden von `scp` mit HDInsight finden Sie unter [Verwenden von SSH mit HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. Um die von diesem Beispiel verwendeten Kafka-Themen zu erstellen, führen Sie die folgenden Schritte aus:

    1. Um den Clusternamen in einer Variablen zu speichern und ein JSON-Analysehilfsprogramm (`jq`) zu installieren, verwenden Sie die folgenden Befehle. Geben Sie bei entsprechender Aufforderung den Namen des Kafka-Clusters ein:
    
        ```bash
        sudo apt -y install jq
        read -p 'Enter your Kafka cluster name:' CLUSTERNAME
        ```
    
    2. Um die Kafka-Brokerhosts und die Zookeeper-Hosts abzurufen, verwenden Sie die folgenden Befehle. Geben Sie bei der entsprechenden Aufforderung das Kennwort des Anmeldekontos (Administrator) für den Cluster ein.
    
        ```bash
        export KAFKAZKHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`; \
        export KAFKABROKERS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`; \
        ```

    3. Um das `test`-Thema zu erstellen, verwenden Sie den folgenden Befehl:

        ```bash
        /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic test --zookeeper $KAFKAZKHOSTS
        ```
    4. Sie können zum Erstellen eines Themas auch die JAR-Datei verwenden. Um das `test2`-Thema zu erstellen, verwenden Sie etwa den folgenden Befehl:

        ```bash
        java -jar kafka-producer-consumer.jar create test2 $KAFKABROKERS
        ```

3. Um den Producer auszuführen und Daten in das Thema zu schreiben, führen Sie folgenden Befehl aus:

    ```bash
    java -jar kafka-producer-consumer.jar producer test $KAFKABROKERS
    ```

4. Wenn der Producer abgeschlossen ist, führen Sie folgenden Befehl aus, um Daten aus dem Thema zu lesen:
   
    ```bash
    java -jar kafka-producer-consumer.jar consumer test $KAFKABROKERS
    ```
   
    Die gelesenen Datensätze und die Anzahl von Datensätzen wird angezeigt.

5. Drücken Sie __STRG+C__, um den Consumer zu beenden.

### <a name="multiple-consumers"></a>Mehrere Consumer

Kafka-Consumer verwenden beim Lesen von Datensätzen eine Consumergruppe. Das Verwenden derselben Gruppe mit mehreren Consumern führt zu Lesevorgängen mit Lastenausgleich aus einem Thema. Jeder Consumer in der Gruppe erhält einen Teil der Datensätze.

Die Consumer-Anwendung akzeptiert einen Parameter, der als Gruppen-ID verwendet wird. Der folgende Befehl beispielsweise startet einen Consumer mit der Gruppen-ID `mygroup`:
   
```bash
java -jar kafka-producer-consumer.jar consumer test $KAFKABROKERS mygroup
```

Führen Sie den folgenden Befehl aus, um diesen Vorgang in Aktion zu sehen:

```bash
tmux new-session 'java -jar kafka-producer-consumer.jar consumer test $KAFKABROKERS mygroup' \; split-w
indow -h 'java -jar kafka-producer-consumer.jar consumer test $KAFKABROKERS mygroup' \; attach
```

Dieser Befehl teilt das Terminal mit `tmux` in zwei Spalten auf. In jeder Spalte wird ein Consumer mit dem gleichen Gruppen-ID-Wert gestartet. Beachten Sie nach Abschluss des Lesens durch die Consumer, dass jeder nur einen Teil der Datensätze gelesen hat. Verwenden Sie __Strg + C __ zweimal, um `tmux` zu beenden.

Der Verbrauch durch die Clients in derselben Gruppe wird über die Partitionen für das Thema gesteuert. Für das zuvor erstellte Thema `test` sind acht Partitionen vorhanden. Wenn Sie acht Consumer starten, liest jeder Consumer Datensätze aus einer einzelnen Partition für das Thema.

> [!IMPORTANT]
> Eine Consumergruppe kann nicht mehr Consumerinstanzen als Partitionen enthalten. In diesem Beispiel kann eine Consumergruppe bis zu acht Consumer enthalten, da dies die Anzahl von Partitionen im Thema ist. Sie können auch mehrere Consumergruppen verwenden, die jeweils nicht mehr als acht Consumer enthalten.

Datensätze werden in Kafka in der Reihenfolge gespeichert, in der sie in einer Partition empfangen werden. Erstellen Sie eine Consumergruppe, bei der die Anzahl von Consumerinstanzen mit der Anzahl von Partitionen übereinstimmt, um für Datensätze *in einer Partition* eine geordnete Bereitstellung zu erzielen. Erstellen Sie eine Consumergruppe mit nur einer Consumerinstanz, um für Datensätze *im Thema* eine geordnete Bereitstellung zu erzielen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument haben Sie erfahren, wie Sie die Apache Kafka Producer- und Consumer-APIs mit Kafka in HDInsight verwenden. Verwenden Sie Folgendes, um weitere Informationen zur Verwendung von Kafka zu erhalten:

* [Analysieren von Kafka-Protokollen](apache-kafka-log-analytics-operations-management.md)
* [Replizieren von Daten zwischen Kafka-Clustern](apache-kafka-mirroring.md)
* [Kafka Streams-API mit HDInsight](apache-kafka-streams-api.md)
