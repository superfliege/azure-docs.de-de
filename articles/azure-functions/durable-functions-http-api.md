---
title: "HTTP-APIs in Durable Functions – Azure"
description: "Es wird beschrieben, wie Sie HTTP-APIs in der Erweiterung „Durable Functions“ für Azure Functions implementieren."
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
ms.openlocfilehash: bb5361022e4c9693812753ae33df5aeb037b5aaa
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="http-apis-in-durable-functions-azure-functions"></a>HTTP-APIs in Durable Functions (Azure Functions)

Die Erweiterung „Durable Task“ macht eine Reihe von HTTP-APIs verfügbar, die zum Durchführen der folgenden Aufgaben verwendet werden können:

* Abrufen des Status einer Orchestrierungsinstanz
* Senden eines Ereignisses an eine Orchestrierungsinstanz im Wartezustand
* Beenden einer ausgeführten Orchestrierungsinstanz

Bei all diesen HTTP-APIs handelt es sich um Webhookvorgänge, die direkt von der Erweiterung „Durable Task“ verarbeitet werden. Sie gelten nicht spezifisch für eine Funktion in der Funktionen-App.

> [!NOTE]
> Diese Vorgänge können auch direkt aufgerufen werden, indem die Instanzverwaltungs-APIs der [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html)-Klasse verwendet werden. Weitere Informationen finden Sie unter [Instanzverwaltung](durable-functions-instance-management.md).

## <a name="http-api-url-discovery"></a>Ermittlung der HTTP-API-URL

Die [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html)-Klasse macht eine [CreateCheckStatusResponse](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateCheckStatusResponse_)-API verfügbar, die zum Generieren einer HTTP-Antwortnutzlast mit Links zu allen unterstützten Vorgängen verwendet werden kann. Hier ist ein Beispiel für eine HTTP-Triggerfunktion angegeben, mit dem die Nutzung dieser API veranschaulicht wird:

[!code-csharp[Main](~/samples-durable-functions/samples/csx/HttpStart/run.csx)]

Diese Beispielfunktion erzeugt die folgenden JSON-Antwortdaten. Der Datentyp aller Felder lautet `string`.

| Feld             |Beschreibung                           |
|-------------------|--------------------------------------|
| id                |ID der Orchestrierungsinstanz |
| statusQueryGetUri |Status-URL der Orchestrierungsinstanz |
| sendEventPostUri  |URL der Orchestrierungsinstanz für die „Ereignisauslösung“ |
| terminatePostUri  |URL der Orchestrierungsinstanz für die „Beendigung“ |

Hier eine Beispielantwort:

```http
HTTP/1.1 202 Accepted
Content-Length: 923
Content-Type: application/json; charset=utf-8
Location: https://{host}/webhookextensions/handler/DurableTaskExtension/instances/34ce9a28a6834d8492ce6a295f1a80e2?taskHub=DurableFunctionsHub&connection=Storage&code=XXX

{
    "id":"34ce9a28a6834d8492ce6a295f1a80e2",
    "statusQueryGetUri":"https://{host}/webhookextensions/handler/DurableTaskExtension/instances/34ce9a28a6834d8492ce6a295f1a80e2?taskHub=DurableFunctionsHub&connection=Storage&code=XXX",
    "sendEventPostUri":"https://{host}/webhookextensions/handler/DurableTaskExtension/instances/34ce9a28a6834d8492ce6a295f1a80e2/raiseEvent/{eventName}?taskHub=DurableFunctionsHub&connection=Storage&code=XXX",
    "terminatePostUri":"https://{host}/webhookextensions/handler/DurableTaskExtension/instances/34ce9a28a6834d8492ce6a295f1a80e2/terminate?reason={text}&taskHub=DurableFunctionsHub&connection=Storage&code=XXX"
}
```
> [!NOTE]
> Das Format der Webhook-URLs kann in Abhängigkeit davon variieren, welche Version des Azure Functions-Hosts Sie ausführen. Das obige Beispiel gilt für den Azure Functions 2.0-Host.

## <a name="async-operation-tracking"></a>Nachverfolgen von asynchronen Vorgängen

Die oben erwähnte HTTP-Antwort wurde als Hilfe bei der Implementierung von asynchronen HTTP-APIs mit langer Ausführungsdauer per Durable Functions konzipiert. Dies wird auch als *Polling Consumer Pattern* (Consumerabrufmuster) bezeichnet. Der Client/Server-Datenfluss funktioniert wie folgt:

1. Der Client sendet eine HTTP-Anforderung, um einen Prozess mit langer Ausführungsdauer zu starten, z.B. eine Orchestratorfunktion.
2. Der HTTP-Zieltrigger gibt eine HTTP 202-Antwort mit dem Header `Location` und dem Wert `statusQueryGetUri` zurück.
3. Der Client fragt die URL im Header `Location` ab. Er erhält weiterhin die HTTP 202-Antworten mit dem Header `Location`.
4. Wenn die Instanz abgeschlossen (oder fehlgeschlagen) ist, gibt der Endpunkt im Header `Location` die Antwort „HTTP 200“ zurück.

