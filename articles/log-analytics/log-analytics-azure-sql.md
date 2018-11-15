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
ms.component: ''
ms.openlocfilehash: 5eb398cf981edbcfe9b98ee89c3dd4e12b3f758f
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2018
ms.locfileid: "51615492"
---
# <a name="monitor-azure-sql-database-using-azure-sql-analytics-preview"></a>Überwachen von Azure SQL-Datenbank mithilfe von Azure SQL-Analyse (Vorschauversion)

![Symbol Azure SQL Analytics](./media/log-analytics-azure-sql/azure-sql-symbol.png)

Azure SQL-Analyse ist eine Cloudüberwachungslösung zum bedarfsorientierten und abonnementübergreifenden Überwachen der Leistung von Azure SQL-Datenbanken, Pools für elastische Datenbanken und verwalteten Instanzen über eine zentrale Oberfläche. Die Lösung erfasst und visualisiert wichtige Azure SQL-Datenbank-Leistungsmetriken und umfasst integrierte Intelligenz für die Problembehandlung der Leistung.

Anhand von mit der Lösung erfassten Metriken können Sie benutzerdefinierte Überwachungsregeln und -warnungen erstellen. Die Lösung hilft Ihnen beim Erkennen von Problemen auf jeder Ebene Ihres Anwendungsstapels. Sie verwendet Azure-Diagnosemetriken zusammen mit Log Analytics-Ansichten, um Daten zu allen Azure SQL-Datenbanken, Pools für elastische Datenbanken und verwalteten Instanzen in einem einzigen Log Analytics-Arbeitsbereich darzustellen. Log Analytics unterstützt Sie beim Erfassen, Korrelieren und Visualisieren strukturierter und nicht strukturierter Daten.

Einen Überblick über die praktische Verwendung der Azure SQL-Analyse und über typische Verwendungsszenarien finden Sie im eingebetteten Video:


> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Get-Intelligent-Insights-for-Improving-Azure-SQL-Database-Performance/player]
>

## <a name="connected-sources"></a>Verbundene Quellen

Azure SQL-Analyse ist eine Cloudüberwachungslösung, die das Streaming von Diagnosetelemetriedaten für Azure SQL-Datenbank, verwaltete Instanzen und Pools für elastische Datenbanken unterstützt.

Da die Lösung keine Agents für die Verbindung mit dem Log Analytics-Dienst verwendet, wird die Überwachung von lokal oder auf virtuellen Computern gehosteten SQL Server-Instanzen nicht unterstützt. Entsprechende Informationen finden Sie in der folgenden Kompatibilitätstabelle.

| Verbundene Quelle | Unterstützt | BESCHREIBUNG |
| --- | --- | --- |
| **[Azure-Diagnose](log-analytics-azure-storage.md)** | **Ja** | Metrik- und Protokolldaten von Azure werden direkt von Azure an Log Analytics gesendet. |
| [Azure-Speicherkonto](log-analytics-azure-storage.md) | Nein  | Log Analytics liest keine Daten aus einem Speicherkonto. |
| [Windows-Agents](log-analytics-agent-windows.md) | Nein  | Direkte Windows-Agents werden von der Lösung nicht verwendet. |
| [Linux-Agents](log-analytics-quick-collect-linux-computer.md) | Nein  | Direkte Linux-Agents werden von der Lösung nicht verwendet. |
| [SCOM-Verwaltungsgruppe](log-analytics-om-agents.md) | Nein  | Von der Lösung wird keine direkte Verbindung zwischen SCOM-Agent und Log Analytics verwendet. |

## <a name="configuration"></a>Konfiguration

Führen Sie die folgenden Schritte aus, um die Azure SQL Analyselösung Ihrem Azure-Dashboard hinzuzufügen.

1. Fügen Sie die Azure SQL-Analyse-Lösung aus dem [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureSQLAnalyticsOMS?tab=Overview) Ihrem Arbeitsbereich hinzu.
2. Klicken Sie im Azure-Portal auf **+ Ressource erstellen**, und suchen Sie dann nach **Azure SQL-Analyse**.  
    ![Überwachung und Verwaltung](./media/log-analytics-azure-sql/monitoring-management.png)
3. Wählen Sie **Azure SQL-Analyse (Vorschau)** in der Liste aus.
4. Klicken Sie im Bereich **Azure SQL Analytics (Vorschau)** auf **Erstellen**.  
    ![Erstellen](./media/log-analytics-azure-sql/portal-create.png)
5. Erstellen Sie im Bereich **Neue Lösung erstellen** eine neue Lösung, oder wählen Sie einen vorhandenen Arbeitsbereich aus, dem Sie die Lösung hinzufügen möchten, und klicken Sie dann auf **Erstellen**.

    ![Zu Arbeitsbereich hinzufügen](./media/log-analytics-azure-sql/add-to-workspace.png)

### <a name="configure-azure-sql-databases-elastic-pools-and-managed-instances-to-stream-diagnostics-telemetry"></a>Konfigurieren von Azure SQL-Datenbanken, Pools für elastische Datenbanken und verwalteten Instanzen zum Streamen von Diagnosetelemetriedaten

Nachdem Sie die Azure SQL-Analyse-Lösung in Ihrem Arbeitsbereich erstellt haben, müssen Sie zum Überwachen der Leistung von Azure SQL-Datenbanken, Datenbanken in verwalteten Instanzen und Pools für elastische Datenbanken **jede** zu überwachende Ressource so konfigurieren, dass die jeweiligen Diagnosetelemetriedaten an die Lösung gestreamt werden. Befolgen Sie dazu die detaillierten Anweisungen auf dieser Seite:

- Aktivieren Sie die Azure-Diagnose für Azure SQL-Datenbank, Datenbanken der verwalteten Instanz und Pools für elastische Datenbanken, um [Diagnosetelemetriedaten an die Azure SQL-Analyse zu streamen](../sql-database/sql-database-metrics-diag-logging.md).

Die oben abgebildete Seite enthält auch Anweisungen zum Aktivieren der Unterstützung für die Überwachung mehrerer Azure-Abonnements über einen einzelnen Azure SQL-Analysearbeitsbereich als zentrale Konsole.

## <a name="using-the-solution"></a>Verwenden der Lösung

Wenn Sie die Lösung zu Ihrem Arbeitsbereich hinzufügen, wird die Kachel „Azure SQL Analytics“ zu Ihrem Arbeitsbereich hinzugefügt und erscheint in der Übersicht. Die Kachel zeigt die Anzahl von Azure SQL-Datenbanken, Pools für elastische Datenbanken, verwaltete Instanzen und Datenbanken in verwalteten Instanzen an, von denen die Lösung Diagnosetelemetriedaten empfängt.

![Kachel „Azure SQL Analytics“](./media/log-analytics-azure-sql/azure-sql-sol-tile.png)

Die Lösung bietet zwei separate Ansichten: eine für die Überwachung von Azure SQL-Datenbanken und Pools für elastische Datenbanken, und eine weitere für die Überwachung der verwalteten Instanz und Datenbanken in verwalteten Instanzen.

Zum Anzeigen des Überwachungsdashboards der Azure SQL-Analyse für Azure SQL-Datenbanken und Pool für elastische Datenbanken klicken Sie in den oberen Bereich der Kachel. Zum Anzeigen des Überwachungsdashboards der Azure SQL-Analyse für Azure SQL-Datenbanken und Pool für elastische Datenbanken klicken Sie in den oberen Bereich der Kachel.

### <a name="viewing-azure-sql-analytics-data"></a>Anzeigen von Azure SQL Analytics-Daten

Das Dashboard umfasst die Übersicht über alle Datenbanken, die aus verschiedenen Perspektiven überwacht werden. Damit verschiedene Perspektiven funktionieren, müssen Sie die richtigen Metriken oder Protokolle auf Ihren SQL-Ressourcen aktivieren, die dann an den Azure Log Analytics-Arbeitsbereich gestreamt werden.

Hinweis: Wenn einige Metriken oder Protokolle nicht in Azure Log Analytics gestreamt werden, werden die Kacheln in der Lösung nicht mit Überwachungsinformationen gefüllt.

### <a name="azure-sql-database-and-elastic-pool-view"></a>Ansicht für Azure SQL-Datenbank und Pools für elastische Datenbanken

Sobald die Kachel „Azure SQL-Analyse“ für Azure SQL-Datenbank und Pools für elastische Datenbanken ausgewählt ist, wird das Überwachungsdashboard angezeigt.

![Azure SQL-Analyse – Übersicht](./media/log-analytics-azure-sql/azure-sql-sol-overview.png)

Die Auswahl einer Kachel öffnet einen Drilldown-Bericht zur entsprechenden Perspektive. Nach Auswahl der Perspektive wird der Drilldownbericht geöffnet.

