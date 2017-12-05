---
title: "Behandlung von Fehlern und Ausnahmen – Azure Logic Apps | Microsoft-Dokumentation"
description: "Enthält Muster für die Behandlung von Fehlern und Ausnahmen in Azure Logic Apps."
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: e50ab2f2-1fdc-4d2a-be40-995a6cc5a0d4
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: 4eb6f743479886374692eadcf218b77b4bfcc933
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/22/2017
---
# <a name="handle-errors-and-exceptions-in-azure-logic-apps"></a>Behandeln von Fehlern und Ausnahmen in Azure Logic Apps

Azure Logic Apps verfügen über umfassende Tools und Muster, damit Sie sicherstellen können, dass Ihre Integrationen robust und widerstandsfähig gegen Fehler sind. Für alle Integrationsarchitekturen muss dafür gesorgt werden, dass Ausfälle oder Probleme abhängiger Systeme richtig behandelt werden. Mit Logik-Apps wird die Behandlung von Fehlern deutlich vereinfacht, da Sie über die Tools verfügen, die Sie zum Reagieren auf Ausnahmen und Fehler in Ihren Workflows benötigen.

## <a name="retry-policies"></a>Wiederholungsrichtlinien

Eine Wiederholungsrichtlinie ist die einfachste Form der Ausnahme- und Fehlerbehandlung. Mit dieser Richtlinie wird definiert, ob und wie für die Aktion ein Wiederholungsversuch durchgeführt werden soll, wenn bei der ersten Anforderung ein Timeout oder ein Fehler aufgetreten ist (Anforderung mit der Antwort 429 oder 5xx). Es gibt drei Arten von Wiederholungsrichtlinien, `exponential`, `fixed` und `none`. Wenn in der Workflowdefinition keine Wiederholungsrichtlinie angegeben ist, wird die Standardrichtlinie verwendet. Sie können Wiederholungsrichtlinien in den **Eingaben** für eine bestimmte Aktion oder für einen bestimmten Trigger konfigurieren, wenn sie wiederholbar ist. In ähnlicher Weise können Wiederholungsrichtlinien (gegebenenfalls) im Logik-App-Designer unter den **Einstellungen** für einen bestimmten Block konfiguriert werden.

Informationen zu den Einschränkungen für Wiederholungsrichtlinien finden Sie unter [Logik-Apps – Einschränkungen und Konfiguration](../logic-apps/logic-apps-limits-and-config.md), und weitere Informationen zur unterstützten Syntax finden Sie im [Abschnitt zu Wiederholungsrichtlinien in Workflow-Aktionen und Triggern][retryPolicyMSDN].

### <a name="exponential-interval"></a>Exponentielles Intervall
Die Richtlinienart `exponential` nimmt die Wiederholung einer Anforderung mit Fehler nach einem zufälligen Zeitintervall aus einem exponentiell anwachsenden Bereich vor. Für jeden Wiederholungsversuch ist dabei garantiert, dass er in einem Zufallsintervall gesendet wird, das größer als **MinimumIntervall** und kleiner als **MaximumIntervall** ist. Für jeden Wiederholungsversuch bis zum Erreichen von **Anzahl** einschließlich wird eine uniforme Zufallsvariable im unten angegebenen Bereich generiert:
<table>
<tr><th> Bereich der Zufallsvariablen </th></tr>
<tr><td>

| Wiederholungsanzahl | Minimales Intervall | Maximales Intervall |
| ------------ |  ------------ |  ------------ |
| 1 | Max(0, **MinimumIntervall**) | Min(Intervall, **MaximumIntervall**) |
| 2 | Max(Intervall, **MinimumIntervall**) | Min(2 * Intervall, **MaximumIntervall**) |
| 3 | Max(2*Intervall, **MinimumIntervall**) | Min(4 * Intervall, **MaximumIntervall**) |
| 4 | Max(4 * Intervall, **MinimumIntervall**) | Min(8 * Intervall, **MaximumIntervall**) |
| ... |

</td></tr></table>

Für Richtlinien vom Typ `exponential` sind **Anzahl** und **Intervall** erforderlich, während **MinimumIntervall** und **MaximumIntervalL** optional angegeben werden können, um die Standardwerte PT5S bzw. PT1D außer Kraft zu setzen.

