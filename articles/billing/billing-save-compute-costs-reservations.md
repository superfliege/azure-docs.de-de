---
title: Was sind Azure-Reservierungen? | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie mehr über Azure-Reservierungen und Preise, mit denen Sie Einsparungen für virtuelle Computer, SQL-Datenbanken und andere Ressourcenkosten erzielen.
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
ms.date: 08/08/2018
ms.author: yashar
ms.openlocfilehash: 82b23f46acc94fefccc871583657200b90a33f05
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2018
ms.locfileid: "43303561"
---
# <a name="what-are-azure-reservations"></a>Was sind Azure-Reservierungen?

Mit Azure-Reservierungen können Sie Geld sparen, indem Sie im Voraus VM-, SQL-Datenbank-Kapazitäten oder andere Azure-Ressourcen für ein Jahr oder drei Jahre bezahlen. Durch die Vorabzahlung können Sie einen Rabatt für die Ressourcen, die Sie nutzen, in Anspruch nehmen. Reservierungen können Ihre Kosten für virtuelle Computer, SQL-Datenbank-Computing oder andere Ressourcen deutlich um bis zu 72 % gegenüber der nutzungsbasierten Bezahlung reduzieren. Reservierungen bieten einen Abrechnungsrabatt und wirken sich nicht auf den Laufzeitstatus Ihrer Ressourcen aus.

Sie können eine Reservierung im [Azure-Portal](https://aka.ms/reservations) erwerben. Weitere Informationen finden Sie in den folgenden Themen:

- [Vorauszahlen für virtuelle Computer mit Azure Reserved VM Instances](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Vorauszahlen von SQL-Datenbank-Computeressourcen mit reservierter Azure SQL-Datenbank-Kapazität](../sql-database/sql-database-reserved-capacity.md)

## <a name="why-should-i-buy-a-reservation"></a>Warum sollte ich eine Reservierung erwerben?

Wenn Sie über virtuelle Computer oder SQL-Datenbank-Instanzen verfügen, die für längere Zeiträume ausgeführt werden, erhalten Sie durch den Erwerb einer reservierten Instanz die kostengünstigste Option. Wenn Sie beispielsweise kontinuierlich vier Instanzen von einem Dienst ohne Reservierung ausführen, fallen die Gebühren für die nutzungsbasierte Bezahlung an. Wenn Sie eine Reservierung für diese Ressourcen erwerben, erhalten Sie sofort den Rabatt auf Reservierungen. Die Ressourcen werden nicht mehr mit den Gebühren für die nutzungsbasierte Bezahlung in Rechnung gestellt.

## <a name="what-charges-does-a-reservation-cover"></a>Welche Gebühren werden von einer Reservierung abgedeckt?

- Reservierte VM-Instanz: Eine Reservierung deckt nur die Computekosten virtueller Computer ab. Eine Reservierung deckt keine zusätzlichen Kosten für Software, Netzwerke oder Speicher ab.
- Reservierte virtuelle Kerne für SQL-Datenbank: In einer Reservierung sind nur die Computekosten enthalten. Die Lizenz wird separat abgerechnet.

Für virtuelle Windows-Computer und SQL-Datenbank können Sie die Lizenzierungskosten mit dem [Azure-Hybridvorteil](https://azure.microsoft.com/pricing/hybrid-benefit/) decken.

## <a name="whos-eligible-to-purchase-a-reservation"></a>Wer ist zum Erwerb einer Reservierung berechtigt?

Azure-Kunden mit den folgenden Abonnementtypen können eine Reservierung erwerben:

- Enterprise Agreement-Abonnement als Angebotstyp (MS-AZR-0017P).
- [Nutzungsbasierte Zahlung](https://azure.microsoft.com/offers/ms-azr-0003p/) als Abonnementsangebotstyp (MS-AZR-003P). Sie müssen die Rolle „Besitzer“ für das Abonnement besitzen, um eine Reservierung erwerben zu können.
- CSP-Partner (Cloud Solution Provider, Cloudlösungsanbieter) können Azure-Reservierungen über das Azure-Portal oder über das [Partner Center](https://docs.microsoft.com/partner-center/azure-reservations) erwerben.

Der Rabatt auf Reservierungen gilt nur für Ressourcen, denen einer der Abonnementtypen „Enterprise“, „Nutzungsbasierte Bezahlung“ oder „CSP“ zugeordnet ist.

## <a name="how-is-a-reservation-billed"></a>Wie wird eine Reservierung abgerechnet?

Die Reservierung wird mit der Zahlungsmethode in Rechnung gestellt, die mit dem Abonnement verknüpft ist. Wenn Sie über ein Enterprise-Abonnement verfügen, werden die Reservierungskosten von Ihrem Verpflichtungsguthaben abgezogen. Wenn Ihr Verpflichtungsguthaben die Kosten für die Reservierung nicht abdeckt, wird Ihnen die Überschreitung in Rechnung gestellt. Wenn Sie über ein Abonnement mit nutzungsbasierter Bezahlung verfügen, wird die Kreditkarte für Ihr Konto umgehend belastet. Wenn Sie Rechnungen erhalten, sind die Gebühren Ihrer nächsten Rechnung aufgeführt.

## <a name="how-is-the-reservation-discount-applied"></a>Wie wird der Rabatt auf Reservierungen angewendet?

Der Reservierungsrabatt gilt für die Ressourcennutzung, die den Attribute entspricht, die Sie beim Erwerb der Reservierung auswählen. Zu diesen Attributen gehört der Bereich, in dem die entsprechenden VMs, SQL-Datenbank-Instanzen oder anderen Ressourcen ausgeführt werden. Beispiel: Wenn Sie einen Rabatt auf eine Reservierung für vier virtuelle Computer des Typs „Standard D2“ in der Region „USA, Westen“ nutzen möchten, wählen Sie das Abonnement aus, in dem die VMs ausgeführt werden. Wenn die virtuellen Computer in verschiedenen Abonnements in Ihrer Registrierung/Ihrem Konto ausgeführt werden, wählen Sie den Bereich „Freigegeben“ aus. Der Bereich „Freigegeben“ ermöglicht die abonnementübergreifende Anwendung des Reservierungsrabatts. Sie können den Bereich nach dem Erwerb einer Reservierung ändern. Weitere Informationen finden Sie unter [Verwalten von Azure-Reservierungen](billing-manage-reserved-vm-instance.md).

Der Rabatt auf Reservierungen gilt nur für Ressourcen, denen einer der Abonnementtypen „Enterprise“, „Nutzungsbasierte Bezahlung“ oder „CSP“ zugeordnet ist. Für Ressourcen, die in einem Abonnement mit anderen Angebotstypen ausgeführt werden, gilt der Reservierungsrabatt nicht. Bei Enterprise-Registrierungen können die Vorteile einer Reservierung nicht für Enterprise Dev/Test-Abonnements genutzt werden.

Um besser zu verstehen, wie sich Reservierungen auf Ihre Abrechnung auswirken, lesen Sie die folgenden Themen:

-  [Grundlegendes zum Rabatt für reservierte Azure-VM-Instanzen](billing-understand-vm-reservation-charges.md)
- [Grundlegendes zum Rabatt für Azure-Reservierungen](billing-understand-vm-reservation-charges.md)
- Grundlegendes zum Rabatt für SUSE und zur Verwendung von Azure-Reservierungen unter SUSE

## <a name="what-happens-when-the-reservation-term-expires"></a>Was geschieht nach Ende der Laufzeit der Reservierung?

Am Ende der Laufzeit der Reservierung läuft der Abrechnungsrabatt ab, und die Infrastruktur der virtuellen Computer, SQL-Datenbank-Instanzen oder anderen Ressourcen wird mit den Gebühren für die nutzungsbasierte Bezahlung in Rechnung gestellt. Azure-Reservierungen verlängern sich nicht automatisch. Zur weiteren Nutzung des Abrechnungsrabatts müssen Sie eine neue Reservierung für Dienste und Software erwerben, die für Reservierungen berechtigt sind.

## <a name="next-steps"></a>Nächste Schritte

Sparen Sie durch den Erwerb einer [reservierten VM-Instanz](../virtual-machines/windows/prepay-reserved-vm-instances.md) oder einer [reservierten SQL-Datenbank-Kapazität](../sql-database/sql-database-reserved-capacity.md) ab sofort Kosten für Ihre virtuellen Computer.

Weitere Informationen zu Azure-Reservierungen finden Sie in den folgenden Artikeln:

- [Verwalten von Azure-Reservierungen](billing-manage-reserved-vm-instance.md)
- [Grundlegendes zur Nutzung von Azure-Reservierungen für das Abonnement mit nutzungsbasierter Bezahlung](billing-understand-reserved-instance-usage.md)
- [Grundlegendes zur Nutzung von Azure-Reservierungen für den Konzernbeitritt](billing-understand-reserved-instance-usage-ea.md)
- [Nicht in Azure-Reservierungen enthaltene Windows-Softwarekosten](billing-reserved-instance-windows-software-costs.md)
- [Verkaufen Microsoft Azure Reserved Instances](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Support kontaktieren

Bei weiteren Fragen [wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), um das Problem schnell zu lösen.
