---
title: "Workflowtrigger und -aktionen – Azure Logic Apps | Microsoft-Dokumentation"
description: "Erfahren Sie mehr über die Trigger und Aktionen zum Erstellen von automatisierten Workflows und Prozessen mit Logik-Apps"
services: logic-apps
author: MandiOhlinger
manager: anneta
editor: 
documentationcenter: 
ms.assetid: 86a53bb3-01ba-4e83-89b7-c9a7074cb159
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/13/2017
ms.author: klam; LADocs
ms.openlocfilehash: af30fd30f389cdc2070c45ae3b6e2cb1165239e7
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2018
---
# <a name="triggers-and-actions-for-logic-app-workflows"></a>Trigger und Aktionen für Logik-App-Workflows

Alle Logik-Apps beginnen mit einem Trigger, auf den dann Aktionen folgen. In diesem Artikel werden die Arten von Triggern und Aktionen beschrieben, die Sie zum Erstellen von Systemintegrationen und zum Automatisieren von Geschäftsworkflows oder -prozessen mit Logik-Apps verwenden können. 
  
## <a name="triggers-overview"></a>Übersicht über Trigger 

Alle Logik-Apps beginnen mit einem Trigger. Dieser gibt die Aufrufe an, die die Ausführung einer Logik-App initiieren können. Hier sind die Typen von Triggern, die Sie verwenden können:

* Ein *Abruftrigger*, der den HTTP-Endpunkt eines Diensts in regelmäßigen Abständen überprüft
* Ein [Pushtrigger*, der die* Workflowdienst-REST-API](https://docs.microsoft.com/rest/api/logic/workflows) aufruft
  
Alle Trigger enthalten folgende allgemeine Elemente:  
  
```json
"<myTriggerName>": {
    "type": "<triggerType>",
    "inputs": { <callSettings> },
    "recurrence": {  
        "frequency": "Second | Minute | Hour | Day | Week | Month | Year",
        "interval": "<recurrence-interval-based-on-frequency>"
    },
    "conditions": [ <array-with-required-conditions> ],
    "splitOn": "<property-used-for-creating-runs>",
    "operationOptions": "<options-for-operations-on-the-trigger>"
}
```

## <a name="trigger-types-and-inputs"></a>Triggertypen und -eingaben  

Jeder Triggertyp verfügt über eine andere Schnittstelle sowie über verschiedene *Eingaben*, die sein Verhalten definieren. 

| Triggertyp | BESCHREIBUNG | 
| ------------ | ----------- | 
| **Serie** | Wird auf der Grundlage eines definierten Zeitplans ausgelöst. Sie können ein Datum und eine Uhrzeit in der Zukunft festlegen, um diesen Trigger auszulösen. Je nach Häufigkeit können Sie auch Zeiten und Tage für die Workflowausführung angeben. | 
| **Anforderung**  | Verwandelt Ihre Logik-App in einen aufrufbaren Endpunkt. (Wird auch als manueller Trigger bezeichnet.) | 
| **HTTP** | Dient zum Überprüfen (oder *Abfragen*) eines HTTP-Webendpunkts. Der HTTP-Endpunkt muss einem bestimmten auslösenden Vertrag entsprechen – entweder durch Verwendung eines asynchronen Musters (202) oder durch Rückgabe eines Arrays. | 
| **ApiConnection** | Vergleichbar mit dem HTTP-Trigger; führt ebenfalls eine Abfrage durch, verwendet aber [von Microsoft verwaltete APIs](../connectors/apis-list.md). | 
| **HTTPWebhook** | Vergleichbar mit dem **Anforderungstrigger**; verwandelt Ihre Logik-App ebenfalls in einen aufrufbaren Endpunkt, ruft allerdings eine bestimmte URL für die Registrierung/Aufhebung der Registrierung auf. |
| **ApiConnectionWebhook** | Vergleichbar mit dem **HTTPWebhook**-Trigger, verwendet aber von Microsoft verwaltete APIs. | 
||| 

Weitere Informationen finden Sie unter [Definitionssprache für Workflows](../logic-apps/logic-apps-workflow-definition-language.md). 

<a name="recurrence-trigger"></a>

## <a name="recurrence-trigger"></a>Recurrence-Trigger  

Dieser Trigger wird auf der Grundlage der angegebenen Wiederholung und des angegebenen Zeitplans ausgeführt, wodurch Sie einen Workflow komfortabel in regelmäßigen Abständen ausführen können. 

Das folgende Beispiel zeigt einen einfachen Wiederholungstrigger, der täglich ausgeführt wird:

```json
"myRecurrenceTrigger": {
    "type": "Recurrence",
    "recurrence": {
        "frequency": "Day",
        "interval": 1
    }
}
```

Für die Triggerauslösung können Sie auch ein Startdatum und eine Startuhrzeit planen. Wenn Sie also beispielsweise jeden Montag einen wöchentlichen Bericht starten möchten, können Sie den Start der Logik-App wie im folgenden Beispiel auf einen bestimmten Montag festlegen: 

```json
"myRecurrenceTrigger": {
    "type": "Recurrence",
    "recurrence": {
        "frequency": "Week",
        "interval": "1",
        "startTime": "2017-09-18T00:00:00Z"
    }
}
```

Definition für diesen Trigger:

```json
"myRecurrenceTrigger": {
    "type": "Recurrence",
    "recurrence": {
        "frequency": "second|minute|hour|day|week|month",
        "interval": <recurrence-interval-based-on-frequency>,
        "schedule": {
            // Applies only when frequency is Day or Week. Separate values with commas.
            "hours": [ <one-or-more-hour-marks> ], 
            // Applies only when frequency is Day or Week. Separate values with commas.
            "minutes": [ <one-or-more-minute-marks> ], 
            // Applies only when frequency is Week. Separate values with commas.
            "weekDays": [ "Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday" ] 
        },
        "startTime": "<start-date-time-with-format-YYYY-MM-DDThh:mm:ss>",
        "timeZone": "<specify-time-zone>"
    }
}
```

| Elementname | Erforderlich | Typ | BESCHREIBUNG | 
| ------------ | -------- | ---- | ----------- | 
| frequency | Ja | Zeichenfolge | Die Zeiteinheit für die Häufigkeit der Triggerauslösung. Zulässige Werte: „second“, „minute“, „hour“, „day“, „week“ oder „month“ | 
| interval | Ja | Ganze Zahl  | Eine positive ganze Zahl, die beschreibt, wie oft der Workflow basierend auf der Häufigkeit ausgeführt wird. <p>Zulässige Mindest- und Maximalintervalle: <p>- Month: 1 - 16 Monate </br>- Day: 1 - 500 Tage </br>- Hour: 1 - 12.000 Stunden </br>- Minute: 1 - 72.000 Minuten </br>- Second: 1–9.999.999 Sekunden<p>Wenn das Intervall also beispielsweise auf „6“ und die Häufigkeit auf „month“ festgelegt ist, erfolgt die Wiederholung alle sechs Monate. | 
| timeZone | Nein  | Zeichenfolge | Nur relevant, wenn Sie eine Startzeit angeben, da dieser Trigger keine [UTC-Abweichung](https://en.wikipedia.org/wiki/UTC_offset) akzeptiert. Geben Sie die anzuwendende Zeitzone an. | 
| startTime | Nein  | Zeichenfolge | Geben Sie Startdatum und -uhrzeit im folgenden Format an: <p>JJJJ-MM-TTTHH:mm:ss (bei Angabe einer Zeitzone; beachten Sie dabei, dass „TT“ den Tag bezeichnet, während das dritte „T“ wie in den nachfolgenden Beispielen gezeigt nicht verändert werden darf) <p>Oder <p>JJJJ-MM-TTTHH:mm:ssZ (ohne Angabe einer Zeitzone) <p>Für „18. September 2017, 14:00 Uhr“ müssten Sie also beispielsweise „2017-09-18T14:00:00“ und eine Zeitzone (etwa „Pacific Standard Time“) angeben. Alternativ können Sie „2017-09-18T14:00:00Z“ ohne Zeitzone angeben. <p>**Hinweis:** Diese Startzeit muss dem [ISO 8601-Format für Datums-/Uhrzeitangaben](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) entsprechen und im [UTC-Datums-/Zeitformat](https://en.wikipedia.org/wiki/Coordinated_Universal_Time), aber ohne [UTC-Abweichung](https://en.wikipedia.org/wiki/UTC_offset) angegeben werden. Wenn Sie keine Zeitzone angeben, müssen Sie den Buchstaben „Z“ ohne Leerzeichen anhängen. „Z“ bezieht sich auf die entsprechende [nautische Zeit](https://en.wikipedia.org/wiki/Nautical_time). <p>Bei einfachen Zeitpläne ist die Startzeit das erste Vorkommen. Bei komplexeren Zeitplänen wird der Trigger nicht vor der Startzeit ausgelöst. Weitere Informationen zu Startdatum und -uhrzeit finden Sie unter [Schedule tasks and workflows that run regularly with logic apps](../connectors/connectors-native-recurrence.md) (Planen von regelmäßig ausgeführten Aufgaben und Workflows mit Logik-Apps). | 
| weekDays | Nein  | Zeichenfolge oder Zeichenfolgenarray | Wenn Sie für `frequency` die Option „Week“ angeben, können Sie einen Tag oder eine kommagetrennte Liste mit Tagen für die Workflowausführung angeben: „Monday“, „Tuesday“, „Wednesday“, „Thursday“, „Friday“, „Saturday“ oder „Sunday“ | 
| hours | Nein  | Ganze Zahl oder Ganzzahlarray | Wenn Sie für `frequency` die Option „Day“ oder „Week“ angeben, können Sie eine ganze Zahl oder eine kommagetrennte Liste mit ganzen Zahlen von 0 bis 23 als die Stunden des Tages angeben, zu denen der Workflow ausgeführt werden soll. <p>Wenn Sie also etwa „10“, „12“ und „14“ angeben, erhalten Sie die vollen Stunden „10 Uhr“, „12 Uhr“ und „14 Uhr“. | 
| minutes | Nein  | Ganze Zahl oder Ganzzahlarray | Wenn Sie für `frequency` die Option „Day“ oder „Week“ angeben, können Sie eine ganze Zahl oder eine kommagetrennte Liste mit ganzen Zahlen von 0 bis 59 als die Minuten der Stunde angeben, zu denen der Workflow ausgeführt werden soll. <p>Wenn Sie also beispielsweise „30“ als Minutenwert angeben und das vorherige Beispiel für Stunden des Tages verwenden, erhalten Sie „10:30 Uhr“, „12:30 Uhr“ und „14:30 Uhr“. | 
||||| 

Der folgende Wiederholungstrigger gibt beispielsweise an, dass die Logik-App frühestens ab dem 9. September 2017, 14:00 Uhr, wöchentlich jeden Montag um 10:30 Uhr, 12:30 Uhr und 14:30 Uhr (Pacific Standard Time) ausgeführt werden soll:

``` json
"myRecurrenceTrigger": {
    "type": "Recurrence",
    "recurrence": {
        "frequency": "Week",
        "interval": 1,
        "schedule": {
            "hours": [
                10,
                12,
                14
            ],
            "minutes": [
                30
            ],
            "weekDays": [
                "Monday"
            ]
        },
       "startTime": "2017-09-07T14:00:00",
       "timeZone": "Pacific Standard Time"
    }
}
```

Weitere Informationen mit Wiederholungs- und Startzeitbeispielen für diesen Trigger finden Sie unter [Schedule tasks and workflows that run regularly with logic apps](../connectors/connectors-native-recurrence.md) (Planen von regelmäßig ausgeführten Aufgaben und Workflows mit Logik-Apps).

## <a name="request-trigger"></a>Anforderungstrigger

Dieser Trigger fungiert als Endpunkt, den Sie verwenden können, um Ihre Logik-App über eine HTTP-Anforderung aufzurufen. Das folgende Beispiel zeigt einen Anforderungstrigger:  
  
```json
"myRequestTrigger": {
    "type": "Request",
    "kind": "Http",
    "inputs": {
        "schema": {
            "type": "Object",
            "properties": {
                "myInputProperty1": { "type" : "string" },
                "myInputProperty2": { "type" : "number" }
            },
            "required": [ "myInputProperty1" ]
        }
    }
} 
```

Dieser Trigger verfügt über eine optionale Eigenschaft namens `schema`:
  
| Elementname | Erforderlich | Typ | BESCHREIBUNG |
| ------------ | -------- | ---- | ----------- |
| schema | Nein  | Objekt | Ein JSON-Schema zur Validierung der eingehenden Anforderung. Hilfreich, um nachfolgende Workflowschritte darüber zu informieren, auf welche Eigenschaften verwiesen werden soll. | 
||||| 

Zum Aufrufen dieses Triggers als Endpunkt muss die `listCallbackUrl`-API aufgerufen werden. Weitere Informationen finden Sie unter [Workflows](https://docs.microsoft.com/rest/api/logic/workflows).

## <a name="http-trigger"></a>HTTP-Trigger  

Dieser Trigger fragt einen angegebenen Endpunkt ab und überprüft anhand der Antwort, ob der Workflow ausgeführt werden soll oder nicht. Hier verwendet das `inputs`-Objekt die folgenden, zum Erstellen eines HTTP-Aufrufs erforderlichen Parameter: 

| Elementname | Erforderlich | Typ | BESCHREIBUNG | 
| ------------ | -------- | ---- | ----------- | 
| method | Ja | Zeichenfolge | Zulässige HTTP-Methoden: „GET“, „POST“, „PUT“, „DELETE“, „PATCH“ oder „HEAD“ | 
| uri | Ja| Zeichenfolge | Der HTTP- oder HTTPS-Endpunkt, den der Trigger überprüft. Maximal zulässige Zeichenfolgengröße: 2 KB | 
| Abfragen | Nein  | Objekt | Stellt die Abfrageparameter dar, die Sie in die URL einbeziehen möchten. <p>`"queries": { "api-version": "2015-02-01" }` fügt der URL beispielsweise `?api-version=2015-02-01` hinzu. | 
| headers | Nein  | Objekt | Stellt die einzelnen Header dar, die in der Anforderung gesendet werden. <p>Verwenden Sie beispielsweise Folgendes, um Sprache und Typ für eine Anforderung festzulegen: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| body | Nein  | Objekt | Stellt die an den Endpunkt gesendete Nutzlast dar. | 
| retryPolicy | Nein  | Objekt | Verwenden Sie dieses Objekt, um das Wiederholungsverhalten bei Fehlern vom Typ „4xx“ oder „5xx“ anzupassen. Weitere Informationen finden Sie unter [Wiederholungsrichtlinien](../logic-apps/logic-apps-exception-handling.md). | 
| authentication | Nein  | Objekt | Stellt die gewünschte Authentifizierungsmethode für die Anforderung dar. Weitere Informationen finden Sie unter [Ausgehende Authentifizierung von Scheduler](../scheduler/scheduler-outbound-authentication.md). <p>Neben Scheduler wird auch noch folgende Eigenschaft unterstützt: `authority`. Ohne Angabe wird standardmäßig der Wert `https://login.windows.net` verwendet. Sie können aber einen anderen Wert verwenden (beispielsweise `https://login.windows\-ppe.net`). | 
||||| 

Eine *Wiederholungsrichtlinie* gilt für vorübergehende Fehler (HTTP-Statuscodes 408, 429 und 5xx) sowie für Verbindungsausnahmen. Sie können diese Richtlinie wie folgt mit dem `retryPolicy`-Objekt definieren:
  
```json
"retryPolicy": {
    "type": "<retry-policy-type>",
    "interval": <retry-interval>,
    "count": <number-of-retry-attempts>
}
```

Für den HTTP-Trigger muss die HTTP-API einem bestimmten Muster entsprechen, damit sie reibungslos mit Ihrer Logik-App zusammenarbeitet. Der Trigger erkennt folgende Eigenschaften:  
  
| response | Erforderlich | BESCHREIBUNG | 
| -------- | -------- | ----------- |  
| Statuscode | Ja | Der Statuscode 200 (OK) bewirkt eine Ausführung. Alle anderen Statuscodes bewirken keine Ausführung. | 
| Retry-After-Header | Nein  | Die Anzahl von Sekunden bis zur erneuten Abfrage des Endpunkts durch die Logik-App. | 
| Adressheader | Nein  | Die URL, die im nächsten Abfrageintervall aufgerufen werden soll. Ohne Angabe wird die ursprüngliche URL verwendet. | 
|||| 

Im Anschluss finden Sie einige Beispiele für das Verhalten verschiedener Anforderungsarten:
  
| Antwortcode | Wiederholungsintervall | Verhalten | 
| ------------- | ----------- | -------- | 
| 200 | {keine} | Führt den Workflow aus und sucht erst nach der definierten Wiederholung wieder nach weiteren Inhalten. | 
| 200 | 10 Sekunden | Führt den Workflow aus und sucht nach zehn Sekunden nach weiteren Inhalten. |  
| 202 | 60 Sekunden | Der Workflow wird nicht ausgelöst. Der nächste Versuch erfolgt gemäß der definierten Wiederholung in einer Minute. Wenn die definierte Wiederholung weniger als eine Minute beträgt, hat der Retry-After-Header Vorrang. Andernfalls wird die definierte Wiederholung verwendet. | 
| 400 | {keine} | Ungültige Anforderung. Der Workflow wird nicht ausgeführt. Wenn keine `retryPolicy` definiert ist, wird die Standardrichtlinie verwendet. Wenn die Anzahl von Wiederholungen erreicht ist, sucht der Trigger nach der definierten Wiederholung erneut nach Inhalten. | 
| 500 | {keine}| Serverfehler. Der Workflow wird nicht ausgeführt. Wenn keine `retryPolicy` definiert ist, wird die Standardrichtlinie verwendet. Wenn die Anzahl von Wiederholungen erreicht ist, sucht der Trigger nach der definierten Wiederholung erneut nach Inhalten. | 
|||| 

HTTP-Triggerausgaben: 
  
| Elementname | Typ | BESCHREIBUNG |
| ------------ | ---- | ----------- |
| headers | Objekt | Die Header der HTTP-Antwort. | 
| body | Objekt | Der Text der HTTP-Antwort. | 
|||| 

<a name="apiconnection-trigger"></a>

## <a name="apiconnection-trigger"></a>APIConnection-Trigger  

Die grundlegende Funktionalität dieses Triggers entspricht dem HTTP-Trigger. Die Parameter zum Identifizieren der Aktion sind jedoch unterschiedlich. Beispiel:    
  
```json
"myDailyReportTrigger": {
    "type": "ApiConnection",
    "inputs": {
        "host": {
            "api": {
                "runtimeUrl": "https://myarticles.example.com/"
            }
        },
        "connection": {
            "name": "@parameters('$connections')['myconnection'].name"
        }
    },  
    "method": "POST",
    "body": {
        "category": "myCategory"
    }
}
```

| Elementname | Erforderlich | Typ | BESCHREIBUNG | 
| ------------ | -------- | ---- | ----------- | 
| host | Ja | Objekt | Das gehostete Gateway und die ID für die API-App. | 
| method | Ja | Zeichenfolge | Zulässige HTTP-Methoden: „GET“, „POST“, „PUT“, „DELETE“, „PATCH“ oder „HEAD“ | 
| Abfragen | Nein  | Objekt | Stellt die Abfrageparameter dar, die Sie in die URL einbeziehen möchten. <p>`"queries": { "api-version": "2015-02-01" }` fügt der URL beispielsweise `?api-version=2015-02-01` hinzu. | 
| headers | Nein  | Objekt | Stellt die einzelnen Header dar, die in der Anforderung gesendet werden. <p>Verwenden Sie beispielsweise Folgendes, um Sprache und Typ für eine Anforderung festzulegen: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| body | Nein  | Objekt | Stellt die an den Endpunkt gesendete Nutzlast dar. | 
| retryPolicy | Nein  | Objekt | Verwenden Sie dieses Objekt, um das Wiederholungsverhalten bei Fehlern vom Typ „4xx“ oder „5xx“ anzupassen. Weitere Informationen finden Sie unter [Wiederholungsrichtlinien](../logic-apps/logic-apps-exception-handling.md). | 
| authentication | Nein  | Objekt | Stellt die gewünschte Authentifizierungsmethode für die Anforderung dar. Weitere Informationen finden Sie unter [Ausgehende Authentifizierung von Scheduler](../scheduler/scheduler-outbound-authentication.md). | 
||||| 

Eigenschaften für das `host`-Objekt:  
  
| Elementname | Erforderlich | BESCHREIBUNG | 
| ------------ | -------- | ----------- | 
| api runtimeUrl | Ja | Der Endpunkt für die verwaltete API. | 
| connection name |  | Der Name der verwalteten API-Verbindung, die der Workflow verwendet. Muss auf einen Parameter namens `$connection` verweisen. |
|||| 

Eine *Wiederholungsrichtlinie* gilt für vorübergehende Fehler (HTTP-Statuscodes 408, 429 und 5xx) sowie für Verbindungsausnahmen. Sie können diese Richtlinie wie folgt mit dem `retryPolicy`-Objekt definieren:
  
```json
"retryPolicy": {
    "type": "<retry-policy-type>",
    "interval": <retry-interval>,
    "count": <number-of-retry-attempts>
}
```

Ausgaben für einen APIConnection-Trigger:
  
| Elementname | Typ | BESCHREIBUNG |
| ------------ | ---- | ----------- |
| headers | Objekt | Die Header der HTTP-Antwort. | 
| body | Objekt | Der Text der HTTP-Antwort. | 
|||| 

Erfahren Sie mehr über die [Preisgestaltung für APIConnection-Trigger](../logic-apps/logic-apps-pricing.md#triggers).

## <a name="httpwebhook-trigger"></a>HTTPWebhook-Trigger  

Dieser Trigger stellt ähnlich wie der `Request`-Trigger einen Endpunkt bereit, ruft aber auch eine angegebene URL für die Registrierung/Aufhebung der Registrierung auf. Das folgende Beispiel zeigt einen HTTPWebhook-Trigger:

```json
"myAppsSpotTrigger": {
    "type": "HttpWebhook",
    "inputs": {
        "subscribe": {
            "method": "POST",
            "uri": "https://pubsubhubbub.appspot.com/subscribe",
            "headers": {},
            "body": {
                "hub.callback": "@{listCallbackUrl()}",
                "hub.mode": "subscribe",
                "hub.topic": "https://pubsubhubbub.appspot.com/articleCategories/technology"
            },
            "authentication": {},
            "retryPolicy": {}
        },
        "unsubscribe": {
            "method": "POST",
            "url": "https://pubsubhubbub.appspot.com/subscribe",
            "body": {
                "hub.callback": "@{workflow().endpoint}@{listCallbackUrl()}",
                "hub.mode": "unsubscribe",
                "hub.topic": "https://pubsubhubbub.appspot.com/articleCategories/technology"
            },
            "authentication": {}
        }
    },
    "conditions": []
}
```

Viele dieser Abschnitte sind optional, und das Verhalten des HTTPWebhook-Triggers hängt davon ab, welche Abschnitte Sie angeben oder weglassen. Eigenschaften für den HTTPWebhook-Trigger:
  
| Elementname | Erforderlich | BESCHREIBUNG | 
| ------------ | -------- | ----------- |  
| subscribe | Nein  | Gibt die ausgehende Anforderung an, die bei der Triggererstellung aufgerufen werden soll und die anfängliche Registrierung ausführt. | 
| unsubscribe | Nein  | Gibt die ausgehende Anforderung an, die beim Löschen des Triggers aufgerufen werden soll. | 
|||| 

Grenzwerte für einen Webhooktrigger können auf die gleiche Weise angegeben werden wie [asynchrone Grenzwerte für HTTP](#asynchronous-limits). Im Anschluss finden Sie weitere Informationen zu den Aktionen `subscribe` und `unsubscribe`:

* `subscribe` wird aufgerufen, damit der Trigger damit beginnen kann, auf Ereignisse zu lauschen. Dieser ausgehende Aufruf beginnt mit den gleichen Parametern wie HTTP-Standardaktionen. Er wird bei jeder Veränderung des Workflows ausgeführt – beispielsweise bei einer Erneuerung der Anmeldeinformationen oder bei einer Änderung der Eingabeparameter des Triggers. 
  
  Zur Unterstützung dieses Aufrufs gibt die Funktion `@listCallbackUrl()` eine eindeutige URL für diesen speziellen Trigger im Workflow zurück. Diese URL stellt den eindeutigen Bezeichner für die Endpunkte dar, die die REST-API des Diensts verwenden.
  
* `unsubscribe` wird automatisch aufgerufen, wenn ein Vorgang dazu führt, dass der Trigger ungültig wird. Hierzu zählen unter anderem folgende Vorgänge:

  * Löschen oder Deaktivieren des Triggers 
  * Löschen oder Deaktivieren des Workflows 
  * Löschen oder Deaktivieren des Abonnements 
  
  Für diese Funktion werden die gleichen Parameter verwendet wie beim HTTP-Trigger.

Im Anschluss finden Sie die Ausgaben des HTTPWebhook-Triggers und die Inhalte der eingehenden Anforderung:
  
| Elementname | Typ | BESCHREIBUNG |
| ------------ | ---- | ----------- |
| headers | Objekt | Die Header der HTTP-Antwort. | 
| body | Objekt | Der Text der HTTP-Antwort. | 
|||| 

## <a name="triggers-conditions"></a>Trigger: Bedingungen

Bei jedem Trigger können Sie mithilfe einzelner oder mehrerer Bedingungen bestimmen, ob der Workflow ausgeführt werden soll. In diesem Beispiel wird der Bericht nur ausgelöst, wenn der Parameter `sendReports` des Workflows auf „true“ festgelegt ist. 

```json
"myDailyReportTrigger": {
    "type": "Recurrence",
    "conditions": [ 
        {
            "expression": "@parameters('sendReports')"
        } 
    ],
    "recurrence": {
        "frequency": "Day",
        "interval": 1
    }
}
```

Bedingungen können außerdem auf den Statuscode des Triggers verweisen. So können Sie beispielsweise festlegen, dass ein Workflow nur gestartet werden soll, wenn Ihre Website den Statuscode 500 zurückgibt:
  
``` json
"conditions": [ 
    {  
      "expression": "@equals(triggers().code, 'InternalServerError')"  
    }  
]  
```  

> [!NOTE]
> Standardmäßig wird ein Trigger nur beim Empfang einer „200 OK“-Antwort ausgelöst. Sobald ein Ausdruck auf den Statuscode des Triggers verweist, wird das Standardverhalten des Triggers ersetzt. Wenn der Trigger also basierend auf mehreren Statuscodes ausgelöst werden soll, z.B. Statuscode 200 und Statuscode 201, müssen Sie diese Anweisung als Bedingung einschließen: 
>
> `@or(equals(triggers().code, 200),equals(triggers().code, 201))` 

<a name="split-on-debatch"></a>

## <a name="triggers-process-an-array-with-multiple-runs"></a>Trigger: Verarbeiten eines Arrays mit mehreren Ausführungen

Wenn der Trigger ein Array zum Verarbeiten durch die Logik-App zurückgibt, könnte eine „for each“-Schleife in einigen Fällen für die Verarbeitung der einzelnen Arrayelemente zu lange dauern. Stattdessen können Sie die **SplitOn**-Eigenschaft in Ihrem Trigger zur *debatch*-Ausführung für das Array verwenden. 

Bei der Batchauflösung werden das Array in seine Elemente aufgeteilt und eine neue Logik-App-Instanz gestartet, die für jedes Arrayelement ausgeführt wird. Dieser Ansatz ist beispielsweise hilfreich, wenn Sie einen Endpunkt abfragen möchten, der zwischen Abfrageintervallen mehrere neue Elemente zurückgeben könnte.
Informationen über die maximale Anzahl der Arrayelemente, die **SplitOn** in einer einzelnen Logik-App-Ausführung verarbeiten kann, finden Sie unter [Logic Apps-Grenzwerte und -Konfiguration](../logic-apps/logic-apps-limits-and-config.md). 

> [!NOTE]
> Sie können Triggern **SplitOn** nur durch manuelles Definieren oder Überschreiben in der Codeansicht für die JSON-Definition der Logik-App hinzufügen. Sie können **SplitOn** nicht verwenden, wenn Sie ein synchrones Antwortmuster implementieren möchten. Jeder Workflow, der **SplitOn** verwendet und eine Antwortaktion enthält, wird asynchron ausgeführt und sendet sofort eine `202 ACCEPTED`-Antwort.

Wenn die Swagger-Datei des Triggers eine Nutzlast beschreibt, die ein Array ist, wird die **SplitOn**-Eigenschaft automatisch dem Trigger hinzugefügt. Fügen Sie diese Eigenschaft andernfalls in der Antwortnutzlast hinzu, die das Array enthält, für das Sie die Batchauflösung ausführen möchten. 

Angenommen, Sie verfügen über eine API, die folgende Antwort zurückgibt: 
  
```json
{
    "Status": "Succeeded",
    "Rows": [ 
        { 
            "id": 938109380,
            "name": "customer-name-one"
        },
        {
            "id": 938109381,
            "name": "customer-name-two"
        }
    ]
}
```
  
Ihre Logik-App benötigt lediglich den Inhalt von `Rows`. Somit können Sie einen Trigger wie im folgenden Beispiel erstellen.

``` json
"myDebatchTrigger": {
    "type": "Http",
    "recurrence": {
        "frequency": "Second",
        "interval": "1"
    },
    "inputs": {
        "uri": "https://mydomain.com/myAPI",
        "method": "GET"
    },
    "splitOn": "@triggerBody()?.Rows"
}
```

> [!NOTE]
> Mit dem `SplitOn`-Befehl können Sie keine Eigenschaften außerhalb des Arrays abrufen. In diesem Beispiel können Sie also nicht die `status`-Eigenschaft in der von der API zurückgegebenen Antwort abrufen.
> 
> Um einen Fehler zu vermeiden, wenn die `?`-Eigenschaft nicht vorhanden ist, wird in diesem Beispiel der `Rows`-Operator verwendet.

Ihre Workflowdefinition kann nun `@triggerBody().name` verwenden, um `customer-name-one` aus der ersten Ausführung und `customer-name-two` aus der zweiten Ausführung abzurufen. Ihre Triggerausgaben sehen nun wie diese Beispiele aus:

```json
{
    "body": {
        "id": 938109380,
        "name": "customer-name-one"
    }
}
```

```json
{
    "body": {
        "id": 938109381,
        "name": "customer-name-two"
    }
}
```
  
## <a name="triggers-fire-only-after-all-active-runs-finish"></a>Trigger: Nur nach Abschluss aller aktiven Ausführungen auslösen

Wiederholungstrigger können so konfiguriert werden, dass sie nur ausgelöst werden, wenn alle aktiven Ausführungen abgeschlossen sind. Diese Einstellung können Sie konfigurieren, indem Sie die `operationOptions`-Eigenschaft auf `singleInstance` festlegen:

```json
"myTrigger": {
    "type": "Http",
    "inputs": { },
    "recurrence": { },
    "operationOptions": "singleInstance"
}
```

Falls eine geplante Wiederholung stattfindet, während bereits eine Workflowinstanz ausgeführt wird, wird der Trigger übersprungen und im nächsten Wiederholungsintervall eine erneute Prüfung durchgeführt.

## <a name="actions-overview"></a>Übersicht über Aktionen

Es gibt viele Arten von Aktionen, die jeweils über ein eigenes Verhalten verfügen. Jeder Aktionstyp verfügt über verschiedene Eingaben, die sein Verhalten definieren. Collection-Aktionen können zahlreiche weitere Aktionen enthalten. 

### <a name="standard-actions"></a>Standardaktionen  

| Aktionstyp | BESCHREIBUNG | 
| ----------- | ----------- | 
| **HTTP** | Fragt einen HTTP-Webendpunkt ab. | 
| **ApiConnection**  | Vergleichbar mit der HTTP-Aktion, verwendet aber [von Microsoft verwaltete APIs](https://docs.microsoft.com/azure/connectors/apis-list). | 
| **ApiConnectionWebhook** | Vergleichbar mit „HTTPWebhook“, verwendet aber von Microsoft verwaltete APIs. | 
| **Antwort** | Definiert die Antwort für einen eingehenden Aufruf. | 
| **Verfassen** | Erstellt ein beliebiges Objekt auf der Grundlage der Aktionseingaben. | 
| **Function** | Stellt eine Azure-Funktion dar. | 
| **Wait** | Wartet eine bestimmte Zeit oder bis zu einer bestimmten Zeit. | 
| **Workflow** | Stellt einen geschachtelten Workflow dar. | 
| **Verfassen** | Erstellt ein beliebiges Objekt auf der Grundlage der Aktionseingaben. | 
| **Abfrage** | Filtert ein Array auf der Grundlage einer Bedingung. | 
| **Auswählen** | Projiziert die einzelnen Elemente eines Arrays in einen neuen Wert. So können Sie beispielsweise ein Zahlenarray in ein Objektarray konvertieren. | 
| **Tabelle** | Konvertiert ein Array von Elementen in eine CSV- oder HTML-Tabelle. | 
| **Terminate** | Beendet eine Workflowausführung. | 
| **Wait** | Wartet eine bestimmte Zeit oder bis zu einer bestimmten Zeit. | 
| **Workflow** | Stellt einen geschachtelten Workflow dar. | 
||| 

### <a name="collection-actions"></a>Auflistungsaktionen

| Aktionstyp | BESCHREIBUNG | 
| ----------- | ----------- | 
| **If** | Wertet einen Ausdruck aus und führt abhängig vom Ergebnis die entsprechende Verzweigung aus. | 
| **Switch** | Führt verschiedene Aktionen basierend auf bestimmten Werten eines Objekts aus. | 
| **ForEach** | Diese Schleifenaktion durchläuft ein Array und führt interne Aktionen für die einzelnen Arrayelemente aus. | 
| **Until** | Diese Schleifenaktion führt interne Aktionen aus, bis eine Bedingung erfüllt ist. | 
| **Umfang** | Dient zum logischen Gruppieren anderer Aktionen. | 
|||  

## <a name="http-action"></a>HTTP-Aktion  

Eine HTTP-Aktion fragt einen angegebenen Endpunkt ab und überprüft anhand der Antwort, ob der Workflow ausgeführt werden soll oder nicht. Beispiel: 
  
```json
"myLatestNewsAction": {
    "type": "Http",
    "inputs": {
        "method": "GET",
        "uri": "https://mynews.example.com/latest"
    }
}
```

Hier verwendet das `inputs`-Objekt die folgenden, zum Erstellen eines HTTP-Aufrufs erforderlichen Parameter: 

| Elementname | Erforderlich | Typ | BESCHREIBUNG | 
| ------------ | -------- | ---- | ----------- | 
| method | Ja | Zeichenfolge | Zulässige HTTP-Methoden: „GET“, „POST“, „PUT“, „DELETE“, „PATCH“ oder „HEAD“ | 
| uri | Ja| Zeichenfolge | Der HTTP- oder HTTPS-Endpunkt, den der Trigger überprüft. Maximal zulässige Zeichenfolgengröße: 2 KB | 
| Abfragen | Nein  | Objekt | Stellt die Abfrageparameter dar, die Sie in die URL einbeziehen möchten. <p>`"queries": { "api-version": "2015-02-01" }` fügt der URL beispielsweise `?api-version=2015-02-01` hinzu. | 
| headers | Nein  | Objekt | Stellt die einzelnen Header dar, die in der Anforderung gesendet werden. <p>Verwenden Sie beispielsweise Folgendes, um Sprache und Typ für eine Anforderung festzulegen: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| body | Nein  | Objekt | Stellt die an den Endpunkt gesendete Nutzlast dar. | 
| retryPolicy | Nein  | Objekt | Verwenden Sie dieses Objekt, um das Wiederholungsverhalten bei Fehlern vom Typ „4xx“ oder „5xx“ anzupassen. Weitere Informationen finden Sie unter [Wiederholungsrichtlinien](../logic-apps/logic-apps-exception-handling.md). | 
| operationsOptions | Nein  | Zeichenfolge | Definiert den Satz spezieller Verhaltensweisen, die überschrieben werden sollen. | 
| authentication | Nein  | Objekt | Stellt die gewünschte Authentifizierungsmethode für die Anforderung dar. Weitere Informationen finden Sie unter [Ausgehende Authentifizierung von Scheduler](../scheduler/scheduler-outbound-authentication.md). <p>Neben Scheduler wird auch noch folgende Eigenschaft unterstützt: `authority`. Ohne Angabe wird standardmäßig der Wert `https://login.windows.net` verwendet. Sie können aber einen anderen Wert verwenden (beispielsweise `https://login.windows\-ppe.net`). | 
||||| 

HTTP-Aktionen und APIConnection-Aktionen unterstützen *Wiederholungsrichtlinien*. Eine Wiederholungsrichtlinie gilt für vorübergehende Fehler (HTTP-Statuscodes 408, 429 und 5xx) sowie für Verbindungsausnahmen. Sie können diese Richtlinie wie folgt mit dem `retryPolicy`-Objekt definieren:
  
```json
"retryPolicy": {
    "type": "<retry-policy-type>",
    "interval": <retry-interval>,
    "count": <number-of-retry-attempts>
}
```

Die folgende HTTP-Beispielaktion wiederholt den Abruf der neuesten Nachrichten zweimal, falls ein vorübergehender Fehler vorliegt. Dabei werden insgesamt drei Ausführungen mit einer jeweils 30-sekündigen Verzögerung zwischen den einzelnen Versuchen verwendet:
  
```json
"myLatestNewsAction": {
    "type": "Http",
    "inputs": {
        "method": "GET",
        "uri": "https://mynews.example.com/latest",
        "retryPolicy": {
            "type": "fixed",
            "interval": "PT30S",
            "count": 2
        }
    }
}
```

Das Wiederholungsintervall ist im [ISO 8601-Format](https://en.wikipedia.org/wiki/ISO_8601) angegeben. Der Mindestwert für dieses Intervall beträgt 20 Sekunden und ist gleichzeitig der Standardwert. Der Maximalwert ist eine Stunde. Die standardmäßige und maximale Wiederholungsanzahl beträgt vier Stunden. Ohne Angabe der Wiederholungsrichtliniendefinition wird eine `fixed`-Strategie mit Standardwerten für Wiederholungsanzahl und Intervall verwendet. Wenn Sie die Wiederholungsrichtlinie deaktivieren möchten, legen Sie den Typ auf `None` fest.

### <a name="asynchronous-patterns"></a>Asynchrone Muster

Standardmäßig unterstützen alle HTTP-basierten Aktionen das Standardmuster für asynchrone Vorgänge. Wenn der Remoteserver also durch eine Antwort vom Typ „202 (ZULÄSSIG)“ angibt, dass die Anforderung zur Verarbeitung akzeptiert wird, fragt die Logic Apps-Engine die im Adressheader der Antwort angegebene URL ab, bis ein Endzustand (eine 202-fremde Antwort) erreicht ist.
  
Wenn Sie dieses asynchrone Verhalten deaktivieren möchten, legen Sie `operationOptions` in den Aktionseingaben auf `DisableAsyncPattern` fest. In diesem Fall basiert die Ausgabe der Aktion auf der ersten 202-Antwort des Servers. Beispiel: 
  
```json
"invokeLongRunningOperationAction": {
    "type": "Http",
    "inputs": {
        "method": "POST",
        "uri": "https://host.example.com/resources"
    },
    "operationOptions": "DisableAsyncPattern"
}
```

<a name="asynchronous-limits"></a>

#### <a name="asynchronous-limits"></a>Asynchrone Grenzwerte

Die Dauer eines asynchronen Musters kann auf ein bestimmtes Zeitintervall begrenzt werden. Falls das Zeitintervall abläuft, ohne dass ein Endzustand erreicht wurde, wird der Status der Aktion mit einem `ActionTimedOut`-Code als `Cancelled` markiert. Der Grenzwert für das Timeout wird im ISO 8601-Format angegeben. In diesem Beispiel wird gezeigt, wie Sie Grenzen angeben können:


``` json
"<action-name>": {
    "type": "Workflow|Webhook|Http|ApiConnectionWebhook|ApiConnection",
    "inputs": { },
    "limit": {
        "timeout": "PT10S"
    }
}
```
  
## <a name="apiconnection-action"></a>APIConnection-Aktion

Diese Aktion verweist auf einen von Microsoft verwalteten Connector und erfordert einen Verweis auf eine gültige Verbindung sowie Informationen zur API und zu den Parametern. Beispiel für eine APIConnection-Aktion:

```json
"Send_Email": {
    "type": "ApiConnection",
    "inputs": {
        "host": {
            "api": {
                "runtimeUrl": "https://logic-apis-df.azure-apim.net/apim/office365"
            },
            "connection": {
                "name": "@parameters('$connections')['office365']['connectionId']"
            }
        },
        "method": "POST",
        "body": {
            "Subject": "New tweet from @{triggerBody()['TweetedBy']}",
            "Body": "@{triggerBody()['TweetText']}",
            "To": "me@example.com"
        },
        "path": "/Mail"
    },
    "runAfter": {}
}
```

| Elementname | Erforderlich | Typ | BESCHREIBUNG | 
| ------------ | -------- | ---- | ----------- | 
| host | Ja | Objekt | Stellt die Connectorinformationen dar (beispielsweise `runtimeUrl` und Verweis auf das Verbindungsobjekt). | 
| method | Ja | Zeichenfolge | Zulässige HTTP-Methoden: „GET“, „POST“, „PUT“, „DELETE“, „PATCH“ oder „HEAD“ | 
| path | Ja | Zeichenfolge | Der Pfad für den API-Vorgang. | 
| Abfragen | Nein  | Objekt | Stellt die Abfrageparameter dar, die Sie in die URL einbeziehen möchten. <p>`"queries": { "api-version": "2015-02-01" }` fügt der URL beispielsweise `?api-version=2015-02-01` hinzu. | 
| headers | Nein  | Objekt | Stellt die einzelnen Header dar, die in der Anforderung gesendet werden. <p>Verwenden Sie beispielsweise Folgendes, um Sprache und Typ für eine Anforderung festzulegen: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| body | Nein  | Objekt | Stellt die an den Endpunkt gesendete Nutzlast dar. | 
| retryPolicy | Nein  | Objekt | Verwenden Sie dieses Objekt, um das Wiederholungsverhalten bei Fehlern vom Typ „4xx“ oder „5xx“ anzupassen. Weitere Informationen finden Sie unter [Wiederholungsrichtlinien](../logic-apps/logic-apps-exception-handling.md). | 
| operationsOptions | Nein  | Zeichenfolge | Definiert den Satz spezieller Verhaltensweisen, die überschrieben werden sollen. | 
| authentication | Nein  | Objekt | Stellt die gewünschte Authentifizierungsmethode für die Anforderung dar. Weitere Informationen finden Sie unter [Ausgehende Authentifizierung von Scheduler](../scheduler/scheduler-outbound-authentication.md). |
||||| 

Eine Wiederholungsrichtlinie gilt für vorübergehende Fehler (HTTP-Statuscodes 408, 429 und 5xx) sowie für Verbindungsausnahmen. Sie können diese Richtlinie wie folgt mit dem `retryPolicy`-Objekt definieren:

```json
"retryPolicy": {
    "type": "<retry-policy-type>",
    "interval": <retry-interval>,
    "count": <number-of-retry-attempts>
}
```

## <a name="apiconnection-webhook-action"></a>APIConnectionWebhook-Aktion

Die APIConnectionWebhook-Aktion verweist auf einen von Microsoft verwalteten Connector. Diese Aktion erfordert einen Verweis auf eine gültige Verbindung sowie Informationen zur API und zu den Parametern. Grenzwerte für eine Webhookaktion können auf die gleiche Weise angegeben werden wie [asynchrone Grenzwerte für HTTP](#asynchronous-limits).

```json
"Send_approval_email": {
    "type": "ApiConnectionWebhook",
    "inputs": {
        "host": {
            "api": {
                "runtimeUrl": "https://logic-apis-df.azure-apim.net/apim/office365"
            },
            "connection": {
                "name": "@parameters('$connections')['office365']['connectionId']"
            }
        },
        "body": {
            "Message": {
                "Subject": "Approval Request",
                "Options": "Approve, Reject",
                "Importance": "Normal",
                "To": "me@email.com"
            }
        },
        "path": "/approvalmail",
        "authentication": "@parameters('$authentication')"
    },
    "runAfter": {}
}
```

| Elementname | Erforderlich | Typ | BESCHREIBUNG | 
| ------------ | -------- | ---- | ----------- | 
| host | Ja | Objekt | Stellt die Connectorinformationen dar (beispielsweise `runtimeUrl` und Verweis auf das Verbindungsobjekt). | 
| path | Ja | Zeichenfolge | Der Pfad für den API-Vorgang. | 
| Abfragen | Nein  | Objekt | Stellt die Abfrageparameter dar, die Sie in die URL einbeziehen möchten. <p>`"queries": { "api-version": "2015-02-01" }` fügt der URL beispielsweise `?api-version=2015-02-01` hinzu. | 
| headers | Nein  | Objekt | Stellt die einzelnen Header dar, die in der Anforderung gesendet werden. <p>Verwenden Sie beispielsweise Folgendes, um Sprache und Typ für eine Anforderung festzulegen: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| body | Nein  | Objekt | Stellt die an den Endpunkt gesendete Nutzlast dar. | 
| retryPolicy | Nein  | Objekt | Verwenden Sie dieses Objekt, um das Wiederholungsverhalten bei Fehlern vom Typ „4xx“ oder „5xx“ anzupassen. Weitere Informationen finden Sie unter [Wiederholungsrichtlinien](../logic-apps/logic-apps-exception-handling.md). | 
| operationsOptions | Nein  | Zeichenfolge | Definiert den Satz spezieller Verhaltensweisen, die überschrieben werden sollen. | 
| authentication | Nein  | Objekt | Stellt die gewünschte Authentifizierungsmethode für die Anforderung dar. Weitere Informationen finden Sie unter [Ausgehende Authentifizierung von Scheduler](../scheduler/scheduler-outbound-authentication.md). |
||||| 

## <a name="response-action"></a>Antwortaktion  

Diese Aktion enthält die gesamte Antwortnutzlast aus einer HTTP-Anforderung sowie `statusCode`, `body` und `headers`:
  
```json
"myResponseAction": {
    "type": "Response",
    "inputs": {
        "statusCode": 200,
        "body": {
            "contentFieldOne": "value100",
            "anotherField": 10.001
        },
        "headers": {
            "x-ms-date": "@utcnow()",
            "Content-type": "application/json"
        }
    },
    "runAfter": {}
}
```

Bei der Response-Aktion müssen besondere Einschränkungen berücksichtigt werden, die bei anderen Aktionen nicht gelten:  
  
* Response-Aktionen können innerhalb einer Logik-App nicht in parallelen Verzweigungen verwendet werden, da die eingehende Anforderung eine deterministische Antwort erfordert.
  
* Falls der Workflow eine Response-Aktion erreicht, nachdem die eingehende Anforderung bereits eine Antwort empfangen hat, wird die Response-Aktion als fehlerhaft oder als Konflikt betrachtet. Die Logik-App-Ausführung wird daraufhin als `Failed` gekennzeichnet.
  
* Bei einem Workflow mit Response-Aktionen kann in der Triggerdefinition der Befehl `splitOn` nicht verwendet werden, da der Aufruf mehrere Ausführungen erstellt. Bei Verwendung des Workflowvorgangs „PUT“ muss daher geprüft werden, ob dieser Fall vorliegt, und ggf. eine Antwort vom Typ „Ungültige Anforderung“ zurückgegeben werden.

## <a name="compose-action"></a>Compose-Aktion

Mit dieser Aktion können Sie ein beliebiges Objekt erstellen. Bei der Ausgabe handelt es sich um das Ergebnis der Auswertung der Aktionseingaben. 

> [!NOTE]
> Die `Compose`-Aktion kann zur Erstellung einer beliebigen Ausgabe verwendet werden. Hierzu zählen unter anderem Objekte und Arrays sowie jede andere Art von Ausgabe, die nativ von Logik-Apps unterstützt wird (etwa XML- und Binärelemente).

Mithilfe der `Compose`-Aktion können Sie beispielsweise Ausgaben mehrerer Aktionen zusammenführen:

```json
"composeUserRecordAction": {
    "type": "Compose",
    "inputs": {
        "firstName": "@actions('getUser').firstName",
        "alias": "@actions('getUser').alias",
        "thumbnailLink": "@actions('lookupThumbnail').url"
    }
}
```

## <a name="function-action"></a>Function-Aktion

Mit dieser Aktion können Sie eine [Azure-Funktion](../azure-functions/functions-overview.md) darstellen und aufrufen. Beispiel:

```json
"<my-Azure-Function-name>": {
   "type": "Function",
    "inputs": {
        "function": {
            "id": "/subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.Web/sites/<your-Azure-function-app-name>/functions/<your-Azure-function-name>"
        },
        "queries": {
            "extrafield": "specialValue"
        },  
        "headers": {
            "x-ms-date": "@utcnow()"
        },
        "method": "POST",
        "body": {
            "contentFieldOne": "value100",
            "anotherField": 10.001
        }
    },
    "runAfter": {}
}
```

| Elementname | Erforderlich | Typ | BESCHREIBUNG | 
| ------------ | -------- | ---- | ----------- |  
| Funktions-ID | Ja | Zeichenfolge | Die Ressourcen-ID für die Azure-Funktion, die Sie aufrufen möchten. | 
| method | Nein  | Zeichenfolge | Die HTTP-Methode zum Aufrufen der Funktion. Ohne Angabe wird standardmäßig „POST“ verwendet. | 
| Abfragen | Nein  | Objekt | Stellt die Abfrageparameter dar, die Sie in die URL einbeziehen möchten. <p>`"queries": { "api-version": "2015-02-01" }` fügt der URL beispielsweise `?api-version=2015-02-01` hinzu. | 
| headers | Nein  | Objekt | Stellt die einzelnen Header dar, die in der Anforderung gesendet werden. <p>Verwenden Sie beispielsweise Folgendes, um Sprache und Typ für eine Anforderung festzulegen: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| body | Nein  | Objekt | Stellt die an den Endpunkt gesendete Nutzlast dar. | 
|||||

Beim Speichern Ihrer Logik-App führt die Logic Apps-Engine einige Überprüfungen der referenzierten Funktion durch:

* Sie müssen auf die Funktion zugreifen können.
* Sie können nur einen HTTP-Standardtrigger oder generischen JSON-Webhooktrigger verwenden.
* Für die Funktion darf keine Route definiert sein.
* Als Autorisierungsebenen sind nur „function“ und „anonymous“ zulässig.

> [!NOTE]
> Die Logic Apps-Engine ruft die zur Runtime verwendete Trigger-URL ab und zwischenspeichert sie. Sollte die zwischengespeicherte URL also aufgrund eines Vorgangs ungültig werden, tritt zur Laufzeit ein Fehler bei der Aktion auf. Speichern Sie zur Umgehung dieses Problems die Logik-App erneut. Dadurch wird die Trigger-URL erneut abgerufen und zwischengespeichert.

## <a name="select-action"></a>Aktion select

Mit dieser Aktion können Sie die einzelnen Elemente eines Arrays in einen neuen Wert projizieren. In diesem Beispiel wird ein Zahlenarray in ein Objektarray konvertiert:

```json
"selectNumbersAction": {
    "type": "Select",
    "inputs": {
        "from": [ 1, 3, 0, 5, 4, 2 ],
        "select": { "number": "@item()" }
    }
}
```

| NAME | Erforderlich | Typ | BESCHREIBUNG | 
| ---- | -------- | ---- | ----------- | 
| from | Ja | Array | Das Quellarray. |
| select | Ja | Beliebig | Die Projektion, die auf die einzelnen Elemente des Quellarrays angewendet wird. |
||||| 

Die `select`-Aktion gibt ein Array aus, dessen Kardinalität mit der des Eingabearrays identisch ist. Jedes Element wird gemäß der Definition der `select`-Eigenschaft transformiert. Wenn die Eingabe ein leeres Array ist, wird auch ein leeres Array ausgegeben.

## <a name="terminate-action"></a>Terminate-Aktion

Diese Aktion beendet eine Workflowausführung, wobei alle laufenden Aktionen abgebrochen und alle übrigen übersprungen werden. Die Terminate-Aktion hat keine Auswirkungen auf bereits abgeschlossene Aktionen.

So wird beispielsweise eine Ausführung mit dem `Failed`-Status beendet:

```json
"HandleUnexpectedResponse": {
    "type": "Terminate",
    "inputs": {
        "runStatus": "Failed",
        "runError": {
            "code": "UnexpectedResponse",
            "message": "Received an unexpected response",
        }
    }
}
```

| NAME | Erforderlich | Typ | BESCHREIBUNG | 
| ---- | -------- | ---- | ----------- | 
| runStatus | Ja | Zeichenfolge | Der Status der Zielausführung (entweder `Failed` oder `Cancelled`). |
| runError | Nein  | Objekt | Die Fehlerdetails. Wird nur unterstützt, wenn `runStatus` auf `Failed` festgelegt ist. |
| Code für „runError“ | Nein  | Zeichenfolge | Der Fehlercode der Ausführung. |
| runError message | Nein  | Zeichenfolge | Die Fehlermeldung der Ausführung. | 
||||| 

## <a name="query-action"></a>Abfrageaktion

Mit dieser Aktion können Sie ein Array auf der Grundlage einer Bedingung filtern. 

> [!NOTE]
> Mit der Aktion „Compose“ können Sie keine beliebige Ausgabe erstellen. Hierzu zählen unter anderem Objekte und Arrays sowie jede andere Art von Ausgabe, die nativ von Logik-Apps unterstützt wird (etwa XML- und Binärelemente).

So können Sie beispielsweise Zahlen auswählen, die größer als zwei sind:

```json
"filterNumbersAction": {
    "type": "Query",
    "inputs": {
        "from": [ 1, 3, 0, 5, 4, 2 ],
        "where": "@greater(item(), 2)"
    }
}
```

| NAME | Erforderlich | Typ | BESCHREIBUNG | 
| ---- | -------- | ---- | ----------- | 
| from | Ja | Array | Das Quellarray. |
| Hierbei gilt: | Ja | Zeichenfolge | Die Bedingung, die auf die einzelnen Elemente aus dem Quellarray angewendet wird. Sollten keine Werte die Bedingung von `where` erfüllen, ist das Ergebnis ein leeres Array. |
||||| 

Die Ausgabe der Aktion `query` ist ein Array mit Elementen aus dem Eingabearray, die die Bedingung erfüllen.

## <a name="table-action"></a>Aktion table

Mit dieser Aktion können Sie ein Array in eine CSV- oder HTML-Tabelle konvertieren. 

```json
"ConvertToTable": {
    "type": "Table",
    "inputs": {
        "from": "<source-array>",
        "format": "CSV | HTML"
    }
}
```

| NAME | Erforderlich | Typ | BESCHREIBUNG | 
| ---- | -------- | ---- | ----------- | 
| from | Ja | Array | Das Quellarray. Wenn der Eigenschaftswert `from` ein leeres Array ist, wird eine leere Tabelle ausgegeben. | 
| format | Ja | Zeichenfolge | Das gewünschte Tabellenformat („CSV“ oder „HTML“). | 
| columns | Nein  | Array | Die gewünschten Tabellenspalten. Dient zum Überschreiben der Standardtabellenform. | 
| column header | Nein  | Zeichenfolge | Die Spaltenüberschrift. | 
| column value | Ja | Zeichenfolge | Der Spaltenwert. | 
||||| 

Nehmen wir an, Sie definieren eine Tabellenaktion wie im folgenden Beispiel:

```json
"convertToTableAction": {
    "type": "Table",
    "inputs": {
        "from": "@triggerBody()",
        "format": "HTML"
    }
}
```

Und verwenden dieses Array für `@triggerBody()`:

```json
[ {"ID": 0, "Name": "apples"},{"ID": 1, "Name": "oranges"} ]
```

Dies ist die Ausgabe aus diesem Beispiel:

<table><thead><tr><th>ID</th><th>NAME</th></tr></thead><tbody><tr><td>0</td><td>apples</td></tr><tr><td>1</td><td>oranges</td></tr></tbody></table>

Sie können die Tabelle anpassen, indem Sie die Spalten explizit angeben. Beispiel:

```json
"ConvertToTableAction": {
    "type": "Table",
    "inputs": {
        "from": "@triggerBody()",
        "format": "html",
        "columns": [ 
            {
                "header": "Produce ID",
                "value": "@item().id"
            },
            {
              "header": "Description",
              "value": "@concat('fresh ', item().name)"
            }
        ]
    }
}
```

Dies ist die Ausgabe aus diesem Beispiel:

<table><thead><tr><th>Produce ID</th><th>BESCHREIBUNG</th></tr></thead><tbody><tr><td>0</td><td>fresh apples</td></tr><tr><td>1</td><td>fresh oranges</td></tr></tbody></table>

## <a name="wait-action"></a>Wait-Aktion  

Diese Aktion hält die Ausführung des Workflows für das angegebene Intervall an. Im folgenden Beispiel wartet der Workflow 15 Minuten:
  
```json
"waitForFifteenMinutesAction": {
    "type": "Wait",
    "inputs": {
        "interval": {
            "unit": "minute",
            "count": 15
        }
    }
}
```
  
Alternativ kann bis zu einem bestimmten Zeitpunkt gewartet werden, wie im folgenden Beispiel zu sehen:
  
```json
"waitUntilOctoberAction": {
    "type": "Wait",
    "inputs": {
        "until": {
            "timestamp": "2017-10-01T00:00:00Z"
        }
    }
}
```
  
> [!NOTE]  
> Sie können die Wartedauer entweder mit dem `interval`-Objekt oder dem `until`-Objekt angeben, aber nicht mit beiden.

| Elementname | Erforderlich | Typ | BESCHREIBUNG | 
| ------------ | -------- | ---- | ----------- | 
| until | Nein  | Objekt | Die Wartezeit auf der Grundlage eines Zeitpunkts. | 
| Zeitstempel für „until“ | Ja | Zeichenfolge | Der Zeitpunkt (im [UTC-Datums-/Uhrzeitformat](https://en.wikipedia.org/wiki/Coordinated_Universal_Time)), zu dem die Wartezeit abläuft. | 
| interval | Nein  | Objekt | Die Wartezeit auf der Grundlage von Intervalleinheit und Anzahl. | 
| interval unit | Ja | Zeichenfolge | Die Zeiteinheit. Zulässige Werte: „second“, „minute“, „hour“, „day“, „week“ oder „month“ | 
| interval count | Ja | Ganze Zahl  | Eine positive ganze Zahl, die die Anzahl von Intervalleinheiten für die Wartezeit darstellt. | 
||||| 

## <a name="workflow-action"></a>Workflow-Aktion

Mit dieser Aktion können Sie einen Workflow schachteln. Die Logic Apps-Engine führt eine Zugriffsprüfung für den untergeordneten Workflow (genauer gesagt: für den Trigger) durch. Sie müssen also auf den untergeordneten Workflow zugreifen können. Beispiel: 

```json
"<my-nested-workflow-action-name>": {
    "type": "Workflow",
    "inputs": {
        "host": {
            "id": "/subscriptions/<my-subscription-ID>/resourceGroups/<my-resource-group-name>/providers/Microsoft.Logic/<my-nested-workflow-action-name>",
            "triggerName": "mytrigger001"
        },
        "queries": {
            "extrafield": "specialValue"
        },  
        "headers": {
            "x-ms-date": "@utcnow()",
            "Content-type": "application/json"
        },
        "body": {
            "contentFieldOne": "value100",
            "anotherField": 10.001
        }
    },
    "runAfter": {}
}
```

| Elementname | Erforderlich | Typ | BESCHREIBUNG | 
| ------------ | -------- | ---- | ----------- |  
| host id | Ja | Zeichenfolge| Die Ressourcen-ID für den Workflow, den Sie aufrufen möchten. | 
| host triggerName | Ja | Zeichenfolge | Der Name des Triggers, den Sie aufrufen möchten. | 
| Abfragen | Nein  | Objekt | Stellt die Abfrageparameter dar, die Sie in die URL einbeziehen möchten. <p>`"queries": { "api-version": "2015-02-01" }` fügt der URL beispielsweise `?api-version=2015-02-01` hinzu. | 
| headers | Nein  | Objekt | Stellt die einzelnen Header dar, die in der Anforderung gesendet werden. <p>Verwenden Sie beispielsweise Folgendes, um Sprache und Typ für eine Anforderung festzulegen: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| body | Nein  | Objekt | Stellt die an den Endpunkt gesendete Nutzlast dar. | 
||||| 

Die Ausgaben der Aktion basieren auf den Definitionen in der `Response`-Aktion für den untergeordneten Workflow. Wenn der untergeordnete Workflow keine `Response`-Aktion definiert, sind die Ausgaben leer.

## <a name="collection-actions-overview"></a>Übersicht über Collection-Aktionen

Um Ihnen beim Steuern der Workflowausführung zu helfen, können Sammlungsaktionen andere Aktionen enthalten. Sie können außerhalb einer Sammlung direkt auf Verweisaktionen in der Sammlung verweisen. Wenn Sie also etwa eine `Http`-Aktion in einem Bereich definieren, ist `@body('http')` weiterhin im gesamten Workflow gültig. Es ist auch möglich, dass Aktionen in einer Sammlung nur „nach“ anderen Aktionen in der gleichen Sammlung ausgeführt werden können.

## <a name="if-action"></a>If-Aktion

Mit dieser Aktion – einer Bedingungsanweisung – können Sie eine Bedingung auswerten und die Ausführung einer Verzweigung davon abhängig machen, ob die Auswertung des Ausdrucks „true“ ergibt. Bei erfolgreicher Auswertung als „true“ wird die Bedingung als „Erfolgreich“ markiert. Aktionen im `actions`- oder `else`-Objekt werden nach diesen Werten ausgewertet:

* „Erfolgreich“, wenn sie erfolgreich ausgeführt wurden
* „Fehlerhaft“, wenn sie nicht erfolgreich ausgeführt wurden
* „Übersprungen“, wenn die entsprechende Verzweigung nicht ausgeführt wird

Weitere Informationen finden Sie unter [Bedingungsanweisungen: Ausführen von Schritten auf Basis einer Bedingung in Logik-Apps](../logic-apps/logic-apps-control-flow-conditional-statement.md).

``` json
"<my-condition-name>": {
  "type": "If",
  "expression": "<condition>",
  "actions": {
    "if-true-run-this-action": {
      "type": <action-type>,
      "inputs": {},
      "runAfter": {}
    }
  },
  "else": {
    "actions": {
        "if-false-run-this-action": {
            "type": <action-type>,
            "inputs": {},
            "runAfter": {}
        }
    }
  },
  "runAfter": {}
}
```

| NAME | Erforderlich | Typ | BESCHREIBUNG | 
| ---- | -------- | ---- | ----------- | 
| Aktionen | Ja | Objekt | Die internen Aktionen, die ausgeführt werden sollen, wenn `expression` als `true` ausgewertet wird. | 
| expression | Ja | Zeichenfolge | Der auszuwertende Ausdruck. |
| else | Nein  | Objekt | Die internen Aktionen, die ausgeführt werden sollen, wenn `expression` als `false` ausgewertet wird. |
||||| 

Beispiel: 

```json
"myCondition": {
    "type": "If",
    "actions": {
        "if-true-check-this-website": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://this-url"
            },
            "runAfter": {}
        }
    },
    "else": {
        "actions": {
            "if-false-check-this-other-website": {
                "type": "Http",
                "inputs": {
                    "method": "GET",
                    "uri": "http://this-other-url"
                },
                "runAfter": {}
            }
        }
    }
}
```  

### <a name="how-conditions-can-use-expressions-in-actions"></a>Wie Bedingungen Ausdrücke in Aktionen verwenden können

Die folgenden Beispiele veranschaulichen die Verwendung von Ausdrücken in Bedingungen:
  
| JSON-Ausdruck | Ergebnis | 
| --------------- | ------ | 
| `"expression": "@parameters('hasSpecialAction')"` | Jeder Wert, der als „true“ ausgewertet wird, bewirkt, dass diese Bedingung als erfüllt betrachtet wird. Unterstützt nur boolesche Ausdrücke. Andere Typen können mithilfe der Funktion `empty` oder `equals` in boolesche Werte konvertiert werden. | 
| `"expression": "@greater(actions('action1').output.value, parameters('threshold'))"` | Unterstützt Vergleichsfunktionen. In diesem Beispiel wird die Aktion nur ausgeführt, wenn die Ausgabe von „action1“ den Schwellenwert übersteigt. | 
| `"expression": "@or(greater(actions('action1').output.value, parameters('threshold')), less(actions('action1').output.value, 100))"` | Unterstützt Logikfunktionen zur Erstellung geschachtelter boolescher Ausdrücke. In diesem Beispiel wird die Aktion ausgeführt, wenn die Ausgabe von „action1“ den Schwellenwert übersteigt oder unter 100 liegt. | 
| `"expression": "@equals(length(actions('action1').outputs.errors), 0))"` | Mithilfe von Arrayfunktionen können Sie prüfen, ob ein Array Elemente enthält. In diesem Beispiel wird die Aktion ausgeführt, wenn das errors-Array leer ist. | 
| `"expression": "parameters('hasSpecialAction')"` | Dieser Ausdruck verursacht einen Fehler und ist keine gültige Bedingung. Bedingungen müssen das „@“-Symbol verwenden. | 
||| 

## <a name="switch-action"></a>Switch-Aktion

Diese Aktion – eine Switch-Anweisung – führt verschiedene Aktionen basierend auf bestimmten Werten eines Objekts, Ausdrucks oder Tokens aus. Diese Aktion wertet das Objekt, den Ausdruck oder das Token aus, wählt den Fall, der dem Ergebnis entspricht, und führt Aktionen nur für diesen Fall aus. Wenn kein Fall dem Ergebnis entspricht, wird die Standardaktion ausgeführt. Beim Ausführen der Switch-Anweisung sollte nur ein Fall mit dem Ergebnis übereinstimmen. Weitere Informationen finden Sie unter [Switch-Anweisungen: Ausführen von verschiedenen Schritten auf Basis einer Bedingung in Logik-Apps](../logic-apps/logic-apps-control-flow-switch-statement.md).

``` json
"<my-switch-statement-name>": {
   "type": "Switch",
   "expression": "<evaluate-this-object-expression-token>",
   "cases": {
      "myCase1" : {
         "actions" : {
           "myAction1": {}
         },
         "case": "<result1>"
      },
      "myCase2": {
         "actions" : {
           "myAction2": {}
         },
         "case": "<result2>"
      }
   },
   "default": {
      "actions": {
          "myDefaultAction": {}
      }
   },
   "runAfter": {}
}
```

| NAME | Erforderlich | Typ | BESCHREIBUNG | 
| ---- | -------- | ---- | ----------- | 
| expression | Ja | Zeichenfolge | Objekt, Ausdruck oder Token, das/der ausgewertet werden soll | 
| cases | Ja | Objekt | Enthält die Sätze von inneren Aktionen, die basierend auf dem Ergebnis des Ausdrucks ausgeführt werden. | 
| case | Ja | Zeichenfolge | Der Wert, der mit dem Ergebnis übereinstimmen soll | 
| Aktionen | Ja | Objekt | Die inneren Aktionen, die für den Fall ausgeführt werden, der dem Ergebnis des Ausdrucks entspricht. | 
| die Standardeinstellung | Nein  | Objekt | Die inneren Aktionen, die ausgeführt werden, wenn keine Fälle mit dem Ergebnis übereinstimmen. | 
||||| 

Beispiel: 

``` json
"myApprovalEmailAction": {
   "type": "Switch",
   "expression": "@body('Send_approval_email')?['SelectedOption']",
   "cases": {
      "Case": {
         "actions" : {
           "Send_an_email": {...}
         },
         "case": "Approve"
      },
      "Case_2": {
         "actions" : {
           "Send_an_email_2": {...}
         },
         "case": "Reject"
      }
   },
   "default": {
      "actions": {}
   },
   "runAfter": {
      "Send_approval_email": [
         "Succeeded"
      ]
   }
}
```

## <a name="foreach-action"></a>ForEach-Aktion

Diese Schleifenaktion durchläuft ein Array und führt interne Aktionen für die einzelnen Arrayelemente aus. Standardmäßig wird die ForEach-Schleife parallel ausgeführt. Welche maximale Anzahl von parallelen Zyklen ForEach-Schleifen ausführen können, erfahren Sie unter [Logic Apps-Grenzwerte und -Konfiguration](../logic-apps/logic-apps-limits-and-config.md). Um die Zyklen sequenziell auszuführen, legen Sie den `operationOptions`-Parameter auf `Sequential` fest. Weitere Informationen finden Sie unter [ForEach-Schleife](../logic-apps/logic-apps-control-flow-loops.md#foreach-loop).

```json
"<my-forEach-loop-name>": {
    "type": "Foreach",
    "actions": {
        "myInnerAction1": {
            "type": "<action-type>",
            "inputs": {}
        },
        "myInnerAction2": {
            "type": "<action-type>",
            "inputs": {}
        }
    },
    "foreach": "<array>",
    "runAfter": {}
}
```

| NAME | Erforderlich | Typ | BESCHREIBUNG | 
| ---- | -------- | ---- | ----------- | 
| Aktionen | Ja | Objekt | Die internen Aktionen, die innerhalb der Schleife ausgeführt werden sollen. | 
| foreach | Ja | Zeichenfolge | Das zu durchlaufende Array. | 
| operationOptions | Nein  | Zeichenfolge | Dient zum Angeben von Vorgangsoptionen für die Verhaltensanpassung. Unterstützt derzeit nur `Sequential` für sequenziell ausgeführte Iterationen mit dem Standardverhalten „Parallel“. |
||||| 

Beispiel: 

```json
"forEach_EmailAction": {
    "type": "Foreach",
    "foreach": "@body('email_filter')",
    "actions": {
        "Send_email": {
            "type": "ApiConnection",
            "inputs": {
                "body": {
                    "to": "@item()",
                    "from": "me@contoso.com",
                    "message": "Hello, thank you for ordering"
                },
                "host": {
                    "connection": {
                        "id": "@parameters('$connections')['office365']['connection']['id']"
                    }
                }
            }
        }
    },
    "foreach": "@body('email_filter')",
    "runAfter": {
        "email_filter": [ "Succeeded" ]
    }
}
```

## <a name="until-action"></a>Until-Aktion

Diese Schleifenaktion führt interne Aktionen aus, bis eine Bedingung als „true“ ausgewertet wird. Weitere Informationen finden Sie unter [„Until“-Schleifen](../logic-apps/logic-apps-control-flow-loops.md#until-loop).

```json
 "<my-Until-loop-name>": {
    "type": "Until",
    "actions": {
        "myActionName": {
            "type": "<action-type>",
            "inputs": {},
            "runAfter": {}
        }
    },
    "expression": "<myCondition>",
    "limit": {
        "count": 1000,
        "timeout": "PT1H"
    },
    "runAfter": {}
}
```

| NAME | Erforderlich | Typ | BESCHREIBUNG | 
| ---- | -------- | ---- | ----------- | 
| Aktionen | Ja | Objekt | Die internen Aktionen, die innerhalb der Schleife ausgeführt werden sollen. | 
| expression | Ja | Zeichenfolge | Der Ausdruck, der nach jeder Iteration ausgewertet werden soll. | 
| limit | Ja | Objekt | Die Grenzwerte für die Schleife. Es muss mindestens ein Grenzwert definiert werden. | 
| count | Nein  | Ganze Zahl  | Der Grenzwert für die Anzahl durchzuführender Iterationen. | 
| timeout | Nein  | Zeichenfolge | Das Zeitlimit (im [ISO 8601-Format](https://en.wikipedia.org/wiki/ISO_8601)), das angibt, wie lange die Schleife ausgeführt werden soll. |
||||| 

Beispiel: 

```json
 "runUntilSucceededAction": {
    "type": "Until",
    "actions": {
        "Http": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {}
        }
    },
    "expression": "@equals(outputs('Http')['statusCode', 200)",
    "limit": {
        "count": 100,
        "timeout": "PT1H"
    },
    "runAfter": {}
}
```

## <a name="scope-action"></a>Scope-Aktion

Mit dieser Aktion können Sie Aktionen in einem Workflow logisch gruppieren. Der Bereich erhält auch seinen Status, nachdem die Ausführung aller Aktionen in diesem Bereich beendet ist. Weitere Informationen finden Sie unter [Bereiche: Ausführen von Schritten auf Basis des Gruppenstatus in Logik-Apps](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md).

```json
"<my-scope-action-name>": {
    "type": "Scope",
    "actions": {
        "myInnerAction1": {
            "type": "<action-type>",
            "inputs": {}
        },
        "myInnerAction2": {
            "type": "<action-type>",
            "inputs": {}
        }
    }
}
```

| NAME | Erforderlich | Typ | BESCHREIBUNG | 
| ---- | -------- | ---- | ----------- |  
| Aktionen | Ja | Objekt | Die internen Aktionen, die innerhalb des Bereichs ausgeführt werden sollen. |
||||| 

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über das [Schema der Definitionssprache für Workflows für Azure Logic Apps](../logic-apps/logic-apps-workflow-definition-language.md).
* Erfahren Sie mehr über die [Workflow-REST-API](https://docs.microsoft.com/rest/api/logic/workflows).