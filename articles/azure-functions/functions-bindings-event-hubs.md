---
title: "Azure Event Hubs-Bindungen für Azure Functions"
description: Erfahren Sie, wie Azure Event Hubs-Bindungen in Azure Functions verwendet werden.
services: functions
documentationcenter: na
author: wesmc7777
manager: cfowler
editor: 
tags: 
keywords: Azure Functions, Funktionen, Ereignisverarbeitung, dynamisches Compute, serverlose Architektur
ms.assetid: daf81798-7acc-419a-bc32-b5a41c6db56b
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/08/2017
ms.author: wesmc
<<<<<<< HEAD
ms.openlocfilehash: 084d3e4244bc6f19797fadab93265291494cf066
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
=======
<<<<<<< HEAD
ms.openlocfilehash: 084d3e4244bc6f19797fadab93265291494cf066
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
=======
ms.openlocfilehash: aee7352ce6f8dd854ce0c6c61c5485fb9a35bb23
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2018
>>>>>>> 6def4612c80a1e9bab4008c57d68ccdbec8d0794
>>>>>>> bb0780f466c4ede2eb00246e1afe01b19bb40688
---
# <a name="azure-event-hubs-bindings-for-azure-functions"></a>Azure Event Hubs-Bindungen für Azure Functions

Dieser Artikel erläutert das Arbeiten mit [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md)-Bindungen für Azure Functions. Azure Functions unterstützt Trigger- und Ausgabebindungen für Event Hubs.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="trigger"></a>Trigger

Der Event Hubs-Trigger kann verwendet werden, um auf ein Ereignis zu reagieren, das an einen Event Hub-Datenstrom gesendet wird. Sie benötigen Lesezugriff auf den Event Hub, um den Trigger einzurichten.

Wenn eine Event Hubs-Triggerfunktion ausgelöst wird, wird die Meldung, die sie auslöst, als Zeichenfolge an die Funktion übergeben.

## <a name="trigger---scaling"></a>Trigger – Skalierung

Jede Instanz einer von Event Hub ausgelösten Funktion wird von nur einer EventProcessorHost-Instanz (EPH) unterstützt. Event Hubs stellt sicher, dass nur ein EPH einen Lease für eine bestimmte Partition erhalten kann.

Nehmen Sie beispielsweise an, wir beginnen mit dem folgenden Setup und den folgenden Annahmen für einen Event Hub:

1. 10 Partitionen.
1. 1.000 gleichmäßig auf alle Partitionen verteilte Ereignisse = > 100 Nachrichten in jeder Partition.

Wenn Ihre Funktion zuerst aktiviert wird, gibt es nur eine Instanz der Funktion. Wir nennen diese Funktionsinstanz „Function_0“. „Function_0“ wird einen EPH aufweisen, der es erreicht, einen Lease für alle 10 Partitionen zu erhalten. Es beginnt mit dem Lesen von Ereignissen von den Partitionen 0-9. Von diesem Punkt an wird eines der folgenden Ereignisse eintreten:

* **Es ist nur eine Instanz der Funktion erforderlich** – „Function_0“ ist in der Lage, alle 1.000 zu verarbeiten, bevor die Skalierungslogik von Azure Functions einsetzt. Daher werden alle 1.000 Nachrichten von „Function_0“ verarbeitet.

* **Es wird eine weitere Funktionsinstanz hinzugefügt** – Die Skalierungslogik von Azure Functions bestimmt, dass „Function_0“ über mehr Nachrichten verfügt, als sie verarbeiten kann. Daher wird eine neue Instanz, „Function_1“, erstellt. Event Hubs erkennt, dass eine neue EPH-Instanz versucht, Nachrichten zu lesen. Event Hubs starten den Lastenausgleich für die Partitionen über die EPH-Instanzen hinweg, z. B. werden die Partitionen 0-4 der „Function_0“ und die Partitionen 5-9 der „Function_1“ zugewiesen. 

