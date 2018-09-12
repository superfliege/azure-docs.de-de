---
title: Erwerben von SUSE Linux-Plänen – Azure-Reservierungen | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Ihre SUSE-Nutzung im Voraus bezahlen und Geld gegenüber den Kosten beim nutzungsbasierten Modell sparen können.
services: virtual-machines-linux
documentationcenter: ''
author: yashesvi
manager: yashesvi
editor: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/29/2018
ms.author: yashar
ms.openlocfilehash: 9c3976a5fa98049de03f2a65b71f1fc927947142
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2018
ms.locfileid: "43310743"
---
# <a name="prepay-for-suse-software-plans-from-azure-reservations"></a>Vorauszahlen für SUSE-Softwarepläne aus Azure-Reservierungen

Bezahlen Sie Ihre SUSE-Nutzung im Voraus, und sparen Sie Geld gegenüber den Kosten beim nutzungsbasierten Modell. Die Rabatte gelten nur für SUSE-Verbrauchseinheiten und nicht für die Nutzung virtueller Computer. Um noch mehr zu sparen, können Sie Reservierungen für virtuelle Computer separat erwerben.

Sie können SUSE-Softwarepläne im Azure-Portal erwerben. Gehen Sie wie folgt vor, um einen Plan zu erwerben:

- Ihnen muss die Besitzerrolle für mindestens ein Enterprise-Abonnement oder ein Abonnement mit nutzungsbasierter Zahlung zugeordnet sein.
- In Enterprise-Abonnements müssen Reservierungskäufe im [EA-Portal](https://ea.azure.com) aktiviert werden.
- Für das Cloud Solution Provider-Programm (CSP) können die Administratoren oder Vertriebsmitarbeiter die SUSE-Pläne erwerben.

## <a name="buy-a-suse-software-plan"></a>Erwerben eines SUSE-Softwareplans

1. Navigieren Sie im Azure-Portal zu [Reservierungen](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade).
1. Wählen Sie **Hinzufügen** aus, und wählen Sie dann „SUSE Linux“ aus.
1. Füllen Sie die erforderlichen Felder aus. Alle SUSE Linux-VMs, die den Attributen des von Ihnen erworbenen Plans entsprechen, erhalten den Rabatt. Die tatsächliche Anzahl der Bereitstellungen, die den Rabatt erhalten, hängt vom ausgewählten Bereich und von der ausgewählten Menge ab.

    | Feld      | BESCHREIBUNG|
    |:------------|:--------------|
    |NAME        |Der Name dieses Erwerbs.|
    |Abonnement|Das zum Bezahlen dieses Plans verwendete Abonnement. Die Zahlungsmethode für das Abonnement wird mit Vorauszahlungen für die Reservierung belastet. Als Abonnementtyp muss „Enterprise Agreement“ (Angebotsnummer: MS-AZR-0017P) oder „Nutzungsbasierte Zahlung“ (Angebotsnummer: MS-AZR-0003P) festgelegt werden. Bei einem Enterprise-Abonnement werden die Gebühren vom Verpflichtungsguthaben der Reservierung abgezogen oder als Überschreitung belastet. Bei einem Abonnement mit nutzungsbasierter Zahlung wird die Kreditkarte mit den Gebühren belastet, oder die Gebühren werden für die Zahlung auf Rechnung in Rechnung gestellt.|
    |Bereich       |Der Bereich kann ein Abonnement oder mehrere Abonnements (freigegebener Bereich) umfassen. Optionen: <ul><li>Einzelabonnement: Der Rabatt für den Plan wird auf die SUSE Linux-Nutzung in diesem Abonnement angewendet. </li><li>Freigegeben: Der Rabatt für den Plan wird auf die SUSE Linux-Nutzung in jedem Abonnement innerhalb des Abrechnungskontexts angewendet. Für Enterprise-Kunden stellt der freigegebene Bereich die Reservierung dar und umfasst alle Abonnements (mit Ausnahme von Dev/Test-Abonnements) innerhalb der Reservierung. Für Kunden mit nutzungsbasierter Zahlung stellt der freigegebene Bereich alle Abonnements mit nutzungsbasierter Zahlung dar, die vom Kontoadministrator erstellt wurden.</li></ul>|
    |Softwareplan     |Wählen Sie den SUSE Linux-Plan aus. Wenn Sie Hilfe beim Ermitteln des zu erwerbenden Plans benötigen, lesen Sie [Grundlegendes zur Anwendung des Rabatts für Reservierungen von SUSE Linux Enterprise-Software](../../billing/billing-understand-suse-reservation-charges.md).|
    |Größe des virtuellen Computers     |Die SUSE Linux-Preise sind von der Anzahl der vCPUs auf dem VM abhängig. Wählen Sie die Option aus, die der Anzahl der vCPUs auf Ihren virtuellen SUSE Linux-Computern entspricht.|
    |Begriff        |Ein Jahr oder drei Jahre|
    |Menge    |Die Anzahl der VMs, für die Sie diesen SUSE Linux-Plan erwerben. Die Menge ist die Anzahl der ausgeführten SUSE Linux-Instanzen, auf die der Abrechnungsrabatt angewendet werden kann.|
1. Wählen Sie die Option **Kaufen**.
1. Klicken Sie auf **Diese Reservierung anzeigen**, um den Status des Einkaufs anzuzeigen.

Der Reservierungsrabatt wird automatisch auf alle ausgeführten SUSE-VMs angewendet, die der Reservierung entsprechen. Der Rabatt gilt nur für die SUSE-Verbrauchseinheit. Die VM-Computegebühren sind durch diesen Plan nicht abgedeckt.

## <a name="discount-applies-to-different-vm-sizes-with-instance-size-flexibility"></a>Der Rabatt gilt für verschiedene VM-Größen mit Flexibilität bei der Instanzgröße

Wie reservierte VM-Instanzen bieten SUSE Linux-Pläne Flexibilität bei der Instanzgröße. Dies bedeutet, dass Ihr Rabatt auch dann gilt, wenn Sie einen virtuellen Computer bereitstellen, der eine andere Größe aufweist als in dem von Ihnen erworbenen SUSE-Plan. Weitere Informationen finden Sie unter [Grundlegendes zur Anwendung des Rabatts für Reservierungen von SUSE Linux Enterprise-Software](../../billing/billing-understand-suse-reservation-charges.md).

## <a name="cancellation-and-exchanges-not-allowed"></a>Stornierungen und Wechsel sind unzulässig

Sie können einen von Ihnen erworbenen SUSE-Plan nicht stornieren oder wechseln. Überprüfen Sie Ihre Nutzung, um sicherzustellen, dass Sie den richtigen Plan erwerben. Wenn Sie Hilfe beim Ermitteln des zu erwerbenden Plans benötigen, lesen Sie [Grundlegendes zur Anwendung des Rabatts für Reservierungen von SUSE Linux Enterprise-Software](../../billing/billing-understand-suse-reservation-charges.md).

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Verwalten einer Reservierung finden Sie unter [Verwalten von reservierten Instanzen in Azure](../../billing/billing-manage-reserved-vm-instance.md).

Weitere Informationen finden Sie in den folgenden Artikeln:

- [Was sind Azure-Reservierungen?](../../billing/billing-save-compute-costs-reservations.md)
- [Verwalten von Reservierungen in Azure](../../billing/billing-manage-reserved-vm-instance.md)
- [Grundlegendes zur Anwendung des Rabatts für SUSE-Reservierungen](../../billing/billing-understand-suse-reservation-charges.md)
- [Grundlegendes zur Nutzung von Azure-Reservierungen für das Abonnement mit nutzungsbasierter Bezahlung](../../billing/billing-understand-reserved-instance-usage.md)
- [Grundlegendes zur Nutzung von Azure-Reservierungen für den Konzernbeitritt](../../billing/billing-understand-reserved-instance-usage-ea.md)

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Support kontaktieren

Bei weiteren Fragen [wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), um das Problem schnell zu lösen.