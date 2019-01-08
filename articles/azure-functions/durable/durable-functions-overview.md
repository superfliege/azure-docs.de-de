---
title: Übersicht zu Durable Functions – Azure
description: Einführung in die Durable Functions-Erweiterung für Azure Functions.
services: functions
author: kashimiz
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 12/7/2018
ms.author: azfuncdf
ms.openlocfilehash: cf643c8c2a0921cfbe1f5599a9e96a27770ef1ef
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/21/2018
ms.locfileid: "53718587"
---
# <a name="durable-functions-overview"></a>Übersicht zu Durable Functions

*Durable Functions* ist eine Erweiterung von [Azure Functions](../functions-overview.md) und [Azure WebJobs](../../app-service/webjobs-create.md), mit der Sie zustandsbehaftete Funktionen in einer serverlosen Umgebung schreiben können. Die Erweiterung verwaltet Status, Prüfpunkte und Neustarts für Sie.

Mit der Erweiterung können Sie zustandsbehaftete Workflows in einer neuen, als [*Orchestratorfunktion*](durable-functions-types-features-overview.md#orchestrator-functions) bezeichneten Art von Funktion definieren. Einige Vorteile der Orchestratorfunktionen sind:

* Sie definieren Workflows im Code. Keine JSON-Schemas oder Designer sind erforderlich.
* Sie können andere Funktionen synchron und asynchron aufrufen. Ausgabe von aufgerufenen Funktionen kann in lokalen Variablen gespeichert werden.
* Sie erstellen immer dann, wenn die Funktion wartet, automatisch Prüfpunkte zu ihrem Status. Der lokale Zustand geht nie verloren, wenn der Prozess oder virtuelle Computer neu gestartet wird.

> [!NOTE]
> Durable Functions ist eine fortgeschrittene Erweiterung für Azure Functions und nicht für alle Anwendungen geeignet. Im weiteren Verlauf dieses Artikels wird vorausgesetzt, dass Sie mit den Konzepten von [Azure Functions](../functions-overview.md) und den Herausforderungen serverloser Anwendungsentwicklung bestens vertraut sind.

Der primäre Anwendungsfall für Durable Functions ist die Vereinfachung komplexer, zustandsbehafteter Koordinationsprobleme in serverlosen Anwendungen. In den folgenden Abschnitten werden einige typische Anwendungsmuster beschrieben, die von Durable Functions profitieren können.

## <a name="pattern-1-function-chaining"></a>Muster 1: Funktionsverkettung

Bei der *Funktionsverkettung* geht es um das Muster für die Ausführung einer Funktionssequenz in einer bestimmten Reihenfolge. Häufig muss die Ausgabe einer Funktion auf die Eingabe einer anderen Funktion angewendet werden.

![Funktionsverkettungsdiagramm](./media/durable-functions-overview/function-chaining.png)

Mit Durable Functions können Sie dieses Muster präzise im Code implementieren.

### <a name="c-script"></a>C#-Skript

```cs
public static async Task<object> Run(DurableOrchestrationContext context)
{
    try
    {
        var x = await context.CallActivityAsync<object>("F1");
        var y = await context.CallActivityAsync<object>("F2", x);
        var z = await context.CallActivityAsync<object>("F3", y);
        return  await context.CallActivityAsync<object>("F4", z);
    }
    catch (Exception)
    {
        // error handling/compensation goes here
    }
}
```

> [!NOTE]
> Beim Schreiben einer vorkompilierten permanenten Funktion in C# gibt es im Vergleich zu dem zuvor vorgestellten C#-Skriptbeispiel geringfügige Unterschiede. Eine vorkompilierte C#-Funktion würde permanente Parameter erfordern, damit sie mit den jeweiligen Attributen versehen werden kann. Ein Beispiel hierfür ist das Attribut `[OrchestrationTrigger]` für den Parameter `DurableOrchestrationContext`. Wenn die Parameter nicht ordnungsgemäß ergänzt werden, wäre die Laufzeit nicht in der Lage, die Variablen in die Funktion einzufügen und würde Fehler ausgeben. Weitere Beispiele finden Sie unter [Beispiel](https://github.com/Azure/azure-functions-durable-extension/blob/master/samples).

### <a name="javascript-functions-2x-only"></a>JavaScript (nur Functions 2.x)

```js
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const x = yield context.df.callActivity("F1");
    const y = yield context.df.callActivity("F2", x);
    const z = yield context.df.callActivity("F3", y);
    return yield context.df.callActivity("F4", z);
});
```

Die Werte „F1“, „F2“, „F3“ und „F4“ sind die Namen von weiteren Funktionen in der Funktionen-App. Die Ablaufsteuerung wird mithilfe normaler imperativer Codierungskonstrukte implementiert. Dies bedeutet, dass der Code von oben nach unten ausgeführt wird und bestehende sprachliche Ablaufsteuerungssemantik wie Bedingungsanweisungen und Schleifen umfassen kann.  Fehlerbehandlungscode kann in Try/Catch/Finally-Blöcken enthalten sein.

Der `context`-Parameter ([DurableOrchestrationContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html)) (.NET) und das `context.df`-Objekt (JavaScript) stellen Methoden zum Aufrufen anderer Funktionen anhand des Namens, zum Übergeben von Parametern und Zurückgeben der Funktionsausgabe bereit. Jedes Mal, wenn der Code `await` (C#) oder `yield` (JavaScript) aufruft, erstellt das Durable Functions Framework *Prüfpunkte* zum Status der aktuellen Funktionsinstanz. Wenn der Prozess oder die VM mitten in der Ausführung neu gestartet wird, wird die Funktionsinstanz ab dem vorherigen `await`- bzw. `yield`-Aufruf fortgesetzt. Später wird dieses Neustartverhalten näher erläutert.

> [!NOTE]
> Das `context`-Objekt in JavaScript stellt den [Funktionskontext als Ganzes] dar, nicht den DurableOrchestrationContext.(../functions-reference-node.md#context-object).

## <a name="pattern-2-fan-outfan-in"></a>Muster 2: Auffächern nach außen/innen

*Auffächern nach außen/innen* bezieht sich auf das Muster der gleichzeitigen Ausführung mehrerer Funktionen und anschließendes Warten darauf, dass alle beendet werden.  Häufig werden von Funktionen zurückgegebene Ergebnisse einer Aggregation unterzogen.

![Diagramm zum Auffächern nach außen/innen](./media/durable-functions-overview/fan-out-fan-in.png)

Bei normalen Funktionen kann die Funktion zum Auffächern nach außen mehrere Nachrichten an eine Warteschlange senden. Das Auffächern zurück nach innen ist jedoch wesentlich schwieriger. Sie müssen Code schreiben, um nachzuverfolgen, wann die von der Warteschlange ausgelösten Funktionen enden und Funktionsausgaben speichern. Die Erweiterung Durable Functions verarbeitet dieses Muster mit relativ einfachem Code.

### <a name="c-script"></a>C#-Skript

```cs
public static async Task Run(DurableOrchestrationContext context)
{
    var parallelTasks = new List<Task<int>>();

    // get a list of N work items to process in parallel
    object[] workBatch = await context.CallActivityAsync<object[]>("F1");
    for (int i = 0; i < workBatch.Length; i++)
    {
        Task<int> task = context.CallActivityAsync<int>("F2", workBatch[i]);
        parallelTasks.Add(task);
    }

    await Task.WhenAll(parallelTasks);

    // aggregate all N outputs and send result to F3
    int sum = parallelTasks.Sum(t => t.Result);
    await context.CallActivityAsync("F3", sum);
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (nur Functions 2.x)

```js
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const parallelTasks = [];

    // get a list of N work items to process in parallel
    const workBatch = yield context.df.callActivity("F1");
    for (let i = 0; i < workBatch.length; i++) {
        parallelTasks.push(context.df.callActivity("F2", workBatch[i]));
    }

    yield context.df.Task.all(parallelTasks);

    // aggregate all N outputs and send result to F3
    const sum = parallelTasks.reduce((prev, curr) => prev + curr, 0);
    yield context.df.callActivity("F3", sum);
});
```

Das Auffächern nach außen wird auf mehrere Instanzen der Funktion `F2` verteilt, und die Arbeit wird mit einer dynamischen Liste von Aufgaben nachverfolgt. Die `Task.WhenAll`-API von .NET oder `context.df.Task.all`-API von JavaScript wird aufgerufen, um auf die Beendigung aller aufgerufenen Funktionen zu warten. Anschließend werden die Ausgaben der `F2`-Funktion aus der dynamischen Aufgabenliste aggregiert und an die `F3`-Funktion übergeben.

Die automatische Prüfpunkterstellung, die beim `await`- oder `yield`-Aufruf für `Task.WhenAll` bzw. `context.df.Task.all` erfolgt, stellt sicher, dass jeglicher Absturz oder Neustart während der Ausführung keinen Neustart bereits abgeschlossener Aufgaben erfordert.

## <a name="pattern-3-async-http-apis"></a>Muster 3: Asynchrone HTTP-APIs

Beim dritten Muster geht es um das Problem der Koordination des Status lange ausgeführter Vorgänge mit externen Clients. Eine allgemeine Möglichkeit, dieses Muster zu implementieren, besteht darin, die lange ausgeführte Aktion durch einen HTTP-Aufruf auszulösen und dann den Client zu einem Statusendpunkt umzuleiten, den er abfragen kann, um zu erfahren, wann der Vorgang abgeschlossen ist.

![HTTP-API-Diagramm](./media/durable-functions-overview/async-http-api.png)

Durable Functions bietet integrierte APIs, die den Code vereinfachen, den Sie für die Interaktion mit langen Funktionsausführungen schreiben. Die Schnellstartbeispiele ([C#](durable-functions-create-first-csharp.md), [JavaScript](quickstart-js-vscode.md)) zeigen einen einfachen REST-Befehl, der zum Starten neuer Orchestratorfunktionsinstanzen verwendet werden kann. Sobald eine Instanz gestartet wird, macht die Erweiterung Webhook-HTTP-APIs verfügbar, die den Status der Orchestratorfunktion abfragen. Das folgende Beispiel zeigt die REST-Befehle zum Starten eines Orchestrators und zum Abfragen seines Status. Zur besseren Übersichtlichkeit werden im Beispiel einige Details weggelassen.

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

### <a name="c"></a>C#

```cs
// HTTP-triggered function to start a new orchestrator function instance.
public static async Task<HttpResponseMessage> Run(
    HttpRequestMessage req,
    DurableOrchestrationClient starter,
    string functionName,
    ILogger log)
{
    // Function name comes from the request URL.
    // Function input comes from the request content.
    dynamic eventData = await req.Content.ReadAsAsync<object>();
    string instanceId = await starter.StartNewAsync(functionName, eventData);

    log.LogInformation($"Started orchestration with ID = '{instanceId}'.");

    return starter.CreateCheckStatusResponse(req, instanceId);
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (nur Functions 2.x)

```javascript
// HTTP-triggered function to start a new orchestrator function instance.
const df = require("durable-functions");

module.exports = async function (context, req) {
    const client = df.getClient(context);

    // Function name comes from the request URL.
    // Function input comes from the request content.
    const eventData = req.body;
    const instanceId = await client.startNew(req.params.functionName, undefined, eventData);

    context.log(`Started orchestration with ID = '${instanceId}'.`);

    return client.createCheckStatusResponse(req, instanceId);
};
```

> [!WARNING]
> Wenn Sie lokal in JavaScript entwickeln, müssen Sie die Umgebungsvariable `WEBSITE_HOSTNAME` auf `localhost:<port>` festlegen, z. B. `localhost:7071`, um Methoden mit `DurableOrchestrationClient` zu verwenden. Weitere Informationen zu dieser Anforderung finden Sie in der [Beschreibung des Problems auf GitHub](https://github.com/Azure/azure-functions-durable-js/issues/28).

In .NET ist der `starter`-Parameter von [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) ein Wert aus der `orchestrationClient`-Ausgabebindung, die Teil der Durable Functions-Erweiterung ist. In JavaScript wird dieses Objekt durch Aufrufen von `df.getClient(context)` zurückgegeben. Diese Objekte stellen Methoden bereit, mit denen neue oder vorhandene Orchestratorfunktionsinstanzen gestartet, beendet und abgefragt sowie Ereignisse an sie gesendet werden können.

Im vorherigen Beispiel nimmt eine über HTTP ausgelöste Funktion einen `functionName`-Wert von der eingehenden URL entgegen und übergibt den Wert an [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_). Die Bindungs-API [CreateCheckStatusResponse](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateCheckStatusResponse_System_Net_Http_HttpRequestMessage_System_String_) gibt dann eine Antwort zurück, die einen `Location`-Header und zusätzliche Informationen zur Instanz enthält, die später verwendet werden können, um den Status der gestarteten Instanz abzurufen oder sie zu beenden.

## <a name="pattern-4-monitoring"></a>Muster 4: Überwachung

Das Monitormuster bezieht sich auf einen flexiblen *wiederkehrenden* Prozess in einem Workflow – wenn beispielsweise bestimmte Elemente solange abgerufen werden, bis bestimmte Bedingungen erfüllt sind. Ein normaler [Timertrigger](../functions-bindings-timer.md) kann für ein einfaches Szenario verwendet werden, beispielsweise einen periodischen Bereinigungsauftrag. Sein Intervall ist jedoch statisch, und die Verwaltung der Instanzlebensdauer wird komplex. Durable Functions ermöglicht flexible Wiederholungsintervalle, die Verwaltung der Aufgabenlebensdauer und die Erstellung mehrerer Monitorprozesse aus einer einzigen Orchestrierung.

Ein Beispiel wäre das Umkehren des früheren asynchronen HTTP-API-Szenarios. Anstatt einen Endpunkt für einen externen Client freizugeben, um einen lang laufenden Vorgang zu überwachen, belegt der lang laufende Monitor einen Eingabeendpunkt und wartet auf einen Zustandswechsel.

![Monitordiagramm](./media/durable-functions-overview/monitor.png)

Mithilfe von Durable Functions können mehrere Monitore, die beliebige Endpunkte beobachten, in ein paar Codezeilen erstellt werden. Die Monitore können die Ausführung beenden, wenn eine Bedingung erfüllt ist, oder durch den [DurableOrchestrationClient](durable-functions-instance-management.md) beendet werden. Zudem kann ihr Warteintervall basierend auf einer Bedingung (z.B. exponentielles Backoff) geändert werden. Der folgende Code implementiert einen grundlegenden Monitor.

### <a name="c-script"></a>C#-Skript

```cs
public static async Task Run(DurableOrchestrationContext context)
{
    int jobId = context.GetInput<int>();
    int pollingInterval = GetPollingInterval();
    DateTime expiryTime = GetExpiryTime();

    while (context.CurrentUtcDateTime < expiryTime)
    {
        var jobStatus = await context.CallActivityAsync<string>("GetJobStatus", jobId);
        if (jobStatus == "Completed")
        {
            // Perform action when condition met
            await context.CallActivityAsync("SendAlert", machineId);
            break;
        }

        // Orchestration will sleep until this time
        var nextCheck = context.CurrentUtcDateTime.AddSeconds(pollingInterval);
        await context.CreateTimer(nextCheck, CancellationToken.None);
    }

    // Perform further work here, or let the orchestration end
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (nur Functions 2.x)

```js
const df = require("durable-functions");
const moment = require("moment");

module.exports = df.orchestrator(function*(context) {
    const jobId = context.df.getInput();
    const pollingInternal = getPollingInterval();
    const expiryTime = getExpiryTime();

    while (moment.utc(context.df.currentUtcDateTime).isBefore(expiryTime)) {
        const jobStatus = yield context.df.callActivity("GetJobStatus", jobId);
        if (jobStatus === "Completed") {
            // Perform action when condition met
            yield context.df.callActivity("SendAlert", machineId);
            break;
        }

        // Orchestration will sleep until this time
        const nextCheck = moment.utc(context.df.currentUtcDateTime).add(pollingInterval, 's');
        yield context.df.createTimer(nextCheck.toDate());
    }

    // Perform further work here, or let the orchestration end
});
```

Wenn eine Anforderung empfangen wird, wird eine neue Orchestrierungsinstanz für diese Auftrags-ID erstellt. Die Instanz fragt den Status ab, bis eine Bedingung erfüllt ist und die Schleife beendet wird. Ein permanenter Timer wird verwendet, um das Abrufintervall zu steuern. Nun können weitere Arbeitsschritte ausgeführt werden, oder die Orchestrierung wird beendet. Wenn die `context.CurrentUtcDateTime` (.NET) oder `context.df.currentUtcDateTime` (JavaScript) die `expiryTime` überschreitet, wird der Monitor beendet.

## <a name="pattern-5-human-interaction"></a>Muster 5: Benutzerinteraktion

Viele Prozesse enthalten eine Form der Benutzerinteraktion. Das Problem bei der Einbeziehung von Menschen in einen automatisierten Prozess ist, dass Personen nicht immer in gleichem Maße verfügbar und reaktionsfähig sind wie Clouddienste. Automatisierte Prozesse müssen dies tolerieren, und hierzu nutzen sie häufig Timeouts und Kompensationscode.

Ein Beispiel eines Geschäftsprozesses, der Benutzerinteraktion umfasst, ist ein Genehmigungsprozess. So ist z.B. für einen Ausgabenbericht, der einen bestimmten Betrag überschreitet, die Genehmigung eines Managers erforderlich. Wenn der Manager die Genehmigung nicht innerhalb von 72 Stunden erteilt (weil er vielleicht im Urlaub ist), wird ein Eskalationsverfahren wirksam, um die Genehmigung von einer anderen Person (z.B. dem Manager des Managers) zu erhalten.

![Diagramm zur Benutzerinteraktion](./media/durable-functions-overview/approval.png)

Dieses Muster kann mithilfe einer Orchestratorfunktion implementiert werden. Der Orchestrator würde einen [permanenten Timer](durable-functions-timers.md) verwenden, um die Genehmigung anzufordern und im Falle eines Timeouts zu eskalieren. Er würde auf ein [externes Ereignis](durable-functions-external-events.md) warten – die durch Benutzerinteraktion generierte Benachrichtigung.

### <a name="c-script"></a>C#-Skript

```cs
public static async Task Run(DurableOrchestrationContext context)
{
    await context.CallActivityAsync("RequestApproval");
    using (var timeoutCts = new CancellationTokenSource())
    {
        DateTime dueTime = context.CurrentUtcDateTime.AddHours(72);
        Task durableTimeout = context.CreateTimer(dueTime, timeoutCts.Token);

        Task<bool> approvalEvent = context.WaitForExternalEvent<bool>("ApprovalEvent");
        if (approvalEvent == await Task.WhenAny(approvalEvent, durableTimeout))
        {
            timeoutCts.Cancel();
            await context.CallActivityAsync("ProcessApproval", approvalEvent.Result);
        }
        else
        {
            await context.CallActivityAsync("Escalate");
        }
    }
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (nur Functions 2.x)

```js
const df = require("durable-functions");
const moment = require('moment');

module.exports = df.orchestrator(function*(context) {
    yield context.df.callActivity("RequestApproval");

    const dueTime = moment.utc(context.df.currentUtcDateTime).add(72, 'h');
    const durableTimeout = context.df.createTimer(dueTime.toDate());

    const approvalEvent = context.df.waitForExternalEvent("ApprovalEvent");
    if (approvalEvent === yield context.df.Task.any([approvalEvent, durableTimeout])) {
        durableTimeout.cancel();
        yield context.df.callActivity("ProcessApproval", approvalEvent.result);
    } else {
        yield context.df.callActivity("Escalate");
    }
});
```

Der permanente Timer wird durch Aufrufen von `context.CreateTimer` (.NET) oder `context.df.createTimer`(JavaScript) erstellt. Die Benachrichtigung wird von `context.WaitForExternalEvent` (.NET) oder `context.df.waitForExternalEvent` (JavaScript) empfangen. Außerdem wird `Task.WhenAny` (.NET) oder `context.df.Task.any` (JavaScript) aufgerufen, um zu entscheiden, ob eine Eskalation erfolgt (das Timeout tritt zuerst auf) oder die Genehmigung verarbeitet wird (die Genehmigung wird vor dem Timeout empfangen).

Ein externer Client kann die Ereignisbenachrichtigung entweder über die [integrierten HTTP-APIs](durable-functions-http-api.md#raise-event) oder die [DurableOrchestrationClient.RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_System_String_System_String_System_Object_)-API einer anderen Funktion an eine wartende Orchestratorfunktion senden:

```csharp
public static async Task Run(string instanceId, DurableOrchestrationClient client)
{
    bool isApproved = true;
    await client.RaiseEventAsync(instanceId, "ApprovalEvent", isApproved);
}
```

```javascript
const df = require("durable-functions");

module.exports = async function (context) {
    const client = df.getClient(context);
    const isApproved = true;
    await client.raiseEvent(instanceId, "ApprovalEvent", isApproved);
};
```

## <a name="the-technology"></a>Die Technologie

Im Hintergrund baut die Erweiterung Durable Functions auf dem [Durable Task Framework](https://github.com/Azure/durabletask) auf, einer Open Source-Bibliothek auf GitHub zum Erstellen von permanenten Aufgabenorchestrierungen. So wie Azure Functions die serverlose Weiterentwicklung von Azure WebJobs ist, so ist Durable Functions die serverlose Weiterentwicklung von Durable Task Framework. Durable Task Framework wird sowohl bei Microsoft als auch außerhalb intensiv zum Automatisieren unternehmenswichtiger Prozesse verwendet. Es ist wie geschaffen für die serverlose Azure Functions-Umgebung.

### <a name="event-sourcing-checkpointing-and-replay"></a>Ereignisherkunftsermittlung, Prüfpunkterstellung und Wiedergabe

Orchestratorfunktionen verwalten ihren Ausführungsstatus zuverlässig mithilfe eines als [Ereignisherkunftsermittlung](https://docs.microsoft.com/azure/architecture/patterns/event-sourcing) bezeichneten Entwurfsmusters. Anstatt den *aktuellen* Status einer Orchestrierung direkt zu speichern, verwendet die Erweiterung Durable Functions einen ausschließlich zum Anfügen bestimmten Speicher, um die *vollständige Reihe der Aktionen* aufzuzeichnen, die von der Funktionsorchestrierung durchgeführt werden. Dies hat im Vergleich zum Sichern des vollständigen Laufzeitstatus zahlreiche Vorteile wie Verbesserung der Leistung, Skalierbarkeit und Reaktionsfähigkeit. Weitere Vorteile sind die endgültige Konsistenz von Transaktionsdaten und das Verwalten vollständiger Überwachungspfade und des Verlaufs. Die Überwachungspfade selbst aktivieren zuverlässige kompensierende Aktionen.

Die Verwendung der Ereignisherkunftsermittlung durch diese Erweiterung ist transparent. Im Hintergrund gibt der `await`-Operator (C#) oder `yield`-Operator (JavaScript) in einer Orchestratorfunktion die Steuerung des Orchestratorthreads an den Durable Task Framework-Verteiler zurück. Der Verteiler committet dann alle neuen Aktionen, die die Orchestratorfunktion geplant hat (z.B. Aufrufen mindestens einer untergeordneten Funktion oder Planen eines permanenten Timers) in den Speicher. Dieser transparente Commitvorgang wird dem *Ausführungsverlauf* der Orchestrierungsinstanz angefügt. Der Verlauf wird in einer Speichertabelle gespeichert. Dann fügt die Commitaktion Nachrichten an eine Warteschlange an, um die eigentliche Arbeit zu planen. An diesem Punkt kann die Orchestratorfunktion aus dem Arbeitsspeicher entladen werden. Die Abrechnung für sie wird beendet, wenn Sie den Verbrauchstarif für Azure Functions verwenden.  Wenn weitere Aufgaben bewältigt werden müssen, wird die Funktion neu gestartet und ihr Status wiederhergestellt.

Sobald eine Orchestrierungsfunktion weitere Aufgaben ausführen muss (z.B. wird eine Antwortnachricht empfangen, oder ein permanenter Timer läuft ab), wird der Orchestrator reaktiviert und führt erneut die gesamte Funktion von Beginn an neu aus, um den lokalen Status wiederherzustellen. Wenn der Code während dieser Wiedergabe versucht, eine Funktion aufzurufen (oder eine andere asynchrone Aktion auszuführen), zieht Durable Task Framework den *Ausführungsverlauf* der aktuellen Orchestrierung zu Rate. Wenn festgestellt wird, dass die [Aktivitätsfunktion](durable-functions-types-features-overview.md#activity-functions) bereits ausgeführt wurde und ein Ergebnis erbracht hat, wird dieses Funktionsergebnis wiedergegeben und der Orchestratorcode weiter ausgeführt. Dieser Vorgang wird fortgesetzt, bis der Funktionscode an einen Punkt gelangt, an dem er entweder abgeschlossen oder neue asynchrone Arbeit geplant ist.

### <a name="orchestrator-code-constraints"></a>Einschränkungen des Orchestratorcodes

Mit dem Wiedergabeverhalten sind Einschränkungen des Codetyps verbunden, der in einer Orchestratorfunktion geschrieben werden kann. Orchestratorcode muss z.B. deterministisch sein, da er viele Male wiedergegeben wird und immer dasselbe Ergebnis produzieren muss. Die vollständige Liste der Einschränkungen finden Sie im Abschnitt [Orchestrator code constraints](durable-functions-checkpointing-and-replay.md#orchestrator-code-constraints) (Einschränkungen des Orchestratorcodes) des Artikels **Checkpoints and replay in Durable Functions (Azure Functions)** (Prüfpunkte und Wiedergabe in Durable Functions [Azure Functions]).

## <a name="language-support"></a>Sprachunterstützung

Derzeit werden für Durable Functions nur C# (Functions 1.x und 2.x), F# und JavaScript (nur Functions 2.x, Durable Functions 1.7.0 oder höher) unterstützt. Dies schließt Orchestratorfunktionen und Aktivitätsfunktionen ein. In der Zukunft werden wir Unterstützung für alle Sprachen hinzufügen, die Azure Functions unterstützt. Die [Problemliste im GitHub-Repository](https://github.com/Azure/azure-functions-durable-extension/issues) zu Azure Functions informiert Sie über den neuesten Stand unserer Arbeit für die Unterstützung zusätzlicher Sprachen.

## <a name="monitoring-and-diagnostics"></a>Überwachung und Diagnose

Die Erweiterung Durable Functions gibt automatisch strukturierte Nachverfolgungsdaten an [Application Insights](../functions-monitoring.md) aus, wenn die Funktions-App mit einem Application Insights-Instrumentierungsschlüssel konfiguriert ist. Diese Nachverfolgungsdaten können verwendet werden, um das Verhalten und den Verlauf Ihrer Orchestrierungen zu überwachen.

Hier ist ein Beispiel für die Anzeige der Durable Functions-Nachverfolgungsereignisse im Application Insights-Portal mit [Application Insights – Analyse](https://docs.microsoft.com/azure/application-insights/app-insights-analytics):

![Application Insights-Abfrageergebnisse](./media/durable-functions-overview/app-insights-1.png)

Das Feld `customDimensions` jedes Protokolleintrags enthält viele nützliche strukturierte Daten. Hier sehen Sie ein Beispiel eines vollständig erweiterten solchen Eintrags.

![customDimensions-Feld in Application Insights-Abfrage](./media/durable-functions-overview/app-insights-2.png)

Aufgrund des Wiedergabeverhaltens des Durable Task Framework-Verteilers können Sie davon ausgehen, redundante Protokolleinträge für wiedergegebene Aktionen zu sehen. Dies kann hilfreich sein, um das Wiedergabeverhalten der Kern-Engine zu verstehen. Der Artikel [Diagnostics in Durable Functions (Azure Functions)](durable-functions-diagnostics.md) (Diagnose in Durable Functions [Azure Functions]) zeigt Beispiele für Abfragen, die Wiedergabeprotokolle ausfiltern, sodass Sie nur die „Echtzeitprotokolle“ sehen.

## <a name="storage-and-scalability"></a>Speicherung und Skalierbarkeit

Die Erweiterung Durable Functions verwendet Azure Storage-Warteschlangen, -Tabellen und -Blobs, um Ausführungsverlaufsstatus und Triggerfunktionsausführung persistent zu speichern. Sie können das Standardspeicherkonto für die Funktionen-App nutzen oder ein separates Speicherkonto konfigurieren. Aufgrund der Speicherdurchsatz-Grenzwerte sollten Sie ggf. ein separates Konto verwenden. Der Orchestratorcode, den Sie schreiben, muss nicht (und sollte nicht) mit den Entitäten in diesen Speicherkonten interagieren. Die Entitäten werden als Implementierungsdetail direkt vom Durable Task Framework verwaltet.

Orchestratorfunktionen planen Aktivitätsfunktionen und empfangen ihre Antworten über interne Warteschlangennachrichten. Wenn eine Funktionen-App im Verbrauchstarif von Azure Functions ausgeführt wird, werden diese Warteschlangen vom [Skalierungscontroller von Azure Functions](../functions-scale.md#how-the-consumption-plan-works) überwacht, und bei Bedarf werden neue Compute-Instanzen hinzugefügt. Beim horizontalen Hochskalieren über mehrere virtuelle Computer kann eine Orchestratorfunktion auf einem virtuellen Computer ausgeführt werden, während von Ihr aufgerufene Aktivitätsfunktionen auf mehreren anderen virtuellen Computern ausgeführt werden. Weitere Informationen über das Skalierungsverhalten von Durable Functions finden Sie in [Performance and scale in Durable Functions (Azure Functions)](durable-functions-perf-and-scale.md) (Leistung und Skalierung in Durable Functions [Azure Functions]).

Tabellenspeicher wird verwendet, um den Ausführungsverlauf für Orchestratorkonten zu speichern. Immer dann, wenn eine Instanz auf einem bestimmten virtuellen Computer aktiviert wird, ruft sie ihren Ausführungsverlauf vom Tabellenspeicher ab, damit er ihren lokalen Zustand wiederherstellen kann. Zu den Vorteilen der Verfügbarkeit des Verlaufs im Tabellenspeicher zählt, dass Sie den Verlauf Ihrer Orchestrierungen mit Tools wie [Microsoft Azure Storage-Explorer](../../vs-azure-tools-storage-manage-with-storage-explorer.md) einsehen können.

Speicher-Blobs werden hauptsächlich als Leasingmechanismus zum Koordinieren der übergreifenden horizontalen Skalierung von Orchestrierungsinstanzen über mehrere VMs hinweg verwendet. Sie werden ferner zum Speicher von Daten für umfangreiche Nachrichten verwendet, die nicht direkt in Tabelle oder Warteschlangen gespeichert werden können.

![Screenshot: Azure Storage-Explorer](./media/durable-functions-overview/storage-explorer.png)

> [!WARNING]
> Es ist zwar einfach und bequem, den Ausführungsverlauf im Tabellenspeicher angezeigt zu bekommen, aber Sie sollten es vermeiden, Abhängigkeiten von dieser Tabelle einzurichten. Dies kann sich im Rahmen der Weiterentwicklung der Erweiterung Durable Functions ändern.

## <a name="known-issues-and-faq"></a>Bekannte Probleme und FAQ

Alle bekannten Probleme sollten in der Liste der [GitHub-Probleme](https://github.com/Azure/azure-functions-durable-extension/issues) nachverfolgt werden. Wenn Sie auf ein Problem stoßen, das in GitHub nicht zu finden ist, öffnen Sie ein neues Problem mit einer ausführlichen Problembeschreibung.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Durable Functions finden Sie unter [Übersicht über die Funktionstypen und Features für Durable Functions (Azure Functions)](durable-functions-types-features-overview.md) oder

> [!div class="nextstepaction"]
> [Erstellen Ihrer ersten Durable Function](durable-functions-create-first-csharp.md)