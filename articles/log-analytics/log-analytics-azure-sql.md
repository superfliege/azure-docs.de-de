---
title: Azure SQL Analytics-Lösung in Log Analytics | Microsoft-Dokumentation
description: Die Azure SQL-Analyse-Lösung hilft Ihnen bei der Verwaltung Ihrer Azure SQL-Datenbanken.
services: log-analytics
documentationcenter: ''
author: danimir
manager: carmonm
ms.reviewer: carlrab
ms.assetid: b2712749-1ded-40c4-b211-abc51cc65171
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/03/2018
ms.author: v-daljep
ms.component: na
ms.openlocfilehash: 47069f0af7409d87cb2d4fbbbce9dda0b1c2056e
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/24/2018
ms.locfileid: "42886559"
---
# <a name="monitor-azure-sql-databases-using-azure-sql-analytics-preview"></a>Überwachen von Azure SQL-Datenbanken mithilfe von Azure SQL-Analyse (Vorschau)

![Symbol Azure SQL Analytics](./media/log-analytics-azure-sql/azure-sql-symbol.png)

Azure SQL-Analyse ist eine Cloudüberwachungslösung zum bedarfsorientierten Überwachen der Leistung von Azure SQL-Datenbanken für mehrere Pools für elastische Datenbanken und Abonnements. Die Lösung erfasst und visualisiert wichtige Azure SQL-Datenbank-Leistungsmetriken und umfasst zusätzlich integrierte Logik zur Problembehandlung der Leistung. 

Anhand von mit der Lösung erfassten Metriken können Sie benutzerdefinierte Überwachungsregeln und -warnungen erstellen. Die Lösung hilft Ihnen beim Erkennen von Problemen auf jeder Ebene Ihres Anwendungsstapels. Sie verwendet Azure-Diagnosemetriken zusammen mit Log Analytics-Ansichten, um Daten zu allen Azure SQL-Datenbanken und Pools für elastische Datenbanken in einem einzelnen Log Analytics-Arbeitsbereich darzustellen. Log Analytics unterstützt Sie beim Erfassen, Korrelieren und Visualisieren strukturierter und nicht strukturierter Daten.

Zurzeit unterstützt diese Vorschaulösung bis zu 150.000 Azure SQL-Datenbanken und 5.000 Pools für elastische SQL-Datenbanken pro Arbeitsbereich.

Einen Überblick über die praktische Verwendung der Azure SQL-Analyse und über typische Verwendungsszenarien finden Sie im eingebetteten Video:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Get-Intelligent-Insights-for-Improving-Azure-SQL-Database-Performance/player]
>

## <a name="connected-sources"></a>Verbundene Quellen

Azure SQL-Analyse ist eine Cloudüberwachungslösung, die das Streaming von Diagnosetelemetriedaten für Azure SQL-Datenbanken und Pools für elastische Datenbanken unterstützt. Da keine Agents für die Verbindung mit dem Log Analytics-Dienst verwendet werden, unterstützt die Lösung keine Verbindung mit Windows-, Linux- oder SCOM-Ressourcen. Entsprechende Informationen finden Sie in der folgenden Tabelle zur Kompatibilität.

| Verbundene Quelle | Support | BESCHREIBUNG |
| --- | --- | --- |
| **[Azure-Diagnose](log-analytics-azure-storage.md)** | **Ja** | Metrik- und Protokolldaten von Azure werden direkt von Azure an Log Analytics gesendet. |
| [Azure-Speicherkonto](log-analytics-azure-storage.md) | Nein  | Log Analytics liest keine Daten aus einem Speicherkonto. |
| [Windows-Agents](log-analytics-windows-agent.md) | Nein  | Direkte Windows-Agents werden von der Lösung nicht verwendet. |
| [Linux-Agents](log-analytics-linux-agents.md) | Nein  | Direkte Linux-Agents werden von der Lösung nicht verwendet. |
| [SCOM-Verwaltungsgruppe](log-analytics-om-agents.md) | Nein  | Von der Lösung wird keine direkte Verbindung zwischen SCOM-Agent und Log Analytics verwendet. |

## <a name="configuration"></a>Konfiguration

Führen Sie die folgenden Schritte aus, um die Azure SQL Analytics-Lösung Ihrem Arbeitsbereich hinzuzufügen.

1. Fügen Sie die Azure SQL-Analyse-Lösung aus dem [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureSQLAnalyticsOMS?tab=Overview) Ihrem Arbeitsbereich hinzu.
2. Klicken Sie im Azure-Portal auf **+ Ressource erstellen**, und suchen Sie dann nach **Azure SQL-Analyse**.  
    ![Überwachung und Verwaltung](./media/log-analytics-azure-sql/monitoring-management.png)
3. Wählen Sie **Azure SQL-Analyse (Vorschau)** in der Liste aus.
4. Klicken Sie im Bereich **Azure SQL Analytics (Vorschau)** auf **Erstellen**.  
    ![Erstellen](./media/log-analytics-azure-sql/portal-create.png)
5. Erstellen Sie im Bereich **Neue Lösung erstellen** eine neue Lösung, oder wählen Sie einen vorhandenen Arbeitsbereich aus, dem Sie die Lösung hinzufügen möchten, und klicken Sie dann auf **Erstellen**.  
    ![Zu Arbeitsbereich hinzufügen](./media/log-analytics-azure-sql/add-to-workspace.png)

### <a name="configure-azure-sql-databases-and-elastic-pools-to-stream-diagnostics-telemetry"></a>Konfigurieren von Azure SQL-Datenbanken und Pools für elastische Datenbanken zum Streamen von Diagnosetelemetriedaten

Nachdem Sie die Azure SQL-Analyse-Lösung in Ihrem Arbeitsbereich erstellt haben, müssen Sie zum Überwachen der Leistung von Azure SQL-Datenbanken und/oder Pools für elastische Datenbanken **jede** zu überwachende Ressource von Azure SQL-Datenbanken und Pools für elastische Datenbanken so konfigurieren, dass die jeweils zugehörigen Diagnosetelemetriedaten an die Lösung gestreamt werden.

- Aktivieren Sie die Azure-Diagnose für Ihre Azure SQL-Datenbanken und Pools für elastische Datenbanken, und [konfigurieren Sie diese so, dass ihre Daten an Log Analytics gesendet werden](../sql-database/sql-database-metrics-diag-logging.md).

### <a name="to-configure-multiple-azure-subscriptions"></a>So konfigurieren Sie mehrere Azure-Abonnements

Wenn mehrere Abonnements unterstützt werden, verwenden Sie das PowerShell-Skript aus [Enable Azure resource metrics logging using PowerShell](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/) (Aktivieren der Protokollierung der Azure-Ressourcenmetriken mit PowerShell). Geben Sie die Ressourcen-ID des Arbeitsbereichs als Parameter beim Ausführen des Skripts zum Senden von Diagnosedaten aus Ressourcen in einem Azure-Abonnement an einen Arbeitsbereich in einem anderen Azure-Abonnement an.

**Beispiel**

```
PS C:\> $WSID = "/subscriptions/<subID>/resourcegroups/oms/providers/microsoft.operationalinsights/workspaces/omsws"
```

```
PS C:\> .\Enable-AzureRMDiagnostics.ps1 -WSID $WSID
```

## <a name="using-the-solution"></a>Verwenden der Lösung

Wenn Sie die Lösung zu Ihrem Arbeitsbereich hinzufügen, wird die Kachel „Azure SQL Analytics“ zu Ihrem Arbeitsbereich hinzugefügt und erscheint in der Übersicht. Die Kachel zeigt die Anzahl der Azure SQL-Datenbanken und der Pools für elastische Azure SQL-Datenbanken, mit denen die Lösung verbunden ist.

![Kachel „Azure SQL Analytics“](./media/log-analytics-azure-sql/azure-sql-sol-tile.png)

### <a name="viewing-azure-sql-analytics-data"></a>Anzeigen von Azure SQL Analytics-Daten

Klicken Sie auf die Kachel **Azure SQL Analytics**, um das Dashboard von Azure SQL Analytics zu öffnen. Das Dashboard umfasst die Übersicht über alle Datenbanken, die aus verschiedenen Perspektiven überwacht werden. Damit verschiedene Perspektiven funktionieren, müssen Sie die richtigen Metriken oder Protokolle auf Ihren SQL-Ressourcen aktivieren, die dann an den Azure Log Analytics-Arbeitsbereich gestreamt werden.

![Azure SQL-Analyse – Übersicht](./media/log-analytics-azure-sql/azure-sql-sol-overview.png)

Die Auswahl einer Kachel öffnet einen Drilldown-Bericht zur entsprechenden Perspektive. Nach Auswahl der Perspektive wird der Drilldownbericht geöffnet.

![Azure SQL-Analyse – Zeitlimits](./media/log-analytics-azure-sql/azure-sql-sol-metrics.png)

Jede Perspektive bietet Zusammenfassungen zum Abonnement, Server, Pool für elastische Datenbanken und zur Datenbankebene. Darüber hinaus zeigt jede Perspektive auf der rechten Seite eine Perspektive an, die für den Bericht spezifisch ist. Durch die Auswahl eines Abonnements, Servers, Anwendungspools oder einer Datenbank aus der Liste wird der Drilldown fortgesetzt.

| Perspektive | BESCHREIBUNG |
| --- | --- |
| Ressource nach Typ | Perspektive, die alle überwachten Ressourcen zählt. Der Drilldown enthält die Zusammenfassung von DTU- und GB- Metriken. |
| Einblicke | Stellt den hierarchischen Drilldown in Intelligent Insights bereit. Weitere Informationen zu Intelligent Insights. |
| Errors | Stellt den hierarchischen Drilldown in SQL-Fehler bereit, die in den Datenbanken aufgetreten sind. |
| Zeitlimits | Stellt den hierarchischen Drilldown in SQL-Zeitlimits bereit, die in den Datenbanken aufgetreten sind. |
| Blockierungen | Stellt den hierarchischen Drilldown in SQL-Blockierungen bereit, die in den Datenbanken aufgetreten sind. |
| Datenbankwartevorgänge | Stellt den hierarchischen Drilldown in SQL-Wartestatistiken auf Datenbankebene bereit. Enthält Zusammenfassungen der gesamten Wartezeit sowie die Wartezeit pro Wartetyp. |
| Abfragedauer | Stellt den hierarchischen Drilldown in die Statistiken zur Abfrageausführung bereit, z.B. Abfragedauer, CPU-Auslastung, Daten-E/A-Auslastung und Protokoll-E/A-Auslastung. |
| Abfragewartevorgänge | Stellt den hierarchischen Drilldown in die Statistiken zu Abfragewartevorgängen nach Wartekategorie bereit. |

### <a name="intelligent-insights-report"></a>Intelligent Insights-Bericht

[Intelligent Insights](../sql-database/sql-database-intelligent-insights.md) von Azure SQL-Datenbank informiert Sie über die Leistung Ihrer Datenbanken. Alle erfassten Intelligent Insights-Daten können visuell dargestellt und über die Insights Perspektive aufgerufen werden.

![Azure SQL-Analyse – Insights](./media/log-analytics-azure-sql/azure-sql-sol-insights.png)

### <a name="elastic-pool-and-database-reports"></a>Berichte zu Pools für elastische Datenbanken und Datenbanken

Sowohl Pools für elastische Datenbanken als auch Datenbanken verfügen über eigene bestimmte Berichte, die alle Daten anzeigen, die für die Ressource im angegebenen Zeitraum gesammelt werden.

![Azure SQL-Analyse –Datenbank](./media/log-analytics-azure-sql/azure-sql-sol-database.png)

![Azure SQL-Analyse – Pool für elastische Datenbanken](./media/log-analytics-azure-sql/azure-sql-sol-pool.png)

### <a name="query-reports"></a>Abfrageberichte

Durch die Abfragedauer- und Abfragewartevorgänge-Perspektive können Sie die Leistung von Abfragen über den Abfragebericht korrelieren. Dieser Bericht vergleicht die Abfrageleistung in verschiedenen Datenbanken und erleichtert das Ermitteln von Datenbanken, die die ausgewählte Abfrage im Vergleich zu langsamen Datenbanken ordnungsgemäß ausführen.

![Azure SQL-Analyse – Abfragen](./media/log-analytics-azure-sql/azure-sql-sol-queries.png)

### <a name="analyze-data-and-create-alerts"></a>Analysieren von Daten und Erstellen von Warnungen

Sie können problemlos [Warnungen](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md) mit den Daten erstellen, die aus Azure SQL-Datenbank-Ressourcen stammen. Hier sind einige nützliche Abfragen für die [Protokollsuche](log-analytics-log-searches.md), die Sie mit einer Protokollwarnung verwenden können:

*Hohe CPU in Azure SQL-Datenbank*

```
AzureMetrics 
| where ResourceProvider=="MICROSOFT.SQL"
| where ResourceId contains "/DATABASES/"
| where MetricName=="cpu_percent" 
| summarize AggregatedValue = max(Maximum) by bin(TimeGenerated, 5m)
| render timechart
```

> [!NOTE]
> - Voraussetzung für die Einrichtung dieser Warnung ist, dass die überwachten Datenbanken Diagnosemetriken (Option „Alle Metriken“) an die Lösung streamen.
> - Ersetzen Sie den MetricName-Wert „cpu_percent“ durch „dtu_consumption_percent“ um stattdessen hohe DTU-Ergebnisse zu erhalten.

*Hohe CPU in Pools für elastische Datenbanken in Azure SQL-Datenbank*

```
AzureMetrics 
| where ResourceProvider=="MICROSOFT.SQL"
| where ResourceId contains "/ELASTICPOOLS/"
| where MetricName=="cpu_percent" 
| summarize AggregatedValue = max(Maximum) by bin(TimeGenerated, 5m)
| render timechart
```

> [!NOTE]
> - Voraussetzung für die Einrichtung dieser Warnung ist, dass überwachte Datenbanken Diagnosemetriken (Option „Alle Metriken“) an die Lösung streamen.
> - Ersetzen Sie den MetricName-Wert „cpu_percent“ durch „dtu_consumption_percent“ um stattdessen hohe DTU-Ergebnisse zu erhalten.

*Azure SQL-Datenbankspeicher im Durchschnitt über 95% in den letzten Stunde*

```
let time_range = 1h;
let storage_threshold = 95;
AzureMetrics
| where ResourceId contains "/DATABASES/"
| where MetricName == "storage_percent"
| summarize max_storage = max(Average) by ResourceId, bin(TimeGenerated, time_range)
| where max_storage > storage_threshold
| distinct ResourceId
```

> [!NOTE]
> - Voraussetzung für die Einrichtung dieser Warnung ist, dass überwachte Datenbanken Diagnosemetriken (Option „Alle Metriken“) an die Lösung streamen.
> - Diese Abfrage erfordert die Einrichtung einer Warnungsregel, die eine Warnung auslöst, wenn Ergebnisse (> 0 Ergebnisse) von der Abfrage vorhanden sind, die angeben, dass die Bedingung in einigen Datenbanken vorhanden ist. Die Ausgabe ist eine Liste von Datenbankressourcen, die im definierten Zeitbereich den Speicherschwellenwert überschreiten.
> - Die Ausgabe ist eine Liste von Datenbankressourcen, die im definierten Zeitbereich den Speicherschwellenwert überschreiten.

*Warnung in Intelligent Insights*

```
let alert_run_interval = 1h;
let insights_string = "hitting its CPU limits";
AzureDiagnostics
| where Category == "SQLInsights" and status_s == "Active" 
| where TimeGenerated > ago(alert_run_interval)
| where rootCauseAnalysis_s contains insights_string
| distinct ResourceId
```

> [!NOTE]
> - Voraussetzung für die Einrichtung dieser Warnung ist, dass überwachte Datenbanken ein SQLInsights-Diagnoseprotokoll an die Lösung streamen.
> - Diese Abfrage erfordert die Einrichtung einer Warnungsregel, die mit der gleichen Häufigkeit wie „alert_run_interval“ ausgeführt wird, um Duplikate zu vermeiden. Die Regel muss für das Auslösen der Warnung eingerichtet werden, wenn Ergebnisse (> 0 Ergebnisse) von der Abfrage vorhanden sind.
> - Passen Sie den Wert „alert_run_interval“ an, um den Zeitbereich festzulegen, für den überprüft wird, ob die Bedingung in Datenbanken aufgetreten ist, die zum Streamen des SQLInsights-Protokolls an die Lösung konfiguriert sind.
> - Passen Sie „insights_string“ so an, dass die Ausgabe des Texts der Insights-Analyse der Grundursache erfasst wird. Der gleiche Text wird auf der Benutzeroberfläche der Lösung angezeigt, die sie in den vorhandenen Erkenntnissen nutzen können. Alternativ können Sie die folgende Abfrage verwenden, um den Text aller in Ihrem Abonnement generierten Einblicke anzuzeigen. Verwenden Sie die Ausgabe der Abfrage, um die unterschiedlichen Zeichenfolgen für die Einrichtung von Warnungen für Insights zu nutzen.

```
AzureDiagnostics
| where Category == "SQLInsights" and status_s == "Active" 
| distinct rootCauseAnalysis_s
```

## <a name="next-steps"></a>Nächste Schritte

- Verwenden Sie die [Protokollsuche](log-analytics-log-searches.md) in Log Analytics, um ausführliche Azure SQL-Daten anzuzeigen.
- [Erstellen eigener Dashboards](log-analytics-dashboards.md), die Azure SQL-Daten anzeigen
- [Erstellen von Warnungen](log-analytics-alerts.md), wenn bestimmte Azure SQL-Ereignisse auftreten
