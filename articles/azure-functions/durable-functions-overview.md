---
title: "Übersicht über Durable Functions – Azure (Vorschauversion)"
description: "Einführung in die Durable Functions-Erweiterung für Azure Functions."
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
ms.openlocfilehash: fa0d5cf7469a1a36fe0ab9a712cd4f8c963ceb48
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2017
---
# <a name="durable-functions-overview-preview"></a>Übersicht über Durable Functions (Vorschauversion)

*Durable Functions* ist eine Erweiterung von [Azure Functions](functions-overview.md) und [Azure WebJobs](../app-service/web-sites-create-web-jobs.md), mit der Sie zustandsbehaftete Funktionen in einer serverlosen Umgebung schreiben können. Die Erweiterung verwaltet Status, Prüfpunkte und Neustarts für Sie.

Mit der Erweiterung können Sie zustandsbehaftete Workflows in einer neuen, als *Orchestratorfunktion* bezeichneten Art von Funktion definieren. Einige Vorteile der Orchestratorfunktionen sind:

* Sie definieren Workflows im Code. Keine JSON-Schemas oder Designer sind erforderlich.
* Sie können andere Funktionen synchron und asynchron aufrufen. Ausgabe von aufgerufenen Funktionen kann in lokalen Variablen gespeichert werden.
* Sie erstellen immer dann, wenn die Funktion wartet, automatisch Prüfpunkte zu ihrem Status. Der lokale Zustand geht nie verloren, wenn der Prozess oder virtuelle Computer neu gestartet wird.

> [!NOTE]
> Durable Functions befindet sich momentan in der Vorschauphase und ist eine fortschrittliche Erweiterung für Azure Functions, die nicht für alle Anwendungen geeignet ist. Im weiteren Verlauf dieses Artikels wird vorausgesetzt, dass Sie mit den Konzepten von [Azure Functions](functions-overview.md) und den Herausforderungen serverloser Anwendungsentwicklung bestens vertraut sind.

Der primäre Anwendungsfall für Durable Functions ist die Vereinfachung komplexer, zustandsbehafteter Koordinationsprobleme in serverlosen Anwendungen. In den folgenden Abschnitten werden einige typische Anwendungsmuster beschrieben, die von Durable Functions profitieren können.

## <a name="pattern-1-function-chaining"></a>Muster 1: Funktionsverkettung

Bei der *Funktionsverkettung* geht es um das Muster für die Ausführung einer Funktionssequenz in einer bestimmten Reihenfolge. Häufig muss die Ausgabe einer Funktion auf die Eingabe einer anderen Funktion angewendet werden.

![Funktionsverkettungsdiagramm](media/durable-functions-overview/function-chaining.png)

Mit Durable Functions können Sie dieses Muster präzise im Code implementieren.

```cs
public static async Task<object> Run(DurableOrchestrationContext ctx)
{
    try
    {
        var x = await ctx.CallActivityAsync<object>("F1");
        var y = await ctx.CallActivityAsync<object>("F2", x);
        var z = await ctx.CallActivityAsync<object>("F3", y);
        return  await ctx.CallActivityAsync<object>("F4", z);
    }
    catch (Exception)
    {
        // error handling/compensation goes here
    }
}
```

Die Werte „F1“, „F2“, „F3“ und „F4“ sind die Namen von weiteren Funktionen in der Funktionen-App. Die Ablaufsteuerung wird mithilfe normaler imperativer Codierungskonstrukte implementiert. Dies bedeutet, dass der Code von oben nach unten ausgeführt wird und bestehende sprachliche Ablaufsteuerungssemantik wie Bedingungsanweisungen und Schleifen umfassen kann.  Fehlerbehandlungscode kann in Try/Catch/Finally-Blöcken enthalten sein.

