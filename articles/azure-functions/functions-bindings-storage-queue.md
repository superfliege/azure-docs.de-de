---
title: "Azure Functions – Queue Storage-Bindungen"
description: Hier erfahren Sie, wie Sie den Azure Queue Storage-Trigger und die entsprechende Ausgabebindung in Azure Functions verwenden.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: Azure Functions, Funktionen, Ereignisverarbeitung, dynamisches Compute, serverlose Architektur
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/23/2017
ms.author: glenga
ms.openlocfilehash: 9cf506d571c8d67a1e48ce34860db3dbc3445509
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="azure-functions-queue-storage-bindings"></a>Azure Functions – Queue Storage-Bindungen

In diesem Artikel erfahren Sie, wie Sie Azure Queue Storage-Bindungen in Azure Functions verwenden. Azure Functions unterstützt Trigger- und Ausgabebindungen für Warteschlangen.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="queue-storage-trigger"></a>Queue Storage-Trigger

Verwenden Sie den Warteschlangentrigger, um eine Funktion zu starten, wenn bei einer Warteschlange ein neues Element eingeht. Die Warteschlangennachricht wird als Eingabe für die Funktion bereitgestellt.

## <a name="trigger---example"></a>Trigger: Beispiel

Sehen Sie sich das sprachspezifische Beispiel an:

