---
title: "Senden von Ereignissen an Azure Event Hubs mit standardmäßigen .NET | Microsoft Docs"
description: "Erste Schritte zum Senden von Ereignissen an Event Hubs in standardmäßigen .NET"
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/27/2017
ms.author: sethm
ms.openlocfilehash: 8af9d70965c1c9ad8c49b7d2bb04244fc207058d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-sending-messages-to-azure-event-hubs-in-net-standard"></a>Erste Schritte zum Senden von Nachrichten an Azure Event Hubs in standardmäßigen .NET

> [!NOTE]
> In diesem Beispiel wird über [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender).

Dieses Lernprogramm zeigt, wie eine .NET Core-Konsolenanwendung zu schreiben, die eine Reihe von Nachrichten an einen Event Hub sendet. Ausführen können die [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) Lösung als-, ersetzt die `EhConnectionString` und `EhEntityPath` Zeichenfolgen, die mit Ihren Event Hub-Werten. Oder Sie können die Schritte in diesem Lernprogramm Erstellen eigener.

## <a name="prerequisites"></a>Voraussetzungen

* [Microsoft Visual Studio 2015 oder 2017](http://www.visualstudio.com). In die Beispielen in diesem Lernprogramm verwenden Visual Studio 2017, aber Visual Studio 2015 wird ebenfalls unterstützt.
* [.NET Core Visual Studio 2015 oder 2017 Tools](http://www.microsoft.com/net/core).
* Ein Azure-Abonnement.
* Ein Event Hub-Namespace.

Zum Senden von Nachrichten an einen Event Hub wird es Visual Studio verwenden, um eine C#-Konsolenanwendung zu schreiben.

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Erstellen eines Event Hubs-Namespace und einen Event hub

Der erste Schritt ist die Verwendung der [Azure-Portal](https://portal.azure.com) erstellen einen Namespace für den Event Hub-Typ und Abrufen der Anmeldeinformationen, die Ihre Anwendung benötigt für die Kommunikation mit dem Event Hub. Um einen Namespace und einen Event Hub erstellt haben, wenden Sie das Verfahren [in diesem Artikel](event-hubs-create.md), und klicken Sie dann mit den folgenden Schritten fort.

## <a name="create-a-console-application"></a>Erstellen einer Konsolenanwendung

Starten Sie Visual Studio. Aus der **Datei** Menü klicken Sie auf **neu**, und klicken Sie dann auf **Projekt**. Erstellen Sie eine .NET Core-Konsolenanwendung.

![Neues Projekt][1]

## <a name="add-the-event-hubs-nuget-package"></a>Fügen Sie das Ereignis-Hubs NuGet-Paket hinzu

Hinzufügen der [ `Microsoft.Azure.EventHubs` ](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) .NET Standard Bibliothek NuGet-Paket, dem Projekt wie folgt: 

1. Mit der rechten Maustaste in des neu erstellten Projekts, und wählen Sie **NuGet-Pakete verwalten**.
2. Klicken Sie auf die **Durchsuchen** Registerkarte, und suchen Sie nach "Microsoft.Azure.EventHubs", und wählen Sie die **Microsoft.Azure.EventHubs** Paket. Klicken Sie auf **installieren** zum Abschließen der Installation schließen Sie dann das Dialogfeld zu öffnen.

## <a name="write-some-code-to-send-messages-to-the-event-hub"></a>Schreiben Sie Code zum Senden von Nachrichten an den Event hub

1. Fügen Sie die folgenden `using` Anweisungen am Anfang der Datei "Program.cs".

    ```csharp
    using Microsoft.Azure.EventHubs;
    using System.Text;
    using System.Threading.Tasks;
    ```

2. Fügen Sie Konstanten, mit denen die `Program` Klasse für Event Hubs und Verbindungspfad (einzelne Event Hub-Name). Ersetzen Sie die Platzhalter in Klammern mit den richtigen Werten, die beim Erstellen des Event Hubs abgerufen wurden.

    ```csharp
    private static EventHubClient eventHubClient;
    private const string EhConnectionString = "{Event Hubs connection string}";
    private const string EhEntityPath = "{Event Hub path/name}";
    ```

3. Fügen Sie eine neue Methode mit dem Namen `MainAsync` auf die `Program` -Klasse wie folgt:

    ```csharp
    private static async Task MainAsync(string[] args)
    {
        // Creates an EventHubsConnectionStringBuilder object from the connection string, and sets the EntityPath.
        // Typically, the connection string should have the entity path in it, but for the sake of this simple scenario
        // we are using the connection string from the namespace.
        var connectionStringBuilder = new EventHubsConnectionStringBuilder(EhConnectionString)
        {
            EntityPath = EhEntityPath
        };

        eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());

        await SendMessagesToEventHub(100);

        await eventHubClient.CloseAsync();

        Console.WriteLine("Press ENTER to exit.");
        Console.ReadLine();
    }
    ```

4. Fügen Sie eine neue Methode mit dem Namen `SendMessagesToEventHub` auf die `Program` -Klasse wie folgt:

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

5. Fügen Sie folgenden Code, der `Main` Methode in der `Program` Klasse.

    ```csharp
    MainAsync(args).GetAwaiter().GetResult();
    ```

   Hier wird die Datei "Program.cs" aussehen sollte.

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
            private const string EhConnectionString = "{Event Hubs connection string}";
            private const string EhEntityPath = "{Event Hub path/name}";

            public static void Main(string[] args)
            {
                MainAsync(args).GetAwaiter().GetResult();
            }

            private static async Task MainAsync(string[] args)
            {
                // Creates an EventHubsConnectionStringBuilder object from the connection string, and sets the EntityPath.
                // Typically, the connection string should have the entity path in it, but for the sake of this simple scenario
                // we are using the connection string from the namespace.
                var connectionStringBuilder = new EventHubsConnectionStringBuilder(EhConnectionString)
                {
                    EntityPath = EhEntityPath
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

6. Führen Sie das Programm, und stellen Sie sicher, dass keine Fehler vorliegen.

Gratulation! Sie haben jetzt Nachrichten an einen Event Hub gesendet.

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über Event Hubs, besuchen Sie die folgenden Links:

* [Ereignisse vom Event Hubs empfangen](event-hubs-dotnet-standard-getstarted-receive-eph.md)
* [Event Hubs – Übersicht](event-hubs-what-is-event-hubs.md)
* [Erstellen eines Event Hubs](event-hubs-create.md)
* [Event Hubs – häufig gestellte Fragen](event-hubs-faq.md)

[1]: ./media/event-hubs-dotnet-standard-getstarted-send/netcore.png
