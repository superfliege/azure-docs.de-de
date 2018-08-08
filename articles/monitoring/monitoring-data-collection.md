---
title: Sammeln von Überwachungsdaten in Azure | Microsoft-Dokumentation
description: Übersicht über die Überwachungsdaten, die aus Anwendungen und Diensten in Azure gesammelt werden, sowie über die Tools, die zu deren Analyse verwendet werden.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: monitoring
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/06/2018
ms.author: bwren
ms.openlocfilehash: 35580d71aa2592fa94f42cfdbad3c192acc303c5
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2018
ms.locfileid: "39363984"
---
# <a name="collect-monitoring-data-in-azure"></a>Sammeln von Überwachungsdaten in Azure
Dieser Artikel bietet eine Übersicht über die Überwachungsdaten, die aus Anwendungen und Diensten in Azure gesammelt werden. Außerdem werden die Tools beschrieben, die Sie zum Analysieren der Daten verwenden können. 

## <a name="types-of-monitoring-data"></a>Typen von Überwachungsdaten
Alle Überwachungsdaten gehören zu einem von zwei grundlegenden Typen: Metriken oder Protokolle. Jeder Typ verfügt über unterschiedliche Eigenschaften und eignet sich am besten für bestimmte Szenarien.

### <a name="metrics"></a>Metriken
Metriken sind numerische Werte, die einen Aspekt eines Systems zu einem bestimmten Zeitpunkt beschreiben. Hierzu gehören folgende Berechtigungen:

* Unterschiedliche Daten, einschließlich des eigentlichen Werts
* Der Zeitpunkt, zu dem der Wert gesammelt wurde
* Der Messungstyp, den der Wert darstellt
* Die Ressource, der der Wert zugeordnet ist 

Metriken werden in regelmäßigen Abständen gesammelt, unabhängig davon, ob sich der Wert ändert oder nicht. Beispielsweise können Sie die Prozessorauslastung eines virtuellen Computers jede Minute oder die Anzahl der bei Ihrer Anwendung angemeldeten Benutzer alle 10 Minuten erfassen.

Metriken sind einfach strukturiert und in der Lage, Szenarien in nahezu Echtzeit zu unterstützen. Sie sind für Warnmeldungen gut geeignet, da Metriken häufig abgetastet werden können und eine Warnung mit relativ einfacher Logik schnell ausgelöst werden kann. Beispielsweise können Sie eine Warnung auslösen, wenn eine Metrik einen Schwellenwert überschreitet. Oder Sie können eine Warnung auslösen, wenn der Unterschied zwischen zwei Metriken einen bestimmten Wert erreicht.

Einzelne Metriken liefern in der Regel nur wenige Einblicke. Sie liefern einen einzigen Wert ohne Kontext außer dem Vergleich mit einem einfachen Schwellenwert. Sie sind äußerst nützlich in Verbindung mit anderen Metriken, um Muster und Trends zu erkennen, oder mit Protokollen, die einen Kontext zu bestimmten Werten liefern. 

Beispielsweise sagt eine bestimmte Anzahl von Benutzern, die zu einem bestimmten Zeitpunkt auf Ihre Anwendung zugreifen, wenig über die Integrität der Anwendung aus. Ein plötzlicher Rückgang der Benutzerzahl, der durch mehrere Werte derselben Metrik angezeigt wird, kann hingegen auf ein Problem hinweisen. Übermäßig viele Ausnahmen, die von der Anwendung ausgelöst und durch eine separate Metrik angezeigt werden, können ein Anwendungsproblem identifizieren, das den Rückgang verursacht. Ereignisse, die die Anwendung zum Identifizieren von Fehlern in den zugehörigen Komponenten erstellt, können beim Ermitteln der Ursache helfen.

Auf Protokollen basierende Warnungen sind nicht so reaktionsfähig wie Warnungen, die auf Metriken basieren, dafür können sie jedoch komplexere Logik enthalten. Es ist möglich, eine Warnung auf der Grundlage der Ergebnisse jeder Abfrage zu erstellen, die eine komplexe Analyse von Daten aus mehreren Quellen durchführt.

### <a name="logs"></a>Protokolle
Protokolle enthalten verschiedene Arten von Daten, die in Datensätzen mit unterschiedlichen Eigenschaften für jeden Typ organisiert sind. Protokolle können numerische Werte wie Metriken enthalten, typischerweise enthalten sie jedoch Textdaten mit detaillierten Beschreibungen. Außerdem unterscheiden sie sich von Metriken in ihrer Struktur und, dass sie oft nicht in regelmäßigen Abständen gesammelt werden.