![Azure SQL-Analyse – Zeitlimits](./media/log-analytics-azure-sql/azure-sql-sol-metrics.png)

Jede Perspektive in dieser Ansicht bietet Zusammenfassungen zum Abonnement, Server, Pool für elastische Datenbanken und zur Datenbankebene. Darüber hinaus zeigt jede Perspektive auf der rechten Seite eine Perspektive an, die für den Bericht spezifisch ist. Durch die Auswahl eines Abonnements, Servers, Anwendungspools oder einer Datenbank aus der Liste wird der Drilldown fortgesetzt.

### <a name="managed-instance-and-databases-in-managed-instance-view"></a>Ansicht für eine verwaltete Instanz und Datenbanken in einer verwalteten Instanz

Sobald die Kachel „Azure SQL-Analyse“ für verwaltete Instanzen und Datenbanken in verwalteten Instanzen ausgewählt ist, wird das Überwachungsdashboard angezeigt.

![Azure SQL-Analyse – Übersicht](./media/log-analytics-azure-sql/azure-sql-sol-overview-mi.png)

Die Auswahl einer Kachel öffnet einen Drilldown-Bericht zur entsprechenden Perspektive. Nach Auswahl der Perspektive wird der Drilldownbericht geöffnet.

Die Auswahl der Kachel „Verwaltete Instanz“ zeigt Details zur Auslastung der verwalteten Instanz, darin enthaltene Datenbanken und Telemetriedaten zu den für die gesamte Instanz ausgeführten Abfragen.

![Azure SQL-Analyse – Zeitlimits](./media/log-analytics-azure-sql/azure-sql-sol-metrics-mi.png)

### <a name="perspectives"></a>Perspektiven

In der folgenden Tabelle werden Perspektiven erläutert, die für zwei Versionen des im Dashboards unterstützt werden: eine für Azure SQL-Datenbank und Pools für elastische Datenbanken, und die andere für eine verwaltete Instanz.

| Perspektive | BESCHREIBUNG | Unterstützung für SQL-Datenbank und Pools für elastische Datenbanken | Unterstützung für die verwaltete Instanz |
| --- | ------- | ----- | ----- |
| Ressource nach Typ | Perspektive, die alle überwachten Ressourcen zählt. | JA | JA | 
| Einblicke | Stellt einen hierarchischen Drilldown in die Leistung in Intelligent Insights bereit. | JA | JA |
| Errors | Stellt den hierarchischen Drilldown in SQL-Fehler bereit, die in den Datenbanken aufgetreten sind. | JA | JA |
| Zeitlimits | Stellt den hierarchischen Drilldown in SQL-Zeitlimits bereit, die in den Datenbanken aufgetreten sind. | JA | Nein  |
| Blockierungen | Stellt den hierarchischen Drilldown in SQL-Blockierungen bereit, die in den Datenbanken aufgetreten sind. | JA | Nein  |
| Datenbankwartevorgänge | Stellt den hierarchischen Drilldown in SQL-Wartestatistiken auf Datenbankebene bereit. Enthält Zusammenfassungen der gesamten Wartezeit sowie die Wartezeit pro Wartetyp. |JA | JA |
| Abfragedauer | Stellt den hierarchischen Drilldown in die Statistiken zur Abfrageausführung bereit, z.B. Abfragedauer, CPU-Auslastung, Daten-E/A-Auslastung und Protokoll-E/A-Auslastung. | JA | JA |
| Abfragewartevorgänge | Stellt den hierarchischen Drilldown in die Statistiken zu Abfragewartevorgängen nach Wartekategorie bereit. | JA | JA |

### <a name="intelligent-insights-report"></a>Intelligent Insights-Bericht

[Intelligent Insights](../sql-database/sql-database-intelligent-insights.md) von Azure SQL-Datenbank informiert Sie über die Leistung Ihrer Azure SQL-Datenbanken und Datenbanken in verwalteten Instanzen. Alle erfassten Intelligent Insights-Daten können visuell dargestellt und über die Insights Perspektive aufgerufen werden.

![Azure SQL-Analyse – Insights](./media/log-analytics-azure-sql/azure-sql-sol-insights.png)

### <a name="elastic-pool-and-database-reports"></a>Berichte zu Pools für elastische Datenbanken und Datenbanken

