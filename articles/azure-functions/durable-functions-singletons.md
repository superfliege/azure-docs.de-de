---
title: "Singletons für Durable Functions – Azure"
description: "Es wird beschrieben, wie Sie Singletons in der Erweiterung „Durable Functons“ für Azure Functions verwenden."
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
ms.openlocfilehash: 8384d17405653a29207cdfa4f6143504d0db2022
ms.sourcegitcommit: 5d772f6c5fd066b38396a7eb179751132c22b681
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2017
---
# <a name="singleton-orchestrators-in-durable-functions-azure-functions"></a>Singleton-Orchestratoren in Durable Functions (Azure Functions)

Für Hintergrundaufträge oder Orchestrierungen im Akteur-Stil müssen Sie häufig sicherstellen, dass nur jeweils eine Instanz eines bestimmten Orchestrators ausgeführt wird. Dies ist in [Durable Functions](durable-functions-overview.md) möglich, indem einem Orchestrator bei der Erstellung eine spezifische Instanz-ID zugewiesen wird.

## <a name="singleton-example"></a>Singleton-Beispiel

Im folgenden C#-Beispiel wird eine HTTP-Triggerfunktion veranschaulicht, mit der eine Orchestrierung für einen Singleton-Hintergrundauftrag erstellt wird. Der Code stellt sicher, dass für eine angegebene Instanz-ID nur eine Instanz vorhanden ist.

```cs
[FunctionName("HttpStartSingle")]
public static async Task<HttpResponseMessage> RunSingle(
    [HttpTrigger(AuthorizationLevel.Function, methods: "post", Route = "orchestrators/{functionName}/{instanceId}")] HttpRequestMessage req,
    [OrchestrationClient] DurableOrchestrationClient starter,
    string functionName,
    string instanceId,
    TraceWriter log)
{
    // Check if an instance with the specified ID already exists.
    var existingInstance = await starter.GetStatusAsync(instanceId);
    if (existingInstance == null)
    {
        // An instance with the specified ID doesn't exist, create one.
        dynamic eventData = await req.Content.ReadAsAsync<object>();
        await starter.StartNewAsync(functionName, instanceId, eventData);
        log.Info($"Started orchestration with ID = '{instanceId}'.");
        return starter.CreateCheckStatusResponse(req, instanceId);
    }
    else
    {
        // An instance with the specified ID exists, don't create one.
        return req.CreateErrorResponse(
            HttpStatusCode.Conflict,
            $"An instance with ID '{instanceId}' already exists.");
    }
}
```

Standardmäßig handelt es sich bei Instanz-IDs um zufällig generierte GUIDs. Aber in diesem Fall wird die Instanz-ID in den Routendaten von der URL übergeben. Der Code ruft [GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_GetStatusAsync_) auf, um zu überprüfen, ob bereits eine Instanz mit der angegebenen ID ausgeführt wird. Ist dies nicht der Fall, wird eine Instanz mit dieser ID erstellt.

Die Implementierungsdetails der Orchestratorfunktion sind hier nicht so wichtig. Es kann eine reguläre Orchestratorfunktion sein, die gestartet und abgeschlossen wird, oder es kann eine Orchestratorfunktion mit unendlicher Ausführung sein (also eine [endlose Orchestrierung](durable-functions-eternal-orchestrations.md)). Der wichtigste Punkt hierbei ist, dass nur jeweils eine Instanz ausgeführt wird.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Informationen zum Aufrufen von untergeordneten Orchestrierungen](durable-functions-sub-orchestrations.md)

> [!div class="nextstepaction"]
> [Ausführen eines Singleton-Beispiels](durable-functions-counter.md)
