---
title: 'Tutorial: Verwenden der Apache Kafka Producer- und Consumer-APIs – Azure HDInsight '
description: Erfahren Sie, wie Sie die Apache Kafka Producer- und Consumer-APIs mit Kafka in HDInsight verwenden. In diesem Tutorial erfahren Sie, wie Sie diese APIs mit Kafka auf HDInsight aus einer Java-Anwendung verwenden.
services: hdinsight
author: dhgoelmsft
ms.author: dhgoel
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: tutorial
ms.date: 11/06/2018
ms.openlocfilehash: 556fc1f04cc6a1d1b594bdd3787ed43d30f607c6
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58091170"
---
# <a name="tutorial-use-the-apache-kafka-producer-and-consumer-apis"></a>Tutorial: Verwenden der Apache Kafka Producer- und Consumer-APIs

Erfahren Sie, wie Sie die Apache Kafka Producer- und Consumer-APIs mit Kafka in HDInsight verwenden.

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

* [Apache Maven](https://maven.apache.org/)

* Ein SSH-Client und der Befehl `scp`. Weitere Informationen finden Sie im Dokument [Herstellen einer Verbindung mit HDInsight (Hadoop) per SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).

* Ein Text-Editor oder eine Java-IDE.

Bei der Installation von Java und dem JDK auf Ihrer Entwicklungsworkstation können die folgenden Umgebungsvariablen festgelegt werden. Sie sollten dennoch prüfen, ob die Variablen vorhanden sind und korrekte Werte für Ihr System enthalten.

* `JAVA_HOME` – sollte auf das Verzeichnis verweisen, in dem das JDK installiert ist.
* `PATH` – sollte die folgenden Pfade enthalten:

    * `JAVA_HOME` (oder entsprechender Pfad)
    * `JAVA_HOME\bin` (oder entsprechender Pfad)
    * Das Verzeichnis, in dem Maven installiert ist

## <a name="set-up-your-deployment-environment"></a>Einrichten Ihrer Bereitstellungsumgebung

Für dieses Tutorial benötigen Sie Apache Kafka in HDInsight 3.6. Informationen zum Erstellen eines Clusters für Kafka in HDInsight finden Sie im Dokument [Schnellstart: Erstellen eines Apache Kafka-Clusters in HDInsight](apache-kafka-get-started.md).

## <a name="understand-the-code"></a>Grundlegendes zum Code

Die exemplarische Anwendung befindet sich unter [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started) im `Producer-Consumer`-Unterverzeichnis. Die Anwendung besteht im Wesentlichen aus vier Dateien:

* `pom.xml`: Diese Datei definiert die Projektabhängigkeiten, Java-Version und Verpackungsmethoden.
* `Producer.java`: Diese Datei sendet mithilfe der Producer-API willkürliche Sätze an Kafka.
* `Consumer.java`: Diese Datei verwendet die Consumer-API zum Lesen von Daten aus Kafka und Ausgeben an STDOUT.
* `Run.java`: Die Befehlszeilenschnittstelle zum Ausführen des Producer- und Consumer-Codes.

### <a name="pomxml"></a>Pom.Xml

Wichtige Informationen zur `pom.xml`-Datei:

* Abhängigkeiten: Dieses Projekt benötigt die Kafka Producer- und Consumer-APIs, die durch das `kafka-clients`-Paket bereitgestellt werden. Der folgende XML-Code definiert diese Abhängigkeit:

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

    * `maven-compiler-plugin`: Wird verwendet, um die vom Projekt verwendete Java-Version auf 8 festzulegen. Dies ist die von HDInsight 3.6 verwendete Java-Version.
    * `maven-shade-plugin`: Wird zum Generieren einer Uber-JAR-Datei verwendet, die diese Anwendung sowie alle Abhängigkeiten enthält. Es wird auch zum Festlegen des Einstiegspunkts der Anwendung verwendet, damit Sie die JAR-Datei direkt ausführen können, ohne die Hauptklasse angeben zu müssen.

### <a name="producerjava"></a>Producer.java

Der Producer kommuniziert mit den Kafka-Brokerhosts (Workerknoten) und sendet Daten an ein Kafka-Thema. Der folgende Codeausschnitt stammt aus der Datei [Producer.java](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started/blob/master/Producer-Consumer/src/main/java/com/microsoft/example/Producer.java) aus dem [GitHub-Repository](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started) und zeigt, wie die Producereigenschaften festgelegt werden:

```java
Properties properties = new Properties();
// Set the brokers (bootstrap servers)
properties.setProperty("bootstrap.servers", brokers);
// Set how to serialize key/value pairs
properties.setProperty("key.serializer","org.apache.kafka.common.serialization.StringSerializer");
properties.setProperty("value.serializer","org.apache.kafka.common.serialization.StringSerializer");
KafkaProducer<String, String> producer = new KafkaProducer<>(properties);
```

### <a name="consumerjava"></a>Consumer.java

Der Consumer kommuniziert mit den Kafka-Brokerhosts (Workerknoten) und liest Datensätze in eine Schleife ein. Der folgende Codeausschnitt aus der Datei [Consumer.java](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started/blob/master/Producer-Consumer/src/main/java/com/microsoft/example/Consumer.java) legt die Consumereigenschaften fest:

```java
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
```

In diesem Code ist der Consumer so konfiguriert, dass er vom Beginn des Themas an liest (`auto.offset.reset` ist auf `earliest` festgelegt.)

### <a name="runjava"></a>Run.java

Die Datei [Run.java](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started/blob/master/Producer-Consumer/src/main/java/com/microsoft/example/Run.java) bietet eine Befehlszeilenschnittstelle, die entweder den Producer- oder Consumer-Code ausführt. Sie müssen die Kafka-Brokerhostinformationen als Parameter angeben. Sie können optional einen Gruppen-ID-Wert einschließen, der vom Consumer-Prozess verwendet wird. Wenn Sie mehrere Consumer-Instanzen mit derselben Gruppen-ID erstellen, wird beim Lesen aus dem Thema ein Lastenausgleich zwischen ihnen durchgeführt.

## <a name="build-and-deploy-the-example"></a>Erstellen und Bereitstellen des Beispiels

Sie können die Schritte 1 und 2 für den Build überspringen und die vorkonfigurierte JAR-Datei („kafka-producer-consumer.jar“) von [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started/tree/master/Prebuilt-Jars](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started/tree/master/Prebuilt-Jars) herunterladen. Kopieren Sie diese JAR-Datei anschließend in Ihren HDInsight-Cluster.

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
    
    2. Um die Kafka-Brokerhosts und die Apache Zookeeper-Hosts abzurufen, verwenden Sie die folgenden Befehle. Geben Sie bei der entsprechenden Aufforderung das Kennwort des Anmeldekontos (Administrator) für den Cluster ein.
    
        ```bash
        export KAFKABROKERS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`; \
        ```

    3. Um das `test`-Thema zu erstellen, verwenden Sie den folgenden Befehl:

        ```bash
        java -jar kafka-producer-consumer.jar create test $KAFKABROKERS
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

Dieser Befehl teilt das Terminal mit `tmux` in zwei Spalten auf. In jeder Spalte wird ein Consumer mit dem gleichen Gruppen-ID-Wert gestartet. Beachten Sie nach Abschluss des Lesens durch die Consumer, dass jeder nur einen Teil der Datensätze gelesen hat. Drücken Sie zweimal __STRG+C__, um `tmux` zu beenden.

Der Verbrauch durch die Clients in derselben Gruppe wird über die Partitionen für das Thema gesteuert. In diesem Codebeispiel besitzt das zuvor erstellte Thema `test` acht Partitionen. Wenn Sie acht Consumer starten, liest jeder Consumer Datensätze aus einer einzelnen Partition für das Thema.

> [!IMPORTANT]  
> Eine Consumergruppe kann nicht mehr Consumerinstanzen als Partitionen enthalten. In diesem Beispiel kann eine Consumergruppe bis zu acht Consumer enthalten, da dies die Anzahl von Partitionen im Thema ist. Sie können auch mehrere Consumergruppen verwenden, die jeweils nicht mehr als acht Consumer enthalten.

Datensätze werden in Kafka in der Reihenfolge gespeichert, in der sie in einer Partition empfangen werden. Erstellen Sie eine Consumergruppe, bei der die Anzahl von Consumerinstanzen mit der Anzahl von Partitionen übereinstimmt, um für Datensätze *in einer Partition* eine geordnete Bereitstellung zu erzielen. Erstellen Sie eine Consumergruppe mit nur einer Consumerinstanz, um für Datensätze *im Thema* eine geordnete Bereitstellung zu erzielen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument haben Sie erfahren, wie Sie die Apache Kafka Producer- und Consumer-APIs mit Kafka in HDInsight verwenden. Verwenden Sie Folgendes, um weitere Informationen zur Verwendung von Kafka zu erhalten:

* [Analysieren von Apache Kafka-Protokollen](apache-kafka-log-analytics-operations-management.md)
* [Replizieren von Daten zwischen Apache Kafka-Clustern](apache-kafka-mirroring.md)
* [Apache Kafka Streams-API mit HDInsight](apache-kafka-streams-api.md)
