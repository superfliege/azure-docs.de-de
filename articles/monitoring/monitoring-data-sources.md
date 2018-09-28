---
title: Quellen für Daten in Azure Monitor| Microsoft-Dokumentation
description: Beschreibt die verfügbaren Daten zum Überwachen von Integrität und Leistung Ihrer Azure-Ressourcen und der darauf ausgeführten Anwendungen.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: monitoring
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/15/2018
ms.author: bwren
ms.openlocfilehash: b10236a1e0307c9464d58e50eb0c7b4e6a60b5e5
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46987772"
---
# <a name="sources-of-data-in-azure-monitor"></a>Quellen für Daten in Azure Monitor
In diesem Artikel werden die Quellen von Daten beschrieben, die von Azure Monitor zum Überwachen von Integrität und Leistung Ihrer Azure-Ressourcen und der darauf ausgeführten Anwendungen gesammelt werden. Diese Ressourcen können in Azure, in einer anderen Cloud oder lokal ausgeführt werden.  Details dazu, wie diese Daten gespeichert werden und wie Sie sie anzeigen können, finden Sie unter [Von Azure Monitor gesammelte Daten](monitoring-data-collection.md).

Zur Überwachung von Daten in Azure werden zahlreiche Quellen genutzt, die in Ebenen organisiert werden können, wobei die höchste Ebene Ihre Anwendung und eventuelle Betriebssysteme und die niedrigste Ebene die Komponenten der Azure-Plattform darstellen. Dies wird im folgenden Diagramm veranschaulicht, und jede Ebene wird in den folgenden Abschnitten ausführlich beschrieben.

![Überwachungsdatenebenen](media/monitoring-data-sources/monitoring-tiers.png)

## <a name="azure-tenant"></a>Azure-Mandant
Die auf Ihren Azure-Mandanten bezogenen Telemetriedaten werden von mandantenweiten Diensten gesammelt, wie z.B. Azure Active Directory.

![Azure-Mandantensammlung](media/monitoring-data-sources/tenant-collection.png)

### <a name="azure-active-directory-audit-logs"></a>Azure Active Directory-Überwachungsprotokolle
[Azure Active Directory-Berichterstellung](../active-directory/reports-monitoring/overview-reports.md) beinhaltet den Verlauf der Anmeldeaktivität und den Überwachungspfad von Änderungen, die innerhalb eines bestimmten Mandanten vorgenommen wurden. Diese Überwachungsprotokolle können zur Analyse mit anderen Protokolldaten nach Log Analytics geschrieben werden.


## <a name="azure-platform"></a>Azure Platform
Telemetriedaten zur Integrität und zum Betrieb von Azure selbst beinhalten Daten zum Betrieb und zur Verwaltung eines Azure-Abonnements. Dies beinhaltet Service Health-Daten, die im Azure-Aktivitätsprotokoll und in den Überwachungsprotokollen von Azure Active Directory gespeichert werden.

![Azure-Abonnementsammlung](media/monitoring-data-sources/azure-collection.png)

### <a name="azure-service-health"></a>Azure Service Health
[Azure Service Health](../monitoring-and-diagnostics/monitoring-service-notifications.md) enthält Informationen zur Integrität der Azure-Dienste in Ihrem Abonnement, von denen Ihre Anwendung und Ressourcen abhängen. Sie können Warnungen erstellen, um über aktuelle und erwartete kritische Probleme benachrichtigt zu werden, die sich möglicherweise auf Ihre Anwendung auswirken. Service Health-Datensätze werden im [Azure-Aktivitätsprotokoll](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) gespeichert, sodass Sie sie im Activity Log Explorer anzeigen und in Log Analytics kopieren können.

### <a name="azure-activity-log"></a>Azure-Aktivitätsprotokoll
Das [Azure-Aktivitätsprotokoll](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) enthält Service Health-Datensätze sowie Datensätze zu jeglichen Konfigurationsänderungen, die an Ihren Azure-Ressourcen vorgenommen wurden. Das Aktivitätsprotokoll ist für alle Azure-Ressourcen verfügbar und stellt ihre _externe_ Ansicht dar. Die spezifischen Typen von Datensätzen im Aktivitätsprotokoll werden im [Ereignisschema des Azure-Aktivitätsprotokolls](../monitoring-and-diagnostics/monitoring-activity-log-schema.md) beschrieben.

Sie können das Aktivitätsprotokoll für eine bestimmte Ressource auf deren Seite im Azure-Portal anzeigen oder Protokolle mehrerer Ressourcen im [Activity Log Explorer](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) anzeigen. Es ist besonders nützlich, die Protokolleinträge in Log Analytics zu kopieren, um sie mit anderen Überwachungsdaten zu kombinieren. Sie können sie auch mithilfe von [Event Hubs](../monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs.md) an andere Speicherorte senden.



