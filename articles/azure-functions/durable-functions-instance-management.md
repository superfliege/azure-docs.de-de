---
title: Verwalten von Instanzen in Durable Functions – Azure
description: Es wird beschrieben, wie Instanzen in der Erweiterung Durable Functions für Azure Functions verwaltet werden.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 08/31/2018
ms.author: azfuncdf
ms.openlocfilehash: bcb87b3030eb673dd3c9a8b93a045c75fae964a9
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50087031"
---
# <a name="manage-instances-in-durable-functions-azure-functions"></a>Verwalten von Instanzen in Durable Functions (Azure Functions)

In [Durable Functions](durable-functions-overview.md) können Orchestrierungsinstanzen gestartet, beendet und abgefragt sowie Benachrichtigungsereignisse gesendet werden. Die gesamte Instanzenverwaltung erfolgt mithilfe der [Orchestrierungsclientbindung](durable-functions-bindings.md). In diesem Artikel wird jeder Instanzenverwaltungsvorgang im Detail erläutert.

## <a name="starting-instances"></a>Starten von Instanzen

Die [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_)-Methode im [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) startet eine neue Instanz einer Orchestratorfunktion. Instanzen dieser Klasse können mithilfe der `orchestrationClient`-Bindung abgerufen werden. Intern stellt diese Methode eine Nachricht in die Steuerwarteschlange, die dann den Start einer Funktion mit dem angegebenen Namen auslöst, der die `orchestrationTrigger`-Bindung verwendet. 

Die Aufgabe ist abgeschlossen, wenn der Orchestrierungsprozess gestartet wird. Der Orchestrierungsprozess sollte innerhalb von 30 Sekunden gestartet werden. Wenn es länger dauert, wird eine `TimeoutException` ausgelöst. 

Die Parameter für [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_) lauten wie folgt:

* **Name**: der Name der zu planenden Orchestratorfunktion.
* **Input**: alle mit JSON serialisierbaren Daten, die der Orchestratorfunktion als Eingabe übergeben werden sollen.
* **InstanceId**: (Optional) die eindeutige ID der Instanz. Wenn nicht angegeben, wird eine zufällige Instanz-ID generiert.

Hier ist ein einfaches Beispiel für C#:

```csharp
[FunctionName("HelloWorldManualStart")]
public static async Task Run(
    [ManualTrigger] string input,
    [OrchestrationClient] DurableOrchestrationClient starter,
    ILogger log)
{
    string instanceId = await starter.StartNewAsync("HelloWorld", input);
    log.LogInformation($"Started orchestration with ID = '{instanceId}'.");
}
```

Für Nicht-.NET-Sprachen kann die Funktionsausgabebindung auch zum Starten neuer Instanzen verwendet werden. In diesem Fall können alle mit JSON serialisierbaren Daten verwendet werden, die die oben genannten drei Parameter als Felder enthalten. Betrachten Sie beispielsweise die folgende JavaScript-Funktion:

```js
module.exports = function (context, input) {
    var id = generateSomeUniqueId();
    context.bindings.starter = [{
        FunctionName: "HelloWorld",
        Input: input,
        InstanceId: id
    }];

    context.done(null);
};
```
Im vorstehenden Code wird davon ausgegangen, dass Sie in der Datei „function.json“ eine ausgehende Bindung mit dem Namen „starter“ und dem Typ „orchestrationClient“ definiert haben. Wenn die Bindung nicht definiert ist, wird die Instanz der permanenten Funktion nicht erstellt.

Für den Aufruf der permanenten Funktion sollte „function.json“ so geändert werden, dass sie eine Bindung für den Orchestrierungsclient enthält, wie unten beschrieben

```js
{
    "bindings": [{
        "name":"starter",
        "type":"orchestrationClient",
        "direction":"out"
    }]
}
```

> [!NOTE]
> Sie sollten einen zufälligen Bezeichner für die Instanz-ID verwenden. Dies gewährleistet eine gleichmäßige Lastenverteilung, wenn Orchestratorfunktionen mehrere virtuelle Computer übergreifend skaliert werden. Wenn die ID aus einer externen Quelle stammen muss oder bei der Implementierung des [Singleton-Orchestrator](durable-functions-singletons.md)-Musters sollten nicht zufällige Instanz-IDs verwendet werden.

