---
title: Erste Schritte mit Azure Service Bus-Warteschlangen | Microsoft-Dokumentation
description: Schreiben Sie eine C#-Konsolenanwendung, für die Service Bus Messaging-Warteschlangen verwendet werden.
services: service-bus-messaging
documentationcenter: .net
author: axisc
manager: timlt
editor: spelluru
ms.assetid: 68a34c00-5600-43f6-bbcc-fea599d500da
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: hero-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 01/23/2019
ms.author: aschhab
ms.openlocfilehash: 81bc37a4bbd63beda122d7254409e2b16f0e3cb1
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2019
ms.locfileid: "54852158"
---
# <a name="get-started-with-service-bus-queues"></a>Erste Schritte mit Service Bus-Warteschlangen

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Dieses Tutorial enthält die folgenden Schritte:

1. Erstellen eines Service Bus-Namespace mithilfe des Azure-Portals
2. Erstellen einer Service Bus-Warteschlange mithilfe des Azure-Portals
3. Schreiben einer .NET Core-Konsolenanwendung, die eine Gruppe von Nachrichten an die Warteschlange sendet
4. Schreiben einer .NET Core-Konsolenanwendung, die diese Nachrichten aus der Warteschlange empfängt

## <a name="prerequisites"></a>Voraussetzungen

