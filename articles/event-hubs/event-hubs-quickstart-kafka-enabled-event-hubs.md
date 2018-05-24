---
title: Streamen von Ereignissen in Azure Event Hubs für das Kafka-Ökosystem | Microsoft-Dokumentation
description: Streamen von Ereignissen in Event Hubs für das Kafka-Protokoll und die APIs.
services: event-hubs
documentationcenter: ''
author: basilhariri
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/03/2018
ms.author: bahariri
ms.openlocfilehash: cabbb7ed6157a6c68530ab6b5f405aa67b31a1b2
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
---
# <a name="stream-into-event-hubs-for-kafka-ecosystem"></a>Streamen von Ereignissen in Event Hubs für das Kafka-Ökosystem

> [!NOTE]
> Dieses Beispiel ist auf [GitHub](https://github.com/Azure/azure-event-hubs) verfügbar.

Dieser Schnellstart veranschaulicht, wie Daten in Kafka-fähigen Event Hubs gestreamt werden können, ohne Ihre Protokollclients ändern oder Ihre eigenen Cluster ausführen zu müssen. Erfahren Sie, wie Sie über Ihre Producer und Consumer mit Kafka-fähigen Event Hubs kommunizieren können – und zwar mit nur einer Konfigurationsänderung in Ihren Anwendungen. Azure Event Hubs für das Kafka-Ökosystem unterstützt [Apache Kafka Version 1.0.](https://kafka.apache.org/10/documentation.html)

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie für diese Schnellstartanleitung sicher, dass Sie über Folgendes verfügen:

* Ein Azure-Abonnement. Wenn Sie keins besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) erstellen, bevor Sie beginnen.
* [Java Development Kit (JDK) 1.7 oder höher](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
* Ein binäres Maven-Archiv ([Download](http://maven.apache.org/download.cgi)/[Installationsanleitung](http://maven.apache.org/install.html))
* [Git-Client](https://www.git-scm.com/)
* [Ein Kafka-fähiger Event Hubs-Namespace](event-hubs-create.md)

## <a name="send-and-receive-messages-with-kafka-in-event-hubs"></a>Senden und Empfangen von Nachrichten mit Kafka in Event Hubs

1. Klonen Sie das [Azure Event Hubs-Repository](https://github.com/Azure/azure-event-hubs).

2. Navigieren Sie zu `azure-event-hubs/samples/kafka/quickstart/producer`.

3. Aktualisieren Sie die Konfigurationsdetails für den Producer in „src/main/resources/producer.config“, wie hier gezeigt wird.

    ```xml
    bootstrap.servers={YOUR.EVENTHUBS.FQDN}:9093
    security.protocol=SASL_SSL
    sasl.mechanism=PLAIN
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
    ```
4. Führen Sie den Code des Producers aus, und streamen Sie Ereignisse in Kafka-fähige Event Hubs.
   
    ```java
    mvn clean package
    mvn exec:java -Dexec.mainClass="TestProducer"                                    
    ```
5. Navigieren Sie zu „azure-event-hubs/samples/kafka/quickstart/consumer“.

6. Aktualisieren Sie die Konfigurationsdetails für den Consumer in „src/main/resources/consumer.config“, wie hier gezeigt wird.
   
    ```xml
    bootstrap.servers={YOUR.EVENTHUBS.FQDN}:9093
    security.protocol=SASL_SSL
    sasl.mechanism=PLAIN
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
    ```

7. Führen Sie den Code des Consumers aus, und verarbeiten Sie ihn über Kafka-fähige Event Hubs unter Verwendung Ihrer Kafka-Clients.

    ```java
    mvn clean package
    mvn exec:java -Dexec.mainClass="TestConsumer"                                    
    ```

Wenn Ihr Kafka-Cluster in Event Hubs in der Warteschlange eingereihte Ereignisse vom Producer enthält, sollten Sie diese nun allmählich vom Consumer empfangen.

## <a name="next-steps"></a>Nächste Schritte

* [Informationen zu Event Hubs für das Kafka-Ökosystem](event-hubs-for-kafka-ecosystem-overview.md)
* [Informationen zu Event Hubs](event-hubs-what-is-event-hubs.md)
* Streamen von Ereignissen aus dem lokalen Kafka-Cluster in Kafka-fähige Event Hubs in der Cloud.] mithilfe von [MirrorMaker](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) (event-hubs-kafka-mirror-maker-tutorial.md)
