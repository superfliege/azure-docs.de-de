---
title: Quellen für Daten in Azure Monitor| Microsoft-Dokumentation
description: Beschreibt die verfügbaren Daten zum Überwachen von Integrität und Leistung Ihrer Azure-Ressourcen und der darauf ausgeführten Anwendungen.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/13/2018
ms.author: bwren
ms.openlocfilehash: 4ae8ae43d8c245270b64611bed0ac4930aa0ecc3
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2019
ms.locfileid: "57763420"
---
# <a name="sources-of-data-in-azure-monitor"></a>Quellen für Daten in Azure Monitor
In diesem Artikel werden die Quellen von Daten beschrieben, die von Azure Monitor zum Überwachen von Integrität und Leistung Ihrer Azure-Ressourcen und der darauf ausgeführten Anwendungen gesammelt werden. Diese Ressourcen können in Azure, in einer anderen Cloud oder lokal ausgeführt werden.  Details dazu, wie diese Daten gespeichert werden und wie Sie sie anzeigen können, finden Sie unter [Von Azure Monitor gesammelte Daten](data-collection.md).

Zur Überwachung von Daten in Azure werden zahlreiche Quellen genutzt, die in Ebenen organisiert werden können, wobei die höchste Ebene Ihre Anwendung und eventuelle Betriebssysteme und die niedrigste Ebene die Komponenten der Azure-Plattform darstellen. Dies wird im folgenden Diagramm veranschaulicht, und jede Ebene wird in den folgenden Abschnitten ausführlich beschrieben.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

![Überwachungsdatenebenen](media/data-sources/monitoring-tiers.png)

## <a name="azure-tenant"></a>Azure-Mandant
Die auf Ihren Azure-Mandanten bezogenen Telemetriedaten werden von mandantenweiten Diensten gesammelt, wie z.B. Azure Active Directory.

![Azure-Mandantensammlung](media/data-sources/tenant-collection.png)

### <a name="azure-active-directory-audit-logs"></a>Azure Active Directory-Überwachungsprotokolle
[Azure Active Directory-Berichterstellung](../../active-directory/reports-monitoring/overview-reports.md) beinhaltet den Verlauf der Anmeldeaktivität und den Überwachungspfad von Änderungen, die innerhalb eines bestimmten Mandanten vorgenommen wurden. Diese Überwachungsprotokolle können zur Analyse mit anderen Protokolldaten in Azure Monitor-Protokolle geschrieben werden.


## <a name="azure-platform"></a>Azure Platform
Telemetriedaten zur Integrität und zum Betrieb von Azure selbst beinhalten Daten zum Betrieb und zur Verwaltung eines Azure-Abonnements. Dies beinhaltet Service Health-Daten, die im Azure-Aktivitätsprotokoll und in den Überwachungsprotokollen von Azure Active Directory gespeichert werden.

![Azure-Abonnementsammlung](media/data-sources/azure-collection.png)

### <a name="azure-service-health"></a>Azure Service Health
[Azure Service Health](service-notifications.md) enthält Informationen zur Integrität der Azure-Dienste in Ihrem Abonnement, von denen Ihre Anwendung und Ressourcen abhängen. Sie können Warnungen erstellen, um über aktuelle und erwartete kritische Probleme benachrichtigt zu werden, die sich möglicherweise auf Ihre Anwendung auswirken. Service Health-Datensätze werden im [Azure-Aktivitätsprotokoll](activity-logs-overview.md) gespeichert, sodass Sie sie im Activity Log Explorer anzeigen und in Azure Monitor-Protokolle kopieren können.

### <a name="azure-activity-log"></a>Azure-Aktivitätsprotokoll
Das [Azure-Aktivitätsprotokoll](activity-logs-overview.md) enthält Service Health-Datensätze sowie Datensätze zu jeglichen Konfigurationsänderungen, die an Ihren Azure-Ressourcen vorgenommen wurden. Das Aktivitätsprotokoll ist für alle Azure-Ressourcen verfügbar und stellt ihre _externe_ Ansicht dar. Die spezifischen Typen von Datensätzen im Aktivitätsprotokoll werden im [Ereignisschema des Azure-Aktivitätsprotokolls](activity-log-schema.md) beschrieben.

Sie können das Aktivitätsprotokoll für eine bestimmte Ressource auf deren Seite im Azure-Portal anzeigen oder Protokolle mehrerer Ressourcen im [Activity Log Explorer](activity-logs-overview.md) anzeigen. Es ist besonders nützlich, die Protokolleinträge in Azure Monitor zu kopieren, um sie mit anderen Überwachungsdaten zu kombinieren. Sie können sie auch mithilfe von [Event Hubs](activity-logs-stream-event-hubs.md) an andere Speicherorte senden.



## <a name="azure-services"></a>Azure-Dienste
Diagnoseprotokolle auf Metriken- und Ressourcenebene enthalten Informationen zum _internen_ Betrieb der Azure-Ressourcen. Diese sind für die meisten Azure-Dienste verfügbar, und Verwaltungslösungen bieten zusätzliche Einblicke in bestimmte Dienste.

![Azure-Ressourcensammlung](media/data-sources/azure-resource-collection.png)


### <a name="metrics"></a>Metriken
Die meisten Azure-Dienste generieren [Plattformmetriken](data-collection.md#metrics), die ihre Leistung und ihren Betrieb widerspiegeln. Die spezifischen [Metriken variieren je nach Ressourcentyp](metrics-supported.md).  In Metrikanalysen kann auf sie zugegriffen werden, und sie können mithilfe von Log Analytics für Trend- und sonstige Analysen in Protokolle kopiert werden.


### <a name="resource-diagnostic-logs"></a>Ressourcendiagnoseprotokolle
Während das Aktivitätsprotokoll Informationen über die Vorgänge liefert, die auf Azure-Ressourcen ausgeführt werden, bieten [Diagnoseprotokolle](diagnostic-logs-overview.md) auf Ressourcenebene Einblicke in den Betrieb der Ressource selbst.   Konfigurationsanforderungen und Inhalt dieser Protokolle [variieren je nach Ressourcentyp](diagnostic-logs-schema.md).

Sie können Diagnoseprotokolle nicht direkt im Azure-Portal anzeigen, aber Sie können [sie zum Archivieren an den Azure-Speicher senden](archive-diagnostic-logs.md) und für die Umleitung zu anderen Diensten in [Event Hub](../../event-hubs/event-hubs-about.md) bzw. zur Analyse in [Azure Monitor](diagnostic-logs-stream-log-store.md) exportieren. Einige Ressourcen können direkt in Azure Monitor schreiben, während andere in ein Speicherkonto schreiben, bevor sie [in Log Analytics importiert](azure-storage-iis-table.md#use-the-azure-portal-to-collect-logs-from-azure-storage) werden.

### <a name="monitoring-solutions"></a>Überwachungslösungen
 [Überwachungslösungen](../../azure-monitor/insights/solutions.md) sammeln Daten, um zusätzliche Erkenntnisse zum Betrieb eines bestimmten Diensts oder einer bestimmten Anwendung zu liefern. Sie sammeln Daten in Azure Monitor-Protokollen, wo sie mit der [Abfragesprache](../../azure-monitor/log-query/log-query-overview.md) oder [Sichten](view-designer.md), die in der Regel in der Lösung enthalten sind, analysiert werden können.


## <a name="guest-operating-system"></a>Gastbetriebssystem
Computeressourcen in Azure, in anderen Clouds und lokal haben ein Gastbetriebssystem zu überwachen. Mit der Installation mindestens eines Agents können Sie Telemetriedaten aus dem Gastbetriebssystem in den gleichen Überwachungstools sammeln, die sie für die Azure-Dienste selbst verwenden.

![Azure-Computeressourcensammlung](media/data-sources/compute-resource-collection.png)

### <a name="azure-diagnostic-extension"></a>Azure-Diagnoseerweiterung
Mit der Azure-Diagnoseerweiterung erhalten Sie eine einfache Überwachungsebene, indem Protokolle und Leistungsdaten aus dem Clientbetriebssystem von Azure-Computeressourcen gesammelt werden.   

### <a name="log-analytics-agent"></a>Log Analytics-Agent
Der Log Analytics-Agent ermöglicht eine umfassende Überwachung und Verwaltung Ihrer virtuellen Windows- oder Linux-Computer bzw. der physischen Computer. Der virtuelle Computer kann in Azure, einer anderen Cloud oder lokal ausgeführt werden, und der Agent stellt entweder direkt oder über eine System Center Operations Manager-Instanz eine Verbindung mit Azure Monitor her. Auf diese Weise können Sie Daten aus von Ihnen konfigurierten [Datenquellen](agent-data-sources.md) oder aus [Überwachungslösungen](../../azure-monitor/insights/solutions.md) sammeln, die zusätzliche Einblicke in die Anwendungen auf dem virtuellen Computer ermöglichen.


### <a name="dependency-agent"></a>Abhängigkeits-Agent
Für die [Dienstzuordnung](../insights/service-map.md) und für [Azure Monitor für VMs](../../azure-monitor/insights/vminsights-overview.md) muss auf virtuellen Windows- und Linux-Computern ein Abhängigkeits-Agent vorhanden sein. Per Integration in den Log Analytics-Agent können ermittelte Daten über Prozesse, die auf dem virtuellen Computer ausgeführt werden, und über Abhängigkeiten von externen Prozessen gesammelt werden. Diese Daten werden in Azure Monitor gespeichert, und die ermittelten verbundenen Komponenten werden visualisiert.  

Weitere Informationen dazu, welche Unterschiede es zwischen den Agents gibt und welche je nach Ihren Überwachungsanforderungen verwendet werden sollten, finden Sie unter [Overview of the Azure monitoring agents](agents-overview.md) (Übersicht über die Azure-Überwachungs-Agents).

## <a name="applications"></a>ANWENDUNGEN
Zusätzlich zu den Telemetriedaten, die Ihre Anwendung möglicherweise in das Gastbetriebssystem schreibt, erfolgt eine ausführliche Anwendungsüberwachung mit [Application Insights](https://docs.microsoft.com/azure/application-insights/). Application Insights kann Daten aus Anwendungen sammeln, die auf einer Vielzahl von Plattformen ausgeführt werden. Die Anwendung kann in Azure, einer anderen Cloud oder lokal ausgeführt werden.

![Anwendungsdatensammlung](media/data-sources/application-collection.png)


### <a name="application-data"></a>Anwendungsdaten
Wenn Sie Application Insights durch Installation eines Instrumentierungspakets für eine Anwendung aktivieren, werden auf Leistung und Betrieb der Anwendung bezogene Metriken und Protokolle gesammelt. Dies umfasst detaillierte Informationen zu Seitenansichten, Anwendungsanforderungen und Ausnahmen. Application Insights speichert die gesammelten Daten in Azure Monitor. Es enthält umfassende Tools zum Analysieren der Daten, aber Sie können sie auch mithilfe von Tools wie Metrikanalysen und Log Analytics mit Daten aus anderen Quellen analysieren.

Sie können mit Application Insights auch [eine benutzerdefinierte Metrik erstellen](../../application-insights/app-insights-api-custom-events-metrics.md).  So können Sie Ihre eigene Logik zum Berechnen eines numerischen Werts und anschließendem Speichern dieses Werts mit anderen Metriken definieren, auf die über Metrikanalysen zugegriffen werden kann und die für [automatische Skalierung](autoscale-custom-metric.md) und Metrikwarnungen verwendet werden können.


### <a name="dependencies"></a>Abhängigkeiten
Um verschiedene logische Vorgänge einer Anwendung zu überwachen, müssen Sie [mehrere Komponenten übergreifend Telemetriedaten sammeln](../../application-insights/app-insights-transaction-diagnostics.md). Application Insights unterstützt die [verteilte Telemetriekorrelation](../../application-insights/application-insights-correlation.md), die die Abhängigkeiten zwischen Komponenten identifiziert, sodass Sie sie zusammen analysieren können.

### <a name="availability-tests"></a>Verfügbarkeitstests
[Verfügbarkeitstests](../../application-insights/app-insights-monitor-web-app-availability.md) in Application Insights ermöglichen Ihnen, Verfügbarkeit und Reaktionsfähigkeit der Anwendung von unterschiedlichen Standorten im öffentlichen Internet aus zu testen. Sie können mit einem einfachen Pingtest sicherstellen, dass die Anwendung aktiv ist, oder mit Visual Studio einen Webtest erstellen, der ein Benutzerszenario simuliert.  Verfügbarkeitstests erfordern keine Instrumentierung in der Anwendung.

## <a name="custom-sources"></a>Benutzerdefinierte Quellen
Über die Standardebenen einer Anwendung hinaus müssen Sie möglicherweise weitere Ressourcen überwachen, deren Telemetrie nicht mit den anderen Datenquellen erfasst werden kann. Für diese Ressourcen müssen Sie diese Daten mithilfe einer Azure Monitor-API schreiben.

![Benutzerdefinierte Datensammlung](media/data-sources/custom-collection.png)

### <a name="data-collector-api"></a>Data Collector API (Datensammler-API)
Azure Monitor kann mithilfe der [Datensammler-API](../../azure-monitor/platform/data-collector-api.md) Protokolldaten von jedem REST-Client erfassen. Dies erlaubt Ihnen das Erstellen von benutzerdefinierten Überwachungsszenarien und das Ausweiten der Überwachung auf Ressourcen, die Telemetriedaten nicht durch andere Quellen verfügbar machen.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [Typen der von Azure Monitor gesammelten Überwachungsdaten](data-collection.md) und wie diese Daten angezeigt und analysiert werden.