Sowohl Pools für elastische Datenbanken als auch SQL-Datenbanken verfügen über eigene bestimmte Berichte, die alle Daten anzeigen, die für die Ressource im angegebenen Zeitraum gesammelt werden.

![Azure SQL-Analyse –Datenbank](./media/log-analytics-azure-sql/azure-sql-sol-database.png)

![Pool für elastische Azure SQL-Datenbanken](./media/log-analytics-azure-sql/azure-sql-sol-pool.png)

### <a name="query-reports"></a>Abfrageberichte

Durch die Abfragedauer- und Abfragewartevorgänge-Perspektive können Sie die Leistung von Abfragen über den Abfragebericht korrelieren. Dieser Bericht vergleicht die Abfrageleistung in verschiedenen Datenbanken und erleichtert das Ermitteln von Datenbanken, die die ausgewählte Abfrage im Vergleich zu langsamen Datenbanken ordnungsgemäß ausführen.

![Azure SQL-Analyse – Abfragen](./media/log-analytics-azure-sql/azure-sql-sol-queries.png)

## <a name="permissions"></a>Berechtigungen

Für die Verwendung von Azure SQL-Analyse benötigen Benutzer mindestens die Rolle „Leser“ in Azure. Diese Rolle erlaubt es Benutzern jedoch nicht, den Abfragetext anzuzeigen oder automatische Optimierungsaktionen durchzuführen. Rollen mit mehr Berechtigungen in Azure, die die Nutzung der Lösung in vollem Umfang ermöglichen, sind „Besitzer“, „Mitwirkender“, „SQL-DB-Mitwirkender“ und „SQL Server-Mitwirkender“. Sie sollten auch erwägen, im Portal eine benutzerdefinierte Rolle mit spezifischen Berechtigungen zu erstellen, die nur erforderlich sind, um Azure SQL-Analyse zu verwenden, aber keinen Zugriff auf die Verwaltung anderer Ressourcen bieten.

### <a name="creating-a-custom-role-in-portal"></a>Erstellen einer benutzerdefinierten Rolle im Portal

Da einige Organisationen strenge Berechtigungskontrollen in Azure erzwingen, können Sie das folgende PowerShell-Skript verwenden. Es ermöglicht das Erstellen der benutzerdefinierten Rolle „SQL Analytics Monitoring Operator“ im Azure-Portal mit nur den Lese- und Schreibberechtigungen, die für die Verwendung von Azure SQL-Analyse in vollem Umfang erforderlich sind.

Ersetzen Sie „{SubscriptionId}“ im Skript unten durch Ihre Azure-Abonnement-ID, und führen Sie das Skript aus, während Sie mit der Rolle „Besitzer“ oder „Mitwirkender“ in Azure angemeldet sind.

   ```powershell
    Connect-AzureRmAccount
    Select-AzureRmSubscription {SubscriptionId}
    $role = Get-AzureRmRoleDefinition -Name Reader
    $role.Name = "SQL Analytics Monitoring Operator"
    $role.Description = "Lets you monitor database performance with Azure SQL Analytics as a reader. Does not allow change of resources."
    $role.IsCustom = $true
    $role.Actions.Add("Microsoft.SQL/servers/databases/read");
    $role.Actions.Add("Microsoft.SQL/servers/databases/topQueries/queryText/*");
    $role.Actions.Add("Microsoft.Sql/servers/databases/advisors/read");
    $role.Actions.Add("Microsoft.Sql/servers/databases/advisors/write");
    $role.Actions.Add("Microsoft.Sql/servers/databases/advisors/recommendedActions/read");
    $role.Actions.Add("Microsoft.Sql/servers/databases/advisors/recommendedActions/write");
    $role.Actions.Add("Microsoft.Sql/servers/databases/automaticTuning/read");
    $role.Actions.Add("Microsoft.Sql/servers/databases/automaticTuning/write");
    $role.Actions.Add("Microsoft.Sql/servers/databases/*");
    $role.Actions.Add("Microsoft.Sql/servers/advisors/read");
    $role.Actions.Add("Microsoft.Sql/servers/advisors/write");
    $role.Actions.Add("Microsoft.Sql/servers/advisors/recommendedActions/read");
    $role.Actions.Add("Microsoft.Sql/servers/advisors/recommendedActions/write");
    $role.Actions.Add("Microsoft.Resources/deployments/write");
    $role.AssignableScopes = "/subscriptions/{SubscriptionId}"
    New-AzureRmRoleDefinition $role
   ```

