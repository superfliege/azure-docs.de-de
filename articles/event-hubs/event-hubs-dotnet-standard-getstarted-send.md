---
title: Senden von Ereignissen an Azure Event Hubs mithilfe von .NET Standard | Microsoft-Dokumentation
description: Erste Schritte beim Senden von Ereignissen an Event Hubs in .NET Standard
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/18/2018
ms.author: shvija
ms.openlocfilehash: e826dcdbc6d32e6f0ad6ddf72a95869c96af6d69
ms.sourcegitcommit: 668b486f3d07562b614de91451e50296be3c2e1f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2018
ms.locfileid: "49456523"
---
# <a name="get-started-sending-messages-to-azure-event-hubs-in-net-standard"></a>Erste Schritte beim Senden von Nachrichten an Azure Event Hubs in .NET Standard
Event Hubs ist ein Dienst, der große Mengen von Ereignisdaten (Telemetriedaten) von verbundenen Geräten und Anwendungen verarbeiten kann. Nach dem Sammeln von Daten auf Ereignis-Hubs können die Daten mithilfe eines Speicherclusters gespeichert oder mit einem Echtzeitanalyse-Anbieter transformiert werden. Diese umfangreiche Ereignissammlung und -verarbeitung ist eine wichtige Komponente moderner Anwendungsarchitekturen. Hierzu zählt auch das Internet der Dinge (Internet of Things, IoT). Eine ausführliche Übersicht über Event Hubs finden Sie unter [Was ist Azure Event Hubs?](event-hubs-about.md) und unter [Event Hubs-Features im Überblick](event-hubs-features.md).

In diesem Tutorial erfahren Sie auch, wie Sie mithilfe von .NET Core Ereignisse über eine in C# geschriebene Konsolenanwendung an einen Event Hub senden. 

> [!NOTE]
> Sie können diese Schnellstartvorlage als Beispiel aus [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) herunterladen, die Zeichenfolgen `EventHubConnectionString` und `EventHubName` durch die Werte für Ihren Event Hub ersetzen, und das Beispiel dann ausführen. Alternativ können Sie auch die Schritte in diesem Tutorial ausführen, um eine eigene Vorlage zu erstellen.