* [Vorkompilierter C#-Code](#trigger---c-example)
* [C#-Skript](#trigger---c-script-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-example"></a>Trigger: C#-Beispiel

Der [vorkompilierte C#-Code](functions-dotnet-class-library.md) des folgenden Beispiels fragt die Warteschlange `myqueue-items` ab und schreibt ein Protokoll, sobald ein neues Warteschlangenelement verarbeitet wird.

```csharp
public static class QueueFunctions
{
    [FunctionName("QueueTrigger")]
    public static void QueueTrigger(
        [QueueTrigger("myqueue-items")] string myQueueItem, 
        TraceWriter log)
    {
        log.Info($"C# function processed: {myQueueItem}");
    }
}
```

### <a name="trigger---c-script-example"></a>Trigger: C#-Skriptbeispiel

Das folgende Beispiel zeigt einen Blobtrigger in einer Datei vom Typ *function.json* sowie [C#-Skriptcode](functions-reference-csharp.md), der die Bindung verwendet. Die Funktion fragt die Warteschlange `myqueue-items` ab und schreibt ein Protokoll, sobald ein Warteschlangenelement verarbeitet wird.

Die Datei *function.json* sieht wie folgt aus:

```json
{
    "disabled": false,
    "bindings": [
        {
            "type": "queueTrigger",
            "direction": "in",
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"MyStorageConnectionAppSetting"
        }
    ]
}
```

Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#trigger---configuration).

Der C#-Skriptcode sieht wie folgt aus:

```csharp
#r "Microsoft.WindowsAzure.Storage"

using Microsoft.WindowsAzure.Storage.Queue;
using System;

public static void Run(CloudQueueMessage myQueueItem, 
    DateTimeOffset expirationTime, 
    DateTimeOffset insertionTime, 
    DateTimeOffset nextVisibleTime,
    string queueTrigger,
    string id,
    string popReceipt,
    int dequeueCount,
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem.AsString}\n" +
        $"queueTrigger={queueTrigger}\n" +
        $"expirationTime={expirationTime}\n" +
        $"insertionTime={insertionTime}\n" +
        $"nextVisibleTime={nextVisibleTime}\n" +
        $"id={id}\n" +
        $"popReceipt={popReceipt}\n" + 
        $"dequeueCount={dequeueCount}");
}
```

Im Abschnitt [Verwendung](#trigger---usage) finden Sie weitere Informationen zu `myQueueItem` (benannt durch die Eigenschaft `name` in „function.json“).  Alle anderen gezeigten Variablen werden im Abschnitt [Nachrichtenmetadaten](#trigger---message-metadata) erläutert.

### <a name="trigger---javascript-example"></a>Trigger: JavaScript-Beispiel

Das folgende Beispiel zeigt eine Blobtriggerbindung in einer Datei vom Typ *function.json* sowie eine [JavaScript-Funktion](functions-reference-node.md), die die Bindung verwendet. Die Funktion fragt die Warteschlange `myqueue-items` ab und schreibt ein Protokoll, sobald ein Warteschlangenelement verarbeitet wird.

Die Datei *function.json* sieht wie folgt aus:

```json
{
    "disabled": false,
    "bindings": [
        {
            "type": "queueTrigger",
            "direction": "in",
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"MyStorageConnectionAppSetting"
        }
    ]
}
```

Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#trigger---configuration).

Der JavaScript-Code sieht wie folgt aus:

```javascript
module.exports = function (context) {
    context.log('Node.js queue trigger function processed work item', context.bindings.myQueueItem);
    context.log('queueTrigger =', context.bindingData.queueTrigger);
    context.log('expirationTime =', context.bindingData.expirationTime);
    context.log('insertionTime =', context.bindingData.insertionTime);
    context.log('nextVisibleTime =', context.bindingData.nextVisibleTime);
    context.log('id =', context.bindingData.id);
    context.log('popReceipt =', context.bindingData.popReceipt);
    context.log('dequeueCount =', context.bindingData.dequeueCount);
    context.done();
};
```

Im Abschnitt [Verwendung](#trigger---usage) finden Sie weitere Informationen zu `myQueueItem` (benannt durch die Eigenschaft `name` in „function.json“).  Alle anderen gezeigten Variablen werden im Abschnitt [Nachrichtenmetadaten](#trigger---message-metadata) erläutert.

## <a name="trigger---attributes-for-precompiled-c"></a>Trigger: Attribute für vorkompilierten C#-Code
 
Verwenden Sie für Funktionen mit [vorkompiliertem C#-Code](functions-dotnet-class-library.md) die folgenden Attribute, um einen Warteschlangentrigger zu konfigurieren:

* [QueueTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueTriggerAttribute.cs) (definiert im NuGet-Paket [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs))

  Der Konstruktor des Attributs akzeptiert den Namen der zu überwachenden Warteschlange, wie im folgenden Beispiel zu sehen:

  ```csharp
  [FunctionName("QueueTrigger")]
  public static void Run(
      [QueueTrigger("myqueue-items")] string myQueueItem, 
      TraceWriter log)
  ```

  Durch Festlegen der Eigenschaft `Connection` können Sie das zu verwendende Speicherkonto angeben, wie im folgenden Beispiel zu sehen:

  ```csharp
  [FunctionName("QueueTrigger")]
  public static void Run(
      [QueueTrigger("myqueue-items", Connection = "StorageConnectionAppSetting")] string myQueueItem, 
      TraceWriter log)
  ```
 
* [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) (definiert im NuGet-Paket [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs))

  Eine weitere Möglichkeit zum Angeben des zu verwendenden Speicherkontos. Der Konstruktor akzeptiert den Namen einer App-Einstellung mit einer Speicherverbindungszeichenfolge. Das Attribut kann auf Parameter-, Methoden- oder Klassenebene angewendet werden. Das folgende Beispiel zeigt die Anwendung auf Klassen- und Methodenebene:

  ```csharp
  [StorageAccount("ClassLevelStorageAppSetting")]
  public static class AzureFunctions
  {
      [FunctionName("QueueTrigger")]
      [StorageAccount("FunctionLevelStorageAppSetting")]
      public static void Run( //...
  ```

Das zu verwendende Speicherkonto wird anhand von Folgendem bestimmt (in der angegebenen Reihenfolge):

* Die Eigenschaft `Connection` des Attributs `QueueTrigger`.
* Das Attribut `StorageAccount`, das auf den gleichen Parameter angewendet wird wie das Attribut `QueueTrigger`.
* Das Attribut `StorageAccount`, das auf die Funktion angewendet wird.
* Das Attribut `StorageAccount`, das auf die Klasse angewendet wird.
* Die App-Einstellung „AzureWebJobsStorage“.

## <a name="trigger---configuration"></a>Trigger: Konfiguration

Die folgende Tabelle gibt Aufschluss über die Bindungskonfigurationseigenschaften, die Sie in der Datei *function.json* und im Attribut `QueueTrigger` festlegen:

|Eigenschaft von „function.json“ | Attributeigenschaft |Beschreibung|
|---------|---------|----------------------|
|**type** | –| Muss auf `queueTrigger` festgelegt sein. Diese Eigenschaft wird automatisch festgelegt, wenn Sie den Trigger im Azure Portal erstellen.|
|**direction**| – | Nur in der Datei *function.json*. Muss auf `in` festgelegt sein. Diese Eigenschaft wird automatisch festgelegt, wenn Sie den Trigger im Azure Portal erstellen. |
|**name** | – |Der Name der Variablen, die die Warteschlange im Funktionscode darstellt.  | 
|**queueName** | **QueueName**| Der Name der abzufragenden Warteschlange. | 
|**connection** | **Connection** |Der Name einer App-Einstellung, die die Storage-Verbindungszeichenfolge für diese Bindung enthält. Falls der Name der App-Einstellung mit „AzureWebJobs“ beginnt, können Sie hier nur den Rest des Namens angeben. Wenn Sie `connection` also beispielsweise auf „MyStorage“ festlegen, sucht die Functions-Laufzeit nach einer App-Einstellung namens „AzureWebJobsMyStorage“. Ohne Angabe für `connection` verwendet die Functions-Laufzeit die standardmäßige Storage-Verbindungszeichenfolge aus der App-Einstellung `AzureWebJobsStorage`.<br/>Wenn Sie lokal entwickeln, werden App-Einstellungen in den Werten der [Datei „local.settings.json“](functions-run-local.md#local-settings-file) gespeichert.|

## <a name="trigger---usage"></a>Trigger: Verwendung
 
Verwenden Sie in C# und C#-Skripts einen Methodenparameter wie `Stream paramName`, um auf die Blobdaten zuzugreifen. In C#-Skripts ist `paramName` der Wert, der in der Eigenschaft `name` von *function.json* angegeben ist. Eine Bindung kann mit folgenden Typen erstellt werden:

* POCO-Objekt: Die Functions-Laufzeit deserialisiert eine JSON-Nutzlast im POCO-Objekt. 
* `string`
* `byte[]`
* [CloudQueueMessage]

Verwenden Sie in JavaScript `context.bindings.<name>`, um auf die Nutzlast des Warteschlangenelements zuzugreifen. Falls es sich um eine JSON-Nutzlast handelt, wird sie in ein Objekt deserialisiert.

## <a name="trigger---message-metadata"></a>Trigger: Nachrichtenmetadaten

Der Warteschlangentrigger stellt mehrere Metadateneigenschaften bereit. Diese Eigenschaften können als Teil der Bindungsausdrücke in anderen Bindungen oder als Parameter im Code verwendet werden. Die Werte haben die gleiche Semantik wie [CloudQueueMessage](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.queue.cloudqueuemessage).

|Eigenschaft|Typ|Beschreibung|
|--------|----|-----------|
|`QueueTrigger`|`string`|Die Warteschlangennutzlast (bei einer gültigen Zeichenfolge). Handelt es sich bei der Warteschlangennutzlast um eine Zeichenfolge, hat `QueueTrigger` den gleichen Wert wie die Variable, die durch die Eigenschaft `name` in *function.json* benannt wird.|
|`DequeueCount`|`int`|Gibt an, wie oft diese Nachricht aus der Warteschlange entfernt wurde.|
|`ExpirationTime`|`DateTimeOffset?`|Die Zeit, zu der die Nachricht abläuft.|
|`Id`|`string`|ID der Warteschlangennachricht|
|`InsertionTime`|`DateTimeOffset?`|Die Zeit, zu der die Nachricht der Warteschlange hinzugefügt wurde.|
|`NextVisibleTime`|`DateTimeOffset?`|Die Zeit, zu der die Nachricht als Nächstes sichtbar wird.|
|`PopReceipt`|`string`|Die POP-Bestätigung der Nachricht.|

## <a name="trigger---poison-messages"></a>Trigger: Nicht verarbeitbare Nachrichten

Falls eine Funktion des Warteschlangentriggers nicht erfolgreich ausgeführt werden kann, versucht Azure Functions für eine bestimmte Warteschlangennachricht bis zu fünf Mal (einschließlich des ersten Versuchs), die Funktion auszuführen. Sind alle fünf Versuche nicht erfolgreich, fügt die Functions-Laufzeit einer Warteschlange namens *&lt;Name der Originalwarteschlange>-poison* eine Nachricht hinzu. Sie können eine Funktion schreiben, um Nachrichten aus der Warteschlange für nicht verarbeitete Nachrichten zu verarbeiten, indem Sie diese protokollieren oder eine Benachrichtigung senden, dass ein manueller Eingriff erforderlich ist.

Überprüfen Sie zur manuellen Behandlung nicht verarbeitbarer Nachrichten den Wert [dequeueCount](#trigger---message-metadata) der Warteschlangennachricht.

## <a name="trigger---hostjson-properties"></a>Trigger: Eigenschaften von „host.json“

Die Datei [host.json](functions-host-json.md#queues) enthält Einstellungen, mit denen das Verhalten des Warteschlangentriggers gesteuert werden kann.

[!INCLUDE [functions-host-json-queues](../../includes/functions-host-json-queues.md)]

## <a name="queue-storage-output-binding"></a>Queue Storage-Ausgabebindung

Verwenden Sie die Azure Queue Storage-Ausgabebindung, um Nachrichten in eine Warteschlange zu schreiben.

## <a name="output---example"></a>Ausgabe: Beispiel

Sehen Sie sich das sprachspezifische Beispiel an:

* [Vorkompilierter C#-Code](#output---c-example)
* [C#-Skript](#output---c-script-example)
* [JavaScript](#output---javascript-example)

### <a name="output---c-example"></a>Ausgabe: C#-Beispiel

Der [vorkompilierte C#-Code](functions-dotnet-class-library.md) des folgenden Beispiels erstellt eine Warteschlangennachricht für jede empfangene HTTP-Anforderung.

```csharp
[StorageAccount("AzureWebJobsStorage")]
public static class QueueFunctions
{
    [FunctionName("QueueOutput")]
    [return: Queue("myqueue-items")]
    public static string QueueOutput([HttpTrigger] dynamic input,  TraceWriter log)
    {
        log.Info($"C# function processed: {input.Text}");
        return input.Text;
    }
}
```

### <a name="output---c-script-example"></a>Ausgabe: C#-Skriptbeispiel

Das folgende Beispiel zeigt einen Blobtrigger in einer Datei vom Typ *function.json* sowie [C#-Skriptcode](functions-reference-csharp.md), der die Bindung verwendet. Die Funktion erstellt ein Warteschlangenelement mit einer POCO-Nutzlast für jede empfangene HTTP-Anforderung.

Die Datei *function.json* sieht wie folgt aus:

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "authLevel": "function",
      "name": "input"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "return"
    },
    {
      "type": "queue",
      "direction": "out",
      "name": "$return",
      "queueName": "outqueue",
      "connection": "MyStorageConnectionAppSetting",
    }
  ]
}
``` 

Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#output---configuration).

Der folgende C#-Skriptcode erstellt eine einzelne Warteschlangennachricht:

```cs
public class CustomQueueMessage
{
    public string PersonName { get; set; }
    public string Title { get; set; }
}

public static CustomQueueMessage Run(CustomQueueMessage input, TraceWriter log)
{
    return input;
}
```

Mithilfe eines Parameters vom Typ `ICollector` oder `IAsyncCollector` können mehrere Nachrichten gleichzeitig gesendet werden. Der folgende C#-Skriptcode sendet mehrere Nachrichten – eine mit den HTTP-Anforderungsdaten und eine mit hartcodierten Werten:

```cs
public static void Run(
    CustomQueueMessage input, 
    ICollector<CustomQueueMessage> myQueueItem, 
    TraceWriter log)
{
    myQueueItem.Add(input);
    myQueueItem.Add(new CustomQueueMessage { PersonName = "You", Title = "None" });
}
```

### <a name="output---javascript-example"></a>Ausgabe: JavaScript-Beispiel

Das folgende Beispiel zeigt eine Blobtriggerbindung in einer Datei vom Typ *function.json* sowie eine [JavaScript-Funktion](functions-reference-node.md), die die Bindung verwendet. Die Funktion erstellt ein Warteschlangenelement für jede empfangene HTTP-Anforderung.

Die Datei *function.json* sieht wie folgt aus:

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "authLevel": "function",
      "name": "input"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "return"
    },
    {
      "type": "queue",
      "direction": "out",
      "name": "$return",
      "queueName": "outqueue",
      "connection": "MyStorageConnectionAppSetting",
    }
  ]
}
``` 

Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#output---configuration).

Der JavaScript-Code sieht wie folgt aus:

```javascript
module.exports = function (context, input) {
    context.done(null, input.body);
};
```

Wenn Sie mehrere Nachrichten gleichzeitig senden möchten, können Sie ein Nachrichtenarray für die Ausgabebindung `myQueueItem` definieren. Der folgende JavaScript-Code sendet zwei Warteschlangennachrichten mit hartcodierten Werten für jede empfangene HTTP-Anforderung.

```javascript
module.exports = function(context) {
    context.bindings.myQueueItem = ["message 1","message 2"];
    context.done();
};
```

## <a name="output---attributes-for-precompiled-c"></a>Ausgabe: Attribute für vorkompilierten C#-Code
 
Verwenden Sie für Funktionen mit [vorkompiliertem C#-Code](functions-dotnet-class-library.md) das im NuGet-Paket [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs) definierte Attribut [QueueAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueAttribute.cs).

Das Attribut gilt für einen Parameter vom Typ `out` oder für den Rückgabewert der Funktion. Der Konstruktor des Attributs akzeptiert den Namen der Warteschlange, wie im folgenden Beispiel zu sehen:

```csharp
[FunctionName("QueueOutput")]
[return: Queue("myqueue-items")]
public static string Run([HttpTrigger] dynamic input,  TraceWriter log)
```

Durch Festlegen der Eigenschaft `Connection` können Sie das zu verwendende Speicherkonto angeben, wie im folgenden Beispiel zu sehen:

```csharp
[FunctionName("QueueOutput")]
[return: Queue("myqueue-items, Connection = "StorageConnectionAppSetting")]
public static string Run([HttpTrigger] dynamic input,  TraceWriter log)
```

Mit dem Attribut `StorageAccount` können Sie das Speicherkonto auf Klassen-, Methoden- oder Parameterebene angeben. Weitere Informationen finden Sie unter [Trigger: Attribute für vorkompilierten C#-Code](#trigger---attributes-for-precompiled-c).

## <a name="output---configuration"></a>Ausgabe: Konfiguration

Die folgende Tabelle gibt Aufschluss über die Bindungskonfigurationseigenschaften, die Sie in der Datei *function.json* und im Attribut `Queue` festlegen:

|Eigenschaft von „function.json“ | Attributeigenschaft |Beschreibung|
|---------|---------|----------------------|
|**type** | – | Muss auf `queue` festgelegt sein. Diese Eigenschaft wird automatisch festgelegt, wenn Sie den Trigger im Azure Portal erstellen.|
|**direction** | – | Muss auf `out` festgelegt sein. Diese Eigenschaft wird automatisch festgelegt, wenn Sie den Trigger im Azure Portal erstellen. |
|**name** | – | Der Name der Variablen, die die Warteschlange im Funktionscode darstellt. Legen Sie diesen Wert auf `$return` fest, um auf den Rückgabewert der Funktion zu verweisen.| 
|**queueName** |**QueueName** | Der Name der Warteschlange. | 
|**connection** | **Connection** |Der Name einer App-Einstellung, die die Storage-Verbindungszeichenfolge für diese Bindung enthält. Falls der Name der App-Einstellung mit „AzureWebJobs“ beginnt, können Sie hier nur den Rest des Namens angeben. Wenn Sie `connection` also beispielsweise auf „MyStorage“ festlegen, sucht die Functions-Laufzeit nach einer App-Einstellung namens „AzureWebJobsMyStorage“. Ohne Angabe für `connection` verwendet die Functions-Laufzeit die standardmäßige Storage-Verbindungszeichenfolge aus der App-Einstellung `AzureWebJobsStorage`.<br>Wenn Sie lokal entwickeln, werden App-Einstellungen in den Werten der [Datei „local.settings.json“](functions-run-local.md#local-settings-file) gespeichert.|

## <a name="output---usage"></a>Ausgabe: Verwendung
 
Verwenden Sie in C# und C#-Skripts einen Methodenparameter wie `out T paramName`, um eine einzelne Warteschlangennachricht zu schreiben. In C#-Skripts ist `paramName` der Wert, der in der Eigenschaft `name` von *function.json* angegeben ist. Anstelle eines Parameters vom Typ `out` können Sie den Rückgabetyp der Methode verwenden, und `T` kann einer der folgenden Typen sein:

* Ein als JSON serialisierbares POCO
* `string`
* `byte[]`
* [CloudQueueMessage] 

Verwenden Sie einen der folgenden Typen, um in C# und C#-Skripts mehrere Warteschlangennachrichten zu schreiben: 

* `ICollector<T>` oder `IAsyncCollector<T>`
* [CloudQueue](/dotnet/api/microsoft.windowsazure.storage.queue.cloudqueue)

Verwenden Sie in JavaScript-Funktionen `context.bindings.<name>`, um auf die Ausgabewarteschlangennachricht zuzugreifen. Für die Nutzlast des Warteschlangenelements kann eine Zeichenfolge oder ein JSON-serialisierbares Objekt verwendet werden.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen einer Funktion, die durch Azure Queue Storage ausgelöst wird](functions-create-storage-queue-triggered-function.md)

> [!div class="nextstepaction"]
> [Hinzufügen von Meldungen in die Warteschlange von Azure Storage mithilfe von Functions](functions-integrate-storage-queue-output-binding.md)

> [!div class="nextstepaction"]
> [Konzepte für Azure Functions-Trigger und -Bindungen](functions-triggers-bindings.md)

<!-- LINKS -->

[CloudQueueMessage]: /dotnet/api/microsoft.windowsazure.storage.queue.cloudqueuemessage