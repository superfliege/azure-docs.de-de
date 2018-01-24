---
title: "Erkunden der neuen Oberfläche „Warnungen (Vorschauversion)“ in Azure Monitor | Microsoft-Dokumentation"
description: "Erkunden Sie, wie die neue und skalierbare Oberfläche in Azure die Erstellung, Anzeige und Verwaltung von Warnungen vereinfacht."
author: manishsm-msft
manager: kmadnani1
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/12/2017
ms.author: mamit
ms.custom: 
ms.openlocfilehash: 5ded43548d9aea106c6e083476df4e735b8c00a6
ms.sourcegitcommit: b7adce69c06b6e70493d13bc02bd31e06f291a91
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/19/2017
---
# <a name="explore-the-new-alerts-preview-experience-in-azure-monitor"></a>Erkunden der neuen Oberfläche „Warnungen (Vorschauversion)“ in Azure Monitor

## <a name="overview"></a>Übersicht
 Die Oberfläche für Warnungen in Azure ist mit einem neuen Design und überarbeiteter Funktionalität ausgestattet. Diese neue Oberfläche ist in Azure Monitor auf der Registerkarte **Warnungen (Vorschauversion)** verfügbar. Im Folgenden werden einige Vorteile durch die Verwendung der neuen Oberfläche „Warnungen (Vorschauversion)“ vorgestellt:

 - **Unterscheidung zwischen ausgelösten Warnungen und Warnungsregeln** – Bei der Oberfläche „Warnungen (Vorschauversion)“ wird zwischen Warnungsregeln (der Definition der Bedingung, die eine Warnung auslöst) und ausgelösten Warnungen (einer Instanz zur Auslösung von Warnungsregeln) unterschieden, sodass die Betriebs- und Konfigurationansichten getrennt sind. 
 - **Einheitliche Oberfläche für die Erstellung von Metrik- und Protokollwarnungen** – Die neue Oberfläche „Warnungen (Vorschauversion)“ zum Erstellen führt den Benutzer durch die Konfiguration einer Warnungsregel, was die Auswahl der entsprechenden Warnungselemente vereinfacht. 
 - **Anzeige von ausgelösten Log Analytics-Warnungen im Azure-Portal** – Auf der Oberfläche „Warnungen (Vorschauversion)“ können Sie nun auch ausgelöste Log Analytics-Warnungen in Ihrem Abonnement sehen.  

In den folgenden Abschnitten wird die Funktionsweise der neuen Oberfläche ausführlicher beschrieben. 

## <a name="taxonomy"></a>Taxonomie
Die Oberfläche „Warnungen (Vorschauversion)“ verwendet die folgenden Begriffe, um zwischen Objekten für Benachrichtigungsregeln und ausgelöste Warnungen zu unterscheiden, während die Oberfläche für die Erstellung für verschiedene Warnungstypen vereinheitlicht wird.

- **Zielressource**: Ein Ziel kann eine beliebige Azure-Ressource sein. Eine Zielressource definiert den Umfang und die für Warnungen verfügbaren Signale. Beispiele für Ziele: Virtuelle Computer, Speicherkonten, VM-Skalierungsgruppen, Log Analytics-Arbeitsbereiche oder Lösungen. 

- **Kriterien**: Kriterien sind eine Kombination aus Signal und Logik, die auf eine Ressource angewendet wird. Beispiele: CPU in Prozent > 70 %, Serverantwortzeit > 4 ms, Anzahl der Ergebnisse einer Protokollabfrage > 100 etc. 

- **Signal**: Signale werden von der Zielressource ausgegeben und können verschiedene Typen annehmen. Diese Vorschau unterstützt Metrik- und Protokollsignaltypen.

- **Logik**: Eine benutzerdefinierte Logik, um zu überprüfen, ob das Signal den erwarteten Bereich bzw. die erwarten Werte aufweist.  
 
- **Aktion**: Eine bestimmte Kontaktaufnahme, die an den Empfänger einer Benachrichtigung gesendet wird (z.B. E-Mail an eine Adresse oder das Posten an eine Webhook-URL). Benachrichtigungen können in der Regel mehrere Aktionen auslösen. Die in dieser Vorschauversion unterstützten Warnungstypen unterstützen Aktionsgruppen.  
 
- **Warnungsregel**: Dies ist die Definition einer Bedingung, die die Warnung auslösen würde. In dieser Vorschauversion erfasst eine Warnungsregel das Ziel und die Kriterien für Warnungen. Die Warnungsregel kann sich im Zustand „Aktiviert“ oder „Deaktiviert“ befinden. 
 
- **Ausgelöste Warnung**: Diese wird erstellt, wenn eine aktivierte Warnungsregel ausgelöst wird. Das Objekt für ausgelöste Warnungen kann sich im Zustand „Ausgelöst“ oder „Nicht aufgelöst“ befinden.

    > [!NOTE]
    > In diesem Punkt unterscheiden sich diese Warnungen von der aktuellen Warnungsoberfläche, auf der die Warnung sowohl die Regel als auch die ausgelöste Warnung darstellt und daher den Warnungszustand „Aktiviert“ oder „Deaktiviert“ aufweisen kann.
    >

## <a name="single-place-to-view-and-manage-alerts"></a>Zentrale Oberfläche zum Anzeigen und Verwalten von Warnungen
Die Oberfläche „Warnungen (Vorschauversion)“ soll all Ihre Azure Warnungen zentral anzeigen und verwalten. In den folgenden Unterabschnitten werden die Funktionen der einzelnen Bildschirme der neuen Oberfläche beschrieben.

