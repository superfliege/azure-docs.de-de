---
title: Quellen für Überwachungsdaten in Azure
description: Erfahren Sie etwas über alle zurzeit in Azure verfügbaren Überwachungsdatenquellen.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/12/2018
ms.author: johnkem
ms.subservice: ''
ms.openlocfilehash: ec61b179627702af7f2c4bc0fc0989230f5eca84
ms.sourcegitcommit: b4755b3262c5b7d546e598c0a034a7c0d1e261ec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2019
ms.locfileid: "54885795"
---
# <a name="consume-monitoring-data-from-azure"></a>Nutzen der Überwachungsdaten von Azure

Wir bringen mit der Azure Monitor-Pipeline die Überwachungsdaten von der gesamten Azure-Plattform an einem Ort zusammen. Wir wissen aber auch, dass bisher noch nicht alle Überwachungsdaten in dieser Pipeline verfügbar sind. In diesem Artikel werden die verschiedenen Möglichkeiten für den programmgesteuerten Zugriff auf Überwachungsdaten von Azure-Diensten zusammengefasst.

## <a name="options-for-data-consumption"></a>Optionen für die Datennutzung

| Datentyp | Category (Kategorie) | Unterstützte Dienste | Zugriffsmethoden |
| --- | --- | --- | --- |
| Azure Monitor-Metriken auf Plattformebene | Metriken | [Siehe Liste hier](metrics-supported.md) | <ul><li>**REST-API:** [Azure Monitor-Metrik-API](https://docs.microsoft.com/rest/api/monitor/metrics)</li><li>**Speicherblob oder Event Hub:** [Diagnoseeinstellungen](diagnostic-logs-overview.md#diagnostic-settings)</li></ul> |
| Compute-Metriken für Gastbetriebssysteme (z.B. Leistungsindikatoren) | Metriken | [Windows](/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines)- und Linux-VMs (V2), [Clouddienste](../../cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md), [Service Fabric](../../service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md) | <ul><li>**Speichertabelle oder -blob:** [Azure-Diagnose unter Windows oder Linux](diagnostics-extension-to-storage.md)</li><li>**Event Hub:** [Azure-Diagnose unter Windows](diagnostics-extension-stream-event-hubs.md)</li></ul> |
| Benutzerdefinierte oder Anwendungsmetriken | Metriken | Alle mit Application Insights instrumentierten Anwendungen | <ul><li>**REST-API:** [Application Insights-REST-API](https://dev.applicationinsights.io/reference)</li></ul> |
| Speichermetrik | Metriken | Azure Storage | <ul><li>**Speichertabelle:** [Speicheranalyse](https://docs.microsoft.com/rest/api/storageservices/storage-analytics)</li></ul> |
| Abrechnungsdaten | Metriken | Alle Azure-Dienste | <ul><li>**REST-API:** [Azure-APIs zur Ressourcennutzung und für RateCard](../../billing/billing-usage-rate-card-overview.md)</li></ul> |
| Aktivitätsprotokoll | Ereignisse | Alle Azure-Dienste | <ul><li>**REST-API:** [Azure Monitor-Ereignis-API](https://docs.microsoft.com/rest/api/monitor/eventcategories)</li><li>**Speicherblob oder Event Hub:** [Protokollprofil](activity-logs-overview.md#export-the-activity-log-with-a-log-profile)</li></ul> |
| Azure Monitor-Diagnoseprotokolle | Ereignisse | [Siehe Liste hier](diagnostic-logs-schema.md) | <ul><li>**Speicherblob oder Event Hub:** [Diagnoseeinstellungen](diagnostic-logs-overview.md#diagnostic-settings)</li></ul> |
| Compute-Protokolle für Gastbetriebssysteme (z.B. IIS, ETW, syslogs) | Ereignisse | [Windows](/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines)- und Linux-VMs (V2), [Clouddienste](../../cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md), [Service Fabric](../../service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md) | <ul><li>**Speichertabelle oder -blob:** [Azure-Diagnose unter Windows oder Linux](diagnostics-extension-to-storage.md)</li><li>**Event Hub:** [Azure-Diagnose unter Windows](diagnostics-extension-stream-event-hubs.md)</li></ul> |
| App Service-Protokolle | Ereignisse | App Services | <ul><li>**Datei-, Tabellen- oder Blobspeicher:** [Web-App-Diagnose](../../app-service/troubleshoot-diagnostic-logs.md)</li></ul> |
| Speicherprotokolle | Ereignisse | Azure Storage | <ul><li>**Speichertabelle:** [Speicheranalyse](https://docs.microsoft.com/rest/api/storageservices/storage-analytics)</li></ul> |
| Security Center-Warnungen | Ereignisse | Azure Security Center | <ul><li>**REST-API:** [Sicherheitswarnungen](https://msdn.microsoft.com/library/mt704050.aspx)</li></ul> |
| Active Directory-Berichte | Ereignisse | Azure Active Directory | <ul><li>**REST-API:** [Azure Active Directory Graph-API](../../active-directory/reports-monitoring/concept-reporting-api.md)</li></ul> |
| Security Center-Ressourcenstatus | Status | [Alle unterstützten Ressourcen](https://msdn.microsoft.com/library/mt704041.aspx#Anchor_1) | <ul><li>**REST-API:** [Sicherheitsstatus](https://msdn.microsoft.com/library/mt704041.aspx)</li></ul> |
| Resource Health | Status | Unterstützte Dienste | <ul><li>**REST-API:** [Ressourcenintegritäts-REST-API](https://azure.microsoft.com/blog/reduce-troubleshooting-time-with-azure-resource-health/)</li></ul> |
| Azure Monitor-Metrikwarnungen | Benachrichtigungen | [Siehe Liste hier](metrics-supported.md) | <ul><li>**Webhook:** [Azure-Metrikwarnungen](alerts-webhooks.md)</li></ul> |
| Azure Monitor-Aktivitätsprotokollwarnungen | Benachrichtigungen | Alle Azure-Dienste | <ul><li>**Webhook:** Azure-Aktivitätsprotokollwarnungen</li></ul> |
| Benachrichtigungen zum automatischen Skalieren | Benachrichtigungen | [Siehe Liste hier](autoscale-overview.md#supported-services-for-autoscale) | <ul><li>**Webhook:** [Benachrichtigung über Autoskalierung mit dem Webhook-Nutzlastschema](autoscale-webhook-email.md#autoscale-notification-webhook-payload-schema)</li></ul> |
| Warnungen zu Protokollsuchabfragen | Benachrichtigungen | Log Analytics | <ul><li>**Webhook:** [Webhookaktion für Protokollwarnungsregeln](alerts-log-webhook.md)</li></ul> |
| Application Insights-Metrikwarnungen | Benachrichtigungen | Application Insights | <ul><li>**Webhook:** [Application Insights-Warnungen](../../azure-monitor/app/alerts.md)</li></ul> |
| Application Insights-Webtests | Benachrichtigungen | Application Insights | <ul><li>**Webhook:** [Application Insights-Warnungen](../../azure-monitor/app/alerts.md)</li></ul> |

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr zu [Azure Monitor-Metriken](data-collection.md).
- Erfahren Sie mehr zum [Azure-Aktivitätsprotokoll](activity-logs-overview.md).
- Erfahren Sie mehr zu [Azure-Diagnoseprotokollen](diagnostic-logs-overview.md).

