---
title: Empfangen von Ereignissen von Azure Event Hubs mithilfe von Java | Microsoft-Dokumentation
description: Erste Schritte zum Empfangen von Event Hubs mit Java
services: event-hubs
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 08/26/2018
ms.author: shvija
ms.openlocfilehash: dce7c4067ba6d96bf14f4e3300d951b594afe930
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2018
ms.locfileid: "50240631"
---
# <a name="receive-events-from-azure-event-hubs-using-java"></a>Empfangen von Ereignissen von Azure Event Hubs mithilfe von Java

Event Hubs ist ein hochgradig skalierbares Erfassungssystem, das Millionen von Ereignissen pro Sekunde verarbeiten kann. Anwendungen erhalten dadurch die Möglichkeit, die von Ihren verbundenen Geräten und Anwendungen erzeugten immensen Datenmengen zu verarbeiten und zu analysieren. Nach der Erfassung in Event Hubs können Sie Daten über einen beliebigen Echtzeitanalyseanbieter oder ein Speichercluster transformieren und speichern.

Weitere Informationen finden Sie unter [Übersicht über Event Hubs][Event Hubs overview].

In diesem Tutorial wird gezeigt, wie Sie mithilfe einer in Java geschriebenen Konsolenanwendung Ereignisse in einem Event Hub empfangen.

## <a name="prerequisites"></a>Voraussetzungen

Zum Durchführen dieses Tutorials benötigen Sie Folgendes:

* Eine Java-Entwicklungsumgebung. In diesem Lernprogramm wird von [Eclipse](https://www.eclipse.org/)ausgegangen.
* Ein aktives Azure-Konto. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto][] erstellen, bevor Sie beginnen.

Der Code in diesem Tutorial basiert auf dem [EventProcessorSample-Code auf GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/Java/Basic/EventProcessorSample), den Sie untersuchen können, um die vollständige funktionierende Anwendung zu sehen.

## <a name="receive-messages-with-eventprocessorhost-in-java"></a>Empfangen von Nachrichten mit EventProcessorHost in Java

**EventProcessorHost** ist eine Java-Klasse, die das Empfangen von Ereignissen von Event Hubs durch die Verwaltung von permanenten Prüfpunkten und parallelen Empfangsvorgängen von diesen Event Hubs vereinfacht. Mit EventProcessorHost können Sie Ereignisse selbst dann auf mehrere Empfänger aufteilen, wenn diese auf verschiedenen Knoten gehostet werden. Dieses Beispiel zeigt, wie EventProcessorHost für einen einzelnen Empfänger verwendet wird.

### <a name="create-a-storage-account"></a>Speicherkonto erstellen

Um EventProcessorHost verwenden zu können, benötigen Sie ein [Azure Storage-Konto][Azure Storage account]:

1. Melden Sie sich beim [Azure-Portal][Azure portal] an, und klicken Sie auf der linken Seite des Bildschirms auf **+ Ressource erstellen**.
2. Klicken Sie auf **Storage** und anschließend auf **Speicherkonto**. Geben Sie im Fenster **Speicherkonto erstellen** einen Namen für das Speicherkonto ein. Füllen Sie die restlichen Felder aus, wählen Sie die gewünschte Region aus, und klicken Sie dann auf **Erstellen**.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage2.png)

3. Klicken Sie auf das neu erstellte Speicherkonto und anschließend auf **Zugriffsschlüssel**:
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage3.png)

    Kopieren Sie den Wert von „key1“ an einen temporären Speicherort. Sie benötigen ihn später in diesem Tutorial.

### <a name="create-a-java-project-using-the-eventprocessor-host"></a>Erstellen eines Java-Projekts mit EventProcessorHost

