---
title: Analysieren von Log Analytics-Daten in Azure Monitor | Microsoft-Dokumentation
description: Sie benötigen eine Protokollsuche zum Abrufen von Daten aus Log Analytics.  In diesem Artikel wird beschrieben, wie neue Protokollsuchen in Log Analytics verwendet werden. Er enthält Konzepte, die Ihnen vor dem Erstellen bekannt sein sollten.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/18/2018
ms.author: bwren
ms.component: ''
ms.openlocfilehash: 6e587f79f0f77446ea5a61cbb989f3f1c3655e0a
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/01/2018
ms.locfileid: "50741758"
---
# <a name="analyze-log-analytics-data-in-azure-monitor"></a>Analysieren von Log Analytics-Daten in Azure Monitor

Die von Azure Monitor gesammelten Protokolldaten werden in einem Log Analytics-Arbeitsbereich gespeichert, der auf dem [Azure Daten-Explorer](/data-explorer) basiert. Er sammelt Telemetriedaten aus einer Vielzahl von Quellen und verwendet zum Abrufen und Analysieren der Daten die [Abfragesprache des Daten-Explorers](/kusto).

> [!NOTE]
> Log Analytics wurde zuvor als eigener Dienst in Azure behandelt. Es wird jetzt als Bestandteil von Azure Monitor angesehen und legt den Schwerpunkt auf die Speicherung und Analyse von Protokolldaten mithilfe seiner Abfragesprache. Features, die als Teil von Log Analytics betrachtet wurden, wie etwa Windows- und Linux-Agents zur Datensammlung, Ansichten zum Visualisieren vorhandener Daten und Warnungen zur proaktiven Benachrichtigung von Benutzern über Probleme haben sich nicht geändert, werden aber jetzt als Teil von Azure Monitor angesehen.



## <a name="log-queries"></a>Protokollabfragen

Sie benötigen eine Protokollabfrage, um Daten beliebiger Art aus Log Analytics abzurufen.  Wenn Sie [Daten im Portal analysieren](log-analytics-log-search-portals.md), eine [Warnungsregel konfigurieren](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md), die Sie bei einer bestimmten Bedingung benachrichtigt, oder Daten mithilfe der [Log Analytics-API](https://dev.loganalytics.io/) abrufen, verwenden Sie jeweils eine Abfrage, um die gewünschten Daten anzugeben.  In diesem Artikel wird beschrieben, wie Protokollabfragen in Log Analytics verwendet werden. Er enthält Konzepte, die Ihnen vor dem Erstellen bekannt sein sollten.



## <a name="where-log-queries-are-used"></a>Orte, an denen Protokollabfragen verwendet werden

Dies sind einige der verschiedenen Möglichkeiten, wie Sie Abfragen in Log Analytics verwenden können:

- **Portale.** Sie können im [Azure-Portal](log-analytics-log-search-portals.md) eine interaktive Analyse von Protokolldaten durchführen.  Dadurch können Sie Ihre Abfrage bearbeiten und die Ergebnisse in einer Vielzahl von Formaten und Visualisierungen analysieren.  
- **Warnungsregeln.** [Warnungsregeln](log-analytics-alerts.md) identifizieren proaktiv Probleme durch die Daten in Ihrem Arbeitsbereich.  Jede Warnungsregel basiert auf einer Protokollsuche, die in regelmäßigen Abständen automatisch ausgeführt wird.  Die Ergebnisse werden überprüft, um zu ermitteln, ob eine Warnung erstellt werden soll.
- **Dashboards.** Sie können die Ergebnisse beliebiger Abfragen in einem [Azure-Dashboard]() anheften, was es Ihnen ermöglicht, Protokoll- und Metrikdaten gemeinsam zu visualisieren und optional mit anderen Azure-Benutzern zu teilen. 
- **Ansichten.**  Sie können mit [Ansicht-Designer](log-analytics-view-designer.md) Visualisierungen von Daten erstellen, die in Benutzerdashboards einbezogen werden sollen.  Protokollabfragen stellen die von [Kacheln](log-analytics-view-designer-tiles.md) und [Visualisierungsparts](log-analytics-view-designer-parts.md) in jeder Ansicht verwendeten Daten bereit.  
- **Export:**  Wenn Sie Daten aus dem Log Analytics-Arbeitsbereich nach Excel oder [Power BI](log-analytics-powerbi.md) importieren, erstellen Sie eine Protokollabfrage, um die zu exportierenden Daten zu definieren.
- **PowerShell.** Sie können ein PowerShell-Skript über eine Befehlszeile oder ein Azure Automation-Runbook ausführen, das Daten mithilfe von [Get-AzureRmOperationalInsightsSearchResults](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/get-azurermoperationalinsightssearchresults?view=azurermps-4.0.0) von Log Analytics abruft.  Dieses Cmdlet erfordert eine Abfrage, um die abzurufenden Daten festzulegen.
- **Log Analytics-API.**  Die [Log Analytics-Protokollsuch-API](log-analytics-log-search-api.md) ermöglicht einem beliebigen REST-API-Client, Protokolldaten aus einem Arbeitsbereich abzurufen.  Die API-Anforderung enthält eine Abfrage, die für Log Analytics ausgeführt wird, um die abzurufenden Daten zu ermitteln.

![Protokollsuchvorgänge](media/log-analytics-queries/queries-overview.png)

## <a name="write-a-query"></a>Schreiben Sie eine Abfrage.
Log Analytics verwendet [eine Version der Daten-Explorer-Abfragesprache](query-language/get-started-queries.md), um Protokolldaten auf vielfältige Weise abzurufen und zu analysieren.  Sie beginnen in der Regel mit grundlegenden Abfragen und arbeiten sich dann zu erweiterten Funktionen weiter, wenn Ihre Anforderungen komplexer werden.

Die Grundstruktur einer Abfrage ist eine Quelltabelle, gefolgt von einer Reihe von Operatoren, die durch einen senkrechten Strich (`|`) getrennt sind.  Sie können mehrere Operatoren verketten, um die Daten einzugrenzen und erweiterte Funktionen auszuführen.

Beispiel: Sie möchten die zehn Computer mit den meisten Fehlerereignissen über den letzten Tag suchen.

```Kusto
Event
| where (EventLevelName == "Error")
| where (TimeGenerated > ago(1days))
| summarize ErrorCount = count() by Computer
| top 10 by ErrorCount desc
```

Oder Sie möchten Computer suchen, die über den letzten Tag keinen Takt aufwiesen.

```Kusto
Heartbeat
| where TimeGenerated > ago(7d)
| summarize max(TimeGenerated) by Computer
| where max_TimeGenerated < ago(1d)  
```

Ein anderes Beispiel ist ein Liniendiagramm mit der Prozessorauslastung für jeden Computer in der letzten Woche.

```Kusto
Perf
| where ObjectName == "Processor" and CounterName == "% Processor Time"
| where TimeGenerated  between (startofweek(ago(7d)) .. endofweek(ago(7d)) )
| summarize avg(CounterValue) by Computer, bin(TimeGenerated, 5min)
| render timechart    
```

Durch diese kurzen Beispiele können Sie sehen, dass unabhängig von der Art der Daten, die denen Sie arbeiten, die Struktur der Abfrage ähnlich ist.  Sie können sie in abgegrenzte Schritte unterteilen, bei denen die resultierenden Daten eines Befehls über die Pipeline an den nächsten Befehl gesendet werden.

Sie können Daten auch Log Analytics-Arbeitsbereiche übergreifend in Ihrem Abonnement abfragen.

```Kusto
union Update, workspace("contoso-workspace").Update
| where TimeGenerated >= ago(1h)
| summarize dcount(Computer) by Classification 
```

## <a name="how-log-analytics-data-is-organized"></a>Aufbau von Log Analytics-Daten
Wenn Sie eine Abfrage erstellen, legen Sie zuerst fest, welche Tabellen die gesuchten Daten aufweisen. Verschiedene Arten von Daten werden in jedem [Log Analytics-Arbeitsbereich](log-analytics-quick-create-workspace.md) auf dedizierte Tabellen aufgeteilt.  Die Dokumentation für die verschiedenen Datenquellen enthält den Namen des Datentyps, der von der einzelnen Datenquelle erstellt wird, und eine Beschreibung jeder seiner einzelnen Eigenschaften.  Für viele Abfragen werden nur Daten aus einer einzelnen Tabelle benötigt, andere greifen jedoch möglicherweise auf verschiedene Optionen zurück, um Daten aus mehreren Tabellen einzuschließen.

Zwar speichert [Application Insights](../application-insights/app-insights-overview.md) Anwendungsdaten wie Anforderungen, Ausnahmen, Ablaufverfolgungen und Nutzung in Log Analytics, sie werden aber auf einer anderen Partition als die anderen Protokolldaten gespeichert. Sie können für den Zugriff auf diese Daten die gleiche Abfragesprache verwenden, müssen aber für den Zugriff die [Application Insights-Konsole](../application-insights/app-insights-analytics.md) oder die [Application Insights-REST-API](https://dev.applicationinsights.io/) verwenden. Sie können [ressourcenübergreifende Abfragen](log-analytics-cross-workspace-search.md) verwenden, um Daten aus Application Insights mit anderen Daten in Log Analytics zu kombinieren.


![Tabellen](media/log-analytics-queries/queries-tables.png)







## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [Portale, die Sie zum Erstellen und Bearbeiten von Protokollsuchen verwenden](log-analytics-log-search-portals.md).
- Sehen Sie sich ein [Tutorial zum Schreiben von Abfragen](log-analytics-tutorial-viewdata.md) mit der neuen Abfragesprache an.
