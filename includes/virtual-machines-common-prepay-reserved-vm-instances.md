---
author: yashesvi
ms.author: yashar
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 09-05-2018
ms.openlocfilehash: 4b51631e8b7d3f53edd1afdba76de3031b112254
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2018
ms.locfileid: "49805080"
---
# <a name="prepay-for-virtual-machines-with-azure-reserved-vm-instances"></a>Vorauszahlen für virtuelle Computer mit Azure Reserved VM Instances

Sie können für virtuelle Computer mit Azure Reserved VM Instances im Voraus bezahlen und Kosten sparen. Weitere Informationen finden Sie unter [Angebot – Azure Reserved VM Instances](https://azure.microsoft.com/pricing/reserved-vm-instances/).

Sie können eine reservierte VM-Instanz im [Azure-Portal](https://portal.azure.com) erwerben. So kaufen Sie ein Instanz:

- Ihnen muss die Besitzerrolle für mindestens ein Enterprise-Abonnement oder ein Abonnement mit nutzungsbasierter Zahlung zugeordnet sein.
- In Enterprise-Abonnements müssen Reservierungskäufe im [EA-Portal](https://ea.azure.com) aktiviert werden.
- Für das Cloud Solution Provider (CSP)-Programm können nur die Administrator- oder Vertriebs-Agents Reservierungen kaufen.

## <a name="determine-the-right-vm-size-before-you-buy"></a>Bestimmen der passenden Größe des virtuellen Computers vor dem Kauf

Die Unterkategorie „Verbrauchseinheit“ und die „Produkt“-Felder in den Verwendungsdaten unterscheiden nicht zwischen VM-Größen, die Storage Premium verwenden, und solchen ohne. Wenn Sie diese Felder verwenden, um die für die Reservierung zu verwendende VM-Größe zu bestimmen, kaufen Sie möglicherweise die falsche Größe und erhalten nicht den Reservierungsrabatt, den Sie erwarten. Verwenden Sie eine der folgenden Methoden, um die richtige VM-Größe zu bestimmen, wenn Sie die Reservierung kaufen:

- Sehen Sie im Feld „AdditionalInfo“ in Ihrer Nutzungsdatei oder Nutzungs-API nach, um die richtige VM-Größe zu bestimmen. Verwenden Sie nicht die Werte aus der Unterkategorie „Verbrauchseinheit“ oder den „Produkt“-Feldern. Diese Felder unterscheiden nicht zwischen S- und Nicht-S-Versionen eines virtuellen Computers.
- Sie erhalten mithilfe von PowerShell, Azure Resource Manager oder VM-Details im Azure-Portal genaue Informationen zur VM-Größe.

Reservierte VM-Instanzen sind für die meisten VM-Größen bis auf einige Ausnahmen verfügbar:

- Der Rabatt für Reservierungen gilt nicht für die folgenden VMs:
  - Klassische VMs und Clouddienste
  - Eingeschränkte vCPU-Größen
  - VM-Serien: A-Serie, Av2-Serie oder G-Serie
  - Virtuelle Computer in Vorschauversionen: jede VM-Serie oder -Größe in Vorschauversionen
- Clouds: Reservierungen sind in den Regionen „Deutschland“ und „China“ nicht zum Kauf verfügbar.
- Nicht genügend Kontingent: Für eine Reservierung, die einem einzelnen Abonnement zugeordnet ist, muss im Abonnement vCPU-Kontingent für die neue RI verfügbar sein. Beispiel: Wenn für das Zielabonnement eine Kontingentgrenze von zehn vCPUs für die D-Serie gilt, können Sie keine Reservierung für elf Standard_D1-Instanzen erwerben. Bei der Kontingentüberprüfung für Reservierungen werden die im Abonnement bereits bereitgestellten virtuellen Computer berücksichtigt. Beispiel: Wenn im Abonnement ein Kontingent von zehn vCPUs für die D-Serie enthalten ist und zwei Standard_D1-Instanzen bereitgestellt sind, können Sie eine Reservierung für zehn Standard_D1-Instanzen in diesem Abonnement erwerben. 
- Kapazitätseinschränkungen: In seltenen Fällen beschränkt Azure den Kauf neuer Reservierungen für eine Teilmenge der VM-Größen aufgrund geringer Kapazität in einer Region.

## <a name="buy-a-reserved-vm-instance"></a>Kaufen einer reservierten VM-Instanz

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie auf **Alle Dienste** > **Reservierungen**.
3. Klicken Sie auf **Hinzufügen**, um eine neue Reservierung zu erwerben.
4. Füllen Sie die erforderlichen Felder aus. Ausgeführte VM-Instanzen, die den ausgewählten Attributen entsprechen, sind für den Reservierungsrabatt berechtigt. Die tatsächliche Anzahl der VM-Instanzen, die den Rabatt erhalten, hängt vom ausgewählten Bereich und von der ausgewählten Menge ab.

    | Feld      | BESCHREIBUNG|
    |:------------|:--------------|
    |NAME        |Der Name dieser Reservierung| 
    |Abonnement|Das zum Bezahlen für die Reservierung verwendete Abonnement. Die Zahlungsmethode für das Abonnement wird mit Vorauszahlungen für die Reservierung belastet. Als Abonnementtyp muss „Enterprise Agreement“ (Angebotsnummer: MS-AZR-0017P) oder „Nutzungsbasierte Zahlung“ (Angebotsnummer: MS-AZR-0003P) festgelegt werden. Bei einem Enterprise-Abonnement werden die Gebühren vom Verpflichtungsguthaben der Reservierung abgezogen oder als Überschreitung belastet. Bei einem Abonnement mit nutzungsbasierter Zahlung wird die Kreditkarte mit den Gebühren belastet, oder die Gebühren werden für die Zahlung auf Rechnung in Rechnung gestellt.|    
    |Bereich       |Der Bereich der Reservierung kann ein Abonnement oder mehrere Abonnements (freigegebener Bereich) umfassen. Optionen: <ul><li>Einzelnes Abonnement: Der Reservierungsrabatt wird auf die virtuellen Computern in diesem Abonnement angewendet. </li><li>Freigegeben: Der Reservierungsrabatt wird auf virtuelle Computer angewendet, die in einem beliebigen Abonnement innerhalb des Abrechnungskontexts ausgeführt werden. Für Enterprise-Kunden stellt der freigegebene Bereich die Reservierung dar und umfasst alle Abonnements (mit Ausnahme von Dev/Test-Abonnements) innerhalb der Reservierung. Für Kunden mit nutzungsbasierter Zahlung stellt der freigegebene Bereich alle Abonnements mit nutzungsbasierter Zahlung dar, die vom Kontoadministrator erstellt wurden.</li></ul>|
    |Region    |Die Azure-Region, die durch die Reservierung abgedeckt wird|    
    |Größe des virtuellen Computers     |Die Größe der VM-Instanzen|
    |Optimiert für     |Mithilfe der Option für Flexibilität bei der VM-Instanzgröße wird der Rabatt für Reservierungen auf weitere VMs in derselben [VM-Größengruppe](https://aka.ms/RIVMGroups) angewendet. Die Option „Kapazitätspriorität“ priorisiert Rechenzentrumskapazität für Ihre Bereitstellungen. So erhalten Sie zusätzliche Sicherheit, dass die VM-Instanzen gestartet werden können, wenn sie benötigt werden. Die Kapazitätspriorität ist nur für den Reservierungsumfang „Einzelabonnement“ verfügbar. |
    |Begriff        |Ein Jahr oder drei Jahre|
    |Menge    |Die Anzahl von Instanzen, die innerhalb der Reservierung erworben werden. Die Menge ist die Anzahl der ausgeführten VM-Instanzen, auf die der Abrechnungsrabatt angewendet werden kann. Beispiel: Wenn Sie zehn virtuelle Computer vom Typ „10 Standard_D2“ in der Region „USA, Osten“ ausführen, geben Sie als Menge 10 an, um den Vorteil für alle ausgeführten Computer zu maximieren. |
5. Sie können die Kosten für die Reservierung anzeigen, indem Sie auf **Kosten berechnen** klicken.

    ![Screenshot vor dem Tätigen des Reservierungskaufs](./media/virtual-machines-buy-compute-reservations/virtualmachines-reservedvminstance-purchase.png)

6. Wählen Sie die Option **Kaufen**.
7. Klicken Sie auf **Diese Reservierung anzeigen**, um den Status des Einkaufs anzuzeigen.

    ![Screenshot nach dem Tätigen des Reservierungskaufs](./media/virtual-machines-buy-compute-reservations/virtualmachines-reservedvmInstance-submit.png)

## <a name="cancellations-and-exchanges"></a>Stornierungen und Umtausch

Wenn Sie Ihre Reservierung stornieren möchten, wird unter Umständen eine Gebühr für die vorzeitige Kündigung in Höhe von 12% berechnet. Rückerstattungen basieren auf dem niedrigsten Preis (entweder Ihrem Kaufpreis oder dem aktuellen Preis für die Reservierung). Rückerstattungen sind auf 50.000 US-Dollar pro Jahr beschränkt. Die Rückerstattung, die Sie erhalten, umfasst den verbleibenden anteiligen Saldo abzüglich der Gebühr für die vorzeitige Kündigung in Höhe von 12%. Navigieren Sie zum Anfordern einer Stornierung im Azure-Portal zu der Reservierung, und wählen Sie **Erstattung** aus, um eine Supportanfrage zu erstellen.

Wenn Sie Ihre Reservierung der reservierten VM-Instanzen in eine andere Region, eine andere VM-Größengruppe oder einen anderen Zeitraum ändern müssen, können Sie sie gegen eine andere Reservierung austauschen, die den gleichen oder einen höheren Wert hat. Das Startdatum des Zeitraums für die neue Reservierung wird nicht aus der umgetauschten Reservierung übernommen. Die Laufzeit von einem oder drei Jahren beginnt ab der Erstellung der neuen Reservierung. Navigieren Sie zum Anfordern eines Umtauschs im Azure-Portal zu der Reservierung, und wählen Sie **Umtausch** aus, um eine Supportanfrage zu erstellen.

## <a name="next-steps"></a>Nächste Schritte

Der Reservierungsrabatt wird automatisch auf die Anzahl der ausgeführten virtuellen Computer angewendet, die dem Reservierungsbereich und den Reservierungsattributen entsprechen. Sie können den Bereich der Reservierung über das [Azure-Portal](https://portal.azure.com), PowerShell, die CLI oder die API aktualisieren.

Informationen zum Verwalten einer Reservierung finden Sie unter [Verwalten von Azure-Reservierungen](../articles/billing/billing-manage-reserved-vm-instance.md).

Weitere Informationen zu Azure-Reservierungen finden Sie in den folgenden Artikeln:

- [Was sind Azure-Reservierungen?](../articles/billing/billing-save-compute-costs-reservations.md)
- [Verwalten von Reservierungen für Ressourcen in Azure](../articles/billing/billing-manage-reserved-vm-instance.md)
- [Grundlegendes zur Anwendung des Rabatts für Azure-Reservierungen auf virtuelle Computer](../articles/billing/billing-understand-vm-reservation-charges.md)
- [Grundlegendes zur Nutzung von Azure-Reservierungen für das Abonnement mit nutzungsbasierter Bezahlung](../articles/billing/billing-understand-reserved-instance-usage.md)
- [Grundlegendes zur Nutzung von Azure-Reservierungen für den Konzernbeitritt](../articles/billing/billing-understand-reserved-instance-usage-ea.md)
- [Nicht in Azure-Reservierungen enthaltene Windows-Softwarekosten](../articles/billing/billing-reserved-instance-windows-software-costs.md)
- [Verkaufen Microsoft Azure Reserved Instances](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Support kontaktieren

Bei weiteren Fragen [wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), um das Problem schnell zu lösen.