Die Java-Clientbibliothek für Event Hubs steht für die Verwendung in Maven-Projekten im [zentralen Maven-Repository][Maven Package] zur Verfügung. Sie können darauf verweisen, indem Sie in der Maven-Projektdatei die folgende Abhängigkeitsdeklaration verwenden. Die aktuelle Version für das Artefakt „azure-eventhubs-eph“ ist 2.0.1, und die aktuelle Version für das Artefakt „azure-eventhubs“ ist 1.0.2:    

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>1.0.2</version>
</dependency>
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs-eph</artifactId>
    <version>2.0.1</version>
</dependency>
```

Für unterschiedliche Arten von Buildumgebungen können Sie explizit die zuletzt veröffentlichten JAR-Dateien aus dem [zentralen Maven-Repository][Maven Package] abrufen.  

1. Erstellen Sie für das folgende Beispiel zuerst ein neues Maven-Projekt für eine Konsolen-/Shellanwendung in Ihrer bevorzugten Java-Entwicklungsumgebung. Die Klasse hat die Bezeichnung `ErrorNotificationHandler`.     
   
    ```java
    import java.util.function.Consumer;
    import com.microsoft.azure.eventprocessorhost.ExceptionReceivedEventArgs;
   
    public class ErrorNotificationHandler implements Consumer<ExceptionReceivedEventArgs>
    {
        @Override
        public void accept(ExceptionReceivedEventArgs t)
        {
            System.out.println("SAMPLE: Host " + t.getHostname() + " received general error notification during " + t.getAction() + ": " + t.getException().toString());
        }
    }
    ```
2. Verwenden Sie den folgenden Code, um eine neue Klasse mit dem Namen `EventProcessorSample`zu erstellen. Ersetzen Sie die Platzhalter durch die Werte, die Sie beim Erstellen des Event Hubs und des Speicherkontos verwendet haben:
   
   ```java
   package com.microsoft.azure.eventhubs.samples.eventprocessorsample;

   import com.microsoft.azure.eventhubs.ConnectionStringBuilder;
   import com.microsoft.azure.eventhubs.EventData;
   import com.microsoft.azure.eventprocessorhost.CloseReason;
   import com.microsoft.azure.eventprocessorhost.EventProcessorHost;
   import com.microsoft.azure.eventprocessorhost.EventProcessorOptions;
   import com.microsoft.azure.eventprocessorhost.ExceptionReceivedEventArgs;
   import com.microsoft.azure.eventprocessorhost.IEventProcessor;
   import com.microsoft.azure.eventprocessorhost.PartitionContext;

   import java.util.concurrent.ExecutionException;
   import java.util.function.Consumer;

   public class EventProcessorSample
   {
       public static void main(String args[]) throws InterruptedException, ExecutionException
       {
           String consumerGroupName = "$Default";
           String namespaceName = "----NamespaceName----";
           String eventHubName = "----EventHubName----";
           String sasKeyName = "----SharedAccessSignatureKeyName----";
           String sasKey = "----SharedAccessSignatureKey----";
           String storageConnectionString = "----AzureStorageConnectionString----";
           String storageContainerName = "----StorageContainerName----";
           String hostNamePrefix = "----HostNamePrefix----";
        
           ConnectionStringBuilder eventHubConnectionString = new ConnectionStringBuilder()
                .setNamespaceName(namespaceName)
                .setEventHubName(eventHubName)
                .setSasKeyName(sasKeyName)
                .setSasKey(sasKey);
        
           EventProcessorHost host = new EventProcessorHost(
                EventProcessorHost.createHostName(hostNamePrefix),
                eventHubName,
                consumerGroupName,
                eventHubConnectionString.toString(),
                storageConnectionString,
                storageContainerName);
        
           System.out.println("Registering host named " + host.getHostName());
           EventProcessorOptions options = new EventProcessorOptions();
           options.setExceptionNotification(new ErrorNotificationHandler());

           host.registerEventProcessor(EventProcessor.class, options)
           .whenComplete((unused, e) ->
           {
               if (e != null)
               {
                   System.out.println("Failure while registering: " + e.toString());
                   if (e.getCause() != null)
                   {
                       System.out.println("Inner exception: " + e.getCause().toString());
                   }
               }
           })
           .thenAccept((unused) ->
           {
               System.out.println("Press enter to stop.");
               try 
               {
                   System.in.read();
               }
               catch (Exception e)
               {
                   System.out.println("Keyboard read failed: " + e.toString());
               }
           })
           .thenCompose((unused) ->
           {
               return host.unregisterEventProcessor();
           })
           .exceptionally((e) ->
           {
               System.out.println("Failure while unregistering: " + e.toString());
               if (e.getCause() != null)
               {
                   System.out.println("Inner exception: " + e.getCause().toString());
               }
               return null;
           })
           .get(); // Wait for everything to finish before exiting main!
        
           System.out.println("End of sample");
       }
    ```
3. Erstellen Sie eine weitere Klasse mit dem Namen `EventProcessor` und dem folgenden Code:
   
    ```java
    public static class EventProcessor implements IEventProcessor
    {
        private int checkpointBatchingCount = 0;

        // OnOpen is called when a new event processor instance is created by the host. 
        @Override
        public void onOpen(PartitionContext context) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is opening");
        }

        // OnClose is called when an event processor instance is being shut down. 
        @Override
        public void onClose(PartitionContext context, CloseReason reason) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is closing for reason " + reason.toString());
        }
        
        // onError is called when an error occurs in EventProcessorHost code that is tied to this partition, such as a receiver failure.
        @Override
        public void onError(PartitionContext context, Throwable error)
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " onError: " + error.toString());
        }

        // onEvents is called when events are received on this partition of the Event Hub. 
        @Override
        public void onEvents(PartitionContext context, Iterable<EventData> events) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " got event batch");
            int eventCount = 0;
            for (EventData data : events)
            {
                try
                {
                    System.out.println("SAMPLE (" + context.getPartitionId() + "," + data.getSystemProperties().getOffset() + "," +
                            data.getSystemProperties().getSequenceNumber() + "): " + new String(data.getBytes(), "UTF8"));
                    eventCount++;
                    
                    // Checkpointing persists the current position in the event stream for this partition and means that the next
                    // time any host opens an event processor on this event hub+consumer group+partition combination, it will start
                    // receiving at the event after this one. 
                    this.checkpointBatchingCount++;
                    if ((checkpointBatchingCount % 5) == 0)
                    {
                        System.out.println("SAMPLE: Partition " + context.getPartitionId() + " checkpointing at " +
                            data.getSystemProperties().getOffset() + "," + data.getSystemProperties().getSequenceNumber());
                        // Checkpoints are created asynchronously. It is important to wait for the result of checkpointing
                        // before exiting onEvents or before creating the next checkpoint, to detect errors and to ensure proper ordering.
                        context.checkpoint(data).get();
                    }
                }
                catch (Exception e)
                {
                    System.out.println("Processing failed for an event: " + e.toString());
                }
            }
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " batch size was " + eventCount + " for host " + context.getOwner());
        }
    }
    ```

Dieses Lernprogramm verwendet eine einzelne Instanz von EventProcessorHost. Zur Erhöhung des Durchsatzes wird empfohlen, mehrere Instanzen von EventProcessorHost auszuführen, wenn möglich auf verschiedenen Computern.  Auf diese Weise wird ebenfalls Redundanz erzielt. In diesen Fällen koordinieren sich die verschiedenen automatisch untereinander, um die Last der eingegangenen Ereignisse ausgeglichen zu verteilen. Wenn mehrere Empfänger für jeden Prozess *alle* Ereignisse verarbeiten sollen, müssen Sie das **ConsumerGroup** -Konzept verwenden. Wenn Ereignisse von anderen Computern empfangen werden, kann es hilfreich sein, die EventProcessorHost-Instanzen nach den Computern (oder Rollen) zu benennen, auf denen sie bereitgestellt worden sind.

## <a name="publishing-messages-to-eventhub"></a>Veröffentlichen von Nachrichten an EventHub

Bevor die Nachrichten von Consumern abgerufen werden, müssen sie zuerst von den Herausgebern an die Partitionen veröffentlicht werden. Hinweis: Wenn Nachrichten mit der sendSync()-Methode auf dem Objekt „com.microsoft.azure.eventhubs.EventHubClient“ synchron an Event Hub veröffentlichen werden, kann die Nachricht je nachdem, ob der Partitionsschlüssel angegeben ist oder nicht, an eine bestimmte Partition gesendet oder anhand der Roundrobin-Methode an alle verfügbaren Partitionen verteilt werden.

Wenn eine Zeichenfolge angegeben ist, die den Partitionsschlüssel darstellt, wird der Schlüssel angegeben wird, wird ein Hashwert für den Schlüssel erstellt, um zu bestimmen, an welche Partition das Ereignis gesendet wird.

Wenn der Partitionsschlüssel nicht festgelegt ist, werden Nachrichten mithilfe von Roundrobin an alle verfügbaren Partitionen gesendet.

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

```

