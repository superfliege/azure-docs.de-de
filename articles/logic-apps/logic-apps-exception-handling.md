---
title: "Fehler- und Ausnahmebehandlung für Logic Apps in Azure | Microsoft-Dokumentation"
description: "Muster für die Behandlung von Fehlern und Ausnahmen in Logic Apps."
services: logic-apps
documentationcenter: .net,nodejs,java
author: derek1ee
manager: anneta
editor: 
ms.assetid: e50ab2f2-1fdc-4d2a-be40-995a6cc5a0d4
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: LADocs; deli
ms.openlocfilehash: a74c7d18306359c9152f139299de1208b5932fe5
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/20/2017
---
# <a name="handle-errors-and-exceptions-in-logic-apps"></a>Behandeln von Fehlern und Ausnahmen in Logic Apps

Logic Apps in Azure verfügen über umfassende Tools und Muster, damit Sie sicherstellen können, dass Ihre Integrationen robust und widerstandsfähig gegen Fehler sind. Für alle Integrationsarchitekturen muss dafür gesorgt werden, dass Ausfälle oder Probleme abhängiger Systeme angemessen behandelt werden. Mit Logic Apps wird das Behandeln von Fehlern zu einer erstklassigen Erfahrung. Sie bieten Ihnen die Tools, die Sie brauchen, um bei Ausnahmen und Fehlern in Ihrer Workflows aktiv zu werden.

## <a name="retry-policies"></a>Wiederholungsrichtlinien

Eine Wiederholungsrichtlinie ist die einfachste Form der Ausnahme- und Fehlerbehandlung. Wenn bei der ersten Anforderung ein Timeout oder ein Fehler auftritt (Anforderung mit der Antwort 429 oder 5xx), wird mit dieser Richtlinie definiert, ob und wie ein Wiederholungsversuch für die Aktion durchgeführt werden soll. 

Es gibt vier Arten von Wiederholungsrichtlinien: Standard, Keine, Festes Intervall und Exponentielles Intervall. Wenn in der Workflowdefinition keine Wiederholungsrichtlinie angegeben ist, wird die Standardrichtlinie verwendet, die vom Dienst definiert wird. 

Sie können Wiederholungsrichtlinien in den *Eingaben* für eine bestimmte Aktion oder für einen bestimmten Trigger konfigurieren, wenn sie wiederholbar ist. In ähnlicher Weise können Sie Wiederholungsrichtlinien im Logik-App-Designer konfigurieren. Um eine Wiederholungsrichtlinie im Logik-App-Designer einzurichten, wechseln Sie zu den **Einstellungen** für eine bestimmte Aktion.

Informationen zu den Einschränkungen von Wiederholungsrichtlinien finden Sie unter [Logic Apps-Grenzwerte und -Konfiguration](../logic-apps/logic-apps-limits-and-config.md). Weitere Informationen zur unterstützten Syntax finden Sie im [Abschnitt „Wiederholungsrichtlinie“ unter „Workflow-Aktionen und -Trigger“][retryPolicyMSDN].

### <a name="default"></a>Standard

Wenn Sie keine Wiederholungsrichtlinie definieren (**retryPolicy** ist nicht definiert), wird die Standardrichtlinie verwendet. Die Standardrichtlinie ist eine Richtlinie mit exponentiellem Intervall, die bis zu vier Wiederholungsversuche mit exponentiell wachsenden Intervallen sendet, die um 7,5 Sekunden skaliert sind. Das Intervall ist auf den Bereich zwischen 5 und 45 Sekunden beschränkt. Diese Standardrichtlinie ist äquivalent zur Richtlinie in dieser Beispieldefinition eines HTTP-Workflows:

```json
"HTTP":
{
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
    "runAfter": {},
    "type": "Http"
}
```

### <a name="none"></a>Keine

Wenn **retryPolicy** auf **Keine** festgelegt ist, wird für eine Anforderung mit Fehler kein Wiederholungsversuch ausgeführt.

| Elementname | Erforderlich | Typ | BESCHREIBUNG |
| ------------ | -------- | ---- | ----------- |
| type | Ja | Zeichenfolge | **keine** |

### <a name="fixed-interval"></a>Festes Intervall

Wenn **retryPolicy** auf **Fest** festgelegt ist, führt die Richtlinie einen Wiederholungsversuch für eine Anforderung mit Fehler aus, indem sie das angegebene Zeitintervall wartet, bevor sie die nächste Anforderung sendet.