* **N weitere Funktionsinstanzen hinzufügen** – Die Skalierungslogik von Azure Functions bestimmt, dass sowohl „Function_0“ als auch „Function_1“ mehr Nachrichten aufweisen, als sie verarbeiten können. Es wird erneut für „Function_2...N“ skaliert, wobei N größer ist als die Event Hub-Partitionen. Event Hubs wird für die Partitionen über Instanzen von „Function_0...9“ hinweg einen Lastenausgleich vornehmen.

Das Besondere an der aktuellen Skalierungslogik von Azure Functions ist die Tatsache, dass N größer ist als die Anzahl der Partitionen. Auf diese Weise wird sichergestellt, dass es immer Instanzen von EPH gibt, die leicht verfügbar sind, um schnell eine Sperre für die Partition(en) zu erhalten, sobald sie über andere Instanzen verfügbar werden. Den Benutzern werden nur die Ressourcen berechnet, die bei der Ausführung der Funktionsinstanz in Anspruch genommen werden, und nicht die Kosten für diese übermäßige Bereitstellung.

Wenn alle Funktionsausführungen fehlerfrei verlaufen, werden Prüfpunkte dem zugehörigen Speicherkonto hinzugefügt. Wenn die Prüfpunkte erfolgreich erstellt wurden, sollten alle 1.000 Nachrichten nie wieder abgerufen werden.

## <a name="trigger---example"></a>Trigger: Beispiel

Sehen Sie sich das sprachspezifische Beispiel an:

* [C#](#trigger---c-example)
* [C#-Skript (.csx)](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-example"></a>Trigger: C#-Beispiel

Das folgende Beispiel zeigt eine [C#-Funktion](functions-dotnet-class-library.md), die den Nachrichtentext des Event Hub-Triggers protokolliert.

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

Um den Zugriff auf die Ereignismetadaten zu erhalten, muss eine Bindung an ein [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata)-Objekt erfolgen (erfordert eine `using`-Anweisung für `Microsoft.ServiceBus.Messaging`).

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] EventData myEventHubMessage, TraceWriter log)
{
    log.Info($"{Encoding.UTF8.GetString(myEventHubMessage.GetBytes())}");
}
```
Um Ereignisse in einem Batch zu empfangen, deklarieren Sie `string` oder `EventData` als ein Array:

```cs
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] string[] eventHubMessages, TraceWriter log)
{
    foreach (var message in eventHubMessages)
    {
        log.Info($"C# Event Hub trigger function processed a message: {message}");
    }
}
```

### <a name="trigger---c-script-example"></a>Trigger: C#-Skriptbeispiel

Das folgende Beispiel zeigt eine Event Hub-Triggerbindung in einer Datei *function.json* sowie eine [C#-Skriptfunktion](functions-reference-csharp.md), die die Bindung verwendet. Die Funktion protokolliert den Nachrichtentext des Event Hub-Triggers.

Bindungsdaten in der Datei *function.json*:

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```
Der C#-Skriptcode sieht wie folgt aus:

```cs
using System;

public static void Run(string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

Um Zugriff auf die Ereignismetadaten zu erhalten, muss eine Bindung an ein [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata)-Objekt erfolgen (erfordert eine using-Anweisung für `Microsoft.ServiceBus.Messaging`).

```cs
#r "Microsoft.ServiceBus"
using System.Text;
using Microsoft.ServiceBus.Messaging;

