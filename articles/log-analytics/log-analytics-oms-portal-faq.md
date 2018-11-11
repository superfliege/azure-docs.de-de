---
title: Häufig gestellte Fragen zum Übergang vom OMS-Portal zum Azure-Portal für Log Analytics-Benutzer | Microsoft-Dokumentation
description: Antworten auf häufig gestellte Fragen von Log Analytics-Benutzern, die vom OMS-Portal zum Azure-Portal wechseln.
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
ms.openlocfilehash: e644f8f0db9e310e8a3c77ebd04de160bcb7fa28
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2018
ms.locfileid: "51008911"
---
# <a name="common-questions-for-transition-from-oms-portal-to-azure-portal-for-log-analytics-users"></a>Häufig gestellte Fragen zum Übergang vom OMS-Portal zum Azure-Portal für Log Analytics-Benutzer
Log Analytics verwendete zunächst ein eigenes Portal, das OMS-Portal, um die Konfiguration zu verwalten und gesammelte Daten zu analysieren.  Alle Funktionen dieses Portals wurden in das Azure-Portal übernommen, wo sie weiter entwickelt werden.

Dieser Artikel beantwortet häufig gestellte Fragen von Benutzern, die diesen Wechsel vornehmen.  Wenn Sie Log Analytics im OMS-Portal verwendet haben, finden Sie hier Antworten darauf, wie Sie die gleichen Aufgaben im Azure-Portal ausführen können.

## <a name="do-i-need-to-migrate-anything"></a>Muss ich eine Migration durchführen?
Nein. Es werden keine Änderungen an Log Analytics selbst vorgenommen, sodass nichts migriert werden muss. Das Einzige, was sich ändert, ist die Schnittstelle, über die Sie darauf zugreifen. Tatsächlich können Sie jetzt über das Azure-Portal auf dieselben Arbeitsbereiche, Lösungen, Ansichten und Protokollsuchen zugreifen, die Sie heute im OMS-Portal verwenden.

## <a name="where-do-i-find-log-analytics-in-azure"></a>Wo finde ich Log Analytics in Azure?
Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.  Klicken Sie auf **Alle Dienste**, und geben Sie in der Liste mit den Ressourcen **Log Analytics** ein. Wählen Sie **Log Analytics**, und wählen Sie dann Ihren Arbeitsbereich aus. Die Seite mit der Zusammenfassung für den Arbeitsbereich wird angezeigt.

![Log Analytics-Arbeitsbereich](media/log-analytics-oms-portal-faq/log-analytics.png)

