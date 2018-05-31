---
title: Was sind reservierte Azure-Instanzen? – Azure-Abrechnung | Microsoft-Dokumentation
description: Informationen zu Azure Reserved Virtual Machine Instances und VM-Preisen, um Kosten für virtuelle Computer zu sparen und das beste Preis-Leistungs-Verhältnis zu erhalten.
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
ms.date: 05/09/2018
ms.author: yashar
ms.openlocfilehash: 93be4bb037af400599b88bb71f34143ee65a5deb
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/18/2018
ms.locfileid: "34301701"
---
# <a name="what-are-azure-reserved-vm-instances"></a>Was ist Azure Reserved VM Instances?
Mit [Azure Reserved VM Instances](https://azure.microsoft.com/pricing/reserved-vm-instances) sparen Sie Geld, indem Sie eine Vorauszahlung für ein oder drei Jahre Rechenkapazität leisten, um einen Rabatt auf die von Ihnen genutzten virtuellen Computern zu erhalten. Reservierte Azure-Instanzen können Ihre Kosten für virtuelle Computer mit einer Vorauszahlung für ein oder drei Jahre erheblich reduzieren – im Vergleich mit den Preisen bei nutzungsbasierter Bezahlung um bis zu 72 Prozent. Reservierte Instanzen bieten einen Abrechnungsrabatt und wirken sich nicht auf den Laufzeitstatus Ihrer virtuellen Computer aus.

Sie können eine reservierte Instanz (RI) im [Azure-Portal](https://aka.ms/reservations) erwerben. Weitere Informationen finden Sie unter [Vorauszahlen für virtuelle Computer und Sparen von Kosten mit reservierten Instanzen](https://go.microsoft.com/fwlink/?linkid=861721).

## <a name="why-should-i-buy-a-reserved-instance"></a>Warum sollte ich eine reservierte Instanz kaufen?
Wenn Sie über virtuelle Computer verfügen, die für längere Zeiträume ausgeführt werden, erhalten Sie durch den Erwerb einer reservierten Instanz die kostengünstigste Option. Wenn Sie beispielsweise vier Instanzen eines virtuellen Computers mit Standard D2 in der Region „USA, Westen“ fortlaufend ohne reservierte Instanz ausführen, werden die Gebühren für die nutzungsbasierte Bezahlung berechnet. Wenn Sie eine reservierte Instanz für diese vier virtuellen Computer erwerben, profitieren Sie sofort von Rechnungsvorteilen dieser virtuellen Computer. Sie werden nicht mehr mit den Gebühren für die nutzungsbasierte Bezahlung in Rechnung gestellt. 

## <a name="what-charges-does-a-reserved-instance-cover"></a>Welche Gebühren deckt eine reservierte Instanz?
Eine reservierte Instanz deckt nur die Gebühren für die Infrastruktur von virtuellen Computern für Ihre virtuellen Windows- oder Linux-Computer. Eine reservierte Instanz deckt keine zusätzlichen Kosten für Software, Netzwerke oder Speicher. Für virtuelle Windows-Computer können Sie die Windows-Lizenzierungskosten mit dem [Azure-Hybridvorteil](https://azure.microsoft.com/pricing/hybrid-benefit/) decken.

## <a name="whos-eligible-to-purchase-a-reserved-instance"></a>Wer ist zum Erwerb einer reservierten Instanz berechtigt?
Azure-Kunden mit diesen Abonnementtypen können eine reservierte Instanz erwerben:
-   Enterprise Agreement-Abonnement als Angebotstyp (MS-AZR-0017P).
-   [Nutzungsbasierte Zahlung](https://azure.microsoft.com/offers/ms-azr-0003p/) als Abonnementsangebotstyp (MS-AZR-003P). Sie müssen über die Rolle „Besitzer“ für das Abonnement verfügen, um eine reservierte Instanz zu kaufen. Für den Erwerb von reservierten Instanzen in einer Enterprise-Registrierung muss der Enterprise-Administrator im EA-Portal den Kauf von reservierten Instanzen aktivieren. Diese Einstellung ist standardmäßig aktiviert.
-   CSP-Partner (Cloud Solution Provider, Cloudlösungsanbieter) können reservierte Instanzen über das Azure-Portal oder über [Partner Center](https://docs.microsoft.com/partner-center/azure-reservations) erwerben.

## <a name="how-is-a-reserved-instance-purchase-billed"></a>Wie wird der Kauf einer reservierten Instanz in Rechnung gestellt?
Der Kauf der reservierten Instanz wird mit der Zahlungsmethode in Rechnung gestellt, die mit dem Abonnement verknüpft ist. Wenn Sie über ein Enterprise-Abonnement verfügen, werden die Kosten für die reservierte Instanz von Ihrem Verpflichtungsguthaben abgezogen. Wenn Ihr Verpflichtungsguthaben die Kosten für die reservierte Instanz nicht abdeckt, wird Ihnen die Überschreitung in Rechnung gestellt.
Wenn Sie über ein Abonnement mit nutzungsbasierter Bezahlung verfügen, wird die Kreditkarte für Ihr Konto umgehend belastet. Wenn Sie Rechnungen erhalten, sind die Gebühren Ihrer nächsten Rechnung aufgeführt.

## <a name="how-is-the-purchased-reserved-instance-discount-applied"></a>Wie wird der Rabatt für reservierte Instanzen angewendet?
Der Rabatt für reservierte Instanzen gilt für die virtuellen Computer, die den Attributen entsprechen, die Sie beim Erwerb der reservierten Instanz auswählen. Zu diesen Attributen gehört der Bereich, in dem die entsprechenden virtuellen Computer ausgeführt werden. Beispiel: Wenn Sie einen Rabatt auf reservierte Instanzen für vier virtuelle Standard D2-Computer in der Region „USA, Westen“ nutzen möchten, wählen Sie das Abonnement aus, in dem die virtuellen Computer ausgeführt werden. Wenn die virtuellen Computer in verschiedenen Abonnements in Ihrer Registrierung/Ihrem Konto ausgeführt werden, wählen Sie den Bereich „Freigegeben“ aus. Der Bereich „Freigegeben“ ermöglicht die abonnementübergreifende Anwendung des Rabatts auf reservierte Instanzen. Sie können den Bereich nach dem Erwerb einer reservierten Instanz ändern. Informationen zum Ändern des Bereichs finden Sie in der Dokumentation zum Verwalten von reservierten Instanzen.

Der Rabatt auf reservierte Instanzen gilt nur für virtuelle Computer, denen die Angebotstypen „Enterprise“ oder „Nutzungsbasierte Bezahlung“ zugeordnet ist. Für virtuelle Computer, die in einem Abonnement mit anderen Angebotstypen ausgeführt werden, gilt der Rabatt auf reservierte Instanzen nicht. Bei Enterprise-Registrierungen können die Vorteile einer reservierten VM-Instanz für Dev/Test-Abonnements nicht genutzt werden.

Zum besseren Verständnis der Auswirkungen einer reservierten Instanz auf die Abrechnung virtueller Computer lesen Sie [Grundlegendes zur Anwendung des Rabatts für reservierte VM-Instanzen](https://go.microsoft.com/fwlink/?linkid=863405).

## <a name="what-happens-when-the-reserved-instance-term-expires"></a>Was geschieht nach Ende der Laufzeit der reservierten Instanz?
Am Ende der Laufzeit der reservierten Instanz läuft der Abrechnungsrabatt ab, und die Infrastruktur der virtuellen Computer wird mit den Gebühren für die nutzungsbasierte Bezahlung in Rechnung gestellt. Reservierte Instanzen werden nicht automatisch verlängert. Zur weiteren Nutzung des Abrechnungsrabatts müssen Sie eine neue reservierte Instanz kaufen. 

## <a name="sizes-and-regional-availability"></a>Größen und regionale Verfügbarkeit
Reservierte Instanzen sind für die meisten VM-Größen bis auf einige Ausnahmen verfügbar:
- Virtuelle Computer in Vorschauversionen: VM-Serien oder -größen in Vorschauversionen sind nicht für den Erwerb einer reservierten Instanz verfügbar.
- Clouds: Reservierte Instanzen sind in den Regionen „Azure US-Regierung“, „Deutschland“ und „China“ nicht zum Kauf verfügbar. 
- Nicht genügend Kontingent: Für eine reservierte Instanz, die einem einzelnen Abonnement zugeordnet ist, muss im Abonnement vCPU Kontingent für die neue RI verfügbar sein. Beispiel: Wenn für das Zielabonnement eine Kontingentgrenze von 10 vCPUs für die D-Serie gilt, können Sie nicht für 11 Standard_D1-Instanzen eine reservierte Instanz kaufen. Bei der Kontingentüberprüfung für reservierte Instanzen werden die im Abonnement bereits bereitgestellten virtuellen Computer berücksichtigt. Beispiel: Wenn im Abonnement ein Kontingent von 10 vCPUs für die D-Serien enthalten ist und zwei standard_D1-Instanzen bereitgestellt sind, können Sie eine reservierte Instanz für 10 standard_D1-Instanzen in diesem Abonnement erwerben. 
- Kapazitätseinschränkungen: In seltenen Fällen beschränkt Azure den Kauf neuer reservierter Instanzen für eine Teilmenge der VM-Größen aufgrund von geringer Kapazität in einer Region.

## <a name="next-steps"></a>Nächste Schritte
Sparen Sie durch den Erwerb einer [reservierten Azure-Instanz](https://go.microsoft.com/fwlink/?linkid=861721) ab sofort Kosten für Ihre virtuellen Computer. 

Weitere Informationen zu reservierten Instanzen finden Sie in den folgenden Artikeln:

- [Vorauszahlen für Virtual Machines mit reservierten Instanzen](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Verwalten von reservierten Azure-VM-Instanzen](billing-manage-reserved-vm-instance.md)
- [Grundlegendes zur Anwendung des Rabatts für reservierte VM-Instanzen](billing-understand-vm-reservation-charges.md)
- [Grundlagen zur Verwendung reservierter Azure-Instanzen für Ihr Abonnement mit nutzungsbasierter Zahlung](billing-understand-reserved-instance-usage.md)
- [Grundlegendes zur Nutzung reservierter Instanzen für die Enterprise-Registrierung](billing-understand-reserved-instance-usage-ea.md)
- [Nicht in reservierten Instanzen enthaltene Windows-Softwarekosten](billing-reserved-instance-windows-software-costs.md)
- [Reservierte Instanzen im Partner Center Cloud Solution Provider (CSP)-Programm](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Support kontaktieren

Bei weiteren Fragen [wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), um das Problem schnell zu lösen.