public static void Run(EventData myEventHubMessage, TraceWriter log)
{
    log.Info($"{Encoding.UTF8.GetString(myEventHubMessage.GetBytes())}");
}
```

Um Ereignisse in einem Batch zu empfangen, deklarieren Sie `string` oder `EventData` als ein Array:

```cs
public static void Run(string[] eventHubMessages, TraceWriter log)
{
    foreach (var message in eventHubMessages)
    {
        log.Info($"C# Event Hub trigger function processed a message: {message}");
    }
}
```

### <a name="trigger---f-example"></a>Trigger: F#-Beispiel

Das folgende Beispiel zeigt eine Event Hub-Triggerbindung in einer Datei vom Typ *function.json* sowie eine [F#-Funktion](functions-reference-fsharp.md), die die Bindung verwendet. Die Funktion protokolliert den Nachrichtentext des Event Hub-Triggers.

Bindungsdaten in der Datei *function.json*:

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

Der F#-Code lautet wie folgt:

```fsharp
let Run(myEventHubMessage: string, log: TraceWriter) =
    log.Info(sprintf "F# eventhub trigger function processed work item: %s" myEventHubMessage)
```

### <a name="trigger---javascript-example"></a>Trigger: JavaScript-Beispiel

Das folgende Beispiel zeigt eine Event Hub-Triggerbindung in einer Datei vom Typ *function.json* sowie eine [JavaScript-Funktion](functions-reference-node.md), die die Bindung verwendet. Die Funktion protokolliert den Nachrichtentext des Event Hub-Triggers.

Bindungsdaten in der Datei *function.json*:

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

Der JavaScript-Code sieht wie folgt aus:

```javascript
module.exports = function (context, myEventHubMessage) {
    context.log('Node.js eventhub trigger function processed work item', myEventHubMessage);    
    context.done();
};
```

## <a name="trigger---attributes"></a>Trigger: Attribute

Verwenden Sie in [C#-Klassenbibliotheken](functions-dotnet-class-library.md) das im NuGet-Paket [Microsoft.Azure.WebJobs.ServiceBus](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus) definierte Attribut [EventHubTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubTriggerAttribute.cs).

Der Konstruktor des Attributs nimmt den Namen des Event Hubs, den Namen der Consumergruppe und den Namen einer App-Einstellung an, die die Verbindungszeichenfolge enthält. Weitere Informationen zu diesen Einstellungen finden Sie im Abschnitt [Triggerkonfiguration](#trigger---configuration). Dieses Beispiel zeigt ein Attribut `EventHubTriggerAttribute`:

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] string myEventHubMessage, TraceWriter log)
{
    ...
}
```

