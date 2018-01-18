---
title: C#-Entwicklerreferenz zu Azure Functions
description: Erfahren Sie, wie Azure Functions mithilfe von C# entwickelt wird.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: Azure Functions, Functions, Ereignisverarbeitung, Webhooks, dynamisches Compute, serverlose Architektur
ms.service: functions
ms.devlang: dotnet
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 12/12/2017
ms.author: glenga
ms.openlocfilehash: 3de1e9b042a7a356c3c88e604e1e26c256d85657
ms.sourcegitcommit: 71fa59e97b01b65f25bcae318d834358fea5224a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2018
---
# <a name="azure-functions-c-developer-reference"></a>C#-Entwicklerreferenz zu Azure Functions

<!-- When updating this article, make corresponding changes to any duplicate content in functions-reference-csharp.md -->

Dieser Artikel ist eine Einführung in die Entwicklung von Azure Functions durch Verwenden von C# in .NET-Klassenbibliotheken.

Azure Functions unterstützt die Programmiersprachen C# und C#-Skript. Wenn Sie nach Anleitungen zum [Verwenden von C# im Azure-Portal](functions-create-function-app-portal.md) suchen, lesen Sie [C#-Skriptentwicklerreferenz (C#-Skript, CSX) zu Azure Functions](functions-reference-csharp.md).

In diesem Artikel wird davon ausgegangen, dass Sie die folgenden Artikel bereits gelesen haben:

* [Azure Functions: Entwicklerhandbuch](functions-reference.md)
* [Azure Functions Visual Studio 2017 Tools](functions-develop-vs.md)

## <a name="functions-class-library-project"></a>Funktionsklassenbibliotheks-Projekt

In Visual Studio wird mit der **Azure Functions**-Projektvorlage ein C#-Klassenbibliotheksprojekt erstellt, das die folgenden Dateien enthält:

* [host.json](functions-host-json.md): Speichert Konfigurationseinstellungen, die sich auf alle Funktionen im Projekt auswirken, wenn es lokal oder in Azure ausgeführt wird.
* [local.settings.json](functions-run-local.md#local-settings-file): Speichert App-Einstellungen und Verbindungszeichenfolgen, die verwendet werden, wenn das Projekt lokal ausgeführt wird.

### <a name="functionname-and-trigger-attributes"></a>„FunctionName“ und Trigger-Attribute

In einer Klassenbibliothek ist eine Funktion eine statische Methode mit einem Funktionsnamen (`FunctionName`) und einem Trigger-Attribut, wie im folgenden Beispiel gezeigt:

```csharp
public static class SimpleExample
{
    [FunctionName("QueueTrigger")]
    public static void Run(
        [QueueTrigger("myqueue-items")] string myQueueItem, 
        TraceWriter log)
    {
        log.Info($"C# function processed: {myQueueItem}");
    }
} 
```

Das Attribut `FunctionName` kennzeichnet die Methode als Funktionseinstiegspunkt. Der Name muss innerhalb eines Projekts eindeutig sein.

Das Trigger-Attribut gibt den Triggertyp an und bindet die Eingabedaten an einen Methodenparameter. Die Beispielfunktion wird durch eine Warteschlangennachricht ausgelöst, und die Warteschlangennachricht wird im `myQueueItem`-Parameter an die Methode übergeben.

### <a name="additional-binding-attributes"></a>Zusätzliche Bindungsattribute

Es können zusätzliche Eingabe- und Ausgabebindungsattribute verwendet werden. Im folgenden Beispiel wird das vorhergehende geändert, indem eine Ausgabewarteschlangenbindung hinzugefügt wird. Die Funktion schreibt die Eingabewarteschlangennachricht in eine neue Warteschlangennachricht in einer anderen Warteschlange.

```csharp
public static class SimpleExampleWithOutput
{
    [FunctionName("CopyQueueMessage")]
    public static void Run(
        [QueueTrigger("myqueue-items-source")] string myQueueItem, 
        [Queue("myqueue-items-destination")] out string myQueueItemCopy,
        TraceWriter log)
    {
        log.Info($"CopyQueueMessage function processed: {myQueueItem}");
        myQueueItemCopy = myQueueItem;
    }
}
```

### <a name="conversion-to-functionjson"></a>Konvertierung in „function.json“

Im Buildprozess wird eine *function.json*-Datei in einem Funktionsordner im Ordner für Builds erstellt. Diese Datei ist nicht dazu vorgesehen, direkt bearbeitet zu werden. Sie können weder die Bindungskonfiguration ändern noch die Funktion deaktivieren, indem Sie diese Datei bearbeiten. 

Der Zweck dieser Datei besteht darin, Informationen für den Skalierungscontroller bereitzustellen, die dieser für [Skalierungsentscheidungen hinsichtlich des Verbrauchsplans](functions-scale.md#how-the-consumption-plan-works) verwenden kann. Aus diesem Grund hat die Datei nur Triggerinformationen und keine Eingabe- oder Ausgabebindungen.

Die generierte *function.json*-Datei enthält eine `configurationSource`-Eigenschaft, die der Runtime mitteilt, dass sie statt der *function.json*-Konfiguration die .NET Attribute für Bindungen verwenden soll. Hier sehen Sie ein Beispiel:

```json
{
  "generatedBy": "Microsoft.NET.Sdk.Functions-1.0.0.0",
  "configurationSource": "attributes",
  "bindings": [
    {
      "type": "queueTrigger",
      "queueName": "%input-queue-name%",
      "name": "myQueueItem"
    }
  ],
  "disabled": false,
  "scriptFile": "..\\bin\\FunctionApp1.dll",
  "entryPoint": "FunctionApp1.QueueTrigger.Run"
}
```

Die Erstellung der *function.json*-Datei wird mit dem NuGet-Paket [Microsoft\.NET\.Sdk\.Functions](http://www.nuget.org/packages/Microsoft.NET.Sdk.Functions) ausgeführt. Der Quellcode ist im GitHub-Repository [azure\-functions\-vs\-build\-sdk](https://github.com/Azure/azure-functions-vs-build-sdk) verfügbar.

## <a name="supported-types-for-bindings"></a>Unterstützte Typen für Bindungen

Jede Bindung hat ihre eigenen unterstützten Typen. Beispielsweise kann ein Blobtriggerattribut auf einen Zeichenfolgeparameter, einen POCO-Parameter, einen `CloudBlockBlob`-Parameter oder einen von mehreren anderen unterstützten Typen angewendet werden. Im [Bindungsreferenzartikel für Blobbindungen](functions-bindings-storage-blob.md#trigger---usage) sind alle unterstützten Parametertypen aufgelistet. Weitere Informationen hierzu finden Sie unter [Trigger und Bindungen](functions-triggers-bindings.md) und in den [Bindungsreferenzdokumenten für jeden Bindungstyp](functions-triggers-bindings.md#next-steps).

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## <a name="binding-to-method-return-value"></a>Binden an den Rückgabewert einer Methode

Sie können für eine Ausgabebindung den Rückgabewert einer Methode verwenden, wie dies im folgende Beispiel veranschaulicht ist:

```csharp
public static class ReturnValueOutputBinding
{
    [FunctionName("CopyQueueMessageUsingReturnValue")]
    [return: Queue("myqueue-items-destination")]
    public static string Run(
        [QueueTrigger("myqueue-items-source-2")] string myQueueItem,
        TraceWriter log)
    {
        log.Info($"C# function processed: {myQueueItem}");
        return myQueueItem;
    }
}
```

## <a name="writing-multiple-output-values"></a>Schreiben von mehreren Ausgabewerten

Um mehrere Werte in eine Ausgabebindung zu schreiben, verwenden Sie die Typen [`ICollector`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) oder [`IAsyncCollector`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs). Diese Typen stellen lesegeschützte Sammlungen dar, die nach Durchführung der Methode in die Ausgabebindung geschrieben werden.

In diesem Beispiel werden mehrere Warteschlangennachrichten mit `ICollector` in die gleiche Warteschlange geschrieben:

```csharp
public static class ICollectorExample
{
    [FunctionName("CopyQueueMessageICollector")]
    public static void Run(
        [QueueTrigger("myqueue-items-source-3")] string myQueueItem,
        [Queue("myqueue-items-destination")] ICollector<string> myQueueItemCopy,
        TraceWriter log)
    {
        log.Info($"C# function processed: {myQueueItem}");
        myQueueItemCopy.Add($"Copy 1: {myQueueItem}");
        myQueueItemCopy.Add($"Copy 2: {myQueueItem}");
    }
}
```

## <a name="logging"></a>Protokollierung

Um eine Ausgabe in C# in Ihren Streamingprotokollen zu dokumentieren, fügen Sie ein Argument vom Typ `TraceWriter` ein. Verwenden Sie hierzu am besten den Namen `log`. Vermeiden Sie die Verwendung von `Console.Write` in Azure Functions. 

`TraceWriter` ist im [Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/TraceWriter.cs) definiert. Die Protokollebene für `TraceWriter` kann in [host.json](functions-host-json.md) konfiguriert werden.

```csharp
public static class SimpleExample
{
    [FunctionName("QueueTrigger")]
    public static void Run(
        [QueueTrigger("myqueue-items")] string myQueueItem, 
        TraceWriter log)
    {
        log.Info($"C# function processed: {myQueueItem}");
    }
} 
```

> [!NOTE]
> Informationen zu einem neueren Protokollierungsframework, das Sie anstelle von `TraceWriter` verwenden können, finden Sie unter [Schreiben von Protokollen in C#-Funktionen](functions-monitoring.md#write-logs-in-c-functions) im Artikel **Überwachen von Azure Functions**.

## <a name="async"></a>Async

Um eine Funktion asynchron auszuführen, verwenden Sie das `async`-Schlüsselwort, und geben Sie ein `Task`-Objekt zurück.

```csharp
public static class AsyncExample
{
    [FunctionName("BlobCopy")]
    public static async Task RunAsync(
        [BlobTrigger("sample-images/{blobName}")] Stream blobInput,
        [Blob("sample-images-copies/{blobName}", FileAccess.Write)] Stream blobOutput,
        CancellationToken token,
        TraceWriter log)
    {
        log.Info($"BlobCopy function processed.");
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
}
```

## <a name="cancellation-tokens"></a>Abbruchtoken

Einige Vorgänge erfordern ein ordnungsgemäßes Herunterfahren. Es empfiehlt sich grundsätzlich, Code zu schreiben, der Abstürze behandeln kann. In Fällen, in denen Anforderungen zum Herunterfahren verarbeitet werden sollen, definieren Sie ein typisiertes [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx)-Argument.  Ein `CancellationToken` wird angegeben, um zu signalisieren, dass das Herunterfahren des Hosts ausgelöst wird.

```csharp
public static class CancellationTokenExample
{
    [FunctionName("BlobCopy")]
    public static async Task RunAsync(
        [BlobTrigger("sample-images/{blobName}")] Stream blobInput,
        [Blob("sample-images-copies/{blobName}", FileAccess.Write)] Stream blobOutput,
        CancellationToken token)
    {
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
}
```

## <a name="environment-variables"></a>Umgebungsvariablen

Verwenden Sie `System.Environment.GetEnvironmentVariable`zum Abrufen einer Umgebungsvariablen oder zum Abrufen des Werts einer App-Einstellung, wie im folgenden Codebeispiel zu sehen:

```csharp
public static class EnvironmentVariablesExample
{
    [FunctionName("GetEnvironmentVariables")]
    public static void Run([TimerTrigger("0 */5 * * * *")]TimerInfo myTimer, TraceWriter log)
    {
        log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
        log.Info(GetEnvironmentVariable("AzureWebJobsStorage"));
        log.Info(GetEnvironmentVariable("WEBSITE_SITE_NAME"));
    }

    public static string GetEnvironmentVariable(string name)
    {
        return name + ": " +
            System.Environment.GetEnvironmentVariable(name, EnvironmentVariableTarget.Process);
    }
}
```

## <a name="binding-at-runtime"></a>Binden zur Laufzeit

In C# und anderen .NET-Sprachen können Sie ein [imperatives](https://en.wikipedia.org/wiki/Imperative_programming) Bindungsmuster verwenden, im Gegensatz zu den [*deklarativen* ](https://en.wikipedia.org/wiki/Declarative_programming) Bindungen in Attributen. Imperative Bindung eignet sich, wenn Bindungsparameter zur Laufzeit statt zur Entwurfszeit berechnet werden müssen. Mit diesem Muster ist die Bindung an unterstützte Eingabe- und Ausgabebindungen direkt im Funktionscode möglich.

Definieren Sie eine imperative Bindung wie folgt:

- Schließen Sie für die gewünschten imperativen Bindungen **kein** Attribut in die Funktionssignatur ein.
- Übergeben Sie den Eingabeparameter [`Binder binder`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/Bindings/Runtime/Binder.cs) oder [`IBinder binder`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IBinder.cs).
- Verwenden Sie das folgende C#-Muster, um die Datenbindung auszuführen.

  ```cs
  using (var output = await binder.BindAsync<T>(new BindingTypeAttribute(...)))
  {
      ...
  }
  ```

  `BindingTypeAttribute` ist das .NET-Attribut, das die Bindung definiert, und `T` ist ein Eingabe- oder Ausgabetyp, der von diesem Bindungstyp unterstützt wird. `T` darf kein `out`-Parametertyp sein (wie etwa `out JObject`). Die ausgehende Bindung für die Tabelle „Mobile Apps“ unterstützt z. B. [sechs Ausgabetypen](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs#L17-L22), Sie können aber nur [ICollector<T>](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) oder [IAsyncCollector<T>](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) mit imperativer Bindung verwenden.

### <a name="single-attribute-example"></a>Beispiel mit einem einzigen Attribut

Mit dem folgenden Beispielcode wird eine [ausgehende Speicherblob-Bindung](functions-bindings-storage-blob.md#output) mit einem Blobpfad erstellt, der zur Laufzeit definiert wird. Dann wird eine Zeichenfolge in das Blob geschrieben.

```cs
public static class IBinderExample
{
    [FunctionName("CreateBlobUsingBinder")]
    public static void Run(
        [QueueTrigger("myqueue-items-source-4")] string myQueueItem,
        IBinder binder,
        TraceWriter log)
    {
        log.Info($"CreateBlobUsingBinder function processed: {myQueueItem}");
        using (var writer = binder.Bind<TextWriter>(new BlobAttribute(
                    $"samples-output/{myQueueItem}", FileAccess.Write)))
        {
            writer.Write("Hello World!");
        };
    }
}
```

[BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs) definiert die Eingabe- oder Ausgabebindung für den [Speicherblob](functions-bindings-storage-blob.md), und [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) ist ein unterstützter Ausgabenbindungstyp.

### <a name="multiple-attribute-example"></a>Beispiel mit mehreren Attributen

Im vorherigen Beispiel wird die App-Einstellung für die Verbindungszeichenfolge (`AzureWebJobsStorage`) des Hauptspeicherkontos der Funktions-App abgerufen. Sie können eine benutzerdefinierte App-Einstellung angeben, die für das Storage-Konto verwendet werden soll, indem Sie [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) hinzufügen und das Attributarray an `BindAsync<T>()` übergeben. Verwenden Sie einen `Binder`-Parameter, nicht `IBinder`.  Beispiel: 

```cs
public static class IBinderExampleMultipleAttributes
{
    [FunctionName("CreateBlobInDifferentStorageAccount")]
    public async static Task RunAsync(
            [QueueTrigger("myqueue-items-source-binder2")] string myQueueItem,
            Binder binder,
            TraceWriter log)
    {
        log.Info($"CreateBlobInDifferentStorageAccount function processed: {myQueueItem}");
        var attributes = new Attribute[]
        {
        new BlobAttribute($"samples-output/{myQueueItem}", FileAccess.Write),
        new StorageAccountAttribute("MyStorageAccount")
        };
        using (var writer = await binder.BindAsync<TextWriter>(attributes))
        {
            await writer.WriteAsync("Hello World!!");
        }
    }
}
```

## <a name="triggers-and-bindings"></a>Trigger und Bindungen 

Die folgende Tabelle enthält die Trigger- und Bindungsattribute, die in einem Azure Functions-Klassenbibliotheksprojekt verfügbar sind. Alle Attribute sind im Namespace `Microsoft.Azure.WebJobs`.

| Trigger | Eingabe | Output|
|------   | ------    | ------  |
| [BlobTrigger](functions-bindings-storage-blob.md#trigger---attributes)| [Blob](functions-bindings-storage-blob.md#input---attributes)| [Blob](functions-bindings-storage-blob.md#output---attributes)|
| [CosmosDBTrigger](functions-bindings-cosmosdb.md#trigger---attributes)| [DocumentDB](functions-bindings-cosmosdb.md#input---attributes)| [DocumentDB](functions-bindings-cosmosdb.md#output---attributes) |
| [EventHubTrigger](functions-bindings-event-hubs.md#trigger---attributes)|| [EventHub](functions-bindings-event-hubs.md#output---attributes) |
| [HTTPTrigger](functions-bindings-http-webhook.md#trigger---attributes)|||
| [QueueTrigger](functions-bindings-storage-queue.md#trigger---attributes)|| [Warteschlange](functions-bindings-storage-queue.md#output---attributes) |
| [ServiceBusTrigger](functions-bindings-service-bus.md#trigger---attributes)|| [ServiceBus](functions-bindings-service-bus.md#output---attributes) |
| [TimerTrigger](functions-bindings-timer.md#attributes) | ||
| |[ApiHubFile](functions-bindings-external-file.md)| [ApiHubFile](functions-bindings-external-file.md)|
| |[MobileTable](functions-bindings-mobile-apps.md#input---attributes)| [MobileTable](functions-bindings-mobile-apps.md#output---attributes) | 
| |[Tabelle](functions-bindings-storage-table.md#input---attributes)| [Tabelle](functions-bindings-storage-table.md#output---attributes)  | 
| ||[NotificationHub](functions-bindings-notification-hubs.md#attributes) |
| ||[SendGrid](functions-bindings-sendgrid.md#attributes) |
| ||[Twilio](functions-bindings-twilio.md#attributes)| 

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Weitere Informationen zu Triggern und Bindungen](functions-triggers-bindings.md)

> [!div class="nextstepaction"]
> [Weitere Informationen zu bewährten Methoden für Azure Functions](functions-best-practices.md)
