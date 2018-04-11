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
ms.openlocfilehash: 16e0fc493a257504e2708336e05c30b36d4bea15
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2018
---
# <a name="the-new-alerts-experience-in-azure-monitor"></a>Die neue Oberfläche „Warnungen“ in Azure Monitor

## <a name="overview"></a>Übersicht

> [!NOTE]
> Dieser Artikel beschreibt neuere Warnungen. Ältere klassische Azure Monitor-Warnungen werden in [Was sind Warnungen in Microsoft Azure?](monitoring-overview-alerts.md) beschrieben. 
>
>

Für Warnungen gibt es eine neue Oberfläche. Die ältere Oberfläche „Warnungen“ befindet sich ab sofort auf der Registerkarte „Warnungen (Klassisch)“. Die neuere Oberfläche „Warnungen“ bietet gegenüber der älteren Oberfläche „Warnungen (Klassisch)“ folgende Vorteile:

-   **Besseres Benachrichtigungssystem:** Alle neuere Warnungen verwenden [Aktionsgruppen]( https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-action-groups). Hierbei handelt es sich um benannte Gruppen von Benachrichtigungen und Aktionen, die in mehreren Warnungen wiederverwendet werden können.  Klassische Metrikwarnungen und ältere Log Analytics-Warnungen verwenden keine Aktionsgruppen. 
- **Einheitliche Oberfläche für die Erstellung** – Alle Funktionen zum Erstellen von Warnungen für Metriken, Protokolle und Aktivitätsprotokolle für Azure Monitor, Log Analytics und Application Insights befinden sich an einem Ort. 
- **Anzeige von ausgelösten Log Analytics-Warnungen im Azure-Portal** – Sie können nun auch ausgelöste Log Analytics-Warnungen in Ihrem Abonnement sehen. Zuvor wurden diese in einem separaten Portal angezeigt. 
- **Unterscheidung zwischen ausgelösten Warnungen und Warnungsregeln** – Warnungsregeln (der Definition der Bedingung, die eine Warnung auslöst) und ausgelöste Warnungen (einer Instanz zur Auslösung von Warnungsregeln) werden unterschieden, sodass die Betriebs- und Konfigurationsansichten getrennt sind.
- **Verbesserter Workflow** – Die neue Oberfläche zum Erstellen von Warnungen führt den Benutzer durch die Konfiguration einer Warnungsregel, was die Auswahl der entsprechenden Warnungselemente vereinfacht.
 
Neuere metrische Warnungen haben insbesondere folgende Verbesserungen:
-   **Kürzere Wartezeit:** Neuere Metrikwarnungen können im Minutentakt ausgeführt werden. Ältere Metrikwarnungen werden immer nur alle fünf Minuten ausgeführt. Aufgrund der Zeit, die für die Erfassung der Protokolle benötigt wird, beträgt die Verzögerung bei Protokollwarnungen weiterhin mehr als eine Minute. 
-   **Unterstützung für mehrdimensionale Metriken**: Es können für dimensionale Metriken Warnungen ausgegeben werden, sodass Sie ein interessantes Segment der Metrik überwachen können.
-   **Mehr Kontrolle über Metrikbedingungen:** Sie können umfangreichere Warnungsregeln definieren. Die neueren Warnungen unterstützen die Überwachung der maximalen, minimalen und durchschnittlichen Werte sowie der Gesamtwerte der Metriken.
-   **Kombinierte Überwachung mehrerer Metriken:** Sie können mehrere Metriken (aktuell bis zu zwei) mit einer einzelnen Regel überwachen. Eine Warnung wird ausgelöst, wenn beide Metriken ihren jeweiligen Schwellenwert für den angegebenen Zeitraum überschreiten.
-   **Metriken aus Protokollen** (eingeschränkte Public Preview): Einige der bei Log Analytics eingehenden Protokolldaten können jetzt extrahiert und in Azure Monitor-Metriken konvertiert werden. Dadurch können sie genau wie andere Metriken in Warnungen genutzt werden. 



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

 ![Warnungen – Übersicht](./media/monitoring-overview-unified-alerts/alerts-preview-overview2.png) 

### <a name="alert-rules-management"></a>Verwaltung von Warnungsregeln
**Monitor – Warnungen > Regeln** ist eine einzelne Seite für die Verwaltung aller Warnungsregeln in Ihren Azure-Abonnements. Sie listet alle (aktivierten oder deaktivierten) Warnungsregeln auf und kann basierend auf Zielressourcen, Ressourcengruppen, Regelnamen oder Status sortiert werden. Warnungsregeln können auf dieser Seite auch deaktiviert bzw. aktiviert oder bearbeitet werden.  

 ![Warnungsregeln](./media/monitoring-overview-unified-alerts/alerts-preview-rules.png)


## <a name="one-alert-authoring-experience-across-all-monitoring-sources"></a>Eine zentrale Oberfläche für die Warnungserstellung für alle Überwachungsquellen
Auf der neuen Oberfläche „Warnungen“ können Warnungen auf konsistente Weise erstellt werden – unabhängig vom Überwachungsdienst oder Signaltyp. Alle ausgelösten Warnungen und zugehörigen Details sind auf einer einzelnen Seite verfügbar.  
 