| Elementname | Erforderlich | Typ | BESCHREIBUNG |
| ------------ | -------- | ---- | ----------- |
| type | Ja | Zeichenfolge | **Fest** |
| count | Ja | Ganze Zahl  | Anzahl der Wiederholungsversuche. Der Grenzwert muss zwischen 1 und 90 liegen. |
| interval | Ja | Zeichenfolge | Wiederholungsintervall im [ISO 8601-Format](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations). Muss zwischen PT5S und PT1D liegen. |

### <a name="exponential-interval"></a>Exponentielles Intervall

Wenn **retryPolicy** auf **Exponentiell** festgelegt ist, führt die Richtlinie einen Wiederholungsversuch für eine Anforderung mit Fehler nach einem zufälligen Zeitintervall aus einem exponentiell anwachsenden Bereich aus. Für jeden Wiederholungsversuch ist dabei garantiert, dass er in einem Zufallsintervall gesendet wird, das größer als **MinimumIntervall** und kleiner als **MaximumIntervall** ist. Für jeden Wiederholungsversuch bis zum Erreichen von **count** einschließlich wird eine uniforme Zufallsvariable im Bereich generiert, wie in der folgenden Tabelle angegeben:

**Bereich der Zufallsvariablen**

| Wiederholungsanzahl | Minimales Intervall | Maximales Intervall |
| ------------ |  ------------ |  ------------ |
| 1 | Max(0, **MinimumIntervall**) | Min(Intervall, **MaximumIntervall**) |
| 2 | Max(Intervall, **MinimumIntervall**) | Min(2 * Intervall, **MaximumIntervall**) |
| 3 | Max(2 * Intervall, **MinimumIntervall**) | Min(4 * Intervall, **MaximumIntervall**) |
| 4 | Max(4 * Intervall, **MinimumIntervall**) | Min(8 * Intervall, **MaximumIntervall**) |
| ... |

Für Richtlinien vom exponentiellen Typ sind **count** und **interval** erforderlich. Werte für **MinimumInterval** und **MaximumInterval** sind optional. Sie können sie hinzufügen, um die Standardwerte PT5S bzw. PT1D zu überschreiben.

| Elementname | Erforderlich | Typ | BESCHREIBUNG |
| ------------ | -------- | ---- | ----------- |
| type | Ja | Zeichenfolge | **exponential** |
| count | Ja | Ganze Zahl  | Anzahl der Wiederholungsversuche. Der Grenzwert muss zwischen 1 und 90 liegen.  |
| interval | Ja | Zeichenfolge | Wiederholungsintervall im [ISO 8601-Format](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations). Muss zwischen PT5S und PT1D liegen. |
| MinimumIntervall | Nein  | Zeichenfolge | Minimum-Wiederholungsintervall im [ISO 8601-Format](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations). Muss zwischen PT5S und **interval** liegen. |
| Maximumintervall | Nein  | Zeichenfolge | Minimum-Wiederholungsintervall im [ISO 8601-Format](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations). Muss zwischen **interval** und PT1D liegen. |

## <a name="catch-failures-with-the-runafter-property"></a>Abfangen von Fehlern mit der runAfter-Eigenschaft

Für jede Logik-App wird deklariert, welche Aktionen beendet werden müssen, bevor die Aktion startet. Das ist so ähnlich wie das Festlegen der Schritte in Ihrem Workflow. In der Aktionsdefinition wird diese Sortierung als **runAfter**-Eigenschaft bezeichnet. 

Die **runAfter**-Eigenschaft ist ein Objekt, mit dem beschrieben wird, von welchen Aktionen und Aktionsstatus die Aktion ausgeführt wird. Standardmäßig ist für alle Aktionen, die Sie mithilfe des Logik-App-Designers hinzugefügt haben, die Ausführung nach dem vorhergehenden Schritt festgelegt, wenn das Ergebnis des vorhergehenden Schritts **Erfolgreich** ist. 

Jedoch können Sie den Wert von **runAfter** so anpassen, dass Aktionen ausgelöst werden, wenn die vorhergehenden Aktionen **Fehler**, **Übersprungen** oder eine mögliche Menge dieser Werte als Ergebnis aufweisen. Wenn Sie einem bestimmen Azure Service Bus-Thema ein Element hinzufügen möchten, nachdem bei einer bestimmten **Insert_Row**-Aktion ein Fehler aufgetreten ist, könnten Sie die folgende **runAfter**-Konfiguration verwenden:

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

