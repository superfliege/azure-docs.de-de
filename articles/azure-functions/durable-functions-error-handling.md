---
title: "Fehlerbehandlung in Durable Functions – Azure"
description: "Erfahren Sie, wie Sie Fehler in der Durable Functions-Erweiterung für Azure Functions behandeln."
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
ms.openlocfilehash: ee5362d33bb9dadadb4194457cfd7726f4825f56
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="handling-errors-in-durable-functions-azure-functions"></a>Fehlerbehandlung in Durable Functions (Azure Functions)

Durable Function-Orchestrierungen sind im Code implementiert und können die Funktionen zur Fehlerbehandlung aus der Programmiersprache nutzen. Daher gibt es eigentlich keine neuen Konzepte, die Sie bei der Einbindung der Fehlerbehandlung und Kompensierung in Ihre Orchestrierungen beachten müssen. Es gibt jedoch einige wenige Verhaltensweisen, die Sie kennen sollten:

## <a name="errors-in-activity-functions"></a>Fehler in Aktivitätsfunktionen

Jede Ausnahme, die in einer Aktivitätsfunktion ausgelöst wird, wird zurück zur Orchestratorfunktion gemarshallt und als `TaskFailedException` ausgelöst. Sie können Code zur Fehlerbehandlung und zur Kompensierung in der Orchestratorfunktion schreiben, der Ihren Bedürfnissen entspricht.

Betrachten Sie beispielsweise die folgende Orchestratorfunktion, die Gelder von einem Konto auf ein anderes überträgt:

```csharp
#r "Microsoft.Azure.WebJobs.Extensions.DurableTask"

public static async Task Run(DurableOrchestrationContext context)
{
    var transferDetails = ctx.GetInput<TransferOperation>();

    await context.CallActivityAsync("DebitAccount",
        new
        { 
            Account = transferDetails.SourceAccount,
            Amount = transferDetails.Amount
        });

    try
    {
        await context.CallActivityAsync("CreditAccount",         
            new
            { 
                Account = transferDetails.DestinationAccount,
                Amount = transferDetails.Amount
            });
    }
    catch (Exception)
    {
        // Refund the source account.
        // Another try/catch could be used here based on the needs of the application.
        await context.CallActivityAsync("CreditAccount",         
            new
            { 
                Account = transferDetails.SourceAccount,
                Amount = transferDetails.Amount
            });
    }
}
```

Wenn der Aufruf der **CreditAccount**-Funktion (Kreditkonto) für das Zielkonto fehlschlägt, wird dies durch die Orchestratorfunktion kompensiert, indem die Gelder auf das Quellkonto zurücküberwiesen werden.

## <a name="automatic-retry-on-failure"></a>Automatische Wiederholung bei einem Fehler

Wenn Sie Aktivitätsfunktionen oder untergeordnete Orchestrierungfunktionen aufrufen, können Sie eine automatische Wiederholungsrichtlinie angeben. Im folgenden Beispiel wird versucht, eine Funktion bis zu 3 Mal aufzurufen, mit 5 Sekunden Wartezeit zwischen den einzelnen Wiederholungsversuchen:

```csharp
public static async Task Run(DurableOrchestrationContext context)
{
    var retryOptions = new RetryOptions(
        firstRetryInterval: TimeSpan.FromSeconds(5),
        maxNumberOfAttempts: 3);

    await ctx.CallActivityWithRetryAsync("FlakyFunction", retryOptions);
    
    // ...
}
```

Die `CallActivityWithRetryAsync`-API erstellt einen `RetryOptions`-Parameter. Untergeordnete Orchestrierungsaufrufe, die die `CallSubOrchestratorWithRetryAsync`-API verwenden, können ebendiese Wiederholungsrichtlinien verwenden.

Es stehen mehrere Optionen zur Verfügung, um die automatische Wiederholungsrichtlinie anzupassen. Dazu zählen:

* **Max number of attempts** (Maximale Anzahl von Versuchen): Die maximale Anzahl von Wiederholungsversuchen.
* **First retry interval** (Erstes Wiederholungsintervall): Die Zeitspanne bis zum Ablauf des ersten Wiederholungsversuchs.
* **Backoff-Koeffizient**: Der Koeffizient, der verwendet wird, um den Anstieg der Backoff-Intervalle zu bestimmen. Der Standardwert lautet 1.
* **Max retry interval** (Maximales Wiederholungsintervall): die maximale Zeitspanne zwischen den Wiederholungsversuchen.
* **Retry timeout** (Timeout wiederholen): die maximale Zeitspanne für das Ausführen von Wiederholungsversuchen. Das Standardverhalten ist das Wiederholen auf unbestimmte Zeit.
* **Benutzerdefiniert**: Ein benutzerdefinierter Rückruf kann angegeben werden, der bestimmt, ob ein Funktionsaufruf wiederholt werden soll.

## <a name="function-timeouts"></a>Funktion-Timeouts

Vielleicht möchten Sie einen Funktionsaufruf innerhalb einer Orchestratorfunktion verwerfen, wenn der Vorgang zu lange dauert. Die richtige Vorgehensweise ist das Erstellen eines [durable timer](durable-functions-timers.md) (permanenter Timer) mit `context.CreateTimer` in Verbindung mit `Task.WhenAny`, wie im folgenden Beispiel:

```csharp
public static async Task<bool> Run(DurableOrchestrationContext context)
{
    TimeSpan timeout = TimeSpan.FromSeconds(30);
    DateTime deadline = context.CurrentUtcDateTime.Add(timeout);

    using (var cts = new CancellationTokenSource())
    {
        Task activityTask = context.CallActivityAsync("FlakyFunction");
        Task timeoutTask = context.CreateTimer(deadline, cts.Token);

        Task winner = await Task.WhenAny(activityTask, timeoutTask);
        if (winner == activityTask)
        {
            // success case
            cts.Cancel();
            return true;
        }
        else
        {
            // timeout case
            return false;
        }
    }
}
```

## <a name="unhandled-exceptions"></a>Nicht behandelte Ausnahmen

Fällt eine Orchestratorfunktion mit einer nicht behandelten Ausnahme aus, werden die Details der Ausnahme protokolliert, und die Instanz wird mit einem `Failed`-Status abgeschlossen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Learn how to diagnose problems (Informationen zur Diagnose von Problemen)](durable-functions-diagnostics.md)