### <a name="alerts-preview-overview-page"></a>Übersichtsseite „Warnungen (Vorschauversion)“
Die Übersichtsseite **Monitor – Warnungen (Vorschauversion)** enthält eine aggregierte Zusammenfassung von allen ausgelösten Warnungen und konfigurierten bzw. aktivierten Warnungsregeln. Sie zeigt außerdem eine Liste aller ausgelösten Warnungen an. Durch Ändern der Abonnements oder Filterparameter wird die Liste der Aggregate und ausgelösten Warnungen aktualisiert.

> [!NOTE]
> Die auf der Oberfläche „Warnungen (Vorschauversion)“ angezeigten ausgelösten Warnungen sind auf unterstützten Metrik- und Protokollwarnungen beschränkt, wobei Azure Monitor die Anzahl der ausgelösten Warnungen sowie älteren Azure-Warnungen anzeigt.

 ![alerts-preview-overview](./media/monitoring-overview-unified/alerts-preview-overview.png) 

### <a name="alert-rules-management"></a>Verwaltung von Warnungsregeln
**Monitor – Warnungen (Vorschauversion) > Regeln** ist eine einzelne Seite für die Verwaltung aller Warnungsregeln in Ihren Azure-Abonnements. Sie listet alle (aktivierten oder deaktivierten) Warnungsregeln auf und kann basierend auf Zielressourcen, Ressourcengruppen, Regelnamen oder Status sortiert werden. Warnungsregeln können auf dieser Seite auch deaktiviert bzw. aktiviert oder bearbeitet werden.  

 ![alerts-preview-rules](./media/monitoring-overview-unified/alerts-preview-rules.png)


## <a name="one-alert-authoring-experience-across-all-monitoring-sources"></a>Eine zentrale Oberfläche für die Warnungserstellung für alle Überwachungsquellen
Auf der Oberfläche „Warnungen (Vorschauversion)“ können Warnungen auf konsistente Weise erstellt werden – unabhängig vom Überwachungsdienst oder Signaltyp. Alle ausgelösten Warnungen und zugehörigen Details sind auf einer einzelnen Seite verfügbar.  
 
Die Erstellung einer Warnung ist ein dreistufiger Vorgang: Zuerst legt der Benutzer ein Ziel für die Warnung fest, wählt dann das entsprechende Signal aus und gibt anschließend die Logik an, die im Rahmen der Warnungsregel auf das Signal angewendet werden soll. Bei dieser vereinfachten Erstellung muss der Benutzer nicht mehr die Überwachungsquelle oder -signale kennen, bevor er eine Azure-Ressource auswählt. Die allgemeine Oberfläche für die Erstellung filtert automatisch die Liste der verfügbaren Signale basierend auf der ausgewählten Zielressource und führt durch die Logik zur Erstellung von Warnungen.

Weitere Informationen zum Erstellen der folgenden Warnungstypen finden Sie [hier](monitor-alerts-unified-usage.md). 
- Metrikwarnungen (auf der aktuellen Oberfläche als „Metrikwarnungen nahezu in Echtzeit“ bezeichnet)
- Protokollwarnungen (Log Analytics)
 

## <a name="alert-types-supported-in-this-preview"></a>In dieser Vorschauversion unterstützte Warnungstypen


| **Signaltyp** | **Überwachungsquelle** | **Beschreibung** | 
|-------------|----------------|-------------|
| Metrik | Azure Monitor | Die auf der aktuellen Oberfläche als [**Metrikwarnungen nahezu in Echtzeit**](monitoring-near-real-time-metric-alerts.md) bezeichneten Metrikwarnungen unterstützen das Auswerten von Metrikbedingungen in Intervallen von bis zu 1 Minute und ermöglichen Regeln mit mehreren Metriken. Eine Liste der unterstützten Ressourcentypen finden Sie [hier](monitoring-near-real-time-metric-alerts.md#what-resources-can-i-create-near-real-time-metric-alerts-for). Andere der [dieser Definition](monitoring-overview-alerts.md#alerts-in-different-azure-services) entsprechenden Metrikwarnungen werden nicht auf der Oberfläche „Warnungen (Vorschauversion)“ unterstützt.|
| Protokolle  | Log Analytics | Erhalten Sie Benachrichtigungen, oder führen Sie automatisierte Aktionen aus, wenn eine Protokollsuchabfrage für Metrik- und/oder Ereignisdaten bestimmte Kriterien erfüllt.|
| Protokolle  | Aktivitätsprotokolle | Nicht auf der Oberfläche „Warnungen (Vorschauversion)“ unterstützt |
| Protokolle  | Application Insights | Nicht auf der Oberfläche „Warnungen (Vorschauversion)“ unterstützt |
| Metrik | Application Insights | Nicht auf der Oberfläche „Warnungen (Vorschauversion)“ unterstützt |
| Verfügbarkeitstests | Application Insights | Nicht auf der Oberfläche „Warnungen (Vorschauversion)“ unterstützt |


## <a name="next-steps"></a>Nächste Schritte
- [Erfahren Sie mehr über das Erstellen, Anzeigen und Verwalten von Warnungen mithilfe der neuen Oberfläche „Warnungen (Vorschauversion)“.](monitor-alerts-unified-usage.md)
- [Erfahren Sie mehr über Protokollwarnungen auf der Oberfläche „Warnungen (Vorschauversion)“.](monitor-alerts-unified-log.md)
- [Erfahren Sie mehr über Metrikwarnungen auf der Azure-Oberfläche „Warnungen (Vorschauversion)“.](monitoring-near-real-time-metric-alerts.md)

