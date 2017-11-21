---
title: "Konfigurieren von Integritätsbenachrichtigungen für vorhandene Problemverwaltungssysteme mit einem Webhook | Microsoft-Dokumentation"
description: Erhalten Sie personalisierte Benachrichtigungen zu Service Health-Ereignissen an Ihr vorhandenes Problemverwaltungssystem.
author: shawntabrizi
manager: scotthit
editor: 
services: service-health
documentationcenter: service-health
ms.assetid: 
ms.service: service-health
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2017
ms.author: shtabriz
ms.openlocfilehash: b6a5f61f61675b825dcfe9c706c80944f5890538
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2017
---
# <a name="configure-health-notifications-for-existing-problem-management-systems-using-a-webhook"></a>Konfigurieren von Integritätsbenachrichtigungen für vorhandene Problemverwaltungssysteme mit einem Webhook

In diesem Artikel wird beschrieben, wie Sie Ihre Service Health-Warnungen zum Senden von Daten über Webhooks an Ihr vorhandenes Benachrichtigungssystem konfigurieren.

Heute können Sie Service Health-Warnungen so konfigurieren, dass Sie per SMS oder E-Mail-benachrichtigt werden, wenn Sie von einem Azure Service-Vorfall betroffen sind.
Allerdings ist möglicherweise bereits ein externes Benachrichtigungssystem vorhanden, das Sie verwenden möchten.
In diesem Dokument werden die wichtigsten Bestandteile der Webhooknutzlast beschrieben, und Sie erfahren, wie Sie benutzerdefinierte Warnungen erstellen, um benachrichtigt zu werden, wenn der Dienstprobleme auftreten, die Sie betreffen.

Wenn Sie eine vorkonfigurierte Integration verwenden möchten, ziehen Sie folgende Artikel zurate:
* [Konfigurieren von Warnungen mit ServiceNow](service-health-alert-webhook-servicenow.md)
* [Konfigurieren von Warnungen mit PagerDuty](service-health-alert-webhook-pagerduty.md)
* [Konfigurieren von Warnungen mit OpsGenie](service-health-alert-webhook-opsgenie.md)

## <a name="configuring-a-custom-notification-using-the-service-health-webhook-payload"></a>Konfigurieren einer benutzerdefinierten Benachrichtigung mit der Service Health-Webhooknutzlast
Wenn Sie eine eigene benutzerdefinierte Webhookintegration einrichten möchten, müssen Sie die während der Service Health-Benachrichtigungen gesendete JSON-Nutzlast analysieren.

[Hier finden Sie ein Beispiel](../monitoring-and-diagnostics/monitoring-activity-log-alerts-webhook.md), wie die `Service Health`-Webhooknutzlast aussieht.

Mit einem Blick auf `context.eventSource == "ServiceHealth"` können Sie erkennen, dass dies eine Service Health-Warnung ist. Die folgenden Eigenschaften sind dabei die wichtigsten:
 * `data.context.activityLog.status`
 * `data.context.activityLog.level`
 * `data.context.activityLog.subscriptionId`
 * `data.context.activityLog.properties.title`
 * `data.context.activityLog.properties.impactStartTime`
 * `data.context.activityLog.properties.communication`
 * `data.context.activityLog.properties.impactedServices`
 * `data.context.activityLog.properties.trackingId`

## <a name="creating-a-direct-link-to-azure-service-health-for-an-incident"></a>Erstellen eines direkten Links zu Azure Service Health für einen Vorfall
Sie können einen direkten Link zu Ihrem personalisierten Azure Service Health-Vorfall auf dem Desktop oder mobilen Gerät erstellen, indem Sie eine spezialisierte URL generieren. Erstellen Sie den Link unter Verwendung der `trackingId` sowie der ersten und letzten drei Ziffern Ihrer `subscriptionId` wie folgt:
```
https://app.azure.com/h/<trackingId>/<first and last three digits of subscriptionId>
```

Wenn Ihre `subscriptionId` z. B. `bba14129-e895-429b-8809-278e836ecdb3` lautet und Ihre `trackingId` `0DET-URB`, dann sieht Ihre personalisierte Azure Service Health-URL folgendermaßen aus:

```
https://app.azure.com/h/0DET-URB/bbadb3
```

## <a name="using-the-level-to-detect-the-severity-of-the-issue"></a>Erkennen des Schweregrads des Problems anhand der Eigenschaft „Level“
Vom niedrigsten Schweregrad zum höchsten Schweregrad kann die Eigenschaft `level` in der Nutzlast folgende Werte annehmen: `Informational`, `Warning`, `Error` und `Critical`.

## <a name="parsing-the-impacted-services-to-understand-the-full-scope-of-the-incident"></a>Analysieren der betroffenen Dienste, um das komplette Ausmaß des Vorfalls zu verstehen
Service Health-Warnungen informieren Sie über Probleme über mehrere Regionen und Dienste hinweg. Um die vollständigen Details zu erhalten, müssen Sie den Wert von `impactedServices` analysieren.
Der enthaltene Inhalt ist eine [JSON-Zeichenfolge mit Escapezeichen](http://json.org/); wenn ohne Escapezeichen, dann enthält es ein anderes JSON-Objekt, das normal analysiert werden kann.

```json
{"data.context.activityLog.properties.impactedServices": "[{\"ImpactedRegions\":[{\"RegionName\":\"Australia East\"},{\"RegionName\":\"Australia Southeast\"}],\"ServiceName\":\"Alerts & Metrics\"},{\"ImpactedRegions\":[{\"RegionName\":\"Australia Southeast\"}],\"ServiceName\":\"App Service\"}]"}
```

Wird zu:

```json
[
   {
      "ImpactedRegions":[
         {
            "RegionName":"Australia East"
         },
         {
            "RegionName":"Australia Southeast"
         }
      ],
      "ServiceName":"Alerts & Metrics"
   },
   {
      "ImpactedRegions":[
         {
            "RegionName":"Australia Southeast"
         }
      ],
      "ServiceName":"App Service"
   }
]
```

Dies zeigt, dass es Probleme mit „Warnungen & Metriken“ in Australien, Osten und Südosten, sowie Probleme mit „App Service“ in Australien, Südosten, gibt.


## <a name="testing-your-webhook-integration-via-an-http-post-request"></a>Testen der Webhookintegration über eine HTTP POST-Anforderung
1. Erstellen Sie die Service Health-Nutzlast, die Sie senden möchten. Eine Service Health-Beispielwebhook-Nutzlast finden Sie unter [Webhooks für Azure-Aktivitätsprotokollwarnungen](../monitoring-and-diagnostics/monitoring-activity-log-alerts-webhook.md).

2. Erstellen Sie eine HTTP POST-Anforderung, indem Sie wie folgt vorgehen:

    ```
    POST        https://your.webhook.endpoint

    HEADERS     Content-Type: application/json

    BODY        <Service Health payload>
    ```
3. Sie sollten eine `2XX - Successful`-Antwort erhalten.

4. Wechseln Sie zu [PagerDuty](https://www.pagerduty.com/), um zu überprüfen, ob Ihre Integration erfolgreich eingerichtet wurde.

## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zum [Webhookschema für Aktivitätsprotokollwarnungen](../monitoring-and-diagnostics/monitoring-activity-log-alerts-webhook.md). 
- Weitere Informationen zu [Dienstintegritätsbenachrichtigungen](../monitoring-and-diagnostics/monitoring-service-notifications.md).
- Weitere Informationen zu [Aktionsgruppen](../monitoring-and-diagnostics/monitoring-action-groups.md).