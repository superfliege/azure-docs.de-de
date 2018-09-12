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
ms.date: 07/04/2018
ms.author: azfuncdf
ms.openlocfilehash: 24889765369a2af7d87ddb0fdc8f7c9f91ac66fd
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44091390"
---
# <a name="overview-of-function-types-and-features-for-durable-functions-azure-functions"></a>Übersicht der Funktionstypen und Features für permanente Funktionen (Azure Functions)

Azure Durable Functions stellt die zustandsbehaftete Orchestrierung der Funktionsausführung bereit.  Eine permanente Funktion stellt eine Lösung dar, die sich aus verschiedenen Azure-Funktionen zusammensetzt.  Jede dieser Funktionen kann im Rahmen einer Orchestrierung verschiedene Rollen spielen.  Das folgende Dokument bietet eine Übersicht der Funktionstypen, die an einer permanenten Funktionsorchestrierung beteiligt sind.  Es enthält darüber hinaus einige allgemeine Muster zum Verbinden von Funktionen untereinander.  

![Typen von permanenten Funktionen][1]  

## <a name="types-of-functions"></a>Funktionstypen

### <a name="activity-functions"></a>Aktivitätsfunktionen

Aktivitätsfunktionen sind die grundlegende Arbeitseinheit in einer permanenten Orchestrierung.  Aktivitätsfunktionen sind die Funktionen und Aufgaben, die im Prozess orchestriert werden.  Sie können beispielsweise eine permanente Funktion zum Bearbeiten einer Bestellung erstellen – Überprüfen des Lagerbestands, Belasten des Kunden und Erstellen einer Lieferung.  Jede dieser Aufgaben wäre eine Aktivitätsfunktion.  Für Aktivitätsfunktionen bestehen hinsichtlich der Arbeiten, die mit ihnen ausgeführt werden können, keine Einschränkungen.  Sie können in jeder Sprache erstellt werden, die von Azure Functions unterstützt wird.  Das permanente Aufgabenframework garantiert, dass jede aufgerufene Aktivitätsfunktion mindestens einmal in einer Orchestrierung ausgeführt wird.

Eine Aktivitätsfunktion muss durch einen [Aktivitätstrigger](durable-functions-bindings.md#activity-triggers) ausgelöst werden.  Diese Funktion empfängt einen [DurableActivityContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContext.html) als Parameter. Sie können den Trigger auch an beliebige andere Objekte binden, um Eingaben an die Funktion zu übergeben.  Ihre Aktivitätsfunktion kann außerdem Werte an den Orchestrator zurückgeben.  Wenn von einer Aktivitätsfunktion viele Werte gesendet oder zurückgegeben werden, können Sie [Tupels oder Arrays nutzen](durable-functions-bindings.md#passing-multiple-parameters).  Aktivitätsfunktionen können nur von einer Orchestrierungsinstanz getriggert werden.  Zwar können eine Aktivitätsfunktion und eine andere Funktion (etwa eine durch HTTP ausgelöste Funktion) Code gemein haben, jede Funktion kann jedoch nur einen Trigger aufweisen.

Weitere Informationen und Beispiele finden sich im [Artikel zu Bindungen für permanente Funktionen](durable-functions-bindings.md#activity-triggers).

### <a name="orchestrator-functions"></a>Orchestratorfunktionen

Orchestratorfunktionen bilden das Herzstück aller permanenten Funktionen.  Orchestratorfunktionen beschreiben die Art und die Reihenfolge, in denen Aktionen ausgeführt werden.  Orchestratorfunktionen beschreiben die Orchestrierung in Code (C# oder JavaScript), wie in der [Übersicht der permanenten Funktionen](durable-functions-overview.md) dargestellt.  Eine Orchestrierung kann viele verschiedene Aktionsarten aufweisen, z. B. [Aktivitätsfunktionen](#activity-functions), [untergeordnete Orchestrierungen](#sub-orchestrations), [Warten auf externe Ereignisse](#external-events) und [Timer](#durable-timers).  

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

Ein Beispiel einer funktionsübergreifenden App-Orchestrierung in C# finden Sie unten.  Eine Aktivität startet die externe Orchestrierung. Eine andere Aktivität ruft dann den Status ab und gibt ihn zurück.  Der Orchestrator wartet auf den Abschluss des Status, bevor er fortfährt.

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

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Bindings for Durable Functions (Azure Functions)](durable-functions-bindings.md) (Bindungen für Durable Functions [Azure Functions])

> [!div class="nextstepaction"]
> [Install the Durable Functions extension and samples (Azure Functions)](durable-functions-install.md) (Installieren der Erweiterung Durable Functions und Beispiele [Azure Functions])

<!-- Media references -->
[1]: media/durable-functions-types-features-overview/durable-concepts.png