Ein gängiger Typ von Protokolleintrag ist ein Ereignis. Ereignisse werden sporadisch erfasst. Sie werden von einer Anwendung oder einem Dienst erstellt und enthalten typischerweise genügend Informationen, um selbst einen vollständigen Kontext zu liefern. Beispielsweise kann ein Ereignis anzeigen, dass eine bestimmte Ressource erstellt oder geändert, ein neuer Host als Reaktion auf erhöhten Datenverkehr gestartet oder ein Fehler in einer Anwendung festgestellt wurde.

Protokolle sind besonders nützlich, um Daten aus einer Vielzahl von Quellen für komplexe Analysen und Trends über einen bestimmten Zeitraum zu kombinieren. Da das Format der Daten variieren kann, können Anwendungen benutzerdefinierte Protokolle mit der von ihnen benötigten Struktur erstellen. Metriken können sogar in Protokollen repliziert werden, um sie mit anderen Überwachungsdaten für Trend- und andere Datenanalysen zu kombinieren.


## <a name="monitoring-tools-in-azure"></a>Überwachungstools in Azure
Die Überwachung von Daten in Azure wird über die folgenden Quellen gesammelt und analysiert.

### <a name="azure-monitor"></a>Azure Monitor
Metriken aus Azure-Ressourcen und -Anwendungen werden in Azure Monitor gesammelt. Metrikdaten sind in die Seiten für Azure-Ressourcen im Azure-Portal integriert. Für virtuelle Computer werden Diagramme mit Metriken wie CPU- und Netzwerkauslastung für den ausgewählten Computer angezeigt. 

Daten können auch mit dem [Metrik-Explorer](../monitoring-and-diagnostics/monitoring-metric-charts.md) analysiert werden, der die Werte mehrerer Metriken im Zeitverlauf darstellt. Sie können die Diagramme interaktiv nutzen oder an ein Dashboard anheften, um sie mit anderen Visualisierungstools anzuzeigen. Zudem haben Sie die Möglichkeit, Metriken mit der [REST-API für die Azure-Überwachung](../monitoring-and-diagnostics/monitoring-rest-api-walkthrough.md) abzurufen.

Details zu den Metrikdaten, die von verschiedenen Arten von Azure-Ressourcen gesammelt werden, finden Sie unter [Quellen für Überwachungsdaten in Azure](monitoring-data-sources.md). 

![Metrik-Explorer](media/monitoring-data-collection/metrics-explorer.png)


### <a name="activity-log"></a>Aktivitätsprotokoll 
Das [Azure-Aktivitätsprotokoll](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) speichert Protokolle über die Konfiguration und die Integrität der Azure-Dienste. Zum Anzeigen der Protokolle im Azure-Portal können Sie den Aktivitätsprotokoll-Explorer verwenden, aber die Protokolle werden üblicherweise [nach Azure Log Analytics kopiert](../log-analytics/log-analytics-activity.md), um mit anderen Protokolldaten analysiert zu werden.

Im Aktivitätsprotokoll-Explorer können Sie die Aktivitätsprotokolle nach bestimmten Kriterien gefiltert anzeigen. Die meisten Ressourcen weisen in ihrem Menü im Azure-Portal auch eine Option **Aktivitätsprotokoll** auf. Hiermit wird der Aktivitätsprotokoll-Explorer angezeigt, für diese Ressource gefiltert. Sie können Aktivitätsprotokolle auch mit der [REST-API für die Azure-Überwachung](../monitoring-and-diagnostics/monitoring-rest-api-walkthrough.md) abrufen.

![Aktivitätsprotokoll-Explorer](media/monitoring-data-collection/activity-log-explorer.png)


### <a name="log-analytics"></a>Log Analytics
Log Analytics bietet eine gemeinsame Datenplattform für die Verwaltung in Azure. Dies ist der primäre Dienst für die Speicherung und Analyse von Protokollen in Azure. Er sammelt Daten aus einer Vielzahl von Quellen, einschließlich Agents auf virtuellen Computern, Verwaltungslösungen und Azure-Ressourcen. Sie können Daten aus anderen Quellen kopieren, einschließlich Metriken und des Aktivitätsprotokolls, um ein vollständiges zentrales Repository von Überwachungsdaten zu erstellen.

Log Analytics verfügt über eine umfangreiche Abfragesprache zur Analyse der gesammelten Daten. Mithilfe von [Protokollsuchportalen](../log-analytics/log-analytics-log-search-portals.md) können Sie interaktiv Abfragen schreiben, testen und deren Ergebnisse analysieren. Zudem können Sie [Ansichten erstellen](../log-analytics/log-analytics-view-designer.md), um die Ergebnisse der Protokollsuche zu visualisieren oder die Ergebnisse einer Abfrage direkt in ein Azure-Dashboard einzufügen.  

