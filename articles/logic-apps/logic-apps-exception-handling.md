---
title: Fehler- und Ausnahmebehandlung für Logic Apps in Azure | Microsoft-Dokumentation
description: Muster für die Behandlung von Fehlern und Ausnahmen in Logic Apps.
services: logic-apps
documentationcenter: ''
author: dereklee
manager: anneta
editor: ''
ms.assetid: e50ab2f2-1fdc-4d2a-be40-995a6cc5a0d4
ms.service: logic-apps
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: logic-apps
ms.date: 01/31/2018
ms.author: deli; LADocs
ms.openlocfilehash: 2ae4f0ae9782ada23089d364e8a1700144ef5ff7
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2018
---
# <a name="handle-errors-and-exceptions-in-logic-apps"></a>Behandeln von Fehlern und Ausnahmen in Logic Apps

Die angemessene Behandlung von Ausfällen oder Problemen abhängiger Systeme kann für alle Integrationsarchitekturen eine Herausforderung darstellen. Zum Erstellen stabiler, gegenüber Problemen und Fehlern widerstandsfähiger Integrationen bietet Logic Apps eine erstklassige Erfahrung bei der Behandlung von Fehlern und Ausnahmen. 

## <a name="retry-policies"></a>Wiederholungsrichtlinien

Für die einfachste Ausnahme- und Fehlerbehandlung können Sie die Wiederholungsrichtlinie verwenden. Mit dieser Richtlinie wird definiert, ob und wie die Aktion erneut versucht, die Anforderung auszuführen, wenn bei der ersten Anforderung ein Timeout oder ein Fehler aufgetreten ist (jede Anforderung mit der Antwort 429 oder 5xx). 

Es gibt vier Arten von Wiederholungsrichtlinien: Standard, Keine, Festes Intervall und Exponentielles Intervall. Wenn in Ihrer Workflowdefinition keine Wiederholungsrichtlinie angegeben ist, wird stattdessen die vom Dienst definierte Standardrichtlinie verwendet.

Öffnen Sie ggf. zum Einrichten von Wiederholungsrichtlinien den Logik-App-Designer für Ihre Logik-App, und wechseln Sie für eine bestimmte Aktion in der Logik-App zu **Einstellungen**. Alternativ können Sie Wiederholungsrichtlinien für eine bestimmte Aktion oder einen Trigger, wenn wiederholbar, im **inputs**-Abschnitt Ihrer Workflowdefinition definieren. Die allgemeine Syntax sieht so aus:

```json
"retryPolicy": {
    "type": "<retry-policy-type>",
    "interval": <retry-interval>,
    "count": <number-of-retry-attempts>
}
```

Weitere Informationen zu Syntax und **inputs**-Abschnitt finden Sie unter [„Trigger und Aktionen für Logik-App-Workflows“ im Abschnitt zur Wiederholungsrichtlinie][retryPolicyMSDN]. Informationen zu Einschränkungen von Wiederholungsrichtlinien finden Sie unter [Logic Apps-Grenzwerte und -Konfiguration](../logic-apps/logic-apps-limits-and-config.md). 

### <a name="default"></a>Standard

