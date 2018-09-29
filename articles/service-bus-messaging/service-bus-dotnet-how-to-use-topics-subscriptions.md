---
title: Erste Schritte mit Azure Service Bus-Themen und -Abonnements | Microsoft-Dokumentation
description: Schreiben Sie eine .NET Core-Konsolenanwendung in C#, die Service Bus Messaging-Themen und -Abonnements verwendet.
services: service-bus-messaging
documentationcenter: .net
author: spelluru
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: hero-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 09/03/2018
ms.author: spelluru
ms.openlocfilehash: ea145a1f5ccad98c19b3f779ea297644350b984b
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47393314"
---
# <a name="get-started-with-service-bus-topics"></a>Erste Schritte mit Service Bus-Themen

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Dieses Tutorial enthält die folgenden Schritte:

1. Erstellen eines Service Bus-Namespace mithilfe des Azure-Portals
2. Erstellen eines Service Bus-Themas mithilfe des Azure-Portals
3. Erstellen eines Service Bus-Abonnements für dieses Thema mit dem Azure-Portal
4. Schreiben einer .NET Core-Konsolenanwendung, die eine Gruppe von Nachrichten an das Thema sendet
5. Schreiben einer .NET Core-Konsolenanwendung, die diese Nachrichten aus dem Abonnement empfängt

## <a name="prerequisites"></a>Voraussetzungen

