---
title: "Bindungen für Durable Functions – Azure"
description: "Es wird beschrieben, wie Sie Trigger und Bindungen für die Erweiterung „Durable Functions“ für Azure Functions verwenden."
services: functions
author: cgillum
manager: cfowler
editor: 
tags: 
keywords: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: 01016294c3ef6fd904a7582e4f9c16ef19330a20
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2017
---
# <a name="bindings-for-durable-functions-azure-functions"></a>Bindungen für Durable Functions (Azure Functions)

Mit der Erweiterung [Durable Functions](durable-functions-overview.md) (Langlebige Funktionen) werden zwei neue Triggerbindungen eingeführt, mit denen die Ausführung von Orchestrator- und Aktivitätsfunktionen gesteuert wird. Außerdem wird eine Ausgabebindung eingeführt, die als Client für die Laufzeit von Durable Functions dient.

## <a name="orchestration-triggers"></a>Orchestrierungstrigger

Mit einem Orchestrierungstrigger können Sie langlebige Orchestratorfunktionen erstellen. Dieser Trigger unterstützt das Starten von neuen Orchestratorfunktionsinstanzen und das Fortsetzen von vorhandenen Orchestratorfunktionsinstanzen, die auf eine Aufgabe „warten“.

Wenn Sie die Visual Studio-Tools für Azure Functions verwenden, wird der Orchestrierungstrigger mit dem .NET-Attribut [OrchestrationTriggerAttribute](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.OrchestrationTriggerAttribute.html) konfiguriert.

Wenn Sie Orchestratorfunktionen in Skriptsprachen schreiben (z.B. im Azure-Portal), wird der Orchestrierungstrigger mit dem folgenden JSON-Objekt im `bindings`-Array der Datei *function.json* definiert:

```json
{
    "name": "<Name of input parameter in function signature>",
    "orchestration": "<Optional - name of the orchestration>",
    "version": "<Optional - version label of this orchestrator function>",
    "type": "orchestrationTrigger",
    "direction": "in"
}
```

* `orchestration` ist der Name der Orchestrierung. Dies ist der Wert, den Clients verwenden müssen, wenn sie neue Instanzen dieser Orchestratorfunktion starten möchten. Diese Eigenschaft ist optional. Wenn sie nicht angegeben ist, wird der Name der Funktion verwendet.
* `version` ist eine Versionsbezeichnung der Orchestrierung. Clients, die eine neue Instanz einer Orchestrierung starten, müssen die passende Versionsbezeichnung einbinden. Diese Eigenschaft ist optional. Wenn sie nicht angegeben ist, wird die leere Zeichenfolge verwendet. Weitere Informationen zur Versionsverwaltung finden Sie unter [Versionsverwaltung](durable-functions-versioning.md).

> [!NOTE]
> Das Festlegen von Werten für `orchestration`- oder `version`-Eigenschaften ist zu diesem Zeitpunkt nicht zu empfehlen.

Intern fragt diese Triggerbindung eine Reihe von Warteschlangen im Standardspeicherkonto für die Funktionen-App ab. Da es sich bei diesen Warteschlangen um interne Implementierungsdetails der Erweiterung handelt, werden sie in den Bindungseigenschaften nicht explizit konfiguriert.

### <a name="trigger-behavior"></a>Triggerverhalten

Hier sind einige Hinweise zum Orchestrierungstrigger angegeben:

* **Single-Threading**: Ein einzelner Verteilerthread wird für die gesamte Ausführung von Orchestratorfunktionen auf einer einzelnen Hostinstanz verwendet. Daher sollten Sie unbedingt sicherstellen, dass der Orchestratorfunktionscode effizient ist und keine E/A-Vorgänge durchführt. Außerdem sollten Sie sicherstellen, dass dieser Thread nur dann asynchrone Arbeitsschritte ausführt, wenn auf Aufgabentypen gewartet wird, die sich auf Durable Functions beziehen.
* **Behandlung von nicht verarbeitbaren Nachrichten**: Orchestrierungstrigger weisen keine Unterstützung für nicht verarbeitbare Nachrichten auf.
* **Sichtbarkeit von Nachrichten**: Nachrichten des Orchestrierungstriggers werden aus der Warteschlange entfernt und bleiben für eine konfigurierbare Dauer unsichtbar. Die Sichtbarkeit dieser Nachrichten wird automatisch erneuert, solange die Funktionen-App ausgeführt wird und fehlerfrei ist.
* **Rückgabewerte**: Rückgabewerte werden in JSON-Code serialisiert und in der Tabelle mit dem Orchestrierungsverlauf im Azure-Tabellenspeicher beibehalten. Diese Rückgabewerte können von der Orchestrierungsclientbindung abgefragt werden, die weiter unten beschrieben wird.

