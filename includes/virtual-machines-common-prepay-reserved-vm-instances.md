---
ms.topic: include
ms.openlocfilehash: 99eaa667e4c6a9d63b4cc43ada8c6e36f7365610
ms.sourcegitcommit: 39f4911b5933f7062dcf5d57af94eab8a0740b2b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/16/2018
ms.locfileid: "35683048"
---
# <a name="prepay-for-virtual-machines-with-azure-reserved-vm-instances"></a>Vorauszahlen für virtuelle Computer mit Azure Reserved VM Instances

Sie können für virtuelle Computer mit Azure Reserved VM Instances im Voraus bezahlen und Kosten sparen. Weitere Informationen finden Sie unter [Angebot – Azure Reserved Instances](https://azure.microsoft.com/pricing/reserved-vm-instances/).

Sie können Azure Reserved Instances im [Azure-Portal](https://portal.azure.com) kaufen. So kaufen Sie eine Reserved Instance
-   Ihnen muss die Besitzerrolle für mindestens ein Enterprise-Abonnement oder ein Abonnement mit nutzungsbasierter Zahlung zugeordnet sein.
-   In Enterprise-Abonnements muss der Kauf von Reserved Instances im [EA-Portal](https://ea.azure.com) aktiviert werden.
-   Für das Cloud Solution Provider-Programm (CSP) können nur die Administrator- oder Vertriebs-Agents Reserved Instances kaufen.

[!IMPORTANT]
Ermitteln Sie die passende VM-Größe beim Reservierungskauf mithilfe einer der folgenden Methoden.

## <a name="determine-the-right-vm-size-before-purchase"></a>Bestimmen der passenden Größe des virtuellen Computers
1. Verweisen Sie auf das Feld „AdditionalInfo“ in Ihrer Nutzungsdatei oder Nutzungs-API, um die richtige VM-Größe bei einem Reservierungskauf zu bestimmen. Verwenden Sie nicht die Werte aus den Feldern „Unterkategorie für Messung“ oder „Produkt“, weil diese Felder nicht zwischen einer S- und Nicht-S-Version eines virtuellen Computers unterscheiden.
2. Sie können auch mithilfe von PowerShell, Azure Resource Manager oder VM-Details im Azure-Portal umfassende Informationen zur VM-Größe erhalten.

## <a name="buy-a-reserved-virtual-machine-instance"></a>Erwerben einer reservierten VM-Instanz
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie auf **Alle Dienste** > **Reservierungen**.
3. Klicken Sie auf **Hinzufügen**, um eine neue Reserved Instance zu kaufen.
4. Füllen Sie die erforderlichen Felder aus. Ausgeführte VM-Instanzen, die den ausgewählten Attributen entsprechen, sind für den Reserved Instance-Rabatt berechtigt. Die tatsächliche Anzahl der VM-Instanzen, die den Rabatt erhalten, hängt vom ausgewählten Bereich und von der ausgewählten Menge ab.

    | Feld      | BESCHREIBUNG|
    |:------------|:--------------|
    |NAME        |Der Name dieser Reserved Instance| 
    |Abonnement|Das zum Bezahlen für die Reserved Instance verwendete Abonnement. Die Zahlungsmethode für das Abonnement wird mit Vorauszahlungen für die Reserved Instance belastet. Als Abonnementtyp muss „Enterprise Agreement“ (Angebotsnummer: MS-AZR-0017P) oder „Nutzungsbasierte Zahlung“ (Angebotsnummer: MS-AZR-0003P) festgelegt werden. Bei einem Enterprise-Abonnement werden die Gebühren vom Verpflichtungsguthaben der Reservierung abgezogen oder als Überschreitung belastet. Bei einem Abonnement mit nutzungsbasierter Zahlung wird die Kreditkarte mit den Gebühren belastet, oder die Gebühren werden für die Zahlung auf Rechnung in Rechnung gestellt.|    
    |Umfang       |Der Bereich der Reserved Instance kann ein oder mehrere Abonnements (freigegebener Bereich) umfassen. Optionen: <ul><li>Einzelnes Abonnement: Der Reserved Instance-Rabatt wird auf die virtuellen Computer in diesem Abonnement angewendet. </li><li>Freigegeben: Der Reserved Instance-Rabatt wird auf virtuelle Computer angewendet, die in einem beliebigen Abonnement innerhalb des Abrechnungskontexts ausgeführt werden. Für Enterprise-Kunden stellt der freigegebene Bereich die Reservierung dar und umfasst alle Abonnements (mit Ausnahme von Dev/Test-Abonnements) innerhalb der Reservierung. Für Kunden mit nutzungsbasierter Zahlung stellt der freigegebene Bereich alle Abonnements mit nutzungsbasierter Zahlung dar, die vom Kontoadministrator erstellt wurden.</li></ul>|
    |Speicherort    |Die Azure-Region, für die die Reserved Instance gilt|    
    |Größe des virtuellen Computers     |Die Größe der VM-Instanzen|
    |Begriff        |Ein Jahr oder drei Jahre|
    |Menge    |Die Anzahl von Instanzen, die innerhalb der Reserved Instance gekauft werden. Die Menge ist die Anzahl der ausgeführten VM-Instanzen, auf die der Abrechnungsrabatt angewendet werden kann. Beispiel: Wenn Sie zehn virtuelle Computer vom Typ „10 Standard_D2“ in der Region „USA, Osten“ ausführen, geben Sie als Menge 10 an, um den Vorteil für alle ausgeführten Computer zu maximieren. |
5. Sie können die Kosten für die Reserved Instance anzeigen, indem Sie auf **Kosten berechnen** klicken.

    ![Screenshot vor dem Übermitteln des Kaufs der Reserved Instance](./media/virtual-machines-buy-compute-reservations/virtualmachines-reservedvminstance-purchase.png)

6. Wählen Sie die Option **Kaufen**.
7. Klicken Sie auf **Diese Reservierung anzeigen**, um den Status des Einkaufs anzuzeigen.

    ![Screenshot nach dem Übermitteln des Kaufs der Reserved Instance](./media/virtual-machines-buy-compute-reservations/virtualmachines-reservedvmInstance-submit.png)

## <a name="next-steps"></a>Nächste Schritte 
Der Reserved Instance-Rabatt wird automatisch auf die Anzahl der ausgeführten virtuellen Computer angewendet, die dem Bereich und den Attributen der Reserved Instance entspricht. Sie können den Bereich der Reserved Instance über das [Azure-Portal](https://portal.azure.com), PowerShell, die Befehlszeilenschnittstelle oder die API aktualisieren. 

Informationen zum Verwalten einer Reserved Instance finden Sie unter [Verwalten von Azure Reserved Instances](../articles/billing/billing-manage-reserved-vm-instance.md).

Weitere Informationen zu Azure Reserved Instances finden Sie in den folgenden Artikeln:

- [Einsparen von Kosten für virtuelle Computer mit Reserved Instances](../articles/billing/billing-save-compute-costs-reservations.md)
- [Verwalten von Azure Reserved Instances](../articles/billing/billing-manage-reserved-vm-instance.md)
- [Grundlegendes zur Anwendung des Rabatts für reservierte VM-Instanzen](../articles/billing/billing-understand-vm-reservation-charges.md)
- [Grundlagen zur Verwendung reservierter Azure-Instanzen für Ihr Abonnement mit nutzungsbasierter Zahlung](../articles/billing/billing-understand-reserved-instance-usage.md)
- [Grundlegendes zur Nutzung reservierter Instanzen für die Enterprise-Registrierung](../articles/billing/billing-understand-reserved-instance-usage-ea.md)
- [Nicht in reservierten Instanzen enthaltene Windows-Softwarekosten](../articles/billing/billing-reserved-instance-windows-software-costs.md)
- [Reservierte Instanzen im Partner Center Cloud Solution Provider (CSP)-Programm](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Support kontaktieren

Bei weiteren Fragen [wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), um das Problem schnell zu lösen.