## <a name="how-do-i-manage-permissions"></a>Wie verwalte ich Berechtigungen?
Wenn Sie keinen Zugriff auf Ihren Log Analytics-Arbeitsbereich im Azure-Portal haben, müssen Sie Ihre Berechtigungen über den [rollenbasierten Zugriff von Azure](../active-directory/role-based-access-control-configure.md) konfigurieren. Weitere Informationen zum Verwalten von Berechtigungen für den Arbeitsbereich finden Sie unter [Verwalten von Arbeitsbereichen](../log-analytics/log-analytics-manage-access.md#manage-accounts-and-users). Informationen zur Verwaltung von Berechtigungen für Warnungen finden Sie unter [Erste Schritte mit Rollen, Berechtigungen und Sicherheit in Azure Monitor](../monitoring-and-diagnostics/monitoring-roles-permissions-security.md).

## <a name="how-do-i-create-a-new-workspace"></a>Wie erstelle ich einen neuen Arbeitsbereich? 
Klicken Sie im Azure-Portal in der Liste der Arbeitsbereiche auf **Hinzufügen**.  Sämtliche Informationen finden Sie unter [Erstellen eines Log Analytics-Arbeitsbereichs im Azure-Portal](../log-analytics/log-analytics-quick-create-workspace.md).

![Seite „Übersicht“](media/log-analytics-oms-portal-faq/new-workspace.png)

## <a name="where-is-my-overview-page"></a>Wo befindet sich meine Seite „Übersicht“?
Der Hauptbildschirm im OMS-Portal zeigt die Kacheln für alle in Ihrem Arbeitsbereich installierten Verwaltungslösungen und alle benutzerdefinierten Ansichten, die Sie erstellt haben. Die gleiche Ansicht steht im Azure-Portal zur Verfügung. Wählen Sie in Ihrem Arbeitsbereich die Option **Arbeitsbereichzusammenfassung** aus.

![Seite „Übersicht“](media/log-analytics-oms-portal-faq/overview.png)

## <a name="how-do-i-open-log-search-and-view-designer"></a>Wie öffne ich die Protokollsuche und den Ansicht-Designer?
Sowohl die **Protokollsuche** als auch der **Ansicht-Designer** sind auf der Hauptseite und im linken Menü Ihres Arbeitsbereichs im Azure-Portal, direkt neben **Übersicht**, verfügbar.

## <a name="where-do-i-find-settings"></a>Wo finde ich die Einstellungen?
Viele der Einstellungen im Abschnitt **Einstellungen** des OMS-Portals stehen im Azure-Portal im Menü **Erweiterte Einstellungen** für den Arbeitsbereich zur Verfügung.

![Erweiterte Einstellungen](media/log-analytics-oms-portal-faq/advanced-settings.png)

Die folgenden Abschnitte enthalten eine vollständige Liste, wie Sie auf Einstellungen zugreifen können, die zuvor im Abschnitt **Einstellungen** des OMS-Portals verfügbar waren.

### <a name="accounts"></a>Konten 
Die Einstellungen der Konten werden an verschiedenen Stellen im Azure-Portal verwaltet, wie in der folgenden Tabelle beschrieben.

| Einstellung im OMS-Portal | Entsprechung im Azure-Portal |
|:---|:---|
| Automation-Konto | Menü **Automation-Konto** für den Arbeitsbereich |
| Azure-Abonnement und Datenplan | Menü **Tarif** für den Arbeitsbereich |
| Verwalten von Benutzern | Verwenden Sie rollenbasierten Zugriff in Azure, um [Berechtigungen für Ihren Arbeitsbereich zu verwalten](#how-do-i-manage-permissions). |
| Informationen zum Arbeitsbereich | Über das Menü **OMS-Arbeitsbereich** verfügbare Informationen für Ihren Arbeitsbereich |

### <a name="alerts"></a>Alerts
Warnungsregeln basierend auf Log Analytics-Abfragen werden jetzt über die [vereinheitlichte Oberfläche für Warnungen](#how-do-i-create-and-manage-alerts) verwaltet. 

### <a name="computer-groups"></a>Computergruppen
Verwalten Sie Computergruppen über das Menü **Erweiterte Einstellungen** für den Arbeitsbereich. 

### <a name="connected-sources"></a>Verbundene Quellen
Verwalten Sie die meisten Einstellungen von „Verbunden Quelle“ im Menü **Erweiterte Einstellungen** für den Arbeitsbereich. Die folgende Tabelle enthält Details zu jedem Abschnitt dieses Menüs.

| Einstellung im OMS-Portal | Entsprechung im Azure-Portal |
|:---|:---|
| Windows-Server   | Menü **Erweiterte Einstellungen** für den Arbeitsbereich |
| Linux-Server   | Menü **Erweiterte Einstellungen** für den Arbeitsbereich |
| Azure Storage     | Menü **Erweiterte Einstellungen** für den Arbeitsbereich |
| System Center     | Menü **Erweiterte Einstellungen** für den Arbeitsbereich |
| Office 365        | Einzelheiten zur Konfiguration finden Sie in der [Dokumentation für die Office 365-Verwaltungslösung](../monitoring/monitoring-solution-office-365.md). |
| Windows-Telemetrie | Menü „Einstellungen“ für die Lösung. Die Konfigurationsdetails finden Sie unter [Windows Analytics im Azure-Portal](/windows/deployment/update/windows-analytics-azure-portal). |
| ITSM-Connector    | Anweisungen für die Verbindung mit Ihrem ITSM-Dienst mit Log Analytics finden Sie unter [Verbinden von ITSM-Produkten/-Diensten mit dem ITSM-Connector](../log-analytics/log-analytics-itsmc-connections.md). |

### <a name="data"></a>Daten
Verwalten Sie die meisten Einstellungen von „Daten“ im Menü **Erweiterte Einstellungen** für den Arbeitsbereich. Die folgende Tabelle enthält Details zu jedem Abschnitt dieses Menüs.

| Einstellung im OMS-Portal | Entsprechung im Azure-Portal |
|:---|:---|
| Windows-Ereignisprotokolle           | Menü **Erweiterte Einstellungen** für den Arbeitsbereich |
| Windows-Leistungsindikatoren | Menü **Erweiterte Einstellungen** für den Arbeitsbereich |
| Linux-Leistungsindikatoren   | Menü **Erweiterte Einstellungen** für den Arbeitsbereich |
| IIS-Protokolle                     | Menü **Erweiterte Einstellungen** für den Arbeitsbereich |
| Benutzerdefinierte Felder                | Menü **Erweiterte Einstellungen** für den Arbeitsbereich |
| Benutzerdefinierte Protokolle                  | Menü **Erweiterte Einstellungen** für den Arbeitsbereich |
| syslog                       | Menü **Erweiterte Einstellungen** für den Arbeitsbereich |
| Application Insights         | Diese Lösung ist veraltet, da Log Analytics und Application Insights nun die gleiche Daten-Engine nutzen.  |
| Nachverfolgung von Windows-Dateien        | Menü **Änderungsnachverfolgung** in Azure Automation Einzelheiten finden Sie unter [Nachverfolgen von Änderungen in Ihrer Umgebung mit der Lösung für die Änderungsnachverfolgung](../automation/automation-change-tracking.md). |
| Nachverfolgung für Windows-Registrierung        | Menü **Änderungsnachverfolgung** in Azure Automation Einzelheiten finden Sie unter [Nachverfolgen von Änderungen in Ihrer Umgebung mit der Lösung für die Änderungsnachverfolgung](../automation/automation-change-tracking.md). |
| Nachverfolgung von Linux-Dateien          | Menü **Änderungsnachverfolgung** in Azure Automation Einzelheiten finden Sie unter [Nachverfolgen von Änderungen in Ihrer Umgebung mit der Lösung für die Änderungsnachverfolgung](../automation/automation-change-tracking.md). |

### <a name="solutions"></a>Lösungen
Verwalten Sie Lösungen über das Menü **Lösungen** für den Arbeitsbereich. 

## <a name="how-do-i-install-and-remove-management-solutions"></a>Wie installiere und entferne ich Verwaltungslösungen?
Im OMS-Portal installierten Sie Verwaltungslösungen aus dem Lösungskatalog und entfernten sie über die **Einstellungen**. Im Azure-Portal [installieren Sie Verwaltungslösungen](../monitoring/monitoring-solutions.md#install-a-management-solution) aus dem Azure Marketplace. [Entfernen Sie Lösungen](../monitoring/monitoring-solutions.md#remove-a-management-solution) aus der Liste der installierten Lösungen.

## <a name="how-do-i-create-and-manage-alerts"></a>Wie erstelle und verwalte ich Warnungen?
Warnungsregeln basierend auf Log Analytics-Abfragen werden jetzt über die [vereinheitlichte Oberfläche für Warnungen](../monitoring-and-diagnostics/alert-metric.md) verwaltet. Weitere Informationen zur Konfiguration und Verwendung von Warnungen im Azure-Portal finden Sie unter [Gewusst wie: Erweitern (Kopieren) von Warnungen aus dem OMS-Portal in Azure](../monitoring-and-diagnostics/monitoring-alerts-extend-tool.md).

## <a name="how-do-i-access-my-dashboards"></a>Wie kann ich auf meine Dashboards zugreifen?
Das Feature [Mein Dashboard](../log-analytics/log-analytics-dashboards.md) in Log Analytics ist veraltet. Mit diesem Feature konnten Sie eine private Sammlung von Ansicht-Designer-Komponenten erstellen. Es wurde durch die integrierte Azure-Dashboard-Funktion ersetzt. Sie können weiterhin Daten in Log Analytics mit dem [Ansicht-Designer](../log-analytics/log-analytics-view-designer.md) für freigegebene Ansichten visualisieren. Sie können auch Visualisierungen aus diesen Ansichten oder für [einzelne Abfragen](../log-analytics/log-analytics-tutorial-dashboards.md) an Azure-Dashboards anheften.

## <a name="how-do-i-check-my-usage"></a>Wie überprüfe ich meine Nutzung?
Die Nutzung und Kosten von Log Analytics können Sie nun einfach einsehen und verwalten, indem Sie in Ihrem Arbeitsbereich **Nutzung und geschätzte Kosten** auswählen.

![Nutzung und geschätzte Kosten](media/log-analytics-oms-portal-faq/usage.png)


## <a name="can-i-still-use-the-classic-portal"></a>Kann ich das klassische Portal weiterhin nutzen?
Für eine begrenzte Zeit können Sie weiterhin über diese URL mit Ihrem eigenen Arbeitsbereichsnamen auf das Portal zugreifen: https://\<Ihr Arbeitsbereichsname\>.portal.mms.microsoft.com. Die Nutzung des Azure-Portals wird jedoch empfohlen. Bitte senden Sie uns bei Blockierproblemen Ihr Feedback unter LAUpgradeFeedback@microsoft.com.

## <a name="next-steps"></a>Nächste Schritte

- [Suchen und Installieren Sie Verwaltungslösungen](../monitoring/monitoring-solutions.md) im Azure-Portal.
- Erfahren Sie mehr zur [Protokollsuche im Azure-Portal](log-analytics-log-search-portals.md).