Zu den Verwaltungslösungen gehören Protokollsuchen und Ansichten in Log Analytics zur Analyse der gesammelten Daten. Andere Dienste wie Azure Application Insights speichern Daten in Log Analytics und stellen zusätzliche Analysetools zur Verfügung.  

![Protokolle](media/monitoring-data-collection/logs.png)

### <a name="application-insights"></a>Application Insights
Application Insights sammelt Telemetriedaten für Webanwendungen, die auf einer Vielzahl von Plattformen installiert sind. Der Dienst speichert seine Daten in Azure Monitor und Log Analytics. Außerdem bietet er einen umfangreichen Satz von Tools zum Analysieren und Visualisieren der Daten. Durch diese Möglichkeiten können Sie einen gemeinsamen Satz von Diensten wie Warnungen, Protokollsuchen und Dashboards nutzen, die Sie für andere Überwachungen verwenden.


![Application Insights](media/monitoring-data-collection/app-insights.png)

### <a name="service-map"></a>Dienstzuordnung
Die Dienstzuordnung bietet eine visuelle Darstellung der virtuellen Computer mit ihren Prozessen und Abhängigkeiten. Die meisten dieser Daten werden in Log Analytics gespeichert, sodass Sie sie mit anderen Verwaltungsdaten analysieren können. Außerdem ruft die Dienstzuordnungskonsole Daten aus Log Analytics ab, um sie im Kontext des zu analysierenden virtuellen Computers darzustellen.

![Dienstzuordnung](media/monitoring-data-collection/service-map.png)


## <a name="transferring-monitoring-data"></a>Übertragen von Überwachungsdaten

### <a name="metrics-to-logs"></a>Metriken auf Protokolle
Sie können Metriken in Log Analytics replizieren, um komplexe Analysen mit anderen Datentypen unter Verwendung der umfangreichen Abfragesprache durchzuführen. Zudem haben Sie die Möglichkeit, Protokolldaten über einen längeren Zeitraum als Metriken aufzubewahren, sodass Sie eine Trendanalyse für einen bestimmten Zeitraum durchführen können. Wenn Metriken oder andere Leistungsdaten in Log Analytics gespeichert werden, fungieren diese Daten als Protokoll. Verwenden Sie Metriken zur Unterstützung von Analysen und Warnungen in nahezu Echtzeit, während Sie Protokolle für Trendanalysen und Analysen mit anderen Daten einsetzen.

Eine Anleitung zum Sammeln von Metriken aus Azure-Ressourcen finden Sie unter [Sammeln von Azure-Dienstprotokollen und Metriken zur Verwendung in Log Analytics](../log-analytics/log-analytics-azure-storage.md). Informationen zum Sammeln von Ressourcenmetriken aus Azure-PaaS-Ressourcen finden Sie unter [Konfigurieren der Sammlung von Azure-PaaS-Ressourcenmetriken mit Log Analytics](../log-analytics/log-analytics-collect-azurepass-posh.md).

### <a name="logs-to-metrics"></a>Protokolle auf Metriken
Wie zuvor beschrieben sind Metriken reaktionsschneller als Protokolle, sodass Sie Warnungen mit geringerer Wartezeit und zu geringeren Kosten erstellen können. Log Analytics sammelt eine bedeutende Menge an numerischen Daten, die für Metriken geeignet wären, aber nicht in Azure Monitor gespeichert sind. 

Ein häufiges Beispiel sind aus Agents und Verwaltungslösungen gesammelte Leistungsdaten. Einige dieser Werte können in Azure Monitor kopiert werden, wo sie für Warnungen und Analysen mit dem Metrik-Explorer zur Verfügung stehen.

Erläuterungen zu diesem Feature finden Sie unter [Faster Metric Alerts for Logs now in public preview](https://azure.microsoft.com/blog/faster-metric-alerts-for-logs-now-in-limited-public-preview/) (Schnellere Metrikwarnungen für Protokolle jetzt in öffentlicher Vorschauversion). Die Liste der unterstützten Werte finden Sie unter [Unterstützte Metriken und Erstellungsmethoden für neue Metrikwarnungen](../monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts.md).

### <a name="event-hubs"></a>Event Hubs
Zusätzlich zur Verwendung der Tools in Azure für die Analyse von Überwachungsdaten können Sie diese auch an ein externes Tool wie ein SIEM-Produkt (Security Information and Event Management) weiterleiten. Die Weiterleitung erfolgt in der Regel über [Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/). 

Weitere Informationen zu den verschiedenen Arten von Überwachungsdaten finden Sie unter [Streamen von Azure-Überwachungsdaten an einen Event Hub für die Verwendung durch ein externes Tool](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md).

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die für verschiedene Ressourcen in Azure [verfügbaren Überwachungsdaten](monitoring-data-sources.md). 