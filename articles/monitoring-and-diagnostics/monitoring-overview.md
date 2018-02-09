---
title: "Überwachen von Azure-Anwendungen und -Ressourcen | Microsoft-Dokumentation"
description: "Übersicht über die verschiedenen Microsoft-Dienste und -Funktionen, die zu einer vollständigen Überwachungsstrategie für Ihre Azure-Dienste und -Anwendungen beitragen."
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
ms.openlocfilehash: ffd9a6f75a549b246a04adc5480e988b1622c5ca
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2018
---
# <a name="monitoring-azure-applications-and-resources"></a>Überwachen von Azure-Anwendungen und -Ressourcen

Überwachung ist das Erfassen und Analysieren von Daten, um die Leistung, Integrität und Verfügbarkeit Ihrer Geschäftsanwendung und der Ressourcen, von denen sie abhängt, zu bestimmen. Eine effektive Überwachungsstrategie erleichtert Ihnen, den detaillierten Einsatz der verschiedenen Komponenten Ihrer Anwendung zu verstehen und die Betriebszeit zu erhöhen, indem Sie proaktiv über kritische Punkte informiert werden, bevor Probleme auftreten.

Azure umfasst mehrere Dienste, die einzeln eine bestimmte Rolle oder Aufgabe im Überwachungsbereich haben und gemeinsam eine umfassende Lösung für das Sammeln, Übertragen und Analysieren von Telemetriedaten aus Ihrer Anwendung und den zugrunde liegenden Azure-Ressourcen, die sie unterstützen, darstellen.  Sie können auch kritische lokale Ressourcen überwachen und so eine hybride Überwachungsumgebung bilden.   Voraussetzung der Entwicklung einer vollständigen Überwachungsstrategie für Ihre Anwendung ist die Kenntnis der verfügbaren Tools und Daten. 

Das folgende Diagramm zeigt eine konzeptionelle Ansicht der verschiedenen Komponenten, die bei der Überwachung der Azure-Ressourcen zusammenwirken.  Jede wird in den folgenden Abschnitten mit Links zu ausführlichen technischen Informationen beschrieben.

![Übersicht zur Überwachung](media/monitoring-overview/overview.png)

## <a name="basic-monitoring"></a>Grundlegende Überwachung
Grundlegende Überwachung ist die erforderliche, alle Azure-Ressourcen übergreifende Überwachung.  Diese Dienste benötigen nur eine minimale Konfiguration und sammeln Kerntelemetriedaten, die von den Premium-Überwachungsdiensten genutzt werden.    

### <a name="azure-monitor"></a>Azure Monitor
[Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md) ermöglicht dem Azure-Dienst grundlegende Überwachung durch die Sammlung von [Metriken](../monitoring-and-diagnostics/monitoring-overview-metrics.md), [Aktivitätsprotokollen](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) und [Diagnoseprotokollen](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).  Beispielsweise erfahren Sie durch das Aktivitätsprotokoll, wann neue Ressourcen erstellt oder geändert werden.  Metriken liefern Leistungsstatistiken für verschiedene Ressourcen und auch das Betriebssystem eines virtuellen Computers.  Sie können diese Daten mit einem der Explorer im Azure-Portal anzeigen, zur Trend- und Detailanalyse an Log Analytics senden oder Warnungsregeln erstellen, damit Sie proaktiv über kritische Punkte informiert werden.

### <a name="service-health"></a>Dienstintegrität
Die Integrität Ihrer Anwendung basiert auf den Azure-Diensten, von denen sie abhängt.  [Azure Service Health](../service-health/service-health-overview.md) identifiziert Probleme mit Azure-Diensten, die sich auf Ihre Anwendung auswirken können, und unterstützt Sie auch bei der Wartungsplanung.

### <a name="azure-advisor"></a>Azure Advisor
[Azure Advisor](../advisor/advisor-overview.md) überwacht ständig Ihre Ressourcenkonfiguration und Nutzungstelemetriedaten, um Ihnen individuelle Empfehlungen auf der Basis bewährter Methoden zu bieten.  Wenn Sie diese Empfehlungen befolgen, verbessern Sie Leistung, Sicherheit und Verfügbarkeit der Ressourcen, die Ihre Anwendungen unterstützen.


## <a name="premium-monitoring-services"></a>Premium-Überwachungsdienste
Die folgenden Azure-Dienste bieten umfassende Funktionen für das Sammeln und Analysieren von Überwachungsdaten.  Sie basieren auf grundlegender Überwachung, nutzen die Grundfunktionen in Azure und ermöglichen leistungsstarke Analysen mit gesammelten Daten, damit Sie einzigartige Einblicke in Ihre Anwendungen und Infrastruktur erhalten.  Sie stellen Daten zielgruppenorientiert im Kontext der jeweiligen Szenarien dar.

### <a name="application-insights"></a>Application Insights
[Application Insights](http://azure.microsoft.com/documentation/services/application-insights) ermöglicht das Überwachen von Verfügbarkeit, Leistung und Nutzung Ihrer in der Cloud oder lokal gehosteten Anwendung.  Mittels Instrumentierung der Anwendung zur Zusammenarbeit mit Application Insights können Sie tiefere Einblicke erreichen, anhand derer Sie Fehler schnell identifizieren und diagnostizieren können, ohne auf den Bericht eines Benutzers warten zu müssen. Mit den gesammelten Informationen können Sie fundierte Entscheidungen zu Wartung und Verbesserung Ihrer Anwendung vornehmen.  Zusätzlich zu den umfassenden Tools für die Interaktion mit den gesammelten Daten speichert Application Insights diese Daten in einem gemeinsamen Repository, um Funktionen wie Warnungen, Dashboards und tiefgehende Analysen gemeinsam mit der Log Analytics-Abfragesprache zu nutzen.

### <a name="log-analytics"></a>Log Analytics
[Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics) spielt bei der Azure-Überwachung eine zentrale Rolle, indem Daten aus einer Vielzahl von Ressourcen gesammelt und in einem einzelnen Repository gespeichert werden, wo sie mit einer leistungsstarken Abfragesprache analysiert werden können.  Application Insights und Azure Security Center speichern ihre Daten im Log Analytics-Datenspeicher und nutzen dessen Analysemodul.  Dies ermöglicht Ihnen zusammen mit den von Azure Monitor gesammelten Daten, Verwaltungslösungen und auf den virtuellen Computern in der Cloud oder lokal installierten Agents, sich ein vollständiges Bild von Ihrer gesamten Umgebung zu machen. 


### <a name="service-map"></a>Dienstzuordnung
[Dienstzuordnung](../operations-management-suite/operations-management-suite-service-map.md) bietet durch die Analyse der virtuellen Computer mit ihren verschiedenen Prozessen und Abhängigkeiten von anderen Computern und externen Prozessen einen Einblick in Ihre IaaS-Umgebung.  Die Anwendung integriert Ereignisse, Leistungsdaten und Verwaltungslösungen in Log Analytics, damit Sie diese Daten im Kontext des einzelnen Computers und seiner Beziehung zum Rest der Umgebung anzeigen können.  Dienstzuordnung ähnelt der [Anwendungszuordnung in Application Insights](../application-insights/app-insights-app-map.md), konzentriert sich aber auf die Infrastrukturkomponenten, die Ihre Anwendungen unterstützen.

### <a name="network-watcher"></a>Network Watcher
[Network Watcher](../network-watcher/network-watcher-monitoring-overview.md) bietet eine szenariobasierte Überwachung und Diagnose für verschiedene Netzwerkszenarien in Azure.  Die Anwendung speichert Daten zur weiteren Analyse in Azure-Metriken und -Diagnose und bietet zusammen mit [Verwaltungslösungen in Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md) die umfassende Überwachung Ihrer Netzwerkressourcen.


### <a name="management-solutions"></a>Verwaltungslösungen
[Verwaltungslösungen](../log-analytics/log-analytics-add-solutions.md) sind Programmpakete, die Einblicke für eine bestimmte Anwendung oder einen bestimmten Dienst liefern.  Zum Speichern und Analysieren der von ihnen gesammelten Überwachungsdaten nutzen sie Log Analytics.  Microsoft und Partner bieten Verwaltungslösungen zur Überwachung für verschiedene Azure- und Drittanbieterdienste an. Beispiele für Überwachungslösungen sind [Container Monitoring](../log-analytics/log-analytics-containers.md) zum Anzeigen und Verwalten Ihrer Containerhosts und [Azure SQL-Analyse](../log-analytics/log-analytics-azure-sql.md) zum Sammeln und Visualisieren von Leistungsmetriken für SQL Azure-Datenbanken.


## <a name="shared-functionality"></a>Gemeinsam genutzte Funktionalität
Die folgenden Azure-Tools bieten wichtige Funktionen für die Premium-Überwachungsdienste.  Sie werden von mehreren Diensten gemeinsam genutzt, sodass Sie allgemeine Funktionalität und Konfigurationen mehrere Dienste übergreifend nutzen können.

### <a name="alerts"></a>Alerts
[Azure Alerts](../monitoring-and-diagnostics/monitoring-overview-alerts.md) informiert Sie proaktiv über kritische Zustände und leitet potenziell Korrekturmaßnahmen ein.  Warnungsregeln können Daten aus mehreren Quellen einschließlich Metriken und Protokollen nutzen. Sie verwenden [Aktionsgruppen](../monitoring-and-diagnostics/monitoring-action-groups.md), die eindeutige Sätze von Empfängern und Aktionen als Reaktion auf eine Warnung enthalten.  Je nach Ihren Anforderungen können Warnungen externe Aktionen mithilfe von Webhooks starten und in Ihre ITSM-Tools integrieren.

### <a name="dashboards"></a>Dashboards
Mit [Azure Dashboards](../azure-portal/azure-portal-dashboards.md) können Sie verschiedene Arten von Daten in einem einzelnen Bereich im Azure-Portal kombinieren und gemeinsam mit anderen Azure-Benutzern nutzen.  Beispielsweise können Sie ein Dashboard erstellen, das Kacheln kombiniert, die ein Diagramm der Metriken, eine Tabelle mit Aktivitätsprotokollen, ein Nutzungsdiagramm von Application Insights und die Ausgabe einer Protokollsuche in Log Analytics zeigen.

Sie können auch Log Analytics-Daten nach [Power BI](https://docs.microsoft.com/power-bi/) exportieren, um weitere Visualisierungen zu nutzen und die Daten auch anderen Personen innerhalb und außerhalb Ihrer Organisation zur Verfügung zu stellen.

### <a name="metrics-explorer"></a>Metrik-Explorer
[Metriken](../monitoring-and-diagnostics/monitoring-overview-metrics.md) sind von Azure-Ressourcen generierte numerische Werte, die Ihnen helfen, Betrieb und Leistung der Ressource zu verstehen. Sie können Metriken für die Analyse mit Daten aus anderen Quellen an Log Analytics senden.



### <a name="activity-logs"></a>Aktivitätsprotokolle
[Aktivitätsprotokolle](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) enthalten Daten zum Betrieb einer Azure-Ressource.  Dies beinhaltet Informationen wie Änderungen an der Konfiguration der Ressource, Dienstintegritätsvorfälle, Empfehlungen zur besseren Nutzung der Ressource und Informationen im Zusammenhang mit Vorgängen zur automatischen Skalierung.  Sie können Protokolle für eine bestimmte Ressource auf deren Seite im Azure-Portal anzeigen oder Protokolle mehrerer Ressourcen im Aktivitätsprotokoll-Explorer anzeigen.  Sie können auch Aktivitätsprotokolle an Log Analytics senden, damit sie mit Daten analysiert werden können, die von Verwaltungslösungen und Agents auf virtuellen Computern gesammelt werden sowie aus anderen Quellen stammen.


## <a name="example-scenarios"></a>Beispielszenarien
Die folgenden Beispiele auf hoher Ebene veranschaulichen, wie Sie verschiedene Überwachungstools in Azure für verschiedene Szenarien nutzen können.

### <a name="monitoring-a-web-application"></a>Überwachen einer Webanwendung
Wir gehen von einer in Azure bereitgestellten Webanwendung aus, die App Services, Azure Storage und eine SQL-Datenbank nutzt.  Beginnen Sie mit dem Zugriff auf [Metriken](../monitoring-and-diagnostics/monitoring-overview-metrics.md) und [Aktivitätsprotokolle](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) für jede einzelne dieser Ressourcen auf deren Seiten im Azure-Portal.  Dies beinhaltet wichtige Informationen, z.B. die Anzahl der an die Anwendung gerichteten Anforderungen und die durchschnittliche Antwortzeit zusätzlich zum Identifizieren etwaiger Konfigurationsänderungen.

Sie können dann im Portal mit „Überwachung“ fortfahren, um Metriken und Protokolle für die verschiedenen Ressourcen zusammen anzuzeigen.  Wenn Sie Standardparameter für die Metriken bestimmen, [erstellen Sie Warnungsregeln](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md), die Sie proaktiv benachrichtigen, wenn z.B. die durchschnittliche Antwortzeit einen Schwellenwert überschreitet.  Um eine kurze Übersicht über die tägliche Leistung Ihrer Anwendung zu erhalten, erstellen Sie ein Azure-Dashboard zum Anzeigen von Diagrammen von Metriken, die wichtige KPIs darstellen.

Wenn Sie eine umfassendere Überwachung Ihrer Anwendung wünschen, [konfigurieren Sie sie für Application Insights](../application-insights/quick-monitor-portal.md).  Sie können jetzt zusätzliche Daten sammeln, die weitere Einblicke in Betrieb und Leistung Ihrer Anwendung liefern.  Application Insights erkennt die zugrunde liegenden Beziehungen zwischen den Komponenten Ihrer App und ermöglicht die visuelle Darstellung über die [Anwendungszuordnung ](../application-insights/app-insights-app-map.md) gekoppelt mit [End-to-End-Ablaufverfolgung](../application-insights/app-insights-transaction-diagnostics.md), um die genaue Komponente, Abhängigkeit oder Ausnahme zu diagnostizieren, wo ein Problem aufgetreten ist.  Sie erstellen [Verfügbarkeitstests](../application-insights/app-insights-monitor-web-app-availability.md), um proaktiv Ihre Anwendung von unterschiedlichen Regionen aus zu testen.  Um Ihre Entwickler zu unterstützen, [aktivieren Sie den Profiler](../application-insights/enable-profiler-compute.md), damit Sie Anforderungen und Ausnahmen bis zu einer bestimmten Codezeile verfolgen können.  

Um weiteren Einblick in Dienste zu erhalten, die in Ihrer Anwendung verwendet werden, fügen Sie die [Lösung SQL-Analyse](../log-analytics/log-analytics-azure-sql.md) zum Sammeln zusätzlicher Daten in Log Analytics hinzu. Nach einiger Zeit möchten Sie die Hauptursache für Zeiträume herausfinden, in denen die Leistung auf der Website unter den Schwellenwert gefallen ist.  Sie schreiben mit Log Analytics eine Abfrage zum Korrelieren der von Application Insights gesammelten Nutzungs- und Leistungsdaten mit Konfigurations- und Leistungsdaten der Azure-Ressourcen, die Ihre Anwendung unterstützen.


### <a name="monitoring-virtual-machines"></a>Überwachung virtueller Computer
Sie führen in Azure eine Mischung aus Windows- und Linux-VMs aus.  Mit Azure Monitor zeigen Sie [Aktivitätsprotokolle](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) und [Metriken auf Hostebene](../monitoring-and-diagnostics/monitoring-overview-metrics.md) an und fügen dann die [Azure-Diagnose-Erweiterung](../virtual-machines/linux/tutorial-monitoring.md#install-diagnostics-extension) den virtuellen Computern hinzu, um Metriken auf dem Gastbetriebssystem zu sammeln.  Dann erstellen Sie [Warnungsregeln](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md), die Sie proaktiv benachrichtigen, wenn Standardmetriken wie Prozessorauslastung und Arbeitsspeicher Schwellenwerte überschreiten.

Um ausführlichere Informationen zu virtuellen Computern mit einer Geschäftsanwendung zu sammeln, [erstellen Sie einen Log Analytics-Arbeitsbereich und aktivieren die VM-Erweiterung](../log-analytics/log-analytics-quick-collect-azurevm.md) auf jedem Computer.  Sie konfigurieren [die Sammlung aus verschiedenen Datenquellen](../log-analytics/log-analytics-data-sources.md) für Ihre Anwendung und [erstellen Ansichten](../log-analytics/log-analytics-view-designer.md), um über ihren täglichen Betrieb und ihre tägliche Leistung zu berichten.  Dann [erstellen Sie Warnungsregeln](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md), die Sie informieren, wenn bestimmte Fehlerereignisse festgestellt werden.  Um die Integrität des installierten Agents fortlaufend zu überwachen, fügen Sie die [Agent-Integritätsverwaltungslösung](../operations-management-suite/oms-solution-agenthealth.md) hinzu.

Um weitere Einblicke in die Anwendung zu erhalten, [fügen Sie den Abhängigkeits-Agent](../operations-management-suite/operations-management-suite-service-map-configure.md) den virtuellen Computern hinzu, um sie der [Dienstzuordnung](../operations-management-suite/operations-management-suite-service-map.md) hinzuzufügen.  Er ermittelt kritische Prozesse und identifiziert Verbindungen zwischen Computern mit anderen Diensten.  Nach einem gemeldeten Ausfall identifizieren Sie mit Hilfe der Dienstzuordnung die Computer, bei denen das Problem aufgetreten ist.  Dann erstellen Sie eine [Abfrage an die Log Analytics-Daten](../log-analytics/log-analytics-log-search-new.md), um das Problem in Zukunft zu identifizieren, und eine Warnungsregel, die Sie proaktiv benachrichtigt, wenn die Bedingung erkannt wurde.



## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen

* [Azure Monitor in einem Video von der Ignite 2016](https://myignite.microsoft.com/videos/4977)
* [Erste Schritte mit Azure Monitor](monitoring-get-started.md)
* [Azure-Diagnose](../azure-diagnostics.md) für die Diagnose von Problemen mit Clouddiensten, virtuellen Computern, VM-Skalierungsgruppen oder Service Fabric-Anwendungen.
* [Application Insights](https://azure.microsoft.com/documentation/services/application-insights/) : Diagnostizieren Sie Probleme mit Ihrer App Service-Web-App.
* [Log Analytics](https://azure.microsoft.com/documentation/services/log-analytics/) zur Analyse der gesammelten Überwachungsdaten.