> [!WARNING]
> Für Orchestratorfunktionen sollten niemals andere Eingabe- oder Ausgabebindungen als die Orchestrierungstriggerbindung verwendet werden. Wenn dies nicht beachtet wird, können sich unter Umständen Probleme mit der Erweiterung für langlebige Aufgaben ergeben, da sich diese Bindungen ggf. nicht an die Regeln für Single-Threading und E/A-Abläufe halten.

### <a name="trigger-usage"></a>Triggerverwendung

Die Orchestrierungstriggerbindung unterstützt sowohl Ein- als auch Ausgaben. Hier sind einige Dinge aufgeführt, die Sie in Bezug auf die Eingabe- und Ausgabeverarbeitung wissen sollten:

* **Eingaben**: Orchestrierungsfunktionen unterstützen nur [DurableOrchestrationContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html) als Parametertyp. Deserialisierungseingaben direkt in die Funktionssignatur werden nicht unterstützt. Im Code muss die [GetInput\<T>](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_GetInput__1)-Methode zum Abrufen von Eingaben der Orchestratorfunktion verwendet werden. Bei diesen Eingaben muss es sich um Typen handeln, für die eine JSON-Serialisierung möglich ist.
* **Ausgaben**: Orchestrierungstrigger unterstützen sowohl Ausgabewerte als auch Eingaben. Der Rückgabewert der Funktion wird verwendet, um den Ausgabewert zuzuweisen, und er muss per JSON serialisierbar sein. Wenn eine Funktion `Task` oder `void` zurückgibt, wird als Ausgabe der Wert `null` gespeichert.

> [!NOTE]
> Orchestrierungstrigger werden derzeit nur in C# unterstützt.

### <a name="trigger-sample"></a>Triggerbeispiel

Hier ist ein Beispiel für eine sehr einfache C#-Orchestratorfunktion vom Typ „Hello World“ angegeben:

```csharp
[FunctionName("HelloWorld")]
public static string Run([OrchestrationTrigger] DurableOrchestrationContext context)
{
    string name = context.GetInput<string>();
    return $"Hello {name}!";
}
```

Die meisten Orchestratorfunktionen rufen andere Funktionen auf. In diesem „Hello World“-Beispiel wird veranschaulicht, wie eine Funktion aufgerufen wird:

```csharp
[FunctionName("HelloWorld")]
public static async Task<string> Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    string name = await context.GetInput<string>();
    string result = await context.CallActivityAsync<string>("SayHello", name);
    return result;
}
```

## <a name="activity-triggers"></a>Aktivitätstrigger

Mit dem Aktivitätstrigger können Sie Funktionen erstellen, die von Orchestratorfunktionen aufgerufen werden.

Wenn Sie Visual Studio verwenden, wird der Aktivitätstrigger mit dem .NET-Attribut [ActvityTriggerAttribute](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.ActivityTriggerAttribute.html) konfiguriert. 

Wenn Sie das Azure-Portal für die Entwicklung verwenden, wird der Aktivitätstrigger mit dem folgenden JSON-Objekt im `bindings`-Array der Datei *function.json* definiert:

```json
{
    "name": "<Name of input parameter in function signature>",
    "activity": "<Optional - name of the activity>",
    "version": "<Optional - version label of this activity function>",
    "type": "activityTrigger",
    "direction": "in"
}
```

* `activity` ist der Name der Aktivität. Dies ist der Wert, der von Orchestratorfunktionen verwendet wird, um diese Aktivitätsfunktion aufzurufen. Diese Eigenschaft ist optional. Wenn sie nicht angegeben ist, wird der Name der Funktion verwendet.
* `version` ist eine Versionsbezeichnung der Aktivität. Orchestratorfunktionen, die eine Aktivität aufrufen, müssen die passende Versionsbezeichnung enthalten. Diese Eigenschaft ist optional. Wenn sie nicht angegeben ist, wird die leere Zeichenfolge verwendet. Weitere Informationen finden Sie unter [Versionsverwaltung](durable-functions-versioning.md).

> [!NOTE]
> Das Festlegen von Werten für `activity`- oder `version`-Eigenschaften ist zu diesem Zeitpunkt nicht zu empfehlen.

Intern fragt diese Triggerbindung eine Warteschlange im Standardspeicherkonto für die Funktionen-App ab. Da es sich bei dieser Warteschlange um ein internes Implementierungsdetail der Erweiterung handelt, wird sie in den Bindungseigenschaften nicht explizit konfiguriert.

### <a name="trigger-behavior"></a>Triggerverhalten

