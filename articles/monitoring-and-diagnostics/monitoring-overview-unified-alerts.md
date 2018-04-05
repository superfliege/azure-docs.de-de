---
title: Erkunden der neuen Oberfläche „Warnungen“ in Azure Monitor | Microsoft-Dokumentation
description: Erkunden Sie, wie die neue und skalierbare Oberfläche in Azure die Erstellung, Anzeige und Verwaltung von Warnungen vereinfacht.
author: manishsm-msft
manager: kmadnani1
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ''
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2018
ms.author: mamit
ms.custom: ''
ms.openlocfilehash: 356988e8ae743d73c8e2cc7cc106cbc5b0d1a423
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="the-new-alerts-experience-in-azure-monitor"></a>Die neue Oberfläche „Warnungen“ in Azure Monitor

## <a name="overview"></a>Übersicht
Für Warnungen gibt es eine neue Oberfläche. Die ältere Oberfläche „Warnungen“ befindet sich ab sofort auf der Registerkarte „Warnungen (Klassisch)“. Die neuere Oberfläche „Warnungen“ bietet gegenüber der älteren Oberfläche „Warnungen (Klassisch)“ folgende Vorteile:

 - **Unterscheidung zwischen ausgelösten Warnungen und Warnungsregeln** – Warnungsregeln (der Definition der Bedingung, die eine Warnung auslöst) und ausgelöste Warnungen (einer Instanz zur Auslösung von Warnungsregeln) werden unterschieden, sodass die Betriebs- und Konfigurationsansichten getrennt sind.
 - **Einheitliche Oberfläche für die Erstellung** – Alle Funktionen zum Erstellen von Warnungen für Metriken, Protokolle und Aktivitätsprotokolle für Azure Monitor, Log Analytics und Application Insights befinden sich an einem Ort. 
 - **Anzeige von ausgelösten Log Analytics-Warnungen im Azure-Portal** – Sie können nun auch ausgelöste Log Analytics-Warnungen in Ihrem Abonnement sehen. Zuvor wurden diese in einem separaten Portal angezeigt. 
 - **Verbesserter Workflow** – Die neue Oberfläche zum Erstellen von Warnungen führt den Benutzer durch die Konfiguration einer Warnungsregel, was die Auswahl der entsprechenden Warnungselemente vereinfacht.
 

In den folgenden Abschnitten wird die Funktionsweise der neuen Oberfläche ausführlicher beschrieben.

## <a name="alert-rules-terminology"></a>Terminologie für Warnungsregeln
Die neue Oberfläche „Warnungen“ verwendet die folgenden Begriffe, um zwischen Objekten für Benachrichtigungsregeln und ausgelöste Warnungen zu unterscheiden, während die Oberfläche für die Erstellung für verschiedene Warnungstypen vereinheitlicht wird.

- **Zielressource**: Ein Ziel kann eine beliebige Azure-Ressource sein. Eine Zielressource definiert den Umfang und die für Warnungen verfügbaren Signale. Beispiele für Ziele: Virtuelle Computer, Speicherkonten, VM-Skalierungsgruppen, Log Analytics-Arbeitsbereiche oder Application Insights-Ressourcen.

- **Kriterien**: Kriterien sind eine Kombination aus Signal und Logik, die auf eine Ressource angewendet wird. Beispiele: CPU in Prozent > 70 %, Serverantwortzeit > 4 ms, Anzahl der Ergebnisse einer Protokollabfrage > 100 etc. 

- **Signal**: Signale werden von der Zielressource ausgegeben und können verschiedene Typen annehmen. **Metrik**, **Aktivitätsprotokoll**, **Application Insights** und **Protokoll** gehören zu den unterstützten Signaltypen.

- **Logik**: Eine benutzerdefinierte Logik, um zu überprüfen, ob das Signal den erwarteten Bereich bzw. die erwarten Werte aufweist.  
 
- **Aktion**: Eine bestimmte Aktion, die ausgeführt wird, sobald die Warnung ausgelöst wird. Beispiele hierfür sind das Versenden einer E-Mail-Adresse oder Aufrufen einer Webhook-URL. Bei einer ausgelösten Warnung können mehrere Aktionen ausgeführt werden. Diese Warnungen unterstützen Aktionsgruppen.  
 
- **Warnungsregel**: Die Bedingung, die die Warnung auslösen würde. Die Warnungsregel erfasst das Ziel und die Kriterien für Warnungen. Die Warnungsregel kann sich im Zustand „Aktiviert“ oder „Deaktiviert“ befinden.
 
    > [!NOTE]
    > Dies ist ein Unterschied zu der Oberfläche „Warnungen (Klassisch)“, auf der die Warnung sowohl die Regel als auch die ausgelöste Warnung darstellt und daher den Warnungszustand „Aktiviert“ oder „Deaktiviert“ aufweisen kann.
    >

## <a name="single-place-to-view-and-manage-alerts"></a>Zentrale Oberfläche zum Anzeigen und Verwalten von Warnungen
Ziel der Oberfläche „Warnungen“ ist es, alle Azure-Warnungen zentral anzuzeigen und zu verwalten. In den folgenden Unterabschnitten werden die Funktionen der einzelnen Bildschirme der neuen Oberfläche beschrieben.

### <a name="alerts-overview-page"></a>Übersichtsseite für Warnungen
Die Übersichtsseite **Monitor – Warnungen** enthält eine aggregierte Zusammenfassung von allen ausgelösten Warnungen und konfigurierten bzw. aktivierten Warnungsregeln. Sie zeigt außerdem eine Liste aller ausgelösten Warnungen an. Durch Ändern der Abonnements oder Filterparameter wird die Liste der Aggregate und ausgelösten Warnungen aktualisiert.

