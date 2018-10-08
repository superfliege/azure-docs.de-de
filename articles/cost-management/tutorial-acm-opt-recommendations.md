---
title: Tutorial – Reduzieren der Azure-Kosten mit Optimierungsempfehlungen | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie mithilfe von Optimierungsempfehlungen die Azure-Kosten senken können.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 09/21/2018
ms.topic: conceptual
ms.service: cost-management
manager: dougeby
ms.custom: ''
ms.openlocfilehash: 4d9e47d6da45eaba19cbe089de3fdf053c36046a
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47030676"
---
# <a name="tutorial-optimize-costs-from-recommendations"></a>Tutorial: Optimieren von Kosten mithilfe von Empfehlungen

Azure Cost Management unterbreitet Ihnen mithilfe von Azure Advisor Empfehlungen zur Kostenoptimierung. Azure Advisor zeigt Ihnen Möglichkeiten der Optimierung und Steigerung der Effizienz auf, indem ungenutzte oder nicht ausreichend genutzte Ressourcen ermittelt werden. Dieses Tutorial führt Sie durch ein Beispiel, bei dem Sie nicht ungenutzte Azure-Ressourcen identifizieren und anschließend Maßnahmen zur Kostenreduzierung ergreifen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Anzeigen von Empfehlungen zur Kostenoptimierung, um mögliche ineffiziente Nutzungen aufzudecken
> * Umsetzen einer Empfehlung, um die Größe eines virtuellen Computers auf eine kostengünstigere Option umzustellen
> * Überprüfen der Aktion, um sicherzustellen, dass die Größe des virtuellen Computers erfolgreich geändert wurde

## <a name="prerequisites"></a>Voraussetzungen
Empfehlungen stehen allen [EA-Kunden (Enterprise Agreement)](https://azure.microsoft.com/pricing/enterprise-agreement/) zur Verfügung. Zum Aufrufen von Kostendaten benötigen Sie für einen oder mehrere der folgenden Bereiche mindestens Lesezugriff.

- Abonnement
- Ressourcengruppe

Sie benötigen aktive virtuelle Computer, die schon mindestens 14 Tage aktiv sind.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure
Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com/) beim Azure-Portal an.

## <a name="view-cost-optimization-recommendations"></a>Anzeigen von Empfehlungen zur Kostenoptimierung

Klicken Sie im Azure-Portal in der Liste der Dienste auf **Kostenverwaltung und Abrechnung**. Wählen Sie dann in der Liste unter **Cost Management** die Option **Advisor-Empfehlungen**. Es werden die Kostenempfehlungen von Advisor angezeigt.

![Advisor-Empfehlungen](./media/tutorial-acm-opt-recommendations/advisor-recommendations.png)

Die Liste der Empfehlungen identifiziert Nutzungsineffizienzen oder zeigt Kaufempfehlungen, die Ihnen helfen können, zusätzliches Geld zu sparen. Die Summe **Potenzielle jährliche Einsparungen** zeigt den Gesamtbetrag, den Sie sparen können, wenn Sie alle Ihre VMs, die den Empfehlungsregeln entsprechen, herunterfahren oder freigeben. Wenn Sie sie nicht herunterfahren möchten, sollten Sie eine Größenänderung auf eine kostengünstigere VM-SKU in Betracht ziehen.