Hier sind einige Hinweise zum Aktivitätstrigger aufgeführt:

* **Threading**: Im Gegensatz zum Orchestrierungstrigger gelten für Aktivitätstrigger keine Einschränkungen in Bezug auf das Threading oder den E/A-Ablauf. Sie können wie reguläre Funktionen behandelt werden.
* **Behandlung von nicht verarbeitbaren Nachrichten**: Aktivitätstrigger weisen keine Unterstützung für nicht verarbeitbare Nachrichten auf.
* **Sichtbarkeit von Nachrichten**: Nachrichten des Aktivitätstriggers werden aus der Warteschlange entfernt und bleiben für eine konfigurierbare Dauer unsichtbar. Die Sichtbarkeit dieser Nachrichten wird automatisch erneuert, solange die Funktionen-App ausgeführt wird und fehlerfrei ist.
* **Rückgabewerte**: Rückgabewerte werden in JSON-Code serialisiert und in der Tabelle mit dem Orchestrierungsverlauf im Azure-Tabellenspeicher beibehalten.

> [!WARNING]
> Das Speicher-Back-End für Aktivitätsfunktionen ist ein Implementierungsdetail, und Benutzercode sollte mit diesen Speicherentitäten nicht direkt interagieren.

### <a name="trigger-usage"></a>Triggerverwendung

Die Aktivitätstriggerbindung unterstützt sowohl Ein- als auch Ausgaben – genau wie der Orchestrierungstrigger. Hier sind einige Dinge aufgeführt, die Sie in Bezug auf die Eingabe- und Ausgabeverarbeitung wissen sollten:

* **Eingaben**: Für Aktivitätsfunktionen wird nativ [DurableActivityContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContext.html) als Parametertyp verwendet. Alternativ dazu kann eine Aktivitätsfunktion mit jedem Parametertyp deklariert werden, der per JSON serialisierbar ist. Bei Verwendung von `DurableActivityContext` können Sie [GetInput\<T>](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContext.html#Microsoft_Azure_WebJobs_DurableActivityContext_GetInput__1) aufrufen, um die Eingabe der Aktivitätsfunktion abzurufen und zu deserialisieren.
* **Ausgaben**: Aktivitätstrigger unterstützen sowohl Ausgabewerte als auch Eingaben. Der Rückgabewert der Funktion wird verwendet, um den Ausgabewert zuzuweisen, und er muss per JSON serialisierbar sein. Wenn eine Funktion `Task` oder `void` zurückgibt, wird als Ausgabe der Wert `null` gespeichert.
* **Metadaten**: Aktivitätsfunktionen können an einen `string instanceId`-Parameter gebunden werden, um die Instanz-ID der übergeordneten Orchestrierung zu erhalten.

> [!NOTE]
> Aktivitätstrigger werden derzeit in Node.js-Funktionen nicht unterstützt.

### <a name="trigger-sample"></a>Triggerbeispiel

Hier ist ein Beispiel dafür angegeben, wie eine einfache C#-Aktivitätsfunktion vom Typ „Hello World“ aussehen könnte:

```csharp
[FunctionName("SayHello")]
public static string SayHello([ActivityTrigger] DurableActivityContext helloContext)
{
    string name = helloContext.GetInput<string>();
    return $"Hello {name}!";
}
```

Der Standardparametertyp für die `ActivityTriggerAttribute`-Bindung lautet `DurableActivityContext`. Aktivitätstrigger unterstützen aber auch die direkte Bindung an JSON-serialisierbare Typen (z.B. primitive Typen), sodass dieselbe Funktion wie folgt vereinfacht werden kann:

```csharp
[FunctionName("SayHello")]
public static string SayHello([ActivityTrigger] string name)
{
    return $"Hello {name}!";
}
```

## <a name="orchestration-client"></a>Orchestrierungsclient

Mit der Bindung des Orchestrierungsclients können Sie Funktionen schreiben, die mit Orchestratorfunktionen interagieren. Beispielsweise können Sie für Orchestrierungsinstanzen Folgendes durchführen:
* Starten
* Abfragen des Status
* Beenden
* Senden von Ereignissen an die Instanzen während der Ausführung

Bei Verwendung von Visual Studio können Sie die Bindung an den Orchestrierungsclient durchführen, indem Sie das .NET-Attribut [OrchestrationClientAttribute](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.OrchestrationClientAttribute.html) verwenden.

Wenn Sie Skriptsprachen (z.B. *CSX*-Dateien) für die Entwicklung nutzen, wird der Orchestrierungstrigger mit dem folgenden JSON-Objekt im `bindings`-Array der Datei „function.json“ definiert:

```json
{
    "name": "<Name of input parameter in function signature>",
    "taskHub": "<Optional - name of the task hub>",
    "connectionName": "<Optional - name of the connection string app setting>",
    "type": "orchestrationClient",
    "direction": "out"
}
```

* `taskHub`: Wird in Szenarien verwendet, in denen mehrere Funktionen-Apps dasselbe Speicherkonto nutzen, aber voneinander isoliert werden müssen. Wenn Sie hier nichts angeben, wird der Standardwert aus `host.json` verwendet. Dieser Wert muss mit dem Wert übereinstimmen, der von den Zielorchestratorfunktionen verwendet wird.
* `connectionName`: Der Name einer App-Einstellung, die eine Speicherverbindungszeichenfolge enthält. Das Speicherkonto, für das diese Verbindungszeichenfolge steht, muss dem Speicherkonto entsprechen, das von den Zielorchestratorfunktionen verwendet wird. Wenn nichts angegeben ist, wird die Standardverbindungszeichenfolge für die Funktionen-App genutzt.

> [!NOTE]
> In den meisten Fällen ist es empfehlenswert, diese Eigenschaften wegzulassen und das Standardverhalten zu nutzen.

### <a name="client-usage"></a>Clientnutzung

In C#-Funktionen richten Sie normalerweise eine Bindung mit `DurableOrchestrationClient` ein, sodass Sie Vollzugriff auf alle Client-APIs erhalten, die von Durable Functions unterstützt werden. Zu den APIs für das Clientobjekt gehören:

* [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_)
* [GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_GetStatusAsync_)
* [TerminateAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_TerminateAsync_)
* [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_)

Alternativ dazu können Sie eine Bindung mit `IAsyncCollector<T>` einrichten, wobei `T` für [StartOrchestrationArgs](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.StartOrchestrationArgs.html) oder `JObject` steht.

Ausführliche Informationen zu diesen Vorgängen finden Sie in der API-Dokumentation zu [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html).

### <a name="client-sample-visual-studio-development"></a>Clientbeispiel (Visual Studio-Entwicklung)

Hier ist ein Beispiel für eine per Warteschlange ausgelöste Funktion angegeben, mit der eine „HelloWorld“-Orchestrierung gestartet wird.

```csharp
[FunctionName("QueueStart")]
public static Task Run(
    [QueueTrigger("durable-function-trigger")] string input,
    [OrchestrationClient] DurableOrchestrationClient starter)
{
    // Orchestration input comes from the queue message content.
    return starter.StartNewAsync("HelloWorld", input);
}
```

### <a name="client-sample-not-visual-studio"></a>Clientbeispiel (nicht Visual Studio)

Wenn Sie für die Entwicklung nicht Visual Studio nutzen, können Sie die folgende Datei „function.json“ erstellen. In diesem Beispiel wird veranschaulicht, wie Sie eine per Warteschlange ausgelöste Funktion konfigurieren, für die die langlebige Orchestrierungsclientbindung verwendet wird:

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "queueTrigger",
      "queueName": "durable-function-trigger",
      "direction": "in"
    },
    {
      "name": "starter",
      "type": "orchestrationClient",
      "direction": "out"
    }
  ],
  "disabled": false
} 
```

Im Folgenden sind sprachspezifische Beispiele angegeben, in denen neue Instanzen von Orchestratorfunktionen gestartet werden.

#### <a name="c-sample"></a>C#-Beispiel

Im folgenden Beispiel wird veranschaulicht, wie Sie die langlebige Orchestrierungsclientbindung nutzen, um eine neue Funktionsinstanz über eine C#-Skriptfunktion zu starten:

```csharp
#r "Microsoft.Azure.WebJobs.Extensions.DurableTask"

public static Task<string> Run(string input, DurableOrchestrationClient starter)
{
    return starter.StartNewAsync("HelloWorld", input);
}
```

#### <a name="nodejs-sample"></a>Node.js-Beispiel

Im folgenden Beispiel wird veranschaulicht, wie Sie die langlebige Orchestrierungsclientbindung nutzen, um eine neue Funktionsinstanz über eine Node.js-Skriptfunktion zu starten:

```js
module.exports = function (context, input) {
    var id = generateSomeUniqueId();
    context.bindings.starter = [{
        FunctionName: "HelloWorld",
        Input: input,
        InstanceId: id
    }];

    context.done(null, id);
};
```

Weitere Informationen zum Starten von Instanzen finden Sie unter [Instanzverwaltung](durable-functions-instance-management.md).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Informationen zum Setzen von Prüfpunkten und Wiedergabeverhalten](durable-functions-checkpointing-and-replay.md)