Wenn Sie im Abschnitt **RetryPolicy** keine Wiederholungsrichtlinie definieren, verwendet Ihre Logik-App die Standardrichtlinie, eine [Richtlinie für ein exponentielles Intervall ](#exponential-interval), die bis zu vier Wiederholungsversuche in exponentiell zunehmenden Intervallen sendet, die mit 7,5 Sekunden skaliert werden. Das Intervall ist auf den Bereich zwischen 5 und 45 Sekunden beschränkt. Diese Richtlinie ist äquivalent zur Richtlinie in dieser Beispieldefinition eines HTTP-Workflows:

```json
"HTTP": {
    "type": "Http",
    "inputs": {
        "method": "GET",
        "uri": "http://myAPIendpoint/api/action",
        "retryPolicy" : {
            "type": "exponential",
            "count": 4,
            "interval": "PT7.5S",
            "minimumInterval": "PT5S",
            "maximumInterval": "PT45S"
        }
    },
    "runAfter": {}
}
```

### <a name="none"></a>Keine

Wenn Sie **retryPolicy** auf **Keine** festlegen, initiiert diese Richtlinie keine Wiederholung fehlerhafter Anforderungen.

| Elementname | Erforderlich | Typ | BESCHREIBUNG | 
| ------------ | -------- | ---- | ----------- | 
| type | Ja | Zeichenfolge | **keine** | 
||||| 

### <a name="fixed-interval"></a>Festes Intervall

Wenn Sie **retryPolicy** auf **Fest** festlegen, führt die Richtlinie einen Wiederholungsversuch für eine Anforderung mit Fehler aus, indem sie das angegebene Zeitintervall wartet, bevor sie die nächste Anforderung sendet.

| Elementname | Erforderlich | Typ | BESCHREIBUNG |
| ------------ | -------- | ---- | ----------- |
| type | Ja | Zeichenfolge | **Fest** |
| count | Ja | Ganze Zahl  | Die Anzahl der Wiederholungsversuche, die zwischen 1 und 90 liegen muss | 
| interval | Ja | Zeichenfolge | Das Wiederholungsintervall im [ISO 8601-Format](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations), das zwischen PT5S und PT1D liegen muss | 
||||| 

<a name="exponential-interval"></a>

### <a name="exponential-interval"></a>Exponentielles Intervall

Wenn Sie **retryPolicy** auf **Exponentiell** festlegen, führt die Richtlinie einen Wiederholungsversuch für eine Anforderung mit Fehler nach einem zufälligen Zeitintervall aus einem exponentiell anwachsenden Bereich aus. Die Richtlinie garantiert auch, dass jeder Wiederholungsversuch in einem Zufallsintervall gesendet wird, das größer als **minimumIntervall** und kleiner als **maximumIntervall** ist. Exponentielle Richtlinien erfordern **count** und **intervall**, während Werte für **minimumInterval** und **maximumInterval** optional sind. Wenn Sie den Standardwert PT5S bzw. PT1D überschreiben möchten, können Sie diese Werte hinzufügen.

| Elementname | Erforderlich | Typ | BESCHREIBUNG |
| ------------ | -------- | ---- | ----------- |
| type | Ja | Zeichenfolge | **exponential** |
| count | Ja | Ganze Zahl  | Die Anzahl der Wiederholungsversuche, die zwischen 1 und 90 liegen muss  |
| interval | Ja | Zeichenfolge | Das Wiederholungsintervall im [ISO 8601-Format](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations), das zwischen PT5S und PT1D liegen muss. |
| MinimumIntervall | Nein  | Zeichenfolge | Das minimale Wiederholungsintervall im [ISO 8601-Format](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations), das zwischen PT5S und **interval** liegen muss. |
| Maximumintervall | Nein  | Zeichenfolge | Das minimale Wiederholungsintervall im [ISO 8601-Format](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations), das zwischen **interval** und PT1D liegen muss. | 
||||| 

Für jeden Wiederholungsversuch bis zum Erreichen von **count** einschließlich wird eine uniforme Zufallsvariable im angegebenen Bereich generiert, wie in der folgenden Tabelle angegeben:

**Bereich der Zufallsvariablen**

| Wiederholungsanzahl | Minimales Intervall | Maximales Intervall |
| ------------ | ---------------- | ---------------- |
| 1 | Max(0, **MinimumIntervall**) | Min(Intervall, **MaximumIntervall**) |
| 2 | Max(Intervall, **MinimumIntervall**) | Min(2 * Intervall, **MaximumIntervall**) |
| 3 | Max(2 * Intervall, **MinimumIntervall**) | Min(4 * Intervall, **MaximumIntervall**) |
| 4 | Max(4 * Intervall, **MinimumIntervall**) | Min(8 * Intervall, **MaximumIntervall**) |
| ... | | | 
|||| 

## <a name="catch-and-handle-failures-with-the-runafter-property"></a>Abfangen und Behandeln von Fehlern mit der RunAfter-Eigenschaft

Jede Logik-App-Aktion deklariert die Aktionen, die beendet werden müssen, bevor diese Aktion startet, ähnlich wie Sie die Reihenfolge der Schritte in Ihrem Workflow angeben. In einer Aktionsdefinition definiert die **runAfter**-Eigenschaft diese Reihenfolge und ist ein Objekt, mit dem beschrieben wird, von welchen Aktionen und Aktionsstatus die Aktion ausgeführt wird.

Standardmäßig ist für alle Aktionen, die im Logik-App-Designer hinzugefügt haben, die Ausführung nach dem vorhergehenden Schritt festgelegt, wenn das Ergebnis des vorhergehenden Schritts **Erfolgreich** ist. Sie können jedoch den **runAfter**-Wert so anpassen, dass Aktionen ausgelöst werden, wenn die vorherigen Aktionen zu dem Ergebnis **Fehlerhaft**, **Übersprungen** oder einer Kombination dieser Werte führen. Um z.B. ein Element einem bestimmten Service Bus-Thema hinzuzufügen, nachdem bei einer bestimmten **Insert_Row**-Aktion ein Fehler aufgetreten ist, könnten Sie diese **runAfter**-Beispieldefinition verwenden:

```json
"Send_message": {
    "inputs": {
        "body": {
            "ContentData": "@{encodeBase64(body('Insert_Row'))}",
            "ContentType": "{ \"content-type\" : \"application/json\" }"
        },
        "host": {
            "api": {
                "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/servicebus"
            },
            "connection": {
                "name": "@parameters('$connections')['servicebus']['connectionId']"
            }
        },
        "method": "post",
        "path": "/@{encodeURIComponent('failures')}/messages"
    },
    "runAfter": {
        "Insert_Row": [
            "Failed"
        ]
    }
}
```

Die Ausführung der **runAfter**-Eigenschaft wird für den Fall festgelegt, dass die **Insert_Row**-Aktion den Status **Fehlerhaft** hat. Um die Aktion auszuführen, wenn der Status der Aktion **Erfolgreich**, **Fehler** oder **Übersprungen** ist, verwenden Sie diese Syntax:

```json
"runAfter": {
        "Insert_Row": [
            "Failed", "Succeeded", "Skipped"
        ]
    }
```

> [!TIP]
> Aktionen, die nach einem Fehler bei einer vorhergehenden Aktion ausgeführt und erfolgreich abgeschlossen werden, werden als **Erfolgreich** gekennzeichnet. Dies bedeutet Folgendes: Wenn das Abfangen aller Fehler eines Workflows erfolgreich ist, wird die gesamte Ausführung selbst als **Erfolgreich** gekennzeichnet.

<a name="scopes"></a>

## <a name="evaluate-actions-with-scopes-and-their-results"></a>Auswerten von Aktionen mit Bereichen und ihren Ergebnissen

Ähnlich wie beim Ausführen von Schritten nach einzelnen Aktionen mit der **runAfter**-Eigenschaft können Sie Aktionen innerhalb eines [Bereichs](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md) gruppieren. Sie können Bereiche verwenden, wenn Sie Aktionen logisch gruppieren, den Aggregatstatus des Bereichs feststellen und basierend auf diesem Status Aktionen ausführen möchten. Nachdem die Ausführung aller Aktionen in einem Bereich beendet ist, erhält der Bereich selbst seinen eigenen Status. 

Zum Überprüfen des Status eines Bereichs können Sie die gleichen Kriterien wie zum Überprüfen des Ausführungsstatus einer Logik-App verwenden, z.B. **Erfolgreich**, **Fehlerhaft** usw. 

Bei erfolgreicher Ausführung aller Aktionen des Bereichs wird der Status des Bereichs als **Erfolgreich** gekennzeichnet. Wenn die letzte Aktion in einem Bereich zu dem Ergebnis **Fehlerhaft** oder **Abgebrochen** führt, wird der Status des Bereichs als **Fehlerhaft** gekennzeichnet. 

Zum Abfangen von Ausnahmen in einem als **Fehlerhaft** gekennzeichneten Bereich und Ausführen von Aktionen, die diese Fehler behandeln, können Sie die **runAfter**-Eigenschaft für diesen **Fehlerhaften** Bereich verwenden. Auf diese Weise können Sie, wenn bei einer *beliebigen* Aktion im Bereich ein Fehler auftritt und Sie die **runAfter**-Eigenschaft für diesen Bereich verwenden, eine einzelne Aktion zum Abfangen von Fehlern erstellen.

Grenzwerte für Bereiche finden Sie unter [Grenzwerte und Konfiguration](../logic-apps/logic-apps-limits-and-config.md).

### <a name="get-context-and-results-for-failures"></a>Abrufen von Kontext und Ergebnissen auf Fehler

Das Abfangen der Fehler eines Bereichs ist nützlich. Aber häufig ist auch der Kontext hilfreich, um genau zu verstehen, für welche Aktionen Fehler aufgetreten sind und welche Fehler oder Statuscodes zurückgegeben wurden. Die **@result()**-Workflowfunktion stellt Kontext zum Ergebnis aller Aktionen in einem Bereich zur Verfügung.

Die **@result()**-Funktion nimmt einen einzelnen Parameter (Bereichsname) an und gibt ein Array mit den Ergebnissen aller Aktionen innerhalb des betreffenden Bereichs zurück. Diese Aktionsobjekte enthalten dieselben Attribute wie die  **@actions()**-Objekte, z.B. Start- und Endzeit der Aktion, Status, Eingaben, Korrelations-IDs und Ausgaben. Sie können eine **@result()**-Funktion problemlos mit einer **runAfter**-Eigenschaft koppeln, um Kontext zu allen Aktionen zu senden, für die im Bereich ein Fehler aufgetreten ist.

Zum Ausführen einer Aktion *für jede* Aktion eines Bereichs mit dem Ergebnis **Fehlerhaft** und zum Filtern des Arrays der Ergebnisse bis hinab zu den fehlerhaften Aktionen können Sie **@result()** mit einer **[Filter_array](../connectors/connectors-native-query.md)**-Aktion und einer **[ForEach](../logic-apps/logic-apps-control-flow-loops.md)**-Schleife koppeln. Sie können für das Array mit den gefilterten Ergebnissen eine Aktion für jeden Fehler durchführen, indem Sie die **ForEach**-Schleife verwenden. 

Hier ist ein Beispiel gefolgt von einer ausführlichen Erklärung angegeben, bei dem eine HTTP POST-Anforderung mit dem Antworttext aller fehlerhaften Aktionen im Bereich „My_Scope“ gesendet wird:

```json
"Filter_array": {
    "inputs": {
        "from": "@result('My_Scope')",
        "where": "@equals(item()['status'], 'Failed')"
    },
    "runAfter": {
        "My_Scope": [
            "Failed"
        ]
    },
    "type": "Query"
},
"For_each": {
    "actions": {
        "Log_Exception": {
            "inputs": {
                "body": "@item()['outputs']['body']",
                "method": "POST",
                "headers": {
                    "x-failed-action-name": "@item()['name']",
                    "x-failed-tracking-id": "@item()['clientTrackingId']"
                },
                "uri": "http://requestb.in/"
            },
            "runAfter": {},
            "type": "Http"
        }
    },
    "foreach": "@body('Filter_array')",
    "runAfter": {
        "Filter_array": [
            "Succeeded"
        ]
    },
    "type": "Foreach"
}
```

In dieser ausführlichen exemplarischen Vorgehensweise wird beschrieben, was in diesem Beispiel geschieht:

1. Um das Ergebnis aller Aktionen in „My_Scope“ abzurufen, filtert die **Filter Array**-Aktion **@result('My_Scope')**.

2. Die Bedingung für **Filter Array** ist ein beliebiges **@result()**-Element, das den Status **Fehlerhaft** aufweist. Mit dieser Bedingung wird das Array aller Aktionsergebnisse aus „My_Scope“ bis hinab zu einem Array gefiltert, dass nur fehlerhafte Aktionen als Ergebnisse enthält.

3. Führen Sie eine **For Each**-Schleifenaktion für die Ausgaben *gefilterter Arrays* aus. In diesem Schritt wird eine Aktion *für jedes* Ergebnis einer fehlgeschlagenen Aktion durchgeführt, das zuvor gefiltert wurde.

   Wenn für eine einzelne Aktion im Bereich ein Fehler aufgetreten ist, werden die Aktionen in der **ForEach**-Schleife nur einmal ausgeführt. 
   Mehrere Aktionen mit Fehler lösen eine Aktion pro Fehler aus.

4. Senden Sie ein HTTP POST für den **ForEach**-Elementantworttext, das **@item()['outputs']['body']** lautet. Die **@result()**-Elementform ist mit der **@actions()**-Form identisch und kann auch genauso analysiert werden.

5. Binden Sie zwei benutzerdefinierte Header mit dem Namen der fehlgeschlagenen Aktion **@item()['name']** und der Clienttracking-ID der fehlgeschlagenen Ausführung **@item()['clientTrackingId']** ein.

Zu Referenzzwecken ist hier ein Beispiel für ein einzelnes **@result()**-Element angegeben, das die Eigenschaften **name**, **body** und **clientTrackingId** enthält, die im vorherigen Beispiel analysiert werden. Außerhalb einer **ForEach**-Aktion gibt **@result()** ein Array dieser Objekte zurück.

```json
{
    "name": "Example_Action_That_Failed",
    "inputs": {
        "uri": "https://myfailedaction.azurewebsites.net",
        "method": "POST"
    },
    "outputs": {
        "statusCode": 404,
        "headers": {
            "Date": "Thu, 11 Aug 2016 03:18:18 GMT",
            "Server": "Microsoft-IIS/8.0",
            "X-Powered-By": "ASP.NET",
            "Content-Length": "68",
            "Content-Type": "application/json"
        },
        "body": {
            "code": "ResourceNotFound",
            "message": "/docs/folder-name/resource-name does not exist"
        }
    },
    "startTime": "2016-08-11T03:18:19.7755341Z",
    "endTime": "2016-08-11T03:18:20.2598835Z",
    "trackingId": "bdd82e28-ba2c-4160-a700-e3a8f1a38e22",
    "clientTrackingId": "08587307213861835591296330354",
    "code": "NotFound",
    "status": "Failed"
}
```

Sie können die oben in diesem Artikel beschriebenen Ausdrücke verwenden, um unterschiedliche Muster für die Ausnahmebehandlung zu nutzen. Außerdem können Sie die Entscheidung treffen, eine einzelne Aktion für die Ausnahmebehandlung außerhalb des Bereichs durchzuführen, bei der das gesamte gefilterte Array mit Fehlern akzeptiert wird, und die **ForEach**-Aktion zu entfernen. Darüber hinaus können Sie noch andere nützliche Eigenschaften aus der **@result()**-Antwort einfügen, wie zuvor beschrieben.

## <a name="azure-diagnostics-and-telemetry"></a>Azure-Diagnose und Telemetrie

Die oben beschriebenen Muster eignen sich gut zum Behandeln von Fehlern und Ausnahmen in einer Ausführung, aber Sie können auch unabhängig von der eigentlichen Ausführung Fehler identifizieren und darauf reagieren. 
[Azure-Diagnose](../logic-apps/logic-apps-monitor-your-logic-apps.md) ist eine einfache Möglichkeit zum Senden aller Workflowereignisse einschließlich aller Ausführungs- und Aktionsstatus an ein Azure Storage-Konto oder an einen mit Azure Event Hubs erstellten Event Hub. 

Sie können die Protokolle und Metriken überwachen oder mit einem von Ihnen bevorzugten Überwachungstool veröffentlichen, um jeweils den Ausführungsstatus auszuwerten. Eine mögliche Option ist das Streamen aller Ereignisse durch Event Hubs nach [Stream Analytics](https://azure.microsoft.com/services/stream-analytics/). In Stream Analytics können Sie Liveabfragen auf der Grundlage von Anomalien, Mittelwerten oder Fehlern aus den Diagnoseprotokollen schreiben. Sie können Stream Analytics zum Senden von Informationen an andere Datenquellen verwenden, etwa an Warteschlangen, Themen, SQL, Azure Cosmos DB oder Power BI.

## <a name="next-steps"></a>Nächste Schritte

* [Szenario: Ausnahmebehandlung und Fehlerprotokollierung für Logik-Apps](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
* [Beispiele und häufige Szenarios für Logik-Apps](../logic-apps/logic-apps-examples-and-scenarios.md)

<!-- References -->
[retryPolicyMSDN]: https://docs.microsoft.com/rest/api/logic/actions-and-triggers#Anchor_9