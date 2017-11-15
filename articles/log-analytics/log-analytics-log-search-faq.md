---
title: "Häufig gestellte Fragen zur neuen Protokollsuche von Log Analytics | Microsoft-Dokumentation"
description: "Dieser Artikel behandelt häufig gestellte Fragen im Hinblick auf das Upgrade von Log Analytics auf die neue Abfragesprache."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/02/2017
ms.author: bwren
ms.openlocfilehash: 1ec815a12cea98228dd4b7ac7361fe5e3554b5d3
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2017
---
# <a name="log-analytics-new-log-search-faq-and-known-issues"></a>Neue Protokollsuche in Log Analytics – häufig gestellte Fragen und bekannte Probleme

Dieser Artikel enthält häufig gestellte Fragen und bekannte Probleme im Hinblick auf das Upgrade von [Log Analytics auf die neue Abfragesprache](log-analytics-log-search-upgrade.md).  Sie sollten diesen Artikel lesen, bevor Sie sich entscheiden, Ihren Arbeitsbereich zu aktualisieren.


## <a name="alerts"></a>Warnungen

### <a name="question-i-have-a-lot-of-alert-rules-do-i-need-to-create-them-again-in-the-new-language-after-i-upgrade"></a>Frage: Ich habe viele Warnungsregeln. Muss ich diese erneut in der neuen Sprache erstellen, nachdem ich das Upgrade durchgeführt habe?  
Nein, Ihre Warnungsregeln werden während des Upgrades automatisch in die neue Suchsprache konvertiert.  

### <a name="question-i-have-alert-rules-with-webhook-and-runbook-actions-will-these-continue-to-work-when-i-upgrade"></a>Frage: Ich habe Warnungsregeln für Webhook- und Runbook-Aktionen. Funktionieren diese Regeln nach dem Upgrade noch?

Nein. Es wurden einige Änderungen an den Webhook- und Runbook-Aktionen vorgenommen, aufgrund derer Sie möglicherweise die Payload anders verarbeiten müssen. Diese Änderungen wurden vorgenommen, um die verschiedenen Ausgabeformate zu standardisieren und die Größe der Payload zu reduzieren. Weitere Informationen zu diesen Formaten finden Sie unter [Add actions to alert rules in Log Analytics (Hinzufügen von Warnungsregeln in Log Analystics)](log-analytics-alerts-actions.md).


## <a name="computer-groups"></a>Computergruppen

### <a name="question-im-getting-errors-when-trying-to-use-computer-groups--has-their-syntax-changed"></a>Frage: Ich erhalte Fehler, wenn ich versuche, Computergruppen zu verwenden.  Hat sich deren Syntax geändert?
Ja, die Syntax für die Verwendung von Computergruppen hat sich geändert, wenn Sie ein Upgrade Ihres Arbeitsbereichs durchgeführt haben.  Weitere Informationen finden Sie unter [Computergruppen in Log Analytics-Protokollsuchen](log-analytics-computer-groups.md).


## <a name="dashboards"></a>Dashboards

### <a name="question-can-i-still-use-dashboards-in-an-upgraded-workspace"></a>Frage: Kann ich in einem aktualisierten Arbeitsbereich weiterhin Dashboards verwenden?
Das Upgrade ist ein erster Schritt dahin, die Unterstützung von **Mein Dashboard** einzustellen.  Sie können weiterhin alle Kacheln verwenden, die Sie dem Dashboard hinzugefügt haben, bevor Ihr Arbeitsbereich aktualisiert wurde, aber Sie können diese Kacheln nicht bearbeiten und auch keine neuen hinzufügen.  Sie können weiterhin mit dem [Ansicht-Designer](log-analytics-view-designer.md), der über mehr Funktionen verfügt, Ansichten erstellen und bearbeiten und auch Dashboards im Azure-Portal erstellen.


## <a name="log-searches"></a>Protokollsuchvorgänge

