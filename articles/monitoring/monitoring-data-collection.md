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
ms.openlocfilehash: efbf0907f3ed75957159c38a536bd31e88a0dbb3
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2018
ms.locfileid: "39213298"
---
# <a name="collecting-monitoring-data-in-azure"></a>Sammeln von Überwachungsdaten in Azure
Dieser Artikel bietet eine Übersicht über die Überwachungsdaten, die aus Anwendungen und Diensten in Azure gesammelt werden, sowie über die Tools, die zu deren Analyse verwendet werden. 

## <a name="types-of-monitoring-data"></a>Typen von Überwachungsdaten
Alle Überwachungsdaten gehören zu einem von zwei grundlegenden Typen: Metriken oder Protokolle. Jeder Typ verfügt über unterschiedliche Eigenschaften und eignet sich am besten für bestimmte Szenarien, wie unten beschrieben.

### <a name="metrics"></a>Metriken
Metriken sind numerische Werte, die einen Aspekt eines Systems zu einem bestimmten Zeitpunkt beschreiben. Sie enthalten verschiedene Daten, darunter den Wert selbst, den Zeitpunkt der Werterfassung, die Art des Messwerts und die jeweilige Ressource, der der Wert zugeordnet ist. Metriken werden in regelmäßigen Abständen gesammelt, unabhängig davon, ob sich der Wert ändert oder nicht. Beispielsweise können Sie die Prozessorauslastung eines virtuellen Computers jede Minute oder die Anzahl der bei Ihrer Anwendung angemeldeten Benutzer alle 10 Minuten erfassen.

Metriken sind einfach strukturiert und in der Lage, Szenarien in nahezu Echtzeit zu unterstützen. Insbesondere für Warnmeldungen sind Metriken gut geeignet, da sie häufig abgetastet werden können und eine Warnung mit relativ einfacher Logik schnell ausgelöst werden kann. Beispielsweise können Sie eine Warnung auslösen, wenn eine Metrik einen Schwellenwert überschreitet, oder, wenn die Differenz zwischen zwei Metriken einen bestimmten Wert erreicht.

Einzelne Metriken liefern in der Regel nur wenige Einblicke. Sie liefern einen einzigen Wert ohne Kontext außer dem Vergleich mit einem einfachen Schwellenwert. Nichtsdestotrotz sind sie äußerst nützlich, wenn sie mit anderen Metriken kombiniert werden, um Muster und Trends zu erkennen, oder, wenn sie mit Protokollen kombiniert werden, die einen Kontext zu bestimmten Werte liefern. Beispielsweise kann eine bestimmte Anzahl von Benutzern, die zu einem bestimmten Zeitpunkt auf Ihre Anwendung zugreifen, wenig über die Integrität der Anwendung aussagen. Ein plötzlicher Rückgang der Benutzerzahl, der durch mehrere Werte der gleichen Metrik angezeigt wird, kann auf ein Problem hinweisen. Übermäßig viele Ausnahmen, die von der Anwendung ausgelöst und durch eine separate Metrik angezeigt werden, können ein Anwendungsproblem identifizieren, das den Rückgang verursacht. Ereignisse, die von der Anwendung erstellt werden, um Fehler in bestimmten Komponenten der Anwendung zu identifizieren, können Ihnen bei der Suche nach der Grundursache helfen.

Auf Protokollen basierende Warnungen sind nicht so reaktionsfähig wie Warnungen, die auf Metriken basieren, dafür können sie jedoch komplexere Logik enthalten. Es ist möglich, eine Warnung auf der Grundlage der Ergebnisse jeder Abfrage zu erstellen, die eine komplexe Analyse von Daten aus mehreren Quellen durchführt.

### <a name="logs"></a>Protokolle
Protokolle enthalten verschiedene Arten von Daten, die in Datensätzen mit unterschiedlichen Eigenschaften für jeden Typ organisiert sind. Protokolle können numerische Werte wie Metriken enthalten, liefern aber typischerweise Textdaten mit detaillierten Beschreibungen. Außerdem unterscheiden sie sich von Metriken in ihrer Struktur und, dass sie oft nicht in regelmäßigen Abständen gesammelt werden.

Ein gängiger Typ von Protokolleintrag ist ein Ereignis. Ereignisse werden sporadisch gesammelt, da sie von einer Anwendung oder einem Dienst erstellt werden und typischerweise genügend Informationen enthalten, um selbst einen vollständigen Kontext zu liefern.  Beispielsweise kann ein Ereignis anzeigen, dass eine bestimmte Ressource erstellt oder geändert, ein neuer Hoststart als Reaktion auf erhöhten Datenverkehr ausgeführt oder ein Fehler in einer Anwendung festgestellt wurde.

Protokolle sind besonders nützlich, um Daten aus einer Vielzahl von Quellen für komplexe Analysen und Trends über einen bestimmten Zeitraum zu kombinieren. Da das Format der Daten variieren kann, können Anwendungen benutzerdefinierte Protokolle mit der von ihnen benötigten Struktur erstellen. Metriken können sogar in Protokollen repliziert werden, um sie mit anderen Überwachungsdaten für Trend- und andere Datenanalysen zu kombinieren.


## <a name="monitoring-tools-in-azure"></a>Überwachungstools in Azure
Überwachungsdaten in Azure werden mithilfe mehrerer Quellen gesammelt und analysiert, die in den folgenden Abschnitten beschrieben werden.

### <a name="azure-metrics"></a>Azure-Metriken
Metriken aus Azure-Ressourcen und -Anwendungen werden in Azure-Metriken gesammelt. In den Seiten des Azure-Portals werden Metrikdaten für bestimmte Azure-Ressourcen wie z.B. virtuelle Computer integriert, die Grafiken zu solchen Metriken enthalten, z.B. die CPU- und Netzwerkauslastung für den ausgewählten Computer. Die Metriken können auch mit dem [Metrik-Explorer](../monitoring-and-diagnostics/monitoring-metric-charts.md) analysiert werden, der die Werte mehrerer Metriken im Zeitverlauf darstellen kann.  Sie können die Diagramme interaktiv nutzen oder an ein Dashboard anheften, um sie mit anderen Visualisierungstools anzuzeigen. Zudem haben Sie die Möglichkeit, Metriken mit der [REST-API für die Azure-Überwachung](../monitoring-and-diagnostics/monitoring-rest-api-walkthrough.md) abzurufen.

Details zu den Metrikdaten, die von verschiedenen Arten von Azure-Ressourcen gesammelt werden, finden Sie unter [Quellen für Überwachungsdaten in Azure](monitoring-data-sources.md). 

![Metrik-Explorer](media/monitoring-data-collection/metrics-explorer.png)


### <a name="azure-activity-log"></a>Azure-Aktivitätsprotokoll 
Das [Azure-Aktivitätsprotokoll](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) speichert Protokolle über die Konfiguration und die Integrität der Azure-Dienste. Zur Anzeige der Protokolle im Azure-Portal können Sie den Aktivitätsprotokoll-Explorer verwenden, aber die Protokolle werden üblicherweise [nach Log Analytics kopiert](../log-analytics/log-analytics-activity.md), um mit anderen Protokolldaten analysiert zu werden.

Im Aktivitätsprotokoll-Explorer können Sie die Aktivitätsprotokolle nach bestimmten Kriterien gefiltert anzeigen.  Die meisten Ressourcen verfügen auch in ihrem Menü im Azure-Portal über eine Aktivitätsprotokolloption, die den für diese Ressource gefilterten Aktivitätsprotokoll-Explorer anzeigt. Alternativ können Sie Aktivitätsprotokolle mit der [REST-API für die Azure-Überwachung](../monitoring-and-diagnostics/monitoring-rest-api-walkthrough.md) abrufen.

![Aktivitätsprotokoll-Explorer](media/monitoring-data-collection/activity-log-explorer.png)


### <a name="log-analytics"></a>Log Analytics
Log Analytics bietet eine gemeinsame Datenplattform für die Verwaltung in Azure. Es ist der primäre Dienst, der für die Speicherung und Analyse von Protokollen in Azure verwendet wird und Daten aus einer Vielzahl von Quellen wie Agents auf virtuellen Computern, Verwaltungslösungen und verschiedenen Azure-Ressourcen sammelt. Daten aus anderen Quellen, einschließlich Metriken und dem Aktivitätsprotokoll, können in Log Analytics kopiert werden, um ein vollständiges zentrales Repository von Überwachungsdaten zu erstellen.

Log Analytics verfügt über eine umfangreiche Abfragesprache zur Analyse der gesammelten Daten.  Mithilfe von [Protokollsuchportalen](../log-analytics/log-analytics-log-search-portals.md) können Sie interaktiv Abfragen schreiben, testen und deren Ergebnisse analysieren. Zudem können Sie [Ansichten erstellen](../log-analytics/log-analytics-view-designer.md), um die Ergebnisse der Protokollsuche zu visualisieren oder die Ergebnisse einer Abfrage direkt in ein Azure-Dashboard einzufügen.  Zu den Verwaltungslösungen gehören Protokollsuchen und Ansichten in Log Analytics zur Analyse der gesammelten Daten. Andere Dienste wie Application Insights speichern Daten in Log Analytics und stellen zusätzliche Analysetools zur Verfügung.  

