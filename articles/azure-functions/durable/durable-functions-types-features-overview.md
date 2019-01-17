---
title: Übersicht der Funktionstypen und Features für permanente Funktionen – Azure
description: Lernen Sie die Typen der Funktionen und Rollen kennen, die eine Kommunikation zwischen Funktionen als Teil einer permanenten Funktionsorchestrierung ermöglichen.
services: functions
author: jeffhollan
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: fbfee92343bfecfbe8395f95775ae1f107b99299
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2019
ms.locfileid: "54037275"
---
# <a name="overview-of-function-types-and-features-for-durable-functions-azure-functions"></a>Übersicht der Funktionstypen und Features für permanente Funktionen (Azure Functions)

Durable Functions stellt eine zustandsbehaftete Orchestrierung der Funktionsausführung bereit. Eine permanente Funktion stellt eine Lösung dar, die sich aus verschiedenen Azure-Funktionen zusammensetzt. Jede dieser Funktionen kann im Rahmen einer Orchestrierung verschiedene Rollen spielen. Das folgende Dokument bietet eine Übersicht der Funktionstypen, die an einer permanenten Funktionsorchestrierung beteiligt sind. Es enthält darüber hinaus einige allgemeine Muster zum Verbinden von Funktionen untereinander.  Um sofort zu beginnen, erstellen Sie Ihre erste Durable Function in [C#](durable-functions-create-first-csharp.md) oder [JavaScript](quickstart-js-vscode.md).

![Typen von permanenten Funktionen][1]  

## <a name="types-of-functions"></a>Funktionstypen

### <a name="activity-functions"></a>Aktivitätsfunktionen

Aktivitätsfunktionen sind die grundlegende Arbeitseinheit in einer permanenten Orchestrierung.  Aktivitätsfunktionen sind die Funktionen und Aufgaben, die im Prozess orchestriert werden.  Sie können beispielsweise eine permanente Funktion zum Bearbeiten einer Bestellung erstellen – Überprüfen des Lagerbestands, Belasten des Kunden und Erstellen einer Lieferung.  Jede dieser Aufgaben wäre eine Aktivitätsfunktion.  Für Aktivitätsfunktionen bestehen hinsichtlich der Arbeiten, die mit ihnen ausgeführt werden können, keine Einschränkungen.  Sie können in jeder [von Durable Functions unterstützten Sprache](durable-functions-overview.md#language-support) erstellt werden. Das Durable Task Framework garantiert, dass jede aufgerufene Aktivitätsfunktion mindestens einmal in einer Orchestrierung ausgeführt wird.

Eine Aktivitätsfunktion muss durch einen [Aktivitätstrigger](durable-functions-bindings.md#activity-triggers) ausgelöst werden.  .NET-Funktionen empfangen einen [DurableActivityContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContext.html) als Parameter. Sie können den Trigger auch an beliebige andere Objekte binden, um Eingaben an die Funktion zu übergeben. In JavaScript kann über die `<activity trigger binding name>`-Eigenschaft des [`context.bindings`-Objekts](../functions-reference-node.md#bindings) auf die Eingabe zugegriffen werden.

Ihre Aktivitätsfunktion kann außerdem Werte an den Orchestrator zurückgeben.  Wenn von einer Aktivitätsfunktion viele Werte gesendet oder zurückgegeben werden, können Sie [Tupels oder Arrays nutzen](durable-functions-bindings.md#passing-multiple-parameters).  Aktivitätsfunktionen können nur von einer Orchestrierungsinstanz getriggert werden.  Zwar können eine Aktivitätsfunktion und eine andere Funktion (etwa eine durch HTTP ausgelöste Funktion) Code gemein haben, jede Funktion kann jedoch nur einen Trigger aufweisen.

Weitere Informationen und Beispiele finden sich im [Artikel zu Bindungen für permanente Funktionen](durable-functions-bindings.md#activity-triggers).

### <a name="orchestrator-functions"></a>Orchestratorfunktionen

Orchestratorfunktionen bilden das Herzstück aller permanenten Funktionen.  Orchestratorfunktionen beschreiben die Art und die Reihenfolge, in denen Aktionen ausgeführt werden.  Orchestratorfunktionen beschreiben die Orchestrierung in Code (C# oder JavaScript), wie unter [Durable Functions patterns and technical concepts](durable-functions-concepts.md) (Muster und technische Konzepte von Durable Functions) dargestellt.  Eine Orchestrierung kann viele verschiedene Aktionsarten aufweisen, z. B. [Aktivitätsfunktionen](#activity-functions), [untergeordnete Orchestrierungen](#sub-orchestrations), [Warten auf externe Ereignisse](#external-events) und [Timer](#durable-timers).  

Eine Orchestratorfunktion muss durch einen [Orchestrierungstrigger](durable-functions-bindings.md#orchestration-triggers) ausgelöst werden.

Ein Orchestrator wird von einem [Orchestratorclient](#client-functions) gestartet, der seinerseits von beliebigen Quellen (HTTP, Warteschlangen, Ereignisdatenströmen) ausgelöst werden kann.  Jede Instanz einer Orchestrierung weist einen Instanzbezeichner auf, der automatisch generiert (empfohlen) oder vom Benutzer generiert sein kann.  Dieser Bezeichner kann zum [Verwalten von Instanzen](durable-functions-instance-management.md) der Orchestrierung verwendet werden.

Weitere Informationen und Beispiele finden sich im [Artikel zu Bindungen für permanente Funktionen](durable-functions-bindings.md#orchestration-triggers).

### <a name="client-functions"></a>Clientfunktionen

Clientfunktionen sind getriggerte Funktionen, die neue Instanzen einer Orchestrierung erstellen.  Sie bilden den Einstiegspunkt zum Erstellen einer Instanz einer permanenten Orchestrierung.  Clientfunktionen können von einem beliebigen Trigger (HTTP, Warteschlangen, Ereignisdatenströmen usw.) ausgelöst und in jeder von der App unterstützten Sprache erstellt werden.  Über den Trigger hinaus weisen Clientfunktionen eine [Orchestrierungsclientbindung](durable-functions-bindings.md#orchestration-client) auf, die es ihnen ermöglicht, permanente Orchestrierungen zu erstellen und zu verwalten.  Das einfachste Beispiel für eine Clientfunktion ist eine per HTTP ausgelöste Funktion, die eine Orchestratorfunktion startet und eine Statusüberprüfungsantwort zurückgibt, wie [im folgenden Beispiel dargestellt](durable-functions-http-api.md#http-api-url-discovery).

Weitere Informationen und Beispiele finden sich im [Artikel zu Bindungen für permanente Funktionen](durable-functions-bindings.md#orchestration-client).

## <a name="features-and-patterns"></a>Funktionen und Muster

### <a name="sub-orchestrations"></a>Untergeordnete Orchestrierungen

Orchestratorfunktionen können nicht nur Aktivitätsfunktionen aufrufen, sondern auch andere Orchestratorfunktionen. Beispielsweise können Sie aus einer Bibliothek mit Orchestratorfunktionen eine größere Orchestrierung erstellen. Oder Sie können mehrere Instanzen einer Orchestratorfunktion parallel ausführen.

Weitere Informationen und Beispiele finden sich im [Artikel über untergeordnete Orchestrierung](durable-functions-sub-orchestrations.md).

### <a name="durable-timers"></a>Permanente Timer

[Durable Functions](durable-functions-overview.md) bieten *permanente Timer* für die Verwendung in Orchestratorfunktionen zum Implementieren von Verzögerungen oder zum Einrichten von Timeouts für asynchrone Aktionen. Permanente Timer sollten in Orchestratorfunktionen anstelle von `Thread.Sleep` und `Task.Delay` (C#) bzw. `setTimeout()` und `setInterval()` (JavaScript) verwendet werden.

Weitere Informationen und Beispiele zu permanenten Treibern finden sich im [Artikel über permanente Treiber](durable-functions-timers.md)

### <a name="external-events"></a>Externe Ereignisse

Orchestratorfunktionen können auf externe Ereignisse warten, um eine Orchestrierungsinstanz zu aktualisieren. Diese Funktion von Durable Functions ist oft bei der Behandlung menschlicher Interaktion oder anderer externer Rückrufe nützlich.

Weitere Informationen und Beispiele finden sich im [Artikel über externe Ereignisse](durable-functions-external-events.md).

### <a name="error-handling"></a>Fehlerbehandlung

Durable Function-Orchestrierungen sind im Code implementiert und können die Funktionen zur Fehlerbehandlung aus der Programmiersprache nutzen.  Das bedeutet, dass Muster wie „try/catch“ in Ihrer Orchestrierung funktionieren.  Permanente Funktionen sind außerdem mit einigen integrierten Wiederholungsrichtlinien ausgestattet.  Eine Aktion kann Aktivitäten bei Ausnahmen automatisch verzögern und wiederholen.  Mithilfe von Wiederholungen können Sie vorübergehende Ausnahmen behandeln, ohne die Orchestrierung aufgeben zu müssen.

Weitere Informationen und Beispiele finden sich im [Artikel zur Fehlerbehandlung](durable-functions-error-handling.md).

### <a name="cross-function-app-communication"></a>Funktionsübergreifende App-Kommunikation

Während eine permanente Orchestrierung im Allgemeinen innerhalb des Kontexts einer einzelnen Funktions-App existiert, gibt es Muster, die Ihnen die übergreifende Koordination von Orchestrierungen über viele Funktions-Apps hinweg ermöglichen.  Auch wenn die App-übergreifende Kommunikation über HTTP erfolgt, ermöglicht es die Verwendung des permanenten Frameworks für jede Aktivität, trotzdem einen übergreifenden permanenten Prozess zwischen zwei Apps aufrecht zu erhalten.

Beispiele für eine funktionsübergreifende App-Orchestrierung in C# und JavaScript finden Sie im Folgenden.  Eine Aktivität startet die externe Orchestrierung. Eine andere Aktivität ruft dann den Status ab und gibt ihn zurück.  Der Orchestrator wartet auf den Abschluss des Status, bevor er fortfährt.

#### <a name="c"></a>C#

```csharp
[FunctionName("OrchestratorA")]
public static async Task RunRemoteOrchestrator(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    // Do some work...

    // Call a remote orchestration
    string statusUrl = await context.CallActivityAsync<string>(
        "StartRemoteOrchestration", "OrchestratorB");

    // Wait for the remote orchestration to complete
    while (true)
    {
        bool isComplete = await context.CallActivityAsync<bool>("CheckIsComplete", statusUrl);
        if (isComplete)
        {
            break;
        }

        await context.CreateTimer(context.CurrentUtcDateTime.AddMinutes(1), CancellationToken.None);
    }

    // B is done. Now go do more work...
}

[FunctionName("StartRemoteOrchestration")]
public static async Task<string> StartRemoteOrchestration([ActivityTrigger] string orchestratorName)
{
    using (var response = await HttpClient.PostAsync(
        $"https://appB.azurewebsites.net/orchestrations/{orchestratorName}",
        new StringContent("")))
    {
        string statusUrl = await response.Content.ReadAsAsync<string>();
        return statusUrl;
    }
}

[FunctionName("CheckIsComplete")]
public static async Task<bool> CheckIsComplete([ActivityTrigger] string statusUrl)
{
    using (var response = await HttpClient.GetAsync(statusUrl))
    {
        // 200 = Complete, 202 = Running
        return response.StatusCode == HttpStatusCode.OK;
    }
}
```

#### <a name="javascript-functions-2x-only"></a>JavaScript (nur Functions 2.x)

```javascript
const df = require("durable-functions");
const moment = require("moment");

module.exports = df.orchestrator(function*(context) {
    // Do some work...

    // Call a remote orchestration
    const statusUrl = yield context.df.callActivity("StartRemoteOrchestration", "OrchestratorB");

    // Wait for the remote orchestration to complete
    while (true) {
        const isComplete = yield context.df.callActivity("CheckIsComplete", statusUrl);
        if (isComplete) {
            break;
        }

        const waitTime = moment(context.df.currentUtcDateTime).add(1, "m").toDate();
        yield context.df.createTimer(waitTime);
    }

    // B is done. Now go do more work...
});
```

```javascript
const request = require("request-promise-native");

module.exports = async function(context, orchestratorName) {
    const options = {
        method: "POST",
        uri: `https://appB.azurewebsites.net/orchestrations/${orchestratorName}`,
        body: ""
    };

    const statusUrl = await request(options);
    return statusUrl;
};
```

```javascript
const request = require("request-promise-native");

module.exports = async function(context, statusUrl) {
    const options = {
        method: "GET",
        uri: statusUrl,
        resolveWithFullResponse: true,
    };

    const response = await request(options);
    // 200 = Complete, 202 = Running
    return response.statusCode === 200;
};
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Bindings for Durable Functions (Azure Functions)](durable-functions-bindings.md) (Bindungen für Durable Functions [Azure Functions])

<!-- Media references -->
[1]: media/durable-functions-types-features-overview/durable-concepts.png