Die Kategorie **Auswirkungen** und die Kategorie **Potenzielle jährliche Einsparungen** sollen helfen, Empfehlungen zu identifizieren, die das Potenzial haben, so viel wie möglich zu sparen. Die wirkungsvollsten Empfehlungen sind [Erwerben reservierter VM-Instanzen, um gegenüber dem nutzungsbasierten Modell Kosten einzusparen](../advisor/advisor-cost-recommendations.md#buy-reserved-virtual-machine-instances-to-save-money-over-pay-as-you-go-costs) und [Optimieren der Kosten für virtuelle Computer durch Ändern der Größe oder Herunterfahren von zu gering ausgelasteten Instanzen](../advisor/advisor-cost-recommendations.md#optimize-virtual-machine-spend-by-resizing-or-shutting-down-underutilized-instances). Empfehlungen mit mittlerer Auswirkung sind [Kostensenkung durch die Beseitigung nicht bereitgestellter ExpressRoute-Verbindungen](../advisor/advisor-cost-recommendations.md#reduce-costs-by-eliminating-unprovisioned-expressroute-circuits) und [Verringern der Kosten durch Löschen oder Neukonfigurieren von Gateways für virtuelle Netzwerke im Leerlauf](../advisor/advisor-cost-recommendations.md#reduce-costs-by-deleting-or-reconfiguring-idle-virtual-network-gateways).

## <a name="act-on-a-recommendation"></a>Umsetzen einer Empfehlung

Azure Advisor überwacht die Verwendung Ihrer virtuellen Computer 14 Tage lang und ermittelt nicht ausgelastete virtuelle Computer. Virtuelle Computer, bei denen an mindestens vier Tagen die CPU-Auslastung unter fünf Prozent und die Netzwerklast unter sieben MB lag, gelten als virtuelle Computer mit geringer Auslastung.

Die CPU-Auslastung von 5 % oder weniger ist die Standardeinstellung, aber Sie können die Einstellungen anpassen. Weitere Informationen zum Anpassen der Einstellung finden Sie unter den [Konfigurieren der Regel für die durchschnittliche CPU-Auslastung ](../advisor/advisor-get-started.md#configure-the-average-cpu-utilization-rule-for-the-low-usage-virtual-machine-recommendation) [ mit der Empfehlung für VMs mit geringer Auslastung](../advisor/advisor-get-started.md#configure-the-average-cpu-utilization-rule-for-the-low-usage-virtual-machine-recommendation).

Obwohl einige Szenarien zielgerichtet eine geringe Auslastung bewirken, können Sie häufig Kosten sparen, indem Sie die Größe des virtuellen Computers auf eine kostengünstigere Größe verkleinern. Die tatsächlichen Einsparungen können variieren, wenn Sie eine Maßnahme zur Größenänderung auswählen. Schauen wir uns ein Beispiel für die Größenänderung bei einem virtuellen Computer an.

Klicken Sie in der Liste der Empfehlungen auf **Größe eines nicht ausgelasteten virtuellen Computers ändern oder virtuellen Computer herunterfahren**. Wählen Sie in der Liste der Kandidaten für virtuelle Computer einen virtuellen Computer aus, dessen Größe Sie ändern möchten, und klicken Sie dann auf den virtuellen Computer. Die Details des virtuellen Computers werden angezeigt, sodass Sie die Nutzungsmetriken überprüfen können. Der Wert der **potenziellen jährliche Einsparungen** gibt an, was Sie sparen können, wenn Sie die VM herunterfahren oder entfernen. Mit einer Größenänderung einer VM können Sie wahrscheinlich Geld sparen, aber Sie werden nicht den vollen Betrag der möglichen jährlichen Einsparungen erzielen.

![Empfehlungsdetails](./media/tutorial-acm-opt-recommendations/recommendation-details.png)

Überprüfen Sie in den VM-Details die Auslastung des virtuellen Computers, um sicherzustellen, dass es sich um einen geeigneten Kandidaten für die Größenänderung handelt.

![VM-Detail](./media/tutorial-acm-opt-recommendations/vm-details.png)

Beachten Sie die aktuelle VM-Größe. Nachdem Sie überprüft haben, ob die Größe des virtuellen Computers geändert werden soll, schließen Sie die VM-Details, sodass wieder die Liste der virtuellen Computer angezeigt wird.

Wählen Sie in der Liste der Kandidaten, die heruntergefahren oder deren Größe geändert werden soll, auf **Größe des virtuellen Computers ändern**.
![Ändern der Größe des virtuellen Computers](./media/tutorial-acm-opt-recommendations/resize-vm.png)

Als Nächstes wird eine Liste der verfügbaren Optionen für die Größenänderung angezeigt. Wählen Sie diejenige, die für Ihr Szenario die beste Leistung und Kosteneffizienz bietet. Im folgenden Beispiel ändert die gewählte Option die Größe von **DS14\_V2** auf **DS13\_V2**. Die Umsetzung der Empfehlung spart 551,30 USD/Monat oder 6.615,60 USD/Jahr.

![Auswählen der Größe](./media/tutorial-acm-opt-recommendations/choose-size.png)

Nachdem Sie eine geeignete Größe ausgewählt haben, klicken Sie auf **Auswählen**, um die Aktion zum Ändern der Größe zu starten.

Die Größenänderung erfordert einen Neustart eines aktiv laufenden virtuellen Computers. Wenn sich der virtuelle Computer in einer Produktionsumgebung befindet, empfehlen wir Ihnen, den Vorgang zur Größenänderung außerhalb der Geschäftszeiten durchzuführen. Durch eine Planung des Neustarts kann Unterbrechungen durch eine vorübergehende Nichtverfügbarkeit reduzieren werden.

## <a name="verify-the-action"></a>Überprüfen der Aktion

Wenn die Größenänderung der VM erfolgreich abgeschlossen ist, wird eine Azure-Benachrichtigung angezeigt.

![Benachrichtigung zur Größenänderung](./media/tutorial-acm-opt-recommendations/resized-notification.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Anzeigen von Empfehlungen zur Kostenoptimierung, um mögliche ineffiziente Nutzungen aufzudecken
> * Umsetzen einer Empfehlung, um die Größe eines virtuellen Computers auf eine kostengünstigere Option umzustellen
> * Überprüfen der Aktion, um sicherzustellen, dass die Größe des virtuellen Computers erfolgreich geändert wurde

Wenn Sie den Artikel über bewährte Methoden im Bereich Cost Management noch nicht gelesen haben, finden Sie hier allgemeine Anleitungen und Grundsätze, die Sie für eine bessere Kostenverwaltung berücksichtigen sollten.

> [!div class="nextstepaction"]
> [Bewährte Methoden für Cost Management](cost-mgt-best-practices.md)
