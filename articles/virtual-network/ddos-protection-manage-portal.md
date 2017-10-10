---
title: Verwalten des Azure DDoS-Schutzstandards mithilfe des Azure-Portals | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie die Azure DDoS-Schutzstandardtelemetrie in Azure Monitor verwenden, um einen Angriff zu entschärfen."
services: virtual-network
documentationcenter: na
author: kumudD
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/22/2017
ms.author: kumud
ms.translationtype: HT
ms.sourcegitcommit: cb9130243bdc94ce58d6dfec3b96eb963cdaafb0
ms.openlocfilehash: 5c599b4cc867dbc9a081af3a081195b998f63954
ms.contentlocale: de-de
ms.lasthandoff: 09/26/2017

---

# <a name="manage-azure-ddos-protection-standard-using-the-azure-portal"></a>Verwalten des Azure DDoS-Schutzstandards mithilfe des Azure-Portals

>[!IMPORTANT]
>Der Azure DDoS-Schutzstandard (DDoS-Schutz) befindet sich derzeit in der Vorschauversion. Der DDoS-Schutz wird von einer begrenzten Anzahl von Azure-Ressourcen und in ausgewählten Regionen unterstützt. Sie müssen sich während der eingeschränkten Vorschau [für den Dienst registrieren](http://aka.ms/ddosprotection), um den DDoS-Schutz für Ihr Abonnement zu aktivieren. Nach der Registrierung setzt sich das DDoS-Team von Azure mit Ihnen in Verbindung, um Sie durch den Aktivierungsprozess zu leiten. Der DDoS-Schutz ist in den Regionen „USA, Osten“, „USA, Westen“ und „USA, Westen-Mitte“ verfügbar. Während der Vorschauphase entstehen für Sie für die Nutzung des Diensts keine Kosten.

In diesem Artikel erfahren Sie, wie Sie das Azure-Portal für die Aktivierung und Deaktivierung des DDoS-Schutzes und für die Nutzung der Telemetrie zur Entschärfung eines Angriffs verwenden. 

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich unter „http://portal.azure.com“ beim Azure-Portal an.

## <a name="enable-ddos-protection"></a>Aktivieren des DDoS-Schutzes

Aktivieren des DDoS-Schutzes in einem neuen oder vorhandenen virtuellen Netzwerk

### <a name="create-a-new-virtual-network-and-enable-ddos-protection"></a>Erstellen eines neuen virtuellen Netzwerks und Aktivieren des DDoS-Schutzes

1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **Neu**.
2. Klicken Sie auf **Netzwerk** und anschließend auf **Virtuelles Netzwerk**.
3. Geben Sie die Informationen zum virtuellen Netzwerk ein. Klicken Sie unter *DDoS-Schutz* auf **Aktiviert** und anschließend auf **Erstellen**.

    ![Virtuelles Netzwerk erstellen](./media/ddos-protection-manage-portal/ddos-create-vnet.png)   

Eine Warnung besagt, dass für die Aktivierung des DDoS-Schutzes Gebühren anfallen. Während der Vorschauphase fallen für den DDoS-Schutz keine Gebühren an. Gebühren fallen bei allgemeiner Verfügbarkeit an, und Kunden erhalten 30 Tage vor dem Start der Gebühren und der allgemeinen Verfügbarkeit eine Benachrichtigung.

### <a name="enable-ddos-protection-on-an-existing-virtual-network"></a>Aktivieren des DDoS-Schutzes in einem vorhandenen virtuellen Netzwerk 

1. Klicken Sie auf **Virtuelles Netzwerk** im Menü des Azure-Portals, und wählen Sie anschließend Ihr virtuelles Netzwerk aus.
2. Klicken Sie auf **DDoS-Schutz** und in der Anzeige *DDoS-Schutz* auf **Aktiviert**. Anschließend klicken Sie auf **Speichern**. 

Eine Warnung besagt, dass für die Aktivierung des DDoS-Schutzes Gebühren anfallen. Während der Vorschauphase fallen für den DDoS-Schutz keine Gebühren an. Gebühren fallen bei allgemeiner Verfügbarkeit an, und Kunden erhalten 30 Tage vor dem Start der Gebühren und der allgemeinen Verfügbarkeit eine Benachrichtigung.

## <a name="disable-ddos-protection"></a>Deaktivieren des DDoS-Schutzes

1. Klicken Sie auf **Virtuelles Netzwerk** im Menü des Azure-Portals, und wählen Sie anschließend Ihr virtuelles Netzwerk aus.
2. Klicken Sie auf **DDoS-Schutz** und in der Anzeige *DDoS-Schutz* auf **Deaktiviert**. Anschließend klicken Sie auf **Speichern**.

## <a name="configure-alerts-on-ddos-protection-metrics"></a>Konfigurieren von Warnungen in DDoS-Schutzmetriken

Durch die Nutzung der Warnungskonfiguration von Azure Monitor können Sie jede der verfügbaren DDoS-Schutzmetriken zum Warnen auswählen, wenn während eines Angriffes eine aktive Entschärfung vorhanden ist. Wenn die Bedingungen erfüllt sind, erhalten Sie eine E-Mail mit einer Warnung an die angegebene Adresse.

1. Klicken Sie auf **Überwachung** und anschließend auf **Metriken**.
2. Wählen Sie in der Anzeige *Metriken* die Ressourcengruppe, den Ressourcentyp **Öffentliche IP-Adresse** und Ihre öffentliche Azure-IP aus.
3. Klicken Sie auf **Klicken Sie, um eine Warnung hinzuzufügen**, um eine E-Mail-Warnung für eine Metrik zu konfigurieren. Eine E-Mail-Warnung kann für jede Metrik erstellt werden, aber die offensichtlichste Metrik ist **Unter DDoS-Angriff oder nicht**. Dies ist ein boolescher Wert 1 oder 0. **1** bedeutet, dass Sie angegriffen werden. **0** bedeutet, dass Sie nicht angegriffen werden.
4. Legen Sie die Metrik für **Unter DDoS-Angriff oder nicht** und **Condition to Greater than zero (0) over the last 5 minutes** (Bedingung: Größer als null (0) in den letzten fünf Minuten) fest, um bei einer Attacke eine E-Mail zu erhalten. Für andere Metriken können ähnliche Warnungen eingerichtet werden.

    ![Konfigurieren von Metriken](./media/ddos-protection-manage-portal/ddos-metrics.png)

    Innerhalb weniger Minuten nach Angriffserkennung werden Sie mithilfe der Metriken von Azure Monitor benachrichtigt.

    ![Angriffswarnung](./media/ddos-protection-manage-portal/ddos-alert.png) 

Sie können auch mehr über das [Konfigurieren von Webhooks](../monitoring-and-diagnostics/insights-webhooks-alerts.md) und über [Logik-Apps](../logic-apps/logic-apps-what-are-logic-apps.md) für die Erstellung von Warnungen erfahren.

## <a name="configure-logging-on-ddos-protection-metrics"></a>Konfigurieren der Protokollierung in DDoS-Schutzmetriken

1. Klicken Sie auf **Überwachung** und anschließend auf **Diagnoseeinstellungen**.
2. Wählen Sie in der Anzeige *Metriken* die Ressourcengruppe, den Ressourcentyp **Öffentliche IP-Adresse** und Ihre öffentliche Azure-IP aus.
3. Klicken Sie auf **Diagnose aktivieren zum Erfassen der folgenden Daten**.

Es stehen drei Optionen für die Protokollierung zur Verfügung:

- **In ein Speicherkonto archivieren**: Schreibt Protokolle in ein Speicherkonto.
- **An einen Event Hub streamen**: Erlaubt einem Protokollempfänger das Erfassen von Protokollen mithilfe von Event Hub. Dies ermöglicht die Integration mit Splunk und anderen SIEM-Systemen.
- **An Log Analytics senden**: Schreibt Protokolle in den Dienst Azure OMS Log Analytics.

## <a name="use-ddos-protection-telemetry"></a>Verwenden der DDoS-Schutztelemetrie

Die Telemetrie für einen Angriff wird in Echtzeit durch Azure Monitor bereitgestellt. Die Telemetrie ist nur so lange verfügbar, wie eine öffentliche IP-Adresse der Entschärfung unterliegt. Vor oder nach der Entschärfung einer Attacke wird Ihnen keine Telemetrie angezeigt.

1. Klicken Sie auf **Überwachung** und anschließend auf **Metriken**. 
2. Wählen Sie in der Anzeige *Metriken* die Ressourcengruppe, den Ressourcentyp **Öffentliche IP-Adresse** und Ihre öffentliche Azure-IP aus. Eine Reihe von verfügbaren Metriken erscheint auf der rechten Seite des Bildschirms. Diese Metriken werden, wenn sie ausgewählt sind, im Metrikdiagramm von Azure Monitor auf dem Übersichtsbildschirm grafisch dargestellt. 

Die Metriknamen stellen verschiedene Pakettypen und -bytes im Vergleich mit Paketen mit einem grundlegenden Konstrukt von Tagnamen für jede Metrik dar:

- **Gelöschter Tagname (z.B. Als DDoS eingehende gelöschte Pakete):** Die Anzahl von durch das DDoS-Schutzsystem gelöschter/bereinigter Pakete
- **Weitergeleiteter Tagname (z.B. Weitergeleitete als DDoS eingehende Pakete):** Die Anzahl von durch das DDoS-System an das Ziel VIP weitergeleiteter Pakete – Datenverkehr, der nicht gefiltert wurde.
- **Kein Tagname (z.B. Als DDoS eingehende Pakete):** Die gesamte Anzahl von Paketen, die in das Bereinigungssystem gelangt sind, welche die Summe der gelöschten und bereinigten Pakete darstellt.

## <a name="next-steps"></a>Nächste Schritte

- [Informieren Sie sich ausführlicher über Azure-Diagnoseprotokolle.](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)
- [Analysieren von Protokollen aus Azure Storage mit Log Analytics](../log-analytics/log-analytics-azure-storage.md)
- [Erste Schritte mit Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