Der `ctx`-Parameter ([DurableOrchestrationContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html)) stellt Methoden zum Aufrufen anderer Funktionen anhand des Namens, Übergeben von Parametern und zur Rückgabe von Funktionausgabe bereit. Jedes Mal, wenn der Code `await` aufruft, erstellt das Framework von Durable Functions *Prüfpunkte* zum Status der aktuellen Funktionsinstanz. Wenn der Prozess oder virtuelle Computer mitten in der Ausführung neu gestartet wird, wird die Funktionsinstanz ab dem vorherigen `await`-Aufruf fortgesetzt. Später wird dieses Neustartverhalten näher erläutert.

## <a name="pattern-2-fan-outfan-in"></a>Muster 2: Auffächern nach außen/innen

*Auffächern nach außen/innen* bezieht sich auf das Muster der gleichzeitigen Ausführung mehrerer Funktionen und anschließendes Warten darauf, dass alle beendet werden.  Häufig werden von Funktionen zurückgegebene Ergebnisse einer Aggregation unterzogen.

![Diagramm zum Auffächern nach außen/innen](media/durable-functions-overview/fan-out-fan-in.png)

Bei normalen Funktionen kann die Funktion zum Auffächern nach außen mehrere Nachrichten an eine Warteschlange senden. Das Auffächern zurück nach innen ist jedoch wesentlich schwieriger. Sie müssen dann Code schreiben, um nachzuverfolgen, wann die von der Warteschlange ausgelösten Funktionen enden und Funktionsausgaben speichern. Die Erweiterung Durable Functions verarbeitet dieses Muster mit relativ einfachem Code.

```cs
public static async Task Run(DurableOrchestrationContext ctx)
{
    var parallelTasks = new List<Task<int>>();
 
    // get a list of N work items to process in parallel
    object[] workBatch = await ctx.CallActivityAsync<object[]>("F1");
    for (int i = 0; i < workBatch.Length; i++)
    {
        Task<int> task = ctx.CallActivityAsync<int>("F2", workBatch[i]);
        parallelTasks.Add(task);
    }
 
    await Task.WhenAll(parallelTasks);
 
    // aggregate all N outputs and send result to F3
    int sum = parallelTasks.Sum(t => t.Result);
    await ctx.CallActivityAsync("F3", sum);
}
```

Das Auffächern nach außen wird auf mehrere Instanzen der Funktion `F2` verteilt, und die Arbeit wird mit einer dynamischen Liste von Aufgaben nachverfolgt. Die .NET-`Task.WhenAll`-API wird aufgerufen, um zu warten, bis alle aufgerufenen Funktionen beendet sind. Dann werden die Ausgaben der `F2`-Funktion aus der dynamischen Aufgabenliste aggregiert und der `F3`-Funktion übergeben.

Die automatische Prüfpunkterstellung, die beim `await`-Aufruf an `Task.WhenAll` erfolgt, stellt sicher, dass jeglicher mitten in der Ausführung stattfindende Absturz oder Neustart keinen Neustart bereits abgeschlossener Aufgaben erfordert.

## <a name="pattern-3-async-http-apis"></a>Muster 3: Asynchrone HTTP-APIs

Beim dritten Muster geht es um das Problem der Koordination des Status lange ausgeführter Vorgänge mit externen Clients. Eine allgemeine Möglichkeit, dieses Muster zu implementieren, besteht darin, die lange ausgeführte Aktion durch einen HTTP-Aufruf auszulösen und dann den Client zu einem Statusendpunkt umzuleiten, den er abfragen kann, um zu erfahren, wann der Vorgang abgeschlossen ist.

![HTTP-API-Diagramm](media/durable-functions-overview/async-http-api.png)

Durable Functions bietet integrierte APIs, die den Code vereinfachen, den Sie für die Interaktion mit langen Funktionsausführungen schreiben. Die [Beispiele](durable-functions-install.md) zeigen einen einfachen REST-Befehl, der zum Starten neuer Orchestratorfunktionsinstanzen verwendet werden kann. Sobald eine Instanz gestartet wird, macht die Erweiterung Webhook-HTTP-APIs verfügbar, die den Status der Orchestratorfunktion abfragen. Das folgende Beispiel zeigt die REST-Befehle zum Starten eines Orchestrators und zum Abfragen seines Status. Zur besseren Übersichtlichkeit werden im Beispiel einige Details weggelassen.

