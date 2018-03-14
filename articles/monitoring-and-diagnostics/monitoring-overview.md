---
title: "Überwachen von Azure-Anwendungen und -Ressourcen | Microsoft-Dokumentation"
description: "Übersicht über die Microsoft-Dienste und -Funktionen, die zu einer vollständigen Überwachungsstrategie für Ihre Azure-Dienste und -Anwendungen beitragen."
author: robb
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 1b962c74-8d36-4778-b816-a893f738f92d
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2018
ms.author: robb,bwren
ms.openlocfilehash: d8da175a551f7c589c313b2289b2a0209dbd2b56
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2018
---
# <a name="monitoring-azure-applications-and-resources"></a>Überwachen von Azure-Anwendungen und -Ressourcen

Unter Überwachung versteht man das Erfassen und Analysieren von Daten, um die Leistung, Integrität und Verfügbarkeit Ihrer Geschäftsanwendung und der Ressourcen, von denen sie abhängt, zu bestimmen. Eine effektive Überwachungsstrategie hilft Ihnen, den detaillierten Einsatz der verschiedenen Komponenten Ihrer Anwendung zu verstehen. Sie hilft Ihnen auch, die Betriebszeit zu erhöhen, da Sie proaktiv über kritische Probleme benachrichtigt werden und diese beheben können, bevor sie auftreten.

Azure umfasst mehrere Dienste, die eine bestimmte Rolle oder Aufgabe im Überwachungsbereich ausführen. Zusammen bieten diese Dienste eine umfassende Lösung für das Sammeln, Übertragen und Analysieren von Telemetriedaten aus Ihrer Anwendung und den zugrunde liegenden Azure-Ressourcen, die sie unterstützen. Sie können auch kritische lokale Ressourcen überwachen und so eine hybride Überwachungsumgebung bilden. Voraussetzung der Entwicklung einer vollständigen Überwachungsstrategie für Ihre Anwendung ist die Kenntnis der verfügbaren Tools und Daten. 

Das folgende Diagramm zeigt eine konzeptionelle Ansicht der Komponenten, die bei der Überwachung der Azure-Ressourcen zusammenwirken. Diese Komponenten werden in den folgenden Abschnitten mit Links zu ausführlichen technischen Informationen beschrieben.

![Übersicht zur Überwachung](media/monitoring-overview/overview.png)

## <a name="basic-monitoring"></a>Grundlegende Überwachung
Unter der grundlegenden Überwachung versteht man die elementare, erforderliche Überwachung aller Azure-Ressourcen. Diese Dienste benötigen nur eine minimale Konfiguration und sammeln die von den Premium-Überwachungsdiensten genutzten Telemetriedaten.    

### <a name="azure-monitor"></a>Azure Monitor
[Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md) ermöglicht dem Azure-Dienst die grundlegende Überwachung durch die Sammlung von [Metriken](../monitoring-and-diagnostics/monitoring-overview-metrics.md), [Aktivitätsprotokollen](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) und [Diagnoseprotokollen](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md). Beispielsweise können Sie dem Aktivitätsprotokoll entnehmen, wann neue Ressourcen erstellt oder geändert werden. 

Metriken stehen zur Verfügung und liefern Leistungsstatistiken für verschiedene Ressourcen und sogar das Betriebssystem der virtuellen Computer. Sie können diese Daten mit einem der Explorer im Azure-Portal anzeigen, zur Trend- und Detailanalyse an Azure Log Analytics senden oder Warnungsregeln erstellen, damit Sie proaktiv über kritische Probleme informiert werden.

### <a name="service-health"></a>Dienstintegrität
Die Integrität Ihrer Anwendung basiert auf den Azure-Diensten, von denen sie abhängt. [Azure Service Health](../service-health/service-health-overview.md) identifiziert Probleme mit Azure-Diensten, die sich auf Ihre Anwendung auswirken können. Service Health unterstützt Sie außerdem bei der geplanten Wartung.