Beachten Sie, dass **runAfter** darauf festgelegt ist, auszulösen, wenn das Ergebnis der **Insert_Row**-Aktion **Fehler** ist. Um die Aktion auszuführen, wenn der Status der Aktion **Erfolgreich**, **Fehler** oder **Übersprungen** ist, verwenden Sie diese Syntax:

```json
"runAfter": {
        "Insert_Row": [
            "Failed", "Succeeded", "Skipped"
        ]
    }
```

> [!TIP]
> Aktionen, die nach einem Fehler bei einer vorhergehenden Aktion ausgeführt und erfolgreich abgeschlossen werden, werden als **Erfolgreich** gekennzeichnet. Dies bedeutet, dass in dem Fall, dass Sie alle Fehler in einem Workflow erfolgreich abfangen, die Ausführung ihrerseits als **Erfolgreich** gekennzeichnet wird.

## <a name="scopes-and-results-to-evaluate-actions"></a>Bereiche und Ergebnisse zum Auswerten von Aktionen

Sie können Aktionen innerhalb eines [Bereichs](../logic-apps/logic-apps-loops-and-scopes.md) gruppieren, ähnlich der Weise, in der die Ausführung nach einzelnen Aktionen erfolgt. Ein Bereich fungiert als logische Gruppierung von Aktionen. 

Bereiche sind nützlich, um Logik-App-Aktionen zu organisieren und Gesamtauswertungen zum Status eines Bereichs durchzuführen. Der Bereich selbst erhält einen Status, nachdem alle Aktionen eines Bereichs abgeschlossen wurden. Der Bereichsstatus wird mit den gleichen Kriterien wie bei einer Ausführung ermittelt. Wenn die abschließende Aktion in einem Ausführungsbranch mit **Fehler** oder **Abgebrochen** endet, ist der Status **Fehler**.

Um bestimmte Aktionen für Fehler auszulösen, die innerhalb des Bereichs aufgetreten sind, können Sie **runAfter** mit einem Bereich verwenden, der als **Fehler** gekennzeichnet ist. Bei einem Fehler einer *beliebigen* Aktion im Bereich können Sie eine einzelne Aktion zum Abfangen von Fehlern erstellen, indem Sie **runAfter** für einen Bereich verwenden.

### <a name="get-the-context-of-failures-with-results"></a>Abrufen des Kontexts von Fehlern mit Ergebnissen

Das Abfangen der Fehler eines Bereichs ist nützlich. Aber häufig ist auch der Kontext hilfreich, um genau zu verstehen, für welche Aktionen Fehler aufgetreten sind und welche Fehler oder Statuscodes zurückgegeben wurden. Die **@result()**-Workflowfunktion stellt Kontext zum Ergebnis aller Aktionen in einem Bereich zur Verfügung.

Die **@result()**-Funktion nimmt einen einzelnen Parameter (Bereichsname) an und gibt ein Array mit den Ergebnissen aller Aktionen innerhalb des betreffenden Bereichs zurück. Diese Aktionsobjekte enthalten die gleichen Attribute wie das **@actions()**-Objekt, z.B. Aktionsstartzeit, Aktionsendzeit, Aktionsstatus, Aktionseingaben, Korrelations-IDs der Aktion und Aktionsausgaben. 

Sie können eine **@result()**-Funktion problemlos mit einer **runAfter**-Eigenschaft koppeln, um Kontext zu allen Aktionen zu senden, für die im Bereich ein Fehler aufgetreten ist.

Zum Ausführen einer Aktion *für jede* Aktion eines Bereichs mit **Fehler** als Ergebnis und zum Filtern des Arrays der Ergebnisse nach fehlgeschlagenen Aktionen können Sie **@result()** mit einer [Filter_array](../connectors/connectors-native-query.md)-Aktion und einer [ForEach](../logic-apps/logic-apps-loops-and-scopes.md)-Schleife koppeln. Mit dem gefilterten Ergebnisarray können Sie eine Aktion für jeden Fehler ausführen, indem Sie die **ForEach**-Schleife verwenden. 

In diesem Beispiel wird eine HTTP POST-Anforderung mit dem Antworttext aller Aktionen gesendet, für die im Bereich My_Scope ein Fehler aufgetreten ist:

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

In dieser ausführlichen exemplarischen Vorgehensweise wird beschrieben, was im vorstehenden Beispiel geschieht:

1. Um das Ergebnis aller Aktionen in My_Scope abzurufen, filtert die **Filter_array**-Aktion **@result('My_Scope')**.