```
> curl -X POST https://myfunc.azurewebsites.net/orchestrators/DoWork -H "Content-Length: 0" -i
HTTP/1.1 202 Accepted
Content-Type: application/json
Location: https://myfunc.azurewebsites.net/admin/extensions/DurableTaskExtension/b79baf67f717453ca9e86c5da21e03ec

{"id":"b79baf67f717453ca9e86c5da21e03ec", ...}

> curl https://myfunc.azurewebsites.net/admin/extensions/DurableTaskExtension/b79baf67f717453ca9e86c5da21e03ec -i
HTTP/1.1 202 Accepted
Content-Type: application/json
Location: https://myfunc.azurewebsites.net/admin/extensions/DurableTaskExtension/b79baf67f717453ca9e86c5da21e03ec

{"runtimeStatus":"Running","lastUpdatedTime":"2017-03-16T21:20:47Z", ...}

> curl https://myfunc.azurewebsites.net/admin/extensions/DurableTaskExtension/b79baf67f717453ca9e86c5da21e03ec -i
HTTP/1.1 200 OK
Content-Length: 175
Content-Type: application/json

{"runtimeStatus":"Completed","lastUpdatedTime":"2017-03-16T21:20:57Z", ...}
```

Da der Status von der Durable Functions-Laufzeit verwaltet wird, müssen Sie keinen eigenen Mechanismus zur Statusnachverfolgung implementieren.

Obwohl die Erweiterung Durable Functions über integrierte Webhooks zur Verwaltung lange ausgeführter Orchestrierungen verfügt, können Sie dieses Muster unter Verwendung Ihrer eigenen Funktionstrigger (z.B. HTTP, Warteschlange oder Event Hub) und der `orchestrationClient`-Bindung selbst implementieren. Sie können z. B. eine Warteschlangennachricht verwenden, um die Beendigung auszulösen.  Sie könnten auch anstelle der integrierten Webhooks, die einen generierten Schlüssel für die Authentifizierung verwenden, einen HTTP-Trigger verwenden, der durch eine Azure Active Directory-Authentifizierungsrichtlinie geschützt ist. 

```cs
// HTTP-triggered function to start a new orchestrator function instance.
public static async Task<HttpResponseMessage> Run(
    HttpRequestMessage req,
    DurableOrchestrationClient starter,
    string functionName,
    TraceWriter log)
{
    // Function name comes from the request URL.
    // Function input comes from the request content.
    dynamic eventData = await req.Content.ReadAsAsync<object>();
    string instanceId = await starter.StartNewAsync(functionName, eventData);
    
    log.Info($"Started orchestration with ID = '{instanceId}'.");
    
    return starter.CreateCheckStatusResponse(req, instanceId);
}
```

Der [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) `starter`-Parameter ist ein Wert aus der `orchestrationClient`-Ausgabebindung, die ein Teil der Erweiterung Durable Functions ist. Sie bietet Methoden, mit denen neue oder vorhandene Orchestratorfunktionsinstanzen gestartet, Ereignisse an sie gesendet, sie beendet und Abfragen an sie gerichtet werden können. Im obigen Beispiel nimmt eine HTTP-ausgelöste-Funktion einen `functionName`-Wert von der eingehenden URL entgegen und übergibt den Wert an [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_). Diese Bindungs-API gibt dann eine Antwort zurück, die einen `Location`-Header und zusätzliche Informationen über die Instanz enthält, die später verwendet werden können, um den Status der gestarteten Instanz abzurufen oder sie zu beenden.

## <a name="pattern-4-stateful-singletons"></a>Muster 4: Zustandsbehaftete Singletons

