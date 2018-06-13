---
title: Verwalten von Instanzen in Durable Functions – Azure
description: Es wird beschrieben, wie Instanzen in der Erweiterung Durable Functions für Azure Functions verwaltet werden.
services: functions
author: cgillum
manager: cfowler
editor: ''
tags: ''
keywords: ''
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/19/2018
ms.author: azfuncdf
ms.openlocfilehash: 0e573b4973ea30b990043b54c5cdcf0805135a40
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
ms.locfileid: "33764654"
---
# <a name="manage-instances-in-durable-functions-azure-functions"></a>Verwalten von Instanzen in Durable Functions (Azure Functions)

In [Durable Functions](durable-functions-overview.md) können Orchestrierungsinstanzen gestartet, beendet und abgefragt sowie Benachrichtigungsereignisse gesendet werden. Die gesamte Instanzenverwaltung erfolgt mithilfe der [Orchestrierungsclientbindung](durable-functions-bindings.md). In diesem Artikel wird jeder Instanzenverwaltungsvorgang im Detail erläutert.

## <a name="starting-instances"></a>Starten von Instanzen

Die [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_)-Methode im [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) startet eine neue Instanz einer Orchestratorfunktion. Instanzen dieser Klasse können mithilfe der `orchestrationClient`-Bindung abgerufen werden. Intern stellt diese Methode eine Nachricht in die Steuerwarteschlange, die dann den Start einer Funktion mit dem angegebenen Namen auslöst, der die `orchestrationTrigger`-Bindung verwendet. 

Die Aufgabe ist abgeschlossen, wenn der Orchestrierungsprozess gestartet wird. Der Orchestrierungsprozess sollte innerhalb von 30 Sekunden gestartet werden. Wenn es länger dauert, wird eine `TimeoutException` ausgelöst. 

Die Parameter für [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_) lauten wie folgt:

* **Name**: der Name der zu planenden Orchestratorfunktion.
* **Input**: alle mit JSON serialisierbaren Daten, die der Orchestratorfunktion als Eingabe übergeben werden sollen.
* **InstanceId**: (Optional) die eindeutige ID der Instanz. Wenn nicht angegeben, wird eine zufällige Instanz-ID generiert.

Hier ist ein einfaches Beispiel für C#:

```csharp
[FunctionName("HelloWorldManualStart")]
public static async Task Run(
    [ManualTrigger] string input,
    [OrchestrationClient] DurableOrchestrationClient starter,
    TraceWriter log)
{
    string instanceId = await starter.StartNewAsync("HelloWorld", input);
    log.Info($"Started orchestration with ID = '{instanceId}'.");
}
```

Für Nicht-.NET-Sprachen kann die Funktionsausgabebindung auch zum Starten neuer Instanzen verwendet werden. In diesem Fall können alle mit JSON serialisierbaren Daten verwendet werden, die die oben genannten drei Parameter als Felder enthalten. Betrachten Sie beispielsweise die folgende JavaScript-Funktion:

```js
module.exports = function (context, input) {
    var id = generateSomeUniqueId();
    context.bindings.starter = [{
        FunctionName: "HelloWorld",
        Input: input,
        InstanceId: id
    }];

    context.done(null);
};
```

> [!NOTE]
> Sie sollten einen zufälligen Bezeichner für die Instanz-ID verwenden. Dies gewährleistet eine gleichmäßige Lastenverteilung, wenn Orchestratorfunktionen mehrere virtuelle Computer übergreifend skaliert werden. Wenn die ID aus einer externen Quelle stammen muss oder bei der Implementierung des [Singleton-Orchestrator](durable-functions-singletons.md)-Musters sollten nicht zufällige Instanz-IDs verwendet werden.

## <a name="querying-instances"></a>Abfragen von Instanzen

Die [GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_GetStatusAsync_)-Methode für die [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html)-Klasse fragt den Status einer Orchestrierungsinstanz ab. Hierfür werden `instanceId` (erforderlich), `showHistory` (optional) und `showHistoryOutput` (optional) als Parameter verwendet. Wenn `showHistory` auf `true` festgelegt ist, enthält die Antwort den Ausführungsverlauf. Wenn `showHistoryOutput` auch auf `true` festgelegt ist, enthält der Ausführungsverlauf Aktivitätsausgaben. Die Methode gibt ein Objekt mit den folgenden Eigenschaften zurück:

* **Name**: der Name der Orchestratorfunktion.
* **InstanceId**: die Instanz-ID der Orchestrierung (muss mit der Eingabe `instanceId` identisch sein).
* **CreatedTime**: der Zeitpunkt, zu dem die Ausführung der Orchestratorfunktion gestartet wurde.
* **LastUpdatedTime**: der Zeitpunkt des letzten Prüfpunkts der Orchestrierung.
* **Input**: die Eingabe der Funktion als JSON-Wert.
* **CustomStatus**: Benutzerdefinierter Orchestrierungsstatus in JSON-Format 
* **Output**: die Ausgabe der Funktion als JSON-Wert (wenn die Funktion abgeschlossen wurde). Wenn ein Fehler bei der Orchestratorfunktion auftritt, enthält diese Eigenschaft die Fehlerdetails. Wenn die Orchestratorfunktion beendet wurde, enthält diese Eigenschaft den angegebenen Grund für die Beendigung (sofern vorhanden).
* **RuntimeStatus**: Einer der folgenden Werte:
    * **Running**: Die Instanz wurde gestartet.
    * **Completed**: Die Instanz wurde normal abgeschlossen.
    * **ContinuedAsNew**: Die Instanz hat sich selbst mit einem neuen Verlauf neu gestartet. Dies ist ein vorübergehender Status.
    * **Failed**: Bei der Instanz ist ein Fehler aufgetreten.
    * **Terminated**: Die Instanz wurde plötzlich beendet.
* **History**: Der Ausführungsverlauf der Orchestrierung. Dieses Feld wird nur gefüllt, wenn `showHistory` auf `true` festgelegt ist.
    
Diese Methode gibt `null` zurück, wenn die Instanz nicht vorhanden ist, oder ihre Ausführung noch nicht gestartet wurde.

```csharp
[FunctionName("GetStatus")]
public static async Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    var status = await client.GetStatusAsync(instanceId);
    // do something based on the current status.
}
```

## <a name="terminating-instances"></a>Beenden von Instanzen

Eine aktive Orchestrierungsinstanz kann mithilfe der [TerminateAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_TerminateAsync_)-Methode der [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html)-Klasse beendet werden. Die beiden Parameter sind eine `instanceId` und eine `reason`-Zeichenfolge, die in Protokolle und den Instanzenstatus geschrieben werden. Die Ausführung einer beendeten Instanz endet, sobald sie den nächsten `await`-Punkt erreicht, oder sofort, wenn sie sich bereits an einem `await`-Punkt befindet. 

```csharp
[FunctionName("TerminateInstance")]
public static Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    string reason = "It was time to be done.";
    return client.TerminateAsync(instanceId, reason);
}
```

> [!NOTE]
> Die Instanzbeendigung wird momentan nicht weitergegeben. Aktivitätsfunktionen und untergeordnete Orchestrierungen werden bis zum Ende ausgeführt, auch wenn die Orchestrierungsinstanz, durch die sie aufgerufen wurden, beendet wurde.

## <a name="sending-events-to-instances"></a>Senden von Ereignissen an Instanzen

Ereignisbenachrichtigungen können an ausgeführte Instanzen gesendet werden, die die [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_)-Methode der [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html)-Klasse verwenden. Instanzen, die auf einen Aufruf zu [WaitForExternalEvent](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_WaitForExternalEvent_) warten, können diese Ereignisse verarbeiten. 

Die Parameter für [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) lauten wie folgt:

* **InstanceId**: die eindeutige ID der Instanz.
* **EventName**: der Name des zu sendenden Ereignisses.
* **EventData**: eine mit JSON serialisierbare Nutzlast zum Senden an die Instanz.

```csharp
#r "Microsoft.Azure.WebJobs.Extensions.DurableTask"

[FunctionName("RaiseEvent")]
public static Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    int[] eventData = new int[] { 1, 2, 3 };
    return client.RaiseEventAsync(instanceId, "MyEvent", eventData);
}
```