2. Die Bedingung für **Filter_array** ist ein beliebiges **@result()**-Element, das den Status **Fehler** aufweist. Mit dieser Bedingung wird das Array nach allen Aktionsergebnissen aus My_Scope gefiltert, so dass sich ein Array ergibt, dass nur Fehleraktionsergebnisse enthält.

3. Führen Sie eine **ForEach**-Aktion für die Ausgaben des *gefilterten Arrays* aus. In diesem Schritt wird eine Aktion *für jedes* Ergebnis einer fehlgeschlagenen Aktion durchgeführt, das zuvor gefiltert wurde.

    Wenn für eine einzelne Aktion im Bereich ein Fehler aufgetreten ist, werden die Aktionen in der **ForEach**-Schleife nur einmal ausgeführt. Mehrere Aktionen mit Fehler lösen eine Aktion pro Fehler aus.

4. Für den **ForEach**-Elementantworttext wird ein HTTP POST oder **@item()['outputs']['body']** gesendet. Die **@result()**-Elementform ist gleich der **@actions()**-Form. Sie kann auf die gleiche Weise analysiert werden.

5. Binden Sie zwei benutzerdefinierte Header mit dem Namen der fehlgeschlagenen Aktion **@item()['name']** und der Clienttracking-ID der fehlgeschlagenen Ausführung **@item()['clientTrackingId']** ein.

Als Referenz finden Sie hier ein Beispiel für ein einzelnes **@result()**-Element. Es zeigt die Eigenschaften **name**, **body** und **clientTrackingId**, die im vorhergehenden Beispiel analysiert werden. Außerhalb einer **ForEach**-Aktion, gibt **@result()** ein Array dieser Objekte zurück.

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

Für andere Muster der Ausnahmebehandlung können Sie die weiter oben im Artikel beschriebenen Ausdrücke verwenden. Außerdem können Sie die Entscheidung treffen, eine einzelne Aktion für die Ausnahmebehandlung außerhalb des Bereichs durchzuführen, bei der das gesamte gefilterte Array mit Fehlern akzeptiert wird, und das **ForEach**-Element zu entfernen. Darüber hinaus können Sie noch andere nützliche Eigenschaften aus der **@result()**-Antwort einfügen, wie zuvor beschrieben.

## <a name="azure-diagnostics-and-telemetry"></a>Azure-Diagnose und Telemetrie

Die in diesem Artikel beschriebenen Muster eignen sich gut zum Behandeln von Fehlern und Ausnahmen in einer Ausführung, aber Sie können auch unabhängig von der eigentlichen Ausführung Fehler identifizieren und darauf reagieren. [Azure-Diagnose](../logic-apps/logic-apps-monitor-your-logic-apps.md) ist eine einfache Möglichkeit zum Senden aller Workflowereignisse (einschließlich aller Ausführungs- und Aktionsstatus) an ein Azure-Speicherkonto oder an einen Event Hub in Azure Event Hubs. 

Sie können die Protokolle und Metriken überwachen oder mit einem von Ihnen bevorzugten Überwachungstool veröffentlichen, um jeweils den Ausführungsstatus auszuwerten. Eine mögliche Option ist das Streamen aller Ereignisse durch Event Hubs nach [Stream Analytics](https://azure.microsoft.com/services/stream-analytics/). In Stream Analytics können Sie Liveabfragen auf der Grundlage von Anomalien, Mittelwerten oder Fehlern aus den Diagnoseprotokollen schreiben. Sie können Stream Analytics zum Senden von Informationen an andere Datenquellen verwenden, etwa an Warteschlangen, Themen SQL, Azure Cosmos DB oder Power BI.

## <a name="next-steps"></a>Nächste Schritte

* [Szenario: Ausnahmebehandlung und Fehlerprotokollierung für Logik-Apps in Azure](../logic-apps/logic-apps-scenario-error-and-exception-handling.md).
* [Beispiele und häufige Szenarios für Logik-Apps](../logic-apps/logic-apps-examples-and-scenarios.md)
* [Erstellen von Vorlagen für die Bereitstellungs- und Versionsverwaltung von Logik-Apps](../logic-apps/logic-apps-create-deploy-template.md)
* Informationen zum [Erstellen und Bereitstellen von Logik-Apps mit Visual Studio](logic-apps-deploy-from-vs.md)

<!-- References -->
[retryPolicyMSDN]: https://docs.microsoft.com/rest/api/logic/actions-and-triggers#Anchor_9