Die meisten Funktionen haben einen expliziten Start und ein explizites Ende und interagieren nicht direkt mit externen Ereignisquellen. Orchestrierungen unterstützen jedoch ein Muster eines [zustandsbehafteten Singletons](durable-functions-singletons.md), das ihnen ermöglicht, sich in der verteilten Verarbeitung wie zuverlässige [Akteure](https://en.wikipedia.org/wiki/Actor_model) zu verhalten.

Das folgende Diagramm veranschaulicht eine Funktion, die während der Verarbeitung von Ereignissen aus externen Quellen in einer Endlosschleife ausgeführt wird.

![Diagramm eines zustandsbehafteten Singletons](media/durable-functions-overview/stateful-singleton.png)

Durable Functions ist zwar keine Implementierung des Akteurmodells, aber Orchestratorfunktionen weisen viele ähnliche Laufzeiteigenschaften auf. Beispielsweise werden sie lange (möglicherweise endlos) ausgeführt, sind zustandsbehaftet, zuverlässig, Singlethreadfunktionen, haben einen transparenten Speicherort und sind global adressierbar. Dies qualifiziert Orchestratorfunktionen für „Akteur“-ähnliche Szenarien.

Gewöhnliche Funktionen sind zustandslos und daher nicht zum Implementieren eines zustandsbehafteten Singletonmusters geeignet. Jedenfalls vereinfacht die Erweiterung Durable Functions die Implementierung des statusbehafteten Singletonmusters enorm. Der folgende Code ist eine einfache Orchestratorfunktion, die einen Leistungsindikator implementiert.

```cs
public static async Task Run(DurableOrchestrationContext ctx)
{
    int counterState = ctx.GetInput<int>();

    string operation = await ctx.WaitForExternalEvent<string>("operation");
    if (operation == "incr")
    {
        counterState++;
    }
    else if (operation == "decr")
    {
        counterState--;
    }

    ctx.ContinueAsNew(counterState);
}
```

Dieser Code könnte als „ewige Orchestrierung“ beschrieben werden – also eine beginnende, aber niemals endende. Er führt die folgenden Schritte aus:

* Beginn mit einem Eingabewert in `counterState`.
* Unbegrenztes Warten auf eine Nachricht namens `operation`.
* Ausführen von Code zum Aktualisieren seines lokalen Status.
* „Neustart“ in Eigeninitiative durch Aufrufen von `ctx.ContinueAsNew`.
* Erneut unbegrenztes Warten auf den nächsten Vorgang.

## <a name="pattern-5-human-interaction"></a>Muster 5: Benutzerinteraktion

Viele Prozesse enthalten eine Form der Benutzerinteraktion. Das Problem bei der Einbeziehung von Menschen in einen automatisierten Prozess ist, dass Personen nicht immer in gleichem Maße verfügbar und reaktionsfähig sind wie Clouddienste. Automatisierte Prozesse müssen dies tolerieren, und hierzu nutzen sie häufig Timeouts und Kompensationscode.

Ein Beispiel eines Geschäftsprozesses, der Benutzerinteraktion umfasst, ist ein Genehmigungsprozess. So ist z.B. für einen Ausgabenbericht, der einen bestimmten Betrag überschreitet, die Genehmigung eines Managers erforderlich. Wenn der Manager die Genehmigung nicht innerhalb von 72 Stunden erteilt (weil er vielleicht im Urlaub ist), wird ein Eskalationsverfahren wirksam, um die Genehmigung von einer anderen Person (z.B. dem Manager des Managers) zu erhalten.

![Diagramm zur Benutzerinteraktion](media/durable-functions-overview/approval.png)

Dieses Muster kann mithilfe einer Orchestratorfunktion implementiert werden. Der Orchestrator würde einen [permanenten Timer](durable-functions-timers.md) verwenden, um die Genehmigung anzufordern und im Falle eines Timeouts zu eskalieren. Er würde auf ein [externes Ereignis](durable-functions-external-events.md) warten – die durch Benutzerinteraktion generierte Benachrichtigung.

```cs
public static async Task Run(DurableOrchestrationContext ctx)
{
    await ctx.CallActivityAsync("RequestApproval");
    using (var timeoutCts = new CancellationTokenSource())
    {
        DateTime dueTime = ctx.CurrentUtcDateTime.AddHours(72);
        Task durableTimeout = ctx.CreateTimer(dueTime, timeoutCts.Token);

        Task<bool> approvalEvent = ctx.WaitForExternalEvent<bool>("ApprovalEvent");
        if (approvalEvent == await Task.WhenAny(approvalEvent, durableTimeout))
        {
            timeoutCts.Cancel();
            await ctx.CallActivityAsync("HandleApproval", approvalEvent.Result);
        }
        else
        {
            await ctx.CallActivityAsync("Escalate");
        }
    }
}
```

Der permanente Timer wird durch den Aufruf von `ctx.CreateTimer` erstellt. Die Benachrichtigung wird von `ctx.WaitForExternalEvent` empfangen. Außerdem wird `Task.WhenAny` aufgerufen, um zu entscheiden, ob (sobald der Timeout in Kraft tritt) eskaliert oder (wenn die Genehmigung vor dem Inkrafttreten des Timeouts erfolgt) die Genehmigung verarbeitet wird.

## <a name="the-technology"></a>Die Technologie

Im Hintergrund baut die Erweiterung Durable Functions auf dem [Durable Task Framework](https://github.com/Azure/durabletask) auf, einer Open Source-Bibliothek auf GitHub zum Erstellen von permanenten Aufgabenorchestrierungen. So wie Azure Functions die serverlose Weiterentwicklung von Azure WebJobs ist, so ist Durable Functions die serverlose Weiterentwicklung von Durable Task Framework. Durable Task Framework wird sowohl bei Microsoft als auch außerhalb intensiv zum Automatisieren unternehmenswichtiger Prozesse verwendet. Es ist wie geschaffen für die serverlose Azure Functions-Umgebung.

### <a name="event-sourcing-checkpointing-and-replay"></a>Ereignisherkunftsermittlung, Prüfpunkterstellung und Wiedergabe

Orchestratorfunktionen verwalten ihren Ausführungsstatus zuverlässig mithilfe eines als [Ereignisherkunftsermittlung](https://docs.microsoft.com/azure/architecture/patterns/event-sourcing) bezeichneten Cloudentwurfsmusters. Anstatt den *aktuellen* Status einer Orchestrierung direkt zu speichern, verwendet die Erweiterung Durable Functions einen ausschließlich zum Anfügen bestimmten Speicher, um die *vollständige Reihe der Aktionen* aufzuzeichnen, die von der Funktionsorchestrierung durchgeführt werden. Dies hat im Vergleich zum Sichern des vollständigen Laufzeitstatus zahlreiche Vorteile wie Verbesserung der Leistung, Skalierbarkeit und Reaktionsfähigkeit. Weitere Vorteile sind die endgültige Konsistenz von Transaktionsdaten und das Verwalten vollständiger Überwachungspfade und des Verlaufs. Die Überwachungspfade selbst aktivieren zuverlässige kompensierende Aktionen.

Die Verwendung der Ereignisherkunftsermittlung durch diese Erweiterung ist transparent. Im Hintergrund gibt der `await`-Operator in einer Orchestratorfunktion die Steuerung des Orchestratorthreads an den Durable Task Framework-Verteiler zurück. Der Verteiler committet dann alle neuen Aktionen, die die Orchestratorfunktion geplant hat (z.B. Aufrufen mindestens einer untergeordneten Funktion oder Planen eines permanenten Timers) in den Speicher. Dieser transparente Commitvorgang wird dem *Ausführungsverlauf* der Orchestrierungsinstanz angefügt. Der Verlauf wird im permanenten Speicher gespeichert. Dann fügt die Commitaktion Nachrichten an eine Warteschlange an, um die eigentliche Arbeit zu planen. An diesem Punkt kann die Orchestratorfunktion aus dem Arbeitsspeicher entladen werden. Die Abrechnung für sie wird beendet, wenn Sie den Verbrauchstarif für Azure Functions verwenden.  Wenn weitere Aufgaben bewältigt werden müssen, wird die Funktion neu gestartet und ihr Status wiederhergestellt.

Sobald eine Orchestrierungsfunktion weitere Aufgaben ausführen muss (z.B. wird eine Antwortnachricht empfangen, oder ein permanenter Timer läuft ab), wird der Orchestrator reaktiviert und führt erneut die gesamte Funktion von Beginn an neu aus, um den lokalen Status wiederherzustellen. Wenn der Code während dieser Wiedergabe versucht, eine Funktion aufzurufen (oder eine andere asynchrone Aktion auszuführen), zieht Durable Task Framework den *Ausführungsverlauf* der aktuellen Orchestrierung zu Rate. Wenn festgestellt wird, dass die Aktivitätsfunktion bereits ausgeführt wurde und ein Ergebnis erbracht hat, wird dieses Funktionsergebnis wiedergegeben und der Orchestratorcode weiter ausgeführt. Dieser Vorgang wird fortgesetzt, bis der Funktionscode an einen Punkt gelangt, an dem er entweder abgeschlossen oder neue asynchrone Arbeit geplant ist.

### <a name="orchestrator-code-constraints"></a>Einschränkungen des Orchestratorcodes

Mit dem Wiedergabeverhalten sind Einschränkungen des Codetyps verbunden, der in einer Orchestratorfunktion geschrieben werden kann. Orchestratorcode muss z.B. deterministisch sein, da er viele Male wiedergegeben wird und immer dasselbe Ergebnis produzieren muss. Die vollständige Liste der Einschränkungen finden Sie im Abschnitt [Orchestrator code constraints](durable-functions-checkpointing-and-replay.md#orchestrator-code-constraints) (Einschränkungen des Orchestratorcodes) des Artikels **Checkpoints and replay in Durable Functions (Azure Functions)** (Prüfpunkte und Wiedergabe in Durable Functions [Azure Functions]).

## <a name="language-support"></a>Sprachunterstützung

Derzeit ist C# die einzige unterstützte Sprache für Durable Functions. Dies schließt Orchestratorfunktionen und Aktivitätsfunktionen ein. In der Zukunft werden wir Unterstützung für alle Sprachen hinzufügen, die Azure Functions unterstützt. Die [Problemliste im GitHub-Repository](https://github.com/Azure/azure-functions-durable-extension/issues) zu Azure Functions informiert Sie über den neuesten Stand unserer Arbeit für die Unterstützung zusätzlicher Sprachen.

## <a name="monitoring-and-diagnostics"></a>Überwachung und Diagnose

Die Erweiterung Durable Functions gibt automatisch strukturierte Nachverfolgungsdaten an [Application Insights](functions-monitoring.md) aus, wenn die Funktionen-App mit einem Application Insights-Schlüssel konfiguriert ist. Diese Nachverfolgungsdaten können verwendet werden, um das Verhalten und den Verlauf Ihrer Orchestrierungen zu überwachen.

Hier ist ein Beispiel für die Anzeige der Durable Functions-Nachverfolgungsereignisse im Application Insights-Portal mit [Application Insights – Analyse](https://docs.microsoft.com/azure/application-insights/app-insights-analytics):

![Application Insights-Abfrageergebnisse](media/durable-functions-overview/app-insights-1.png)

Das Feld `customDimensions` jedes Protokolleintrags enthält viele nützliche strukturierte Daten. Hier sehen Sie ein Beispiel eines vollständig erweiterten solchen Eintrags.

![customDimensions-Feld in Application Insights-Abfrage](media/durable-functions-overview/app-insights-2.png)

Aufgrund des Wiedergabeverhaltens des Durable Task Framework-Verteilers können Sie davon ausgehen, redundante Protokolleinträge für wiedergegebene Aktionen zu sehen. Dies kann hilfreich sein, um das Wiedergabeverhalten des Kernmoduls zu verstehen. Der Artikel [Diagnostics in Durable Functions (Azure Functions)](durable-functions-diagnostics.md) (Diagnose in Durable Functions [Azure Functions]) zeigt Beispiele für Abfragen, die Wiedergabeprotokolle ausfiltern, sodass Sie nur die „Echtzeitprotokolle“ sehen.

## <a name="storage-and-scalability"></a>Speicherung und Skalierbarkeit

Die Erweiterung Durable Functions verwendet Azure Storage-Warteschlangen, -Tabellen und -Blobs, um Ausführungsverlaufsstatus und Triggerfunktionsausführung persistent zu speichern. Sie können das Standardspeicherkonto für die Funktionen-App nutzen oder ein separates Speicherkonto konfigurieren. Aufgrund der Speicherdurchsatz-Grenzwerte sollten Sie ggf. ein separates Konto verwenden. Der Orchestratorcode, den Sie schreiben, muss nicht (und sollte nicht) mit den Entitäten in diesen Speicherkonten interagieren. Die Entitäten werden als Implementierungsdetail direkt vom Durable Task Framework verwaltet.

Orchestratorfunktionen planen Aktivitätsfunktionen und empfangen ihre Antworten über interne Warteschlangennachrichten. Wenn eine Funktionen-App im Verbrauchstarif von Azure Functions ausgeführt wird, werden diese Warteschlangen vom [Skalierungscontroller von Azure Functions](functions-scale.md#how-the-consumption-plan-works) überwacht, und bei Bedarf werden neue Compute-Instanzen hinzugefügt. Beim horizontalen Hochskalieren über mehrere virtuelle Computer kann eine Orchestratorfunktion auf einem virtuellen Computer ausgeführt werden, während von Ihr aufgerufene Aktivitätsfunktionen auf mehreren anderen virtuellen Computern ausgeführt werden. Weitere Informationen über das Skalierungsverhalten von Durable Functions finden Sie in [Performance and scale in Durable Functions (Azure Functions)](durable-functions-perf-and-scale.md) (Leistung und Skalierung in Durable Functions [Azure Functions]).

Tabellenspeicher wird verwendet, um den Ausführungsverlauf für Orchestratorkonten zu speichern. Immer dann, wenn eine Instanz auf einem bestimmten virtuellen Computer aktiviert wird, ruft sie ihren Ausführungsverlauf vom Tabellenspeicher ab, damit er ihren lokalen Zustand wiederherstellen kann. Zu den Vorteilen der Verfügbarkeit des Verlaufs im Tabellenspeicher zählt, dass Sie den Verlauf Ihrer Orchestrierungen mit Tools wie z.B. [Microsoft Azure Storage-Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer) einsehen können.

![Screenshot: Azure Storage-Explorer](media/durable-functions-overview/storage-explorer.png)

> [!WARNING]
> Es ist zwar einfach und bequem, den Ausführungsverlauf im Tabellenspeicher angezeigt zu bekommen, aber Sie sollten es vermeiden, Abhängigkeiten von dieser Tabelle einzurichten. Dies kann sich im Rahmen der Weiterentwicklung der Erweiterung Durable Functions ändern.

## <a name="known-issues-and-faq"></a>Bekannte Probleme und FAQ

Im Allgemeinen sollten alle bekannten Probleme in der Liste der [GitHub-Probleme](https://github.com/Azure/azure-functions-durable-extension/issues) nachverfolgt werden. Wenn Sie auf ein Problem stoßen, das in GitHub nicht zu finden ist, öffnen Sie ein neues Problem mit einer ausführlichen Problembeschreibung. Auch wenn Sie einfach nur eine Frage stellen möchten, können Sie ein GitHub-Problem öffnen und es als Frage kennzeichnen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Bindings for Durable Functions (Azure Functions)](durable-functions-bindings.md) (Bindungen für Durable Functions [Azure Functions])

> [!div class="nextstepaction"]
> [Install the Durable Functions extension and samples (Azure Functions)](durable-functions-install.md) (Installieren der Erweiterung Durable Functions und Beispiele [Azure Functions])

