---
title: 'Zustandsbehaftete Singletons in Durable Functions: Azure'
description: "In diesem Artikel wird erläutert, wie ein zustandsbehaftetes Singleton in der Erweiterung Durable Functions für Azure Functions implementiert wird."
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
ms.openlocfilehash: d62bc24a0439aa8c11ced9d5f42917f9b6de1f24
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="stateful-singletons-in-durable-functions---counter-sample"></a>Zustandsbehaftete Singletons in Durable Functions: Zählerbeispiel

Zustandsbehaftete Singletons sind Orchestratorfunktionen mit langer Ausführungszeit (möglicherweise endlos), die einen Zustand speichern können und von anderen Funktionen aufgerufen und abgefragt werden können. Zustandsbehaftete Singletons ähneln dem [Actormodell](https://en.wikipedia.org/wiki/Actor_model) bei der verteilten Verarbeitung.

Orchestratorfunktionen stellen zwar keine ordnungsgemäße „Actor“-Implementierung dar, weisen jedoch viele gleiche Laufzeitmerkmale auf. Beispielsweise sind sie zustandsbehaftete, zuverlässige Singlethreadobjekte, haben einen transparenten Speicherort und sind global adressierbar. Durch diese Merkmale sind echte Actorimplementierungen besonders hilfreich, ohne dass ein separates Framework nötig ist.

In diesem Artikel wird gezeigt, wie das *Zählerbeispiel* ausgeführt wird. In dem Beispiel wird ein Singleton-Objekt gezeigt, das *Inkrement*- und *Dekrementvorgänge* unterstützt und den zugehörigen internen Zustand entsprechend aktualisiert.

## <a name="prerequisites"></a>Voraussetzungen

* Befolgen Sie die Anweisungen unter [Install Durable Functions](durable-functions-install.md) (Installieren von Durable Functions), um das Beispiel einzurichten.
* In diesem Artikel wird vorausgesetzt, dass Sie das [Hello Sequence](durable-functions-sequence.md)-Beispiel bereits Schritt für Schritt durchgegangen sind.

## <a name="scenario-overview"></a>Übersicht über das Szenario

Das Zählerszenario lässt sich über reguläre zustandslose Funktionen erstaunlich schwer implementieren. Eine der größten Herausforderungen besteht in der Verwaltung der **Parallelität**. Vorgänge wie *Inkrement* und *Dekrement* müssen atomisch sein. Andernfalls könnten Racebedingungen dazu führen, dass sich Vorgänge gegenseitig überschreiben.

Eine Option stellt die Verwendung eines einzelnen virtuellen Computers zum Hosten der Zählerdaten dar. Dies ist jedoch teuer und das Verwalten der **Zuverlässigkeit** kann eine Herausforderung sein, da ein einzelner virtueller Computer möglicherweise in regelmäßigen Zeitabständen neu gestartet wird. Alternativ könnten Sie eine verteilte Plattform mit Synchronisierungtools wie Blobleases verwenden, die bei der Verwaltung der Parallelität helfen sollen. Dies führt jedoch zu einer hohen **Komplexität**.

Durable Functions vereinfacht die Implementierung dieser Art von Szenario, da Orchestrierungsinstanzen eine Affinität zu einem einzelnen virtuellen Computer haben, und die Ausführung der Orchestratorfunktion immer als Singlethreadfunktion erfolgt. Nicht nur das, sie werden zudem lange ausgeführt, sind zustandsbehaftet und können auf externe Ereignisse reagieren. Der folgende Beispielcode zeigt, wie ein solcher Zähler als Orchestratorfunktion mit langer Ausführungsdauer implementiert wird.

## <a name="the-sample-function"></a>Die Beispielfunktion

In diesem Artikel wird die Funktion **E3_Counter** in der Beispiel-App schrittweise erläutert.

In den folgenden Abschnitten wird der Code erläutert, der für die Visual Studio-Entwicklung verwendet wird. Der Code für die Entwicklung im Azure-Portal ist ähnlich.

## <a name="the-counter-orchestration"></a>Die Zählerorchestrierung

Im Folgenden wird der Code dargestellt, der die Orchestratorfunktion implementiert:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/Counter.cs)]

Diese Orchestratorfunktion führt im Wesentlichen Folgendes aus:

1. Sie überwacht ein externes Ereignis mit dem Namen *Vorgang* mit [WaitForExternalEvent](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_WaitForExternalEvent_).
2. Sie erhöht oder verringert abhängig von dem angeforderten Vorgang die lokale Variable `counterState`.
3. Sie startet den Orchestrator mit der Methode [ContinueAsNew](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_ContinueAsNew_) neu und legt dabei den aktuellen Wert von `counterState` als neue Eingabe fest.
4. Sie wird endlos weiter ausgeführt oder bis eine Meldung über das *Ende* empfangen wird.

Dies ist ein Beispiel für eine *endlose Orchestrierung*, d.h. eine Orchestrierung, die möglicherweise niemals endet. Sie reagiert auf von der Methode [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) gesendete Meldungen. Diese Methode kann von einer Nicht-Orchestratorfunktion abgerufen werden.

Ein eindeutiges Merkmal dieser Orchestratorfunktion ist, dass sie effektiv keinen Verlauf hat: Die Methode `ContinueAsNew` setzt den Verlauf nach jedem verarbeiteten Ereignis zurück. Dies ist die bevorzugte Methode für die Implementierung eines Orchestrators mit beliebiger Lebensdauer. Durch die Verwendung einer `while`-Schleife könnte der Verlauf der Orchestratorfunktion unbegrenzt wachsen. Dies wiederum könnte zu einer unnötig hohen Speicherauslastung führen.

> [!NOTE]
> Neben endlosen Orchestrierungen gibt es weitere Anwendungsfälle für die Methode `ContinueAsNew`. Weitere Informationen finden Sie unter [Endlose Orchestrierungen](durable-functions-eternal-orchestrations.md).

## <a name="running-the-sample"></a>Ausführen des Beispiels

Wenn Sie die über HTTP ausgelösten Funktionen verwenden, die im Beispiel enthalten sind, können Sie mit der Orchestrierung beginnen, indem Sie folgende HTTP POST-Anforderung verwenden. Damit `counterState` bei 0 (null) beginnen kann (der Standardwert für `int`), umfasst dieser Inhalt keine Anforderung.

```
POST http://{host}/orchestrators/E3_Counter HTTP/1.1
Content-Length: 0
```

```
HTTP/1.1 202 Accepted
Content-Length: 719
Content-Type: application/json; charset=utf-8
Location: http://{host}/admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}

{"id":"bcf6fb5067b046fbb021b52ba7deae5a","statusQueryGetUri":"http://{host}/admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}","sendEventPostUri":"http://{host}/admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a/raiseEvent/{eventName}?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}","terminatePostUri":"http://{host}/admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a/terminate?reason={text}&taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}"}
```

Die Instanz **E3_Counter** wird gestartet und wartet anschließend darauf, dass über `RaiseEventAsync` oder über den HTTP POST-Webhook **SendEventUrl** ein Ereignis gesendet wird. Auf diesen Webhook wird in der 202-Antwort verwiesen. Gültige `eventName`-Werte sind *incr*, *decr* und *end*.

```
POST http://{host}/admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a/raiseEvent/operation?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey} HTTP/1.1
Content-Type: application/json
Content-Length: 6

"incr"
```

Sie können die Ergebnisse des Vorgangs „incr“ anzeigen, indem Sie sich die Funktionsprotokolle im Azure Functions-Portal anschauen.

```
2017-06-29T18:54:53.998 Function started (Id=34e34a61-38b3-4eac-b6e2-98b85e32eec8)
2017-06-29T18:54:53.998 Current counter state is 0. Waiting for next operation.
2017-06-29T18:58:01.458 Function started (Id=b45d6c2f-39f3-42a2-b904-7761b2614232)
2017-06-29T18:58:01.458 Current counter state is 0. Waiting for next operation.
2017-06-29T18:58:01.458 Received 'incr' operation.
2017-06-29T18:58:01.458 Function completed (Success, Id=b45d6c2f-39f3-42a2-b904-7761b2614232, Duration=8ms)
2017-06-29T18:58:11.518 Function started (Id=e1f38cb2-546a-404d-ab22-1ac8f81a93d9)
2017-06-29T18:58:11.518 Current counter state is 1. Waiting for next operation.
```

Entsprechend können Sie sehen, dass das Feld `input` auf den aktualisierten Wert (1) festgelegt wurde, wenn Sie den Orchestratorzustand überprüfen.

```
GET http://{host}/admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey} HTTP/1.1
```

```
HTTP/1.1 202 Accepted
Content-Length: 129
Content-Type: application/json; charset=utf-8
Location: http://{host}/admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}

{"runtimeStatus":"Running","input":1,"output":null,"createdTime":"2017-06-29T18:58:01Z","lastUpdatedTime":"2017-06-29T18:58:11Z"}
```

Sie können weiterhin neue Vorgänge an diese Instanz senden und beobachten, wie der zugehörige Zustand entsprechend aktualisiert wird. Sie können einen *Beendigungsvorgang* senden, wenn Sie die Instanz beenden möchten.

> [!WARNING]
> Zum Zeitpunkt des Schreibens liegen beim Aufrufen von `ContinueAsNew` bekannte Racebedingungen vor, während gleichzeitig Nachrichten wie externe Ereignisse oder Beendigungsanforderungen verarbeitet werden. Aktuelle Informationen zu diesen Racebedingungen finden Sie in diesem [GitHub-Problem](https://github.com/Azure/azure-functions-durable-extension/issues/67).

## <a name="wrapping-up"></a>Zusammenfassung

Jetzt kennen Sie einige der erweiterten Funktionen von Durable Functions besser, insbesondere die Funktionen `WaitForExternalEvent` und `ContinueAsNew`. Mit diesen Tools können Sie verschiedene Formen der zustandsbehafteten Singletons, z.B. Zähler und Aggregatoren, schreiben.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Ausführen des Beispiels für Benutzerinteraktion](durable-functions-phone-verification.md)



