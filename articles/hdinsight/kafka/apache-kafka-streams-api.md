---
title: "Verwenden der Apache Kafka Streams-API – Azure HDInsight | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie die Apache Kafka Streams-API mit Kafka in HDInsight verwenden. Mit dieser API können Sie eine Streamverarbeitung zwischen Themen in Kafka ausführen."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/19/2018
ms.author: larryfr
ms.openlocfilehash: be6ed6d4c0c3a5fa55166b84b128881d434c4ab2
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/09/2018
---
# <a name="apache-kafka-streams-api"></a>Apache Kafka Streams-API

Erfahren Sie, wie Sie eine Anwendung erstellen, die die Apache Kafka Streams-API verwendet, und diese mit Kafka in HDInsight ausführen.

Beim Arbeiten mit Apache Kafka erfolgt die Streamverarbeitung häufig über Apache Spark oder Storm. Die Kafka Streams-API wurde in Kafka Version 0.10.0 (in HDInsight 3.5 und 3.6) eingeführt. Mit dieser API können Sie Datenstreams mithilfe einer Anwendung, die auf Kafka ausgeführt wird, zwischen Eingabe- und Ausgabethemen transformieren. In einigen Fällen kann dies eine Alternative zum Erstellen einer Spark- oder Storm-Streaminglösung sein. Weitere Informationen zu Kafka Streams finden Sie in der Dokumentation [Intro to Streams](https://kafka.apache.org/10/documentation/streams/) (Einführung in Streams) auf Apache.org.

## <a name="set-up-your-development-environment"></a>Einrichten der Entwicklungsumgebung

Die folgenden Komponenten müssen in der Entwicklungsumgebung installiert sein:

* [Java JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) oder ein gleichwertiges Kit, z.B. OpenJDK.

* [Apache Maven](http://maven.apache.org/)

* Ein SSH-Client und der Befehl `scp`. Weitere Informationen finden Sie im Dokument [Herstellen einer Verbindung mit HDInsight (Hadoop) per SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="set-up-your-deployment-environment"></a>Einrichten Ihrer Bereitstellungsumgebung

Dieses Beispiel erfordert Kafka in HDInsight 3.6. Informationen zum Erstellen eines Clusters für Kafka in HDInsight finden Sie im Dokument [Einstieg in Apache Kafka in HDInsight](apache-kafka-get-started.md).

## <a name="build-and-deploy-the-example"></a>Erstellen und Bereitstellen des Beispiels

Führen Sie die folgenden Schritte aus, um das Projekt in Ihrem Cluster für Kafka in HDInsight zu erstellen und bereitzustellen.

1. Laden Sie die Beispiele unter [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started) herunter.

2. Wechseln Sie zum Verzeichnis `Streaming`, und erstellen Sie dann mit dem folgenden Befehl ein JAR-Paket:

    ```bash
    mvn clean package
    ```

    Mit diesem Befehl wird ein Verzeichnis mit dem Namen `target` erstellt, das eine Datei namens `kafka-streaming-1.0-SNAPSHOT.jar` enthält.

3. Verwenden Sie den folgenden Befehl, um die Datei `kafka-streaming-1.0-SNAPSHOT.jar` in Ihren HDInsight-Cluster zu kopieren:
   
    ```bash
    scp ./target/kafka-streaming-1.0-SNAPSHOT.jar SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net:kafka-streaming.jar
    ```
   
    Ersetzen Sie **SSHUSER** durch den SSH-Benutzer für Ihren Cluster, und ersetzen Sie **CLUSTERNAME** durch den Namen Ihres Clusters. Geben Sie bei entsprechender Aufforderung das Kennwort für das SSH-Benutzerkonto ein. Weitere Informationen zum Verwenden von `scp` mit HDInsight finden Sie unter [Verwenden von SSH mit HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="run-the-example"></a>Ausführen des Beispiels

1. Um eine SSH-Verbindung mit dem Cluster zu öffnen, führen Sie folgenden Befehl aus:

    ```bash
    ssh SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    Ersetzen Sie **SSHUSER** durch den SSH-Benutzer für Ihren Cluster, und ersetzen Sie **CLUSTERNAME** durch den Namen Ihres Clusters. Geben Sie bei entsprechender Aufforderung das Kennwort für das SSH-Benutzerkonto ein. Weitere Informationen zum Verwenden von `scp` mit HDInsight finden Sie unter [Verwenden von SSH mit HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

4. Um die von diesem Beispiel verwendeten Kafka-Themen zu erstellen, führen Sie die folgenden Befehle aus:

    ```bash
    sudo apt -y install jq

    CLUSTERNAME='your cluster name'

    export KAFKAZKHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`

    export KAFKABROKERS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`

    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic test --zookeeper $KAFKAZKHOSTS

    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic wordcounts --zookeeper $KAFKAZKHOSTS
    ```

    Ersetzen Sie __your cluster name__ durch den Namen Ihres HDInsight-Clusters. Geben Sie bei entsprechender Aufforderung das Kennwort des Anmeldekontos für den HDInsight-Cluster ein.

    > [!NOTE]
    > Wenn Ihre Clusteranmeldung vom Standardwert `admin` abweicht, ersetzen Sie den `admin`-Wert in den vorherigen Befehlen durch den Anmeldenamen Ihres Clusters.

5. Um dieses Beispiel auszuführen, müssen Sie zuvor Folgendes tun:

    * Starten Sie die in `kafka-streaming.jar` enthaltene Streams-Lösung.
    * Starten Sie einen Producer, der in das Thema `test` schreibt.
    * Starten Sie einen Consumer, sodass Sie die in das Thema `wordcounts` geschriebene Ausgabe anzeigen können.

    > [!NOTE]
    > Sie müssen sicherstellen, dass die Eigenschaft `auto.create.topics.enable` in der Kafka-Broker-Konfigurationsdatei auf `true` gesetzt ist. Diese Eigenschaft kann in der erweiterten Kafka-Broker-Konfigurationsdatei mithilfe der Ambari-Webbenutzeroberfläche angezeigt und geändert werden. Andernfalls müssen Sie das Zwischenthema `RekeyedIntermediateTopic` manuell erstellen, bevor Sie dieses Beispiel mit dem folgenden Befehl ausführen:
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic RekeyedIntermediateTopic  --zookeeper $KAFKAZKHOSTS
    ```
    
    Sie könnten diese drei Vorgänge in drei verschiedenen SSH-Sitzungen ausführen. Dann müssten Sie aber für jeden Vorgang `$KAFKABROKERS` und `$KAFKAZKHOSTS` festlegen, indem Sie in jeder SSH-Sitzung Schritt 4 dieses Abschnitts ausführen. Eine einfachere Lösung ist das Hilfsprogramm `tmux`, das die aktuelle SSH-Anzeige in mehrere Abschnitte unterteilen kann. Um Stream, Producer und Consumer mithilfe von `tmux` zu starten, führen Sie folgenden Befehl aus:

    ```bash
    tmux new-session '/usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKABROKERS --topic wordcounts --formatter kafka.tools.DefaultMessageFormatter --property print.key=true --property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer' \; split-window -h 'java -jar kafka-streaming.jar $KAFKABROKERS $KAFKAZKHOSTS' \; split-window -v '/usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $KAFKABROKERS --topic test' \; attach
    ```

    Dieser Befehl teilt die SSH-Anzeige in drei Abschnitte auf:

    * Im linken Abschnitt wird ein Konsolenconsumer ausgeführt, der Nachrichten aus dem Thema `wordcounts` liest: `/usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKABROKERS --topic wordcounts --formatter kafka.tools.DefaultMessageFormatter --property print.key=true --property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer`

        > [!NOTE]
        > Die `--property`-Parameter teilen dem Konsolenconsumer mit, dass der Schlüssel (Wort) zusammen mit der Anzahl (Wert) ausgegeben werden soll. Diese Parameter konfigurieren auch das Deserialisierungsprogramm, das beim Lesen dieser Werte aus Kafka verwendet werden soll.

    * Im oberen rechten Abschnitt wird die Streams-API-Lösung ausgeführt: `java -jar kafka-streaming.jar $KAFKABROKERS $KAFKAZKHOSTS`

    * Im unteren rechten Abschnitt wird der Konsolenproducer ausgeführt. Hier geben Sie Nachrichten ein, die an das Thema `test` gesendet werden sollen: `/usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $KAFKABROKERS --topic test`
 
6. Nachdem der `tmux`-Befehl die Anzeige unterteilt hat, befindet sich der Cursor im unteren rechten Abschnitt. Beginnen Sie mit der Eingabe von Sätzen. Nach jedem Satz wird der linke Abschnitt aktualisiert und zeigt die Anzahl eindeutiger Wörter. Die Ausgabe sieht in etwa wie folgender Text aus:
   
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
    > Die Anzahl wird jedes Mal erhöht, wenn ein Wort eingeht.

7. Drücken Sie __STRG+C__, um den Producer zu beenden. Drücken Sie __STRG+C__ erneut, um die Anwendung und den Consumer zu beenden.

## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument haben Sie erfahren, wie Sie die Kafka Streams-API mit Kafka in HDInsight verwenden. Verwenden Sie Folgendes, um weitere Informationen zur Verwendung von Kafka zu erhalten:

* [Analysieren von Kafka-Protokollen](apache-kafka-log-analytics-operations-management.md)
* [Replizieren von Daten zwischen Kafka-Clustern](apache-kafka-mirroring.md)
* [Kafka Producer- und Consumer-APIs mit HDInsight](apache-kafka-producer-consumer-api.md)
* [Verwenden von Apache Spark-Streaming (DStream) mit Kafka in HDInsight](../hdinsight-apache-spark-with-kafka.md)
* [Verwenden von strukturiertem Spark-Streaming mit Kafka in HDInsight](../hdinsight-apache-kafka-spark-structured-streaming.md)
* [Verwenden von strukturiertem Apache Spark-Streaming zum Verschieben von Daten aus Kafka in HDInsight nach Cosmos DB](../apache-kafka-spark-structured-streaming-cosmosdb.md)
* [Verwenden von Apache Storm mit Kafka in HDInsight](../hdinsight-apache-storm-with-kafka.md)
* [Herstellen einer Verbindung mit Kafka über eine Azure Virtual Network-Instanz](apache-kafka-connect-vpn-gateway.md)