## <a name="querying-instances"></a>Abfragen von Instanzen

Die [GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_GetStatusAsync_)-Methode für die [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html)-Klasse fragt den Status einer Orchestrierungsinstanz ab. Hierfür werden `instanceId` (erforderlich), `showHistory` (optional) und `showHistoryOutput` (optional) als Parameter verwendet. Wenn `showHistory` auf `true` festgelegt ist, enthält die Antwort den Ausführungsverlauf. Wenn `showHistoryOutput` auch auf `true` festgelegt ist, enthält der Ausführungsverlauf Aktivitätsausgaben. Die Methode gibt ein Objekt mit den folgenden Eigenschaften zurück:

* **Name**: der Name der Orchestratorfunktion.
* **InstanceId**: die Instanz-ID der Orchestrierung (muss mit der Eingabe `instanceId` identisch sein).
* **CreatedTime**: der Zeitpunkt, zu dem die Ausführung der Orchestratorfunktion gestartet wurde.
* **LastUpdatedTime**: der Zeitpunkt des letzten Prüfpunkts der Orchestrierung.
* **Input**: die Eingabe der Funktion als JSON-Wert.
* **CustomStatus**: Benutzerdefinierter Orchestrierungsstatus in JSON-Format 
* **Output**: die Ausgabe der Funktion als JSON-Wert (wenn die Funktion abgeschlossen wurde). Wenn ein Fehler bei der Orchestratorfunktion auftritt, enthält diese Eigenschaft die Fehlerdetails. Wenn die Orchestratorfunktion beendet wurde, enthält diese Eigenschaft den angegebenen Grund für die Beendigung (sofern vorhanden).
* **RuntimeStatus**: Einer der folgenden Werte:
    * **Pending:** Die Instanz wurde geplant, die Ausführung jedoch noch nicht gestartet.
    * **Running**: Die Instanz wurde gestartet.
    * **Completed**: Die Instanz wurde normal abgeschlossen.
    * **ContinuedAsNew**: Die Instanz hat sich selbst mit einem neuen Verlauf neu gestartet. Dies ist ein vorübergehender Status.
    * **Failed**: Bei der Instanz ist ein Fehler aufgetreten.
    * **Terminated**: Die Instanz wurde plötzlich beendet.
* **History**: Der Ausführungsverlauf der Orchestrierung. Dieses Feld wird nur gefüllt, wenn `showHistory` auf `true` festgelegt ist.
    
Diese Methode gibt `null` zurück, wenn die Instanz nicht vorhanden ist, oder ihre Ausführung noch nicht gestartet wurde.

```csharp
[FunctionName("GetStatus")]
public static async Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    var status = await client.GetStatusAsync(instanceId);
    // do something based on the current status.
}
```
## <a name="querying-all-instances"></a>Abfragen aller Instanzen

Sie können die `GetStatusAsync`-Methode verwenden, um die Status aller Instanzen der Orchestrierung abzufragen. Sie nimmt keine Parameter entgegen, oder Sie können ein `CancellationToken`-Objekt übergeben, falls Sie sie abbrechen möchten. Die Methode gibt Objekte mit denselben Eigenschaften wie die `GetStatusAsync`-Methode mit Parametern zurück, jedoch keinen Verlauf. 

```csharp
[FunctionName("GetAllStatus")]
public static async Task Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post")]HttpRequestMessage req,
    [OrchestrationClient] DurableOrchestrationClient client,
    ILogger log)
{
    IList<DurableOrchestrationStatus> instances = await starter.GetStatusAsync(); // You can pass CancellationToken as a parameter.
    foreach (var instance in instances)
    {
        log.LogInformation(JsonConvert.SerializeObject(instance));
    };
}
```
## <a name="querying-instances-with-filters"></a>Abfragen von Instanzen mit Filtern

Die Methode `GetStatusAsync` kann außerdem verwendet werden, um eine Liste mit Orchestrierungsinstanzen abzurufen, die einem Satz vordefinierter Filter entspricht. Zu den möglichen Filteroptionen gehören der Erstellungszeitpunkt der Orchestrierung und ihr Laufzeitstatus.