1. [Visual Studio 2017 Update 3 (Version 15.3, 26730.01)](https://www.visualstudio.com/vs) oder höher
2. [NET Core SDK](https://www.microsoft.com/net/download/windows) ab Version 2.0
2. Ein Azure-Abonnement.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="create-a-namespace-using-the-azure-portal"></a>Erstellen eines Namespace mithilfe des Azure-Portals

> [!NOTE] 
> Service Bus-Namespace und -Messagingentitäten können auch mit [PowerShell](/powershell/azure/get-started-azureps) erstellt werden. Weitere Informationen finden Sie unter [Verwenden von PowerShell zum Verwalten von Service Bus-Ressourcen](service-bus-manage-with-ps.md).

Falls Sie bereits einen Service Bus Messaging-Namespace erstellt haben, können Sie zum Abschnitt [Erstellen einer Warteschlange mit dem Azure-Portal](#2-create-a-queue-using-the-azure-portal) springen.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-a-queue-using-the-azure-portal"></a>Erstellen einer Warteschlange mithilfe des Azure-Portals

Falls Sie bereits eine Service Bus-Warteschlange erstellt haben, fahren Sie mit dem Abschnitt [Senden von Nachrichten an die Warteschlange](#3-send-messages-to-the-queue) fort.

[!INCLUDE [service-bus-create-queue-portal](../../includes/service-bus-create-queue-portal.md)]

## <a name="send-messages-to-the-queue"></a>Senden von Nachrichten an die Warteschlange

Erstellen Sie mithilfe von Visual Studio eine C#-Konsolenanwendung, um Nachrichten an die Warteschlange senden zu können.

### <a name="create-a-console-application"></a>Erstellen einer Konsolenanwendung

Starten Sie Visual Studio, und erstellen Sie ein neues Projekt vom Typ **Konsolen-App (.NET Core)**.

### <a name="add-the-service-bus-nuget-package"></a>Hinzufügen des NuGet-Pakets "Service Bus"

1. Klicken Sie mit der rechten Maustaste auf das neu erstellte Projekt, und wählen Sie **NuGet-Pakete verwalten** aus.
2. Klicken Sie auf die Registerkarte **Durchsuchen**, suchen Sie nach **[Microsoft.Azure.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus/)**, und wählen Sie anschließend das Element **Microsoft.Azure.ServiceBus** aus. Klicken Sie auf **Installieren**, um die Installation abzuschließen. Schließen Sie danach dieses Dialogfeld.
   
    ![Auswählen eines NuGet-Pakets][nuget-pkg]

### <a name="write-code-to-send-messages-to-the-queue"></a>Schreiben von Code für das Senden von Nachrichten an die Warteschlange

1. Fügen Sie in „Program.cs“ am Beginn der Namespacedefinition die folgenden `using`-Anweisungen vor der Klassendeklaration hinzu:
   
    ```csharp
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.ServiceBus;
    ```

2. Deklarieren Sie innerhalb der Klasse `Program` die folgenden Variablen. Legen Sie die Variable `ServiceBusConnectionString` auf die Verbindungszeichenfolge fest, die Sie beim Erstellen des Namespace erhalten haben, und `QueueName` auf den Namen, den Sie beim Erstellen der Warteschlange verwendet haben:
   
    ```csharp
    const string ServiceBusConnectionString = "<your_connection_string>";
    const string QueueName = "<your_queue_name>";
    static IQueueClient queueClient;
    ``` 

3. Ersetzen Sie den Standardinhalt von `Main()` durch die folgende Codezeile:

    ```csharp
    MainAsync().GetAwaiter().GetResult();
    ```

4. Fügen Sie direkt nach `Main()` die folgende asynchrone `MainAsync()`-Methode hinzu, die die Methode für den Nachrichtenversand aufruft:

    ```csharp
    static async Task MainAsync()
    {
        const int numberOfMessages = 10;
        queueClient = new QueueClient(ServiceBusConnectionString, QueueName);

        Console.WriteLine("======================================================");
        Console.WriteLine("Press ENTER key to exit after sending all the messages.");
        Console.WriteLine("======================================================");

        // Send messages.
        await SendMessagesAsync(numberOfMessages);

        Console.ReadKey();

        await queueClient.CloseAsync();
    }
    ```

5. Fügen Sie direkt nach der `MainAsync()`-Methode die folgende `SendMessagesAsync()`-Methode hinzu. Diese Methode dient zum Senden der durch `numberOfMessagesToSend` angegebenen Anzahl von Nachrichten. Die Anzahl ist aktuell auf 10 festgelegt:

    ```csharp
    static async Task SendMessagesAsync(int numberOfMessagesToSend)
    {
        try
        {
            for (var i = 0; i < numberOfMessagesToSend; i++)
            {
                // Create a new message to send to the queue.
                string messageBody = $"Message {i}";
                var message = new Message(Encoding.UTF8.GetBytes(messageBody));

                // Write the body of the message to the console.
                Console.WriteLine($"Sending message: {messageBody}");

                // Send the message to the queue.
                await queueClient.SendAsync(message);
            }
        }
        catch (Exception exception)
        {
            Console.WriteLine($"{DateTime.Now} :: Exception: {exception.Message}");
        }
    }
    ```
   
6. Die Datei „Program.cs“ sollte nun wie folgt aussehen:
   
    ```csharp
    namespace CoreSenderApp
    {
        using System;
        using System.Text;
        using System.Threading;
        using System.Threading.Tasks;
        using Microsoft.Azure.ServiceBus;

        class Program
        {
            // Connection String for the namespace can be obtained from the Azure portal under the 
            // 'Shared Access policies' section.
            const string ServiceBusConnectionString = "<your_connection_string>";
            const string QueueName = "<your_queue_name>";
            static IQueueClient queueClient;

            static void Main(string[] args)
            {
                MainAsync().GetAwaiter().GetResult();
            }

            static async Task MainAsync()
            {
                const int numberOfMessages = 10;
                queueClient = new QueueClient(ServiceBusConnectionString, QueueName);

                Console.WriteLine("======================================================");
                Console.WriteLine("Press ENTER key to exit after sending all the messages.");
                Console.WriteLine("======================================================");

                // Send Messages
                await SendMessagesAsync(numberOfMessages);
                        
                Console.ReadKey();

                await queueClient.CloseAsync();
            }

            static async Task SendMessagesAsync(int numberOfMessagesToSend)
            {
                try
                {
                    for (var i = 0; i < numberOfMessagesToSend; i++)
                    {
                        // Create a new message to send to the queue
                        string messageBody = $"Message {i}";
                        var message = new Message(Encoding.UTF8.GetBytes(messageBody));

                        // Write the body of the message to the console
                        Console.WriteLine($"Sending message: {messageBody}");

                        // Send the message to the queue
                        await queueClient.SendAsync(message);
                    }
                }
                catch (Exception exception)
                {
                    Console.WriteLine($"{DateTime.Now} :: Exception: {exception.Message}");
                }
            }
        }
    }
    ```

7. Führen Sie das Programm aus, und prüfen Sie im Azure-Portal Folgendes: Klicken Sie im Fenster **Übersicht** auf den Namen Ihrer Warteschlange im Namespace. Für die Warteschlange wird der Bildschirm **Zusammenfassung** angezeigt. Unter **Anzahl aktiver Nachrichten** ist für die Warteschlange nun der Wert **10** angegeben. Jedes Mal, wenn Sie die Absenderanwendung ausführen, ohne die Nachrichten abzurufen (wie im nächsten Abschnitt beschrieben), erhöht sich dieser Wert um 10. Beachten Sie auch, dass sich bei der aktuellen Warteschlangengröße der Wert **Aktuell** im Fenster **Zusammenfassung** jeweils erhöht, wenn der Warteschlange durch die App Nachrichten hinzugefügt werden.
   
      ![Nachrichtengröße][queue-message]

## <a name="receive-messages-from-the-queue"></a>Empfangen von Nachrichten aus der Warteschlange

Erstellen Sie zum Empfangen der soeben gesendeten Nachrichten eine weitere .NET Core-Konsolenanwendung, und installieren Sie das NuGet-Paket **Microsoft.Azure.ServiceBus**. Die Vorgehensweise ist dabei ähnlich wie bei der Absenderanwendung.

### <a name="write-code-to-receive-messages-from-the-queue"></a>Schreiben von Code für das Empfangen von Nachrichten aus der Warteschlange

1. Fügen Sie in „Program.cs“ am Beginn der Namespacedefinition die folgenden `using`-Anweisungen vor der Klassendeklaration hinzu:
   
    ```csharp
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.ServiceBus;
    ```

2. Deklarieren Sie innerhalb der Klasse `Program` die folgenden Variablen. Legen Sie die Variable `ServiceBusConnectionString` auf die Verbindungszeichenfolge fest, die Sie beim Erstellen des Namespace erhalten haben, und `QueueName` auf den Namen, den Sie beim Erstellen der Warteschlange verwendet haben:
   
    ```csharp
    const string ServiceBusConnectionString = "<your_connection_string>";
    const string QueueName = "<your_queue_name>";
    static IQueueClient queueClient;
    ```

3. Ersetzen Sie den Standardinhalt von `Main()` durch die folgende Codezeile:

    ```csharp
    MainAsync().GetAwaiter().GetResult();
    ```

4. Fügen Sie direkt nach `Main()` die folgende asynchrone `MainAsync()`-Methode hinzu, die die `RegisterOnMessageHandlerAndReceiveMessages()`-Methode aufruft:

    ```csharp
    static async Task MainAsync()
    {
        queueClient = new QueueClient(ServiceBusConnectionString, QueueName);

        Console.WriteLine("======================================================");
        Console.WriteLine("Press ENTER key to exit after receiving all the messages.");
        Console.WriteLine("======================================================");

        // Register the queue message handler and receive messages in a loop
        RegisterOnMessageHandlerAndReceiveMessages();

        Console.ReadKey();

        await queueClient.CloseAsync();
    }
    ```

5. Fügen Sie direkt nach der `MainAsync()`-Methode die folgende Methode hinzu, die den Nachrichtenhandler registriert und die von der Absenderanwendung gesendeten Nachrichten empfängt:

    ```csharp
    static void RegisterOnMessageHandlerAndReceiveMessages()
    {
        // Configure the message handler options in terms of exception handling, number of concurrent messages to deliver, etc.
        var messageHandlerOptions = new MessageHandlerOptions(ExceptionReceivedHandler)
        {
            // Maximum number of concurrent calls to the callback ProcessMessagesAsync(), set to 1 for simplicity.
            // Set it according to how many messages the application wants to process in parallel.
            MaxConcurrentCalls = 1,

            // Indicates whether the message pump should automatically complete the messages after returning from user callback.
            // False below indicates the complete operation is handled by the user callback as in ProcessMessagesAsync().
            AutoComplete = false
        };

        // Register the function that processes messages.
        queueClient.RegisterMessageHandler(ProcessMessagesAsync, messageHandlerOptions);
    }
    ```

6. Fügen Sie direkt nach der vorherigen Methode die folgende `ProcessMessagesAsync()`-Methode für die Verarbeitung der empfangenen Nachrichten hinzu:
 
    ```csharp
    static async Task ProcessMessagesAsync(Message message, CancellationToken token)
    {
        // Process the message.
        Console.WriteLine($"Received message: SequenceNumber:{message.SystemProperties.SequenceNumber} Body:{Encoding.UTF8.GetString(message.Body)}");

        // Complete the message so that it is not received again.
        // This can be done only if the queue Client is created in ReceiveMode.PeekLock mode (which is the default).
        await queueClient.CompleteAsync(message.SystemProperties.LockToken);

        // Note: Use the cancellationToken passed as necessary to determine if the queueClient has already been closed.
        // If queueClient has already been closed, you can choose to not call CompleteAsync() or AbandonAsync() etc.
        // to avoid unnecessary exceptions.
    }
    ```

7. Fügen Sie abschließend die folgende Methode zur Behandlung möglicherweise auftretender Ausnahmen hinzu:
 
    ```csharp
    // Use this handler to examine the exceptions received on the message pump.
    static Task ExceptionReceivedHandler(ExceptionReceivedEventArgs exceptionReceivedEventArgs)
    {
        Console.WriteLine($"Message handler encountered an exception {exceptionReceivedEventArgs.Exception}.");
        var context = exceptionReceivedEventArgs.ExceptionReceivedContext;
        Console.WriteLine("Exception context for troubleshooting:");
        Console.WriteLine($"- Endpoint: {context.Endpoint}");
        Console.WriteLine($"- Entity Path: {context.EntityPath}");
        Console.WriteLine($"- Executing Action: {context.Action}");
        return Task.CompletedTask;
    }    
    ```    
   
8. Die Datei „Program.cs“ sollte nun wie folgt aussehen:
   
    ```csharp
    namespace CoreReceiverApp
    {
        using System;
        using System.Text;
        using System.Threading;
        using System.Threading.Tasks;
        using Microsoft.Azure.ServiceBus;

        class Program
        {
            // Connection String for the namespace can be obtained from the Azure portal under the 
            // 'Shared Access policies' section.
            const string ServiceBusConnectionString = "<your_connection_string>";
            const string QueueName = "<your_queue_name>";
            static IQueueClient queueClient;

            static void Main(string[] args)
            {
                MainAsync().GetAwaiter().GetResult();
            }

            static async Task MainAsync()
            {
                queueClient = new QueueClient(ServiceBusConnectionString, QueueName);

                Console.WriteLine("======================================================");
                Console.WriteLine("Press ENTER key to exit after receiving all the messages.");
                Console.WriteLine("======================================================");

                // Register QueueClient's MessageHandler and receive messages in a loop
                RegisterOnMessageHandlerAndReceiveMessages();
                    
                Console.ReadKey();

                await queueClient.CloseAsync();
            }

            static void RegisterOnMessageHandlerAndReceiveMessages()
            {
                // Configure the MessageHandler Options in terms of exception handling, number of concurrent messages to deliver etc.
                var messageHandlerOptions = new MessageHandlerOptions(ExceptionReceivedHandler)
                {
                    // Maximum number of Concurrent calls to the callback `ProcessMessagesAsync`, set to 1 for simplicity.
                    // Set it according to how many messages the application wants to process in parallel.
                    MaxConcurrentCalls = 1,

                    // Indicates whether MessagePump should automatically complete the messages after returning from User Callback.
                    // False below indicates the Complete will be handled by the User Callback as in `ProcessMessagesAsync` below.
                    AutoComplete = false
                };

                // Register the function that will process messages
                queueClient.RegisterMessageHandler(ProcessMessagesAsync, messageHandlerOptions);
            }

            static async Task ProcessMessagesAsync(Message message, CancellationToken token)
            {
                // Process the message
                Console.WriteLine($"Received message: SequenceNumber:{message.SystemProperties.SequenceNumber} Body:{Encoding.UTF8.GetString(message.Body)}");

                // Complete the message so that it is not received again.
                // This can be done only if the queueClient is created in ReceiveMode.PeekLock mode (which is default).
                await queueClient.CompleteAsync(message.SystemProperties.LockToken);

                // Note: Use the cancellationToken passed as necessary to determine if the queueClient has already been closed.
                // If queueClient has already been Closed, you may chose to not call CompleteAsync() or AbandonAsync() etc. calls 
               // to avoid unnecessary exceptions.
            }

            static Task ExceptionReceivedHandler(ExceptionReceivedEventArgs exceptionReceivedEventArgs)
            {
                Console.WriteLine($"Message handler encountered an exception {exceptionReceivedEventArgs.Exception}.");
                var context = exceptionReceivedEventArgs.ExceptionReceivedContext;
                Console.WriteLine("Exception context for troubleshooting:");
                Console.WriteLine($"- Endpoint: {context.Endpoint}");
                Console.WriteLine($"- Entity Path: {context.EntityPath}");
                Console.WriteLine($"- Executing Action: {context.Action}");
                return Task.CompletedTask;
            }
        }
    }
    ```
4. Führen Sie das Programm aus, und überprüfen Sie die Angaben im Portal erneut. Beachten Sie, dass die Werte für **Anzahl aktiver Nachrichten** und **Aktuell** jetzt **0** lauten.
   
    ![Warteschlangenlänge][queue-message-receive]

Glückwunsch! Sie haben eine Warteschlange erstellt, eine Reihe von Nachrichten an diese Warteschlange gesendet und diese Nachrichten aus der Warteschlange empfangen.

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich das [GitHub-Repository mit Beispielen](https://github.com/Azure/azure-service-bus/tree/master/samples) an, mit denen einige erweiterte Features des Service Bus Messaging veranschaulicht werden.

<!--Image references-->

[nuget-pkg]: ./media/service-bus-dotnet-get-started-with-queues/nuget-package.png
[queue-message]: ./media/service-bus-dotnet-get-started-with-queues/queue-message.png
[queue-message-receive]: ./media/service-bus-dotnet-get-started-with-queues/queue-message-receive.png

