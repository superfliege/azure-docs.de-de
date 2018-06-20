---
title: Streamen von Ereignissen in Azure Event Hubs für das Kafka-Ökosystem | Microsoft-Dokumentation
description: Streamen von Ereignissen in Event Hubs für das Kafka-Protokoll und die APIs.
services: event-hubs
documentationcenter: ''
author: basilhariri
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/08/2018
ms.author: bahariri
ms.openlocfilehash: 8ef6240d19ce1ac1b891c95ce525a8bd211a2900
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2018
ms.locfileid: "35297222"
---
# <a name="stream-into-event-hubs-for-the-kafka-ecosystem"></a>Streamen an Event Hubs für das Kafka-Ökosystem

> [!NOTE]
> Dieses Beispiel ist auf [GitHub](https://github.com/Azure/azure-event-hubs) verfügbar.

In dieser Schnellstartanleitung erfahren Sie, wie Daten an Kafka-fähige Event Hubs gestreamt werden können, ohne Protokollclients zu ändern oder eigene Cluster zu betreiben. Sie lernen, wie Sie über Ihre Producer und Consumer mit Kafka-fähigen Event Hubs kommunizieren – mit nur einer Konfigurationsänderung in Ihren Anwendungen. Azure Event Hubs für das Kafka-Ökosystem unterstützt [Apache Kafka Version 1.0](https://kafka.apache.org/10/documentation.html).

## <a name="prerequisites"></a>Voraussetzungen

Für diese Schnellstartanleitung benötigen Sie Folgendes:

* Ein Azure-Abonnement. Wenn Sie keins besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) erstellen, bevor Sie beginnen.
* [Java Development Kit (JDK) 1.7 oder höher](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
* Ein binäres Maven-Archiv ([Download](http://maven.apache.org/download.cgi)/[Installationsanleitung](http://maven.apache.org/install.html))
* [Git-Client](https://www.git-scm.com/)
* [Ein Kafka-fähiger Event Hubs-Namespace](event-hubs-create.md)

## <a name="send-and-receive-messages-with-kafka-in-event-hubs"></a>Senden und Empfangen von Nachrichten mit Kafka in Event Hubs

1. Klonen Sie das [Azure Event Hubs-Repository](https://github.com/Azure/azure-event-hubs).

2. Navigieren Sie zu `azure-event-hubs/samples/kafka/quickstart/producer`.

3. Aktualisieren Sie die Konfigurationsdetails für die Producer in `src/main/resources/producer.config` wie folgt:

    ```xml
    bootstrap.servers={YOUR.EVENTHUBS.FQDN}:9093
    security.protocol=SASL_SSL
    sasl.mechanism=PLAIN
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
    ```
4. Führen Sie den Producer-Code aus, und streamen Sie Daten an Kafka-fähige Event Hubs:
   
    ```shell
    mvn clean package
    mvn exec:java -Dexec.mainClass="TestProducer"                                    
    ```
5. Navigieren Sie zu `azure-event-hubs/samples/kafka/quickstart/consumer`.

6. Aktualisieren Sie die Konfigurationsdetails für die Consumer in `src/main/resources/consumer.config` wie folgt:
   
    ```xml
    bootstrap.servers={YOUR.EVENTHUBS.FQDN}:9093
    security.protocol=SASL_SSL
    sasl.mechanism=PLAIN
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
    ```

7. Führen Sie den Consumer-Code und den Prozess über Kafka-fähige Event Hubs unter Verwendung Ihrer Kafka-Clients aus:

    ```java
    mvn clean package
    mvn exec:java -Dexec.mainClass="TestConsumer"                                    
    ```

Wenn Ihr Kafka-Cluster in Event Hubs über Ereignisse verfügt, werden diese nun allmählich vom Consumer empfangen.

## <a name="next-steps"></a>Nächste Schritte

* [Informationen zu Event Hubs](event-hubs-what-is-event-hubs.md)
* [Informationen zu Event Hubs für das Kafka-Ökosystem](event-hubs-for-kafka-ecosystem-overview.md)
* Verwenden Sie [MirrorMaker](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) zum [Streamen von Ereignissen aus der lokalen Kafka-Instanz an Kafka-fähige Event Hubs in der Cloud](event-hubs-kafka-mirror-maker-tutorial.md).
* Erfahren Sie, wie Sie [Apache Flink](event-hubs-kafka-flink-tutorial.md) oder [Akka Streams](event-hubs-kafka-akka-streams-tutorial.md) zum Streamen an Kafka-fähige Event Hubs verwenden.