> [!WARNING]
> Wenn keine Orchestrierungsinstanz mit der angegebenen *Instanz-ID* vorhanden ist, oder die Instanz nicht auf den angegebenen *Ereignisnamen* wartet, wird die Ereignisnachricht verworfen. Weitere Informationen zu diesem Verhalten finden Sie unter [Expected behavior when unexpected event is received?](https://github.com/Azure/azure-functions-durable-extension/issues/29) (Erwartetes Verhalten, wenn ein unerwartetes Ereignis empfangen wird?).

## <a name="wait-for-orchestration-completion"></a>Warten auf den Abschluss der Orchestrierung

Die [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html)-Klasse macht eine [WaitForCompletionOrCreateCheckStatusResponseAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_WaitForCompletionOrCreateCheckStatusResponseAsync_)-API verfügbar, die verwendet werden kann, um synchron die tatsächliche Ausgabe einer Orchestrierungsinstanz zu erhalten. Für die Methode wird ein Standardwert von 10 Sekunden für `timeout` und von 1 Sekunde für `retryInterval` verwendet, wenn nichts anderes festgelegt wird.  

Hier ist ein Beispiel für eine HTTP-Triggerfunktion angegeben, mit dem die Nutzung dieser API veranschaulicht wird:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HttpSyncStart.cs)]

Die Funktion kann mit der folgenden Zeile aufgerufen werden, indem eine Zeitlimit von 2 Sekunden und ein Wiederholungsintervall von 0,5 Sekunden verwendet wird:

```bash
    http POST http://localhost:7071/orchestrators/E1_HelloSequence/wait?timeout=2&retryInterval=0.5
```

Je nach Zeitraum, der zum Abrufen der Antwort aus der Orchestrierungsinstanz erforderlich ist, gibt es zwei Fälle:

1. Die Orchestrierungsinstanzen werden innerhalb des definierten Zeitraums (hier: 2 Sekunden) abgeschlossen, und die Antwort ist die tatsächliche Orchestrierungsinstanzausgabe, die synchron bereitgestellt wird:

    ```http
        HTTP/1.1 200 OK
        Content-Type: application/json; charset=utf-8
        Date: Thu, 14 Dec 2017 06:14:29 GMT
        Server: Microsoft-HTTPAPI/2.0
        Transfer-Encoding: chunked

        [
            "Hello Tokyo!",
            "Hello Seattle!",
            "Hello London!"
        ]
    ```

2. Die Orchestrierungsinstanzen können nicht innerhalb des definierten Zeitraums (hier: 2 Sekunden) abgeschlossen werden, und die Antwort ist die Standardantwort, die unter **Ermittlung der HTTP-API-URL** beschrieben ist:

    ```http
        HTTP/1.1 202 Accepted
        Content-Type: application/json; charset=utf-8
        Date: Thu, 14 Dec 2017 06:13:51 GMT
        Location: http://localhost:7071/admin/extensions/DurableTaskExtension/instances/d3b72dddefce4e758d92f4d411567177?taskHub={taskHub}&connection={connection}&code={systemKey}
        Retry-After: 10
        Server: Microsoft-HTTPAPI/2.0
        Transfer-Encoding: chunked

        {
            "id": "d3b72dddefce4e758d92f4d411567177",
            "sendEventPostUri": "http://localhost:7071/admin/extensions/DurableTaskExtension/instances/d3b72dddefce4e758d92f4d411567177/raiseEvent/{eventName}?taskHub={taskHub}&connection={connection}&code={systemKey}",
            "statusQueryGetUri": "http://localhost:7071/admin/extensions/DurableTaskExtension/instances/d3b72dddefce4e758d92f4d411567177?taskHub={taskHub}&connection={connection}&code={systemKey}",
            "terminatePostUri": "http://localhost:7071/admin/extensions/DurableTaskExtension/instances/d3b72dddefce4e758d92f4d411567177/terminate?reason={text}&taskHub={taskHub}&connection={connection}&code={systemKey}"
        }
    ```

> [!NOTE]
> Das Format der Webhook-URLs kann in Abhängigkeit davon variieren, welche Version des Azure Functions-Hosts Sie ausführen. Das obige Beispiel gilt für den Azure Functions 2.0-Host.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [HTTP APIs in Durable Functions (Azure Functions)](durable-functions-http-api.md) (HTTP-APIs in Durable Functions [Azure Functions])