![Protokolle](media/monitoring-data-collection/logs.png)

### <a name="application-insights"></a>Application Insights
Application Insights sammelt Telemetriedaten für Webanwendungen, die auf einer Vielzahl von Plattformen installiert sind. Die Daten werden in Azure-Metriken und Log Analytics gespeichert. Application Insights bietet zusätzlich zu den vorhandenen Tools zur Analyse dieser Daten eine umfangreiche Auswahl an Tools zur Analyse und Visualisierung seiner Daten. Auf diese Weise können Sie einen gemeinsamen Satz von Diensten wie Warnungen, Protokollsuche und Dashboards nutzen, die Sie für andere Überwachungen verwenden.


![App Insights](media/monitoring-data-collection/app-insights.png)

### <a name="service-map"></a>Dienstzuordnung
Die Dienstzuordnung bietet eine visuelle Darstellung der virtuellen Computer mit ihren Prozessen und Abhängigkeiten. Die meisten dieser Daten werden in Log Analytics gespeichert, sodass Sie sie mit anderen Verwaltungsdaten analysieren können. Außerdem ruft die Dienstzuordnungskonsole Daten aus Log Analytics ab, um sie im Kontext des zu analysierenden virtuellen Computers darzustellen.

![Dienstzuordnung](media/monitoring-data-collection/service-map.png)


## <a name="transferring-monitoring-data"></a>Übertragen von Überwachungsdaten

### <a name="metrics-to-logs"></a>Metriken auf Protokolle
Metriken können auch in Log Analytics repliziert werden, um komplexe Analysen mit anderen Datentypen unter Verwendung der umfangreichen Abfragesprache durchzuführen. Zudem haben Sie die Möglichkeit, Protokolldaten über einen längeren Zeitraum als Metriken aufzubewahren, wodurch Sie eine Trendanalyse für einen bestimmten Zeitraum durchführen können. Wenn Metriken oder andere Leistungsdaten in Log Analytics gespeichert werden, fungieren diese Daten als Protokoll. Verwenden Sie Metriken zur Unterstützung von Analysen und Warnungen in nahezu Echtzeit, während Sie Protokolle für Trendanalysen und Analysen mit anderen Daten einsetzen.

Eine Anleitung zum Sammeln von Metriken aus Azure-Ressourcen finden Sie unter [Sammeln von Azure-Dienstprotokollen und Metriken zur Verwendung in Log Analytics](../log-analytics/log-analytics-azure-storage.md). Informationen zum Sammeln von Ressourcenmetriken aus Azure-PaaS-Ressourcen finden Sie unter [Konfigurieren der Sammlung von Azure-PaaS-Ressourcenmetriken mit Log Analytics](../log-analytics/log-analytics-collect-azurepass-posh.md).

### <a name="logs-to-metrics"></a>Protokolle auf Metriken
Wie oben beschrieben, sind Metriken reaktionsschneller als Protokolle, sodass Sie Warnungen mit geringerer Wartezeit und zu geringeren Kosten erstellen können. Log Analytics sammelt eine bedeutende Menge an numerischen Daten, die für Metriken geeignet wären, aber nicht in Azure-Metriken gespeichert sind. Ein häufiges Beispiel sind aus Agents und Verwaltungslösungen gesammelte Leistungsdaten. Einige dieser Werte können in Azure-Metriken kopiert werden, wo sie für Warnungen und Analysen mit dem Metrik-Explorer zur Verfügung stehen.

Erläuterungen zu diesem Feature finden Sie unter [Faster Metric Alerts for Logs now in public preview](https://azure.microsoft.com/blog/faster-metric-alerts-for-logs-now-in-limited-public-preview/) (Schnellere Metrikwarnungen für Protokolle jetzt in öffentlicher Vorschauversion). Die Liste der unterstützten Werte finden Sie unter [Unterstützte Metriken und Erstellungsmethoden für neue Metrikwarnungen](../monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts.md).

### <a name="event-hub"></a>Event Hub
Zusätzlich zur Verwendung der Tools in Azure für die Analyse von Überwachungsdaten können Sie diese auch an ein externes Tool wie ein SIEM-Produkt (Security Information and Event Management) weiterleiten. Dies geschieht in der Regel mit [Azure Event Hub](https://docs.microsoft.com/azure/event-hubs/). 

Weitere Informationen zu den verschiedenen Arten von Überwachungsdaten finden Sie unter [Streamen von Azure-Überwachungsdaten an einen Event Hub für die Verwendung durch ein externes Tool](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md).

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die für verschiedene Ressourcen in Azure [verfügbaren Überwachungsdaten](monitoring-data-sources.md). 