| Elementname | Erforderlich | Typ | Beschreibung |
| ------------ | -------- | ---- | ----------- |
| Typ | Ja | String | `exponential` |
| count | Ja | Integer | Die Anzahl der Wiederholungsversuche muss zwischen 1 und 90 liegen  |
| interval | Ja | String | Das Wiederholungsintervall im [ISO 8601-Format](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) muss zwischen PT5S und PT1D liegen |
| MinimumIntervall | Nein| String | Das Minimumintervall für die Wiederholung im [ISO 8601-Format](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) muss zwischen PT5S und **Intervall** liegen |
| Maximumintervall | Nein| String | Das Maximumintervall für die Wiederholung im [ISO 8601-Format](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) muss zwischen **Intervall** und PT1D liegen |

### <a name="fixed-interval"></a>Festes Intervall

Beim Richtlinientyp `fixed` wird die Ausführung einer Anforderung mit Fehler nach dem Abwarten für die angegebene Zeitspanne durch erneutes Senden der Anforderung erneut versucht.

| Elementname | Erforderlich | Typ | Beschreibung |
| ------------ | -------- | ---- | ----------- |
| Typ | Ja | String | `fixed`|
| count | Ja | Integer | Die Anzahl der Wiederholungsversuche muss zwischen 1 und 90 liegen |
| interval | Ja | String | Das Wiederholungsintervall im [ISO 8601-Format](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) muss zwischen PT5S und PT1D liegen |

### <a name="none"></a>Keine
Der Richtlinientyp `none` bewirkt keinen erneuten Ausführungsversuch einer Anforderung mit Fehler.

| Elementname | Erforderlich | Typ | Beschreibung |
| ------------ | -------- | ---- | ----------- |
| Typ | Ja | String | `none`|

### <a name="default"></a>Standard
Falls keine Wiederholungsrichtlinie angegeben ist, wird die Standardrichtlinie verwendet. Die Standardrichtlinie ist eine Richtlinie mit exponentiellem Intervall, die bis zu 4 Wiederholungsversuche mit exponentiell wachsenden Intervallen sendet, die um 7,5 Sekunden skaliert und auf den Bereich zwischen 5 und 45 Sekunden beschränkt sind. Diese Standardrichtlinie (die bei nicht definiertem Wert für **retryPolicy** verwendet wird) ist äquivalent zur Richtlinie in dieser Beispieldefinition eines HTTP-Workflows:

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

## <a name="catch-failures-with-the-runafter-property"></a>Abfangen von Fehlern mit der RunAfter-Eigenschaft

Für jede Logik-App wird deklariert, welche Aktionen beendet werden müssen, bevor die Aktion startet, z.B. beim Sortieren der Schritte im Workflow. In der Aktionsdefinition wird diese Sortierung als `runAfter`-Eigenschaft bezeichnet. Diese Eigenschaft ist ein Objekt, mit dem beschrieben wird, von welchen Aktionen und Aktionsstatus die Aktion ausgeführt wird. Standardmäßig werden alle Aktionen, die mit dem Logik-App-Designer hinzugefügt werden, auf die Ausführung nach dem vorherigen Schritt festgelegt (`runAfter`), wenn der vorherige Schritt `Succeeded` lautet. Sie können diesen Wert aber so anpassen, dass Aktionen ausgelöst werden, wenn für die vorherigen Aktionen `Failed` oder `Skipped` oder ein möglicher Satz mit diesen Werten gilt. Wenn Sie beispielsweise ein Element einem angegebenen Service Bus-Thema hinzufügen möchten, nachdem die spezifische Aktion `Insert_Row` fehlgeschlagen ist, können Sie die folgende `runAfter`-Konfiguration verwenden:

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

Beachten Sie, dass die `runAfter`-Eigenschaft für die Auslösung festgelegt wird, wenn die Aktion `Insert_Row` den Status `Failed` hat. Verwenden Sie die folgende Syntax, um die Aktion auszuführen, wenn der Aktionsstatus `Succeeded`, `Failed` oder `Skipped` lautet:

```json
"runAfter": {
        "Insert_Row": [
            "Failed", "Succeeded", "Skipped"
        ]
    }
```

> [!TIP]
> Aktionen, die nach dem Fehlschlagen einer vorhergehenden Aktion ausgeführt werden und erfolgreich sind, werden als `Succeeded` gekennzeichnet. Dies bedeutet Folgendes: Wenn das Abfangen aller Fehler eines Workflows erfolgreich ist, wird die gesamte Ausführung als `Succeeded` gekennzeichnet.

## <a name="scopes-and-results-to-evaluate-actions"></a>Bereiche und Ergebnisse zum Auswerten von Aktionen

Ähnlich wie bei der Ausführung nach einzelnen Aktionen, können Sie Aktionen auch in einem [Bereich](../logic-apps/logic-apps-loops-and-scopes.md) gruppieren. Ein Bereich dient also als logische Gruppierung von Aktionen. Bereich sind nützlich, um Logik-App-Aktionen zu organisieren und Gesamtauswertungen zum Status eines Bereichs durchzuführen. Der Bereich selbst erhält einen Status, nachdem alle Aktionen eines Bereichs abgeschlossen wurden. Der Bereichsstatus wird mit den gleichen Kriterien wie bei einer Ausführung ermittelt. Wenn für die letzte Aktion einer Ausführungsverzweigung `Failed` oder `Aborted` gilt, lautet der Status `Failed`.

Zum Auslösen von bestimmten Aktionen für Fehler, die innerhalb des Bereichs aufgetreten sind, können Sie `runAfter` mit einem Bereich verwenden, der als `Failed` gekennzeichnet ist. Durch die Ausführung nach dem Fehlschlagen eines Bereichs können Sie eine einzelne Aktion erstellen, um Fehler abzufangen, wenn für *beliebige* Aktionen im Bereich Fehler auftreten.

### <a name="getting-the-context-of-failures-with-results"></a>Abrufen des Kontexts von Fehlern mit Ergebnissen

Das Abfangen der Fehler eines Bereichs ist nützlich. Aber häufig ist auch der Kontext hilfreich, um genau zu verstehen, für welche Aktionen Fehler aufgetreten sind und welche Fehler oder Statuscodes zurückgegeben wurden. Die Workflowfunktion `@result()` liefert Kontext zum Ergebnis aller Aktionen innerhalb eines Bereichs.

Für `@result()` wird ein einzelner Parameter (Bereichsname) verwendet und ein Array mit allen Aktionsergebnissen des Bereichs zurückgegeben. Diese Aktionsobjekte enthalten die gleichen Attribute wie das `@actions()`-Objekt, z.B. Aktionsstartzeit, Aktionsendzeit, Aktionsstatus, Aktionseingaben, Korrelations-IDs der Aktion und Aktionsausgaben. Sie können eine `@result()`-Funktion problemlos mit `runAfter` koppeln, um Kontext zu allen Aktionen zu senden, für die im Bereich ein Fehler aufgetreten ist.

Zum Ausführen einer Aktion *für jede* Aktion eines Bereichs, die fehlgeschlagen ist (`Failed`), filtern Sie das Array mit den Ergebnissen nach den fehlgeschlagenen Aktionen. Sie können `@result()` mit der Aktion **[Array filtern](../connectors/connectors-native-query.md)** (Filter_array) und einer **[ForEach](../logic-apps/logic-apps-loops-and-scopes.md)**-Schleife koppeln. Sie können für das Array mit den gefilterten Ergebnissen eine Aktion für jeden Fehler durchführen, indem Sie die **ForEach**-Schleife verwenden. Hier ist ein Beispiel gefolgt von einer ausführlichen Erklärung angegeben, bei dem eine HTTP POST-Anforderung mit dem Antworttext aller fehlgeschlagenen Aktionen im Bereich `My_Scope` gesendet wird.

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

In dieser ausführlichen exemplarischen Vorgehensweise wird beschrieben, was passiert:

1. Um die Ergebnisse aller Aktionen in `My_Scope` zu erhalten, wird mit der Aktion **Array filtern** `@result('My_Scope')` gefiltert.

2. Die Bedingung für **Array filtern** ist ein beliebiges `@result()`-Element, dessen Status `Failed` lautet. Mit dieser Bedingung wird das Array mit allen Aktionsergebnissen aus `My_Scope` gefiltert, um ein Array nur mit Aktionen mit Fehlerergebnissen zu erhalten.

3. Durchführen einer **For Each**-Aktion für Ausgaben vom Typ **Array gefiltert**. In diesem Schritt wird eine Aktion *für jedes* Ergebnis einer fehlgeschlagenen Aktion durchgeführt, das zuvor gefiltert wurde.

    Wenn eine einzelne Aktion im Bereich fehlgeschlagen ist, werden die Aktionen in der `foreach`-Ausführung nur einmal ausgeführt. 
    Viele fehlgeschlagene Aktionen führen zu einer Aktion pro Fehler.

4. Senden von HTTP POST für den Antworttext des `foreach`-Elements oder `@item()['outputs']['body']`. Die `@result()`-Elementform ist mit der `@actions()`-Form identisch und kann auch genauso analysiert werden.

5. Einbinden von zwei benutzerdefinierten Headern mit dem Namen der Fehleraktion `@item()['name']` und der Clientnachverfolgungs-ID der Fehlerausführung `@item()['clientTrackingId']`.

Zu Referenzzwecken ist hier ein Beispiel für ein einzelnes `@result()`-Element angegeben, das die Eigenschaften `name`, `body` und `clientTrackingId` enthält, die im vorherigen Beispiel analysiert werden. Außerhalb eines `foreach`-Elements wird mit `@result()` ein Array mit diesen Objekten zurückgegeben.

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

Sie können die obigen Ausdrücke verwenden, um unterschiedliche Muster für die Ausnahmebehandlung zu nutzen. Außerdem können Sie die Entscheidung treffen, eine einzelne Aktion für die Ausnahmebehandlung außerhalb des Bereichs durchzuführen, bei der das gesamte gefilterte Array mit Fehlern akzeptiert wird, und das `foreach`-Element zu entfernen. Darüber hinaus können Sie noch andere nützliche Eigenschaften aus der obigen `@result()`-Antwort einfügen.

## <a name="azure-diagnostics-and-telemetry"></a>Azure-Diagnose und Telemetrie

Die oben beschriebenen Muster eignen sich gut zum Behandeln von Fehlern und Ausnahmen in einer Ausführung, aber Sie können auch unabhängig von der eigentlichen Ausführung Fehler identifizieren und darauf reagieren. 
[Azure-Diagnose](../logic-apps/logic-apps-monitor-your-logic-apps.md) ist eine einfache Möglichkeit zum Senden aller Workflowereignisse (einschließlich aller Ausführungs- und Aktionsstatus) an ein Azure Storage-Konto oder einen Azure Event Hub. Sie können die Protokolle und Metriken überwachen oder mit einem von Ihnen bevorzugten Überwachungstool veröffentlichen, um jeweils den Ausführungsstatus auszuwerten. Eine potenzielle Option ist das Streamen aller Ereignisse über den Azure Event Hub in [Stream Analytics](https://azure.microsoft.com/services/stream-analytics/). In Stream Analytics können Sie Liveabfragen für Anomalien, Mittelwerte oder Fehler aus den Diagnoseprotokollen schreiben. Für Stream Analytics ist die Ausgabe in andere Datenquellen wie Warteschlangen, Themen, SQL, Azure Cosmos DB und Power BI problemlos möglich.

## <a name="next-steps"></a>Nächste Schritte

* [Szenario: Ausnahmebehandlung und Fehlerprotokollierung für Logik-Apps](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
* [Beispiele und häufige Szenarios für Logik-Apps](../logic-apps/logic-apps-examples-and-scenarios.md)
* [Erstellen von Vorlagen für die Bereitstellungs- und Versionsverwaltung von Logik-Apps](../logic-apps/logic-apps-create-deploy-template.md)
* [Erstellen und Bereitstellen von Logik-Apps mit Visual Studio](logic-apps-deploy-from-vs.md)

<!-- References -->
[retryPolicyMSDN]: https://docs.microsoft.com/rest/api/logic/actions-and-triggers#Anchor_9
