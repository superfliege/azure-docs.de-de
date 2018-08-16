---
author: yashesvi
ms.author: yashar
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 08-07-2018
ms.openlocfilehash: 19a153a5cdc9d5f878494984313baebd12dbcbb5
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/08/2018
ms.locfileid: "39631162"
---
# <a name="prepay-for-virtual-machines-with-azure-reserved-vm-instances"></a>Vorauszahlen für virtuelle Computer mit Azure Reserved VM Instances

Sie können für virtuelle Computer mit Azure Reserved VM Instances im Voraus bezahlen und Kosten sparen. Weitere Informationen finden Sie unter [Angebot – Azure Reserved VM Instances](https://azure.microsoft.com/pricing/reserved-vm-instances/).

Sie können eine reservierte VM-Instanz im [Azure-Portal](https://portal.azure.com) erwerben. So kaufen Sie ein Instanz:

- Ihnen muss die Besitzerrolle für mindestens ein Enterprise-Abonnement oder ein Abonnement mit nutzungsbasierter Zahlung zugeordnet sein.
- In Enterprise-Abonnements müssen Reservierungskäufe im [EA-Portal](https://ea.azure.com) aktiviert werden.
- Für das Cloud Solution Provider (CSP)-Programm können nur die Administrator- oder Vertriebs-Agents die Reservierungen erwerben.

## <a name="determine-the-right-vm-size-before-purchase"></a>Bestimmen der passenden Größe des virtuellen Computers

Die Felder für die Unterkategorie der Verbrauchseinheit und für das Produkt in den Nutzungsdaten unterscheiden nicht zwischen VM-Größen, die Storage Premium nutzen, und VM-Größen, die nicht Storage Premium nutzen. Wenn Sie diese Felder zum Bestimmen der VM-Größe für den Reservierungserwerb verwenden, kann dies einen falschen Reservierungserwerb zur Folge haben und dazu führen, dass keine Reservierungsrabatte gewährt werden. Verwenden Sie eine der folgenden Methoden, um die richtige VM-Größe für den Reservierungserwerb zu bestimmen.

- Verweisen Sie auf das Feld „AdditionalInfo“ in Ihrer Nutzungsdatei oder Nutzungs-API, um die richtige VM-Größe bei einem Reservierungskauf zu bestimmen. Verwenden Sie nicht die Werte aus den Feldern „Unterkategorie für Messung“ oder „Produkt“, weil diese Felder nicht zwischen einer S- und Nicht-S-Version eines virtuellen Computers unterscheiden.
- Sie können auch mithilfe von PowerShell, Azure Resource Manager oder VM-Details im Azure-Portal umfassende Informationen zur VM-Größe erhalten.

Reservierte VM-Instanzen sind für die meisten VM-Größen bis auf einige Ausnahmen verfügbar:

- Virtuelle Computer in Vorschauversionen: VM-Serien oder -Größen in Vorschauversionen sind nicht für den Reservierungserwerb verfügbar.
- Clouds: Reservierungen sind in den Regionen „Azure US-Regierung“, „Deutschland“ und „China“ nicht zum Kauf verfügbar.
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
    |Standort    |Die Azure-Region, die durch die Reservierung abgedeckt wird|    
    |Größe des virtuellen Computers     |Die Größe der VM-Instanzen|
    |Optimiert für     |Mithilfe der Option für Flexibilität bei der VM-Instanzgröße wird der Rabatt für Reservierungen auf weitere VMs in derselben [VM-Größengruppe](https://aka.ms/RIVMGroups) angewendet. Über die Option „Kapazitätspriorität“ wird Datencenterkapazität für Ihre Bereitstellungen reserviert. So erhalten Sie zusätzliche Sicherheit, dass die VM-Instanzen gestartet werden können, wenn sie benötigt werden. Die Kapazitätspriorität ist nur für den Reservierungsumfang „Einzelabonnement“ verfügbar. |
    |Begriff        |Ein Jahr oder drei Jahre|
    |Menge    |Die Anzahl von Instanzen, die innerhalb der Reservierung erworben werden. Die Menge ist die Anzahl der ausgeführten VM-Instanzen, auf die der Abrechnungsrabatt angewendet werden kann. Beispiel: Wenn Sie zehn virtuelle Computer vom Typ „10 Standard_D2“ in der Region „USA, Osten“ ausführen, geben Sie als Menge 10 an, um den Vorteil für alle ausgeführten Computer zu maximieren. |
5. Sie können die Kosten für die Reservierung anzeigen, indem Sie auf **Kosten berechnen** klicken.

    ![Screenshot vor dem Tätigen des Reservierungskaufs](./media/virtual-machines-buy-compute-reservations/virtualmachines-reservedvminstance-purchase.png)

6. Wählen Sie die Option **Kaufen**.
7. Klicken Sie auf **Diese Reservierung anzeigen**, um den Status des Einkaufs anzuzeigen.

    ![Screenshot nach dem Tätigen des Reservierungskaufs](./media/virtual-machines-buy-compute-reservations/virtualmachines-reservedvmInstance-submit.png)

## <a name="next-steps"></a>Nächste Schritte

Der Reservierungsrabatt wird automatisch auf die Anzahl der ausgeführten virtuellen Computer angewendet, die dem Reservierungsbereich und den Reservierungsattributen entsprechen. Sie können den Bereich der Reservierung über das [Azure-Portal](https://portal.azure.com), PowerShell, die CLI oder die API aktualisieren.

Informationen zum Verwalten einer Reservierung finden Sie unter [Verwalten von reservierten Instanzen in Azure](../articles/billing/billing-manage-reserved-vm-instance.md).

Weitere Informationen zu Azure-Reservierungen finden Sie in den folgenden Artikeln:

- [Was ist Azure Reserved VM Instances?](../articles/billing/billing-save-compute-costs-reservations.md)
- [Verwalten von reservierten Instanzen in Azure](../articles/billing/billing-manage-reserved-vm-instance.md)
- [Grundlegendes zur Anwendung des Rabatts für reservierte Instanzen](../articles/billing/billing-understand-vm-reservation-charges.md)
- [Grundlagen zur Verwendung reservierter Azure-Instanzen für Ihr Abonnement mit nutzungsbasierter Zahlung](../articles/billing/billing-understand-reserved-instance-usage.md)
- [Grundlegendes zur Nutzung reservierter Azure-Instanzen für die Enterprise-Registrierung](../articles/billing/billing-understand-reserved-instance-usage-ea.md)
- [Nicht in reservierten Azure-Instanzen enthaltene Windows-Softwarekosten](../articles/billing/billing-reserved-instance-windows-software-costs.md)
- [Verkaufen von Microsoft Azure Reserved VM Instances](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Support kontaktieren

Bei weiteren Fragen [wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), um das Problem schnell zu lösen.
