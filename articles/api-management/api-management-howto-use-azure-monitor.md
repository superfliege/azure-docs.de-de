---
title: Überwachen von veröffentlichten APIs in Azure API Management | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie Schritt für Schritt, wie Sie Ihre API in API Management überwachen.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 06/15/2018
ms.author: apimpm
ms.openlocfilehash: 3501284be2430941863bbae90f044df8ba5e7a2a
ms.sourcegitcommit: 75fef8147209a1dcdc7573c4a6a90f0151a12e17
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2019
ms.locfileid: "56454803"
---
# <a name="monitor-published-apis"></a>Überwachen von veröffentlichten APIs

Mit Azure Monitor können Sie Metriken oder Protokolle aus Azure-Ressourcen visualisieren, abfragen, weiterleiten und archivieren und ggf. notwendige Maßnahmen ergreifen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Anzeigen von Aktivitätsprotokollen
> * Anzeigen von Diagnoseprotokollen
> * Anzeigen von Metriken Ihrer API 
> * Einrichten einer Warnungsregel, wenn Ihre API nicht autorisierte Aufrufe empfängt

Im folgenden Video wird die Überwachung von API Management mithilfe von Azure Monitor veranschaulicht. 

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Monitor-API-Management-with-Azure-Monitor/player]

## <a name="prerequisites"></a>Voraussetzungen

+ Machen Sie sich mit der [Azure API Management-Terminologie](api-management-terminology.md) vertraut.
+ Absolvieren Sie die folgende Schnellstartanleitung: [Erstellen einer neuen Azure API Management-Dienstinstanz](get-started-create-service-instance.md)
+ Absolvieren Sie außerdem das folgende Tutorial: [Importieren und Veröffentlichen Ihrer ersten API](import-and-publish.md).

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="view-metrics-of-your-apis"></a>Anzeigen von Metriken Ihrer API

Von API Management werden jede Minute Metriken ausgegeben, sodass Sie einen Überblick über den Zustand und die Integrität Ihrer APIs nahezu in Echtzeit erhalten. Hier sehen Sie eine Übersicht über einige der verfügbaren Metriken:

* Kapazität (Vorschau): Unterstützt Sie beim Treffen von Entscheidungen in Bezug auf Upgrades/Downgrades Ihrer APIM-Dienste. Die Metrik wird minütlich ausgegeben und spiegelt die Gatewaykapazität zum Zeitpunkt der Meldung wider. Der Wert der Metrik kann zwischen 0 und 100 liegen und wird basierend auf Gatewayressourcen wie CPU und Speicherauslastung berechnet.
* Gatewayanforderungen gesamt: Anzahl der API-Anforderungen innerhalb des Zeitraums. 
* Erfolgreiche Gatewayanforderungen: Anzahl der API-Anforderungen, die erfolgreiche HTTP-Antwortcodes einschließlich 304, 307 und alle unter 301 (z.B. 200) erhalten haben.
* Fehlerhafte Gatewayanforderungen: Anzahl der API-Anforderungen, die fehlerhafte HTTP-Antwortcodes einschließlich 400 und alle über 500 erhalten haben.
* Nicht autorisierte Gatewayanforderungen: Anzahl der API-Anforderungen, die HTTP-Antwortcodes einschließlich 401, 403 und 429 erhalten haben.
* Andere Gateway-Anforderungen: Anzahl der API-Anforderungen, die HTTP-Antwortcodes erhalten haben, die keiner der oben genannten Kategorien angehören (z.B. 418).

![Metrikdiagramm](./media/api-management-azure-monitor/apim-monitor-metrics.png)

So greifen Sie auf Metriken zu:

1. Klicken Sie im Menü am unteren Seitenrand auf **Metriken**.

    ![Metriken](./media/api-management-azure-monitor/api-management-metrics-blade.png)

2. Wählen Sie im Dropdownmenü die gewünschten Metriken aus. Beispiel: **Successful Gateway Requests** (Erfolgreiche Gatewayanforderungen). Sie können auch weitere Metriken zum Diagramm hinzufügen.
3. Im Diagramm ist die Gesamtanzahl erfolgreicher API-Aufrufe gezeigt.

## <a name="set-up-an-alert-rule-for-unauthorized-request"></a>Einrichten einer Warnungsregel für nicht autorisierte Anforderungen

Sie können die Konfiguration so durchführen, dass Warnungen basierend auf Metriken und Aktivitätsprotokollen empfangen werden. Mit Azure Monitor können Sie eine Warnung so konfigurieren, dass Folgendes erfolgt, wenn sie ausgelöst wird:

* Senden einer E-Mail-Benachrichtigung
* Aufrufen eines Webhooks
* Aufrufen einer Azure-Logik-App

So konfigurieren Sie Warnungen:

1. Klicken Sie auf der Menüleiste am unteren Seitenrand auf **Warnungen (klassisch)**.

    ![alerts](./media/api-management-azure-monitor/api-management-alert-rules-blade.png)

