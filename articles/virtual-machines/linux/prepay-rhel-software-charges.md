---
title: Erwerben von Red Hat Enterprise Linux-Plänen – Azure-Reservierungen | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Ihre Red Hat-Nutzung im Voraus bezahlen und gegenüber den Kosten bei nutzungsbasierter Bezahlung bares Geld sparen können.
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
ms.date: 01/18/2019
ms.author: yashar
ms.openlocfilehash: e90241d9da140ca33d1aae8ffc5636f8eb2fbc74
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2019
ms.locfileid: "58652834"
---
# <a name="prepay-for-red-hat-enterprise-linux-plans-from-azure-reservations"></a>Vorauszahlen für Red Hat Enterprise Linux-Pläne über Azure-Reservierungen

Bezahlen Sie Ihre Red Hat-Nutzung im Voraus, und sparen Sie bares Geld gegenüber den Kosten bei nutzungsbasierter Bezahlung. Die Rabatte gelten nur für Red Hat-Verbrauchseinheiten, nicht für die Nutzung virtueller Computer. Um noch mehr zu sparen, können Sie Reservierungen für virtuelle Computer separat erwerben.

Sie können Red Hat-Softwarepläne im Azure-Portal erwerben. Gehen Sie wie folgt vor, um einen Plan zu erwerben:

- Ihnen muss die Besitzerrolle für mindestens ein Enterprise-Abonnement oder ein Abonnement mit nutzungsbasierter Zahlung zugeordnet sein.
- Bei Enterprise-Abonnements muss im [EA-Portal](https://ea.azure.com) die Option **Reservierte Instanzen hinzufügen** aktiviert werden. Wenn diese Einstellung deaktiviert ist, müssen Sie ein EA-Administrator für das Abonnement sein.
- Für das Cloud Solution Provider-Programm (CSP) können Administratoren oder Vertriebsmitarbeiter die Red Hat-Pläne erwerben.

## <a name="buy-a-red-hat-software-plan"></a>Erwerben eines Red Hat-Softwareplans

1. Navigieren Sie im Azure-Portal zu [Reservierungen](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade).
1. Klicken Sie auf **Hinzufügen**, und wählen Sie „Red Hat Linux“ aus.
1. Füllen Sie die erforderlichen Felder aus. Der Rabatt wird auf alle Red Hat Linux-VMs angewendet, die den Attributen des von Ihnen erworbenen Plans entsprechen. Die tatsächliche Anzahl der Bereitstellungen, die den Rabatt erhalten, hängt vom ausgewählten Bereich und von der ausgewählten Menge ab.

    | Feld      | BESCHREIBUNG|
    |:------------|:--------------|
    |NAME        |Der Name dieses Erwerbs.|
    |Abonnement|Das zum Bezahlen dieses Plans verwendete Abonnement. Die Zahlungsmethode für das Abonnement wird mit Vorauszahlungen für die Reservierung belastet. Der Abonnementtyp muss „Enterprise Agreement“ (Angebotsnummern: MS-AZR-0017P oder MS-AZR-0148P) oder „Nutzungsbasierte Zahlung“ (Angebotsnummern: MS-AZR-0003P oder MS-AZR-0023P) sein. Bei einem Enterprise-Abonnement werden die Gebühren vom Verpflichtungsguthaben der Reservierung abgezogen oder als Überschreitung belastet. Bei einem Abonnement mit nutzungsbasierter Zahlung wird die Kreditkarte mit den Gebühren belastet, oder die Gebühren werden für die Zahlung auf Rechnung in Rechnung gestellt.|
    |Bereich       |Der Bereich kann ein Abonnement oder mehrere Abonnements (freigegebener Bereich) umfassen. Optionen: <ul><li>Einzelabonnement: Der Rabatt für den Plan wird auf die Red Hat Linux-Nutzung in diesem Abonnement angewendet. </li><li>Freigegeben: Der Rabatt für den Plan wird auf die Red Hat Linux-Nutzung in jedem Abonnement im Abrechnungskontext angewendet. Für Enterprise-Kunden stellt der freigegebene Bereich die Registrierung dar und umfasst alle Abonnements in der Registrierung. Für Kunden mit nutzungsbasierter Zahlung stellt der freigegebene Bereich alle Abonnements mit nutzungsbasierter Zahlung dar, die vom Kontoadministrator erstellt wurden.</li></ul>|
    |Plan     |Wählen Sie den Red Hat Linux-Plan aus. Wenn Sie Hilfe beim Ermitteln des zu erwerbenden Plans benötigen, finden Sie weitere Informationen unter „Grundlegendes zur Anwendung des Rabatts für Reservierungen von Red Hat Linux Enterprise-Software“.|
    |Größe des virtuellen Computers     |Die Red Hat Linux-Preise richten sich nach der Anzahl der vCPUs auf der VM. Wählen Sie die Option aus, die der Anzahl der vCPUs auf Ihren Red Hat Linux-VMs entspricht.|
    |Begriff        |Ein Jahr oder drei Jahre|
    |Menge    |Die Anzahl der VMs, für die Sie diesen Red Hat Linux-Plan erwerben. Die Menge ist die Anzahl der ausgeführten Red Hat Linux-Instanzen, auf die der Abrechnungsrabatt angewendet werden kann.|
1. Wählen Sie die Option **Kaufen**.
1. Klicken Sie auf **Diese Reservierung anzeigen**, um den Status des Einkaufs anzuzeigen.

Der Reservierungsrabatt wird automatisch auf alle ausgeführten Red Hat-VMs angewendet, die der Reservierung entsprechen. Der Rabatt gilt nur für die Red Hat-Verbrauchseinheit. Die VM-Computegebühren sind durch diesen Plan nicht abgedeckt.

## <a name="discount-applies-to-different-vm-sizes"></a>Rabatt gilt für verschiedene VM-Größen

Ebenso wie reservierte VM-Instanzen bieten Red Hat Linux-Pläne Flexibilität bei der Instanzgröße. Das bedeutet, dass Ihr Rabatt auch dann gilt, wenn Sie einen virtuellen Computer bereitstellen, dessen Größe sich von dem von Ihnen erworbenen Red Hat-Plan unterscheidet. Weitere Informationen finden Sie unter „Grundlegendes zur Anwendung des Rabatts für Reservierungen von Red Hat Linux Enterprise-Software“.

## <a name="cancellation-and-exchanges-not-allowed"></a>Stornierungen und Wechsel sind unzulässig

Sie können einen von Ihnen erworbenen Red Hat-Plan weder stornieren noch wechseln. Überprüfen Sie Ihre Nutzung, um sicherzustellen, dass Sie den richtigen Plan erwerben. Wenn Sie Hilfe beim Ermitteln des zu erwerbenden Plans benötigen, finden Sie weitere Informationen unter „Grundlegendes zur Anwendung des Rabatts für Reservierungen von Red Hat Linux Enterprise-Software“.

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Verwalten einer Reservierung finden Sie unter [Verwalten von reservierten Instanzen in Azure](../../billing/billing-manage-reserved-vm-instance.md).

Weitere Informationen finden Sie in den folgenden Artikeln:

- [Was sind Azure-Reservierungen?](../../billing/billing-save-compute-costs-reservations.md)
- [Verwalten von Reservierungen für Ressourcen in Azure](../../billing/billing-manage-reserved-vm-instance.md)
- Grundlegendes zur Anwendung des Rabatts für Red Hat-Reservierungen
- [Grundlegendes zur Nutzung von Azure-Reservierungen für das Abonnement mit nutzungsbasierter Bezahlung](../../billing/billing-understand-reserved-instance-usage.md)
- [Grundlegendes zur Nutzung von Azure-Reservierungen für den Konzernbeitritt](../../billing/billing-understand-reserved-instance-usage-ea.md)

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Support kontaktieren

Bei weiteren Fragen [wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), um das Problem schnell zu lösen.