Nachdem die neue Rolle erstellt wurde, weisen Sie sie allen Benutzern zu, denen Sie benutzerdefinierte Berechtigungen zum Verwenden von Azure SQL-Analyse gewähren möchten.

## <a name="analyze-data-and-create-alerts"></a>Analysieren von Daten und Erstellen von Warnungen

Die Datenanalyse in Azure SQL-Analyse basiert auf der [Log Analytics-Sprache](./query-language/get-started-queries.md) für Ihre benutzerdefinierten Abfragen und Berichte. Eine Beschreibung der von der Datenbankressource für benutzerdefinierte Abfragen gesammelten Daten finden Sie unter [Verfügbare Metriken und Protokolle](../sql-database/sql-database-metrics-diag-logging.md#metrics-and-logs-available).

Automatisierte Warnungen in der Lösung basieren auf dem Schreiben einer Log Analytics-Abfrage, die eine Warnung auslöst, wenn eine Bedingung erfüllt ist. Im Folgenden finden Sie mehrere Beispiele für Log Analytics-Abfragen, für die in der Lösung Warnungen eingerichtet werden können.

### <a name="creating-alerts-for-azure-sql-database"></a>Erstellen von Warnungen für Azure SQL-Datenbank

Sie können problemlos [Warnungen](../monitoring-and-diagnostics/alert-metric.md) mit den Daten erstellen, die aus Azure SQL-Datenbank-Ressourcen stammen. Hier sind einige nützliche Abfragen für die [Protokollsuche](log-analytics-queries.md), die Sie mit einer Protokollwarnung verwenden können:

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

*Azure SQL-Datenbankspeicher im Durchschnitt über 95 % in der letzten Stunde*

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

### <a name="creating-alerts-for-managed-instance"></a>Erstellen von Warnungen für eine verwaltete Instanz

*Speicher der verwalteten Instanzen ist über 90 % ausgelastet

```
let storage_percentage_treshold = 90;
AzureDiagnostics
| where Category =="ResourceUsageStats"
| summarize (TimeGenerated, calculated_storage_percentage) = arg_max(TimeGenerated, todouble(storage_space_used_mb_s) *100 / todouble (reserved_storage_mb_s))
   by ResourceId
| where calculated_storage_percentage > storage_percentage_treshold
```

> [!NOTE]
> - Voraussetzung für die Einrichtung dieser Warnung ist, dass für die überwachte verwaltete Instanz das Streaming des ResourceUsageStats-Protokolls an die Lösung aktiviert ist.
> - Diese Abfrage erfordert die Einrichtung einer Warnungsregel, die eine Warnung auslöst, wenn Ergebnisse (> 0 Ergebnisse) von der Abfrage vorhanden sind, die angeben, dass die Bedingung in der verwalteten Instanz vorhanden ist. Die Ausgabe ist der prozentuale Speicherverbrauch in der verwalteten Instanz.

### <a name="pricing"></a>Preise

Obwohl die Lösung kostenlos verwendet werden kann, fallen für die Nutzung von Diagnosetelemetriedaten über die kostenlosen Einheiten hinaus, die für die Datenerfassung jeden Monat zugeteilt werden, Gebühren an. Weitere Informationen finden Sie unter [Log Analytics – Preise](https://azure.microsoft.com/en-us/pricing/details/monitor). Die kostenlosen Einheiten der bereitgestellten Datenerfassung ermöglichen die kostenlose Überwachung von mehreren Datenbanken pro Monat. Beachten Sie, dass bei einer größeren Anzahl aktiver Datenbanken mit umfangreicheren Workloads mehr Daten als bei Datenbanken im Leerlauf erfasst werden. Sie können problemlos Ihre Datenerfassungsnutzung in der Lösung überwachen, indem Sie den OMS-Arbeitsbereich im Navigationsmenü der Azure SQL-Analyse auswählen und dann „Nutzungs- und geschätzte Kosten“ auswählen.

## <a name="next-steps"></a>Nächste Schritte

- Verwenden Sie die [Protokollsuche](log-analytics-queries.md) in Log Analytics, um ausführliche Azure SQL-Daten anzuzeigen.
- [Erstellen eigener Dashboards](log-analytics-dashboards.md), die Azure SQL-Daten anzeigen
- [Erstellen von Warnungen](../monitoring-and-diagnostics/monitoring-overview-alerts.md), wenn bestimmte Azure SQL-Ereignisse auftreten