## <a name="implementing-a-custom-checkpointmanager-for-eventprocessorhost-eph"></a>Implementieren eines benutzerdefinierten Prüfpunkt-Managers für EventProcessorHost (EPH)

Die API bietet einen Mechanismus zum Implementieren Ihres benutzerdefinierten Prüfpunkt-Managers für Szenarien, in denen die Standardimplementierung nicht mit Ihrem Anwendungsfall kompatibel ist.

Der standardmäßige Prüfpunkt-Manager verwendet Blobspeicher, aber wenn Sie den von EPH verwendeten Prüfpunkt-Manager durch Ihre eigene Implementierung ersetzen, können Sie Speicher Ihrer Wahl zum Sichern Ihrer Prüfpunkt-Manager-Implementierung verwenden.

Erstellen Sie eine Klasse, die die Schnittstelle „com.microsoft.azure.eventprocessorhost.ICheckpointManager“ implementiert.

Verwenden Sie Ihre benutzerdefinierte Implementierung des Prüfpunkts-Managers (com.microsoft.azure.eventprocessorhost.ICheckpointManager).

In Ihrer Implementierung können Sie den standardmäßigen Prüfpunktmechanismus außer Kraft setzen und Ihre eigenen Prüfpunkte basierend auf Ihrem eigenen Datenspeicher (SQL Server, CosmosDB, Redis Cache usw.) implementieren. Es wird empfohlen, dass der zum Sichern Ihrer Prüfpunkt-Manager-Implementierung verwendete Speicher für alle EPH-Instanzen zugänglich ist, die Ereignisse für die Consumergruppe verarbeiten.

Sie können einen beliebigen Datenspeicher verwenden, der in Ihrer Umgebung verfügbar ist.

Die Klasse „com.microsoft.azure.eventprocessorhost.EventProcessorHost“ bietet Ihnen zwei Konstruktoren, mit denen Sie den Prüfpunkt-Manager für Ihre EventProcessorHost-Instanz außer Kraft setzen können.

## <a name="next-steps"></a>Nächste Schritte
In diesem Schnellstart haben Sie eine Java-Anwendung erstellt, die Nachrichten von einem Event Hub empfängt. Informationen dazu, wie Sie Ereignisse unter Verwendung von Java an einen Event Hub senden, finden Sie unter [Erste Schritte beim Senden von Nachrichten an Azure Event Hubs: Java](event-hubs-java-get-started-send.md).

<!-- Links -->
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
[Azure Storage account]: ../storage/common/storage-create-storage-account.md
[Azure portal]: https://portal.azure.com
[Maven Package]: https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22

<!-- Images -->
[11]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp2.png
[12]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp3.png
[kostenloses Konto]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
