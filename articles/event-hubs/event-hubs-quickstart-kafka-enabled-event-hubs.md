---
title: Streamen in Azure Event Hubs für Apache Kafka | Microsoft-Dokumentation
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
ms.date: 08/01/2018
ms.author: bahariri
ms.openlocfilehash: ec6061cac7188f3f94fa1ec0bf138b9398387099
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39413619"
---
# <a name="stream-into-event-hubs-for-the-apache-kafka"></a>Streamen in Event Hubs für Apache Kafka
In dieser Schnellstartanleitung erfahren Sie, wie Daten an Kafka-fähige Event Hubs gestreamt werden können, ohne Protokollclients zu ändern oder eigene Cluster zu betreiben. Sie lernen, wie Sie über Ihre Producer und Consumer mit Kafka-fähigen Event Hubs kommunizieren – mit nur einer Konfigurationsänderung in Ihren Anwendungen. Azure Event Hubs unterstützt [Apache Kafka Version 1.0.](https://kafka.apache.org/10/documentation.html)

> [!NOTE]
> Dieses Beispiel ist auf [GitHub](https://github.com/Azure/azure-event-hubs) verfügbar.

## <a name="prerequisites"></a>Voraussetzungen

Für diese Schnellstartanleitung benötigen Sie Folgendes:

* Ein Azure-Abonnement. Wenn Sie keins besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) erstellen, bevor Sie beginnen.
* [Java Development Kit (JDK) 1.7 oder höher](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
* Ein binäres Maven-Archiv ([Download](http://maven.apache.org/download.cgi)/[Installationsanleitung](http://maven.apache.org/install.html))
* [Git-Client](https://www.git-scm.com/)
* [Ein Kafka-fähiger Event Hubs-Namespace](event-hubs-create.md)

## <a name="create-a-kafka-enabled-event-hubs-namespace"></a>Erstellen eines Kafka-fähigen Event Hubs-Namespace

1. Melden Sie sich beim [Azure-Portal][Azure-Portal] an, und klicken Sie oben links auf **Ressource erstellen**.

2. Suchen Sie nach Event Hubs, und wählen Sie die hier gezeigten Optionen aus:
    
    ![Suchen nach Event Hubs im Portal](./media/event-hubs-create-kafka-enabled/event-hubs-create-event-hubs.png)
 
3. Stellen Sie einen eindeutigen Namen bereit, und aktivieren Sie Kafka für den Namespace. Klicken Sie auf **Create**.
    
    ![Erstellen eines Namespace](./media/event-hubs-create-kafka-enabled/create-kafka-namespace.png)
 
4. Nachdem der Namespace erstellt wurde, klicken Sie auf der Registerkarte **Einstellungen** auf **SAS-Richtlinien**, um die Verbindungszeichenfolge abzurufen.

    ![Klicken auf „SAS-Richtlinien“](./media/event-hubs-create/create-event-hub7.png)

5. Sie können die Standardrichtlinie **RootManageSharedAccessKey** auswählen oder eine neue Richtlinie hinzufügen. Klicken Sie auf den Richtliniennamen, und kopieren Sie die Verbindungszeichenfolge. 
    
    ![Auswählen einer Richtlinie](./media/event-hubs-create/create-event-hub8.png)
 
6. Fügen Sie diese Verbindungszeichenfolge zur Konfiguration Ihrer Kafka-Anwendung hinzu.

Nun können Sie Ereignisse von Ihren Anwendungen streamen, die das Kafka-Protokoll in Event Hubs verwenden.

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
In diesem Artikel haben Sie erfahren, wie Daten an Kafka-fähige Event Hubs gestreamt werden können, ohne dass Sie Protokollclients ändern oder eigene Cluster betreiben müssen. Fahren Sie mit dem folgenden Tutorial fort, um weitere Informationen zu erhalten:

> [!div class="nextstepaction"]
> [Verwenden von Kafka MirrorMaker mit Event Hubs](event-hubs-kafka-mirror-maker-tutorial.md)