### <a name="azure-advisor"></a>Azure Advisor
[Azure Advisor](../advisor/advisor-overview.md) überwacht ständig Ihre Ressourcenkonfiguration und Nutzungstelemetriedaten. Sie erhalten personalisierte Empfehlungen auf der Basis von Best Practices. Wenn Sie diese Empfehlungen befolgen, verbessern Sie Leistung, Sicherheit und Verfügbarkeit der Ressourcen, die Ihre Anwendungen unterstützen.


## <a name="premium-monitoring-services"></a>Premium-Überwachungsdienste
Die folgenden Azure-Dienste bieten umfassende Funktionen für das Sammeln und Analysieren von Überwachungsdaten. Diese Dienste bauen auf der grundlegenden Überwachung auf und nutzen allgemeine Funktionen in Azure. Sie bieten leistungsstarke Analysen der gesammelten Daten und somit einzigartige Einblicke Ihre Anwendungen und Infrastrukturen. Sie stellen Daten im Kontext von Szenarien dar, die auf verschiedene Zielgruppen ausgerichtet sind.

### <a name="application-insights"></a>Application Insights
Mit [Azure Application Insights](http://azure.microsoft.com/documentation/services/application-insights) können Sie die Verfügbarkeit, Leistung und Nutzung Ihrer in der Cloud oder lokal gehosteten Anwendung überwachen. 

Durch die Instrumentierung Ihrer Anwendung zur Zusammenarbeit mit Application Insights können Sie umfassende Einblicke erzielen. Sie können Fehler dann schnell erkennen und diagnostizieren, ohne darauf warten zu müssen, dass diese von Benutzern gemeldet werden. Mit den gesammelten Informationen können Sie fundierte Entscheidungen zu Wartung und Verbesserung Ihrer Anwendung vornehmen. 

Application Insights verfügt über umfassende Tools für die Interaktion mit den gesammelten Daten. Application Insights speichert die Daten in einem gemeinsamen Repository. Sie können gemeinsame Funktionen wie Warnungen, Dashboards und umfassende Analysen mit der Log Analytics-Abfragesprache nutzen.

### <a name="log-analytics"></a>Log Analytics
[Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics) spielt bei der Azure-Überwachung eine zentrale Rolle, indem Daten aus einer Vielzahl von Ressourcen gesammelt und in einem einzelnen Repository gespeichert werden. Dort können Sie die Daten mit einer leistungsfähigen Abfragesprache analysieren. 

Application Insights und Azure Security Center speichern Daten im Log Analytics-Datenspeicher und nutzen das zugehörige Analysemodul. Daten werden von Azure Monitor, Verwaltungslösungen und auf den virtuellen Computern in der Cloud oder lokal installierten Agents gesammelt. Dadurch erhalten Sie ein vollständiges Bild von Ihrer gesamten Umgebung. 


### <a name="service-map"></a>Dienstzuordnung
[Dienstzuordnung](../operations-management-suite/operations-management-suite-service-map.md) bietet durch die Analyse der virtuellen Computer mit ihren verschiedenen Prozessen und Abhängigkeiten von anderen Computern und externen Prozessen einen Einblick in Ihre IaaS-Umgebung. Die Anwendung integriert Ereignisse, Leistungsdaten und Verwaltungslösungen in Log Analytics. Sie können diese Daten dann im Kontext der einzelnen Computer und deren Beziehung zum Rest Ihrer Umgebung anzeigen. 

Die Dienstzuordnung weist Ähnlichkeiten zur [Anwendungszuordnung in Application Insights](../application-insights/app-insights-app-map.md) auf. Ihr Schwerpunkt liegt auf den Infrastrukturkomponenten, die Ihre Anwendungen unterstützen.

### <a name="network-watcher"></a>Network Watcher
[Network Watcher](../network-watcher/network-watcher-monitoring-overview.md) bietet eine szenariobasierte Überwachung und Diagnose für verschiedene Netzwerkszenarien in Azure. Der Dienst speichert Daten zur weiteren Analyse in Azure-Metriken. Mit den folgenden Netzwerküberwachungslösungen bietet er eine umfassende Überwachung verschiedener Aspekte Ihres Netzwerks:
* [Netzwerkleistungsmonitor (Network Performance Monitor, NPM)](https://blogs.msdn.microsoft.com/azuregov/2017/09/05/network-performance-monitor-general-availability/): Eine cloudbasierte Lösung für die Netzwerküberwachung, die die Konnektivität zwischen öffentlichen Clouds, Rechenzentren und lokalen Umgebungen überwacht.
* [ExpressRoute Monitor](https://azure.microsoft.com/en-in/blog/monitoring-of-azure-expressroute-in-preview/): Eine NPM-Funktion, die die gesamte Konnektivität und Leistung von Azure ExpressRoute-Verbindungen überwacht.
* Traffic Analytics: Eine cloudbasierte Lösung, die Einblick in Benutzer- und Anwendungsaktivitäten in Ihrem Cloudnetzwerk bietet.
* [DNS-Analyse](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-dns): Bietet Sicherheit, Leistung und vorgangsbezogene Einblicke basierend auf Ihren DNS-Servern.

### <a name="management-solutions"></a>Verwaltungslösungen
[Verwaltungslösungen](../log-analytics/log-analytics-add-solutions.md) sind Programmpakete, die Einblicke für eine bestimmte Anwendung oder einen bestimmten Dienst liefern. Sie nutzen Log Analytics zum Speichern und Analysieren der von ihnen gesammelten Überwachungsdaten. 

Microsoft und Partner bieten Verwaltungslösungen zur Überwachung verschiedener Azure- und Drittanbieterdienste. Beispiele für Überwachungslösungen:
* [Containerüberwachung](../log-analytics/log-analytics-containers.md) hilft Ihnen beim Anzeigen und Verwalten der Containerhosts.
* [Azure SQL-Analyse](../log-analytics/log-analytics-azure-sql.md) erfasst und visualisiert Leistungsmetriken für Azure SQL-Datenbanken.


## <a name="shared-functionality"></a>Gemeinsam genutzte Funktionalität
Die folgenden Azure-Tools bieten wichtige Funktionen für die Premium-Überwachungsdienste. Sie werden von mehreren Diensten gemeinsam genutzt. Sie können also allgemeine Funktionen und Konfigurationen für mehrere Dienste nutzen.

### <a name="alerts"></a>Alerts
[Azure-Warnungen](../monitoring-and-diagnostics/monitoring-overview-alerts.md) informieren Sie proaktiv über kritische Zustände und leiten potenziell Korrekturmaßnahmen ein. Warnungsregeln können Daten aus mehreren Quellen einschließlich Metriken und Protokollen nutzen. Sie verwenden [Aktionsgruppen](../monitoring-and-diagnostics/monitoring-action-groups.md), die eindeutige Sätze von Empfängern und Aktionen als Reaktion auf eine Warnung enthalten. Je nach Ihren Anforderungen können Warnungen externe Aktionen mithilfe von Webhooks starten und in Ihre ITSM-Tools integrieren.

### <a name="dashboards"></a>Dashboards
Mit [Azure Dashboards](../azure-portal/azure-portal-dashboards.md) können Sie verschiedene Arten von Daten zentral in einem Bereich im Azure-Portal kombinieren. Sie können das Dashboard gemeinsam mit anderen Azure-Benutzern nutzen. 

Sie können z.B. ein Dashboard erstellen, das Folgendes kombiniert:
- Kacheln, die ein Diagramm der Metriken zeigen
- eine Tabelle mit Aktivitätsprotokollen
- ein Nutzungsdiagramm von Application Insights
- die Ausgabe einer Protokollsuche in Log Analytics

Sie können auch Log Analytics-Daten nach [Power BI](https://docs.microsoft.com/power-bi/) exportieren. Dort können Sie von weiteren Visualisierungen profitieren. Sie können die Daten auch anderen Personen innerhalb und außerhalb Ihrer Organisation zur Verfügung stellen.

### <a name="metrics-explorer"></a>Metrik-Explorer
[Metriken](../monitoring-and-diagnostics/monitoring-overview-metrics.md) sind von einer Azure-Ressource generierte numerische Werte, die Ihnen helfen, Betrieb und Leistung der Ressource zu verstehen. Mit dem Metrik-Explorer können Sie Metriken für die Analyse mit Daten aus anderen Quellen an Log Analytics senden.



### <a name="activity-log"></a>Aktivitätsprotokoll
[Aktivitätsprotokolle](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) enthalten Daten zum Betrieb einer Azure-Ressource. Folgende Informationen sind vorhanden:
- Konfigurationsänderungen an der Ressource
- Dienstintegritätsvorfälle
- Empfehlungen zur besseren Nutzung der Ressource
- Informationen zu automatischen Skalierungsvorgängen 

Sie können Protokolle für eine bestimmte Ressource auf der zugehörigen Seite im Azure-Portal anzeigen. Zudem können Sie Protokolle mehrerer Ressourcen im Aktivitätsprotokoll-Explorer anzeigen. 

Außerdem können Sie Aktivitätsprotokolle auch an Log Analytics senden. Dort können Sie die Protokolle mithilfe von Daten analysieren, die von Verwaltungslösungen, Agents auf virtuellen Computern und anderen Quellen gesammelt wurden.


## <a name="example-scenarios"></a>Beispielszenarien
Die folgenden allgemeinen Beispiele zeigen, wie Sie verschiedene Überwachungstools in Azure für verschiedene Szenarien nutzen können.

### <a name="monitoring-a-web-application"></a>Überwachen einer Webanwendung
Wir gehen von einer Webanwendung aus, die über Azure App Servics, Azure Storage und eine SQL-Datenbank in Azure bereitgestellt wird. Greifen Sie zuerst auf [Metriken](../monitoring-and-diagnostics/monitoring-overview-metrics.md) und [Aktivitätsprotokolle](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) für diese Ressourcen auf den zugehörigen Seiten im Azure-Portal zu. Hier finden Sie wichtige Informationen, z.B. die Anzahl der an die Anwendung gerichteten Anforderungen und die durchschnittliche Antwortzeit. Außerdem können Sie hier Konfigurationsänderungen ermitteln.

Fahren Sie dann im Portal mit der Überwachung fort, um die Metriken und Protokolle der Ressourcen anzuzeigen. Wenn Sie Standardparameter für die Metriken bestimmen, [erstellen Sie Warnungsregeln](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md). Diese Regeln benachrichtigt Sie proaktiv, wenn z.B. die durchschnittliche Antwortzeit einen Schwellenwert überschreitet. Um eine kurze Übersicht über die tägliche Leistung Ihrer Anwendung zu erhalten, erstellen Sie ein Azure-Dashboard zum Anzeigen von Diagrammen von Metriken, die wichtige KPIs darstellen.

Wenn Sie eine umfassendere Überwachung Ihrer Anwendung wünschen, [konfigurieren Sie sie für Application Insights](../application-insights/quick-monitor-portal.md). Sie können jetzt zusätzliche Daten sammeln, die weitere Einblicke in Betrieb und Leistung Ihrer Anwendung liefern. Application Insights erkennt die zugrunde liegenden Beziehungen zwischen den Komponenten Ihrer App. Das ermöglicht die visuelle Darstellung über die [Anwendungszuordnung ](../application-insights/app-insights-app-map.md) gekoppelt mit [End-to-End-Ablaufverfolgung](../application-insights/app-insights-transaction-diagnostics.md), um die genaue Komponente, Abhängigkeit oder Ausnahme zu diagnostizieren, in der ein Problem aufgetreten ist. 

Sie erstellen [Verfügbarkeitstests](../application-insights/app-insights-monitor-web-app-availability.md), um proaktiv Ihre Anwendung von unterschiedlichen Regionen aus zu testen. Um Ihre Entwickler zu unterstützen, [aktivieren Sie den Profiler](../application-insights/enable-profiler-compute.md), damit Sie Anforderungen und Ausnahmen bis zu einer bestimmten Codezeile verfolgen können. Um weiteren Einblick in Dienste zu erhalten, die in Ihrer Anwendung verwendet werden, fügen Sie die [SQL Analytics-Lösung](../log-analytics/log-analytics-azure-sql.md) zum Sammeln zusätzlicher Daten in Log Analytics hinzu. 

Nach einiger Zeit möchten Sie herausfinden, warum in bestimmten Zeiträumen die Leistung auf der Website unter den Schwellenwert gefallen ist. Sie schreiben mit Log Analytics eine Abfrage. Diese hilft Ihnen beim Korrelieren der von Application Insights gesammelten Nutzungs- und Leistungsdaten mit Konfigurations- und Leistungsdaten der Azure-Ressourcen, die Ihre Anwendung unterstützen.


### <a name="monitoring-virtual-machines"></a>Überwachung virtueller Computer
Sie führen in Azure sowohl virtuelle Windows- als auch Linux-Computer aus. Mit Azure Monitor können Sie [Aktivitätsprotokolle](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) und [Metriken auf Hostebene](../monitoring-and-diagnostics/monitoring-overview-metrics.md) anzeigen. Fügen Sie den virtuellen Computern die [Azure-Diagnoseerweiterung](../virtual-machines/linux/tutorial-monitoring.md#install-diagnostics-extension) hinzu, um Metriken auf dem Gastbetriebssystem zu sammeln. Erstellen Sie dann [Warnungsregeln](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md), die Sie proaktiv benachrichtigen, wenn Standardmetriken wie Prozessorauslastung und Arbeitsspeicher Schwellenwerte überschreiten.

Um ausführlichere Informationen zu virtuellen Computern mit einer Geschäftsanwendung zu sammeln, [erstellen Sie einen Log Analytics-Arbeitsbereich und aktivieren die VM-Erweiterung](../log-analytics/log-analytics-quick-collect-azurevm.md) auf jedem Computer. Wenn Sie die [Sammlung aus verschiedenen Datenquellen](../log-analytics/log-analytics-data-sources.md) für Ihre Anwendung konfigurieren und [Ansichten erstellen](../log-analytics/log-analytics-view-designer.md), erhalten Sie Berichte zum tägliche Betrieb und zur täglichen Leistung. Dann [erstellen Sie Warnungsregeln](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md), die Sie informieren, wenn bestimmte Fehlerereignisse festgestellt werden. 

Fügen Sie die [Agent-Integritätsverwaltungslösung](../operations-management-suite/oms-solution-agenthealth.md) hinzu, um die Integrität des installierten Agents fortlaufend zu überwachen. Wenn Sie weitere Einblicke in die Anwendung wünschen, [fügen Sie den Dependency-Agent](../operations-management-suite/operations-management-suite-service-map-configure.md) den virtuellen Computern hinzu, um diese der [Dienstzuordnung](../operations-management-suite/operations-management-suite-service-map.md) hinzuzufügen. Die Dienstzuordnung ermittelt kritische Prozesse und identifiziert Verbindungen zwischen Computern mit anderen Diensten. 

Nach einem gemeldeten Ausfall identifizieren Sie mit Hilfe der Dienstzuordnung die Computer, bei denen das Problem aufgetreten ist. Dann erstellen Sie eine [Abfrage für die Log Analytics-Daten](../log-analytics/log-analytics-log-search-new.md), um das Problem in Zukunft zu beheben. Zudem erstellen Sie eine Warnungsregel, die Sie proaktiv benachrichtigt, wenn die Bedingung erkannt wird.



## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen:

* [Azure Monitor in einem Video von der Ignite 2016](https://myignite.microsoft.com/videos/4977)
* [Erste Schritte mit Azure Monitor](monitoring-get-started.md)
* [Azure-Diagnose](../azure-diagnostics.md) diagnostiziert Probleme mit Clouddiensten, virtuellen Computern, VM-Skalierungsgruppen oder Azure Service Fabric-Anwendungen.
* [Application Insights](https://azure.microsoft.com/documentation/services/application-insights/) diagnostiziert Probleme mit Ihrer App Service-Web-App.
* [Log Analytics](https://azure.microsoft.com/documentation/services/log-analytics/) zur Analyse der gesammelten Überwachungsdaten.