Ein vollständiges Beispiel finden Sie unter [Trigger: C#-Beispiel](#trigger---c-example).

## <a name="trigger---configuration"></a>Trigger: Konfiguration

Die folgende Tabelle gibt Aufschluss über die Bindungskonfigurationseigenschaften, die Sie in der Datei *function.json* und im Attribut `EventHubTrigger` festlegen:

|Eigenschaft von „function.json“ | Attributeigenschaft |BESCHREIBUNG|
|---------|---------|----------------------|
|**type** | – | Muss auf `eventHubTrigger` festgelegt sein. Diese Eigenschaft wird automatisch festgelegt, wenn Sie den Trigger im Azure Portal erstellen.|
|**direction** | – | Muss auf `in` festgelegt sein. Diese Eigenschaft wird automatisch festgelegt, wenn Sie den Trigger im Azure Portal erstellen. |
|**name** | – | Der Name der Variablen, die das Ereigniselement im Funktionscode darstellt. | 
|**path** |**EventHubName** | Der Name des Event Hubs. | 
|**consumerGroup** |**ConsumerGroup** | Eine optionale Eigenschaft, die zum Festlegen der [Consumergruppe](../event-hubs/event-hubs-features.md#event-consumers) verwendet wird, mit der Ereignisse im Hub abonniert werden. Wird sie nicht angegeben, wird die Consumergruppe `$Default` verwendet. | 
|**Verbindung** |**Connection** | Der Name einer App-Einstellung, die die Zeichenfolge für die Verbindung mit dem Namespace des Event Hubs enthält. Kopieren Sie diese Verbindungszeichenfolge, indem Sie für den [Namespace](../event-hubs/event-hubs-create.md#create-an-event-hubs-namespace) (nicht für den eigentlichen Event Hub) auf die Schaltfläche **Verbindungsinformationen** klicken. Diese Verbindungszeichenfolge muss mindestens über Leseberechtigungen verfügen, um den Trigger zu aktivieren.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---hostjson-properties"></a>Trigger: Eigenschaften von „host.json“

Die Datei [host.json](functions-host-json.md#eventhub) enthält Einstellungen, die das Verhalten des Event Hubs-Triggers steuern.

[!INCLUDE [functions-host-json-event-hubs](../../includes/functions-host-json-event-hubs.md)]

## <a name="output"></a>Output

Mit der Event Hubs-Ausgabebindung werden Ereignisse in einen Ereignisdatenstrom geschrieben. Um Ereignisse in einen Event Hub schreiben zu können, müssen Sie über eine Sendeberechtigung verfügen.

## <a name="output---example"></a>Ausgabe: Beispiel

Sehen Sie sich das sprachspezifische Beispiel an:

* [C#](#output---c-example)
* [C#-Skript (.csx)](#output---c-script-example)
* [F#](#output---f-example)
* [JavaScript](#output---javascript-example)

### <a name="output---c-example"></a>Ausgabe: C#-Beispiel

Das folgende Beispiel zeigt eine [C#-Funktion](functions-dotnet-class-library.md), die eine Nachricht in einen Event Hub schreibt. Dabei wird der Rückgabewert der Methode als Ausgabe verwendet:

```csharp
[FunctionName("EventHubOutput")]
[return: EventHub("outputEventHubMessage", Connection = "EventHubConnectionAppSetting")]
public static string Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    return $"{DateTime.Now}";
}
```

### <a name="output---c-script-example"></a>Ausgabe: C#-Skriptbeispiel

Das folgende Beispiel zeigt eine Event Hub-Triggerbindung in einer Datei *function.json* sowie eine [C#-Skriptfunktion](functions-reference-csharp.md), die die Bindung verwendet. Die Funktion schreibt eine Nachricht in einen Event Hub.

Bindungsdaten in der Datei *function.json*:

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```

Der folgende C#-Skriptcode erstellt eine Nachricht:

```cs
using System;

public static void Run(TimerInfo myTimer, out string outputEventHubMessage, TraceWriter log)
{
    String msg = $"TimerTriggerCSharp1 executed at: {DateTime.Now}";
    log.Verbose(msg);   
    outputEventHubMessage = msg;
}
```

Der folgende C#-Skriptcode erstellt mehrere Nachrichten:

```cs
public static void Run(TimerInfo myTimer, ICollector<string> outputEventHubMessage, TraceWriter log)
{
    string message = $"Event Hub message created at: {DateTime.Now}";
    log.Info(message);
    outputEventHubMessage.Add("1 " + message);
    outputEventHubMessage.Add("2 " + message);
}
```

### <a name="output---f-example"></a>Ausgabe: F#-Beispiel

Das folgende Beispiel zeigt eine Event Hub-Triggerbindung in einer Datei vom Typ *function.json* sowie eine [F#-Funktion](functions-reference-fsharp.md), die die Bindung verwendet. Die Funktion schreibt eine Nachricht in einen Event Hub.

Bindungsdaten in der Datei *function.json*:

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```

Der F#-Code lautet wie folgt:

```fsharp
let Run(myTimer: TimerInfo, outputEventHubMessage: byref<string>, log: TraceWriter) =
    let msg = sprintf "TimerTriggerFSharp1 executed at: %s" DateTime.Now.ToString()
    log.Verbose(msg);
    outputEventHubMessage <- msg;
```

### <a name="output---javascript-example"></a>Ausgabe: JavaScript-Beispiel

Das folgende Beispiel zeigt eine Event Hub-Triggerbindung in einer Datei vom Typ *function.json* sowie eine [JavaScript-Funktion](functions-reference-node.md), die die Bindung verwendet. Die Funktion schreibt eine Nachricht in einen Event Hub.

Bindungsdaten in der Datei *function.json*:

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```

Der folgende JaveScript-Code sendet eine einzelne Nachricht:

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    context.log('Event Hub message created at: ', timeStamp);   
    context.bindings.outputEventHubMessage = "Event Hub message created at: " + timeStamp;
    context.done();
};
```

Der folgende JaveScript-Code sendet mehrere Nachrichten:

```javascript
module.exports = function(context) {
    var timeStamp = new Date().toISOString();
    var message = 'Event Hub message created at: ' + timeStamp;

    context.bindings.outputEventHubMessage = [];

    context.bindings.outputEventHubMessage.push("1 " + message);
    context.bindings.outputEventHubMessage.push("2 " + message);
    context.done();
};
```

## <a name="output---attributes"></a>Ausgabe: Attribute

Verwenden Sie in [C#-Klassenbibliotheken](functions-dotnet-class-library.md) das im NuGet-Paket [Microsoft.Azure.WebJobs.ServiceBus](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus) definierte Attribut [EventHubAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs).

Der Konstruktor des Attributs nimmt den Namen des Event Hubs und den Namen einer App-Einstellung an, die die Verbindungszeichenfolge enthält. Weitere Informationen zu diesen Einstellungen finden Sie unter [Ausgabe: Konfiguration](#output---configuration). Dieses Beispiel zeigt ein Attribut `EventHub`:

```csharp
[FunctionName("EventHubOutput")]
[return: EventHub("outputEventHubMessage", Connection = "EventHubConnectionAppSetting")]
public static string Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, TraceWriter log)
{
    ...
}
```

Ein vollständiges Beispiel finden Sie unter [Ausgabe: C#-Beispiel](#output---c-example).

## <a name="output---configuration"></a>Ausgabe: Konfiguration

Die folgende Tabelle gibt Aufschluss über die Bindungskonfigurationseigenschaften, die Sie in der Datei *function.json* und im Attribut `EventHub` festlegen:

|Eigenschaft von „function.json“ | Attributeigenschaft |BESCHREIBUNG|
|---------|---------|----------------------|
|**type** | – | Muss auf „eventHub“ festgelegt werden. |
|**direction** | – | Muss auf „out“ festgelegt werden. Dieser Parameter wird automatisch festgelegt, wenn Sie die Bindung im Azure Portal erstellen. |
|**name** | – | Der Variablenname, der in Funktionscode verwendet wird, der das Ereignis darstellt. | 
|**path** |**EventHubName** | Der Name des Event Hubs. | 
|**Verbindung** |**Connection** | Der Name einer App-Einstellung, die die Zeichenfolge für die Verbindung mit dem Namespace des Event Hubs enthält. Kopieren Sie diese Verbindungszeichenfolge, indem Sie für den *Namespace* (nicht für den eigentlichen Event Hub) auf die Schaltfläche **Verbindungsinformationen** klicken. Diese Verbindungszeichenfolge muss über Sendeberechtigungen zum Senden der Nachricht an den Ereignisstrom verfügen.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Ausgabe: Verwendung

Verwenden Sie in C# und C#-Skripts einen Methodenparameter wie `out string paramName`, um Nachrichten zu senden. In C#-Skripts ist `paramName` der Wert, der in der Eigenschaft `name` von *function.json* angegeben ist. Um mehrere Nachrichten zu schreiben, können Sie `ICollector<string>` oder `IAsyncCollector<string>` anstelle von `out string` verwenden.

Greifen Sie in JavaScript mit `context.bindings.<name>` auf das Ausgabeereignis zu. `<name>` ist der Wert, der in der Eigenschaft `name` von *function.json* angegeben wird.

## <a name="exceptions-and-return-codes"></a>Ausnahmen und Rückgabecodes

| Bindung | Verweis |
|---|---|
| Event Hub | [Betriebsleitfaden](https://docs.microsoft.com/rest/api/eventhub/publisher-policy-operations) |

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Konzepte für Azure Functions-Trigger und -Bindungen](functions-triggers-bindings.md)
