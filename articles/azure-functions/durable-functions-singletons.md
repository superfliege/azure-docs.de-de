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
ms.openlocfilehash: e82cc53d53a6d0296aaab2c3a76ad4e2f6c12c54
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="singleton-orchestrators-in-durable-functions-azure-functions"></a>Singleton-Orchestratoren in Durable Functions (Azure Functions)

Für Hintergrundaufträge oder Orchestrierungen im Akteur-Stil müssen Sie häufig sicherstellen, dass nur jeweils eine Instanz eines bestimmten Orchestrators ausgeführt wird. Dies ist in [Durable Functions](durable-functions-overview.md) möglich, indem einem Orchestrator bei der Erstellung eine spezifische Instanz-ID zugewiesen wird.

## <a name="singleton-example"></a>Singleton-Beispiel

Im folgenden C#-Beispiel wird eine HTTP-Triggerfunktion veranschaulicht, mit der eine Orchestrierung für einen Singleton-Hintergrundauftrag erstellt wird. Es wird eine bekannte Instanz-ID verwendet, um sicherzustellen, dass nur eine Instanz vorhanden ist.

```cs
[FunctionName("EnsureSingletonTrigger")]
public static async Task<HttpResponseMessage> Ensure(
    [HttpTrigger(AuthorizationLevel.Function, methods: "post")] HttpRequestMessage req,
    [OrchestrationClient] DurableOrchestrationClient starter,
    TraceWriter log)
{
    // Ensure only one instance is ever running at a time
    const string OrchestratorName = "MySingletonOrchestrator";
    const string InstanceId = "MySingletonInstanceId";

    var existingInstance = await starter.GetStatusAsync(InstanceId);
    if (existingInstance == null)
    {
        log.Info($"Creating singleton instance with ID = {InstanceId}...");
        await starter.StartNewAsync(OrchestratorName, InstanceId, input: null);
    }

    return starter.CreateCheckStatusResponse(req, InstanceId);
}
```

Standardmäßig handelt es sich bei Instanz-IDs um zufällig generierte GUIDs. Beachten Sie in diesem Fall aber, dass für die Triggerfunktion eine vordefinierte `InstanceId`-Variable mit dem Wert `MySingletonInstanceId` verwendet wird, um der Orchestratorfunktion vorab eine Instanz-ID zuzuweisen. So kann der Trigger überprüfen und ermitteln, ob die bekannte Instanz ausgeführt wird, indem [GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_GetStatusAsync_) aufgerufen wird.

Die Implementierungsdetails der Orchestratorfunktion sind hier nicht so wichtig. Es kann eine reguläre Orchestratorfunktion sein, die gestartet und abgeschlossen wird, oder es kann eine Orchestratorfunktion mit unendlicher Ausführung sein (also eine [endlose Orchestrierung](durable-functions-eternal-orchestrations.md)). Der wichtigste Punkt hierbei ist, dass nur jeweils eine Instanz ausgeführt wird.

> [!NOTE]
> Wenn die Singleton-Orchestrierungsinstanz beendet wird, fehlschlägt oder abgeschlossen wird, kann sie nicht mit derselben ID wiederhergestellt werden. In diesen Fällen sollten Sie darauf vorbereitet sein, die Wiederherstellung mit einer neuen Instanz-ID durchzuführen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Informationen zum Aufrufen von untergeordneten Orchestrierungen](durable-functions-sub-orchestrations.md)

> [!div class="nextstepaction"]
> [Ausführen eines Singleton-Beispiels](durable-functions-counter.md)