## <a name="prerequisites"></a>Voraussetzungen
* [Microsoft Visual Studio 2015 oder 2017](http://www.visualstudio.com) In den Beispielen in diesem Tutorial wird Visual Studio 2017 verwendet, aber Visual Studio 2015 wird ebenfalls unterstützt.
* [.NET Core Visual Studio 2015- oder 2017-Tools](http://www.microsoft.com/net/core). 

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Erstellen eines Event Hubs-Namespace und eines Event Hubs
Verwenden Sie zunächst das [Azure-Portal](https://portal.azure.com), um einen Namespace vom Typ „Event Hubs“ zu erstellen, und beschaffen Sie die Verwaltungsanmeldeinformationen, die Ihre Anwendung für die Kommunikation mit dem Event Hub benötigt. Folgen Sie dem Ablauf in [diesem Artikel](event-hubs-create.md), um einen Namespace und einen Event Hub zu erstellen, und fahren Sie dann mit den folgenden Schritten in diesem Tutorial fort.

## <a name="create-a-console-application"></a>Erstellen einer Konsolenanwendung

Starten Sie Visual Studio. Klicken Sie im Menü **Datei** auf **Neu** und dann auf **Projekt**. Erstellen Sie eine .NET Core-Konsolenanwendung.

![Neues Projekt][1]

## <a name="add-the-event-hubs-nuget-package"></a>Hinzufügen des Event Hubs-NuGet-Pakets

Führen Sie die folgenden Schritte aus, um Ihrem Projekt das NuGet-Paket der [`Microsoft.Azure.EventHubs`](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) .NET Standard-Bibliothek hinzuzufügen: 

1. Klicken Sie mit der rechten Maustaste auf das neu erstellte Projekt, und wählen Sie **NuGet-Pakete verwalten** aus.
2. Klicken Sie auf die Registerkarte **Durchsuchen**, und suchen Sie nach „Microsoft.Azure.EventHubs“. Wählen Sie anschließend das Paket **Microsoft.Azure.EventHubs** aus. Klicken Sie auf **Installieren**, um die Installation abzuschließen. Schließen Sie danach dieses Dialogfeld.

## <a name="write-code-to-send-messages-to-the-event-hub"></a>Schreiben von Code zum Senden von Nachrichten an den Event Hub

1. Fügen Sie am Anfang der Datei „Program.cs“ die folgenden `using`-Anweisungen hinzu:

    ```csharp
    using Microsoft.Azure.EventHubs;
    using System.Text;
    using System.Threading.Tasks;
    ```

2. Fügen Sie der `Program`-Klasse Konstanten für die Event Hubs-Verbindungszeichenfolge und den Event Hubs-Entitätspfad (einzelner Event Hub-Name) hinzu. Ersetzen Sie die Platzhalter in Klammern durch die entsprechenden Werte, die beim Erstellen des Event Hubs abgerufen wurden. Stellen Sie sicher, dass es sich bei `{Event Hubs connection string}` um die Verbindungszeichenfolge auf Namespaceebene und nicht um die Event Hub-Zeichenfolge handelt. 

    ```csharp
    private static EventHubClient eventHubClient;
    private const string EventHubConnectionString = "{Event Hubs connection string}";
    private const string EventHubName = "{Event Hub path/name}";
    ```

3. Fügen Sie wie folgt eine neue Methode mit dem Namen `MainAsync` in die `Program`-Klasse ein:

    ```csharp
    private static async Task MainAsync(string[] args)
    {
        // Creates an EventHubsConnectionStringBuilder object from the connection string, and sets the EntityPath.
        // Typically, the connection string should have the entity path in it, but this simple scenario
        // uses the connection string from the namespace.
        var connectionStringBuilder = new EventHubsConnectionStringBuilder(EventHubConnectionString)
        {
            EntityPath = EventHubName
        };

        eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());

        await SendMessagesToEventHub(100);

        await eventHubClient.CloseAsync();

        Console.WriteLine("Press ENTER to exit.");
        Console.ReadLine();
    }
    ```

4. Fügen Sie wie folgt eine neue Methode mit dem Namen `SendMessagesToEventHub` in die `Program`-Klasse ein:

    ```csharp
    // Creates an event hub client and sends 100 messages to the event hub.
    private static async Task SendMessagesToEventHub(int numMessagesToSend)
    {
        for (var i = 0; i < numMessagesToSend; i++)
        {
            try
            {
                var message = $"Message {i}";
                Console.WriteLine($"Sending message: {message}");
                await eventHubClient.SendAsync(new EventData(Encoding.UTF8.GetBytes(message)));
            }
            catch (Exception exception)
            {
                Console.WriteLine($"{DateTime.Now} > Exception: {exception.Message}");
            }

            await Task.Delay(10);
        }

        Console.WriteLine($"{numMessagesToSend} messages sent.");
    }
    ```

5. Fügen Sie der `Main`-Methode in der `Program`-Klasse den folgenden Code hinzu:

    ```csharp
    MainAsync(args).GetAwaiter().GetResult();
    ```

   „Program.cs“ sollte nun wie folgt aussehen:

    ```csharp
    namespace SampleSender
    {
        using System;
        using System.Text;
        using System.Threading.Tasks;
        using Microsoft.Azure.EventHubs;

        public class Program
        {
            private static EventHubClient eventHubClient;
            private const string EventHubConnectionString = "{Event Hubs connection string}";
            private const string EventHubName = "{Event Hub path/name}";

            public static void Main(string[] args)
            {
                MainAsync(args).GetAwaiter().GetResult();
            }

            private static async Task MainAsync(string[] args)
            {
                // Creates an EventHubsConnectionStringBuilder object from the connection string, and sets the EntityPath.
                // Typically, the connection string should have the entity path in it, but for the sake of this simple scenario
                // we are using the connection string from the namespace.
                var connectionStringBuilder = new EventHubsConnectionStringBuilder(EventHubConnectionString)
                {
                    EntityPath = EventHubName
                };

                eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());

                await SendMessagesToEventHub(100);

                await eventHubClient.CloseAsync();

                Console.WriteLine("Press ENTER to exit.");
                Console.ReadLine();
            }

            // Creates an event hub client and sends 100 messages to the event hub.
            private static async Task SendMessagesToEventHub(int numMessagesToSend)
            {
                for (var i = 0; i < numMessagesToSend; i++)
                {
                    try
                    {
                        var message = $"Message {i}";
                        Console.WriteLine($"Sending message: {message}");
                        await eventHubClient.SendAsync(new EventData(Encoding.UTF8.GetBytes(message)));
                    }
                    catch (Exception exception)
                    {
                        Console.WriteLine($"{DateTime.Now} > Exception: {exception.Message}");
                    }

                    await Task.Delay(10);
                }

                Console.WriteLine($"{numMessagesToSend} messages sent.");
            }
        }
    }
    ```

6. Führen Sie das Programm aus, und stellen Sie sicher, dass keine Fehler auftreten.

Glückwunsch! Sie haben jetzt Nachrichten an einen Event Hub gesendet.

## <a name="next-steps"></a>Nächste Schritte
In dieser Schnellstartanleitung haben Sie Nachrichten mithilfe von .NET Standard an einen Event Hub gesendet. Informationen dazu, wie Sie Ereignisse unter Verwendung von .NET Standard von einem Event Hub empfangen, finden Sie unter [Erste Schritte zum Empfangen von Nachrichten mit dem Ereignisprozessorhost (EventProcessorHost) in .NET Standard](event-hubs-dotnet-standard-getstarted-receive-eph.md).

[1]: ./media/event-hubs-dotnet-standard-getstarted-send/netcoresnd.png