1. [Visual Studio 2017 Update 3 (Version 15.3, 26730.01)](http://www.visualstudio.com/vs) oder höher
2. [NET Core SDK](https://www.microsoft.com/net/download/windows) ab Version 2.0
2. Ein Azure-Abonnement.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a>1. Erstellen eines Namespace mithilfe des Azure-Portals

> [!NOTE] 
> Service Bus-Namespace und -Messagingentitäten können auch mit [PowerShell](/powershell/azure/get-started-azureps) erstellt werden. Weitere Informationen finden Sie unter [Verwenden von PowerShell zum Verwalten von Service Bus-Ressourcen](service-bus-manage-with-ps.md).

Falls Sie bereits einen Service Bus Messaging-Namespace erstellt haben, können Sie zum Abschnitt [Erstellen eines Themas mit dem Azure-Portal](#2-create-a-topic-using-the-azure-portal) springen.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="2-create-a-topic-using-the-azure-portal"></a>2. Erstellen eines Themas mit dem Azure-Portal

1. Melden Sie sich beim [Azure-Portal][azure-portal] an.
2. Klicken Sie im linken Navigationsbereich des Portals auf **Service Bus**. (Sollte **Service Bus** nicht angezeigt werden, klicken Sie auf **Alle Dienste** oder auf **Alle Ressourcen**.) Klicken Sie auf den Namespace, in dem das Thema erstellt werden soll. 
3. Das Fenster mit der Namespaceübersicht wird geöffnet. Klicken Sie auf **Themen**:
   
    ![Erstellen eines Themas][createtopic1]
4. Klicken Sie auf **+ Thema**.
   
    ![Auswählen von Themen][createtopic2]
5. Geben Sie einen Namen für das Thema ein. Behalten Sie bei den anderen Optionen die Standardwerte bei.
   
    ![„Neu“ wählen][createtopic3]
6. Klicken Sie im unteren Bereich des Dialogfelds auf **Erstellen**.

## <a name="3-create-a-subscription-to-the-topic"></a>3. Erstellen eines Abonnements für das Thema

1. Klicken Sie im Bereich mit den Portalressourcen auf den Namespace, den Sie in Schritt 1 erstellt haben, klicken Sie auf **Themen**, und klicken Sie anschließend auf den Namen des Themas, das Sie in Schritt 2 erstellt haben.
2. Klicken Sie oben im Übersichtsbereich auf **+ Abonnement**, um diesem Thema ein Abonnement hinzuzufügen.

    ![Erstellen des Abonnements][createtopic4]

3. Geben Sie einen Namen für das Abonnement ein. Behalten Sie bei den anderen Optionen die Standardwerte bei.

## <a name="4-send-messages-to-the-topic"></a>4. Senden von Nachrichten an das Thema

Erstellen Sie mithilfe von Visual Studio eine C#-Konsolenanwendung, um Nachrichten an das Thema senden zu können.

### <a name="create-a-console-application"></a>Erstellen einer Konsolenanwendung

Starten Sie Visual Studio, und erstellen Sie ein neues Projekt vom Typ **Konsolen-App (.NET Core)**.

### <a name="add-the-service-bus-nuget-package"></a>Hinzufügen des NuGet-Pakets "Service Bus"

1. Klicken Sie mit der rechten Maustaste auf das neu erstellte Projekt, und wählen Sie **NuGet-Pakete verwalten** aus.
2. Klicken Sie auf die Registerkarte **Durchsuchen**, suchen Sie nach **[Microsoft.Azure.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus/)**, und wählen Sie anschließend das Element **Microsoft.Azure.ServiceBus** aus. Klicken Sie auf **Installieren**, um die Installation abzuschließen. Schließen Sie danach dieses Dialogfeld.
   
    ![Auswählen eines NuGet-Pakets][nuget-pkg]

### <a name="write-code-to-send-messages-to-the-topic"></a>Schreiben von Code für das Senden von Nachrichten an das Thema

1. Fügen Sie in „Program.cs“ am Beginn der Namespacedefinition die folgenden `using`-Anweisungen vor der Klassendeklaration hinzu:
   
    ```csharp
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.ServiceBus;
    ```

2. Deklarieren Sie innerhalb der Klasse `Program` die folgenden Variablen. Legen Sie die Variable `ServiceBusConnectionString` auf die Verbindungszeichenfolge fest, die Sie beim Erstellen des Namespace erhalten haben, und `TopicName` auf den Namen, den Sie beim Erstellen des Themas verwendet haben:
   
    ```csharp
    const string ServiceBusConnectionString = "<your_connection_string>";
    const string TopicName = "<your_topic_name>";
    static ITopicClient topicClient;
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
        topicClient = new TopicClient(ServiceBusConnectionString, TopicName);

        Console.WriteLine("======================================================");
        Console.WriteLine("Press ENTER key to exit after sending all the messages.");
        Console.WriteLine("======================================================");

        // Send messages.
        await SendMessagesAsync(numberOfMessages);

        Console.ReadKey();

        await topicClient.CloseAsync();
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
                // Create a new message to send to the topic.
                string messageBody = $"Message {i}";
                var message = new Message(Encoding.UTF8.GetBytes(messageBody));

                // Write the body of the message to the console.
                Console.WriteLine($"Sending message: {messageBody}");

                // Send the message to the topic.
                await topicClient.SendAsync(message);
            }
        }
        catch (Exception exception)
        {
            Console.WriteLine($"{DateTime.Now} :: Exception: {exception.Message}");
        }
    }
    ```
  
6. Die Datei „Program.cs“ für den Versand sollte nun wie folgt aussehen:
   
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
            const string ServiceBusConnectionString = "<your_connection_string>";
            const string TopicName = "<your_topic_name>";
            static ITopicClient topicClient;

            static void Main(string[] args)
            {
                MainAsync().GetAwaiter().GetResult();
            }

            static async Task MainAsync()
            {
                const int numberOfMessages = 10;
                topicClient = new TopicClient(ServiceBusConnectionString, TopicName);

                Console.WriteLine("======================================================");
                Console.WriteLine("Press ENTER key to exit after sending all the messages.");
                Console.WriteLine("======================================================");

                // Send messages.
                await SendMessagesAsync(numberOfMessages);

                Console.ReadKey();

                await topicClient.CloseAsync();
            }

            static async Task SendMessagesAsync(int numberOfMessagesToSend)
            {
                try
                {
                    for (var i = 0; i < numberOfMessagesToSend; i++)
                    {
                        // Create a new message to send to the topic
                        string messageBody = $"Message {i}";
                        var message = new Message(Encoding.UTF8.GetBytes(messageBody));

                        // Write the body of the message to the console
                        Console.WriteLine($"Sending message: {messageBody}");

                        // Send the message to the topic
                        await topicClient.SendAsync(message);
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

3. Führen Sie das Programm aus, und gehen Sie im Azure-Portal wie folgt vor: Klicken Sie im Fenster **Übersicht** auf den Namen Ihres Themas im Namespace. Für das Thema wird der Bildschirm **Zusammenfassung** angezeigt. Beachten Sie, dass der Wert **Nachrichtenanzahl** für das unten auf dem Blatt aufgeführte Abonnement jetzt **10** lautet. Jedes Mal, wenn Sie die Absenderanwendung ausführen, ohne die Nachrichten abzurufen (wie im nächsten Abschnitt beschrieben), erhöht sich dieser Wert um 10. Beachten Sie auch, dass sich bei der aktuellen Themengröße der Wert **Aktuell** im Fenster **Zusammenfassung** jeweils erhöht, wenn dem Thema durch die App Nachrichten hinzugefügt werden.
   
      ![Nachrichtengröße][topic-message]

## <a name="5-receive-messages-from-the-subscription"></a>5. Empfangen von Nachrichten aus dem Abonnement

Erstellen Sie zum Empfangen der soeben gesendeten Nachrichten eine weitere .NET Core-Konsolenanwendung, und installieren Sie das NuGet-Paket **Microsoft.Azure.ServiceBus**. Die Vorgehensweise ist dabei ähnlich wie bei der Absenderanwendung.

### <a name="write-code-to-receive-messages-from-the-subscription"></a>Schreiben von Code für das Empfangen von Nachrichten aus dem Abonnement

1. Fügen Sie in „Program.cs“ am Beginn der Namespacedefinition die folgenden `using`-Anweisungen vor der Klassendeklaration hinzu:
   
    ```csharp
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.ServiceBus;
    ```

2. Deklarieren Sie innerhalb der Klasse `Program` die folgenden Variablen. Legen Sie die `ServiceBusConnectionString`-Variable auf die Verbindungszeichenfolge fest, die Sie beim Erstellen des Namespace erhalten haben, legen Sie `TopicName` auf den Namen fest, den Sie beim Erstellen des Themas verwendet haben, und legen Sie `SubscriptionName` auf den Namen fest, den Sie beim Erstellen des Abonnements für das Thema verwendet haben:
   
    ```csharp
    const string ServiceBusConnectionString = "<your_connection_string>";
    const string TopicName = "<your_topic_name>";
    const string SubscriptionName = "<your_subscription_name>";
    static ISubscriptionClient subscriptionClient;
    ```

3. Ersetzen Sie den Standardinhalt von `Main()` durch die folgende Codezeile:

    ```csharp
    MainAsync().GetAwaiter().GetResult();
    ```

4. Fügen Sie direkt nach `Main()` die folgende asynchrone `MainAsync()`-Methode hinzu, die die `RegisterOnMessageHandlerAndReceiveMessages()`-Methode aufruft:

    ```csharp
    static async Task MainAsync()
    {
        subscriptionClient = new SubscriptionClient(ServiceBusConnectionString, TopicName, SubscriptionName);

        Console.WriteLine("======================================================");
        Console.WriteLine("Press ENTER key to exit after receiving all the messages.");
        Console.WriteLine("======================================================");

        // Register subscription message handler and receive messages in a loop
        RegisterOnMessageHandlerAndReceiveMessages();

        Console.ReadKey();

        await subscriptionClient.CloseAsync();
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
        subscriptionClient.RegisterMessageHandler(ProcessMessagesAsync, messageHandlerOptions);
    }
    ```    

6. Fügen Sie direkt nach der vorherigen Methode die folgende `ProcessMessagesAsync()`-Methode für die Verarbeitung der empfangenen Nachrichten hinzu:
 
    ```csharp
    static async Task ProcessMessagesAsync(Message message, CancellationToken token)
    {
        // Process the message.
        Console.WriteLine($"Received message: SequenceNumber:{message.SystemProperties.SequenceNumber} Body:{Encoding.UTF8.GetString(message.Body)}");

        // Complete the message so that it is not received again.
        // This can be done only if the subscriptionClient is created in ReceiveMode.PeekLock mode (which is the default).
        await subscriptionClient.CompleteAsync(message.SystemProperties.LockToken);

        // Note: Use the cancellationToken passed as necessary to determine if the subscriptionClient has already been closed.
        // If subscriptionClient has already been closed, you can choose to not call CompleteAsync() or AbandonAsync() etc.
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

8. Die Datei „Program.cs“ für den Empfang sollte nun wie folgt aussehen:
   
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
            const string ServiceBusConnectionString = "<your_connection_string>";
            const string TopicName = "<your_topic_name>";
            const string SubscriptionName = "<your_subscription_name>";
            static ISubscriptionClient subscriptionClient;

            static void Main(string[] args)
            {
                MainAsync().GetAwaiter().GetResult();
            }

            static async Task MainAsync()
            {
                subscriptionClient = new SubscriptionClient(ServiceBusConnectionString, TopicName, SubscriptionName);

                Console.WriteLine("======================================================");
                Console.WriteLine("Press ENTER key to exit after receiving all the messages.");
                Console.WriteLine("======================================================");

                // Register subscription message handler and receive messages in a loop.
                RegisterOnMessageHandlerAndReceiveMessages();

                Console.ReadKey();

                await subscriptionClient.CloseAsync();
            }

            static void RegisterOnMessageHandlerAndReceiveMessages()
            {
                // Configure the message handler options in terms of exception handling, number of concurrent messages to deliver, etc.
                var messageHandlerOptions = new MessageHandlerOptions(ExceptionReceivedHandler)
                {
                    // Maximum number of concurrent calls to the callback ProcessMessagesAsync(), set to 1 for simplicity.
                    // Set it according to how many messages the application wants to process in parallel.
                    MaxConcurrentCalls = 1,

                    // Indicates whether MessagePump should automatically complete the messages after returning from User Callback.
                    // False below indicates the Complete will be handled by the User Callback as in `ProcessMessagesAsync` below.
                    AutoComplete = false
                };

                // Register the function that processes messages.
                subscriptionClient.RegisterMessageHandler(ProcessMessagesAsync, messageHandlerOptions);
            }

            static async Task ProcessMessagesAsync(Message message, CancellationToken token)
            {
                // Process the message.
                Console.WriteLine($"Received message: SequenceNumber:{message.SystemProperties.SequenceNumber} Body:{Encoding.UTF8.GetString(message.Body)}");

                // Complete the message so that it is not received again.
                // This can be done only if the subscriptionClient is created in ReceiveMode.PeekLock mode (which is the default).
                await subscriptionClient.CompleteAsync(message.SystemProperties.LockToken);

                // Note: Use the cancellationToken passed as necessary to determine if the subscriptionClient has already been closed.
                // If subscriptionClient has already been closed, you can choose to not call CompleteAsync() or AbandonAsync() etc.
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
9. Führen Sie das Programm aus, und überprüfen Sie die Angaben im Portal erneut. Beachten Sie, dass die Werte für **Nachrichtenanzahl** und **Aktuell** jetzt **0** lauten.
   
    ![Länge des Themas][topic-message-receive]

Glückwunsch! Sie haben mit der .NET Standard-Bibliothek ein Thema und ein Abonnement erstellt, zehn Nachrichten gesendet und diese Nachrichten empfangen.

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich das [GitHub-Repository mit Beispielen](https://github.com/Azure/azure-service-bus/tree/master/samples) für Service Bus an, mit denen einige erweiterte Service Bus-Messaging-Features veranschaulicht werden.

<!--Image references-->

[nuget-pkg]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/nuget-package.png
[topic-message]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/topic-message.png
[topic-message-receive]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/topic-message-receive.png
[createtopic1]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic1.png
[createtopic2]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic2.png
[createtopic3]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic3.png
[createtopic4]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic4.png
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples
[azure-portal]: https://portal.azure.com
