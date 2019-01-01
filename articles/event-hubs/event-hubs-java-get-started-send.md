---
title: 'Senden von Ereignissen unter Verwendung von Java: Azure Event Hubs | Microsoft-Dokumentation'
description: Dieser Artikel enthält eine exemplarische Vorgehensweise für die Erstellung einer Java-Anwendung, die Ereignisse an Azure Event Hubs sendet.
services: event-hubs
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 80c413c874ca3e1bf46bfa4e5becb184223c5eeb
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2018
ms.locfileid: "53091283"
---
# <a name="send-events-to-azure-event-hubs-using-java"></a>Senden von Ereignissen an Azure Event Hubs mithilfe von Java

Azure Event Hubs ist eine Big Data-Streamingplattform und ein Ereigniserfassungsdienst, der pro Sekunde Millionen von Ereignissen empfangen und verarbeiten kann. Event Hubs kann Ereignisse, Daten oder Telemetriedaten, die von verteilter Software und verteilten Geräten erzeugt wurden, verarbeiten und speichern. An einen Event Hub gesendete Daten können transformiert und mit einem beliebigen Echtzeitanalyse-Anbieter oder Batchverarbeitungs-/Speicheradapter gespeichert werden. Eine ausführliche Übersicht über Event Hubs finden Sie unter [Was ist Azure Event Hubs?](event-hubs-about.md) und [Event Hubs-Features im Überblick](event-hubs-features.md).

In diesem Tutorial wird gezeigt, wie Sie mithilfe einer Konsolenanwendung in Java geschriebene Ereignisse an einen Event Hub senden. 

> [!NOTE]
> Sie können diese Schnellstartvorlage als Beispiel aus [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/Java/Basic/SimpleSend) herunterladen, die Zeichenfolgen `EventHubConnectionString` und `EventHubName` durch die Werte für Ihren Event Hub ersetzen und das Beispiel dann ausführen. Alternativ können Sie auch die Schritte in diesem Tutorial ausführen, um eine eigene Vorlage zu erstellen.

## <a name="prerequisites"></a>Voraussetzungen

Zum Durchführen dieses Tutorials benötigen Sie Folgendes:

* Eine Java-Entwicklungsumgebung. In diesem Lernprogramm wird [Eclipse](https://www.eclipse.org/) verwendet.

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Erstellen eines Event Hubs-Namespace und eines Event Hubs
Verwenden Sie zunächst das [Azure-Portal](https://portal.azure.com), um einen Namespace vom Typ „Event Hubs“ zu erstellen, und beschaffen Sie die Verwaltungsanmeldeinformationen, die Ihre Anwendung für die Kommunikation mit dem Event Hub benötigt. Erstellen Sie anhand der Anleitung in [diesem Artikel](event-hubs-create.md) einen Namespace und einen Event Hub.

Gehen Sie wie im Artikel beschrieben vor, um den Wert des Zugriffsschlüssels für den Event Hub abzurufen: [Abrufen der Verbindungszeichenfolge](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). Sie verwenden den Zugriffsschlüssel im Code, den Sie später in diesem Tutorial schreiben. Der Standardschlüsselname lautet: **RootManageSharedAccessKey**.

Fahren Sie nun mit den folgenden Schritten in diesem Tutorial fort:

## <a name="add-reference-to-azure-event-hubs-library"></a>Hinzufügen von Verweisen auf die Azure Event Hubs-Bibliothek

Die Java-Clientbibliothek für Event Hubs steht für die Verwendung in Maven-Projekten über das [zentrale Maven-Repository](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22) bereit. Sie können über die folgende Abhängigkeitsdeklaration in Ihrer Maven-Projektdatei auf diese Bibliothek verweisen: Die aktuelle Version ist 1.0.2:    

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>1.0.2</version>
</dependency>
```

Für unterschiedliche Arten von Buildumgebungen können Sie explizit die zuletzt veröffentlichten JAR-Dateien aus dem [zentralen Maven-Repository](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22) abrufen.  

Um einen einfachen Ereignisherausgeber zu erhalten, importieren Sie das Paket *com.microsoft.azure.eventhubs* für die Event Hubs-Clientklassen und das Paket *com.microsoft.azure.servicebus* für Hilfsklassen, z.B. häufige Ausnahmen für den Azure Service Bus-Messagingclient. 

## <a name="write-code-to-send-messages-to-the-event-hub"></a>Schreiben von Code zum Senden von Nachrichten an den Event Hub

Erstellen Sie für das folgende Beispiel zuerst ein neues Maven-Projekt für eine Konsolen-/Shellanwendung in Ihrer bevorzugten Java-Entwicklungsumgebung. Geben Sie der Klassen den Namen `SimpleSend`:     

```java
package com.microsoft.azure.eventhubs.samples.send;

import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.microsoft.azure.eventhubs.ConnectionStringBuilder;
import com.microsoft.azure.eventhubs.EventData;
import com.microsoft.azure.eventhubs.EventHubClient;
import com.microsoft.azure.eventhubs.EventHubException;

import java.io.IOException;
import java.nio.charset.Charset;
import java.time.Instant;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.Executors;
import java.util.concurrent.ExecutorService;

public class SimpleSend {

    public static void main(String[] args)
            throws EventHubException, ExecutionException, InterruptedException, IOException {
            
            
    }
 }
```

### <a name="construct-connection-string"></a>Erstellen einer Verbindungszeichenfolge

Verwenden Sie die ConnectionStringBuilder-Klasse, um einen Verbindungszeichenfolgenwert an die Event Hubs-Clientinstanz zu übergeben. Ersetzen Sie die Platzhalter in Klammern durch die Werte, die beim Erstellen des Namespace und von Event Hub abgerufen wurden:

```java
final ConnectionStringBuilder connStr = new ConnectionStringBuilder()
        .setNamespaceName("Your Event Hubs namespace name")
        .setEventHubName("Your event hub")
        .setSasKeyName("Your policy name")
        .setSasKey("Your primary SAS key");
```

### <a name="send-events"></a>Senden von Ereignisse

Erstellen Sie ein einzelnes Ereignis, indem Sie eine Zeichenfolge in die entsprechende UTF-8-Bytecodierung umwandeln. Erstellen Sie dann auf der Grundlage der Verbindungszeichenfolge eine neue Event Hubs-Clientinstanz, und senden Sie die Nachricht:   

```java 
String payload = "Message " + Integer.toString(i);
byte[] payloadBytes = gson.toJson(payload).getBytes(Charset.defaultCharset());
EventData sendEvent = EventData.create(payloadBytes);

final EventHubClient ehClient = EventHubClient.createSync(connStr.toString(), executorService);
ehClient.sendSync(sendEvent);
    
// close the client at the end of your program
ehClient.closeSync();

``` 

Erstellen Sie das Programm, führen Sie es aus, und stellen Sie sicher, dass keine Fehler ausgelöst werden.

Glückwunsch! Sie haben jetzt Nachrichten an einen Event Hub gesendet.

### <a name="appendix-how-messages-are-routed-to-eventhub-partitions"></a>Anhang: So werden Nachrichten an Event Hub-Partitionen weitergeleitet

Bevor die Nachrichten von Consumern abgerufen werden, müssen sie zuerst von den Herausgebern an die Partitionen veröffentlicht werden. Wenn Nachrichten mit der sendSync()-Methode auf dem Objekt „com.microsoft.azure.eventhubs.EventHubClient“ synchron an Event Hub veröffentlichen werden, kann die Nachricht je nachdem, ob der Partitionsschlüssel angegeben ist oder nicht, an eine bestimmte Partition gesendet oder anhand der Roundrobin-Methode an alle verfügbaren Partitionen verteilt werden.

Wenn eine Zeichenfolge angegeben ist, die den Partitionsschlüssel darstellt, wird der Schlüssel angegeben wird, wird ein Hashwert für den Schlüssel erstellt, um zu bestimmen, an welche Partition das Ereignis gesendet wird.

Wenn der Partitionsschlüssel nicht festgelegt ist, werden Nachrichten per Roundrobin an alle verfügbaren Partitionen gesendet.

```java
// Serialize the event into bytes
byte[] payloadBytes = gson.toJson(messagePayload).getBytes(Charset.defaultCharset());

// Use the bytes to construct an {@link EventData} object
EventData sendEvent = EventData.create(payloadBytes);

// Transmits the event to event hub without a partition key
// If a partition key is not set, then we will round-robin to all topic partitions
eventHubClient.sendSync(sendEvent);

//  the partitionKey will be hash'ed to determine the partitionId to send the eventData to.
eventHubClient.sendSync(sendEvent, partitionKey);

// close the client at the end of your program
eventHubClient.closeSync();

```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie Nachrichten mithilfe von Java an einen Event Hub gesendet. Informationen dazu, wie Sie Ereignisse unter Verwendung von Java von einem Event Hub empfangen, erhalten Sie unter [Empfangen von Ereignissen von Azure Event Hubs mithilfe von Java](event-hubs-java-get-started-receive-eph.md).

<!-- Links -->
[Event Hubs overview]: event-hubs-overview.md
[free account]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio

