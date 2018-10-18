---
title: Funktionsverkettung in Durable Functions – Azure
description: Es wird beschrieben, wie Sie ein Durable Functions-Beispiel mit einer Funktionssequenz ausführen.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 09/06/2018
ms.author: azfuncdf
ms.openlocfilehash: c84977dacddcf9ccca7fde735ad4acb8a1523fa9
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2018
ms.locfileid: "44378702"
---
# <a name="function-chaining-in-durable-functions---hello-sequence-sample"></a>Funktionsverkettung in Durable Functions – „Hello Sequence“-Beispiel

Bei der Funktionsverkettung geht es um das Muster für die Ausführung einer Funktionssequenz in einer bestimmten Reihenfolge. Häufig muss die Ausgabe einer Funktion auf die Eingabe einer anderen Funktion angewendet werden. In diesem Artikel wird ein Beispiel beschrieben, in dem [Durable Functions](durable-functions-overview.md) zum Implementieren der Funktionsverkettung verwendet wird.

## <a name="prerequisites"></a>Voraussetzungen

* [Sie haben Durable Functions installiert](durable-functions-install.md).

## <a name="the-functions"></a>Die Funktionen

In diesem Artikel werden die folgenden Funktionen in der Beispiel-App beschrieben:

* `E1_HelloSequence`: Eine Orchestratorfunktion, mit der `E1_SayHello` in einer Sequenz mehrfach aufgerufen wird. Die Ausgaben der `E1_SayHello`-Aufrufe werden gespeichert, und die Ergebnisse werden aufgezeichnet.
* `E1_SayHello`: Eine Aktivitätsfunktion, mit der einer Zeichenfolge das Wort „Hello“ vorangestellt wird.

In den folgenden Abschnitten werden die Konfiguration und der Code beschrieben, die für C#-Skripts und JavaScript verwendet werden. Der Code für die Visual Studio-Entwicklung ist am Ende des Artikels angegeben.

> [!NOTE]
> Durable Functions steht in JavaScript nur in der Functions v2-Runtime zur Verfügung.

## <a name="e1hellosequence"></a>E1_HelloSequence
### <a name="functionjson-file"></a>Datei „function.json“

Hier ist der Inhalt der Datei *function.json* für die Orchestratorfunktion angegeben, falls Sie Visual Studio Code oder das Azure-Portal für die Entwicklung verwenden. Die meisten *function.json*-Dateien für Orchestratoren ähneln dieser Datei stark.

[!code-json[Main](~/samples-durable-functions/samples/csx/E1_HelloSequence/function.json)]

Ein wichtiger Punkt ist der Bindungstyp `orchestrationTrigger`. Alle Orchestratorfunktionen müssen diesen Triggertyp verwenden.

> [!WARNING]
> Um die Regel „keine E/A“ für Orchestratorfunktionen zu beachten, sollten Sie keine Eingabe- oder Ausgabebindungen verwenden, wenn Sie die Triggerbindung `orchestrationTrigger` einsetzen.  Falls andere Eingabe- oder Ausgabebindungen erforderlich sind, sollten sie stattdessen im Kontext von `activityTrigger`-Funktionen verwendet werden, die vom Orchestrator aufgerufen werden.

### <a name="c-script-visual-studio-code-and-azure-portal-sample-code"></a>C#-Skript (Visual Studio Code und Azure-Portal-Beispielcode) 

Hier ist der Quellcode angegeben:

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E1_HelloSequence/run.csx)]

Alle C#-Orchestrierungsfunktionen müssen über einen Parameter vom Typ `DurableOrchestrationContext` verfügen, der in der Assembly `Microsoft.Azure.WebJobs.Extensions.DurableTask` enthalten ist. Wenn Sie C#-Skript verwenden, kann auf die Assembly mit der Notation `#r` verwiesen werden. Mit diesem Kontextobjekt können Sie andere Funktionen vom Typ *Aktivität* aufrufen und Eingabeparameter mit der dazugehörigen [CallActivityAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CallActivityAsync_)-Methode übergeben.

Im Code wird `E1_SayHello` dreimal nacheinander mit unterschiedlichen Parameterwerten aufgerufen. Der Rückgabewert jedes Aufrufs wird der Liste `outputs` hinzugefügt, die am Ende der Funktion zurückgegeben wird.

### <a name="javascript"></a>JavaScript

Hier ist der Quellcode angegeben:

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E1_HelloSequence/index.js)]

Alle JavaScript-Orchestrierungsfunktionen müssen das Modul `durable-functions` enthalten. Dabei handelt es sich um eine JavaScript-Bibliothek, die die Aktionen der Orchestrierungsfunktion in das Ausführungsprotokoll von Durable Functions für vorgangsexterne Sprachen überträgt. Zwischen einer Orchestrierungsfunktion und anderen JavaScript-Funktionen bestehen drei wesentliche Unterschiede:

1. Die Funktion ist eine [Generatorfunktion](https://docs.microsoft.com/scripting/javascript/advanced/iterators-and-generators-javascript).
2. Die Funktion wird von einem Aufruf des Moduls `durable-functions` umschlossen (hier `df`).
3. Die Funktion wird mit dem Aufruf von `return` und nicht von `context.done` beendet.

Das `context`-Objekt enthält ein `df`-Objekt, mit dem Sie andere *Aktivitätsfunktionen* aufrufen und Eingabeparameter mit der zugehörigen `callActivityAsync`-Methode übergeben können. Im Code wird `E1_SayHello` dreimal nacheinander mit unterschiedlichen Parameterwerten aufgerufen. Dabei wird `yield` verwendet, um anzugeben, dass für die Ausführung auf die Rückgabe der asynchronen Aufrufe der Aktivitätsfunktion gewartet werden muss. Der Rückgabewert jedes Aufrufs wird der Liste `outputs` hinzugefügt, die am Ende der Funktion zurückgegeben wird.

## <a name="e1sayhello"></a>E1_SayHello
### <a name="functionjson-file"></a>Datei „function.json“

Die Datei *function.json* für die Aktivitätsfunktion `E1_SayHello` ähnelt der Datei von `E1_HelloSequence`, aber es wird nicht der Bindungstyp `orchestrationTrigger`, sondern `activityTrigger` verwendet.

[!code-json[Main](~/samples-durable-functions/samples/csx/E1_SayHello/function.json)]

> [!NOTE]
> Alle Funktionen, die von einer Orchestrierungsfunktion aufgerufen werden, müssen die Bindung `activityTrigger` verwenden.

Die Implementierung von `E1_SayHello` ist ein relativ einfacher Zeichenfolgen-Formatierungsvorgang.

### <a name="c"></a>C#

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E1_SayHello/run.csx)]

Diese Funktion verfügt über einen Parameter vom Typ [DurableActivityContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContext.html). Er wird genutzt, um Eingaben zu erhalten, die über den Aufruf an [`CallActivityAsync<T>`](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CallActivityAsync_) der Orchestratorfunktion übergeben wurden.

### <a name="javascript"></a>JavaScript

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E1_SayHello/index.js)]

Im Unterschied zu einer JavaScript-Orchestrierungsfunktion erfordert eine JavaScript-Aktivitätsfunktion keine spezielle Einrichtung. Die von der Orchestratorfunktion übergebene Eingabe befindet sich im Objekt `context.bindings` unter dem Namen der Bindung `activitytrigger`, in diesem Fall `context.bindings.name`. Der Name der Bindung kann als Parameter der exportierten Funktion eingerichtet werden und der Zugriff darauf direkt erfolgen. Dies ist bei dem Beispielcode der Fall.

## <a name="run-the-sample"></a>Ausführen des Beispiels

Senden Sie die folgende HTTP POST-Anforderung, um die `E1_HelloSequence`-Orchestrierung auszuführen.

```
POST http://{host}/orchestrators/E1_HelloSequence
```

> [!NOTE]
> Im vorherigen HTTP-Ausschnitt wird davon ausgegangen, dass ein Eintrag in der Datei `host.json` vorhanden ist, der das Standardpräfix `api/` aus den URLs aller HTTP-Triggerfunktionen entfernt. Sie finden das Markup für diese Konfiguration in der Datei `host.json` in den Beispielen.

Wenn Sie das Beispiel z. B. in einer Funktions-App namens „myfunctionapp“ ausführen, ersetzen Sie „{host}“ durch „myfunctionapp.azurewebsites.net“.

Das Ergebnis ist eine HTTP 202-Antwort, die wie folgt aussehen kann (hier gekürzt):

```
HTTP/1.1 202 Accepted
Content-Length: 719
Content-Type: application/json; charset=utf-8
Location: http://{host}/admin/extensions/DurableTaskExtension/instances/96924899c16d43b08a536de376ac786b?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}

(...trimmed...)
```

An diesem Punkt wird die Orchestrierung in die Warteschlange eingereiht und sofort ausgeführt. Die URL im Header `Location` kann verwendet werden, um den Status der Ausführung zu überprüfen.

```
GET http://{host}/admin/extensions/DurableTaskExtension/instances/96924899c16d43b08a536de376ac786b?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}
```

Das Ergebnis ist der Status der Orchestrierung. Die Ausführung und der Abschluss werden schnell durchgeführt, sodass Sie den Status *Completed* (Abgeschlossen) mit dieser Antwort sehen (hier gekürzt):

```
HTTP/1.1 200 OK
Content-Length: 179
Content-Type: application/json; charset=utf-8

{"runtimeStatus":"Completed","input":null,"output":["Hello Tokyo!","Hello Seattle!","Hello London!"],"createdTime":"2017-06-29T05:24:57Z","lastUpdatedTime":"2017-06-29T05:24:59Z"}
```

Sie sehen, dass das `runtimeStatus`-Element der Instanz *Completed* lautet und dass `output` das per JSON serialisierte Ergebnis der Ausführung der Orchestratorfunktion enthält.

> [!NOTE]
> Der HTTP POST-Endpunkt, von dem die Orchestratorfunktion gestartet wurde, wird in der Beispiel-App als HTTP-Triggerfunktion mit dem Namen „HttpStart“ implementiert. Sie können eine ähnliche Startlogik auch für andere Triggertypen wie `queueTrigger`, `eventHubTrigger` oder `timerTrigger` implementieren.

Sehen Sie sich die Protokolle zur Funktionsausführung an. Die Funktion `E1_HelloSequence` wurde mehrmals gestartet und abgeschlossen. Dies liegt am Wiedergabeverhalten, das in der [Übersicht](durable-functions-overview.md) beschrieben ist. Andererseits ist es nur zu drei Ausführungen von `E1_SayHello` gekommen, da diese Funktionsausführungen nicht wiedergegeben werden.

## <a name="visual-studio-sample-code"></a>Visual Studio-Beispielcode

So sieht die Orchestrierung als einzelne C#-Datei in einem Visual Studio-Projekt aus:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HelloSequence.cs)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Beispiel wird eine einfache Funktionsverkettungsorchestrierung veranschaulicht. Im nächsten Beispiel wird die Implementierung des Musters „Auffächern nach außen/innen“ gezeigt. 

> [!div class="nextstepaction"]
> [Beispiel für „Auffächern nach außen/innen“ ausführen](durable-functions-cloud-backup.md)