### <a name="question-i-have-saved-searches-outside-of-my-upgraded-workspace-can-i-convert-them-to-the-new-search-language-automatically"></a>Frage: Ich habe Suchen außerhalb meines aktualisierten Arbeitsbereichs gespeichert. Kann ich diese automatisch in die neue Suchsprache konvertieren?
Sie können den das Tool zum Konvertieren der Sprache auf der Seite der Protokollsuche verwenden, um sie zu konvertieren.  Es gibt keine Möglichkeit, mehrere Suchen automatisch zu konvertieren, ohne ein Upgrade auf den Arbeitsbereich durchzuführen.

### <a name="question-why-are-my-query-results-not-sorted"></a>Frage: Warum sind meine Abfrageergebnisse nicht sortiert?
Ergebnisse werden in der neuen Abfragesprache standardmäßig nicht sortiert.  Verwenden Sie den [sort-Operator](https://go.microsoft.com/fwlink/?linkid=856079), um die Ergebnisse nach einer oder mehreren Eigenschaften zu sortieren.

### <a name="question-where-did-the-metrics-view-go-after-i-upgraded"></a>Frage: Wo befindet sich die Metrikansicht nach dem Upgrade?
In der Metrikansicht wurden Leistungsdaten aus einer Protokollsuche graphisch dargestellt.  Diese Ansicht ist nach dem Upgrade nicht mehr verfügbar.  Sie können den [Render-Operator](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/render-operator) nutzen, um Ausgaben aus einer Abfrage in einem Zeitdiagramm zu formatieren.

### <a name="question-where-did-minify-go-after-i-upgraded"></a>Frage: Wo befindet sich die Minifyfunktion nach dem Upgrade?
Mit der Minifyfunktion kann eine Übersicht über Ihre Suchergebnisse angezeigt werden.  Nach dem Upgrade wird die Minifyoption nicht mehr im Portal für die Protokollsuche angezeigt.  Eine ähnliche Funktionalität erhalten Sie mit der neuen Suchsprache unter Verwendung von [reduce](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/reduce-operator) oder [autocluster_v2](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/evaluate-operator/autocluster). 

    Event
    | where TimeGenerated > ago(10h)
    | reduce by RenderedDescription

    Event
    | where TimeGenerated > ago(10h)
    | evaluate autocluster_v2()



## <a name="log-search-api"></a>Protokollsuch-API

### <a name="question-does-the-log-search-api-get-updated-after-i-upgrade"></a>Frage: Wird die Protokollsuch-API nach dem Upgrade aktualisiert?
Die Legacy-[Protokollsuch-API](log-analytics-log-search-api.md) funktioniert nicht mehr, wenn Sie den Arbeitsbereich aktualisiert haben.  Nähere Informationen zur neuen API finden Sie unter [Azure Log Analytics REST API](https://dev.loganalytics.io/) (Azure Log Analytics-REST-API).


## <a name="portals"></a>Portale

### <a name="question-should-i-use-the-new-advanced-analytics-portal-or-keep-using-the-log-search-portal"></a>Frage: Sollte ich das neue Advanced Analytics-Portal oder weiterhin das Portal für die Protokollsuche verwenden?
Einen Vergleich der beiden Portale finden Sie unter [Portale zum Erstellen und Bearbeiten von Protokollabfragen in Azure Log Analytics](log-analytics-log-search-portals.md).  Jedes Portal verfügt über spezielle Vorteile, daher können Sie das am besten für Ihre Anforderungen geeignete verwenden.  Häufig werden Abfragen im Advanced Analytics-Portal geschrieben und an anderen Stellen eingefügt, z.B. im Ansicht-Designer.  Lesen Sie in diesem Fall die [zu berücksichtigenden Aspekte](log-analytics-log-search-portals.md#advanced-analytics-portal).


### <a name="question--after-upgrade-i-get-an-error-trying-to-run-queries-and-am-also-seeing-errors-in-my-views"></a>Frage: Nach dem Upgrade erhalte ich eine Fehlermeldung bei dem Versuch, Abfragen auszuführen, und sehe auch Fehlermeldungen in meinen Ansichten.

Ihr Browser erfordert Zugriff auf die folgenden Adressen, um Log Analytics-Abfragen nach dem Upgrade auszuführen.  Wenn der Browser über eine Firewall auf das Azure-Portal zugreift, müssen Sie den Zugriff auf diese Adressen aktivieren.

| Uri | IP | Ports |
|:---|:---|:---|
| portal.loganalytics.io | Dynamisch | 80, 443 |
| api.loganalytics.io    | Dynamisch | 80, 443 |
| docs.loganalytics.io   | Dynamisch | 80, 443 |



## <a name="power-bi"></a>Power BI

### <a name="question-does-anything-change-with-powerbi-integration"></a>Frage: Ändert sich etwas bei der Power BI-Integration?
Ja.  Sobald für Ihren Arbeitsbereich ein Upgrade durchgeführt wurde, funktioniert dieses Verfahren zum Exportieren von Log Analytics-Daten in Power BI nicht mehr.  Alle vorhandenen Zeitpläne, die Sie vor dem Upgrade erstellt haben, werden deaktiviert.  

Nach dem Upgrade verwendet Azure Log Analytics dieselbe Plattform wie Application Insights, und Sie verwenden zum Exportieren von Log Analytics-Abfragen nach Power BI dasselbe Verfahren wie zum [Exportieren von Application Insights-Abfragen nach Power BI](../application-insights/app-insights-export-power-bi.md#export-analytics-queries).  Der Export nach Power BI ruft nun direkt den API-Endpunkt auf. Dadurch können Sie bis zu 500.000 Zeilen oder 64.000.000 Byte an Daten abrufen, lange Abfragen exportieren und das Timeout der Abfrage anpassen (das Standardtimeout beträgt 3 Minuten, und das maximale Timeout beträgt 10 Minuten).

## <a name="powershell-cmdlets"></a>PowerShell-Cmdlets

### <a name="question-does-the-log-search-powershell-cmdlet-get-updated-after-i-upgrade"></a>Frage: Wird das PowerShell-Cmdlet für die Protokollsuche nach dem Upgrade aktualisiert?
Die [Get-AzureRmOperationalInsightsSearchResults](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/Get-AzureRmOperationalInsightsSearchResults) sind veraltet, wenn das Upgrade aller Arbeitsbereiche abgeschlossen ist.  Verwenden Sie das [Invoke-LogAnalyticsQuery](https://dev.loganalytics.io/documentation/Tools/PowerShell-Cmdlets)-Cmdlet für Protokollsuchen in aktualisierten Arbeitsbereichen.




## <a name="resource-manager-templates"></a>Resource Manager-Vorlagen

### <a name="question-can-i-create-an-upgraded-workspace-with-a-resource-manager-template"></a>Frage: Kann ich einen aktualisierten Arbeitsbereich mithilfe einer Resource Manager-Vorlage erstellen?
Ja.  Sie müssen die API-Version 2017-03-15-preview verwenden und wie im folgenden Beispiel gezeigt einen Abschnitt **features** in die Vorlage einfügen.

    "resources": [
        {
            "type": "Microsoft.OperationalInsights/workspaces",
            "apiVersion": "2017-03-15-preview",
            "name": "[parameters('workspaceName')]",
            "location": "[parameters('workspaceRegion')]",
            "properties": {
                "sku": {
                    "name": "Free"
                },
                "features": {
                    "legacy": 0,
                    "searchVersion": 1
                }
            }
        }
    ],



## <a name="solutions"></a>Lösungen

### <a name="question-will-my-solutions-continue-to-work"></a>Frage: Bleiben meine Lösungen weiterhin funktionsfähig?
Alle Lösungen bleiben in einem aktualisierten Arbeitsbereich weiterhin funktionsfähig. Ihre Leistung verbessert sich jedoch, wenn sie in die neue Abfragesprache konvertiert werden.  Es gibt bekannte Probleme mit einigen vorhandenen Lösungen, die in diesem Abschnitt beschrieben werden.

### <a name="known-issue-perspectives-in-application-insights-connector"></a>Bekanntes Problem: Perspektiven im Application Insights-Connector
Perspektiven werden im [Application Insights-Connector](log-analytics-app-insights-connector.md) nicht mehr unterstützt.  Sie können mit dem Ansicht-Designer benutzerdefinierte Ansichten mit Application Insights-Daten erstellen.

### <a name="known-issue-backup-solution"></a>Bekanntes Problem: Sicherungslösung
Die Sicherungslösung sammelt möglicherweise keine Daten, wenn sie vor dem Aktualisieren eines Arbeitsbereichs installiert wurde. Deinstallieren Sie die Lösung, und installieren Sie dann die neueste Version.  Die neue Version der Lösung unterstützt keine klassischen Sicherungstresore, daher müssen Sie auch die Recovery Services-Tresore aktualisieren, um die Lösung weiterhin zu verwenden.

## <a name="upgrade-process"></a>Upgradeprozess

### <a name="question-i-have-several-workspaces-can-i-upgrade-all-workspaces-at-the-same-time"></a>Frage: Ich besitze mehrere Arbeitsbereiche. Kann ich alle Arbeitsbereiche gleichzeitig upgraden?  
Nein.  Das Upgrade gilt für jeweils nur einen Arbeitsbereich. Aktuell können Sie nicht mehrere Arbeitsbereiche gleichzeitig upgraden. Beachten Sie, dass andere Benutzer des upgegradeten Arbeitsbereichs ebenfalls betroffen sind.  

### <a name="question-will-existing-log-data-collected-in-my-workspace-be-modified-if-i-upgrade"></a>Frage: Werden vorhandene Protokolldaten, die in meinem Arbeitsbereich gesammelt wurden, geändert, wenn ich ein Upgrade durchführe?  
Nein. Die verfügbaren Protokolldaten, die für Ihre Arbeitsbereich-Suchvorgänge verfügbar sind, sind durch das Upgrade nicht betroffen. Gespeicherte Suchvorgänge, Warnungen und Ansichten, werden automatisch in die neue Suchsprache konvertiert.  

### <a name="question-what-happens-if-i-dont-upgrade-my-workspace"></a>Frage: Was passiert, wenn ich kein Upgrade für meinen Arbeitsbereich durchführe?  
Die ältere Protokollsuche wird in den kommenden Monaten als veraltet gekennzeichnet. Arbeitsbereiche, die zu dieser Zeit noch nicht upgegradet worden sind, erhalten automatisch ein Upgrade.

### <a name="question-can-i-revert-back-after-i-upgrade"></a>Frage: Ist eine Wiederherstellung nach einem Upgrade möglich?
Vor der allgemeinen Verfügbarkeit könnten Sie Ihren Arbeitsbereich nach dem Upgrade wiederherstellen.  Da die neue Sprache jetzt allgemein verfügbar ist, wurde diese Möglichkeit aufgehoben, da wir beginnen, die veraltete Plattform außer Kraft zu setzen.



## <a name="views"></a>Ansichten

### <a name="question-how-do-i-create-a-new-view-with-view-designer"></a>Frage: Wie erstelle ich mit dem Ansicht-Designer eine neue Ansicht?
Vor dem Upgrade konnten Sie mit dem Ansicht-Designer eine neue Ansicht aus einer Kachel im Hauptdashboard erstellen.  Nach dem Upgrade Ihres Arbeitsbereichs wird diese Kachel entfernt.  Sie können eine neue Ansicht mit dem Ansicht-Designer im OMS-Portal erstellen, indem Sie auf das grüne Pluszeichen (+) im linken Menü klicken.


## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum [Upgrade Ihres Arbeitsbereichs auf die neue Log Analytics-Abfragesprache](log-analytics-log-search-upgrade.md).
