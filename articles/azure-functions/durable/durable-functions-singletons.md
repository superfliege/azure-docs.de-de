---
title: Singletons für Durable Functions – Azure
description: Es wird beschrieben, wie Sie Singletons in der Erweiterung „Durable Functons“ für Azure Functions verwenden.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: 58e5b06d613ee3e3311b58af64abd2411c637449
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/30/2018
ms.locfileid: "52637445"
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
    ILogger log)
{
    // Check if an instance with the specified ID already exists.
    var existingInstance = await starter.GetStatusAsync(instanceId);
    if (existingInstance == null)
    {
        // An instance with the specified ID doesn't exist, create one.
        dynamic eventData = await req.Content.ReadAsAsync<object>();
        await starter.StartNewAsync(functionName, instanceId, eventData);
        log.LogInformation($"Started orchestration with ID = '{instanceId}'.");
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

> [!NOTE]
> In diesem Beispiel gibt es eine potenzielle Racebedingung. Wenn zwei Instanzen von **HttpStartSingle** gleichzeitig ausgeführt werden, sind als Ergebnis zwei verschiedene erstellte Instanzen des Singleton möglich, wobei eine die andere überschreibt. Je nach Ihren Anforderungen kann dies unerwünschte Nebenwirkungen haben. Aus diesem Grund muss sichergestellt werden, dass diese Triggerfunktion nicht von zwei Anforderungen gleichzeitig ausgeführt werden kann.

Die Implementierungsdetails der Orchestratorfunktion sind hier nicht so wichtig. Es kann eine reguläre Orchestratorfunktion sein, die gestartet und abgeschlossen wird, oder es kann eine Orchestratorfunktion mit unendlicher Ausführung sein (also eine [endlose Orchestrierung](durable-functions-eternal-orchestrations.md)). Der wichtigste Punkt hierbei ist, dass nur jeweils eine Instanz ausgeführt wird.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Informationen zum Aufrufen von untergeordneten Orchestrierungen](durable-functions-sub-orchestrations.md)
