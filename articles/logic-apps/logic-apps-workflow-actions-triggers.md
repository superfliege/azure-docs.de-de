---
title: "Workflowtrigger und -aktionen – Azure Logic Apps | Microsoft-Dokumentation"
description: "Hier erfahren Sie mehr über die Arten von Triggern und Aktionen, die Sie zur Erstellung und Automatisierung von Workflows und Prozessen mit Azure Logic Apps verwenden können."
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
ms.date: 11/17/2016
ms.author: LADocs; mandia
ms.openlocfilehash: 7e0266cdc477715a5d2f9067c6dcea73da9ba763
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="triggers-and-actions-for-logic-app-workflows"></a>Trigger und Aktionen für Logik-App-Workflows

Alle Logik-Apps beginnen mit einem Trigger, auf den dann Aktionen folgen. In diesem Thema werden die Arten von Triggern und Aktionen beschrieben, die Sie zum Erstellen von Systemintegrationen und zum Automatisieren von Geschäftsworkflows oder -prozessen mit Logik-Apps verwenden können. 
  
## <a name="triggers-overview"></a>Übersicht über Trigger 

Alle Logik-Apps beginnen mit einem Trigger. Dieser gibt die Aufrufe an, die die Ausführung einer Logik-App initiieren können. Die Ausführung Ihres Workflows kann auf zwei Arten initiiert werden:  

