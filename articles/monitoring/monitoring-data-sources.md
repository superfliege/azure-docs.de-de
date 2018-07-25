---
title: Quellen für Überwachungsdaten in Azure | Microsoft-Dokumentation
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
ms.date: 07/05/2018
ms.author: bwren
ms.openlocfilehash: 262099bbe45e483efd269445aa8042b30668ebe3
ms.sourcegitcommit: 04fc1781fe897ed1c21765865b73f941287e222f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2018
ms.locfileid: "39036522"
---
# <a name="sources-of-monitoring-data-in-azure"></a>Quellen für Überwachungsdaten in Azure
In diesem Artikel werden die verfügbaren Daten zum Überwachen von Integrität und Leistung Ihrer Azure-Ressourcen und der darauf ausgeführten Anwendungen beschrieben.  Sammeln und Analysieren dieser Daten mit den in [Sammeln von Überwachungsdaten in Azure](monitoring-data-collection.md) beschriebenen Tools

Zur Überwachung von Daten in Azure werden zahlreiche Quellen genutzt, die in Ebenen organisiert werden können, wobei die höchste Ebene Ihre Anwendung und die niedrigsten Ebene die Azure-Plattform ist. Dies wird im folgenden Diagramm veranschaulicht, und jede Ebene wird in den folgenden Abschnitten ausführlich beschrieben.

![Überwachungsdatenebenen](media/monitoring-data-sources/monitoring-tiers.png)


## <a name="azure-platform"></a>Azure Platform
Telemetriedaten zur Integrität und zum Betrieb von Azure selbst beinhalten Daten zum Betrieb und zur Verwaltung eines Azure-Abonnements oder -Mandanten. Dies beinhaltet Service Health-Daten, die im Azure-Aktivitätsprotokoll und in den Überwachungsprotokollen von Azure Active Directory gespeichert werden.

![Azure-Sammlung](media/monitoring-data-sources/azure-collection.png)

### <a name="azure-service-health"></a>Azure Service Health
[Azure Service Health](../monitoring-and-diagnostics/monitoring-service-notifications.md) enthält Informationen zur Integrität der Azure-Dienste in Ihrem Abonnement, von denen Ihre Anwendung und Ressourcen abhängen. Sie können Warnungen erstellen, um über aktuelle und erwartete kritische Probleme benachrichtigt zu werden, die sich möglicherweise auf Ihre Anwendung auswirken. Service Health-Datensätze werden im [Azure-Aktivitätsprotokoll](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) gespeichert, sodass Sie sie im Activity Log Explorer anzeigen und in Log Analytics kopieren können.

### <a name="azure-activity-log"></a>Azure-Aktivitätsprotokoll
Das [Azure-Aktivitätsprotokoll](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) enthält Service Health-Datensätze sowie Datensätze zu jeglichen Konfigurationsänderungen, die an Ihren Azure-Ressourcen vorgenommen wurden. Das Aktivitätsprotokoll ist für alle Azure-Ressourcen verfügbar und stellt ihre _externe_ Ansicht dar. Die spezifischen Typen von Datensätzen im Aktivitätsprotokoll werden im [Ereignisschema des Azure-Aktivitätsprotokolls](../monitoring-and-diagnostics/monitoring-activity-log-schema.md) beschrieben.

Sie können das Aktivitätsprotokoll für eine bestimmte Ressource auf deren Seite im Azure-Portal anzeigen oder Protokolle mehrerer Ressourcen im [Activity Log Explorer](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) anzeigen. Es ist besonders nützlich, die Protokolleinträge in Log Analytics zu kopieren, um sie mit anderen Überwachungsdaten zu kombinieren. Sie können sie auch mithilfe von [Event Hubs](../monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs.md) an andere Speicherorte senden.


### <a name="azure-active-directory-audit-logs"></a>Azure Active Directory-Überwachungsprotokolle
[Azure Active Directory-Berichterstellung](../active-directory/active-directory-reporting-azure-portal.md) beinhaltet den Verlauf der Anmeldeaktivität und den Überwachungspfad von Änderungen, die innerhalb eines bestimmten Mandanten vorgenommen wurden. Sie können Azure Active Directory-Überwachungsdaten derzeit nicht mit anderen Überwachungsdaten kombinieren, da nur über Azure Active Directory und die [Azure Active Directory-Berichterstellungs-API](../active-directory/active-directory-reporting-api-getting-started-azure-portal.md) darauf zugegriffen werden kann.


## <a name="azure-services"></a>Azure-Dienste
Diagnoseprotokolle auf Metriken- und Ressourcenebene enthalten Informationen zum _internen_ Betrieb der Azure-Ressourcen. Diese sind für die meisten Azure-Dienste verfügbar, und Verwaltungslösungen bieten zusätzliche Einblicke in bestimmte Dienste.

![Azure-Ressourcensammlung](media/monitoring-data-sources/azure-resource-collection.png)


### <a name="metrics"></a>Metriken
Die meisten Azure-Dienste generieren Metriken, die ihre Leistung und ihren Betrieb widerspiegeln. Die spezifischen [Metriken variieren je nach Ressourcentyp](../monitoring-and-diagnostics/monitoring-supported-metrics.md).  Im Metrik-Explorer kann auf sie zugegriffen werden, und sie können für Trend- und sonstige Analysen in Log Analytics kopiert werden.


### <a name="resource-diagnostic-logs"></a>Ressourcendiagnoseprotokolle
Während das Aktivitätsprotokoll Informationen über die Vorgänge liefert, die auf Azure-Ressourcen ausgeführt werden, bieten [Diagnoseprotokolle](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) auf Ressourcenebene Einblicke in den Betrieb der Ressource selbst.   Konfigurationsanforderungen und Inhalt dieser Protokolle [variieren je nach Ressourcentyp](../monitoring-and-diagnostics/monitoring-diagnostic-logs-schema.md).

Sie können Diagnoseprotokolle nicht direkt im Azure-Portal anzeigen, aber Sie können [sie zum Archivieren an den Azure-Speicher senden](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md) und für die Umleitung zu anderen Diensten nach [Event Hub](../event-hubs/event-hubs-what-is-event-hubs.md) bzw. zur Analyse nach [Log Analytics](../monitoring-and-diagnostics/monitor-stream-diagnostic-logs-log-analytics.md) exportieren. Einige Ressourcen können direkt in Log Analytics schreiben, während andere in ein Speicherkonto schreiben, bevor sie [in Log Analytics importiert](../log-analytics/log-analytics-azure-storage-iis-table.md#use-the-azure-portal-to-collect-logs-from-azure-storage) werden.

### <a name="management-solutions"></a>Verwaltungslösungen
 [Verwaltungslösungen](../monitoring/monitoring-solutions.md) sammeln Daten, um zusätzliche Erkenntnisse zum Betrieb eines bestimmten Diensts zu liefern. Sie sammeln Daten in Log Analytics, wo sie mit der [Abfragesprache](../log-analytics/log-analytics-log-search.md) oder Sichten, die in der Regel in der Lösung enthalten sind, analysiert werden können.

## <a name="guest-operating-system"></a>Gastbetriebssystem
Zusätzlich zu den Telemetriedaten, die von allen Azure-Diensten generiert werden, haben Computeressourcen ein Gastbetriebssystem, das überwacht werden muss. Mit der Installation mindestens eines Agents können Sie Telemetriedaten aus dem Gastbetriebssystem in den gleichen Überwachungstools sammeln, die sie für die Azure-Dienste selbst verwenden.

![Azure-Computeressourcensammlung](media/monitoring-data-sources/compute-resource-collection.png)

### <a name="diagnostic-extension"></a>Diagnoseerweiterung
Mit der [Azure-Diagnoseerweiterung](../monitoring-and-diagnostics/azure-diagnostics.md) können Sie Protokolle und Leistungsdaten des Clientbetriebssystems der Azure-Computeressourcen sammeln. Sowohl auf den Clients gesammelte Metriken als auch Protokolle werden in einem Azure-Speicherkonto gespeichert, sodass Sie [Log Analytics konfigurieren können, daraus zu importieren](../log-analytics/log-analytics-azure-storage-iis-table.md#use-the-azure-portal-to-collect-logs-from-azure-storage).  Der Metrik-Explorer kann aus dem Speicherkonto lesen und enthält die Clientmetriken zusammen mit anderen gesammelten Metriken.


### <a name="log-analytics-agent"></a>Log Analytics-Agent
Sie können den Log Analytics-Agent auf jeder Windows- oder Linux-VM bzw. jedem physischen Computer installieren. Der virtuelle Computer kann in Azure, einer anderen Cloud oder lokal ausgeführt werden.  Der Agent stellt entweder direkt oder über eine [verbundene System Center Operations Manager-Verwaltungsgruppe](../log-analytics/log-analytics-om-agents.md) eine Verbindung mit Log Analytics her und ermöglicht Ihnen, Daten aus [Datenquellen](../log-analytics/log-analytics-data-sources.md) zu sammeln, die Sie konfigurieren, oder aus [Verwaltungslösungen](../monitoring/monitoring-solutions.md), die zusätzliche Einblicke in Anwendungen ermöglichen, die auf dem virtuellen Computer ausgeführt werden.

### <a name="service-map"></a>Dienstzuordnung
[Service Map](../operations-management-suite/operations-management-suite-service-map.md) erfordert einen Dependency-Agent für Windows- und Linux-VMs. So lassen sich mit dem Log Analytics-Agent Daten über Prozesse sammeln, die auf dem virtuellen Computer ausgeführten werden, und über Abhängigkeiten von externen Prozessen. Er speichert diese Daten in Log Analytics und enthält eine Konsole, die die gesammelten Daten zusätzlich zu anderen in Log Analytics gespeicherten Daten anzeigt.

## <a name="applications"></a>ANWENDUNGEN
Zusätzlich zu den Telemetriedaten, die Ihre Anwendung möglicherweise in das Gastbetriebssystem schreibt, erfolgt eine ausführliche Anwendungsüberwachung mit [Application Insights](https://docs.microsoft.com/azure/application-insights/). Application Insights kann Daten aus Anwendungen sammeln, die auf einer Vielzahl von Plattformen ausgeführt werden. Die Anwendung kann in Azure, einer anderen Cloud oder lokal ausgeführt werden.

![Anwendungsdatensammlung](media/monitoring-data-sources/application-collection.png)


#### <a name="application-data"></a>Anwendungsdaten
Wenn Sie Application Insights durch Installation eines Instrumentierungspakets für eine Anwendung aktivieren, werden auf Leistung und Betrieb der Anwendung bezogene Metriken und Protokolle gesammelt. Dies umfasst detaillierte Informationen zu Seitenansichten, Anwendungsanforderungen und Ausnahmen. Application Insights speichert die gesammelten Daten in Azure-Metriken und Log Analytics. Es enthält umfassende Tools zum Analysieren der Daten, aber Sie können sie auch mithilfe von Tools wie dem Metrik-Explorer und Protokollsuchen mit Daten aus anderen Quellen analysieren.

Sie können mit Application Insights auch [eine benutzerdefinierte Metrik erstellen](../application-insights/app-insights-api-custom-events-metrics.md).  So können Sie Ihre eigene Logik zum Berechnen eines numerischen Werts und anschließendem Speichern dieses Werts mit anderen Metriken definieren, auf die mit dem Metrik-Explorer zugegriffen werden kann und die für [automatische Skalierung](../monitoring-and-diagnostics/monitoring-autoscale-scale-by-custom-metric.md) und Metrikwarnungen verwendet werden können.

#### <a name="dependencies"></a>Abhängigkeiten
Um verschiedene logische Vorgänge einer Anwendung zu überwachen, müssen Sie [mehrere Komponenten übergreifend Telemetriedaten sammeln](../application-insights/app-insights-transaction-diagnostics.md). Application Insights unterstützt die [verteilte Telemetriekorrelation](../application-insights/application-insights-correlation.md), die die Abhängigkeiten zwischen Komponenten identifiziert, sodass Sie sie zusammen analysieren können.

#### <a name="availability-tests"></a>Verfügbarkeitstests
[Verfügbarkeitstests](../application-insights/app-insights-monitor-web-app-availability.md) in Application Insights ermöglichen Ihnen, Verfügbarkeit und Reaktionsfähigkeit der Anwendung von unterschiedlichen Standorten im öffentlichen Internet aus zu testen. Sie können mit einem einfachen Pingtest sicherstellen, dass die Anwendung aktiv ist, oder mit Visual Studio einen Webtest erstellen, der ein Benutzerszenario simuliert.  Verfügbarkeitstests erfordern keine Instrumentierung in der Anwendung.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [Typen von Überwachungsdaten und die Azure-Tools](monitoring-data-collection.md), mit denen sie gesammelt und analysiert werden.
