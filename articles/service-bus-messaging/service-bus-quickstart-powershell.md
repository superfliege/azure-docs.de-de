---
title: 'Schnellstart: Senden und Empfangen von Nachrichten an bzw. von Azure Service Bus | Microsoft-Dokumentation'
description: In dieser Schnellstartanleitung wird beschrieben, wie Sie Service Bus-Nachrichten mit PowerShell und dem .NET Standard-Client senden und empfangen.
services: service-bus-messaging
author: spelluru
manager: timlt
ms.service: service-bus-messaging
ms.devlang: dotnet
ms.topic: quickstart
ms.custom: mvc
ms.date: 09/22/2018
ms.author: spelluru
ms.openlocfilehash: 2669b3ad9abc2370b4ecb66596d6edde17c82ddd
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51242431"
---
# <a name="quickstart-send-and-receive-messages-using-azure-powershell-and-net"></a>Schnellstart: Senden und Empfangen von Nachrichten mit Azure PowerShell und .NET

Microsoft Azure Service Bus ist ein Nachrichtenbroker für die Unternehmensintegration, der sicheres Messaging und absolute Zuverlässigkeit bietet. Ein typisches Service Bus-Szenario umfasst normalerweise die Entkopplung von zwei oder mehr Anwendungen, Diensten oder Prozessen und die Übertragung von Zustands- oder Datenänderungen. Szenarien dieser Art können die Planung mehrerer Batchaufträge in einer anderen Anwendung oder einem Dienst oder die Auslösung der Auftragsabwicklung umfassen. Beispielsweise kann es sein, dass ein Einzelhandelsunternehmen seine Point-of-Sale-Daten an ein Backoffice oder regionales Verteilungszentrum sendet, um den Bestand auffüllen bzw. aktualisieren zu lassen. Bei diesem Szenario sendet die Client-App Nachrichten an eine Service Bus-Warteschlange und empfängt Nachrichten von dieser Warteschlange.

![queue](./media/service-bus-quickstart-powershell/quick-start-queue.png)

In dieser Schnellstartanleitung wird beschrieben, wie Sie Nachrichten an eine Service Bus-Warteschlange senden und von dieser empfangen. Sie verwenden Azure PowerShell, um einen Messaging-Namespace und eine Warteschlange innerhalb dieses Namespace zu erstellen und die Anmeldeinformationen dieses Namespace für die Autorisierung zu beschaffen. Anschließend wird gezeigt, wie Sie für diese Warteschlange das Senden und Empfangen von Nachrichten über die [.NET-Standardbibliothek](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus) durchführen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto][] erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Damit Sie dieses Tutorial ausführen können, müssen folgende Komponenten installiert sein:

- [Visual Studio 2017 Update 3 (Version 15.3, 26730.01)](https://www.visualstudio.com/vs) oder höher
- [NET Core SDK](https://www.microsoft.com/net/download/windows) ab Version 2.0

Diese Schnellstartanleitung setzt voraus, dass Sie die aktuelle Version von Azure PowerShell verwenden. Wenn Sie eine Installation oder ein Upgrade durchführen müssen, helfen Ihnen die Informationen unter [Installieren und Konfigurieren von Azure PowerShell][] weiter.

## <a name="log-in-to-azure"></a>Anmelden an Azure

1. Installieren Sie zunächst das Service Bus-PowerShell-Modul, sofern noch nicht geschehen:

   ```azurepowershell-interactive
   Install-Module AzureRM.ServiceBus
   ```

2. Führen Sie den folgenden Befehl aus, um sich bei Azure anzumelden:

   ```azurepowershell-interactive
   Login-AzureRmAccount
   ```

3. Führen Sie die folgenden Befehle aus, um den aktuellen Abonnementkontext festzulegen oder das derzeit aktive Abonnement anzuzeigen:

   ```azurepowershell-interactive
   Select-AzureRmSubscription -SubscriptionName "MyAzureSubName" 
   Get-AzureRmContext
   ```

## <a name="provision-resources"></a>Bereitstellen von Ressourcen

Führen Sie an der PowerShell-Eingabeaufforderung die folgenden Befehle aus, um Service Bus-Ressourcen bereitzustellen. Achten Sie darauf, dass Sie alle Platzhalter durch die entsprechenden Werte ersetzen:

```azurepowershell-interactive
# Create a resource group 
New-AzureRmResourceGroup –Name my-resourcegroup –Location eastus

# Create a Messaging namespace
New-AzureRmServiceBusNamespace -ResourceGroupName my-resourcegroup -NamespaceName namespace-name -Location eastus

# Create a queue 
New-AzureRmServiceBusQueue -ResourceGroupName my-resourcegroup -NamespaceName namespace-name -Name queue-name -EnablePartitioning $False

# Get primary connection string (required in next step)
Get-AzureRmServiceBusKey -ResourceGroupName my-resourcegroup -Namespace namespace-name -Name RootManageSharedAccessKey
```

Kopieren Sie nach Ausführung des `Get-AzureRmServiceBusKey`-Cmdlets die Verbindungszeichenfolge und den von Ihnen ausgewählten Warteschlangennamen an einen temporären Ort, z.B. den Editor. Sie benötigen diese Angaben im nächsten Schritt.

## <a name="send-and-receive-messages"></a>Senden und Empfangen von Nachrichten

Wenn der Namespace und die Warteschlange erstellt wurden und Sie über die erforderlichen Anmeldeinformationen verfügen, sind Sie zum Senden und Empfangen von Nachrichten bereit. Sie können sich den Code in [diesem GitHub-Ordner mit Beispielen](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/GettingStarted/BasicSendReceiveQuickStart) ansehen.

Gehen Sie wie folgt vor, um den Code auszuführen:

1. Klonen Sie das [Service Bus-GitHub-Repository](https://github.com/Azure/azure-service-bus/), indem Sie den folgenden Befehl ausführen:

   ```shell
   git clone https://github.com/Azure/azure-service-bus.git
   ```

3. Navigieren Sie zum Beispielordner `azure-service-bus\samples\DotNet\GettingStarted\BasicSendReceiveQuickStart\BasicSendReceiveQuickStart`.

4. Rufen Sie die Verbindungszeichenfolge mit dem folgenden PowerShell-Cmdlet ab, falls Sie dies noch nicht getan haben. Achten Sie darauf, dass Sie `my-resourcegroup` und `namespace-name` durch Ihre jeweiligen Werte ersetzen: 

   ```azurepowershell-interactive
   Get-AzureRmServiceBusKey -ResourceGroupName my-resourcegroup -Namespace namespace-name -Name RootManageSharedAccessKey
   ```

5.  Geben Sie an der PowerShell-Eingabeaufforderung den folgenden Befehl ein:

   ```shell
   dotnet build
   ```

6.  Navigieren Sie zum Ordner `bin\Debug\netcoreapp2.0`.

7.  Geben Sie den folgenden Befehl ein, um das Programm auszuführen. Ersetzen Sie `myConnectionString` durch den zuvor abgerufenen Wert und `myQueueName` durch den Namen der von Ihnen erstellten Warteschlange:

   ```shell
   dotnet BasicSendReceiveQuickStart.dll -ConnectionString "myConnectionString" -QueueName "myQueueName"
   ``` 

8. Verfolgen Sie, wie zehn Nachrichten an die Warteschlange gesendet und anschließend von der Warteschlange empfangen werden:

   ![Programmausgabe](./media/service-bus-quickstart-powershell/dotnet.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, den Namespace und alle zugehörigen Ressourcen zu entfernen:

```powershell-interactive
Remove-AzureRmResourceGroup -Name my-resourcegroup
```

## <a name="understand-the-sample-code"></a>Grundlagen des Beispielcodes

Dieser Abschnitt enthält weitere Details zur Funktionsweise des Beispielcodes. 

### <a name="get-connection-string-and-queue"></a>Abrufen der Verbindungszeichenfolge und der Warteschlange

Die Verbindungszeichenfolge und der Warteschlangenname werden als Befehlszeilenargumente an die `Main()`-Methode übergeben. `Main()` deklariert zwei Zeichenfolgenvariablen für diese Werte:

```csharp
static void Main(string[] args)
{
    string ServiceBusConnectionString = "";
    string QueueName = "";

    for (int i = 0; i < args.Length; i++)
    {
        var p = new Program();
        if (args[i] == "-ConnectionString")
        {
            Console.WriteLine($"ConnectionString: {args[i+1]}");
            ServiceBusConnectionString = args[i + 1]; 
        }
        else if(args[i] == "-QueueName")
        {
            Console.WriteLine($"QueueName: {args[i+1]}");
            QueueName = args[i + 1];
        }                
    }

    if (ServiceBusConnectionString != "" && QueueName != "")
        MainAsync(ServiceBusConnectionString, QueueName).GetAwaiter().GetResult();
    else
    {
        Console.WriteLine("Specify -Connectionstring and -QueueName to execute the example.");
        Console.ReadKey();
    }                            
}
```
 
Die `Main()`-Methode startet dann die asynchrone Nachrichtenschleife `MainAsync()`.

### <a name="message-loop"></a>Nachrichtenschleife

Die Methode „MainAsync()“ erstellt einen Warteschlangenclient mit den Befehlszeilenargumenten, ruft einen Nachrichtenhandler für den Empfang mit dem Namen `RegisterOnMessageHandlerAndReceiveMessages()` auf und sendet die Nachrichten:

```csharp
static async Task MainAsync(string ServiceBusConnectionString, string QueueName)
{
    const int numberOfMessages = 10;
    queueClient = new QueueClient(ServiceBusConnectionString, QueueName);

    Console.WriteLine("======================================================");
    Console.WriteLine("Press any key to exit after receiving all the messages.");
    Console.WriteLine("======================================================");

    // Register QueueClient's MessageHandler and receive messages in a loop
    RegisterOnMessageHandlerAndReceiveMessages();

    // Send Messages
    await SendMessagesAsync(numberOfMessages);

    Console.ReadKey();

    await queueClient.CloseAsync();
}
```

Die `RegisterOnMessageHandlerAndReceiveMessages()`-Methode legt lediglich einige Nachrichtenhandleroptionen fest und ruft dann die `RegisterMessageHandler()`-Methode des Warteschlangenclients auf, über die der Empfangsvorgang gestartet wird:

```csharp
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
```

### <a name="send-messages"></a>Senden von Nachrichten

Die Vorgänge zum Erstellen und Senden von Nachrichten erfolgen in der `SendMessagesAsync()`-Methode:

```csharp
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
```

### <a name="process-messages"></a>Verarbeiten von Nachrichten

Die `ProcessMessagesAsync()`-Methode führt die Bestätigung, Verarbeitung und den Abschluss des Empfangsvorgangs für die Nachrichten durch:

```csharp
static async Task ProcessMessagesAsync(Message message, CancellationToken token)
{
    // Process the message
    Console.WriteLine($"Received message: SequenceNumber:{message.SystemProperties.SequenceNumber} Body:{Encoding.UTF8.GetString(message.Body)}");

    // Complete the message so that it is not received again.
    await queueClient.CompleteAsync(message.SystemProperties.LockToken);
}
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie einen Service Bus-Namespace und weitere erforderliche Ressourcen zum Senden und Empfangen von Nachrichten für eine Warteschlange erstellt. Weitere Informationen zum Schreiben von Code für das Senden und Empfangen von Nachrichten erhalten Sie im folgenden Tutorial zu Service Bus:

> [!div class="nextstepaction"]
> [Tutorial: Aktualisieren des Bestands mit PowerShell und Themen/Abonnements](./service-bus-tutorial-topics-subscriptions-powershell.md)

[kostenloses Konto]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
[Installieren und Konfigurieren von Azure PowerShell]: /powershell/azure/install-azurerm-ps