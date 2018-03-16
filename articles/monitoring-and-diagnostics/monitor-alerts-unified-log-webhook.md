---
title: "Webhookaktionen für Protokollwarnungen in Azure Alerts (Vorschauversion) | Microsoft-Dokumentation"
description: "In diesem Artikel wird beschrieben, wie eine Protokollwarnungsregel mit Log Analytics oder Application Insights per Pushvorgang Daten als HTTP-Webhook überträgt, und es werden Details zu den verschiedenen möglichen Anpassungen erläutert."
author: msvijayn
manager: kmadnani1
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 2/2/2018
ms.author: vinagara
ms.openlocfilehash: 5852eb099f6620656aa69e5085447c2a8b4e0c01
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2018
---
# <a name="webhook-actions-for-log-alert-rules"></a>Webhookaktionen für Protokollwarnungsregeln
Wenn eine [Warnung in Azure (Vorschauversion) erstellt wird](monitor-alerts-unified-usage.md), haben Sie die Möglichkeit [mithilfe von Aktionsgruppen die Ausführung einer oder mehrerer Aktionen zu konfigurieren](monitoring-action-groups.md).  Dieser Artikel beschreibt die verschiedenen verfügbaren Webhookaktionen und Details zur Konfiguration des benutzerdefinierten JSON-basierten Webhooks.


## <a name="webhook-actions"></a>Webhookaktionen

