---
title: "Diagnose in Durable Functions – Azure"
description: "Es wird beschrieben, wie Sie mit der Erweiterung „Durable Functions“ für Azure Functions Probleme diagnostizieren."
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
ms.openlocfilehash: 5ebab8660dfe21984e1a7f9a1cb925aea60de213
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="diagnostics-in-durable-functions-azure-functions"></a>Diagnose in Durable Functions (Azure Functions)

Es gibt mehrere Möglichkeiten, um Probleme mithilfe von [Durable Functions](durable-functions-overview.md) zu diagnostizieren. Einige dieser Optionen sind für reguläre Funktionen identisch, und andere gelten nur für Durable Functions.

## <a name="application-insights"></a>Application Insights

Es wird empfohlen, [Application Insights](../application-insights/app-insights-overview.md) zum Durchführen der Diagnose und Überwachung in Azure Functions zu verwenden. Dasselbe gilt für Durable Functions. Eine Übersicht darüber, wie Sie Application Insights in Ihrer Funktionen-App nutzen, finden Sie unter [Überwachen von Azure Functions](functions-monitoring.md).

Die Erweiterung „Durable Functions“ von Azure Functions gibt auch *Nachverfolgungsereignisse* aus, mit denen Sie die End-to-End-Ausführung einer Orchestrierung verfolgen können. Sie können sie ermitteln und abfragen, indem Sie im Azure-Portal das Tool [Application Insights Analytics](../application-insights/app-insights-analytics.md) verwenden.

### <a name="tracking-data"></a>Nachverfolgungsdaten

Jedes Lebenszyklusereignis einer Orchestrierungsinstanz bewirkt, dass in Application Insights ein Nachverfolgungsereignis in die Sammlung **traces** geschrieben wird. Dieses Ereignis enthält die Nutzlast **customDimensions** mit mehreren Feldern.  Feldnamen wird immer `prop__` vorangestellt.