Dieses Protokoll ermöglicht die Koordination von Prozessen mit langer Ausführungsdauer mit externen Clients oder Diensten, die das Abfragen eines HTTP-Endpunkts unterstützen, und das Beachten des Headers `Location`. Die grundlegenden Teile sind bereits in die Durable Functions-HTTP-APIs integriert.

> [!NOTE]
> Standardmäßig unterstützen alle HTTP-basierten Aktionen, die von [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/) bereitgestellt werden, das Standardmuster für asynchrone Vorgänge. Dies ermöglicht das Einbetten eines Durable Functions-Elements als Teil eines Logic Apps-Workflows. Weitere Informationen zur Unterstützung von Logik-Apps für asynchrone HTTP-Muster finden Sie im Artikel [Workflowaktionen und -trigger für Azure Logic Apps](../logic-apps/logic-apps-workflow-actions-triggers.md#asynchronous-patterns).

## <a name="http-api-reference"></a>HTTP-API-Referenz

Alle HTTP-APIs, die von der Erweiterung implementiert werden, verwenden die folgenden Parameter. Alle Parameter haben den Datentyp `string`.

| Parameter  | Parametertyp  | Beschreibung |
|------------|-----------------|-------------|
| instanceId | URL             | ID der Orchestrierungsinstanz |
| taskHub    | Abfragezeichenfolge    | Der Name des [Aufgabenhub](durable-functions-task-hubs.md). Wenn er nicht angegeben ist, wird der Name des Aufgabenhub der aktuellen Funktionen-App verwendet. |
| connection | Abfragezeichenfolge    | Der **Name** der Verbindungszeichenfolge für das Speicherkonto. Wenn nichts angegeben ist, wird die Standardverbindungszeichenfolge für die Funktionen-App genutzt. |
| systemKey  | Abfragezeichenfolge    | Der Autorisierungsschlüssel, der zum Aufrufen der API erforderlich ist. |

`systemKey` ist ein Autorisierungsschlüssel, der vom Azure Functions-Host automatisch generiert wird. Er gewährt spezifischen Zugriff auf die APIs der Erweiterung „Durable Task“ und kann genauso wie [andere Autorisierungsschlüssel](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Key-management-API) verwaltet werden. Die einfachste Methode zum Ermitteln des `systemKey`-Werts ist die Verwendung der oben erwähnten `CreateCheckStatusResponse`-API.

In den nächsten Abschnitten werden die spezifischen HTTP-APIs behandelt, die von der Erweiterung unterstützt werden, und es sind Anwendungsbeispiele vorhanden.

### <a name="get-instance-status"></a>Abrufen des Instanzstatus (Get instance status)

Dient zum Abrufen des Status einer angegebenen Orchestrierungsinstanz.

#### <a name="request"></a>Anforderung

Für Functions 1.0 lautet das Anforderungsformat wie folgt:

```http
GET /admin/extensions/DurableTaskExtension/instances/{instanceId}?taskHub={taskHub}&connection={connection}&code={systemKey}
```

Das Functions 2.0-Format verfügt über die gleichen Parameter, aber es gibt eine leichte Abweichung beim URL-Präfix:

```http
GET /webhookextensions/handler/DurableTaskExtension/instances/{instanceId}?taskHub={taskHub}&connection={connection}&code={systemKey}
```

#### <a name="response"></a>Antwort

Es können mehrere mögliche Statuscodewerte zurückgegeben werden.

* **HTTP 200 (OK)**: Die angegebene Instanz befindet sich im Status „Completed“ (Abgeschlossen).
* **HTTP 202 (Accepted)**: Die angegebene Instanz befindet sich in der Bearbeitung.
* **HTTP 400 (Bad Request)**: Für die angegebene Instanz ist ein Fehler aufgetreten, oder sie wurde beendet.
* **HTTP 404 (Not Found)**: Die angegebene Instanz ist nicht vorhanden, oder die Ausführung wurde noch nicht gestartet.

Die Antwortnutzlast für die Fälle **HTTP 200** und **HTTP 202** ist ein JSON-Objekt mit den folgenden Feldern:

| Feld           | Datentyp | Beschreibung |
|-----------------|-----------|-------------|
| runtimeStatus   | string    | Der Laufzeitstatus der Instanz. Mögliche Werte sind *Running*, *Pending*, *Failed*, *Canceled*, *Terminated* und *Completed*. |
| input           | JSON      | Die JSON-Daten, die zum Initialisieren der Instanz verwendet werden. |
| output          | JSON      | Die JSON-Ausgabe der Instanz. Dieses Feld ist `null`, wenn die Instanz nicht den Status „Completed“ (Abgeschlossen) aufweist. |
| createdTime     | string    | Der Zeitpunkt, zu dem die Instanz erstellt wurde. Es wird die erweiterte ISO 8601-Notation verwendet. |
| lastUpdatedTime | string    | Der Zeitpunkt, zu dem die Instanz zuletzt persistent gemacht wurde. Es wird die erweiterte ISO 8601-Notation verwendet. |

Hier ist ein Beispiel für eine Antwortnutzlast angegeben (zur besseren Lesbarkeit formatiert):

```json
{
  "runtimeStatus": "Completed",
  "input": null,
  "output": [
    "Hello Tokyo!",
    "Hello Seattle!",
    "Hello London!"
  ],
  "createdTime": "2017-10-06T18:30:24Z",
  "lastUpdatedTime": "2017-10-06T18:30:30Z"
}
```

Die Antwort **HTTP 202** enthält auch den Antwortheader **Location**, in dem auf die gleiche URL wie im oben erwähnten Feld `statusQueryGetUri` verwiesen wird.

### <a name="raise-event"></a>Auslösen eines Ereignisses (Raise event)

Sendet eine Ereignisbenachrichtigung an eine ausgeführte Orchestrierungsinstanz.

#### <a name="request"></a>Anforderung

Für Functions 1.0 lautet das Anforderungsformat wie folgt:

```http
POST /admin/extensions/DurableTaskExtension/instances/{instanceId}/raiseEvent/{eventName}?taskHub=DurableFunctionsHub&connection={connection}&code={systemKey}
```

Das Functions 2.0-Format verfügt über die gleichen Parameter, aber es gibt eine leichte Abweichung beim URL-Präfix:

```http
POST /webhookextensions/handler/DurableTaskExtension/instances/{instanceId}/raiseEvent/{eventName}?taskHub=DurableFunctionsHub&connection={connection}&code={systemKey}
```

Anforderungsparameter für diese API enthalten den bereits erwähnten Standardsatz sowie die folgenden eindeutigen Parameter:

| Feld       | Parametertyp  | Datentyp | Beschreibung |
|-------------|-----------------|-----------|-------------|
| eventName   | URL             | string    | Der Name des Ereignisses, das die Zielorchestrierungsinstanz erwartet. |
| {content}   | Inhalt anfordern | JSON      | Ereignisnutzlast in JSON-Formatierung |

#### <a name="response"></a>Antwort

Es können mehrere mögliche Statuscodewerte zurückgegeben werden.

* **HTTP 202 (Accepted)**: Das ausgelöste Ereignis wurde zur Verarbeitung akzeptiert.
* **HTTP 400 (Bad request)**: Der Anforderungsinhalt hatte nicht den Typ `application/json` oder war kein gültiger JSON-Code.
* **HTTP 404 (Not Found)**: Die angegebene Instanz wurde nicht gefunden.
* **HTTP 410 (Gone)**: Die angegebene Instanz wurde abgeschlossen, oder es ist ein Fehler aufgetreten, sodass keine ausgelösten Ereignisse verarbeitet werden können.

Hier ist eine Beispielanforderung angegeben, bei der die JSON-Zeichenfolge `"incr"` an eine Instanz gesendet wird, die auf ein Ereignis mit dem Namen **operation** (aus dem [Counter](durable-functions-counter.md)-Beispiel) wartet:

```
POST /admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a/raiseEvent/operation?taskHub=DurableFunctionsHub&connection=Storage&code=XXX
Content-Type: application/json
Content-Length: 6

"incr"
```

Die Antworten für diese API enthalten keinen Inhalt.

### <a name="terminate-instance"></a>Beenden der Instanz (Terminate instance)

Dient zum Beenden einer ausgeführten Orchestrierungsinstanz.

#### <a name="request"></a>Anforderung

Für Functions 1.0 lautet das Anforderungsformat wie folgt:

```http
DELETE /admin/extensions/DurableTaskExtension/instances/{instanceId}/terminate?reason={reason}&taskHub={taskHub}&connection={connection}&code={systemKey}
```

Das Functions 2.0-Format verfügt über die gleichen Parameter, aber es gibt eine leichte Abweichung beim URL-Präfix:

```http
DELETE /webhookextensions/handler/DurableTaskExtension/instances/{instanceId}/terminate?reason={reason}&taskHub={taskHub}&connection={connection}&code={systemKey}
```

Anforderungsparameter für diese API enthalten den bereits erwähnten Standardsatz sowie die folgenden eindeutigen Parameter:

| Feld       | Parametertyp  | Datentyp | Beschreibung |
|-------------|-----------------|-----------|-------------|
| reason      | Abfragezeichenfolge    | string    | Optional. Gibt den Grund für die Beendigung der Orchestrierungsinstanz an. |

#### <a name="response"></a>Antwort

Es können mehrere mögliche Statuscodewerte zurückgegeben werden.

* **HTTP 202 (Accepted)**: Die Beendigungsanforderung wurde zur Verarbeitung akzeptiert.
* **HTTP 404 (Not Found)**: Die angegebene Instanz wurde nicht gefunden.
* **HTTP 410 (Gone)**: Die angegebene Instanz wurde abgeschlossen, oder es ist ein Fehler aufgetreten.

Hier ist eine Beispielanforderung angegeben, mit der eine ausgeführte Instanz beendet und als Grund **buggy** (fehlerhaft) angegeben wird:

```
DELETE /admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a/terminate?reason=buggy&taskHub=DurableFunctionsHub&connection=Storage&code=XXX
```

Die Antworten für diese API enthalten keinen Inhalt.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Informationen zur Fehlerbehandlung](durable-functions-error-handling.md)