```csharp
[FunctionName("QueryStatus")]
public static async Task Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post")]HttpRequestMessage req,
    [OrchestrationClient] DurableOrchestrationClient client,
    ILogger log)
{
    IEnumerable<OrchestrationRuntimeStatus> runtimeStatus = new List<OrchestrationRuntimeStatus> {
        OrchestrationRuntimeStatus.Completed,
        OrchestrationRuntimeStatus.Running
    };
    IList<DurableOrchestrationStatus> instances = await starter.GetStatusAsync(
        new DateTime(2018, 3, 10, 10, 1, 0),
        new DateTime(2018, 3, 10, 10, 23, 59),
        runtimeStatus
    ); // You can pass CancellationToken as a parameter.
    foreach (var instance in instances)
    {
        log.LogInformation(JsonConvert.SerializeObject(instance));
    };
}
```

## <a name="terminating-instances"></a>Beenden von Instanzen

Eine aktive Orchestrierungsinstanz kann mithilfe der [TerminateAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_TerminateAsync_)-Methode der [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html)-Klasse beendet werden. Die beiden Parameter sind eine `instanceId` und eine `reason`-Zeichenfolge, die in Protokolle und den Instanzenstatus geschrieben werden. Die Ausführung einer beendeten Instanz endet, sobald sie den nächsten `await`-Punkt erreicht, oder sofort, wenn sie sich bereits an einem `await`-Punkt befindet. 

```csharp
[FunctionName("TerminateInstance")]
public static Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    string reason = "It was time to be done.";
    return client.TerminateAsync(instanceId, reason);
}
```

> [!NOTE]
> Die Instanzbeendigung wird momentan nicht weitergegeben. Aktivitätsfunktionen und untergeordnete Orchestrierungen werden bis zum Ende ausgeführt, auch wenn die Orchestrierungsinstanz, durch die sie aufgerufen wurden, beendet wurde.

## <a name="sending-events-to-instances"></a>Senden von Ereignissen an Instanzen

Ereignisbenachrichtigungen können an ausgeführte Instanzen gesendet werden, die die [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_)-Methode der [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html)-Klasse verwenden. Instanzen, die auf einen Aufruf zu [WaitForExternalEvent](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_WaitForExternalEvent_) warten, können diese Ereignisse verarbeiten. 

Die Parameter für [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) lauten wie folgt:

* **InstanceId**: die eindeutige ID der Instanz.
* **EventName**: der Name des zu sendenden Ereignisses.
* **EventData**: eine mit JSON serialisierbare Nutzlast zum Senden an die Instanz.

```csharp
[FunctionName("RaiseEvent")]
public static Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    int[] eventData = new int[] { 1, 2, 3 };
    return client.RaiseEventAsync(instanceId, "MyEvent", eventData);
}
```

> [!WARNING]
> Wenn keine Orchestrierungsinstanz mit der angegebenen *Instanz-ID* vorhanden ist, oder die Instanz nicht auf den angegebenen *Ereignisnamen* wartet, wird die Ereignisnachricht verworfen. Weitere Informationen zu diesem Verhalten finden Sie unter [Expected behavior when unexpected event is received?](https://github.com/Azure/azure-functions-durable-extension/issues/29) (Erwartetes Verhalten, wenn ein unerwartetes Ereignis empfangen wird?).

## <a name="wait-for-orchestration-completion"></a>Warten auf den Abschluss der Orchestrierung

Die [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html)-Klasse macht eine [WaitForCompletionOrCreateCheckStatusResponseAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_WaitForCompletionOrCreateCheckStatusResponseAsync_)-API verfügbar, die verwendet werden kann, um synchron die tatsächliche Ausgabe einer Orchestrierungsinstanz zu erhalten. Für die Methode wird ein Standardwert von 10 Sekunden für `timeout` und von 1 Sekunde für `retryInterval` verwendet, wenn nichts anderes festgelegt wird.  

Hier ist ein Beispiel für eine HTTP-Triggerfunktion angegeben, mit dem die Nutzung dieser API veranschaulicht wird:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HttpSyncStart.cs)]

