---
title: "Was hat sich in Azure Log Analytics geändert? | Microsoft-Dokumentation"
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
ms.date: 11/08/2017
ms.author: bwren
ms.openlocfilehash: 783223a37c2a13c9affbf382209ca2aa4f1ba4c7
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2017
---
# <a name="whats-changed-in-azure-log-analytics"></a>Was hat sich in Azure Log Analytics geändert?
Zusätzlich zur Abfragesprache selbst wurden verschiedene Verbesserungen und Änderungen durchgeführt, über die Sie informiert sein sollten, wenn Ihr Log Analytics-Arbeitsbereich [auf die neue Abfragesprache aktualisiert wird](log-analytics-log-search-new.md).  In diesem Artikel werden kurz die Änderungen zwischen einem Legacyarbeitsbereich und einem aktualisierten Arbeitsbereich beschrieben und jeweils Links zu ausführlichen weiteren Informationen angegeben. 

Eine Beschreibung der bekannten Probleme beim Upgradevorgang und Antworten auf häufig gestellte Fragen finden Sie unter [Neue Protokollsuche in Log Analytics – häufig gestellte Fragen und bekannte Probleme](log-analytics-log-search-faq.md).  

## <a name="query-language"></a>Abfragesprache
Die wichtigste Änderung beim Log Analytics-Upgrade ist die neue Abfragesprache, die über erhebliche Verbesserungen gegenüber der vorherigen Sprachversion verfügt.  

Einen direkten Vergleich der wichtigsten Vorgänge in der alten und neuen Sprache finden Sie unter [Übergang zur neuen Azure Log Analytics-Abfragesprache](log-analytics-log-search-transition.md).  Eine vollständige Dokumentation zur neuen Sprache finden Sie auf der Seite zur [Azure Log Analytics-Abfragesprache](https://docs.loganalytics.io).


## <a name="computer-groups"></a>Computergruppen
Die Methode zum Erstellen einer Computergruppe hat sich nicht geändert, aber Sie müssen jetzt für jede Gruppe einen eindeutigen Alias angeben.  Für Computergruppen, die auf Protokollsuchen basieren, muss die Syntax der neuen Sprache verwendet werden.

Es ist eine neue Syntax für die Verwendung von Computergruppen in einer Protokollsuche vorhanden.  Anstatt die Funktion „$ComputerGroups“ zu verwenden, werden Computergruppen jetzt jeweils als Funktion mit eindeutigem Alias implementiert.  Sie verwenden den Alias in der Protokollsuche wie jede andere Funktion.  

Details hierzu finden Sie unter [Computergruppen in Log Analytics-Protokollsuchen](log-analytics-computer-groups.md).


## <a name="log-search-portals"></a>Portale für die Protokollsuche
Zusätzlich zum Portal für die Protokollsuche für die Erstellung und Ausführung von Protokollsuchen können Sie jetzt auch das Advanced Analytics-Portal nutzen, das über erheblich verbesserte Bearbeitungsfeatures verfügt.

Eine kurze Beschreibung der beiden Portale finden Sie unter [Portale zum Erstellen und Bearbeiten von Protokollabfragen in Azure Log Analytics](log-analytics-log-search-portals.md).  Unter [Erste Schritte mit dem Analytics-Portal](https://docs.loganalytics.io/docs/Learn/Getting-Started/Getting-started-with-the-Analytics-portal) können Sie ein Tutorial zum neuen Portal durcharbeiten.

## <a name="alerts"></a>Warnungen
Warnungen funktionieren in den aktualisierten Arbeitsbereichen genauso wie vorher, aber die darin ausgeführten Abfragen müssen in der neuen Sprache geschrieben sein.  Alle vorhandenen Warnungsregeln, die Sie vor dem Upgrade verwendet haben, werden automatisch in die neue Sprache konvertiert.  Weitere Details finden Sie unter [Grundlegendes zu Warnungen in Log Analytics](log-analytics-alerts.md).

Das Format der Nutzlast für Runbooks und Webhooks, die für Warnungen gesendet werden, hat sich geändert.  Details zum neuen Nutzlastformat finden Sie unter [Hinzufügen von Aktionen zu Warnungsregeln in Log Analytics](log-analytics-alerts-actions.md).

## <a name="dashboards"></a>Dashboards
[Dashboards](log-analytics-dashboards.md) werden zurzeit ausgesondert.  Sie können weiterhin alle Kacheln verwenden, die Sie dem Dashboard hinzugefügt haben, bevor Ihr Arbeitsbereich aktualisiert wurde, aber Sie können diese Kacheln nicht bearbeiten und auch keine neuen hinzufügen.  Verwenden Sie den Ansicht-Designer zum Erstellen von benutzerdefinierten Ansichten, da der Designer einen umfassenderen Featuresatz als Dashboards verfügt.

Details zum Ansicht-Designer finden Sie unter [Erstellen benutzerdefinierter Ansichten in Log Analytics mithilfe des Ansicht-Designers](log-analytics-view-designer.md).

## <a name="power-bi"></a>Power BI
Der Prozess zum Exportieren von Log Analytics-Daten nach Power BI hat sich für aktualisierte Arbeitsbereiche geändert, und alle vorhandenen Zeitpläne, die Sie vor der Durchführung des Upgrades erstellt haben, werden deaktiviert.  

Details hierzu finden Sie unter [Exportieren von Log Analytics-Daten nach Power BI](log-analytics-powerbi.md).

## <a name="powershell"></a>PowerShell
Get-AzureRmOperationalInsightsSearchResults zum Ausführen von Protokollsuchen mit PowerShell funktioniert für einen aktualisierten Arbeitsbereich nicht.  Verwenden Sie in einem aktualisierten Arbeitsbereich Invoke-LogAnalytics für diese Funktion.

Details hierzu finden Sie unter [Azure Log Analytics REST API – PowerShell Cmdlets](https://dev.loganalytics.io/documentation/Tools/PowerShell-Cmdlets) (Azure Log Analytics-REST-API – PowerShell-Cmdlets).

## <a name="log-search-api"></a>Protokollsuch-API
Die REST-API für die Protokollsuche hat sich geändert. Alle Lösungen, für die die Legacyversion genutzt wird, müssen aktualisiert werden, damit die neue Version der API verwendet wird.   

Details zur neuen Version der API finden Sie auf der Seite zur [Azure Log Analytics-REST-API](https://dev.loganalytics.io/).

## <a name="next-steps"></a>Nächste Schritte

- Eine Beschreibung der bekannten Probleme beim Upgradevorgang und Antworten auf häufig gestellte Fragen finden Sie unter [Neue Protokollsuche in Log Analytics – häufig gestellte Fragen und bekannte Probleme](log-analytics-log-search-faq.md).