> [!NOTE]
> Die auf der Oberfläche „Warnungen“ angezeigten ausgelösten Warnungen sind auf unterstützte Metrik- und Aktivitätsprotokollwarnungen beschränkt. In der Übersicht von Azure Monitor wird die Anzahl der ausgelösten Warnungen und auch älteren Azure-Warnungen anzeigt.

 ![Warnungen – Übersicht](./media/monitoring-overview-unified/alerts-preview-overview.png) 

### <a name="alert-rules-management"></a>Verwaltung von Warnungsregeln
**Monitor – Warnungen > Regeln** ist eine einzelne Seite für die Verwaltung aller Warnungsregeln in Ihren Azure-Abonnements. Sie listet alle (aktivierten oder deaktivierten) Warnungsregeln auf und kann basierend auf Zielressourcen, Ressourcengruppen, Regelnamen oder Status sortiert werden. Warnungsregeln können auf dieser Seite auch deaktiviert bzw. aktiviert oder bearbeitet werden.  

 ![Warnungsregeln](./media/monitoring-overview-unified/alerts-preview-rules.png)


## <a name="one-alert-authoring-experience-across-all-monitoring-sources"></a>Eine zentrale Oberfläche für die Warnungserstellung für alle Überwachungsquellen
Auf der neuen Oberfläche „Warnungen“ können Warnungen auf konsistente Weise erstellt werden – unabhängig vom Überwachungsdienst oder Signaltyp. Alle ausgelösten Warnungen und zugehörigen Details sind auf einer einzelnen Seite verfügbar.  
 
Die Erstellung einer Warnung ist ein dreistufiger Vorgang: Zuerst legt der Benutzer ein Ziel für die Warnung fest, wählt dann das entsprechende Signal aus und gibt anschließend die Logik an, die im Rahmen der Warnungsregel auf das Signal angewendet werden soll. Bei dieser vereinfachten Erstellung muss der Benutzer nicht mehr die Überwachungsquelle oder -signale kennen, bevor er eine Azure-Ressource auswählt. Die allgemeine Oberfläche für die Erstellung filtert automatisch die Liste der verfügbaren Signale basierend auf der ausgewählten Zielressource und führt durch die Logik zur Erstellung von Warnungen.

Weitere Informationen zum Erstellen der folgenden Warnungstypen finden Sie [hier](monitor-alerts-unified-usage.md).
- Metrikwarnungen
- Protokollwarnungen (Log Analytics)
- Protokollwarnungen (Aktivitätsprotokolle)
- Protokollwarnungen (Application Insights)

 

## <a name="alert-types-supported"></a>Unterstützte Warnungstypen


| **Signaltyp** | **Überwachungsquelle** | **Beschreibung** | 
|-------------|----------------|-------------|
| Metrik | Azure Monitor | Die auch als [**Near-Real-Time Metric Alerts (Metrikwarnungen nahezu in Echtzeit)**](monitoring-near-real-time-metric-alerts.md) bezeichneten Metrikwarnungen unterstützen das Auswerten von Metrikbedingungen in Intervallen von bis zu 1 Minute und ermöglichen Regeln mit mehreren Metriken. Eine Liste der unterstützten Ressourcentypen finden Sie [hier](monitoring-near-real-time-metric-alerts.md#metrics-and-dimensions-supported). Ältere, [dieser Definition](monitoring-overview-alerts.md#alerts-in-different-azure-services) entsprechende Metrikwarnungen werden nicht auf der Oberfläche „Warnungen“ unterstützt. Sie finden diese unter „Warnungen (Klassisch)“.|
| Protokolle  | Log Analytics | Erhalten Sie Benachrichtigungen, oder führen Sie automatisierte Aktionen aus, wenn eine Protokollsuchabfrage für Metrik- und/oder Ereignisdaten bestimmte Kriterien erfüllt.|
| Aktivitätsprotokoll | Aktivitätsprotokolle | Diese Kategorie enthält die Datensätze für alle Erstellen-, Aktualisieren- und Löschen-Aktionen, die über das ausgewählte Ziel (Ressource/Ressourcengruppe/Abonnement) ausgeführt werden. |
| Protokolle  | Service Health-Protokolle | Nicht auf der Oberfläche „Warnungen“ unterstützt.   |
| Protokolle  | Application Insights | Diese Kategorie enthält Protokolle mit den Leistungsdaten Ihrer Anwendung. Mithilfe von Analyseabfragen und basierend auf dem Anwendungsdaten können Sie die Bedingungen für die auszuführenden Aktionen definieren. |
| Metrik | Application Insights | Nicht auf der Oberfläche „Warnungen“ unterstützt. Sie finden diese unter „Warnungen (Klassisch)“. |
| Verfügbarkeitstests | Application Insights | Nicht auf der Oberfläche „Warnungen“ unterstützt. Sie finden diese unter „Warnungen (Klassisch)“. |


## <a name="next-steps"></a>Nächste Schritte
- [Erfahren Sie mehr über das Erstellen, Anzeigen und Verwalten von Warnungen mithilfe der neuen Oberfläche „Warnungen“.](monitor-alerts-unified-usage.md)
- [Erfahren Sie mehr über Protokollwarnungen auf der Oberfläche „Warnungen“.](monitor-alerts-unified-log.md)
- [Erfahren Sie mehr über Metrikwarnungen auf der Oberfläche „Warnungen“.](monitoring-near-real-time-metric-alerts.md)
- [Erfahren Sie mehr über Aktivitätsprotokollwarnungen auf der Oberfläche „Warnungen“.](monitoring-activity-log-alerts-new-experience.md)
