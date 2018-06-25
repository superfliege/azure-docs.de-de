---
title: Senden von Ereignissen an Azure Event Hubs mithilfe von Java | Microsoft-Dokumentation
description: Erste Schritte zum Senden an Event Hubs mit Java
services: event-hubs
author: sethmanheim
manager: timlt
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 05/30/2018
ms.author: sethm
ms.openlocfilehash: 6d3bf0b8ac5c5bdc7bf3deda21e800fe3cc6be2e
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34626410"
---
# <a name="send-events-to-azure-event-hubs-using-java"></a>Senden von Ereignissen an Azure Event Hubs mithilfe von Java

Event Hubs ist ein hochgradig skalierbares Erfassungssystem, das Millionen von Ereignissen pro Sekunde verarbeiten kann. Anwendungen erhalten dadurch die Möglichkeit, die von Ihren verbundenen Geräten und Anwendungen erzeugten immensen Datenmengen zu verarbeiten und zu analysieren. Nach der Erfassung in einem Event Hub können Sie Daten über einen beliebigen Echtzeitanalyseanbieter oder einen Speichercluster transformieren und speichern.

Weitere Informationen finden Sie unter [Übersicht über Event Hubs][Event Hubs overview].

In diesem Tutorial wird gezeigt, wie Sie mithilfe einer Konsolenanwendung in Java Ereignisse an einen Event Hub senden. Informationen zum Empfangen von Ereignissen mithilfe der Java-Bibliothek des Ereignisprozessorhosts finden Sie in [diesem Artikel](event-hubs-java-get-started-receive-eph.md). Alternativ können Sie auch links im Inhaltsverzeichnis auf die entsprechende Empfangssprache klicken.

## <a name="prerequisites"></a>Voraussetzungen

Zum Durchführen dieses Tutorials benötigen Sie Folgendes:

* Eine Java-Entwicklungsumgebung. In diesem Lernprogramm wird [Eclipse](https://www.eclipse.org/) verwendet.
* Ein aktives Azure-Konto. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto][] erstellen, bevor Sie beginnen.

Der Code in diesem Tutorial basiert auf dem [GitHub-Beispiel für SimpleSend](https://github.com/Azure/azure-event-hubs/tree/master/samples/Java/Basic/SimpleSend), das die vollständige funktionierende Anwendung zeigt.

## <a name="send-events-to-event-hubs"></a>Senden von Ereignissen an Event Hubs

Die Java-Clientbibliothek für Event Hubs steht für die Verwendung in Maven-Projekten über das [zentrale Maven-Repository](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22) bereit. Sie können über die folgende Abhängigkeitsdeklaration in Ihrer Maven-Projektdatei auf diese Bibliothek verweisen: Die aktuelle Version ist 1.0.0:    

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>1.0.0</version>
</dependency>
```

Für unterschiedliche Arten von Buildumgebungen können Sie explizit die zuletzt veröffentlichten JAR-Dateien aus dem [zentralen Maven-Repository](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22) abrufen.  

Um einen einfachen Ereignisherausgeber zu erhalten, importieren Sie das Paket *com.microsoft.azure.eventhubs* für die Event Hubs-Clientklassen und das Paket *com.microsoft.azure.servicebus* für Hilfsklassen, z.B. häufige Ausnahmen für den Azure Service Bus-Messagingclient. 

### <a name="declare-the-send-class"></a>Deklarieren der Send-Klasse

Erstellen Sie für das folgende Beispiel zuerst ein neues Maven-Projekt für eine Konsolen-/Shellanwendung in Ihrer bevorzugten Java-Entwicklungsumgebung. Geben Sie der Klassen den Namen `Send`:     

```java
package com.microsoft.azure.eventhubs.samples.send;

import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.microsoft.azure.eventhubs.ConnectionStringBuilder;
import com.microsoft.azure.eventhubs.EventData;
import com.microsoft.azure.eventhubs.EventHubClient;
import com.microsoft.azure.eventhubs.PartitionSender;
import com.microsoft.azure.eventhubs.EventHubException;

import java.io.IOException;
import java.nio.charset.Charset;
import java.time.Instant;
import java.util.Random;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.Executors;
import java.util.concurrent.ExecutorService;

public class Send {

    public static void main(String[] args)
            throws EventHubException, ExecutionException, InterruptedException, IOException {
```

### <a name="construct-connection-string"></a>Erstellen einer Verbindungszeichenfolge

Verwenden Sie die ConnectionStringBuilder-Klasse, um einen Verbindungszeichenfolgenwert an die Event Hubs-Clientinstanz zu übergeben. Ersetzen Sie die Platzhalter in Klammern durch die Werte, die beim Erstellen des Namespace und von Event Hub abgerufen wurden:

```java
   final ConnectionStringBuilder connStr = new ConnectionStringBuilder()
      .setNamespaceName("----NamespaceName-----")
      .setEventHubName("----EventHubName-----")
      .setSasKeyName("-----SharedAccessSignatureKeyName-----")
      .setSasKey("---SharedAccessSignatureKey----");
```

### <a name="send-events"></a>Senden von Ereignisse

Erstellen Sie ein einzelnes Ereignis, indem Sie eine Zeichenfolge in die entsprechende UTF-8-Bytecodierung umwandeln. Erstellen Sie dann auf der Grundlage der Verbindungszeichenfolge eine neue Event Hubs-Clientinstanz, und senden Sie die Nachricht:   

```java 
byte[] payloadBytes = "Test AMQP message from JMS".getBytes("UTF-8");
EventData sendEvent = new EventData(payloadBytes);

final EventHubClient ehClient = EventHubClient.createSync(connStr.toString(), executorService);
ehClient.sendSync(sendEvent);
    
// close the client at the end of your program
ehClient.closeSync();

``` 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Event Hubs finden Sie unter den folgenden Links:

* [Receive events using the Event Processor Host](event-hubs-java-get-started-receive-eph.md) (Empfangen von Ereignissen mithilfe des Ereignisprozessorhosts)
* [Übersicht über Event Hubs][Event Hubs overview]
* [Erstellen eines Event Hubs](event-hubs-create.md)
* [Event Hubs – häufig gestellte Fragen](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-overview.md
[Kostenloses Konto]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio

