---
title: Untergeordnete Orchestrierungen für Durable Functions – Azure
description: Es wird beschrieben, wie Sie Orchestrierungen aus Orchestrierungen in der Erweiterung „Durable Functions“ für Azure Functions aufrufen.
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
ms.openlocfilehash: 7545a371749ed9af88f08af23cce3a513f494374
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
ms.locfileid: "33761332"
---
# <a name="sub-orchestrations-in-durable-functions-azure-functions"></a>Untergeordnete Orchestrierungen in Durable Functions (Azure Functions)

Orchestratorfunktionen können nicht nur Aktivitätsfunktionen aufrufen, sondern auch andere Orchestratorfunktionen. Beispielsweise können Sie aus einer Bibliothek mit Orchestratorfunktionen eine größere Orchestrierung erstellen. Oder Sie können mehrere Instanzen einer Orchestratorfunktion parallel ausführen.

Eine Orchestratorfunktion kann eine andere Orchestratorfunktion über die [CallSubOrchestratorAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CallSubOrchestratorAsync_)- oder die [CallSubOrchestratorWithRetryAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CallSubOrchestratorWithRetryAsync_)-Methode aufrufen. Der Artikel zur [Fehlerbehandlung und -kompensierung](durable-functions-error-handling.md#automatic-retry-on-failure) enthält weitere Informationen zur automatischen Wiederholung.

Untergeordnete Orchestratorfunktionen verhalten sich aus Sicht des Aufrufers genauso wie Aktivitätsfunktionen. Sie können einen Wert zurückgeben und eine Ausnahme auslösen, und die übergeordnete Orchestratorfunktion kann sie erwarten.

> [!NOTE]
> Die Methoden `CallSubOrchestratorAsync` und `CallSubOrchestratorWithRetryAsync` sind in JavaScript noch nicht verfügbar.

## <a name="example"></a>Beispiel

Im folgenden Beispiel wird ein IoT-Szenario (Internet der Dinge) veranschaulicht, in dem mehrere Geräte bereitgestellt werden müssen. Es wird eine bestimmte Orchestrierung verwendet, die jeweils für alle Geräte durchgeführt werden muss. Diese kann in etwa wie folgt aussehen:

```csharp
public static async Task DeviceProvisioningOrchestration(
    [OrchestrationTrigger] DurableOrchestrationContext ctx)
{
    string deviceId = ctx.GetInput<string>();

    // Step 1: Create an installation package in blob storage and return a SAS URL.
    Uri sasUrl = await ctx.CallActivityAsync<Uri>("CreateInstallationPackage", deviceId);

    // Step 2: Notify the device that the installation package is ready.
    await ctx.CallActivityAsync("SendPackageUrlToDevice", Tuple.Create(deviceId, sasUrl));

    // Step 3: Wait for the device to acknowledge that it has downloaded the new package.
    await ctx.WaitForExternalEvent<bool>("DownloadCompletedAck");

    // Step 4: ...
}
```

Diese Orchestratorfunktion kann für eine einmalige Gerätebereitstellung unverändert verwendet werden oder Teil einer größeren Orchestrierung sein. Im letzteren Fall kann die übergeordnete Orchestratorfunktion Instanzen von `DeviceProvisioningOrchestration` mit der `CallSubOrchestratorAsync`-API planen.

In diesem Beispiel wird veranschaulicht, wie Sie mehrere Orchestratorfunktionen parallel ausführen.

```csharp
[FunctionName("ProvisionNewDevices")]
public static async Task ProvisionNewDevices(
    [OrchestrationTrigger] DurableOrchestrationContext ctx)
{
    string[] deviceIds = await ctx.CallActivityAsync<string[]>("GetNewDeviceIds");

    // Run multiple device provisioning flows in parallel
    var provisioningTasks = new List<Task>();
    foreach (string deviceId in deviceIds)
    {
        Task provisionTask = ctx.CallSubOrchestratorAsync("DeviceProvisioningOrchestration", deviceId);
        provisioningTasks.Add(provisionTask);
    }

    await Task.WhenAll(provisioningTasks);

    // ...
}
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Informationen zu Aufgabenhubs und ihrer Konfiguration](durable-functions-task-hubs.md)