Die Funktion kann mit der folgenden Zeile aufgerufen werden, indem eine Zeitlimit von 2 Sekunden und ein Wiederholungsintervall von 0,5 Sekunden verwendet wird:

```bash
    http POST http://localhost:7071/orchestrators/E1_HelloSequence/wait?timeout=2&retryInterval=0.5
```

Je nach Zeitraum, der zum Abrufen der Antwort aus der Orchestrierungsinstanz erforderlich ist, gibt es zwei Fälle:

1. Die Orchestrierungsinstanzen werden innerhalb des definierten Zeitraums (hier: 2 Sekunden) abgeschlossen, und die Antwort ist die tatsächliche Orchestrierungsinstanzausgabe, die synchron bereitgestellt wird:

    ```http
        HTTP/1.1 200 OK
        Content-Type: application/json; charset=utf-8
        Date: Thu, 14 Dec 2017 06:14:29 GMT
        Server: Microsoft-HTTPAPI/2.0
        Transfer-Encoding: chunked

        [
            "Hello Tokyo!",
            "Hello Seattle!",
            "Hello London!"
        ]
    ```

2. Die Orchestrierungsinstanzen können nicht innerhalb des definierten Zeitraums (hier: 2 Sekunden) abgeschlossen werden, und die Antwort ist die Standardantwort, die unter **Ermittlung der HTTP-API-URL** beschrieben ist:

    ```http
        HTTP/1.1 202 Accepted
        Content-Type: application/json; charset=utf-8
        Date: Thu, 14 Dec 2017 06:13:51 GMT
        Location: http://localhost:7071/admin/extensions/DurableTaskExtension/instances/d3b72dddefce4e758d92f4d411567177?taskHub={taskHub}&connection={connection}&code={systemKey}
        Retry-After: 10
        Server: Microsoft-HTTPAPI/2.0
        Transfer-Encoding: chunked

        {
            "id": "d3b72dddefce4e758d92f4d411567177",
            "sendEventPostUri": "http://localhost:7071/admin/extensions/DurableTaskExtension/instances/d3b72dddefce4e758d92f4d411567177/raiseEvent/{eventName}?taskHub={taskHub}&connection={connection}&code={systemKey}",
            "statusQueryGetUri": "http://localhost:7071/admin/extensions/DurableTaskExtension/instances/d3b72dddefce4e758d92f4d411567177?taskHub={taskHub}&connection={connection}&code={systemKey}",
            "terminatePostUri": "http://localhost:7071/admin/extensions/DurableTaskExtension/instances/d3b72dddefce4e758d92f4d411567177/terminate?reason={text}&taskHub={taskHub}&connection={connection}&code={systemKey}",
            "rewindPostUri": "https://localhost:7071/admin/extensions/DurableTaskExtension/instances/d3b72dddefce4e758d92f4d411567177/rewind?reason={text}&taskHub={taskHub}&connection={connection}&code={systemKey}"
        }
    ```

> [!NOTE]
> Das Format der Webhook-URLs kann in Abhängigkeit davon variieren, welche Version des Azure Functions-Hosts Sie ausführen. Das obige Beispiel gilt für den Azure Functions 2.0-Host.

## <a name="retrieving-http-management-webhook-urls"></a>Abrufen von HTTP-Management-Webhook-URLs

Externe Systeme können mit robusten Funktionen über die Webhook-URLs kommunizieren, die Teil der in [HTTP-APIs in Durable Functions (Azure Functions)](durable-functions-http-api.md) beschriebenen Standardantwort sind. Allerdings kann auch im Orchestrierungsclient oder in einer Aktivitätsfunktion über die [CreateHttpManagementPayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateHttpManagementPayload_)-Methode der [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html)-Klasse programmgesteuert auf die Webhook-URLs zugegriffen werden. 

[CreateHttpManagementPayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateHttpManagementPayload_) verfügt über einen Parameter:

* **instanceId**: die eindeutige ID der Instanz.