## <a name="azure-services"></a>Azure-Dienste
Diagnoseprotokolle auf Metriken- und Ressourcenebene enthalten Informationen zum _internen_ Betrieb der Azure-Ressourcen. Diese sind für die meisten Azure-Dienste verfügbar, und Verwaltungslösungen bieten zusätzliche Einblicke in bestimmte Dienste.

![Azure-Ressourcensammlung](media/monitoring-data-sources/azure-resource-collection.png)


### <a name="metrics"></a>Metriken
Die meisten Azure-Dienste generieren [Plattformmetriken](monitoring-data-collection.md#metrics), die ihre Leistung und ihren Betrieb widerspiegeln. Die spezifischen [Metriken variieren je nach Ressourcentyp](../monitoring-and-diagnostics/monitoring-supported-metrics.md).  Im Metrik-Explorer kann auf sie zugegriffen werden, und sie können für Trend- und sonstige Analysen in Log Analytics kopiert werden.


### <a name="resource-diagnostic-logs"></a>Ressourcendiagnoseprotokolle
Während das Aktivitätsprotokoll Informationen über die Vorgänge liefert, die auf Azure-Ressourcen ausgeführt werden, bieten [Diagnoseprotokolle](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) auf Ressourcenebene Einblicke in den Betrieb der Ressource selbst.   Konfigurationsanforderungen und Inhalt dieser Protokolle [variieren je nach Ressourcentyp](../monitoring-and-diagnostics/monitoring-diagnostic-logs-schema.md).

Sie können Diagnoseprotokolle nicht direkt im Azure-Portal anzeigen, aber Sie können [sie zum Archivieren an den Azure-Speicher senden](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md) und für die Umleitung zu anderen Diensten nach [Event Hub](../event-hubs/event-hubs-what-is-event-hubs.md) bzw. zur Analyse nach [Log Analytics](../monitoring-and-diagnostics/monitor-stream-diagnostic-logs-log-analytics.md) exportieren. Einige Ressourcen können direkt in Log Analytics schreiben, während andere in ein Speicherkonto schreiben, bevor sie [in Log Analytics importiert](../log-analytics/log-analytics-azure-storage-iis-table.md#use-the-azure-portal-to-collect-logs-from-azure-storage) werden.

### <a name="monitoring-solutions"></a>Überwachungslösungen
 [Überwachungslösungen](../monitoring/monitoring-solutions.md) sammeln Daten, um zusätzliche Erkenntnisse zum Betrieb eines bestimmten Diensts oder einer bestimmten Anwendung zu liefern. Sie sammeln Daten in Log Analytics, wo sie mit der [Abfragesprache](../log-analytics/log-analytics-log-search.md) oder [Sichten](../log-analytics/log-analytics-view-designer.md), die in der Regel in der Lösung enthalten sind, analysiert werden können.

## <a name="guest-operating-system"></a>Gastbetriebssystem
Computeressourcen in Azure, in anderen Clouds und lokal haben ein Gastbetriebssystem zu überwachen. Mit der Installation mindestens eines Agents können Sie Telemetriedaten aus dem Gastbetriebssystem in den gleichen Überwachungstools sammeln, die sie für die Azure-Dienste selbst verwenden.

![Azure-Computeressourcensammlung](media/monitoring-data-sources/compute-resource-collection.png)

### <a name="diagnostic-extension"></a>Diagnoseerweiterung
Mit der [Azure-Diagnoseerweiterung](../monitoring-and-diagnostics/azure-diagnostics.md) können Sie Protokolle und Leistungsdaten des Clientbetriebssystems der Azure-Computeressourcen sammeln. Sowohl auf den Clients gesammelte Metriken als auch Protokolle werden in einem Azure-Speicherkonto gespeichert, sodass Sie [Log Analytics konfigurieren können, daraus zu importieren](../log-analytics/log-analytics-azure-storage-iis-table.md#use-the-azure-portal-to-collect-logs-from-azure-storage).  Der Metrik-Explorer kann aus dem Speicherkonto lesen und enthält die Clientmetriken zusammen mit anderen gesammelten Metriken.


### <a name="log-analytics-agent"></a>Log Analytics-Agent
Sie können den Log Analytics-Agent auf jeder [Windows](../log-analytics/log-analytics-agent-windows.md)- oder [Linux]()-VM bzw. jedem physischen Computer installieren. Der virtuelle Computer kann in Azure, einer anderen Cloud oder lokal ausgeführt werden.  Der Agent stellt entweder direkt oder über eine [verbundene System Center Operations Manager-Verwaltungsgruppe](../log-analytics/log-analytics-om-agents.md) eine Verbindung mit Log Analytics her und ermöglicht Ihnen, Daten aus [Datenquellen](../log-analytics/log-analytics-data-sources.md) zu sammeln, die Sie konfigurieren, oder aus [Verwaltungslösungen](../monitoring/monitoring-solutions.md), die zusätzliche Einblicke in Anwendungen ermöglichen, die auf dem virtuellen Computer ausgeführt werden.

### <a name="service-map"></a>Dienstzuordnung
[Service Map](../operations-management-suite/operations-management-suite-service-map.md) erfordert einen Dependency-Agent für Windows- und Linux-VMs. So lassen sich mit dem Log Analytics-Agent Daten über Prozesse sammeln, die auf dem virtuellen Computer ausgeführten werden, und über Abhängigkeiten von externen Prozessen. Er speichert diese Daten in Log Analytics und enthält eine Konsole, die die gesammelten Daten zusätzlich zu anderen in Log Analytics gespeicherten Daten anzeigt.

## <a name="applications"></a>ANWENDUNGEN
Zusätzlich zu den Telemetriedaten, die Ihre Anwendung möglicherweise in das Gastbetriebssystem schreibt, erfolgt eine ausführliche Anwendungsüberwachung mit [Application Insights](https://docs.microsoft.com/azure/application-insights/). Application Insights kann Daten aus Anwendungen sammeln, die auf einer Vielzahl von Plattformen ausgeführt werden. Die Anwendung kann in Azure, einer anderen Cloud oder lokal ausgeführt werden.

![Anwendungsdatensammlung](media/monitoring-data-sources/application-collection.png)


### <a name="application-data"></a>Anwendungsdaten
Wenn Sie Application Insights durch Installation eines Instrumentierungspakets für eine Anwendung aktivieren, werden auf Leistung und Betrieb der Anwendung bezogene Metriken und Protokolle gesammelt. Dies umfasst detaillierte Informationen zu Seitenansichten, Anwendungsanforderungen und Ausnahmen. Application Insights speichert die gesammelten Daten in Azure-Metriken und Log Analytics. Es enthält umfassende Tools zum Analysieren der Daten, aber Sie können sie auch mithilfe von Tools wie dem Metrik-Explorer und Protokollsuchen mit Daten aus anderen Quellen analysieren.

Sie können mit Application Insights auch [eine benutzerdefinierte Metrik erstellen](../application-insights/app-insights-api-custom-events-metrics.md).  So können Sie Ihre eigene Logik zum Berechnen eines numerischen Werts und anschließendem Speichern dieses Werts mit anderen Metriken definieren, auf die mit dem Metrik-Explorer zugegriffen werden kann und die für [automatische Skalierung](../monitoring-and-diagnostics/monitoring-autoscale-scale-by-custom-metric.md) und Metrikwarnungen verwendet werden können.

### <a name="dependencies"></a>Abhängigkeiten
Um verschiedene logische Vorgänge einer Anwendung zu überwachen, müssen Sie [mehrere Komponenten übergreifend Telemetriedaten sammeln](../application-insights/app-insights-transaction-diagnostics.md). Application Insights unterstützt die [verteilte Telemetriekorrelation](../application-insights/application-insights-correlation.md), die die Abhängigkeiten zwischen Komponenten identifiziert, sodass Sie sie zusammen analysieren können.

### <a name="availability-tests"></a>Verfügbarkeitstests
[Verfügbarkeitstests](../application-insights/app-insights-monitor-web-app-availability.md) in Application Insights ermöglichen Ihnen, Verfügbarkeit und Reaktionsfähigkeit der Anwendung von unterschiedlichen Standorten im öffentlichen Internet aus zu testen. Sie können mit einem einfachen Pingtest sicherstellen, dass die Anwendung aktiv ist, oder mit Visual Studio einen Webtest erstellen, der ein Benutzerszenario simuliert.  Verfügbarkeitstests erfordern keine Instrumentierung in der Anwendung.

## <a name="custom-sources"></a>Benutzerdefinierte Quellen
Über die Standardebenen einer Anwendung hinaus müssen Sie möglicherweise weitere Ressourcen überwachen, deren Telemetrie nicht mit den anderen Datenquellen erfasst werden kann. Für diese Ressourcen müssen Sie diese Daten mithilfe einer Azure Monitor-API schreiben.

![Benutzerdefinierte Datensammlung](media/monitoring-data-sources/custom-collection.png)

### <a name="data-collector-api"></a>Data Collector API (Datensammler-API)
Azure Monitor kann mithilfe der [Datensammler-API](../log-analytics/log-analytics-data-collector-api.md) Protokolldaten von jedem REST-Client erfassen. Dies erlaubt Ihnen das Erstellen von benutzerdefinierten Überwachungsszenarien und das Ausweiten der Überwachung auf Ressourcen, die Telemetriedaten nicht durch andere Quellen verfügbar machen.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [Typen der von Azure Monitor gesammelten Überwachungsdaten](monitoring-data-collection.md) und wie diese Daten angezeigt und analysiert werden.
