---
title: "Verwalten von Instanzen in Durable Functions – Azure"
description: "Es wird beschrieben, wie Instanzen in der Erweiterung Durable Functions für Azure Functions verwaltet werden."
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
ms.openlocfilehash: 8c9085042496fef4c499f57e874e0b8ba059fd65
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="manage-instances-in-durable-functions-azure-functions"></a>Verwalten von Instanzen in Durable Functions (Azure Functions)

In [Durable Functions](durable-functions-overview.md) können Orchestrierungsinstanzen gestartet, beendet und abgefragt sowie Benachrichtigungsereignisse gesendet werden. Die gesamte Instanzenverwaltung erfolgt mithilfe der [Orchestrierungsclientbindung](durable-functions-bindings.md). In diesem Artikel wird jeder Instanzenverwaltungsvorgang im Detail erläutert.

## <a name="starting-instances"></a>Starten von Instanzen

Die [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_)-Methode im [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) startet eine neue Instanz einer Orchestratorfunktion. Instanzen dieser Klasse können mithilfe der `orchestrationClient`-Bindung abgerufen werden. Intern stellt diese Methode eine Nachricht in die Steuerwarteschlange, die dann den Start einer Funktion mit dem angegebenen Namen auslöst, der die `orchestrationTrigger`-Bindung verwendet.

Die Parameter lauten wie folgt:

* **Name**: der Name der zu planenden Orchestratorfunktion.
* **Input**: alle mit JSON serialisierbaren Daten, die der Orchestratorfunktion als Eingabe übergeben werden sollen.
* **InstanceId**: (Optional) die eindeutige ID der Instanz. Wenn nicht angegeben, wird eine zufällige Instanz-ID generiert.

Hier ist ein einfaches Beispiel für C#:

```csharp
[FunctionName("HelloWorldManualStart")]
public static Task Run(
    [ManualTrigger] string input,
    [OrchestrationClient] DurableOrchestrationClient starter,
    TraceWriter log)
{
    string instanceId = starter.StartNewAsync("HelloWorld", input);
    log.Info($"Started orchestration with ID = '{instanceId}'.");
}
```

Für Nicht-.NET-Sprachen kann die Funktionsausgabebindung auch zum Starten neuer Instanzen verwendet werden. In diesem Fall können alle mit JSON serialisierbaren Daten verwendet werden, die die oben genannten drei Parameter als Felder enthalten. Betrachten Sie beispielsweise die folgende Node.js-Funktion:

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
> Im Allgemeinen sollten Sie einen zufälligen Bezeichner für die Instanz-ID verwenden. Dies gewährleistet eine gleichmäßige Lastenverteilung, wenn Orchestratorfunktionen mehrere virtuelle Computer übergreifend skaliert werden. Wenn die ID aus einer externen Quelle stammen muss oder bei der Implementierung des [Singleton-Orchestrator](durable-functions-counter.md)-Musters sollten nicht zufällige Instanz-IDs verwendet werden.

## <a name="querying-instances"></a>Abfragen von Instanzen

Die [GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_GetStatusAsync_)-Methode für die [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html)-Klasse fragt den Status einer Orchestrierungsinstanz ab. Sie übernimmt eine `instanceId` als Parameter und gibt ein Objekt mit folgenden Eigenschaften zurück:

* **Name**: der Name der Orchestratorfunktion.
* **InstanceId**: die Instanz-ID der Orchestrierung (muss mit der Eingabe `instanceId` identisch sein).
* **CreatedTime**: der Zeitpunkt, zu dem die Ausführung der Orchestratorfunktion gestartet wurde.
* **LastUpdatedTime**: der Zeitpunkt des letzten Prüfpunkts der Orchestrierung.
* **Input**: die Eingabe der Funktion als JSON-Wert.
* **Output**: die Ausgabe der Funktion als JSON-Wert (wenn die Funktion abgeschlossen wurde). Wenn ein Fehler bei der Orchestratorfunktion auftritt, enthält diese Eigenschaft die Fehlerdetails. Wenn die Orchestratorfunktion beendet wurde, enthält diese Eigenschaft den angegebenen Grund für die Beendigung (sofern vorhanden).
* **RuntimeStatus**: Einer der folgenden Werte:
    * **Running**: Die Instanz wurde gestartet.
    * **Completed**: Die Instanz wurde normal abgeschlossen.
    * **ContinuedAsNew**: Die Instanz hat sich selbst mit einem neuen Verlauf neu gestartet. Dies ist ein vorübergehender Status.
    * **Failed**: Bei der Instanz ist ein Fehler aufgetreten.
    * **Terminated**: Die Instanz wurde plötzlich beendet.
    
Diese Methode gibt `null` zurück, wenn die Instanz nicht vorhanden ist, oder ihre Ausführung noch nicht gestartet wurde.

```csharp
[FunctionName("GetStatus")]
public static async Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    var status = await checker.GetStatusAsync(instanceId);
    // do something based on the current status.
}
```

> [!NOTE]
> Instanzabfragen werden zurzeit nur für C#-Funktionen unterstützt.

## <a name="terminating-instances"></a>Beenden von Instanzen

Eine ausgeführte Instanz kann mithilfe der [TerminateAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_TerminateAsync_)-Methode der [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html)-Klasse beendet werden. Die beiden Parameter sind eine `instanceId` und eine `reason`-Zeichenfolge, die in Protokolle und den Instanzenstatus geschrieben werden. Die Ausführung einer beendeten Instanz endet, sobald sie den nächsten `await`-Punkt erreicht, oder sofort, wenn sie sich bereits an einem `await`-Punkt befindet.

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
> Die Instanzbeendigung wird zurzeit nur für C#-Funktionen unterstützt.

## <a name="sending-events-to-instances"></a>Senden von Ereignissen an Instanzen

Ereignisbenachrichtigungen können an ausgeführte Instanzen gesendet werden, die die [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_)-Methode der [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html)-Klasse verwenden. Instanzen, die auf einen Aufruf zu [WaitForExternalEvent](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_WaitForExternalEvent_) warten, können diese Ereignisse verarbeiten. Die Eingaben sind:

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

> [!NOTE]
> Auslösen von Ereignissen wird derzeit nur für C#-Funktionen unterstützt.

> [!WARNING]
> Wenn keine Orchestrierungsinstanz mit der angegebenen *Instanz-ID* vorhanden ist, oder die Instanz nicht auf den angegebenen *Ereignisnamen* wartet, wird die Ereignisnachricht verworfen. Weitere Informationen zu diesem Verhalten finden Sie unter [Expected behavior when unexpected event is received?](https://github.com/Azure/azure-functions-durable-extension/issues/29) (Erwartetes Verhalten, wenn ein unerwartetes Ereignis empfangen wird?).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [HTTP APIs in Durable Functions (Azure Functions)](durable-functions-http-api.md) (HTTP-APIs in Durable Functions [Azure Functions])
