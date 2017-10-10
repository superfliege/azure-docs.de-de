---
title: "Azure SQL Analytics-Lösung in Log Analytics | Microsoft-Dokumentation"
description: "Die Azure SQL Analytics-Lösung hilft Ihnen bei der Verwaltung Ihrer Azure SQL-Datenbanken."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: b2712749-1ded-40c4-b211-abc51cc65171
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2017
ms.author: banders
ms.translationtype: HT
ms.sourcegitcommit: 0e862492c9e17d0acb3c57a0d0abd1f77de08b6a
ms.openlocfilehash: 0b0d91b130172eb3506fdebb9547ab6ba5cc3780
ms.contentlocale: de-de
ms.lasthandoff: 09/27/2017

---


# <a name="monitor-azure-sql-database-using-azure-sql-analytics-preview-in-log-analytics"></a>Überwachen von Azure SQL-Datenbank mithilfe von Azure SQL Analytics (Vorschau) in Log Analytics

![Symbol Azure SQL Analytics](./media/log-analytics-azure-sql/azure-sql-symbol.png)

Die Azure SQL Analytics-Lösung in Azure Log Analytics erfasst und visualisiert wichtige SQL Azure-Leistungsmetriken. Anhand der mit der Lösung erfassten Kennzahlen können Sie benutzerdefinierte Überwachungsregeln und -warnungen erstellen. Zudem können Sie Azure SQL-Datenbank und Kennzahlen für Pools für elastische Datenbanken über mehrere Azure-Abonnements und Pools für elastische Datenbanken hinweg überwachen und visuell darstellen. Die Lösung hilft Ihnen außerdem beim Erkennen von Problemen auf jeder Stufe Ihres Anwendungsstapels.  Sie verwendet [Azure-Diagnosemetriken](log-analytics-azure-storage.md) zusammen mit Log Analytics-Ansichten, um Daten zu allen Azure SQL-Datenbanken und Pools für elastische Datenbanken in einem einzelnen Log Analytics-Arbeitsbereich darzustellen.

Zurzeit unterstützt diese Vorschaulösung bis zu 150.000 Azure SQL-Datenbanken und 5.000 Pools für elastische SQL-Datenbanken pro Arbeitsbereich.

Die Azure SQL Analytics-Lösung unterstützt Sie wie andere für Log Analytics verfügbare Lösungen bei der Überwachung und dem Empfang von Benachrichtigungen über den Zustand der Azure-Ressourcen, in diesem Fall von Azure SQL-Datenbank. Microsoft Azure SQL-Datenbank ist ein skalierbarer relationaler Datenbankdienst, der vertraute SQL Server-ähnliche Funktionen für Anwendungen bereitstellt, die in der Azure-Cloud ausgeführt werden. Log Analytics unterstützt Sie beim Erfassen, Korrelieren und Visualisieren strukturierter und nicht strukturierter Daten.

## <a name="connected-sources"></a>Verbundene Quellen

Die Azure SQL Analytics-Lösung verwendet keine Agents für die Verbindung mit dem Log Analytics-Dienst.

In der folgenden Tabelle sind die verbundenen Quellen beschrieben, die von der Lösung unterstützt werden.

| Verbundene Quelle | Support | Beschreibung |
| --- | --- | --- |
| [Windows-Agents](log-analytics-windows-agents.md) | Nein | Direkte Windows-Agents werden von der Lösung nicht verwendet. |
| [Linux-Agents](log-analytics-linux-agents.md) | Nein | Direkte Linux-Agents werden von der Lösung nicht verwendet. |
| [SCOM-Verwaltungsgruppe](log-analytics-om-agents.md) | Nein | Von der Lösung wird keine direkte Verbindung zwischen SCOM-Agent und Log Analytics verwendet. |
| [Azure-Speicherkonto](log-analytics-azure-storage.md) | Nein | Log Analytics liest keine Daten aus einem Speicherkonto. |
| [Azure-Diagnose](log-analytics-azure-storage.md) | Ja | Metrik- und Protokolldaten von Azure werden direkt von Azure an Log Analytics gesendet. |

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Abonnement. Falls Sie keines haben, können Sie [kostenlos](https://azure.microsoft.com/free/) eines erstellen.
- Einen Log Analytics-Arbeitsbereich Sie können ein vorhandenes verwenden oder [ein neues erstellen](log-analytics-get-started.md), bevor Sie diese Lösung verwenden.
- Aktivieren Sie die Azure-Diagnose für Ihre Azure SQL-Datenbanken und Pools für elastische Datenbanken, und [konfigurieren Sie diese so, dass ihre Daten an Log Analytics gesendet werden](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/).

## <a name="configuration"></a>Konfiguration

Führen Sie die folgenden Schritte aus, um die Azure SQL Analytics-Lösung Ihrem Arbeitsbereich hinzuzufügen.

1. Fügen Sie mithilfe des [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureSQLAnalyticsOMS?tab=Overview) oder des unter [Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog](log-analytics-add-solutions.md) beschriebenen Prozesses Ihrem Arbeitsbereich die Azure SQL Analytics-Lösung hinzu.
2. Klicken Sie im Azure-Portal auf **Neu** (das Symbol +), und wählen Sie dann in der Liste der Ressourcen **Überwachung und Verwaltung** aus.  
    ![Überwachung und Verwaltung](./media/log-analytics-azure-sql/monitoring-management.png)
3. Klicken Sie in der Liste **Überwachung und Verwaltung** auf **See all** (Alle anzeigen).
4. Klicken Sie in der Liste **Empfohlen** auf **Weitere Informationen**, suchen Sie in der neuen Liste **Azure SQL Analytics (Vorschau)**, und wählen Sie dann diese Option aus.  
    ![Azure SQL Analytics-Lösung](./media/log-analytics-azure-sql/azure-sql-solution-portal.png)
5. Klicken Sie im Bereich **Azure SQL-Analyse (Vorschauversion)** auf **Erstellen**.  
    ![Erstellen](./media/log-analytics-azure-sql/portal-create.png)
6. Wählen Sie im Bereich **Neue Lösung erstellen** den Arbeitsbereich aus, dem Sie die Lösung hinzufügen möchten, und klicken Sie dann auf **Erstellen**.  
    ![Zu Arbeitsbereich hinzufügen](./media/log-analytics-azure-sql/add-to-workspace.png)


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

>[!NOTE]
> Aktualisieren Sie Ihren Log Analytics-Arbeitsbereich, um die neueste Version der Azure SQL-Analyse zu erhalten.
>

Wenn Sie die Lösung zu Ihrem Arbeitsbereich hinzufügen, wird die Kachel „Azure SQL Analytics“ zu Ihrem Arbeitsbereich hinzugefügt und erscheint in der Übersicht. Die Kachel zeigt die Anzahl der Azure SQL-Datenbanken und der Pools für elastische SQL Azure-Datenbanken, mit denen die Lösung verbunden ist.

![Kachel „Azure SQL Analytics“](./media/log-analytics-azure-sql/azure-sql-sol-tile.png)

### <a name="viewing-azure-sql-analytics-data"></a>Anzeigen von Azure SQL Analytics-Daten

Klicken Sie auf die Kachel **Azure SQL Analytics**, um das Dashboard von Azure SQL Analytics zu öffnen. Das Dashboard umfasst die Übersicht über alle Datenbanken, die aus verschiedenen Perspektiven überwacht werden. Damit verschiedene Perspektiven funktionieren, müssen Sie die richtigen Metriken oder Protokolle auf Ihren SQL-Ressourcen aktivieren, die dann an den Azure Log Analytics-Arbeitsbereich gestreamt werden. 

![Azure SQL-Analyse – Übersicht](./media/log-analytics-azure-sql/azure-sql-sol-overview.png)

Die Auswahl einer Kachel öffnet einen Drilldown-Bericht zur entsprechenden Perspektive.

![Azure SQL-Analyse – Zeitlimits](./media/log-analytics-azure-sql/azure-sql-sol-timeouts.png)

Jede Perspektive bietet Zusammenfassungen zum Abonnement, Server, elastischen Pool und zur Datenbankebene. Darüber hinaus zeigt jede Perspektive auf der rechten Seite einen speziellen Bericht an. Durch die Auswahl eines Abonnements, Servers, Anwendungspools oder einer Datenbank aus der Liste wird der Drilldown nach fortgesetzt.

| Perspektive | Beschreibung |
| --- | --- |
| Ressource nach Typ | Perspektive, die alle überwachten Ressourcen zählt. Der Drilldown enthält die Zusammenfassung von DTU- und GB- Metriken. |
| Erkenntnisse | Stellt den hierarchischen Drilldown in Intelligent Insights bereit. Weitere Informationen zu Intelligent Insights. |
| Errors | Stellt den hierarchischen Drilldown in SQL-Fehler bereit, die in den Datenbanken aufgetreten sind. |
| Zeitlimits | Stellt den hierarchischen Drilldown in SQL-Zeitlimits bereit, die in den Datenbanken aufgetreten sind. |
| Blockierungen | Stellt den hierarchischen Drilldown in SQL-Blockierungen bereit, die in den Datenbanken aufgetreten sind. |
| Datenbankwartevorgänge | Stellt den hierarchischen Drilldown in SQL-Wartestatistiken auf Datenbankebene bereit. Enthält Zusammenfassungen der gesamten Wartezeit sowie die Wartezeit pro Wartetyp. |
| Abfragedauer | Stellt den hierarchischen Drilldown in die Statistiken zur Abfrageausführung bereit, z.B. Abfragedauer, CPU-Auslastung, Daten-E/A-Auslastung und Protokoll-E/A-Auslastung. |
| Abfragewartevorgänge | Stellt den hierarchischen Drilldown in die Statistiken zu Abfragewartevorgängen nach Wartekategorie bereit. |

### <a name="intelligent-insights-report"></a>Intelligent Insights-Bericht

Alle erfassten Intelligent Insights-Daten können visuell dargestellt und über die Insights Perspektive aufgerufen werden. [Klicken Sie hier, um mehr über Intelligent Insights zu erfahren.](../sql-database/sql-database-intelligent-insights.md)

![Azure SQL-Analyse – Insights](./media/log-analytics-azure-sql/azure-sql-sol-insights.png)

### <a name="elastic-pool-and-database-reports"></a>Berichte zu elastischen Pools und Datenbanken

Sowohl elastische Pools als auch Datenbanken verfügen über eigene bestimmte Berichte, die alle Daten anzeigen, die für die Ressource im angegebenen Zeitraum gesammelt werden.

![Azure SQL-Analyse –Datenbank](./media/log-analytics-azure-sql/azure-sql-sol-database.png)

![Azure SQL-Analyse – elastischer Pool](./media/log-analytics-azure-sql/azure-sql-sol-pool.png)

### <a name="query-reports"></a>Abfrageberichte

Durch die Abfragedauer- und Abfragewartevorgänge-Perspektive können Sie die Leistung von Abfragen über den Abfragebericht korrelieren. Dieser Bericht vergleicht die Abfrageleistung in verschiedenen Datenbanken und erleichtert das Ermitteln von Datenbanken, die die ausgewählte Abfrage im Vergleich zu langsamen Datenbanken ordnungsgemäß ausführen.

![Azure SQL-Analyse – Abfragen](./media/log-analytics-azure-sql/azure-sql-sol-queries.png)

### <a name="analyze-data-and-create-alerts"></a>Analysieren von Daten und Erstellen von Warnungen

Sie können problemlos Warnungen mit den Daten erstellen, die aus Azure SQL-Datenbank-Ressourcen stammen. Hier sind einige nützliche Abfragen für die [Protokollsuche](log-analytics-log-searches.md), die Sie für Warnungen verwenden können:

*Hohe DTU in Azure SQL-Datenbank*

```
AzureMetrics | where ResourceProvider=="MICROSOFT.SQL" and ResourceId contains "/DATABASES/" and MetricName=="dtu_consumption_percent" | summarize avg(Maximum) by ResourceId
```

*Hohe DTU in Pool für elastische Datenbanken in Azure SQL-Datenbank*

```
AzureMetrics | where ResourceProvider=="MICROSOFT.SQL" and ResourceId contains "/ELASTICPOOLS/" and MetricName=="dtu_consumption_percent" | summarize avg(Maximum) by ResourceId
```

Sie können diese warnungsbasierten Abfragen verwenden, um bei bestimmten Schwellenwerten für Azure SQL-Datenbanken und Pools für elastische Datenbanken eine Warnung auszugeben. So konfigurieren Sie eine Warnung für Ihren OMS-Arbeitsbereich:

#### <a name="to-configure-an-alert-for-your-workspace"></a>Konfigurieren einer Warnung für Ihren Arbeitsbereich

1. Wechseln Sie zum [OMS-Portal](http://mms.microsoft.com/), und melden Sie sich an.
2. Öffnen Sie den Arbeitsbereich, den Sie für die Lösung konfiguriert haben.
3. Klicken Sie auf der Seite „Übersicht“ auf die Kachel **Azure SQL Analytics (Vorschau)**.
4. Führen Sie eine der Beispielabfragen aus.
5. Klicken Sie in der Protokollsuche auf **Warnung**.  
![Erstellen einer Warnung in der Suche](./media/log-analytics-azure-sql/create-alert01.png)
6. Konfigurieren Sie auf der Seite **Warnungsregel hinzufügen** die entsprechenden Eigenschaften und die spezifischen Schwellenwerte nach Bedarf, und klicken Sie dann auf **Speichern**.  
![Warnungsregel hinzufügen](./media/log-analytics-azure-sql/create-alert02.png)

## <a name="see-also"></a>Weitere Informationen

- Verwenden Sie die [Protokollsuche](log-analytics-log-searches.md) in Log Analytics, um ausführliche Azure SQL-Daten anzuzeigen.
- [Erstellen eigener Dashboards](log-analytics-dashboards.md), die Azure SQL-Daten anzeigen
- [Erstellen von Warnungen](log-analytics-alerts.md), wenn bestimmte Azure SQL-Ereignisse auftreten

