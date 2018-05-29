---
title: Mit vorab bezahlten virtuellen Azure-Computern Geld sparen – Azure | Microsoft-Dokumentation
description: Informieren Sie sich über reservierte VM-Instanzen, um Kosten für virtuelle Computer einzusparen.
services: billing
documentationcenter: ''
author: yashesvi
manager: yashesvi
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/09/2017
ms.author: yashar
ms.openlocfilehash: 9c73102f09e252b449f78603debaf707b3c89c3b
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/11/2018
ms.locfileid: "34056685"
---
# <a name="save-money-on-virtual-machines-with-reserved-virtual-machine-instances"></a>Mit reservierten VM-Instanzen Kosten für virtuelle Computer sparen 
Reservierte VM-Instanzen ermöglichen Ihnen eine Vorausbezahlung für ein oder drei Jahre Rechenkapazität, um einen Rabatt auf die von Ihnen genutzten virtuellen Computern zu erhalten. Mit einer Vorauszahlung für ein oder drei Jahre reduzieren Sie Ihre Kosten für virtuelle Computer erheblich – im Vergleich mit den Preisen bei nutzungsbasierter Bezahlung um bis zu 72 Prozent. Bei reservierten VM-Instanzen handelt es sich um einen Rechnungsrabatt, der sich nicht auf den Laufzeitstatus von virtuellen Computern auswirkt.