* **hubName**: Der Name des Aufgabenhub, unter dem Ihre Orchestrierungen ausgeführt werden.
* **appName**: Der Name der Funktionen-App. Dies ist nützlich, wenn mehrere Funktionen-Apps dieselbe Application Insights-Instanz gemeinsam nutzen.
* **slotName**: Der [Bereitstellungsslot](https://blogs.msdn.microsoft.com/appserviceteam/2017/06/13/deployment-slots-preview-for-azure-functions/), in dem die aktuelle Funktionen-App ausgeführt wird. Dies ist nützlich, wenn Sie Bereitstellungsslots nutzen, um Ihre Orchestrierungen mit einer Version zu versehen.
* **functionName**: Der Name der Orchestrator- oder Aktivitätsfunktion.
* **functionType**: Der Typ der Funktion, z.B. **Orchestrator** oder **Activity**.
* **instanceId**: Die eindeutige ID der Orchestrierungsinstanz.
* **state**: Der Lebenszyklus-Ausführungsstatus der Instanz. Folgende Werte sind gültig:
    * **Scheduled**: Die Funktion wurde für die Ausführung eingeplant, aber die Ausführung wurde noch nicht gestartet.
    * **Started**: Die Ausführung der Funktion wurde gestartet, aber der Wartezustand oder der Abschluss ist noch nicht erreicht.
    * **Awaited**: Der Orchestrator hat Arbeit geplant und wartet darauf, dass sie abgeschlossen wird.
    * **Listening**: Der Orchestrator lauscht auf eine externe Ereignisbenachrichtigung.
    * **Completed**: Die Funktion wurde erfolgreich abgeschlossen.
    * **Failed**: Für die Funktion ist ein Fehler aufgetreten.
* **reason**: Zusätzliche Daten zum Nachverfolgungsereignis. Wenn eine Instanz beispielsweise auf eine externe Ereignisbenachrichtigung wartet, wird in diesem Feld der Name des Ereignisses angegeben, auf das gewartet wird. Wenn eine Funktion fehlgeschlagen ist, sind hier die Fehlerdetails enthalten.
* **isReplay**: Boolescher Wert, der angibt, ob das Nachverfolgungsereignis für die wiedergegebene Ausführung bestimmt ist.
* **extensionVersion**: Die Version der Erweiterung „Durable Task“. Dies ist besonders wichtig, wenn mögliche Fehler der Erweiterung gemeldet werden. Instanzen mit langer Ausführungsdauer melden unter Umständen mehrere Versionen, wenn während der Ausführung ein Update durchgeführt wird. 

Der Ausführlichkeitsgrad der Nachverfolgung von Daten, die für Application Insights ausgegeben werden, kann im Abschnitt `logger` der Datei `host.json` konfiguriert werden.

```json
{
    "logger": {
        "categoryFilter": {
            "categoryLevels": {
                "Host.Triggers.DurableTask": "Information"
            }
        }
    }
}
```

Standardmäßig werden alle Nachverfolgungsereignisse ausgegeben. Die Datenmenge kann reduziert werden, indem `Host.Triggers.DurableTask` auf `"Warning"` oder `"Error"` festgelegt wird. In diesem Fall werden Nachverfolgungsereignisse nur für außergewöhnliche Situationen ausgegeben.

> [!WARNING]
> Standardmäßig werden für die Application Insights-Telemetriedaten von der Azure Functions-Laufzeit Stichproben erstellt, um zu verhindern, dass Daten zu häufig ausgegeben werden. Dies kann dazu führen, dass Nachverfolgungsinformationen verloren gehen, wenn viele Lebenszyklusereignisse in kurzer Zeit auftreten. Im [Artikel zur Azure Functions-Überwachung](functions-monitoring.md#configure-sampling) wird beschrieben, wie Sie dieses Verhalten konfigurieren.

### <a name="single-instance-query"></a>Abfrage für einzelne Instanzen

Mit der folgenden Abfrage werden Verlaufsdaten für die Nachverfolgung einer Einzelinstanz der [Hello Sequence](durable-functions-sequence.md)-Funktionsorchestrierung angezeigt. Sie wurde mit der [Application Insights Query Language (AIQL)](https://docs.loganalytics.io/docs/Language-Reference) geschrieben. Die Wiedergabeausführung wird herausgefiltert, sodass nur der *logische* Ausführungspfad angezeigt wird.

```AIQL
let targetInstanceId = "bf71335b26564016a93860491aa50c7f";
let start = datetime(2017-09-29T00:00:00);
traces
| where timestamp > start and timestamp < start + 30m
| where customDimensions.Category == "Host.Triggers.DurableTask"
| extend functionName = customDimensions["prop__functionName"]
| extend instanceId = customDimensions["prop__instanceId"]
| extend state = customDimensions["prop__state"]
| extend isReplay = tobool(tolower(customDimensions["prop__isReplay"]))
| where isReplay == false
| where instanceId == targetInstanceId
| project timestamp, functionName, state, instanceId, appName = cloud_RoleName
```
Das Ergebnis ist eine Liste mit Nachverfolgungsereignissen, für die der Ausführungspfad der Orchestrierung angezeigt wird, z.B. alle Aktivitätsfunktionen.

![Application Insights-Abfrage](media/durable-functions-diagnostics/app-insights-single-instance-query.png)

> [!NOTE]
> Einige dieser Nachverfolgungsereignisse sind aufgrund der fehlenden Genauigkeit in der Spalte `timestamp` unter Umständen fehlerhaft. Dies wird auf GitHub als [Issue #71](https://github.com/Azure/azure-functions-durable-extension/issues/71) (Problem 71) nachverfolgt.

### <a name="instance-summary-query"></a>Instanz-Zusammenfassungsabfrage

Mit der folgenden Abfrage wird der Status aller Orchestrierungsinstanzen angezeigt, die in einem angegebenen Zeitraum ausgeführt wurden.

```AIQL
let start = datetime(2017-09-30T04:30:00);
traces
| where timestamp > start and timestamp < start + 1h
| where customDimensions.Category == "Host.Triggers.DurableTask"
| extend functionName = tostring(customDimensions["prop__functionName"])
| extend instanceId = tostring(customDimensions["prop__instanceId"])
| extend state = tostring(customDimensions["prop__state"])
| extend isReplay = tobool(tolower(customDimensions["prop__isReplay"]))
| extend output = tostring(customDimensions["prop__output"])
| where isReplay == false
| summarize arg_max(timestamp, *) by instanceId
| project timestamp, instanceId, functionName, state, output, appName = cloud_RoleName
| order by timestamp asc
```
Das Ergebnis ist eine Liste mit Instanz-IDs und dem aktuellen Laufzeitstatus.

![Application Insights-Abfrage](media/durable-functions-diagnostics/app-insights-single-summary-query.png)

## <a name="logging"></a>Protokollierung

Es ist wichtig, das Wiedergabeverhalten von Orchestratoren zu beachten, wenn Protokolle direkt über eine Orchestratorfunktion geschrieben werden. Sehen Sie sich beispielsweise die folgende Orchestratorfunktion an:

```cs
public static async Task Run(
    DurableOrchestrationContext ctx,
    TraceWriter log)
{
    log.Info("Calling F1.");
    await ctx.CallActivityAsync("F1");
    log.Info("Calling F2.");
    await ctx.CallActivityAsync("F2");
    log.Info("Calling F3");
    await ctx.CallActivityAsync("F3");
    log.Info("Done!");
}
```

Die sich ergebenden Protokolldaten sehen dann in etwa wie folgt aus:

```txt
Calling F1.
Calling F1.
Calling F2.
Calling F1.
Calling F2.
Calling F3.
Calling F1.
Calling F2.
Calling F3.
Done!
```

> [!NOTE]
> Beachten Sie Folgendes: In den Protokollen ist zwar angegeben, dass F1, F2 und F3 aufgerufen werden, aber diese Funktionen werden *nur* beim ersten Auftreten aufgerufen. Nachfolgende Aufrufe während der Wiedergabe werden übersprungen, und die Ausgaben werden für die Orchestratorlogik wiedergegeben.

Wenn Sie nur die Anmeldung für die Ausführung ohne Wiedergabe durchführen möchten, können Sie einen bedingten Ausdruck schreiben, damit die Anwendung nur erfolgt, sofern `IsReplaying` auf `false` festgelegt ist. Hier ist das obige Beispiel angegeben, aber es enthält jetzt Wiedergabeprüfungen.

```cs
public static async Task Run(
    DurableOrchestrationContext ctx,
    TraceWriter log)
{
    if (!ctx.IsReplaying) log.Info("Calling F1.");
    await ctx.CallActivityAsync("F1");
    if (!ctx.IsReplaying) log.Info("Calling F2.");
    await ctx.CallActivityAsync("F2");
    if (!ctx.IsReplaying) log.Info("Calling F3");
    await ctx.CallActivityAsync("F3");
    log.Info("Done!");
}
```
Nach dieser Änderung lautet die Protokollausgabe wie folgt:

```txt
Calling F1.
Calling F2.
Calling F3.
Done!
```

## <a name="debugging"></a>Debuggen

Azure Functions unterstützt das direkte Debuggen des Funktionscodes, und diese Unterstützung gilt auch für Durable Functions – unabhängig davon, ob die Ausführung in Azure oder lokal erfolgt. Beim Debuggen sollten aber einige Verhaltensweisen beachtet werden:

* **Replay** (Wiedergabe): Orchestratorfunktionen werden regelmäßig wiederholt, wenn neue Eingaben empfangen werden. Dies bedeutet Folgendes: Eine einzelne *logische* Ausführung einer Orchestratorfunktion kann dazu führen, dass derselbe Breakpoint mehrfach erreicht wird. Dies gilt vor allem, wenn er sich im Anfangsbereich des Funktionscodes befindet.
* **Await** (Wartezustand): Immer wenn das `await`-Element auftritt, wird die Steuerung zurück an den Durable Task Framework-Verteiler übergeben. Wenn dies das erste Mal ist, dass ein bestimmtes `await`-Element auftritt, wird die zugeordnete Aufgabe *nie* fortgesetzt. Aus diesem Grund ist das *Überspringen* des Wartezustands (F10 in Visual Studio) nicht möglich. Das Überspringen funktioniert nur, wenn eine Aufgabe wiedergegeben wird.
* **Messaging timeouts** (Messaging-Timeouts): In Durable Functions werden intern Warteschlangennachrichten für die Ausführung von Orchestrator- und Aktivitätsfunktionen verwendet. In einer Umgebung mit mehreren VMs kann das Unterbrechen des Debuggens über längere Zeiträume dazu führen, dass eine andere VM die Nachricht aufnimmt. Das Ergebnis wäre dann eine doppelte Ausführung. Dieses Verhalten tritt auch für reguläre Warteschlangentriggerfunktionen auf, aber es ist wichtig, dies in diesem Zusammenhang zu erwähnen, da es sich bei den Warteschlangen um ein Implementierungsdetail handelt.

> [!TIP]
> Gehen Sie wie folgt vor, wenn Sie beim Festlegen von Breakpoints erreichen möchten, dass nur bei Nichtwiedergabeausführungen unterbrochen wird: Legen Sie einen bedingten Breakpoint fest, bei dem nur eine Unterbrechung auftritt, wenn `IsReplaying` den Wert `false` hat.

## <a name="storage"></a>Speicher

Durable Functions speichert den Status standardmäßig in Azure Storage. Dies bedeutet, dass Sie den Status Ihrer Orchestrierungen mit Tools wie [Microsoft Azure Storage-Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer) untersuchen können.

![Screenshot: Azure Storage-Explorer](media/durable-functions-diagnostics/storage-explorer.png)

Dies ist hilfreich beim Debuggen, da Sie genau sehen, in welchem Zustand sich eine Orchestrierung befindet. Nachrichten in den Warteschlangen können ebenfalls untersucht werden, um zu ermitteln, welche Arbeitsschritte ausstehen (oder ggf. hängen).

> [!WARNING]
> Es ist zwar bequem, den Ausführungsverlauf im Tabellenspeicher angezeigt zu bekommen, aber Sie sollten es vermeiden, Abhängigkeiten von dieser Tabelle einzurichten. Dies kann sich ggf. ändern, wenn die Erweiterung „Durable Functions“ weiterentwickelt wird.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Informationen zur Verwendung von langlebigen Timern](durable-functions-timers.md)