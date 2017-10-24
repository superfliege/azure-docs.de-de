---
title: "Endlose Orchestrierungen in Durable Functions – Azure"
description: "Es wird beschrieben, wie Sie endlose Orchestrierungen implementieren, indem Sie die Erweiterung „Durable Functions“ für Azure Functions verwenden."
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
ms.openlocfilehash: 79fcbbf5f506858789a6bd1e6ad2e292c72b65a4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="eternal-orchestrations-in-durable-functions-azure-functions"></a>Endlose Orchestrierungen in Durable Functions (Azure Functions)

*Endlose Orchestrierungen* sind Orchestratorfunktionen, die niemals enden. Sie sind nützlich, wenn Sie [Durable Functions](durable-functions-overview.md) für Aggregatoren und Szenarien einsetzen möchten, für die eine Endlosschleife erforderlich ist.

## <a name="orchestration-history"></a>Orchestrierungsverlauf

Wie unter [Prüfpunkte und Wiedergabe in Durable Functions (Azure Functions)](durable-functions-checkpointing-and-replay.md) beschrieben, verfolgt das Durable Task Framework den Verlauf jeder einzelnen Funktionsorchestrierung nach. Dieser Verlauf nimmt ständig an Umfang zu, solange die Orchestratorfunktion neue Arbeit plant. Wenn die Orchestratorfunktion in eine Endlosschleife eintritt und fortlaufend Arbeit plant, kann dieser Verlauf eine kritische Größe erreichen und zu erheblichen Leistungsproblemen führen. Das Konzept der *endlosen Orchestrierung* wurde entworfen, um diese Art von Problemen für Anwendungen zu lösen, die Endlosschleifen benötigen.

## <a name="resetting-and-restarting"></a>Zurücksetzen und Neustarten

Anstatt Endlosschleifen zu verwenden, setzen Orchestratorfunktionen ihren Status zurück, indem sie die [ContinueAsNew](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_ContinueAsNew_)-Methode aufrufen. Bei dieser Methode wird ein einzelner Parameter verwendet, der per JSON serialisierbar ist. Er wird zur neuen Eingabe für die nächste Generierung einer Orchestratorfunktion.

Wenn `ContinueAsNew` aufgerufen wird, reiht die Instanz vor dem Beenden eine Nachricht an sich selbst in die Warteschlange ein. Die Nachricht bewirkt, dass die Instanz mit dem neuen Eingabewert neu gestartet wird. Es wird dieselbe Instanz-ID beibehalten, aber der Verlauf der Orchestratorfunktion wird praktisch abgeschnitten.

> [!NOTE]
> Das Durable Task Framework behält dieselbe Instanz-ID bei, aber intern wird eine neue *Ausführungs-ID* für die Orchestratorfunktion erstellt, die mit `ContinueAsNew` zurückgesetzt wird. Diese Ausführungs-ID wird im Allgemeinen nicht extern verfügbar gemacht, aber es kann hilfreich sein, sie zu kennen, wenn das Debuggen der Orchestrierungsausführung erfolgen soll.

## <a name="periodic-work-example"></a>Beispiel für regelmäßige Arbeit

Ein Anwendungsfall für endlose Orchestrierungen ist Code, der unendlich lange regelmäßig Arbeitsschritte ausführen muss.

```csharp
[FunctionName("Periodic_Cleanup_Loop")]
public static async Task Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    await context.CallActivityAsync("DoCleanup");

    // sleep for one hour between cleanups
    DateTime nextCleanup = context.CurrentUtcDateTime.AddHours(1);
    await context.CreateTimer<string>(nextCleanup);

    context.ContinueAsNew();
}
```

Der Unterschied zwischen diesem Beispiel und einer per Timer ausgelösten Funktion besteht darin, dass Bereinigungstriggerzeiten hier nicht auf einem Zeitplan basieren. Beispiel: Ein CRON-Zeitplan, für den stündlich eine Funktion ausgeführt wird (um 1:00, 2:00, 3:00 Uhr usw.), können ggf. Probleme mit Überlappungen auftreten. Wenn die Bereinigung in diesem Beispiel aber 30 Minuten dauert, wird sie um 1:00, 2:30, 4:00 Uhr usw. geplant, sodass es nicht zu Überlappungen kommt.

## <a name="counter-example"></a>Counter-Beispiel

Hier ist ein vereinfachtes Beispiel für eine *counter*-Funktion (Zähler) angegeben, die endlos lange auf *increment*- und *decrement*-Ereignisse lauscht.

```csharp
[FunctionName("SimpleCounter")]
public static async Task Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    int counterState = context.GetInput<int>();

    string operation = await context.WaitForExternalEvent<string>("operation");

    if (operation == "incr")
    {
        counterState++;
    }
    else if (operation == "decr")
    {
        counterState--;
    }
    
    context.ContinueAsNew(counterState);
}
```

## <a name="exit-from-an-eternal-orchestration"></a>Beenden einer endlosen Orchestrierung

Wenn eine Orchestratorfunktion schließlich abgeschlossen werden soll, müssen Sie lediglich *nicht* `ContinueAsNew` aufrufen und die Funktion enden lassen.

Falls sich eine Orchestratorfunktion in einer Endlosschleife befindet und beendet werden muss, können Sie die [TerminateAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_TerminateAsync_)-Methode verwenden. Weitere Informationen finden Sie unter [Instanzverwaltung](durable-functions-instance-management.md).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Singleton-Orchestratoren in Durable Functions (Azure Functions)](durable-functions-singletons.md)

> [!div class="nextstepaction"]
> Informationen zur [Ausführung eines Beispiels für eine endlose Orchestrierung](durable-functions-counter.md)