Die Erstellung einer Warnung ist ein dreistufiger Vorgang: Zuerst legt der Benutzer ein Ziel für die Warnung fest, wählt dann das entsprechende Signal aus und gibt anschließend die Logik an, die im Rahmen der Warnungsregel auf das Signal angewendet werden soll. Bei dieser vereinfachten Erstellung muss der Benutzer nicht mehr die Überwachungsquelle oder -signale kennen, bevor er eine Azure-Ressource auswählt. Die allgemeine Oberfläche für die Erstellung filtert automatisch die Liste der verfügbaren Signale basierend auf der ausgewählten Zielressource und führt durch die Logik zur Erstellung von Warnungen.

Weitere Informationen zum Erstellen der folgenden Warnungstypen finden Sie [hier](monitor-alerts-unified-usage.md).
- Metrikwarnungen
- Protokollwarnungen (Log Analytics)
- Protokollwarnungen (Aktivitätsprotokolle)
- Protokollwarnungen (Application Insights)

 
## <a name="alerts-supported-in-new-experience"></a>Auf der neuen Oberfläche unterstützte Warnungen
Warnungen sind übergreifend für mehrere Azure-Überwachungsdienste verfügbar. Informationen dazu, wie und wann Sie diese Dienste verwenden können, finden Sie in [diesem Artikel](./monitoring-overview.md). Hier ist eine Aufschlüsselung der in Azure verfügbaren Warnungstypen und dessen, was derzeit von der neuen Warnungenoberfläche unterstützt wird. 


| **Signaltyp** | **Überwachungsquelle** | **Beschreibung** | 
|-------------|----------------|-------------|
| Metrik | Azure Monitor | Die auch als [Metrikwarnungen nahezu in Echtzeit](monitoring-near-real-time-metric-alerts.md) bezeichneten Metrikwarnungen unterstützen das Auswerten von Metrikbedingungen in Intervallen von bis zu 1 Minute und ermöglichen Regeln mit mehreren Metriken sowie mehrdimensionale Metrikregeln. Eine Liste der unterstützten Ressourcentypen finden Sie [hier](monitoring-near-real-time-metric-alerts.md#metrics-and-dimensions-supported). |
| Metrik | Azure Monitor | [Ältere klassische Metrikwarnungen](monitoring-overview-alerts.md) werden nicht von der neuen Warnungenoberfläche unterstützt. Sie finden diese im Azure-Portal unter „Warnungen (Klassisch)“. Die klassischen Warnungen unterstützen einige Metrikentypen, die noch nicht auf die neueren Warnungen verschoben wurden. Eine vollständige Liste finden Sie unter [Unterstützte Metriken von Azure Monitor](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-supported-metrics).
| Protokolle  | Log Analytics | Erhalten Sie Benachrichtigungen, oder führen Sie automatisierte Aktionen aus, wenn eine Protokollsuchabfrage für Metrik- und/oder Ereignisdaten bestimmte Kriterien erfüllt. Ältere Log Analytics-Warnungen sind weiterhin verfügbar, werden aber [in die neue Oberfläche kopiert](monitoring-alerts-extend.md). Darüber hinaus ist eine [Vorschau von *Log Analytics-Protokollen als Metriken*](monitoring-alerts-extend-tool.md) verfügbar. In der Vorschau können Sie einige Arten von Protokollen in Metriken konvertieren, zu denen Sie dann mit der neuen Warnungenoberfläche Warnungen erstellen können. Die Vorschau ist nützlich, wenn Sie Nicht-Azure-Protokolle parallel zu nativen Azure Monitor-Metriken abrufen möchten. |
| Aktivitätsprotokoll | Aktivitätsprotokolle (allgemein) | Enthält die Datensätze für alle Erstellen-, Aktualisieren- und Löschen-Aktionen, die über das ausgewählte Ziel (Ressource/Ressourcengruppe/Abonnement) ausgeführt werden. |
| Aktivitätsprotokoll  | Dienstintegrität | Nicht von der neuen Warnungenoberfläche unterstützt. Siehe [Erstellen von Aktivitätsprotokollwarnungen zu Dienstbenachrichtigungen](monitoring-activity-log-alerts-on-service-notifications.md).  |
| Protokolle  | Application Insights | Enthält Protokolle mit den Leistungsdaten Ihrer Anwendung. Mithilfe von Analyseabfragen und basierend auf dem Anwendungsdaten können Sie die Bedingungen für die auszuführenden Aktionen definieren. |
| Metrik | Application Insights | Nicht von der neuen Warnungenoberfläche unterstützt. Siehe [Metrikwarnungen](../application-insights/app-insights-alerts.md). |
| Webverfügbarkeitstests | Application Insights | Nicht auf der Oberfläche „Warnungen“ unterstützt.  Siehe [Webtestwarnungen](../application-insights/app-insights-monitor-web-app-availability.md). Verfügbar für jede Website, die für das Senden von Daten an Application Insights instrumentiert ist. Sie erhalten eine Benachrichtigung, wenn die Verfügbarkeit oder Reaktionsfähigkeit einer Website nicht den Erwartungen entspricht. |




## <a name="next-steps"></a>Nächste Schritte
- [Erfahren Sie mehr über das Erstellen, Anzeigen und Verwalten von Warnungen mithilfe der neuen Oberfläche „Warnungen“.](monitor-alerts-unified-usage.md)
- [Erfahren Sie mehr über Protokollwarnungen auf der Oberfläche „Warnungen“.](monitor-alerts-unified-log.md)
- [Erfahren Sie mehr über Metrikwarnungen auf der Oberfläche „Warnungen“.](monitoring-near-real-time-metric-alerts.md)
- [Erfahren Sie mehr über Aktivitätsprotokollwarnungen auf der Oberfläche „Warnungen“.](monitoring-activity-log-alerts-new-experience.md)
