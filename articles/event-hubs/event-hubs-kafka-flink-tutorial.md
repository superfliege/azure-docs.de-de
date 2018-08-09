---
title: Verwenden von Apache Flink mit Azure Event Hubs für Apache Kafka | Microsoft-Dokumentation
description: Verbindung von Apache Flink mit einem Kafka-fähigen Event Hub
services: event-hubs
documentationcenter: ''
author: basilhariri
manager: timlt
ms.service: event-hubs
ms.topic: article
ms.custom: mvc
ms.date: 06/06/2018
ms.author: bahariri
ms.openlocfilehash: ce1665c3cfd58d0d5aa8e253b5db317505b1959e
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/27/2018
ms.locfileid: "39284576"
---
# <a name="use-apache-flink-with-azure-event-hubs-for-apache-kafka"></a>Verwenden von Apache Flink mit Azure Event Hubs für Apache Kafka

Einer der Hauptvorteile der Verwendung von Apache Kafka ist das Ökosystem an Frameworks, mit dem eine Verbindung hergestellt werden kann. Kafka-fähige Event Hubs kombiniert die Flexibilität von Kafka mit der Skalierbarkeit, Konsistenz und Unterstützung des Azure-Ökosystems.

Dieses Tutorial veranschaulicht, wie Sie Apache Flink mit Kafka-fähigen Event Hubs verbinden können, ohne Ihre Protokollclients ändern oder Ihre eigenen Cluster ausführen zu müssen. Azure Event Hubs unterstützt [Apache Kafka Version 1.0](https://kafka.apache.org/10/documentation.html).

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie vor dem Durchführen dieses Tutorials sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Ein Azure-Abonnement. Wenn Sie keins besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) erstellen, bevor Sie beginnen.
* [Java Development Kit (JDK) 1.7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
    * Führen Sie unter Ubuntu `apt-get install default-jdk` aus, um das JDK zu installieren.
    * Achten Sie darauf, dass die Umgebungsvariable „JAVA_HOME“ auf den Ordner verweist, in dem das JDK installiert ist.
* Führen Sie den [Download](http://maven.apache.org/download.cgi) und die [Installation](http://maven.apache.org/install.html) eines binären Maven-Archivs durch.
    * Unter Ubuntu können Sie `apt-get install maven` ausführen, um Maven zu installieren.
* [Git-Client](https://www.git-scm.com/downloads)
    * Unter Ubuntu können Sie `sudo apt-get install git` ausführen, um Git zu installieren.

## <a name="create-an-event-hubs-namespace"></a>Erstellen eines Event Hubs-Namespace

Ein Event Hubs-Namespace ist erforderlich, um Nachrichten an einen Event Hubs-Dienst zu senden oder von diesem zu empfangen. Informationen zum Abrufen eines Kafka-fähigen Event Hubs-Endpunkts finden Sie unter [Create a Kafka Enabled Event Hub](event-hubs-create-kafka-enabled.md) (Erstellen eines Kafka-fähigen Event Hub). Kopieren Sie die Event Hubs-Verbindungszeichenfolge zur späteren Verwendung.

## <a name="clone-the-example-project"></a>Klonen des Beispielprojekts

Nachdem Sie nun über eine Kafka-fähige Event Hubs-Verbindungszeichenfolge verfügen, können Sie das Azure Event Hubs-Repository klonen und zum Unterordner `flink` navigieren:

```shell
git clone https://github.com/Azure/azure-event-hubs.git
cd azure-event-hubs/samples/kafka/flink
```

## <a name="flink-producer"></a>Flink-Producer

Verwenden Sie das bereitgestellte Beispiel des Flink-Producers, und senden Sie Nachrichten an den Event Hubs-Dienst.

### <a name="provide-an-event-hubs-kafka-endpoint"></a>Bereitstellen eines Kafka-fähigen Event Hubs-Endpunkts

#### <a name="producerconfig"></a>producer.config

Update der `bootstrap.servers`- und `sasl.jaas.config`-Werte in `producer/src/main/resources/producer.config` zur Weiterleitung des Producers an den Kafka-fähigen Event Hubs-Endpunkt mit der richtigen Authentifizierung.

```xml
bootstrap.servers={YOUR.EVENTHUBS.FQDN}:9093
client.id=FlinkExampleProducer
sasl.mechanism=PLAIN
security.protocol=SASL_SSL
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required \
   username="$ConnectionString" \
   password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
```

### <a name="run-producer-from-the-command-line"></a>Ausführen des Producers über die Befehlszeile

Zum Ausführen des Producers über die Befehlszeile generieren Sie die JAR-Datei. Führen Sie sie innerhalb von Maven aus (oder generieren Sie die JAR-Datei mit Maven, und führen Sie sie dann durch Hinzufügen der erforderlichen Kafka-JAR(s) an den Klassenpfad in Java aus):

```shell
mvn clean package
mvn exec:java -Dexec.mainClass="FlinkTestProducer"
```

Der Producer beginnt mit dem Senden von Ereignissen an den Kafka-fähigen Event Hub zu Thema `test` und dem Drucken der Ereignisse an „stdout“.

## <a name="flink-consumer"></a>Flink-Consumer

Verwenden Sie das bereitgestellte Consumer-Beispiel, und empfangen Sie Nachrichten aus dem Kafka-fähigen Event Hubs-Dienst.

### <a name="provide-an-event-hubs-kafka-endpoint"></a>Bereitstellen eines Kafka-fähigen Event Hubs-Endpunkts

#### <a name="consumerconfig"></a>consumer.config

Aktualisieren Sie die `bootstrap.servers`- und `sasl.jaas.config`-Werte in `consumer/src/main/resources/consumer.config` zur Weiterleitung des Consumers an den Kafka-fähigen Event Hubs-Endpunkt mit der richtigen Authentifizierung.

```xml
bootstrap.servers={YOUR.EVENTHUBS.FQDN}:9093
group.id=FlinkExampleConsumer
sasl.mechanism=PLAIN
security.protocol=SASL_SSL
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required \
   username="$ConnectionString" \
   password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
```

### <a name="run-consumer-from-the-command-line"></a>Ausführen des Consumers über die Befehlszeile

Zum Ausführen des Consumers über die Befehlszeile generieren Sie die JAR-Datei, und führen Sie sie innerhalb von Maven aus (oder generieren Sie die JAR-Datei mit Maven, und führen Sie sie dann durch Hinzufügen der erforderlichen Kafka-JAR(s) an den Klassenpfad in Java aus):

```shell
mvn clean package
mvn exec:java -Dexec.mainClass="FlinkTestConsumer"
```

Wenn der Kafka-fähige Event Hub über Ereignisse verfügt (z.B., wenn der Producer auch ausgeführt wird), erhält der Consumer jetzt Ereignisse aus dem Thema `test`.

Weitere Informationen zur Verbindung von Flink und Kafka finden Sie unter [Flink's Kafka Connector Guide (Flinks Leitfaden zur Kafka-Verbindung)](https://ci.apache.org/projects/flink/flink-docs-stable/dev/connectors/kafka.html).

## <a name="next-steps"></a>Nächste Schritte

* [Informationen zu Event Hubs](event-hubs-what-is-event-hubs.md)
* [Informationen zu Event Hubs für Kafka](event-hubs-for-kafka-ecosystem-overview.md)
* Verwenden Sie [MirrorMaker](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) zum [Streamen von Ereignissen aus der lokalen Kafka-Instanz an Kafka-fähige Event Hubs in der Cloud](event-hubs-kafka-mirror-maker-tutorial.md).
* Erfahren Sie, wie Sie [native Kafka-Anwendungen](event-hubs-quickstart-kafka-enabled-event-hubs.md) oder [Akka Streams](event-hubs-kafka-akka-streams-tutorial.md) zum Streamen an Kafka-fähige Event Hubs verwenden.
