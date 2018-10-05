---
title: Von Azure Monitor gesammelte Überwachungsdaten | Microsoft-Dokumentation
description: Die von Azure Monitor gefassten Überwachungsdaten unterteilen sich in Metriken, die einfach strukturiert und imstande sind, Szenarien nahezu in Echtzeit zu unterstützen, und Protokolle, die für die erweiterte Analyse in Log Analytics gespeichert werden.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: monitoring
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/27/2018
ms.author: bwren
ms.openlocfilehash: 19a611ca88310f06503bea2b8606699fe3e1c709
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47406039"
---
# <a name="monitoring-data-collected-by-azure-monitor"></a>Von Azure Monitor gesammelte Überwachungsdaten
[Azure Monitor](../azure-monitor/overview.md) ist ein Dienst, der Sie bei der Überwachung Ihrer Anwendungen und der Ressourcen, auf denen sie aufbauen, unterstützt. Im Zentrum dieser Funktionalität steht die Speicherung von Telemetriedaten und weiteren Daten aus überwachten Ressourcen. Dieser Artikel enthält eine vollständige Beschreibung der Weise, in der diese Daten von Azure Monitor gespeichert und verwendet werden.

Alle von Azure Monitor gesammelten Daten gehören einem von zwei Grundtypen an, [Metriken](#metrics) und [Protokollen](#logs). Metriken sind numerische Werte, die einen Aspekt eines Systems zu einem bestimmten Zeitpunkt beschreiben. Sie sind einfach strukturiert und in der Lage, Szenarien nahezu in Echtzeit zu unterstützen. Protokolle enthalten verschiedene Arten von Daten, die in Datensätzen mit unterschiedlichen Eigenschaften für jeden Typ organisiert sind. Telemetriedaten wie etwa Ereignisse und Ablaufverfolgungen werden als Protokolle zusätzlich zu Leistungsdaten gespeichert, die alle zur Analyse kombiniert werden können.

![Azure Monitor – Übersicht](../azure-monitor/media/overview/overview.png)

## <a name="metrics"></a>Metriken
Metriken sind numerische Werte, die einen Aspekt eines Systems zu einem bestimmten Zeitpunkt beschreiben. Sie sind einfach strukturiert und in der Lage, Szenarien nahezu in Echtzeit zu unterstützen. Metriken werden in regelmäßigen Abständen gesammelt, unabhängig davon, ob sich der Wert ändert oder nicht. Sie sind für Warnmeldungen gut geeignet, da sie häufig abgetastet werden können und eine Warnung mit relativ einfacher Logik schnell ausgelöst werden kann. 

Beispielsweise können Sie die Prozessorauslastung eines virtuellen Computers jede Minute oder die Anzahl der bei Ihrer Anwendung angemeldeten Benutzer alle 10 Minuten erfassen. Sie können eine Warnung auslösen, wenn einer dieser erfassten Werte – oder sogar die Differenz zwischen zwei Werten – einen definierten Schwellenwert überschreitet.

Dies sind einige der spezifischen Attribute von Metriken in Azure:

* Werden mit einer Häufigkeit von 1 Minute erfasst, sofern in der Definition der Metrik nicht anders angegeben.
* Eindeutig durch einen Metriknamen und einen Namespace identifiziert, der als Kategorie dient.
* 93 Tage lang gespeichert. Sie können Metriken für die langfristige Trendanalyse nach Log Analytics kopieren.

Jeder Metrikwert weist die folgenden Eigenschaften auf:
* Den Zeitpunkt, zu dem der Wert erfasst wurde
* Den Messungstyp, den der Wert darstellt
* Die Ressource, auf die sich der Wert bezieht
* Den Wert selbst
* Einige Metriken können mehrere Dimensionen aufweisen, wie im nächsten Abschnitt beschrieben. Benutzerdefinierte Metriken können über bis zu 10 Dimensionen verfügen.

### <a name="multi-dimensional-metrics"></a>Mehrdimensionale Metriken
Dimensionen einer Metrik sind Name/Wert-Paare, die zusätzliche Daten zum Beschreiben des Metrikwerts enthalten. Eine Metrik namens _Verfügbarer Speicherplatz_ kann z.B. eine Dimension mit dem Namen _Laufwerk_ und den Werten _C:_ und _D:_ aufweisen. In diesem Fall kann der verfügbare Speicherplatz entweder für beide Laufwerke oder für jedes Laufwerk separat angezeigt werden. 

Im folgenden Beispiel werden zwei Datasets für eine hypothetische Metrik namens _Netzwerkdurchsatz_ dargestellt. Das erste Dataset weist keine Dimensionen auf. Das zweite Dataset enthält Werte mit den zwei Dimensionen „IP-Adresse“ und _Richtung_:

### <a name="network-throughput"></a>Netzwerkdurchsatz

 |Zeitstempel        | Metrikwert | 
   | ------------- |:-------------| 
   | 8/9/2017 8:14 | 1.331,8 KBit/s | 
   | 8/9/2017 8:15 | 1.141,4 KBit/s |
   | 8/9/2017 8:16 | 1.110,2 KBit/s |

Diese nichtdimensionale Metrik kann nur grundlegende Fragen wie etwa Folgende beantworten: „Wie hoch war mein Netzwerkdurchsatz zu einem bestimmten Zeitpunkt?“.

### <a name="network-throughput--two-dimensions-ip-and-direction"></a>Netzwerkdurchsatz mit zwei Dimensionen („IP“ und „Richtung“)

| Zeitstempel          | Dimension „IP“ | Dimension „Richtung“ | Metrikwert| 
   | ------------- |:-----------------|:------------------- |:-----------|  
   | 8/9/2017 8:14 | IP="192.168.5.2" | Direction="Send"    | 646,5 KBit/s |
   | 8/9/2017 8:14 | IP="192.168.5.2" | Direction="Receive" | 420,1 KBit/s |
   | 8/9/2017 8:14 | IP="10.24.2.15"  | Direction="Send"    | 150,0 KBit/s | 
   | 8/9/2017 8:14 | IP="10.24.2.15"  | Direction="Receive" | 115,2 KBit/s |
   | 8/9/2017 8:15 | IP="192.168.5.2" | Direction="Send"    | 515,2 KBit/s |
   | 8/9/2017 8:15 | IP="192.168.5.2" | Direction="Receive" | 371,1 KBit/s |
   | 8/9/2017 8:15 | IP="10.24.2.15"  | Direction="Send"    | 155,0 KBit/s |
   | 8/9/2017 8:15 | IP="10.24.2.15"  | Direction="Receive" | 100,1 KBit/s |

Diese Metrik kann Fragen wie die folgenden beantworten: „Wie hoch war der Netzwerkdurchsatz für jede IP-Adresse?“ und „Wie viele Daten wurden gesendet und empfangen?“. Mehrdimensionale Metriken enthalten im Gegensatz zu nichtdimensionalen Metriken zusätzlich einen Analyse- und Diagnosewert.

### <a name="value-of-metrics"></a>Der Wert von Metriken
Einzelne Metriken liefern in der Regel nur wenige Einblicke. Sie liefern einen einzigen Wert ohne Kontext außer dem Vergleich mit einem einfachen Schwellenwert. Sie sind äußerst nützlich in Verbindung mit anderen Metriken, um Muster und Trends zu erkennen, oder mit Protokollen, die einen Kontext zu bestimmten Werten liefern. 

Beispielsweise sagt eine bestimmte Anzahl von Benutzern, die zu einem bestimmten Zeitpunkt auf Ihre Anwendung zugreifen, wenig über die Integrität der Anwendung aus. Ein plötzlicher Rückgang der Benutzerzahl, der durch mehrere Werte derselben Metrik angezeigt wird, kann hingegen auf ein Problem hinweisen. Übermäßig viele Ausnahmen, die von der Anwendung ausgelöst und durch eine separate Metrik angezeigt werden, können ein Anwendungsproblem identifizieren, das den Rückgang verursacht. Ereignisse, die die Anwendung zum Identifizieren von Fehlern in den zugehörigen Komponenten erstellt, können beim Ermitteln der Ursache helfen.

### <a name="sources-of-metric-data"></a>Quellen von Metrikdaten
Es gibt drei grundlegende Quellen von Metriken, die von Azure Monitor erfasst werden. Alle diese Metriken stehen im Metrikspeicher zur Verfügung, wo sie unabhängig von ihrer Quelle zusammen bewertet werden können.

**Plattformmetriken** werden von Azure-Ressourcen erstellt und geben Ihnen einen Einblick in ihre Integrität und Leistung. Jeder Ressourcentyp erstellt einen [eigenen Satz von Metriken](../monitoring-and-diagnostics/monitoring-supported-metrics.md); dazu ist keinerlei Konfiguration erforderlich. 

**Anwendungsmetriken** werden von Application Insights für Ihre überwachten Anwendungen erstellt und helfen Ihnen beim Erkennen von Leistungsproblemen und beim Nachverfolgen von Trends in der Nutzung Ihrer Anwendung. Dies beinhaltet solche Werte wie die _Serverantwortzeit_ und _Browserausnahmen_.

**Benutzerdefinierte Metriken** sind Metriken, die Sie über die automatisch verfügbaren Standardmetriken hinaus definieren. Benutzerdefinierte Metriken müssen für eine einzelne Ressource in der gleichen Region erstellt werden, in der sich auch die Ressource befindet. Sie können benutzerdefinierte Metriken mit den folgenden Methoden erstellen:
    - [Definieren Sie benutzerdefinierte Metriken in Ihrer Anwendung](../application-insights/app-insights-api-custom-events-metrics.md), die von Application Insights überwacht wird. Diese stellen eine Ergänzung des Standardsatzes von Anwendungsmetriken dar.
    - Veröffentlichen Sie benutzerdefinierte Metriken aus Ihren virtuellen Windows-Computern mithilfe von [Windows Diagnostic Extension (WAD)](../monitoring-and-diagnostics/azure-diagnostics.md).
    - Veröffentlichen Sie benutzerdefinierte Metriken aus Ihren virtuellen Linux-Computern mithilfe von [InfluxData Telegraf Agent](https://www.influxdata.com/time-series-platform/telegraf/).
    - Schreiben Sie benutzerdefinierte Metriken aus einem Azure-Dienst mithilfe der API für benutzerdefinierte Metriken.
    
![Übersicht der Metriken](media/monitoring-data-collection/metrics-overview.png)

### <a name="what-can-you-do-with-metrics"></a>Wozu kann ich Metriken nutzen?
Dies sind einige der Aufgaben, die Sie mit Metriken ausführen können:

- Verwenden des [Metrik-Explorers](../monitoring-and-diagnostics/monitoring-metric-charts.md) zum Analysieren der gesammelten Metriken und zu ihrer Darstellung in einem Diagramm. Nachverfolgen der Leistung einer Ressource (wie etwa einer VM, Website oder Logik-App) durch Anheften von Diagrammen an ein [Azure-Dashboard](../azure-portal/azure-portal-dashboards.md).
- Konfigurieren einer [Warnungsregel für eine Metrik](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md), die eine Benachrichtigung sendet oder eine [automatisierte Aktion](../monitoring-and-diagnostics/monitoring-action-groups.md) ausführt, sobald der Metrikwert einen Schwellenwert überschreitet.
- Verwenden von [Autoskalierung](../monitoring-and-diagnostics/monitoring-overview-autoscale.md), um Ressourcen basierend auf einem Schwellenwert, der von einer Metrik über- oder unterschritten wird, herauf- oder herabzusetzen.
- Weiterleiten von Metriken an Log Analytics, um Metrikdaten zusammen mit Protokolldaten zu analysieren und Metrikdaten länger als 93 Tage zu speichern. 
- Streamen von Metriken an einen [Event Hub](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md), um sie an [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) oder an externe Systeme weiterzuleiten.
- [Archivieren](../monitoring-and-diagnostics/monitor-tutorial-archive-monitoring-data.md) des Leistungs- oder Integritätsverlaufs Ihrer Ressourcen zu Kompatibilitäts-/Überwachungszwecken oder zur Offline-Berichterstellung.
- Zugreifen auf Metrikwerte von einer Befehlszeile oder einer benutzerdefinierten Anwendung aus mithilfe von [PowerShell-Cmdlets](https://docs.microsoft.com/powershell/module/azurerm.insights/?view=azurermps-6.7.0) oder [REST-API](../monitoring-and-diagnostics/monitoring-rest-api-walkthrough.md).



### <a name="viewing-metrics"></a>Anzeigen von Metriken
Metriken in Azure werden im Azure Monitor-Metrikspeicher gesammelt. Dieser ist eine Zeitreihendatenbank, die für den schnellen Abruf optimiert ist und Metrikwerte 93 Tage lang speichert. Kopieren Sie Metriken zur langfristigen Analyse und Trendanalyse nach Log Analytics.

Metrikdaten werden auf vielfältige Weise verwendet, wie oben beschrieben. Verwenden Sie den [Metrik-Explorer](../monitoring-and-diagnostics/monitoring-metric-charts.md), um die Daten in Ihrem Metrikspeicher direkt zu analysieren und die Werte mehrerer Metriken im zeitlichen Verlauf in Diagrammen darzustellen. Sie können die Diagramme interaktiv nutzen oder an ein Dashboard anheften, um sie mit anderen Visualisierungstools anzuzeigen. Zudem haben Sie die Möglichkeit, Metriken mit der [REST-API für die Azure-Überwachung](../monitoring-and-diagnostics/monitoring-rest-api-walkthrough.md) abzurufen.

![Metrik-Explorer](media/monitoring-data-collection/metrics-explorer.png)





## <a name="logs"></a>Protokolle
Protokolle enthalten verschiedene Arten von Daten, die in Datensätzen mit unterschiedlichen Eigenschaften für jeden Typ organisiert sind. Protokolle können numerische Werte wie Metriken enthalten, liefern aber typischerweise Textdaten mit detaillierten Beschreibungen. Außerdem unterscheiden sie sich von Metriken in ihrer Struktur und, dass sie oft nicht in regelmäßigen Abständen gesammelt werden.

Ein häufiger Typ von Protokolleintrag ist ein Ereignis, das sporadisch erfasst wird. Sie werden von einer Anwendung oder einem Dienst erstellt und enthalten typischerweise genügend Informationen, um selbst einen vollständigen Kontext zu liefern. Beispielsweise kann ein Ereignis anzeigen, dass eine bestimmte Ressource erstellt oder geändert, ein neuer Host als Reaktion auf erhöhten Datenverkehr gestartet oder ein Fehler in einer Anwendung festgestellt wurde.

Protokolle sind besonders nützlich, um Daten aus einer Vielzahl von Quellen für komplexe Analysen und Trends über einen bestimmten Zeitraum zu kombinieren. Da das Format der Daten variieren kann, können Anwendungen benutzerdefinierte Protokolle mit der von ihnen benötigten Struktur erstellen. Metriken können sogar in Protokollen repliziert werden, um sie mit anderen Überwachungsdaten für Trend- und andere Datenanalysen zu kombinieren.



### <a name="log-analytics"></a>Log Analytics
Die von Azure Monitor gesammelten Protokolle werden in Log Analytics gespeichert, das Telemetrie- und andere Daten aus einer Vielzahl von Quellen sammelt. Es bietet eine umfangreiche Abfragesprache und eine Analyseengine, die Ihnen Einblicke in den Betrieb Ihrer Anwendungen und Ressourcen gibt. Andere Azure-Dienste, wie z.B. [Azure Security Center](../security-center/security-center-intro.md), speichern ihre Daten in Log Analytics, um eine übergreifende gemeinsame Datenplattform in der Azure-Verwaltung bereitzustellen.

> [!IMPORTANT]
> Daten aus Application Insights werden in Log Analytics wie andere Protokolldaten gespeichert, jedoch mit dem Unterschied, dass sie in einer separaten Partition gespeichert werden. Dadurch wird die gleiche Funktionalität wie für andere Log Analytics-Daten unterstützt, jedoch müssen Sie die [Application Insights-Konsole](/application-insights/app-insights-analytics.md) oder die [Application Insights-API](https://dev.applicationinsights.io/) für den Zugriff auf diese Daten verwenden. Sie können eine [ressourcenübergreifende Abfrage](../log-analytics/log-analytics-cross-workspace-search.md) verwenden, um Anwendungsdaten zusammen mit anderen Protokolldaten zu analysieren.


### <a name="sources-of-log-data"></a>Quellen von Protokolldaten
Log Analytics kann Daten aus einer Vielzahl von Quellen sammeln, sowohl innerhalb von Azure als auch aus lokalen Ressourcen. Dies sind einige der Quellen, deren Daten in Log Analytics geschrieben werden:

- [Aktivitätsprotokolle](../log-analytics/log-analytics-activity.md) von Azure-Ressourcen, die Informationen zu deren Konfiguration und Integrität beinhalten, und [Diagnoseprotokolle](../monitoring-and-diagnostics/monitor-stream-diagnostic-logs-log-analytics.md), die Einblick in ihren Betrieb geben.
- Agents auf virtuellen [Windows](../log-analytics/log-analytics-windows-agent.md)- und [Linux](../log-analytics/log-analytics-linux-agents.md)-Computern senden Telemetriedaten basierend auf den von Ihnen konfigurierten [Datenquellen](../log-analytics/log-analytics-data-sources.md) aus dem Gastbetriebssystem und aus Anwendungen an Log Analytics.
- Von [Application Insights](https://docs.microsoft.com/azure/application-insights/) gesammelte Anwendungsdaten.
- Daten, die Einblick in eine bestimmte Anwendung oder einen bestimmten Dienst geben und aus [Überwachungslösungen](../monitoring/monitoring-solutions.md) oder Features wie Container Insights, VM Insights oder Resource Group Insights stammen.
- Vom [Azure Security Center](https://docs.microsoft.com/azure/security-center/) erfasste Sicherheitsdaten.
- [Metriken](#metrics) aus Azure-Ressourcen. Sie erhalten so die Möglichkeit, Metriken länger als 93 Tage aufzubewahren und sie zusammen mit anderen Protokolldaten zu analysieren.
- In [Azure Storage](../log-analytics/log-analytics-azure-storage-iis-table.md) geschriebene Telemetriedaten.
- Benutzerdefinierte Daten von beliebigen REST-API-Clients mithilfe des [HTTP Data Collector-API](../log-analytics/log-analytics-data-collector-api.md)-Clients oder aus einem [Azure Logik-App](https://docs.microsoft.com/azure/logic-apps/)-Workflow.

![Log Analytics-Komponenten](media/monitoring-data-collection/logs-overview.png)




### <a name="what-can-you-do-with-logs"></a>Wozu können Sie Protokolle nutzen?
Dies sind einige der Aufgaben, die Sie mit Protokollen ausführen können:

- Verwenden der [Log Analytics-Seite](../log-analytics/query-language/get-started-analytics-portal.md) im Azure-Portal zum Schreiben von Abfragen zur Analyse von Protokolldaten.  Anheften der als Tabellen oder Diagramme gerenderten Ergebnisse an ein [Azure-Dashboard](../azure-portal/azure-portal-dashboards.md).
- Konfigurieren einer [Protokollwarnungsregel](../monitoring-and-diagnostics/alert-log.md), die eine Benachrichtigung sendet oder eine [automatisierte Aktion](../monitoring-and-diagnostics/monitoring-action-groups.md) ausführt, wenn die Ergebnisse der Abfrage mit einem bestimmten Ergebnis übereinstimmen.
- Erstellen eines auf Daten in Log Analytics basierenden Workflows mithilfe von [Logik-Apps]().
- Exportieren der Ergebnisse einer Abfrage nach [Power BI](../log-analytics/log-analytics-powerbi.md), um verschiedene Visualisierungen zu verwenden und sie mit Benutzern außerhalb von Azure zu teilen.
- Zugreifen auf Metrikwerte von einer Befehlszeile oder einer benutzerdefinierten Anwendung aus mithilfe von [PowerShell-Cmdlets](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/?view=azurermps-6.8.1) oder [REST-API](https://dev.loganalytics.io/).

### <a name="viewing-log-data"></a>Anzeigen von Protokolldaten
Alle Daten aus Log Analytics werden mithilfe einer [Protokollabfrage](../log-analytics/log-analytics-queries.md) abgerufen, die einen bestimmten Satz Daten angibt. Abfragen werden mithilfe der [Log Analytics Abfragesprache](../log-analytics/query-language/get-started-queries.md) erstellt, einer umfassenden Abfragesprache, mit der gesammelte Daten schnell abgerufen, zusammengefasst und analysiert werden können. Verwenden Sie die [Log Analytics-Seite](../log-analytics/log-analytics-log-search-portals.md) im Azure-Portal, um die Daten in Ihrem Metrikspeicher direkt zu analysieren und die Werte mehrerer Metriken im zeitlichen Verlauf in Diagrammen darzustellen. Sie können die Diagramme interaktiv nutzen oder an ein Dashboard anheften, um sie mit anderen Visualisierungstools anzuzeigen. Zudem haben Sie die Möglichkeit, Metriken mit der [REST-API für die Azure-Überwachung](../monitoring-and-diagnostics/monitoring-rest-api-walkthrough.md) abzurufen.

![Protokolle](media/monitoring-data-collection/logs.png)

## <a name="convert-monitoring-data"></a>Konvertieren von Überwachungsdaten

### <a name="metrics-to-logs"></a>Metriken auf Protokolle
Sie können Metriken in Log Analytics kopieren, um komplexe Analysen mit anderen Datentypen unter Verwendung der umfangreichen Abfragesprache durchzuführen. Zudem haben Sie die Möglichkeit, Protokolldaten über einen längeren Zeitraum als Metriken aufzubewahren, sodass Sie eine Trendanalyse für einen bestimmten Zeitraum durchführen können. Wenn Metriken oder andere Leistungsdaten in Log Analytics gespeichert werden, fungieren diese Daten als Protokoll. Verwenden Sie Metriken zur Unterstützung von Analysen und Warnungen in nahezu Echtzeit, während Sie Protokolle für Trendanalysen und Analysen mit anderen Daten einsetzen.

Eine Anleitung zum Sammeln von Metriken aus Azure-Ressourcen finden Sie unter [Sammeln von Azure-Dienstprotokollen und Metriken zur Verwendung in Log Analytics](../log-analytics/log-analytics-azure-storage.md). Informationen zum Sammeln von Ressourcenmetriken aus Azure-PaaS-Ressourcen finden Sie unter [Konfigurieren der Sammlung von Azure-PaaS-Ressourcenmetriken mit Log Analytics](../log-analytics/log-analytics-collect-azurepass-posh.md).

### <a name="logs-to-metrics"></a>Protokolle auf Metriken
Wie oben beschrieben, sind Metriken reaktionsschneller als Protokolle, sodass Sie Warnungen mit geringerer Wartezeit und zu geringeren Kosten erstellen können. Log Analytics sammelt eine bedeutende Menge an numerischen Daten, die für Metriken geeignet wären, aber nicht im Azure-Metrikspeicher gespeichert sind.  Ein häufiges Beispiel sind aus Agents und Verwaltungslösungen gesammelte Leistungsdaten. Einige dieser Werte können in den Metrikspeicher kopiert werden, wo sie für Warnungen und Analysen mit dem Metrik-Explorer zur Verfügung stehen.

Erläuterungen zu diesem Feature finden Sie unter [Erstellen von Metrikwarnungen für Protokolle in Azure Monitor](../monitoring-and-diagnostics/monitoring-metric-alerts-logs.md). Die Liste der unterstützten Werte finden Sie unter [Unterstützte Metriken von Azure Monitor](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftoperationalinsightsworkspaces).

## <a name="stream-data-to-external-systems"></a>Streamen von Daten zu externen Systemen
Über die Verwendung der Tools in Azure für die Analyse von Überwachungsdaten hinaus besteht bei Ihnen möglicherweise die Anforderung, diese auch an ein externes Tool wie ein SIEM-Produkt (Security Information and Event Management) weiterzuleiten. Diese Weiterleitung erfolgt normalerweise mithilfe von [Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/) direkt von den überwachten Ressourcen aus. 

Weitere Informationen zu den verschiedenen Arten von Überwachungsdaten finden Sie unter [Streamen von Azure-Überwachungsdaten an einen Event Hub für die Verwendung durch ein externes Tool](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md).

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die für verschiedene Ressourcen in Azure [verfügbaren Überwachungsdaten](monitoring-data-sources.md).