* Durch einen Abfragetrigger  
* Durch einen Pushtrigger (der die [Workflow Service-REST-API](https://docs.microsoft.com/rest/api/logic/workflows) aufruft)  
  
Alle Trigger enthalten folgende allgemeine Elemente:  
  
```json
"trigger-name": {
    "type": "trigger-type",
    "inputs": { call-settings },
    "recurrence": {  
        "frequency": "Second|Minute|Hour|Day|Week|Month",
        "interval": recurrence-interval-based-on-frequency
    },
    "conditions": [ array-of-required-conditions ],
    "splitOn": "property-used-for-creating-separate-workflows",
    "operationOptions": "operation-options-for-trigger"
}
```

### <a name="trigger-types-and-inputs"></a>Triggertypen und -eingaben  

Jeder Triggertyp verfügt über eine andere Schnittstelle sowie über verschiedene *Eingaben*, die sein Verhalten definieren. 

| Triggertyp | Beschreibung | 
| ------------ | ----------- | 
| **Serie** | Wird auf der Grundlage eines definierten Zeitplans ausgelöst. Sie können ein Datum und eine Uhrzeit in der Zukunft festlegen, um diesen Trigger auszulösen. Je nach Häufigkeit können Sie auch Zeiten und Tage für die Workflowausführung angeben. | 
| **Anforderung**  | Verwandelt Ihre Logik-App in einen aufrufbaren Endpunkt. (Wird auch als manueller Trigger bezeichnet.) | 
| **HTTP** | Dient zum Überprüfen (oder *Abfragen*) eines HTTP-Webendpunkts. Der HTTP-Endpunkt muss einem bestimmten auslösenden Vertrag entsprechen – entweder durch Verwendung eines asynchronen Musters (202) oder durch Rückgabe eines Arrays. | 
| **ApiConnection** | Vergleichbar mit dem HTTP-Trigger; führt ebenfalls eine Abfrage durch, verwendet aber [von Microsoft verwaltete APIs](../connectors/apis-list.md). | 
| **HTTPWebhook** | Vergleichbar mit dem Anforderungstrigger; verwandelt Ihre Logik-App ebenfalls in einen aufrufbaren Endpunkt, ruft allerdings eine bestimmte URL für die Registrierung/Aufhebung der Registrierung auf. |
| **ApiConnectionWebhook** | Vergleichbar mit dem **HTTPWebhook**-Trigger, verwendet aber von Microsoft verwaltete APIs. | 
||| 

Ausführlichere Informationen finden Sie unter [Schema der Definitionssprache für Workflows für Azure Logic Apps](../logic-apps/logic-apps-workflow-definition-language.md). 
  
## <a name="recurrence-trigger"></a>Recurrence-Trigger  

Dieser Trigger wird auf der Grundlage der angegebenen Wiederholung und des angegebenen Zeitplans ausgeführt, wodurch Sie einen Workflow komfortabel in regelmäßigen Abständen ausführen können. Das folgende Beispiel zeigt einen einfachen Wiederholungstrigger, der täglich ausgeführt wird:

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
        "startTime" : "2017-09-18T00:00:00Z"
    }
}
```

Definition für diesen Trigger: 

```json
"myRecurrenceTrigger": {
    "type": "Recurrence",
    "recurrence": {
        "frequency": "second|minute|hour|day|week|month",
        "interval": recurrence-interval-based-on-frequency,
        "schedule": {
            // Applies only when frequency is Day or Week. Separate values with commas.
            "hours": [ one-or-more-hour-marks ], 
            // Applies only when frequency is Day or Week. Separate values with commas.
            "minutes": [ one-or-more-minute-marks ], 
            // Applies only when frequency is Week. Separate values with commas.
            "weekDays": [ "Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday" ] 
        },
        "startTime": "start-date-time-with-format-YYYY-MM-DDThh:mm:ss",
        "timeZone": "specify-time-zone"
    }
}
```

| Elementname | Erforderlich | Typ | Beschreibung | 
| ------------ | -------- | ---- | ----------- | 
| frequency | Ja | String | Die Zeiteinheit für die Häufigkeit der Triggerauslösung. Zulässige Werte: „second“, „minute“, „hour“, „day“, „week“ oder „month“ | 
| interval | Ja | Integer | Eine positive ganze Zahl, die beschreibt, wie oft der Workflow basierend auf der Häufigkeit ausgeführt wird. <p>Zulässige Mindest- und Maximalintervalle: <p>-Month: 1–16 Monate </br>- Day: 1–500 Tage </br>- Hour: 1–12.000 Stunden </br>-Minute: 1–72.000 Minuten </br>- Second: 1–9.999.999 Sekunden<p>Wenn das Intervall also beispielsweise auf „6“ und die Häufigkeit auf „month“ festgelegt ist, erfolgt die Wiederholung alle sechs Monate. | 
| timeZone | Nein | String | Nur relevant, wenn Sie eine Startzeit angeben, da dieser Trigger keine [UTC-Abweichung](https://en.wikipedia.org/wiki/UTC_offset) akzeptiert. Geben Sie die anzuwendende Zeitzone an. | 
| startTime | Nein | String | Geben Sie Startdatum und -uhrzeit im folgenden Format an: <p>JJJJ-MM-TTTHH:mm:ss (bei Angabe einer Zeitzone; beachten Sie dabei, dass „TT“ den Tag bezeichnet, während das dritte „T“ wie in den nachfolgenden Beispielen gezeigt nicht verändert werden darf) <p>Oder <p>JJJJ-MM-TTTHH:mm:ssZ (ohne Angabe einer Zeitzone) <p>Für „18. September 2017, 14:00 Uhr“ müssten Sie also beispielsweise „2017-09-18T14:00:00“ und eine Zeitzone (etwa „Pacific Standard Time“) angeben. Alternativ können Sie „2017-09-18T14:00:00Z“ ohne Zeitzone angeben. <p>**Hinweis:** Diese Startzeit muss dem [ISO 8601-Format für Datums-/Uhrzeitangaben](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) entsprechen und im [UTC-Datums-/Zeitformat](https://en.wikipedia.org/wiki/Coordinated_Universal_Time), aber ohne [UTC-Abweichung](https://en.wikipedia.org/wiki/UTC_offset) angegeben werden. Wenn Sie keine Zeitzone angeben, müssen Sie den Buchstaben „Z“ ohne Leerzeichen anhängen. „Z“ bezieht sich auf die entsprechende [nautische Zeit](https://en.wikipedia.org/wiki/Nautical_time). <p>Bei einfachen Zeitpläne ist die Startzeit das erste Vorkommen. Bei komplexeren Zeitplänen wird der Trigger nicht vor der Startzeit ausgelöst. Weitere Informationen zu Startdatum und -uhrzeit finden Sie unter [Schedule tasks and workflows that run regularly with logic apps](../connectors/connectors-native-recurrence.md) (Planen von regelmäßig ausgeführten Aufgaben und Workflows mit Logik-Apps). | 
| weekDays | Nein | Zeichenfolge oder Zeichenfolgenarray | Wenn Sie für `frequency` die Option „Week“ angeben, können Sie einen Tag oder eine kommagetrennte Liste mit Tagen für die Workflowausführung angeben: „Monday“, „Tuesday“, „Wednesday“, „Thursday“, „Friday“, „Saturday“ oder „Sunday“ | 
| hours | Nein | Ganze Zahl oder Ganzzahlarray | Wenn Sie für `frequency` die Option „Day“ oder „Week“ angeben, können Sie eine ganze Zahl oder eine kommagetrennte Liste mit ganzen Zahlen von 0 bis 23 als die Stunden des Tages angeben, zu denen der Workflow ausgeführt werden soll. <p>Wenn Sie also etwa „10“, „12“ und „14“ angeben, erhalten Sie die vollen Stunden „10 Uhr“, „12 Uhr“ und „14 Uhr“. | 
| minutes | Nein | Ganze Zahl oder Ganzzahlarray | Wenn Sie für `frequency` die Option „Day“ oder „Week“ angeben, können Sie eine ganze Zahl oder eine kommagetrennte Liste mit ganzen Zahlen von 0 bis 59 als die Minuten der Stunde angeben, zu denen der Workflow ausgeführt werden soll. <p>Wenn Sie also beispielsweise „30“ als Minutenwert angeben und das vorherige Beispiel für Stunden des Tages verwenden, erhalten Sie „10:30 Uhr“, „12:30 Uhr“ und „14:30 Uhr“. | 
|||||| 

Der folgende Wiederholungstrigger gibt beispielsweise an, dass die Logik-App frühestens ab dem 9. September 2017, 14:00 Uhr, wöchentlich jeden Montag um 10:30 Uhr, 12:30 Uhr und 14:30 Uhr (Pacific Standard Time) ausgeführt werden soll:

``` json
{
    "triggers": {
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

Dieser Trigger verfügt über eine optionale Eigenschaft namens *schema*:
  
| Elementname | Erforderlich | Typ | Beschreibung |
| ------------ | -------- | ---- | ----------- |
| schema | Nein | Objekt | Ein JSON-Schema zur Validierung der eingehenden Anforderung. Hilfreich, um nachfolgende Workflowschritte darüber zu informieren, auf welche Eigenschaften verwiesen werden soll. | 
||||| 

Zum Aufrufen dieses Endpunkts muss die API *listCallbackUrl* aufgerufen werden. Weitere Informationen finden Sie unter [Workflows](https://docs.microsoft.com/rest/api/logic/workflows).

## <a name="http-trigger"></a>HTTP-Trigger  

HTTP-Trigger fragen einen angegebenen Endpunkt ab und überprüfen anhand der Antwort, ob der Workflow ausgeführt werden soll. Hier verwendet das `inputs`-Objekt die folgenden, zum Erstellen eines HTTP-Aufrufs erforderlichen Parameter:  

| Elementname | Erforderlich | Typ | Beschreibung | 
| ------------ | -------- | ---- | ----------- | 
| method | Ja | String | Zulässige HTTP-Methoden: „GET“, „POST“, „PUT“, „DELETE“, „PATCH“ oder „HEAD“ | 
| uri | Ja| String | Der HTTP- oder HTTPS-Endpunkt, den der Trigger überprüft. Maximal zulässige Zeichenfolgengröße: 2 KB | 
| Abfragen | Nein | Objekt | Stellt die Abfrageparameter dar, die Sie in die URL einbeziehen möchten. <p>`"queries": { "api-version": "2015-02-01" }` fügt der URL beispielsweise `?api-version=2015-02-01` hinzu. | 
| headers | Nein | Objekt | Stellt die einzelnen Header dar, die in der Anforderung gesendet werden. <p>Verwenden Sie beispielsweise Folgendes, um Sprache und Typ für eine Anforderung festzulegen: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| body | Nein | Objekt | Stellt die an den Endpunkt gesendete Nutzlast dar. | 
| retryPolicy | Nein | Objekt | Verwenden Sie dieses Objekt, um das Wiederholungsverhalten bei Fehlern vom Typ „4xx“ oder „5xx“ anzupassen. | 
| authentication | Nein | Objekt | Stellt die gewünschte Authentifizierungsmethode für die Anforderung dar. Weitere Informationen finden Sie unter [Ausgehende Authentifizierung von Scheduler](../scheduler/scheduler-outbound-authentication.md). <p>Neben Scheduler wird auch noch folgende Eigenschaft unterstützt: `authority`. Ohne Angabe wird standardmäßig der Wert `https://login.windows.net` verwendet. Sie können aber einen anderen Wert verwenden (beispielsweise `https://login.windows\-ppe.net`). | 
||||| 

Eine *Wiederholungsrichtlinie* gilt für vorübergehende Fehler (HTTP-Statuscodes 408, 429 und 5xx) sowie für Verbindungsausnahmen. Sie können diese Richtlinie wie folgt mit dem `retryPolicy`-Objekt definieren:
  
```json
"retryPolicy": {
    "type": "retry-policy-type",
    "interval": retry-interval,
    "count": number-of-retry-attempts
}
```
 
Für den HTTP-Trigger muss die HTTP-API einem bestimmten Muster entsprechen, damit sie reibungslos mit Ihrer Logik-App zusammenarbeitet. Der Trigger erkennt folgende Eigenschaften:  
  
| Antwort | Erforderlich | Beschreibung | 
| -------- | -------- | ----------- |  
| Statuscode | Ja | Der Statuscode 200 (OK) bewirkt eine Ausführung. Alle anderen Statuscodes bewirken keine Ausführung. | 
| Retry-After-Header | Nein | Die Anzahl von Sekunden bis zur erneuten Abfrage des Endpunkts durch die Logik-App. | 
| Adressheader | Nein | Die URL, die im nächsten Abfrageintervall aufgerufen werden soll. Ohne Angabe wird die ursprüngliche URL verwendet. | 
|||| 

Im Anschluss finden Sie einige Beispiele für das Verhalten verschiedener Anforderungsarten:
  
| Antwortcode | Wiederholungsintervall | Verhalten | 
| ------------- | ----------- | -------- | 
| 200 | -- | Führt den Workflow aus und sucht erst nach der definierten Wiederholung wieder nach weiteren Inhalten. | 
| 200 | 10 Sekunden | Führt den Workflow aus und sucht nach zehn Sekunden nach weiteren Inhalten. |  
| 202 | 60 Sekunden | Der Workflow wird nicht ausgelöst. Der nächste Versuch erfolgt gemäß der definierten Wiederholung in einer Minute. Wenn die definierte Wiederholung weniger als eine Minute beträgt, hat der Retry-After-Header Vorrang. Andernfalls wird die definierte Wiederholung verwendet. | 
| 400 | -- | Ungültige Anforderung. Der Workflow wird nicht ausgeführt. Wenn keine `retryPolicy` definiert ist, wird die Standardrichtlinie verwendet. Wenn die Anzahl von Wiederholungen erreicht ist, sucht der Trigger nach der definierten Wiederholung erneut nach Inhalten. | 
| 500 | --| Serverfehler. Der Workflow wird nicht ausgeführt. Wenn keine `retryPolicy` definiert ist, wird die Standardrichtlinie verwendet. Wenn die Anzahl von Wiederholungen erreicht ist, sucht der Trigger nach der definierten Wiederholung erneut nach Inhalten. | 
|||| 

HTTP-Triggerausgaben: 
  
| Elementname | Typ | Beschreibung |
| ------------ | ---- | ----------- |
| headers | Objekt | Die Header der HTTP-Antwort. | 
| body | Objekt | Der Text der HTTP-Antwort. | 
|||| 

## <a name="api-connection-trigger"></a>APIConnection-Trigger  

Die Grundfunktion des APIConnection-Triggers ist mit der des HTTP-Triggers vergleichbar. Die Parameter zum Identifizieren der Aktion sind jedoch unterschiedlich. Beispiel:  
  
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
        "category": "awesomest"
    }
}
```

| Elementname | Erforderlich | Typ | Beschreibung | 
| ------------ | -------- | ---- | ----------- | 
| host | Ja | Objekt | Das gehostete Gateway und die ID für die API-App. | 
| method | Ja | String | Zulässige HTTP-Methoden: „GET“, „POST“, „PUT“, „DELETE“, „PATCH“ oder „HEAD“ | 
| Abfragen | Nein | Objekt | Stellt die Abfrageparameter dar, die Sie in die URL einbeziehen möchten. <p>`"queries": { "api-version": "2015-02-01" }` fügt der URL beispielsweise `?api-version=2015-02-01` hinzu. | 
| headers | Nein | Objekt | Stellt die einzelnen Header dar, die in der Anforderung gesendet werden. <p>Verwenden Sie beispielsweise Folgendes, um Sprache und Typ für eine Anforderung festzulegen: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| body | Nein | Objekt | Stellt die an den Endpunkt gesendete Nutzlast dar. | 
| retryPolicy | Nein | Objekt | Verwenden Sie dieses Objekt, um das Wiederholungsverhalten bei Fehlern vom Typ „4xx“ oder „5xx“ anzupassen. | 
| authentication | Nein | Objekt | Stellt die gewünschte Authentifizierungsmethode für die Anforderung dar. Weitere Informationen finden Sie unter [Ausgehende Authentifizierung von Scheduler](../scheduler/scheduler-outbound-authentication.md). | 
||||| 

Eigenschaften für das `host`-Objekt:  
  
| Elementname | Erforderlich | Beschreibung | 
| ------------ | -------- | ----------- | 
| api runtimeUrl | Ja | Der Endpunkt für die verwaltete API. | 
| connection name |  | Der Name der verwalteten API-Verbindung, die der Workflow verwendet. Muss auf einen Parameter namens `$connection` verweisen. |
|||| 

Eine *Wiederholungsrichtlinie* gilt für vorübergehende Fehler (HTTP-Statuscodes 408, 429 und 5xx) sowie für Verbindungsausnahmen. Sie können diese Richtlinie wie folgt mit dem `retryPolicy`-Objekt definieren:
  
```json
"retryPolicy": {
    "type": "retry-policy-type",
    "interval": retry-interval,
    "count": number-of-retry-attempts
}
```

Ausgaben für einen APIConnection-Trigger:
  
| Elementname | Typ | Beschreibung |
| ------------ | ---- | ----------- |
| headers | Objekt | Die Header der HTTP-Antwort. | 
| body | Objekt | Der Text der HTTP-Antwort. | 
|||| 
  
## <a name="httpwebhook-trigger"></a>HTTPWebhook-Trigger  

Der HTTPWebhook-Trigger stellt ähnlich wie der Anforderungstrigger einen Endpunkt bereit, ruft aber auch eine angegebene URL für die Registrierung/Aufhebung der Registrierung auf. Das folgende Beispiel zeigt einen HTTPWebhook-Trigger:  

```json
"myAppsSpotTrigger": {
    "type": "HttpWebhook",
    "inputs": {
        "subscribe": {
            "method": "POST",
            "uri": "https://pubsubhubbub.appspot.com/subscribe",
            "headers": { },
            "body": {
                "hub.callback": "@{listCallbackUrl()}",
                "hub.mode": "subscribe",
                "hub.topic": "https://pubsubhubbub.appspot.com/articleCategories/technology"
            },
            "authentication": { },
            "retryPolicy": { }
        },
        "unsubscribe": {
            "url": "https://pubsubhubbub.appspot.com/subscribe",
            "body": {
                "hub.callback": "@{workflow().endpoint}@{listCallbackUrl()}",
                "hub.mode": "unsubscribe",
                "hub.topic": "https://pubsubhubbub.appspot.com/articleCategories/technology"
            },
            "method": "POST",
            "authentication": { }
        }
    },
    "conditions": [ ]
}
```

Viele dieser Abschnitte sind optional, und das Verhalten des HTTPWebhook-Triggers hängt davon ab, welche Abschnitte Sie angeben oder weglassen. Eigenschaften für den HTTPWebhook-Trigger:
  
| Elementname | Erforderlich | Beschreibung | 
| ------------ | -------- | ----------- |  
| subscribe | Nein | Gibt die ausgehende Anforderung an, die bei der Triggererstellung aufgerufen werden soll und die anfängliche Registrierung ausführt. | 
| unsubscribe | Nein | Gibt die ausgehende Anforderung an, die beim Löschen des Triggers aufgerufen werden soll. | 
|||| 

Grenzwerte für eine Webhookaktion können auf die gleiche Weise angegeben werden wie [asynchrone Grenzwerte für HTTP](#asynchronous-limits). Im Anschluss finden Sie weitere Informationen zu den Aktionen `subscribe` und `unsubscribe`:

* `subscribe` wird aufgerufen, damit der Trigger damit beginnen kann, auf Ereignisse zu lauschen. Dieser ausgehende Aufruf beginnt mit den gleichen Parametern wie HTTP-Standardaktionen. Er wird bei jeder Veränderung des Workflows ausgeführt – beispielsweise bei einer Erneuerung der Anmeldeinformationen oder bei einer Änderung der Eingabeparameter des Triggers. 
  
  Zur Unterstützung dieses Aufrufs gibt die Funktion `@listCallbackUrl()` eine eindeutige URL für diesen speziellen Trigger im Workflow zurück. Diese URL stellt den eindeutigen Bezeichner für die Endpunkte dar, die die REST-API des Diensts verwenden.
  
* `unsubscribe` wird automatisch aufgerufen, wenn ein Vorgang dazu führt, dass der Trigger ungültig wird. Hierzu zählen unter anderem folgende Vorgänge:

  * Löschen oder Deaktivieren des Triggers 
  * Löschen oder Deaktivieren des Workflows 
  * Löschen oder Deaktivieren des Abonnements 
  
  Für diese Funktion werden die gleichen Parameter verwendet wie beim HTTP-Trigger.

Im Anschluss finden Sie die Ausgaben des HTTPWebhook-Triggers und die Inhalte der eingehenden Anforderung:
  
| Elementname | Typ | Beschreibung |
| ------------ | ---- | ----------- |
| headers | Objekt | Die Header der HTTP-Antwort. | 
| body | Objekt | Der Text der HTTP-Antwort. | 
|||| 

## <a name="conditions"></a>Bedingungen  

Bei jedem Trigger können Sie mithilfe einzelner oder mehrerer Bedingungen bestimmen, ob der Workflow ausgeführt werden soll. Beispiel:  

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

In diesem Fall wird der Bericht nur ausgelöst, wenn der Parameter `sendReports` des Workflows auf „true“ festgelegt ist. Bedingungen können außerdem auf den Statuscode des Triggers verweisen. So können Sie beispielsweise festlegen, dass ein Workflow nur gestartet werden soll, wenn Ihre Website den Statuscode 500 zurückgibt. Beispiel:
  
``` json
"conditions": [ 
    {  
      "expression": "@equals(triggers().code, 'InternalServerError')"  
    }  
]  
```  
  
> [!NOTE]  
> Sobald ein Ausdruck auf den Statuscode des Triggers verweist, wird das Standardverhalten „nur bei 200 (OK) auslösen“ ersetzt. Wenn die Auslösung also beispielsweise sowohl für den Statuscode 200 als auch für den Statuscode 201 erfolgen soll, müssen Sie als Bedingung Folgendes einfügen: `@or(equals(triggers().code, 200),equals(triggers().code,201))`
  
## <a name="start-multiple-runs-for-a-request"></a>Starten mehrerer Ausführungen für eine Anforderung

Wenn Sie mehrere Ausführungen für eine einzelne Anforderung starten möchten, können Sie `splitOn` verwenden. Dies ist beispielsweise hilfreich, wenn Sie einen Endpunkt abfragen möchten, der zwischen Abfrageintervallen über mehrere neue Elemente verfügen kann.
  
Bei `splitOn` geben Sie die Eigenschaft innerhalb der Antwortnutzlast mit dem Elementarray an, dessen Elemente Sie jeweils verwenden möchten, um eine Ausführung des Triggers zu starten. Angenommen, Sie verfügen über eine API, die folgende Antwort zurückgibt:  
  
```json
{
    "status": "Succeeded",
    "rows": [
        {  
            "id" : 938109380,
            "name" : "myFirstRow"
        },
        {
            "id" : 938109381,
            "name" : "mySecondRow"
        }
    ]
}
```
  
Ihre Logik-App benötigt lediglich den Inhalt von `rows`. Somit können Sie Ihren Trigger wie im folgenden Beispiel erstellen:  

```json
"mySplitterTrigger": {
    "type": "Http",
    "recurrence": {
        "frequency": "minute",
        "interval": 1
    },
    "intputs": {
        "uri": "https://mydomain.com/myAPI",
        "method": "GET"
    },
    "splitOn": "@triggerBody()?.rows"
}
```
> [!NOTE]  
> Bei Verwendung von `SplitOn` können Sie keine Eigenschaften abrufen, die sich außerhalb des Arrays befinden. In diesem Beispiel können Sie also die `status`-Eigenschaft aus der von der API zurückgegebenen Antwort nicht abrufen.
> In diesem Beispiel verwenden wir zudem den `?`-Operator, um einen Fehler zu vermeiden, wenn die `rows`-Eigenschaft nicht vorhanden ist. 

In der Workflowdefinition gibt `@triggerBody().name` also für die erste Ausführung `myFirstRow` und für die zweite Ausführung `mySecondRow` zurück. Das folgende Beispiel zeigt die Triggerausgaben:  

```json
{
    "body": {
        "id": 938109380,
        "name": "mySecondRow"
    }
}
```

```json
{
    "body": {
        "id": 938109381,
        "name": "mySecondRow"
    }
}
```
  
## <a name="single-run-instance"></a>Instanz mit einzelner Ausführung

Wiederholungstrigger können so konfiguriert werden, dass sie nur ausgelöst werden, wenn alle aktiven Ausführungen abgeschlossen sind. Falls eine geplante Wiederholung stattfindet, während bereits eine Workflowinstanz ausgeführt wird, wird der Trigger übersprungen und im nächsten Wiederholungsintervall eine erneute Prüfung durchgeführt.
Diese Einstellung können Sie konfigurieren, indem Sie die `operationOptions`-Eigenschaft auf `singleInstance` festlegen:

```json
"triggers": {
    "myHTTPTrigger": {
        "type": "Http",
        "inputs": { ... },
        "recurrence": { ... },
        "operationOptions": "singleInstance"
    }
}
```

## <a name="actions-overview"></a>Übersicht über Aktionen

Es gibt viele Arten von Aktionen, die jeweils über ein eigenes Verhalten verfügen. Jeder Aktionstyp verfügt über verschiedene Eingaben, die sein Verhalten definieren. Collection-Aktionen können zahlreiche weitere Aktionen enthalten. 

### <a name="standard-actions"></a>Standardaktionen  

| Aktionstyp | Beschreibung | 
| ----------- | ----------- | 
| **HTTP** | Fragt einen HTTP-Webendpunkt ab. | 
| **ApiConnection**  | Vergleichbar mit der HTTP-Aktion, verwendet aber [von Microsoft verwaltete APIs](https://docs.microsoft.com/azure/connectors/apis-list). | 
| **ApiConnectionWebhook** | Vergleichbar mit „HTTPWebhook“, verwendet aber von Microsoft verwaltete APIs. | 
| **Response** | Definiert die Antwort für einen eingehenden Aufruf. | 
| **Function** | Stellt eine Azure-Funktion dar. | 
| **Wait** | Wartet eine bestimmte Zeit oder bis zu einer bestimmten Zeit. | 
| **Workflow** | Stellt einen geschachtelten Workflow dar. | 
| **Compose** | Erstellt ein beliebiges Objekt auf der Grundlage der Aktionseingaben. | 
| **Query** | Filtert ein Array auf der Grundlage einer Bedingung. | 
| **Select** | Projiziert die einzelnen Elemente eines Arrays in einen neuen Wert. So können Sie beispielsweise ein Zahlenarray in ein Objektarray konvertieren. | 
| **Tabelle** | Konvertiert ein Array von Elementen in eine CSV- oder HTML-Tabelle. | 
| **Terminate** | Beendet eine Workflowausführung. | 
||| 

### <a name="collection-actions"></a>Collection-Aktionen

| Aktionstyp | Beschreibung | 
| ----------- | ----------- | 
| **Condition** | Wertet einen Ausdruck aus und führt abhängig vom Ergebnis die entsprechende Verzweigung aus. | 
| **Scope** | Dient zum logischen Gruppieren anderer Aktionen. | 
| **ForEach** | Diese Schleifenaktion durchläuft ein Array und führt interne Aktionen für die einzelnen Arrayelemente aus. | 
| **Until** | Diese Schleifenaktion führt interne Aktionen aus, bis eine Bedingung erfüllt ist. | 
||| 

## <a name="http-action"></a>HTTP-Aktion  

HTTP-Aktionen rufen einen angegebenen Endpunkt auf und überprüfen anhand der Antwort, ob der Workflow ausgeführt werden soll. Beispiel:
  
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

| Elementname | Erforderlich | Typ | Beschreibung | 
| ------------ | -------- | ---- | ----------- | 
| method | Ja | String | Zulässige HTTP-Methoden: „GET“, „POST“, „PUT“, „DELETE“, „PATCH“ oder „HEAD“ | 
| uri | Ja| String | Der HTTP- oder HTTPS-Endpunkt, den der Trigger überprüft. Maximal zulässige Zeichenfolgengröße: 2 KB | 
| Abfragen | Nein | Objekt | Stellt die Abfrageparameter dar, die Sie in die URL einbeziehen möchten. <p>`"queries": { "api-version": "2015-02-01" }` fügt der URL beispielsweise `?api-version=2015-02-01` hinzu. | 
| headers | Nein | Objekt | Stellt die einzelnen Header dar, die in der Anforderung gesendet werden. <p>Verwenden Sie beispielsweise Folgendes, um Sprache und Typ für eine Anforderung festzulegen: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| body | Nein | Objekt | Stellt die an den Endpunkt gesendete Nutzlast dar. | 
| retryPolicy | Nein | Objekt | Verwenden Sie dieses Objekt, um das Wiederholungsverhalten bei Fehlern vom Typ „4xx“ oder „5xx“ anzupassen. | 
| operationsOptions | Nein | string | Definiert den Satz spezieller Verhaltensweisen, die überschrieben werden sollen. | 
| Authentifizierung | Nein | Objekt | Stellt die gewünschte Authentifizierungsmethode für die Anforderung dar. Weitere Informationen finden Sie unter [Ausgehende Authentifizierung von Scheduler](../scheduler/scheduler-outbound-authentication.md). <p>Neben Scheduler wird auch noch folgende Eigenschaft unterstützt: `authority`. Ohne Angabe wird standardmäßig der Wert `https://login.windows.net` verwendet. Sie können aber einen anderen Wert verwenden (beispielsweise `https://login.windows\-ppe.net`). | 
||||| 

HTTP-Aktionen und APIConnection-Aktionen unterstützen *Wiederholungsrichtlinien*. Eine Wiederholungsrichtlinie gilt für vorübergehende Fehler (HTTP-Statuscodes 408, 429 und 5xx) sowie für Verbindungsausnahmen. Sie können diese Richtlinie wie folgt mit dem `retryPolicy`-Objekt definieren:
  
```json
"retryPolicy": {
    "type": "retry-policy-type",
    "interval": retry-interval,
    "count": number-of-retry-attempts
}
```
Die folgende HTTP-Beispielaktion wiederholt den Abruf der neuesten Nachrichten zweimal, falls ein vorübergehender Fehler vorliegt. Dabei werden insgesamt drei Ausführungen mit einer jeweils 30-sekündigen Verzögerung zwischen den einzelnen Versuchen verwendet:
  
```json
"myLatestNewsAction": {
    "type": "Http",
    "inputs": {
        "method": "GET",
        "uri": "https://mynews.example.com/latest",
        "retryPolicy" : {
            "type": "fixed",
            "interval": "PT30S",
            "count": 2
        }
    }
}
```

Das Wiederholungsintervall ist im [ISO 8601-Format](https://en.wikipedia.org/wiki/ISO_8601) angegeben. Der Mindestwert für dieses Intervall beträgt 20 Sekunden und ist gleichzeitig der Standardwert. Der Maximalwert ist eine Stunde. Die standardmäßige und maximale Wiederholungsanzahl beträgt vier Stunden. Ohne Angabe der Wiederholungsrichtliniendefinition wird eine `fixed`-Strategie mit Standardwerten für Wiederholungsanzahl und Intervall verwendet. Wenn Sie die Wiederholungsrichtlinie deaktivieren möchten, legen Sie den Typ auf `None` fest.

### <a name="asynchronous-patterns"></a>Asynchrone Muster

Standardmäßig unterstützen alle HTTP-basierten Aktionen das Standardmuster für asynchrone Vorgänge. Wenn der Remoteserver also durch eine Antwort vom Typ „202 (ZULÄSSIG)“ angibt, dass die Anforderung zur Verarbeitung akzeptiert wird, fragt das Logic Apps-Modul die im Adressheader der Antwort angegebene URL ab, bis ein Endzustand (eine 202-fremde Antwort) erreicht ist.
  
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

Die Dauer eines asynchronen Musters kann auf ein bestimmtes Zeitintervall begrenzt werden. Falls das Zeitintervall abläuft, ohne dass ein Endzustand erreicht wurde, wird der Status der Aktion mit einem `ActionTimedOut`-Code als `Cancelled` markiert. Der Grenzwert für das Timeout wird im ISO 8601-Format angegeben. Grenzwerte können wie folgt angegeben werden:

``` json
"action-name": {
    "type": "Workflow|Webhook|Http|ApiConnectionWebhook|ApiConnection",
    "inputs": { },
    "limit": {
        "timeout": "PT10S"
    }
}
```
  
## <a name="apiconnection-action"></a>APIConnection-Aktion

Die APIConnection-Aktion verweist auf einen von Microsoft verwalteten Connector. Diese Aktion erfordert einen Verweis auf eine gültige Verbindung sowie Informationen zur API und zu den Parametern.
Beispiel für eine APIConnection-Aktion:

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

| Elementname | Erforderlich | Typ | Beschreibung | 
| ------------ | -------- | ---- | ----------- | 
| host | Ja | Objekt | Stellt die Connectorinformationen dar (beispielsweise `runtimeUrl` und Verweis auf das Verbindungsobjekt). | 
| method | Ja | String | Zulässige HTTP-Methoden: „GET“, „POST“, „PUT“, „DELETE“, „PATCH“ oder „HEAD“ | 
| path | Ja | String | Der Pfad für den API-Vorgang. | 
| Abfragen | Nein | Objekt | Stellt die Abfrageparameter dar, die Sie in die URL einbeziehen möchten. <p>`"queries": { "api-version": "2015-02-01" }` fügt der URL beispielsweise `?api-version=2015-02-01` hinzu. | 
| headers | Nein | Objekt | Stellt die einzelnen Header dar, die in der Anforderung gesendet werden. <p>Verwenden Sie beispielsweise Folgendes, um Sprache und Typ für eine Anforderung festzulegen: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| body | Nein | Objekt | Stellt die an den Endpunkt gesendete Nutzlast dar. | 
| retryPolicy | Nein | Objekt | Verwenden Sie dieses Objekt, um das Wiederholungsverhalten bei Fehlern vom Typ „4xx“ oder „5xx“ anzupassen. | 
| operationsOptions | Nein | string | Definiert den Satz spezieller Verhaltensweisen, die überschrieben werden sollen. | 
| Authentifizierung | Nein | Objekt | Stellt die gewünschte Authentifizierungsmethode für die Anforderung dar. Weitere Informationen finden Sie unter [Ausgehende Authentifizierung von Scheduler](../scheduler/scheduler-outbound-authentication.md). |
||||| 

Eine Wiederholungsrichtlinie gilt für vorübergehende Fehler (HTTP-Statuscodes 408, 429 und 5xx) sowie für Verbindungsausnahmen. Sie können diese Richtlinie wie folgt mit dem `retryPolicy`-Objekt definieren:
  
```json
"retryPolicy": {
    "type": "retry-policy-type",
    "interval": retry-interval,
    "count": number-of-retry-attempts
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

| Elementname | Erforderlich | Typ | Beschreibung | 
| ------------ | -------- | ---- | ----------- | 
| host | Ja | Objekt | Stellt die Connectorinformationen dar (beispielsweise `runtimeUrl` und Verweis auf das Verbindungsobjekt). | 
| path | Ja | String | Der Pfad für den API-Vorgang. | 
| Abfragen | Nein | Objekt | Stellt die Abfrageparameter dar, die Sie in die URL einbeziehen möchten. <p>`"queries": { "api-version": "2015-02-01" }` fügt der URL beispielsweise `?api-version=2015-02-01` hinzu. | 
| headers | Nein | Objekt | Stellt die einzelnen Header dar, die in der Anforderung gesendet werden. <p>Verwenden Sie beispielsweise Folgendes, um Sprache und Typ für eine Anforderung festzulegen: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| body | Nein | Objekt | Stellt die an den Endpunkt gesendete Nutzlast dar. | 
| retryPolicy | Nein | Objekt | Verwenden Sie dieses Objekt, um das Wiederholungsverhalten bei Fehlern vom Typ „4xx“ oder „5xx“ anzupassen. | 
| operationsOptions | Nein | string | Definiert den Satz spezieller Verhaltensweisen, die überschrieben werden sollen. | 
| Authentifizierung | Nein | Objekt | Stellt die gewünschte Authentifizierungsmethode für die Anforderung dar. Weitere Informationen finden Sie unter [Ausgehende Authentifizierung von Scheduler](../scheduler/scheduler-outbound-authentication.md). |
||||| 

## <a name="response-action"></a>Response-Aktion  

Diese Aktion enthält die gesamte Antwortnutzlast aus einer HTTP-Anforderung sowie `statusCode`, `body` und `headers`:
  
```json
"myResponseAction": {
    "type": "response",
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

## <a name="function-action"></a>Function-Aktion   

Mit dieser Aktion können Sie eine [Azure-Funktion](../azure-functions/functions-overview.md) darstellen und aufrufen. Beispiel:

```json
"my-Azure-Function-name": {
   "type": "Function",
    "inputs": {
        "function": {
            "id": "/subscriptions/{Azure-subscription-ID}/resourceGroups/{Azure-resource-group}/providers/Microsoft.Web/sites/{your-Azure-function-app-name}/functions/{your-Azure-function-name}"
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
| Elementname | Erforderlich | Typ | Beschreibung | 
| ------------ | -------- | ---- | ----------- |  
| Funktions-ID | Ja | String | Die Ressourcen-ID für die Azure-Funktion, die Sie aufrufen möchten. | 
| method | Nein | String | Die HTTP-Methode zum Aufrufen der Funktion. Ohne Angabe wird standardmäßig „POST“ verwendet. | 
| Abfragen | Nein | Objekt | Stellt die Abfrageparameter dar, die Sie in die URL einbeziehen möchten. <p>`"queries": { "api-version": "2015-02-01" }` fügt der URL beispielsweise `?api-version=2015-02-01` hinzu. | 
| headers | Nein | Objekt | Stellt die einzelnen Header dar, die in der Anforderung gesendet werden. <p>Verwenden Sie beispielsweise Folgendes, um Sprache und Typ für eine Anforderung festzulegen: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| body | Nein | Objekt | Stellt die an den Endpunkt gesendete Nutzlast dar. | 
|||||

Beim Speichern Ihrer Logik-App überprüft Azure Logic Apps die referenzierte Funktion:

* Sie müssen auf die Funktion zugreifen können.
* Sie können nur HTTP-Standardtrigger oder generische JSON-Webhooktrigger verwenden.
* Für die Funktion darf keine Route definiert sein.
* Als Autorisierungsebene sind nur „function“ und „anonymous“ zulässig.

Die Trigger-URL wird abgerufen, zwischengespeichert und zur Laufzeit verwendet. Sollte die zwischengespeicherte URL also aufgrund eines Vorgangs ungültig werden, tritt zur Laufzeit ein Fehler bei der Aktion auf. Speichern Sie zur Umgehung dieses Problems die Logik-App erneut. Dadurch wird die Trigger-URL erneut abgerufen und zwischengespeichert.

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
> Die Wartezeit kann entweder mit dem `until`-Objekt oder mit dem `interval`-Objekt angegeben werden, aber nicht mit beiden.
  
| Elementname | Erforderlich | Typ | Beschreibung | 
| ------------ | -------- | ---- | ----------- | 
| until | Nein | Objekt | Die Wartezeit auf der Grundlage eines Zeitpunkts. | 
| Zeitstempel für „until“ | Ja | String | Der Zeitpunkt (im [UTC-Datums-/Uhrzeitformat](https://en.wikipedia.org/wiki/Coordinated_Universal_Time)), zu dem die Wartezeit abläuft. | 
| interval | Nein | Objekt | Die Wartezeit auf der Grundlage von Intervalleinheit und Anzahl. | 
| interval unit | Ja | String | Die Zeiteinheit. Zulässige Werte: „second“, „minute“, „hour“, „day“, „week“ oder „month“ | 
| interval count | Ja | Integer | Eine positive ganze Zahl, die die Anzahl von Intervalleinheiten für die Wartezeit darstellt. | 
||||| 

## <a name="workflow-action"></a>Workflow-Aktion   

Diese Aktion stellt einen weiteren Workflow dar. Logic Apps führt eine Zugriffsprüfung für den Workflow (genauer gesagt: für den Trigger) durch. Sie müssen also auf den Workflow zugreifen können.

Die Ausgaben der Aktion basieren auf den Definitionen in der `response`-Aktion für den untergeordneten Workflow. Falls Sie keine `response`-Aktion definiert haben, sind die Ausgaben leer.

```json
"myNestedWorkflowAction": {
    "type": "Workflow",
    "inputs": {
        "host": {
            "id": "/subscriptions/xxxxyyyyzzz/resourceGroups/rg001/providers/Microsoft.Logic/mywf001",
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

| Elementname | Erforderlich | Typ | Beschreibung | 
| ------------ | -------- | ---- | ----------- |  
| host id | Ja | String| Die Ressourcen-ID für den Workflow, den Sie aufrufen möchten. | 
| host triggerName | Ja | String | Der Name des Triggers, den Sie aufrufen möchten. | 
| Abfragen | Nein | Objekt | Stellt die Abfrageparameter dar, die Sie in die URL einbeziehen möchten. <p>`"queries": { "api-version": "2015-02-01" }` fügt der URL beispielsweise `?api-version=2015-02-01` hinzu. | 
| headers | Nein | Objekt | Stellt die einzelnen Header dar, die in der Anforderung gesendet werden. <p>Verwenden Sie beispielsweise Folgendes, um Sprache und Typ für eine Anforderung festzulegen: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| body | Nein | Objekt | Stellt die an den Endpunkt gesendete Nutzlast dar. | 
|||||   

## <a name="compose-action"></a>Compose-Aktion

Mit dieser Aktion können Sie ein beliebiges Objekt erstellen. Bei der Ausgabe handelt es sich um das Ergebnis der Auswertung der Aktionseingaben. 

> [!NOTE]
> Die `Compose`-Aktion kann zur Erstellung einer beliebigen Ausgabe verwendet werden. Hierzu zählen unter anderem Objekte und Arrays sowie jede andere Art von Ausgabe, die nativ von Logik-Apps unterstützt wird (etwa XML- und Binärelemente).

Mithilfe der Compose-Aktion können Sie beispielsweise Ausgaben mehrerer Aktionen zusammenführen:

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

## <a name="select-action"></a>Select-Aktion

Mit dieser Aktion können Sie die einzelnen Elemente eines Arrays in einen neuen Wert projizieren.
Zum Konvertieren eines Arrays von Zahlen in ein Array von Objekten können Sie beispielsweise Folgendes verwenden:

```json
"selectNumbersAction": {
    "type": "Select",
    "inputs": {
        "from": [ 1, 3, 0, 5, 4, 2 ],
        "select": { "number": "@item()" }
    }
}
```

| Name | Erforderlich | Typ | Beschreibung | 
| ---- | -------- | ---- | ----------- | 
| Aus | Ja | Array | Das Quellarray. |
| select | Ja | Beliebig | Die Projektion, die auf die einzelnen Elemente des Quellarrays angewendet wird. |
||||| 

Die `select`-Aktion gibt ein Array aus, dessen Kardinalität mit der des Eingabearrays identisch ist. Jedes Element wird gemäß der Definition der `select`-Eigenschaft transformiert. Wenn die Eingabe ein leeres Array ist, wird auch ein leeres Array ausgegeben.

## <a name="query-action"></a>Query-Aktion

Mit dieser Aktion können Sie ein Array auf der Grundlage einer Bedingung filtern. Im folgenden Beispiel werden Zahlen größer zwei ausgewählt:

```json
"filterNumbersAction": {
    "type": "Query",
    "inputs": {
        "from": [ 1, 3, 0, 5, 4, 2 ],
        "where": "@greater(item(), 2)"
    }
}
```

Die Ausgabe der Aktion `query` ist ein Array mit Elementen aus dem Eingabearray, die die Bedingung erfüllen.

> [!NOTE]
> Sollten keine Werte die Bedingung von `where` erfüllen, ist das Ergebnis ein leeres Array.

| Name | Erforderlich | Typ | Beschreibung | 
| ---- | -------- | ---- | ----------- | 
| Aus | Ja | Array | Das Quellarray. |
| Hierbei gilt: | Ja | String | Die Bedingung, die auf die einzelnen Elemente aus dem Quellarray angewendet wird. |
||||| 

## <a name="table-action"></a>Table-Aktion

Mit dieser Aktion können Sie ein Array von Elementen in eine **CSV**- oder **HTML**-Tabelle konvertieren. Nehmen wir beispielsweise an, Sie verfügen über das folgende `@triggerBody()`-Array:

```json
[ 
    {
      "id": 0,
      "name": "apples"
    },
    {
      "id": 1, 
      "name": "oranges"
    }
]
```

Nehmen wir weiter an, Sie definieren eine Table-Aktion wie im folgenden Beispiel:

```json
"convertToTableAction": {
    "type": "Table",
    "inputs": {
        "from": "@triggerBody()",
        "format": "html"
    }
}
```

In diesem Beispiel ergibt sich folgende HTML-Tabelle: 

<table><thead><tr><th>id</th><th>name</th></tr></thead><tbody><tr><td>0</td><td>apples</td></tr><tr><td>1</td><td>oranges</td></tr></tbody></table>

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

In diesem Beispiel ergibt sich folgende HTML-Tabelle: 

<table><thead><tr><th>Produce ID</th><th>Beschreibung</th></tr></thead><tbody><tr><td>0</td><td>fresh apples</td></tr><tr><td>1</td><td>fresh oranges</td></tr></tbody></table>

| Name | Erforderlich | Typ | Beschreibung | 
| ---- | -------- | ---- | ----------- | 
| Aus | Ja | Array | Das Quellarray. Wenn der Eigenschaftswert `from` ein leeres Array ist, wird eine leere Tabelle ausgegeben. | 
| format | Ja | String | Das gewünschte Tabellenformat (**CSV** oder **HTML**). | 
| columns | Nein | Array | Die gewünschten Tabellenspalten. Dient zum Überschreiben der Standardtabellenform. | 
| column header | Nein | String | Die Spaltenüberschrift. | 
| column value | Ja | String | Der Spaltenwert. | 
||||| 

## <a name="terminate-action"></a>Terminate-Aktion

Diese Aktion beendet die Workflowausführung, bricht alle aktiven Aktionen ab und überspringt die restlichen Aktionen (sofern vorhanden). Die Terminate-Aktion hat keine Auswirkungen auf bereits abgeschlossene Aktionen.

Zum Beenden einer Ausführung mit dem Status „Failed“ können Sie beispielsweise Folgendes verwenden:

```json
"handleUnexpectedResponseAction": {
    "type": "Terminate",
    "inputs": {
        "runStatus": "Failed",
        "runError": {
            "code": "UnexpectedResponse",
            "message": "Received an unexpected response"
        }
    }
}
```

| Name | Erforderlich | Typ | Beschreibung | 
| ---- | -------- | ---- | ----------- | 
| runStatus | Ja | String | Der Status der Zielausführung (entweder `Failed` oder `Cancelled`). |
| runError | Nein | Objekt | Die Fehlerdetails. Wird nur unterstützt, wenn `runStatus` auf `Failed` festgelegt ist. |
| Code für „runError“ | Nein | String | Der Fehlercode der Ausführung. |
| runError message | Nein | String | Die Fehlermeldung der Ausführung. |
||||| 

## <a name="collection-actions-overview"></a>Übersicht über Collection-Aktionen

Einige Aktionen können weitere Aktionen enthalten. Auf Verweisaktionen in einer Auflistung kann direkt außerhalb der Auflistung verwiesen werden. Wenn Sie also etwa `Http` in einem Bereich (`scope`) definieren, ist `@body('http')` weiterhin im gesamten Workflow gültig. Aktionen in einer `runAfter`-Auflistungseigenschaft können nur mit anderen Aktionen in der gleichen Auflistung kombiniert werden.

## <a name="condition-if-action"></a>Bedingung: If-Aktion

Mit dieser Aktion können Sie eine Bedingung auswerten und die Ausführung einer Verzweigung davon abhängig machen, ob die Auswertung des Ausdrucks `true` ergibt. 
  
```json
"myCondition": {
    "type": "If",
    "actions": {
        "if_true": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {}
        }
    },
    "else": {
        "actions": {
            "if_false": {
                "type": "Http",
                "inputs": {
                    "method": "GET",
                    "uri": "http://myurl"
                },
                "runAfter": {}
            }
        }
    },
    "expression": "@equals(triggerBody(), json(true))",
    "runAfter": {}
}
``` 

| Name | Erforderlich | Typ | Beschreibung | 
| ---- | -------- | ---- | ----------- | 
| Aktionen | Ja | Objekt | Die internen Aktionen, die ausgeführt werden sollen, wenn `expression` als `true` ausgewertet wird. | 
| expression | Ja | String | Der auszuwertende Ausdruck. |
| else | Nein | Objekt | Die internen Aktionen, die ausgeführt werden sollen, wenn `expression` als `false` ausgewertet wird. |
||||| 

Erfolgreich ausgewertete Bedingungen werden als `Succeeded` markiert. Aktionen in den Objekten `actions` und `else` werden wie folgt ausgewertet: 

* `Succeeded`, wenn sie erfolgreich ausgeführt wurden
* `Failed`, wenn sie nicht erfolgreich ausgeführt wurden
* `Skipped`, wenn die entsprechende Verzweigung nicht ausgeführt wird

Die folgenden Beispiele veranschaulichen die Verwendung von Bedingungen durch Ausdrücke in einer Aktion:
  
| JSON-Wert | Ergebnis | 
| ---------- | -------| 
| `"expression": "@parameters('hasSpecialAction')"` | Jeder Wert, der als „true“ ausgewertet wird, bewirkt, dass diese Bedingung als erfüllt betrachtet wird. Unterstützt nur boolesche Ausdrücke. Andere Typen können mithilfe der Funktionen `empty` und `equals` in boolesche Werte konvertiert werden. | 
| `"expression": "@greater(actions('act1').output.value, parameters('threshold'))"` | Unterstützt Vergleichsfunktionen. In diesem Beispiel wird die Aktion nur ausgeführt, wenn die Ausgabe von `act1` den Schwellenwert übersteigt. | 
| `"expression": "@or(greater(actions('act1').output.value, parameters('threshold')), less(actions('act1').output.value, 100))"` | Unterstützt Logikfunktionen zur Erstellung geschachtelter boolescher Ausdrücke. In diesem Beispiel wird die Aktion nur ausgeführt, wenn die Ausgabe von `act1` den Schwellenwert übersteigt oder kleiner 100 ist. | 
| `"expression": "@equals(length(actions('act1').outputs.errors), 0))"` | Mithilfe von Arrayfunktionen können Sie prüfen, ob ein Array Elemente enthält. In diesem Beispiel wird die Aktion ausgeführt, wenn das `errors`-Array leer ist. | 
| `"expression": "parameters('hasSpecialAction')"` | Fehler: Keine gültige Bedingung, da „@“ für Bedingungen erforderlich ist. |  
|||

## <a name="scope-action"></a>Scope-Aktion

Mit dieser Aktion können Sie Aktionen in einem Workflow logisch gruppieren.

```json
"myScope": {
    "type": "Scope",
    "actions": {
        "call_bing": {
            "type": "Http",
             "inputs": {
                "url": "http://www.bing.com"
            }
        }
    }
}
```

| Name | Erforderlich | Typ | Beschreibung | 
| ---- | -------- | ---- | ----------- |  
| Aktionen | Ja | Objekt | Die internen Aktionen, die innerhalb des Bereichs ausgeführt werden sollen. |
||||| 

## <a name="foreach-action"></a>ForEach-Aktion

Diese Schleifenaktion durchläuft ein Array und führt interne Aktionen für die einzelnen Arrayelemente aus. Die `foreach`-Schleife wird standardmäßig parallel ausgeführt und kann gleichzeitig 20 parallele Instanzen ausführen. Ausführungsregeln können mithilfe des `operationOptions`-Parameters festgelegt werden.

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
    "runAfter": {
        "email_filter": [ "Succeeded" ]
    }
}
```

| Name | Erforderlich | Typ | Beschreibung | 
| ---- | -------- | ---- | ----------- | 
| Aktionen | Ja | Objekt | Die internen Aktionen, die innerhalb der Schleife ausgeführt werden sollen. | 
| foreach | Ja | String | Das zu durchlaufende Array. | 
| operationOptions | Nein | String | Dient zum Angeben von Vorgangsoptionen für die Verhaltensanpassung. Unterstützt derzeit nur `Sequential` für sequenziell ausgeführte Iterationen mit dem Standardverhalten „Parallel“. |
||||| 

## <a name="until-action"></a>Until-Aktion

Diese Schleifenaktion führt interne Aktionen aus, bis eine Bedingung erfüllt ist.

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
        "count": 1000,
        "timeout": "PT1H"
    },
    "runAfter": {}
}
```

| Name | Erforderlich | Typ | Beschreibung | 
| ---- | -------- | ---- | ----------- | 
| Aktionen | Ja | Objekt | Die internen Aktionen, die innerhalb der Schleife ausgeführt werden sollen. | 
| expression | Ja | String | Der Ausdruck, der nach jeder Iteration ausgewertet werden soll. | 
| limit | Ja | Objekt | Die Grenzwerte für die Schleife. Es muss mindestens ein Grenzwert definiert werden. | 
| count | Nein | Integer | Der Grenzwert für die Anzahl durchzuführender Iterationen. | 
| timeout | Nein | String | Das Zeitlimit (im [ISO 8601-Format](https://en.wikipedia.org/wiki/ISO_8601)), das angibt, wie lange die Schleife ausgeführt werden soll. |
||||| 

## <a name="next-steps"></a>Nächste Schritte

* [Definitionssprache für Workflows](../logic-apps/logic-apps-workflow-definition-language.md)
* [Workflow-REST-API](https://docs.microsoft.com/rest/api/logic/workflows)