Mit Webhookaktionen können Sie einen externen Prozess über eine HTTP POST-Anforderung aufrufen.  Der aufgerufene Dienst sollte Webhooks unterstützen, und es sollte festgelegt werden, wie empfangene Nutzlasten verwendet werden.   Beispiele für die Verwendung eines Webhooks als Antwort auf eine Warnung sind das Senden einer Nachricht in [Slack](http://slack.com) oder Erstellen eines Vorfalls in [PagerDuty](http://pagerduty.com/).  

Für Webhookaktionen sind die in der folgenden Tabelle aufgeführten Eigenschaften erforderlich:

| Eigenschaft | BESCHREIBUNG |
|:--- |:--- |
| Webhook-URL |Die URL des Webhooks. |
| Benutzerdefinierte JSON-Nutzlast |Benutzerdefinierte Nutzlast zum Senden mit dem Webhook, wenn diese Option während der Warnungserstellung ausgewählt wird. Ausführliche Informationen finden Sie im Artikel zum [Verwalten von Warnungen mithilfe von Azure Alerts (Vorschauversion)](monitor-alerts-unified-usage.md) |

> [!NOTE]
> Die Schaltfläche „Testwebhook“ neben der Option *Benutzerdefinierte JSON-Nutzlast für Webhook einschließen* für die Protokollwarnung löst einen Pseudoaufruf zum Testen der Webhook-URL aus. Es sind keine tatsächlichen Daten enthalten, und der Vorgang ist für ein JSON-Schema für Protokollwarnungen repräsentativ. 

Webhooks enthalten eine URL und eine Nutzlast im JSON-Format, wobei es sich um die an den externen Dienst gesendeten Daten handelt.  Standardmäßig enthält die Nutzlast die Werte in der folgenden Tabelle: Sie können diese Nutzlast durch eine eigene benutzerdefinierte Nutzlast ersetzen, wenn Sie dies wünschen.  In diesem Fall können Sie die Variablen in der Tabelle für die einzelnen Parameter verwenden, um deren Wert in die benutzerdefinierte Nutzlast einzubinden.


| Parameter | Variable | BESCHREIBUNG |
|:--- |:--- |:--- |
| AlertRuleName |#alertrulename |Der Name der Warnungsregel. |
| Severity |#severity |Festgelegter Schweregrad für die erste ausgelöste Protokollwarnung |
| AlertThresholdOperator |#thresholdoperator |Schwellenwertoperator für die Warnungsregel  *Größer als* oder *Kleiner als* |
| AlertThresholdValue |#thresholdvalue |Wert des Schwellenwerts für die Warnungsregel |
| LinkToSearchResults |#linktosearchresults |Link zur Log Analytics-Protokollsuche, mit der die Datensätze aus der Abfrage zurückgegeben werden, mit der die Warnung erstellt wurde |
| ResultCount |#searchresultcount |Anzahl von Datensätzen in den Suchergebnissen |
| Endzeit für das Suchintervall |#searchintervalendtimeutc |Endzeit für die Abfrage im UTC-Format |
| Suchintervall |#searchinterval |Zeitfenster für die Warnungsregel |
| Startzeit für das Suchintervall |#searchintervalstarttimeutc |Startzeit für die Abfrage im UTC-Format 
| SearchQuery |#searchquery |Von der Warnungsregel verwendete Protokollsuchabfrage |
| SearchResults |"IncludeSearchResults": true|Von der Abfrage als JSON-Tabelle zurückgegebene Datensätze, die auf die ersten 1.000 Datensätze beschränkt sind; wenn "IncludeSearchResults": true in der benutzerdefinierten JSON-Webhookdefinition als Eigenschaft der obersten Ebene hinzugefügt wird |
| WorkspaceID |#workspaceid |ID Ihres Log Analytics-Arbeitsbereichs (OMS) |
| Anwendungs-ID |#applicationid |ID Ihrer Application Insight-App |
| Abonnement-ID |#subscriptionid |ID Ihres Azure-Abonnements, das für Application Insights verwendet wird 


Sie können beispielsweise die folgende benutzerdefinierte Nutzlast angeben, die einen einzelnen Parameter wie *text*enthält.  Der Dienst, der von diesem Webhook aufgerufen wird, erwartet diesen Parameter.

    {
        "text":"#alertrulename fired with #searchresultcount over threshold of #thresholdvalue."
    }

Diese Beispielnutzlast wird ähnlich wie hier dargestellt aufgelöst, wenn sie an den Webhook gesendet wird.

    {
        "text":"My Alert Rule fired with 18 records over threshold of 10 ."
    }

Um Suchergebnisse in eine benutzerdefinierte Nutzlast einzuschließen, muss **IncudeSearchResults** als Eigenschaft der obersten Ebene in der JSON-Nutzlast festgelegt werden. 

## <a name="sample-payloads"></a>Beispielnutzlasten
Dieser Abschnitt zeigt eine Beispielnutzlast für den Webhook für Protokollwarnungen, einschließlich Fällen mit Standardnutzlast und benutzerdefinierter Nutzlast.

> [!NOTE]
> Zum Gewährleisten der Abwärtskompatibilität ist die Standard-Webhooknutzlast für Warnungen mit Azure Log Analytics identisch mit der [OMS-Warnungsverwaltung](../log-analytics/log-analytics-alerts-creating.md). Für Protokollwarnungen mit [Application Insights](../application-insights/app-insights-analytics.md) basiert die Standard-Webhooknutzlast aber auf dem Aktionsgruppenschema.

### <a name="standard-webhook-for-log-alerts"></a>Standardwebhook für Protokollwarnungen 
In beiden Beispielen wurde eine Pseudonutzlast mit nur zwei Spalten und zwei Zeilen angegeben.

#### <a name="log-alert-for-azure-log-analytics"></a>Protokollwarnung für Azure Log Analytics
Im Folgenden wird eine Beispielnutzlast für eine Standard-Webhookaktion *ohne benutzerdefinierte JSON-Nutzlast* für Log Analytics-basierte Warnungen verwendet.

    {
    "WorkspaceId":"12345a-1234b-123c-123d-12345678e",
    "AlertRuleName":"AcmeRule","SearchQuery":"search *",
    "SearchResult":
        {
        "tables":[
                    {"name":"PrimaryResult","columns":
                        [
                        {"name":"$table","type":"string"},
                        {"name":"Id","type":"string"},
                        {"name":"TimeGenerated","type":"datetime"},
                        ],
                    "rows":
                        [
                            ["Fabrikam","33446677a","2018-02-02T15:03:12.18Z"],
                            ["Contoso","33445566b","2018-02-02T15:16:53.932Z"]
                        ]
                    }
                ]
        }
    "SearchIntervalStartTimeUtc": "2018-03-26T08:10:40Z",
    "SearchIntervalEndtimeUtc": "2018-03-26T09:10:40Z",
    "AlertThresholdOperator": "Greater Than",
    "AlertThresholdValue": 0,
    "ResultCount": 2,
    "SearchIntervalInSeconds": 3600,
    "LinkToSearchResults": "https://workspaceID.portal.mms.microsoft.com/#Workspace/search/index?_timeInterval.intervalEnd=2018-03-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
    "Description": null,
    "Severity": "Warning"
    }
    


#### <a name="log-alert-for-azure-application-insights"></a>Protokollwarnung für Azure Application Insights
Im Folgenden sehen Sie eine Beispielnutzlast für einen Standardwebhook *ohne benutzerdefinierte JSON-Nutzlast* bei der Verwendung für Application Insights-basierte Protokollwarnungen.
    

    {
    "schemaId":"Microsoft.Insights/LogAlert","data":
    { 
    "SubscriptionId":"12345a-1234b-123c-123d-12345678e",
    "AlertRuleName":"AcmeRule","SearchQuery":"search *",
    "SearchResult":
        {
        "tables":[
                    {"name":"PrimaryResult","columns":
                        [
                        {"name":"$table","type":"string"},
                        {"name":"Id","type":"string"},
                        {"name":"TimeGenerated","type":"datetime"},
                        ],
                    "rows":
                        [
                            ["Fabrikam","33446677a","2018-02-02T15:03:12.18Z"],
                            ["Contoso","33445566b","2018-02-02T15:16:53.932Z"]
                        ]
                    }
                ]
        }
    "SearchIntervalStartTimeUtc": "2018-03-26T08:10:40Z",
    "SearchIntervalEndtimeUtc": "2018-03-26T09:10:40Z",
    "AlertThresholdOperator": "Greater Than",
    "AlertThresholdValue": 0,
    "ResultCount": 2,
    "SearchIntervalInSeconds": 3600,
    "LinkToSearchResults": "https://analytics.applicationinsights.io/subscriptions/12345a-1234b-123c-123d-12345678e/?query=search+*+&timeInterval.intervalEnd=2018-03-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
    "Description": null,
    "Severity": "Error"
    "ApplicationId": "123123f0-01d3-12ab-123f-abc1ab01c0a1"
    }
    }


#### <a name="log-alert-with-custom-json-payload"></a>Protokollwarnung mit benutzerdefinierter JSON-Nutzlast
Sie können beispielsweise Folgendes verwenden, um eine benutzerdefinierte Nutzlast zu erstellen, die nur den Warnungsnamen und die Suchergebnisse enthält: 

    {
       "alertname":"#alertrulename",
       "IncludeSearchResults":true
    }

Im Folgenden sehen Sie eine Beispielnutzlast für eine benutzerdefinierte Webhookaktion für eine beliebige Protokollwarnung.
    

    {
    "alertname":"AcmeRule","IncludeSearchResults":true,
    "SearchResult":
        {
        "tables":[
                    {"name":"PrimaryResult","columns":
                        [
                        {"name":"$table","type":"string"},
                        {"name":"Id","type":"string"},
                        {"name":"TimeGenerated","type":"datetime"},
                        ],
                    "rows":
                        [
                            ["Fabrikam","33446677a","2018-02-02T15:03:12.18Z"],
                            ["Contoso","33445566b","2018-02-02T15:16:53.932Z"]
                        ]
                    }
                ]
        }
    }




## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über [Protokollwarnungen auf der Azure-Oberfläche „Warnungen (Vorschauversion)“](monitor-alerts-unified-log.md).
- Erstellen und Verwalten von [Aktionsgruppen in Azure](monitoring-action-groups.md)
- Weitere Informationen zu [Application Insights](../application-insights/app-insights-analytics.md)
- Erfahren Sie mehr über [Log Analytics](../log-analytics/log-analytics-overview.md). 