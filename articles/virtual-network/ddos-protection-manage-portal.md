---
title: Verwalten von Azure DDoS Protection Standard mithilfe des Azure-Portals | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie Azure DDoS Protection Standard-Telemetrie in Azure Monitor verwenden, um einen Angriff zu entschärfen."
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/13/2017
ms.author: jdial
ms.openlocfilehash: 6a5ab1ba44197c0103e1e7d353a116dc01dfc163
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="manage-azure-ddos-protection-standard-using-the-azure-portal"></a>Verwalten von Azure DDoS Protection Standard mithilfe des Azure-Portals

Erfahren Sie, wie Sie den DDoS-Schutz (Distributed Denial of Service) aktivieren oder deaktivieren und die Telemetrie von Azure DDoS Protection Standard verwenden, um einen DDoS-Angriff zu entschärfen. DDoS Protection Standard schützt Azure-Ressourcen wie virtuelle Computer, Lastenausgleichsmodule und Anwendungsgateways, denen eine [öffentliche Azure-IP-Adresse](virtual-network-public-ip-address.md) zugewiesen ist. Weitere Informationen zum DDoS Protection Standard-Dienst und seinen Funktionen finden Sie unter [Übersicht über DDoS Protection Standard](ddos-protection-overview.md). 

>[!IMPORTANT]
>Azure DDoS Protection Standard (DDoS Protection) befindet sich derzeit in der Vorschauversion. DDoS Protection wird von einer begrenzten Anzahl von Azure-Ressourcen unterstützt und ist nur in ausgewählten Regionen verfügbar. Eine Liste der verfügbaren Regionen finden Sie unter [Übersicht über DDoS Protection Standard](ddos-protection-overview.md). Sie müssen sich während der eingeschränkten Vorschau [für den Dienst registrieren](http://aka.ms/ddosprotection), um DDoS Protection für Ihr Abonnement zu aktivieren. Nach der Registrierung setzt sich das DDoS-Team von Azure mit Ihnen in Verbindung und führt Sie durch den Aktivierungsprozess. 

## <a name="enable-ddos-protection-standard---new-virtual-network"></a>Aktivieren von DDoS Protection Standard – Neues virtuelles Netzwerk

1. Melden Sie sich unter „http://portal.azure.com“ beim Azure-Portal an. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.
2. Klicken Sie im Azure-Portal links oben auf **Ressource erstellen**.
3. Klicken Sie auf **Netzwerk** und anschließend auf **Virtuelles Netzwerk**.
4. Erstellen Sie ein virtuelles Netzwerk mit den ausgewählten Einstellungen. Weitere Informationen zum Erstellen virtueller Netzwerke finden Sie unter [Erstellen eines virtuellen Netzwerks](manage-virtual-network.md#create-a-virtual-network). Klicken Sie unter **DDoS-Schutz** auf **Aktiviert** und anschließend auf **Erstellen**. Wenn Sie **DDoS-Schutz** nicht angezeigt wird, ist Ihr Abonnement wahrscheinlich für das Feature nicht registriert. Sie müssen die [Registrierung](http://aka.ms/ddosprotection) abschließen und eine Benachrichtigung darüber erhalten, dass Ihr Abonnement für das Feature aktiviert wurde, bevor **DDoS-Schutz** angezeigt wird.

    ![Virtuelles Netzwerk erstellen](./media/ddos-protection-manage-portal/ddos-create-vnet.png)   

    > [!WARNING]
    > Beim Auswählen einer Region wählen Sie eine unterstützte Region aus der Liste in der [Übersicht über Azure DDoS Protection Standard](ddos-protection-overview.md) aus. Wenn Sie keine unterstützte Region auswählen, tritt beim Erstellen des virtuellen Netzwerks ein Fehler auf.

    Eine Warnung gibt an, dass für die Aktivierung von DDoS Protection Gebühren anfallen. Während der Vorschauphase fallen für DDoS Protection keine Gebühren an. Gebühren fallen bei allgemeiner Verfügbarkeit an. Sie erhalten 30 Tage vor dem Start der Gebühren und der allgemeinen Verfügbarkeit eine Benachrichtigung.

## <a name="enable-ddos-protection-standard---existing-virtual-network"></a>Aktivieren von DDoS Protection Standard – Vorhandenes virtuelles Netzwerk 

1. Klicken Sie auf **Virtuelles Netzwerk** im Menü des Azure-Portals, und wählen Sie anschließend Ihr virtuelles Netzwerk aus.
2. Klicken Sie auf **DDoS Protection** und in der Anzeige *DDoS Protection* auf **Aktiviert**. Anschließend klicken Sie auf **Speichern**. Wenn Sie **DDoS-Schutz** nicht angezeigt wird, ist Ihr Abonnement wahrscheinlich für das Feature nicht registriert. Sie müssen die [Registrierung](http://aka.ms/ddosprotection) abschließen und eine Benachrichtigung darüber erhalten, dass Ihr Abonnement für das Feature aktiviert wurde, bevor **DDoS-Schutz** angezeigt wird. 

    > [!WARNING]
    > Das virtuelle Netzwerk muss in einer unterstützten Region vorhanden sein. Eine Liste der unterstützten Regionen finden Sie unter [Übersicht über Azure DDoS Protection Standard](ddos-protection-overview.md).

    Eine Warnung gibt an, dass für die Aktivierung von DDoS Protection Gebühren anfallen. Während der Vorschauphase fallen für DDoS Protection keine Gebühren an. Gebühren fallen bei allgemeiner Verfügbarkeit an. Sie erhalten 30 Tage vor dem Start der Gebühren und der allgemeinen Verfügbarkeit eine Benachrichtigung.

## <a name="disable-ddos-protection-on-a-virtual-network"></a>Deaktivieren von DDoS Protection in einem virtuellen Netzwerk

1. Klicken Sie auf **Virtuelles Netzwerk** im Menü des Azure-Portals, und wählen Sie anschließend Ihr virtuelles Netzwerk aus.
2. Klicken Sie auf **DDoS Protection** und in der Anzeige *DDoS Protection* auf **Deaktiviert**. Anschließend klicken Sie auf **Speichern**.

## <a name="configure-alerts-on-ddos-protection-metrics"></a>Konfigurieren von Warnungen in DDoS Protection-Metriken

Mithilfe der Warnungskonfiguration von Azure Monitor können Sie jede der verfügbaren DDoS Protection-Metriken zum Warnen auswählen, wenn während eines Angriffs eine aktive Entschärfung vorhanden ist. Wenn die Bedingungen erfüllt sind, trifft eine E-Mail mit einer Warnung an der angegebenen Adresse ein.

1. Klicken Sie auf **Überwachung** und anschließend auf **Metriken**.
2. Wählen Sie in der Anzeige *Metriken* die Ressourcengruppe, den Ressourcentyp **Öffentliche IP-Adresse** und Ihre öffentliche Azure-IP-Adresse aus.
3. Klicken Sie auf **Metrikwarnung hinzufügen**, um eine E-Mail-Warnung für eine Metrik zu konfigurieren. Eine E-Mail-Warnung kann für jede Metrik erstellt werden, aber die offensichtlichste Metrik ist **Unter DDoS-Angriff oder nicht**. Dies ist ein boolescher Wert 1 oder 0. **1** bedeutet, dass Sie angegriffen werden. **0** bedeutet, dass Sie nicht angegriffen werden.
4. Legen Sie die Metrik für **Unter DDoS-Angriff oder nicht** und **Condition to Greater than zero (0) over the last 5 minutes** (Bedingung: Größer als null (0) in den letzten fünf Minuten) fest, um bei einer Attacke eine E-Mail zu erhalten. Für andere Metriken können ähnliche Warnungen eingerichtet werden.

    ![Konfigurieren von Metriken](./media/ddos-protection-manage-portal/ddos-metrics.png)

    Innerhalb weniger Minuten nach Angriffserkennung werden Sie mithilfe der Metriken von Azure Monitor benachrichtigt.

    ![Angriffswarnung](./media/ddos-protection-manage-portal/ddos-alert.png) 

Sie können auch mehr über das [Konfigurieren von Webhooks](../monitoring-and-diagnostics/insights-webhooks-alerts.md) und über [Logik-Apps](../logic-apps/logic-apps-overview.md) für die Erstellung von Warnungen erfahren.

## <a name="configure-logging-on-ddos-protection-standard-metrics"></a>Konfigurieren der Protokollierung von DDoS Protection Standard-Metriken

1. Klicken Sie auf **Überwachung** und anschließend auf **Diagnoseeinstellungen**.
2. Wählen Sie in der Anzeige *Metriken* die Ressourcengruppe, den Ressourcentyp **Öffentliche IP-Adresse** und Ihre öffentliche Azure-IP-Adresse aus.
3. Klicken Sie auf **Diagnose aktivieren zum Erfassen der folgenden Daten**.

Es stehen drei Optionen für die Protokollierung zur Verfügung:

- **In ein Speicherkonto archivieren**: Schreibt Protokolle in ein Speicherkonto.
- **An einen Event Hub streamen**: Erlaubt einem Protokollempfänger das Erfassen von Protokollen mithilfe von Event Hub. Dies ermöglicht die Integration mit Splunk und anderen SIEM-Systemen.
- **An Log Analytics senden**: Schreibt Protokolle in den Dienst Azure OMS Log Analytics.

## <a name="use-ddos-protection-telemetry"></a>Verwenden der DDoS Protection-Telemetrie

Die Telemetrie für einen Angriff wird in Echtzeit durch Azure Monitor bereitgestellt. Die Telemetrie ist nur so lange verfügbar, wie eine öffentliche IP-Adresse der Entschärfung unterliegt. Vor oder nach der Entschärfung einer Attacke wird Ihnen keine Telemetrie angezeigt.

1. Klicken Sie auf **Überwachung** und anschließend auf **Metriken**. 
2. Wählen Sie in der Anzeige *Metriken* die Ressourcengruppe, den Ressourcentyp **Öffentliche IP-Adresse** und Ihre öffentliche Azure-IP-Adresse aus. Eine Reihe von **verfügbaren Metriken** erscheint auf der linken Seite des Bildschirms. Diese Metriken werden, wenn sie ausgewählt sind, im **Metrikdiagramm von Azure Monitor** auf dem Übersichtsbildschirm grafisch dargestellt. 

Die Metriknamen stellen verschiedene Pakettypen und Bytes im Vergleich zu Paketen mit einem grundlegenden Konstrukt von Tagnamen für jede Metrik dar:

- **Gelöschter Tagname (z. B. Als DDoS eingehende gelöschte Pakete):** Die Anzahl der durch das DDoS-Schutzsystem gelöschten/bereinigten Pakete.
- **Weitergeleiteter Tagname (z. B. Weitergeleitete als DDoS eingehende Pakete):** Die Anzahl der durch das DDoS-System an das Ziel-VIP weitergeleiteten Pakete – Datenverkehr, der nicht gefiltert wurde.
- **Kein Tagname (z.B. Als DDoS eingehende Pakete):** Die gesamte Anzahl von Paketen, die in das Bereinigungssystem gelangt sind, welche die Summe der gelöschten und bereinigten Pakete darstellt.

## <a name="next-steps"></a>Nächste Schritte

- [Weitere Informationen zu Azure-Diagnoseprotokollen](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Analysieren von Protokollen aus Azure Storage mit Log Analytics](../log-analytics/log-analytics-azure-storage.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Erste Schritte mit Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md?toc=%2fazure%2fvirtual-network%2ftoc.json)