Sie können eine reservierte VM-Instanz im [Azure-Portal](https://aka.ms/reservations) erwerben. Weitere Informationen finden Sie unter [Vorauszahlen für virtuelle Computer und Sparen von Kosten mit reservierten VM-Instanzen](https://go.microsoft.com/fwlink/?linkid=861721).

## <a name="why-should-i-buy-a-reserved-virtual-machine-instance"></a>Warum sollte ich eine reservierte VM-Instanz kaufen?
Wenn Sie über virtuelle Computer verfügen, die für längere Zeiträume ausgeführt werden, erhalten Sie durch den Erwerb einer reservierten VM-Instanz das beste Preis-Leistungs-Verhältnis. Wenn Sie beispielsweise vier Instanzen von Standard D2 in der Region „USA, Westen“ fortlaufend ohne Reservierung ausführen, werden fallen die Gebühren für die nutzungsbasierte Bezahlung an. Wenn Sie eine reservierte VM-Instanz für diese vier virtuellen Computer erwerben, profitieren Sie sofort von Rechnungsvorteilen dieser virtuellen Computer. Sie werden nicht mehr mit den Gebühren für die nutzungsbasierte Bezahlung in Rechnung gestellt. 

## <a name="what-charges-does-a-reserved-virtual-machine-instance-cover"></a>Welche Gebühren deckt eine reservierte VM-Instanz?
Eine Reservierung deckt nur die Gebühren für die Infrastruktur von virtuellen Computern für Ihre virtuellen Windows- oder Linux-Computer. Eine Reservierung deckt keine zusätzlichen Kosten für Software, Netzwerke oder Speicher. Für virtuelle Windows-Computer können Sie die Windows-Lizenzierungskosten mit dem [Azure-Hybridvorteil](https://azure.microsoft.com/pricing/hybrid-benefit/) decken.

## <a name="whos-eligible-to-purchase-a-reserved-virtual-machine-instance"></a>Wer ist zum Erwerb einer reservierten VM-Instanz berechtigt?
Azure-Kunden mit diesen Abonnementtypen können eine reservierte VM-Instanz erwerben:
-   Enterprise Agreement-Abonnement als Angebotstyp (MS-AZR-0017P).
-   [Nutzungsbasierte Zahlung](https://azure.microsoft.com/offers/ms-azr-0003p/) als Abonnementsangebotstyp (MS-AZR-003P).
Sie müssen über die Rolle „Besitzer“ für das Abonnement verfügen, um eine reservierte Instanz zu kaufen. Für den Erwerb von Reservierungen in einer Enterprise-Registrierung muss der Enterprise-Administrator im EA-Portal Reservierungskäufe aktivieren; diese Einstellung ist standardmäßig aktiviert.
-   CSP-Partner (Cloud Solution Provider, Cloudlösungsanbieter) können Reservierungen über das Azure-Verwaltungsportal oder über [Partner Center](https://docs.microsoft.com/partner-center/azure-reservations) erwerben.

## <a name="how-is-a-reserved-virtual-machine-instances-purchase-billed"></a>Wie wird der Erwerb einer reservierten VM-Instanz in Rechnung gestellt?
Der Kauf der Reservierung wird mit der Zahlungsmethode in Rechnung gestellt, die mit dem Abonnement verknüpft ist. Wenn Sie über ein Enterprise-Abonnement verfügen, werden die Reservierungskosten von Ihrem Verpflichtungsguthaben abgezogen. Wenn Ihr Verpflichtungsguthaben die Kosten für die Reservierung nicht abdeckt, wird Ihnen die Überschreitung in Rechnung gestellt.
Wenn Sie über ein Abonnement mit nutzungsbasierter Bezahlung verfügen, wird die Kreditkarte für Ihr Konto umgehend belastet. Wenn Sie Rechnungen erhalten, sind die Gebühren Ihrer nächsten Rechnung aufgeführt.

## <a name="how-is-the-purchased-reserved-virtual-machine-instance-discount-applied"></a>Wie wird der Rabatt für reservierte VM-Instanzen angewendet?
Der Rabatt für reservierte VM-Instanzen gilt für die virtuellen Computer, die den Attributen entsprechen, die Sie beim Erwerb der Reservierung auswählen. Zu diesen Attributen gehört der Bereich, in dem die entsprechenden virtuellen Computer ausgeführt werden. Beispiel: Wenn Sie einen Rabatt auf reservierte VM-Instanzen für vier virtuelle Standard D2-Computer in der Region „USA, Westen“ nutzen möchten, wählen Sie das Abonnement aus, in dem die virtuellen Computer ausgeführt werden. Wenn die virtuellen Computer in verschiedenen Abonnements in Ihrer Registrierung/Ihrem Konto ausgeführt werden, wählen Sie den Bereich „Freigegeben“ aus. Der Bereich „Freigegeben“ ermöglicht die abonnementübergreifende Anwendung des Reservierungsrabatts.
Sie können den Bereich nach dem Erwerb einer reservierten VM-Instanz ändern. Informationen zum Ändern des Bereichs finden Sie in der Dokumentation zum Verwalten von Reservierungen.

Der Reservierungsrabatt gilt nur für virtuelle Computer in Abonnements mit den Angebotstypen „Enterprise“ oder „Nutzungsbasierte Bezahlung“. Für virtuelle Computer, die in einem Abonnement mit anderen Angebotstypen ausgeführt werden, gilt der Reservierungsrabatt nicht. Bei Enterprise-Registrierungen können die Vorteile einer reservierten VM-Instanz für Dev/Test-Abonnements nicht genutzt werden.

Auswirkungen der Reservierung auf die Abrechnung für virtuelle Computer wird in [Grundlegendes zur Anwendung des Abrechnungsvorteils von Reservierungen](https://go.microsoft.com/fwlink/?linkid=863405) erläutert.

## <a name="what-happens-when-the-reservation-term-expires"></a>Was geschieht nach Ende der Laufzeit der Reservierung?
Am Ende der Laufzeit der Reservierung läuft der Abrechnungsrabatt ab, und die Infrastruktur der virtuellen Computer wird mit den Gebühren für die nutzungsbasierte Bezahlung in Rechnung gestellt. Reservierungen werden nicht automatisch verlängert. Zur weiteren Nutzung des Abrechnungsrabatts müssen Sie eine neue reservierte VM-Instanz kaufen. 

## <a name="sizes-and-regional-availability"></a>Größen und regionale Verfügbarkeit
Reservierungen sind für die meisten VM-Größen bis auf einige Ausnahmen verfügbar:
- Vorschauversionen von VM-Größen: Vorschauversionen von Größen sind nicht für den Erwerb einer reservierten VM-Instanz verfügbar.
- Clouds – reservierte VM-Instanzen sind in den Regionen Azure US-Regierung, Deutschland und China nicht zum Kauf verfügbar. 
- Nicht genügend Kontingent: Für eine reservierte VM-Instanz, die einem einzelnen Abonnement zugeordnet ist, muss im Abonnement für die neue reservierte VM-Instanz vCPU Kontingent verfügbar sein. Beispiel: Wenn für das Zielabonnement eine Kontingentgrenze von 10 vCPUs für die Familie „D-Serie“ gilt, können Sie nicht für 11 Standard_D1-Instanzen eine reservierte VM-Instanz kaufen. Bei der Kontingentüberprüfung für Reservierungen werden die im Abonnement bereits bereitgestellten virtuellen Computer berücksichtigt. Angenommen, das Abonnement verfügt über ein Kontingent von 10 vCPUs für die D-Serie-Familie. Wenn in diesem Abonnement zwei standard_D1-Instanzen bereitgestellt sind, können Sie eine reservierte VM-Instanz für 10 standard_D1-Instanzen in diesem Abonnement erwerben. 
- Kapazitätseinschränkungen: In seltenen Fällen beschränkt Azure den Kauf neuer Reservierungen für eine Teilmenge der VM-Größen aufgrund geringer Kapazität in einer Region.

## <a name="next-steps"></a>Nächste Schritte
Sparen Sie durch den Erwerb einer [reservierten VM-Instanz](https://go.microsoft.com/fwlink/?linkid=861721) ab sofort Kosten für Ihre virtuellen Computer. 

Weitere Informationen zu reservierten VM-Instanzen finden Sie in den folgenden Artikeln.

- [Vorauszahlen für virtuelle Computer mit reservierten VM-Instanzen](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Verwalten von reservierten Azure-VM-Instanzen](billing-manage-reserved-vm-instance.md)
- [Grundlegendes zur Anwendung des Rabatts für reservierte VM-Instanzen](billing-understand-vm-reservation-charges.md)
- [Grundlagen zur Verwendung reservierter Azure-Instanzen für Ihr Abonnement mit nutzungsbasierter Zahlung](billing-understand-reserved-instance-usage.md)
- [Grundlegendes zur Nutzung reservierter Instanzen für die Enterprise-Registrierung](billing-understand-reserved-instance-usage-ea.md)
- [Nicht in reservierten Instanzen enthaltene Windows-Softwarekosten](billing-reserved-instance-windows-software-costs.md)
- [Reservierte Instanzen im Partner Center Cloud Solution Provider (CSP)-Programm](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Support kontaktieren
[Wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), falls Sie weitere Hilfe benötigen, um das Problem schnell beheben zu lassen.
