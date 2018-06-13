---
title: Behandeln von externen Ereignissen in Durable Functions – Azure
description: Es wird beschrieben, wie Sie externe Ereignisse in der Erweiterung Durable Functions für Azure Functions behandeln.
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
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: 77087f04ea641c24a92edd2091432cbcb4329ecd
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
ms.locfileid: "33763194"
---
# <a name="handling-external-events-in-durable-functions-azure-functions"></a>Behandeln von externen Ereignissen in Durable Functions (Azure Functions)

Orchestratorfunktionen können warten und auf externe Ereignisse lauschen. Das Feature [Durable Functions](durable-functions-overview.md) ist oft hilfreich für die Behandlung menschlicher Interaktion oder anderer externer Trigger.

## <a name="wait-for-events"></a>Warten auf Ereignisse

Die [WaitForExternalEvent](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_WaitForExternalEvent_)-Methode ermöglicht einer Orchestratorfunktion, asynchron auf ein externes Ereignis zu warten und darauf zu lauschen. Die lauschende Orchestratorfunktion deklariert den *Namen* des Ereignisses und die *Form der Daten*, die sie zu empfangen erwartet.

#### <a name="c"></a>C#

```csharp
[FunctionName("BudgetApproval")]
public static async Task Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    bool approved = await context.WaitForExternalEvent<bool>("Approval");
    if (approved)
    {
        // approval granted - do the approved action
    }
    else
    {
        // approval denied - send a notification
    }
}
```

#### <a name="javascript-functions-v2-only"></a>JavaScript (nur Functions v2)

```javascript
const df = require("durable-functions");

module.exports = df(function*(context) {
    const approved = yield context.df.waitForExternalEvent("Approval");
    if (approved) {
        // approval granted - do the approved action
    } else {
        // approval denied - send a notification
    }
});
```

Im vorangehenden Beispiel wird auf ein bestimmtes einzelnes Ereignis gelauscht und bei Empfang eine entsprechende Aktion ausgeführt.

Sie können wie im folgenden Beispiel, in dem auf eine von drei möglichen Ereignisbenachrichtigungen gewartet wird, auf mehrere Ereignisse gleichzeitig lauschen.

#### <a name="c"></a>C#

```csharp
[FunctionName("Select")]
public static async Task Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    var event1 = context.WaitForExternalEvent<float>("Event1");
    var event2 = context.WaitForExternalEvent<bool>("Event2");
    var event3 = context.WaitForExternalEvent<int>("Event3");

    var winner = await Task.WhenAny(event1, event2, event3);
    if (winner == event1)
    {
        // ...
    }
    else if (winner == event2)
    {
        // ...
    }
    else if (winner == event3)
    {
        // ...
    }
}
```

#### <a name="javascript-functions-v2-only"></a>JavaScript (nur Functions v2)

```javascript
const df = require("durable-functions");

module.exports = df(function*(context) {
    const event1 = context.df.waitForExternalEvent("Event1");
    const event2 = context.df.waitForExternalEvent("Event2");
    const event3 = context.df.waitForExternalEvent("Event3");

    const winner = yield context.df.Task.any([event1, event2, event3]);
    if (winner === event1) {
        // ...
    } else if (winner === event2) {
        // ...
    } else if (winner === event3) {
        // ...
    }
});
```

Im vorherigen Beispiel wird auf *alle* von mehreren Ereignissen gelauscht. Es ist auch möglich, auf *alle* Ereignisse zu warten.

#### <a name="c"></a>C#

```csharp
[FunctionName("NewBuildingPermit")]
public static async Task Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    string applicationId = context.GetInput<string>();

    var gate1 = context.WaitForExternalEvent("CityPlanningApproval");
    var gate2 = context.WaitForExternalEvent("FireDeptApproval");
    var gate3 = context.WaitForExternalEvent("BuildingDeptApproval");

    // all three departments must grant approval before a permit can be issued
    await Task.WhenAll(gate1, gate2, gate3);

    await context.CallActivityAsync("IssueBuildingPermit", applicationId);
}
```

#### <a name="javascript-functions-v2-only"></a>JavaScript (nur Functions v2)

```javascript
const df = require("durable-functions");

module.exports = df(function*(context) {
    const applicationId = context.df.getInput();

    const gate1 = context.df.waitForExternalEvent("CityPlanningApproval");
    const gate2 = context.df.waitForExternalEvent("FireDeptApproval");
    const gate3 = context.df.waitForExternalEvent("BuildingDeptApproval");

    // all three departments must grant approval before a permit can be issued
    yield context.df.Task.all([gate1, gate2, gate3]);

    yield context.df.callActivityAsync("IssueBuildingPermit", applicationId);
});
```

[WaitForExternalEvent](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_WaitForExternalEvent_) wartet unbegrenzt auf Eingaben.  Die Funktionen-App kann während des Wartens problemlos entladen werden. Wenn ein Ereignis für diese Orchestrierungsinstanz eingeht, wird sie automatisch aktiviert und verarbeitet das Ereignis sofort.

> [!NOTE]
> Wenn Ihre Funktions-App den Verbrauchstarif verwendet, fallen, während eine Orchestratorfunktion auf eine Aufgabe von `WaitForExternalEvent` wartet, keine Kosten an, unabhängig davon, wie lange sie wartet.

Wenn in .NET die Ereignisnutzlast nicht in den erwarteten Typ `T` konvertiert werden kann, wird eine Ausnahme ausgelöst.

## <a name="send-events"></a>Senden von Ereignisse

Die [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_)-Methode der [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html)-Klasse sendet die Ereignisse, auf die `WaitForExternalEvent` wartet.  Die `RaiseEventAsync`-Methode übernimmt *eventName* und *eventData* als Parameter. Die Ereignisdaten müssen JSON-serialisierbar sein.

Im Folgenden finden Sie eine durch eine Warteschlange ausgelöste Beispielfunktion, die ein „Genehmigungs“-Ereignis an eine Orchestratorfunktionsinstanz sendet. Die Orchestrierungsinstanz-ID stammt aus dem Text der Warteschlangennachricht.

```csharp
[FunctionName("ApprovalQueueProcessor")]
public static async Task Run(
    [QueueTrigger("approval-queue")] string instanceId,
    [OrchestrationClient] DurableOrchestrationClient client)
{
    await client.RaiseEventAsync(instanceId, "Approval", true);
}
```

Intern fügt `RaiseEventAsync` eine Nachricht in die Warteschlange ein, die von der wartenden Orchestratorfunktion übernommen wird.

> [!WARNING]
> Wenn keine Orchestrierungsinstanz mit der angegebenen *Instanz-ID* vorhanden ist, oder die Instanz nicht auf den angegebenen *Ereignisnamen* wartet, wird die Ereignisnachricht verworfen. Weitere Informationen zu diesem Verhalten finden Sie unter [Expected behavior when unexpected event is received?](https://github.com/Azure/azure-functions-durable-extension/issues/29) (Erwartetes Verhalten, wenn ein unerwartetes Ereignis empfangen wird?).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Eternal orchestrations in Durable Functions (Azure Functions)](durable-functions-eternal-orchestrations.md) (Ewige Orchestrierungen in Durable Functions [Azure Functions])

> [!div class="nextstepaction"]
> [Stateful singletons in Durable Functions - Counter sample](durable-functions-phone-verification.md) (Statusbehaftete Singletons in Durable Functions – Zählerbeispiel)

> [!div class="nextstepaction"]
> [Human interaction in Durable Functions - Phone verification sample](durable-functions-phone-verification.md) (Menschliche Interaktion in Durable Functions – Telefonüberprüfungsbeispiel)

