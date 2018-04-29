---
title: Verwenden der Apache Kafka Producer- und Consumer-APIs – Azure HDInsight | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Apache Kafka Producer- und Consumer-APIs mit Kafka in HDInsight verwenden. Mit diesen APIs können Sie Anwendungen entwickeln, die in Apache Kafka schreiben und lesen.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/10/2018
ms.author: larryfr
ms.openlocfilehash: 01592401c4c88adeed49b11df4e7963e27b1bcee
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="apache-kafka-producer-and-consumer-apis"></a>Apache Kafka Producer- und Consumer-APIs

Erfahren Sie, wie Sie eine Anwendung erstellen, die die Apache Kafka Producer- und Consumer-APIs mit Kafka in HDInsight verwendet.

Eine Dokumentation der APIs finden Sie unter [Producer-API](https://kafka.apache.org/documentation/#producerapi) und [Consumer-API](https://kafka.apache.org/documentation/#consumerapi).

## <a name="set-up-your-development-environment"></a>Einrichten der Entwicklungsumgebung

Die folgenden Komponenten müssen in der Entwicklungsumgebung installiert sein:

* [Java JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) oder ein gleichwertiges Kit, z.B. OpenJDK.

* [Apache Maven](http://maven.apache.org/)

* Ein SSH-Client und der Befehl `scp`. Weitere Informationen finden Sie im Dokument [Herstellen einer Verbindung mit HDInsight (Hadoop) per SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="set-up-your-deployment-environment"></a>Einrichten Ihrer Bereitstellungsumgebung

Dieses Beispiel erfordert Kafka in HDInsight 3.6. Informationen zum Erstellen eines Clusters für Kafka in HDInsight finden Sie im Dokument [Einstieg in Apache Kafka in HDInsight](apache-kafka-get-started.md).

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

2. Um die von diesem Beispiel verwendeten Kafka-Themen zu erstellen, führen Sie die folgenden Befehle aus:

    ```bash
    sudo apt -y install jq

    CLUSTERNAME='your cluster name'

    export KAFKAZKHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`

    export KAFKABROKERS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`

    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic test --zookeeper $KAFKAZKHOSTS
    ```

    Ersetzen Sie __your cluster name__ durch den Namen Ihres HDInsight-Clusters. Geben Sie bei entsprechender Aufforderung das Kennwort des Anmeldekontos für den HDInsight-Cluster ein.

    > [!NOTE]
    > Wenn Ihre Clusteranmeldung vom Standardwert `admin` abweicht, ersetzen Sie den `admin`-Wert in den vorherigen Befehlen durch den Anmeldenamen Ihres Clusters.

3. Um den Producer auszuführen und Daten in das Thema zu schreiben, führen Sie folgenden Befehl aus:

    ```bash
    java -jar kafka-producer-consumer.jar producer $KAFKABROKERS
    ```

4. Wenn der Producer abgeschlossen ist, führen Sie folgenden Befehl aus, um Daten aus dem Thema zu lesen:
   
    ```bash
    java -jar kafka-producer-consumer.jar consumer $KAFKABROKERS
    ```
   
    Die gelesenen Datensätze und die Anzahl von Datensätzen wird angezeigt.

5. Drücken Sie __STRG+C__, um den Consumer zu beenden.

### <a name="multiple-consumers"></a>Mehrere Consumer

Kafka-Consumer verwenden beim Lesen von Datensätzen eine Consumergruppe. Das Verwenden derselben Gruppe mit mehreren Consumern führt zu Lesevorgängen mit Lastenausgleich aus einem Thema. Jeder Consumer in der Gruppe erhält einen Teil der Datensätze.

Die Consumer-Anwendung akzeptiert einen Parameter, der als Gruppen-ID verwendet wird. Der folgende Befehl beispielsweise startet einen Consumer mit der Gruppen-ID `mygroup`:
   
```bash
java -jar kafka-producer-consumer.jar consumer $KAFKABROKERS mygroup
```

Führen Sie die folgenden Schritte aus, um diesen Vorgang in Aktion zu sehen:

1. Wiederholen Sie die Schritt 1 und 2 im obigen Abschnitt [Ausführen des Beispiels](#run), um eine neue SSH-Sitzung zu öffnen.

2. Geben Sie in beiden SSH-Sitzungen folgenden Befehl ein:

    ```bash
    java -jar kafka-producer-consumer.jar consumer $KAFKABROKERS mygroup
    ```
    
    > [!IMPORTANT]
    > Geben Sie beide Befehle gleichzeitig ein, sodass beide zur gleichen Zeit ausgeführt werden.

    Beachten Sie, dass die Anzahl von Lesevorgängen für Nachrichten nicht die gleiche ist wie im vorherigen Abschnitt, in dem Sie nur einen Consumer verwendet haben. Stattdessen werden die Lesevorgänge für Nachrichten zwischen den Instanzen aufgeteilt.

Der Verbrauch durch die Clients in derselben Gruppe wird über die Partitionen für das Thema gesteuert. Für das zuvor erstellte Thema `test` sind acht Partitionen vorhanden. Wenn Sie acht SSH-Sitzungen öffnen und einen Consumer in allen Sitzungen starten, liest jeder Consumer Datensätze aus einer einzelnen Partition für das Thema aus.

> [!IMPORTANT]
> Eine Consumergruppe kann nicht mehr Consumerinstanzen als Partitionen enthalten. In diesem Beispiel kann eine Consumergruppe bis zu acht Consumer enthalten, da dies die Anzahl von Partitionen im Thema ist. Sie können auch mehrere Consumergruppen verwenden, die jeweils nicht mehr als acht Consumer enthalten.

Datensätze werden in Kafka in der Reihenfolge gespeichert, in der sie in einer Partition empfangen werden. Erstellen Sie eine Consumergruppe, bei der die Anzahl von Consumerinstanzen mit der Anzahl von Partitionen übereinstimmt, um für Datensätze *in einer Partition* eine geordnete Bereitstellung zu erzielen. Erstellen Sie eine Consumergruppe mit nur einer Consumerinstanz, um für Datensätze *im Thema* eine geordnete Bereitstellung zu erzielen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument haben Sie erfahren, wie Sie die Apache Kafka Producer- und Consumer-APIs mit Kafka in HDInsight verwenden. Verwenden Sie Folgendes, um weitere Informationen zur Verwendung von Kafka zu erhalten:

* [Analysieren von Kafka-Protokollen](apache-kafka-log-analytics-operations-management.md)
* [Replizieren von Daten zwischen Kafka-Clustern](apache-kafka-mirroring.md)
* [Kafka Streams-API mit HDInsight](apache-kafka-streams-api.md)
* [Verwenden von Apache Spark-Streaming (DStream) mit Kafka in HDInsight](../hdinsight-apache-spark-with-kafka.md)
* [Verwenden von strukturiertem Spark-Streaming mit Kafka in HDInsight](../hdinsight-apache-kafka-spark-structured-streaming.md)
* [Verwenden von strukturiertem Apache Spark-Streaming zum Verschieben von Daten aus Kafka in HDInsight nach Cosmos DB](../apache-kafka-spark-structured-streaming-cosmosdb.md)
* [Verwenden von Apache Storm mit Kafka in HDInsight](../hdinsight-apache-storm-with-kafka.md)
* [Herstellen einer Verbindung mit Kafka über eine Azure Virtual Network-Instanz](apache-kafka-connect-vpn-gateway.md)