2. Wählen Sie **Metrikwarnung hinzufügen** aus.
3. Geben Sie unter **Name** einen Namen für die Warnung ein.
4. Wählen Sie als zu überwachende Metrik **Nicht autorisierte Gatewayanforderungen** aus.
5. Wählen Sie **Email owners, contributors, and readers** (E-Mail an Besitzer, Mitwirkende und Leser senden) aus.
6. Klicken Sie auf **OK**.
7. Versuchen Sie, die Konferenz-API ohne API-Schlüssel aufzurufen. Als Besitzer dieses API Management-Diensts erhalten Sie eine E-Mail-Benachrichtigung. 

    > [!TIP]
    > Die Warnungsregel kann bei Auslösung auch einen Webhook oder eine Azure-Logik-App aufrufen.

    ![set-up-alert](./media/api-management-azure-monitor/set-up-alert.png)

## <a name="activity-logs"></a>Aktivitätsprotokolle

Aktivitätsprotokolle geben Einblick in die Vorgänge, die für Ihre API Management-Dienste ausgeführt wurden. Mit dem Aktivitätsprotokoll können Sie die Antworten auf die Fragen „Was“, „Wer“ und „Wann“ für alle Schreibvorgänge (PUT, POST, DELETE) ermitteln, die für Ihre API Management-Dienste durchgeführt wurden.

> [!NOTE]
> Aktivitätsprotokolle enthalten keine Lesevorgänge (GET) oder Vorgänge, die im Azure-Portal oder mithilfe der ursprünglichen Management-APIs durchgeführt wurden.

Sie können in Ihrem API Management-Dienst auf Aktivitätsprotokolle oder in Azure Monitor auf Protokolle all Ihrer Azure-Ressourcen zugreifen. 

![Aktivitätsprotokolle](./media/api-management-azure-monitor/apim-monitor-activity-logs.png)

So zeigen Sie Aktivitätsprotokolle an:

1. Wählen Sie Ihre APIM-Dienstinstanz aus.
2. Klicken Sie auf **Aktivitätsprotokoll**.

    ![Aktivitätsprotokoll](./media/api-management-azure-monitor/api-management-activity-logs-blade.png)

3. Wählen Sie den gewünschten Filterungsbereich, und klicken Sie auf **Anwenden**.

## <a name="diagnostic-logs"></a>Diagnoseprotokolle

Diagnoseprotokolle bieten umfassende Informationen zu Vorgängen und Fehlern, die zur Überwachung und Problembehandlung relevant sind. Diagnoseprotokolle unterscheiden sich von Aktivitätsprotokollen. Aktivitätsprotokolle geben Einblick in die Vorgänge, die für Ihre Azure-Ressourcen ausgeführt wurden. Diagnoseprotokolle bieten Einblick in Vorgänge, die Ihre Ressource selbst ausgeführt hat.

So konfigurieren Sie Diagnoseprotokolle:

1. Wählen Sie Ihre APIM-Dienstinstanz aus.
2. Klicken Sie auf **Diagnoseeinstellungen**.

    ![Diagnoseprotokolle](./media/api-management-azure-monitor/api-management-diagnostic-logs-blade.png)

3. Klicken Sie auf **Diagnose aktivieren**. Sie können Diagnoseprotokolle zusammen mit Metriken in einem Speicherkonto archivieren, an einen Event Hub streamen oder an Azure Monitor-Protokolle senden. 

API Management bietet derzeit Diagnoseprotokolle (stündlich erfasst) zu einzelnen API-Anforderungen, bei denen jeder Eintrag das folgende Schema aufweist:

```json
{  
    "isRequestSuccess" : "",
    "time": "",
    "operationName": "",
    "category": "",
    "durationMs": ,
    "callerIpAddress": "",
    "correlationId": "",
    "location": "",
    "httpStatusCodeCategory": "",
    "resourceId": "",
    "properties": {   
        "method": "", 
        "url": "", 
        "clientProtocol": "", 
        "responseCode": , 
        "backendMethod": "", 
        "backendUrl": "", 
        "backendResponseCode": ,
        "backendProtocol": "",  
        "requestSize": , 
        "responseSize": , 
        "cache": "", 
        "cacheTime": "", 
        "backendTime": , 
        "clientTime": , 
        "apiId": "",
        "operationId": "", 
        "productId": "", 
        "userId": "", 
        "apimSubscriptionId": "", 
        "backendId": "",
        "lastError": { 
            "elapsed" : "", 
            "source" : "", 
            "scope" : "", 
            "section" : "" ,
            "reason" : "", 
            "message" : ""
        } 
    }      
}  
```