Die Methode gibt eine Instanz von [HttpManagementPayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.Extensions.DurableTask.HttpManagementPayload.html#Microsoft_Azure_WebJobs_Extensions_DurableTask_HttpManagementPayload_) mit den folgenden Zeichenfolgeneigenschaften zurück:

* **Id**: die Instanz-ID der Orchestrierung (muss mit der Eingabe `InstanceId` identisch sein).
* **StatusQueryGetUri**: die Status-URL der Orchestrierungsinstanz.
* **SendEventPostUri**: URL der Orchestrierungsinstanz für die „Ereignisauslösung“.
* **TerminatePostUri**: URL der Orchestrierungsinstanz für das „Beenden“.
* **RewindPostUri**: Die „Zurückspulen“-URL der Orchestrierungsinstanz.

Aktivitätsfunktionen können eine Instanz von [HttpManagementPayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.Extensions.DurableTask.HttpManagementPayload.html#Microsoft_Azure_WebJobs_Extensions_DurableTask_HttpManagementPayload_) zum Überwachen oder Auslösen von Ereignissen in einer Orchestrierung an externe Systeme senden:

```csharp
[FunctionName("SendInstanceInfo")]
public static void SendInstanceInfo(
    [ActivityTrigger] DurableActivityContext ctx,
    [OrchestrationClient] DurableOrchestrationClient client,
    [DocumentDB(
        databaseName: "MonitorDB",
        collectionName: "HttpManagementPayloads",
        ConnectionStringSetting = "CosmosDBConnection")]out dynamic document)
{
    HttpManagementPayload payload = client.CreateHttpManagementPayload(ctx.InstanceId);

    // send the payload to Cosmos DB
    document = new { Payload = payload, id = ctx.InstanceId };
}
```

## <a name="rewinding-instances-preview"></a>Zurückspulen-Instanzen (Vorschau)

Eine fehlerhafte Orchestrierungsinstanz kann mithilfe der [RewindAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RewindAsync_System_String_System_String_)-API zu einem vorherigen integren Zustand *zurückgespult* werden. Dies funktioniert durch Zurückversetzen der Orchestrierung in den Status *Ausgeführt* und erneutes Ausführen der Ausführungsfehler der Aktivität und/oder Unterorchestrierung, die zum Fehler bei der Orchestrierung geführt haben.

> [!NOTE]
> Diese API ist nicht als Ersatz für ordnungsgemäße Richtlinien zur Fehlerbehandlung und Wiederholung gedacht. Sie soll nur in Fällen verwendet werden, in denen Orchestrierungsinstanzen aus unerwarteten Gründen ausfallen. Weitere Informationen zur Richtlinien für Fehlerbehandlung und Wiederholung finden Sie im Thema [Fehlerbehandlung](durable-functions-error-handling.md).

Ein Beispiel-Anwendungsfall für *Zurückspulen* ist ein Workflow, der eine Reihe von [menschlichen Genehmigungen](durable-functions-overview.md#pattern-5-human-interaction) umfasst. Angenommen, eine Reihe von Aktivitätsfunktionen benachrichtigt jemanden, dass seine Genehmigung erforderlich ist und wartet in Echtzeit auf die Antwort. Nachdem alle Genehmigungsaktivitäten Antworten erhalten haben oder abgelaufen sind, tritt ein Fehler bei einer anderen Aktivität aufgrund einer Fehlkonfiguration einer Anwendung auf (z.B. eine ungültige Datenbank-Verbindungszeichenfolge). Das Ergebnis ist ein Orchestrierungsfehler tief im Workflow. Mit der `RewindAsync`-API kann ein Anwendungsadministrator den Konfigurationsfehler beheben und die fehlerhafte Orchestrierung zum Zustand unmittelbar vor dem Fehler *zurückspulen*. Keiner der Schritte, die menschliche Interaktion erforderten, muss erneut genehmigt werden, und die Orchestrierung kann jetzt erfolgreich abgeschlossen werden.

> [!NOTE]
> Die Funktion *Zurückspulen* unterstützt kein Zurückspulen von Orchestrierungsinstanzen, die permanente Timer verwenden.

```csharp
[FunctionName("RewindInstance")]
public static Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    string reason = "Orchestrator failed and needs to be revived.";
    return client.RewindAsync(instanceId, reason);
}
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [HTTP APIs in Durable Functions (Azure Functions)](durable-functions-http-api.md) (HTTP-APIs in Durable Functions [Azure Functions])