| Eigenschaft  | Typ | BESCHREIBUNG |
| ------------- | ------------- | ------------- |
| isRequestSuccess | boolean | „True“, wenn die HTTP-Anforderung mit einem Antwortstatuscode im Bereich 2xx oder 3xx abgeschlossen wird |
| time | date-time | Zeitstempel des Eingangs der HTTP-Anforderung beim Gateway |
| operationName | Zeichenfolge | Konstanter Wert „Microsoft.ApiManagement/GatewayLogs“ |
| category | Zeichenfolge | Konstanter Wert „GatewayLogs“ |
| durationMs | integer | Anzahl von Millisekunden zwischen dem Zeitpunkt, zu dem das Gateway die Anforderung empfangen hat, und dem Zeitpunkt, zu dem die Antwort vollständig gesendet wurde |
| callerIpAddress | Zeichenfolge | IP-Adresse des unmittelbaren Gateway-Aufrufers (kann auch ein Zwischenaufrufer sein) |
| correlationId | Zeichenfolge | Von API Management zugewiesene eindeutige HTTP-Anforderungs-ID |
| location | Zeichenfolge | Name der Azure-Region, in der sich das Gateway befindet, das die Anforderung verarbeitet hat |
| httpStatusCodeCategory | Zeichenfolge | Kategorie des HTTP-Antwortstatuscodes: Erfolgreich (301 oder darunter, 304 oder 307), Nicht autorisiert (401, 403, 429), Erroneous (Fehler) (400, zwischen 500 und 600), Other (Sonstiges) |
| Ressourcen-ID | Zeichenfolge | ID der API Management-Ressource: /SUBSCRIPTIONS/<subscription>/RESOURCEGROUPS/<resource-group>/PROVIDERS/MICROSOFT.APIMANAGEMENT/SERVICE/<name> |
| Eigenschaften | object | Eigenschaften der aktuellen Anforderung |
| method | Zeichenfolge | HTTP-Methode der eingehenden Anforderung |
| URL | Zeichenfolge | URL der eingehenden Anforderung |
| clientProtocol | Zeichenfolge | HTTP-Protokollversion der eingehenden Anforderung |
| responseCode | integer | Statuscode der an einen Client gesendeten HTTP-Antwort |
| backendMethod | Zeichenfolge | HTTP-Methode der an ein Back-End gesendeten Anforderung |
| backendUrl | Zeichenfolge | URL der an ein Back-End gesendeten Anforderung |
| backendResponseCode | integer | Code der von einem Back-End empfangenen HTTP-Antwort |
| backendProtocol | Zeichenfolge | HTTP-Protokollversion der an ein Back-End gesendeten Anforderung | 
| requestSize | integer | Anzahl der Bytes, die von einem Client während der Anforderungsverarbeitung empfangen werden | 
| responseSize | integer | Anzahl der Bytes, die während der Anforderungsverarbeitung an einen Client gesendet werden | 
| cache | Zeichenfolge | Status der API Management-Cachenutzung bei der Anforderungsverarbeitung (d.h. Treffer, Fehler, Keiner) | 
| cacheTime | integer | Anzahl von Millisekunden für alle API Management-Cache-E/A-Vorgänge (Verbindungsherstellung, Senden und Empfangen von Bytes) | 
| backendTime | integer | Anzahl von Millisekunden für alle Back-End-E/A-Vorgänge (Verbindungsherstellung, Senden und Empfangen von Bytes) | 
| clientTime | integer | Anzahl von Millisekunden für alle Client-E/A-Vorgänge (Verbindungsherstellung, Senden und Empfangen von Bytes) | 
| apiId | Zeichenfolge | API-Entitätsbezeichner für die aktuelle Anforderung | 
| operationId | Zeichenfolge | Vorgangsentitätsbezeichner für die aktuelle Anforderung | 
| productId | Zeichenfolge | Produktentitätsbezeichner für die aktuelle Anforderung | 
| userId | Zeichenfolge | Benutzerentitätsbezeichner für die aktuelle Anforderung | 
| apimSubscriptionId | Zeichenfolge | Abonnemententitätsbezeichner für die aktuelle Anforderung | 
| backendId | Zeichenfolge | Back-End-Entitätsbezeichner für die aktuelle Anforderung | 
| LastError | object | Letzter Anforderungsverarbeitungsfehler | 
| elapsed | integer | Anzahl der Millisekunden, die zwischen dem Eingehen der Anforderung beim Gateway und dem Auftreten des Fehlers vergangen sind | 
| Quelle | Zeichenfolge | Der Fehler wurde durch den Namen der Richtlinie oder die Verarbeitung des internen Handlers verursacht. | 
| scope | Zeichenfolge | Der Fehler wurde durch den Bereich des Richtliniendokuments verursacht, das die Richtlinie enthält. | 
| section | Zeichenfolge | Der Fehler wurde durch den Abschnitt des Richtliniendokuments verursacht, der die Richtlinie enthält. | 
| reason | Zeichenfolge | Fehlerursache | 
| Message: | Zeichenfolge | Fehlermeldung | 

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Anzeigen von Aktivitätsprotokollen
> * Anzeigen von Diagnoseprotokollen
> * Anzeigen von Metriken Ihrer API
> * Einrichten einer Warnungsregel, wenn Ihre API nicht autorisierte Aufrufe empfängt

Fahren Sie mit dem nächsten Tutorial fort:

> [!div class="nextstepaction"]
> [Verwenden des API-Inspektors zur Verfolgung von Aufrufen in Azure API Management](api-management-howto-api-inspector.md)
