# <a name="prepay-for-virtual-machines-with-reserved-vm-instances"></a>Vorauszahlen für virtuelle Computer mit reservierten VM-Instanzen

Sie können für virtuelle Computer vorauszahlen und Kosten mit reservierten VM-Instanzen sparen. Weitere Informationen finden Sie unter [Reservierte Azure-VM-Instanzen](https://azure.microsoft.com/pricing/reserved-vm-instances/).

Sie können reservierte VM-Instanzen im [Azure-Portal](https://portal.azure.com) erwerben. So erwerben Sie eine reservierte VM-Instanz:
-   Ihnen muss die Besitzerrolle für mindestens ein Enterprise-Abonnement oder ein Abonnement mit nutzungsbasierter Zahlung zugeordnet sein.
-   In Enterprise-Abonnements müssen Reservierungskäufe im [EA-Portal](https://ea.azure.com) aktiviert werden.

## <a name="buy-a-reserved-virtual-machine-instance"></a>Erwerben einer reservierten VM-Instanz
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie auf **Weitere Dienste** > **Reservierungen**.
3. Klicken Sie auf **Hinzufügen**, um eine neue Reservierung zu erwerben.
4. Füllen Sie die erforderlichen Felder aus. Ausgeführte VM-Instanzen, die den ausgewählten Attributen entsprechen, sind für den Reservierungsrabatt berechtigt. Die tatsächliche Anzahl der VM-Instanzen, die den Rabatt erhalten, hängt vom ausgewählten Bereich und von der ausgewählten Menge ab.

    | Feld      | BESCHREIBUNG|
    |:------------|:--------------|
    |NAME        |Der Name dieser Reservierung| 
    |Abonnement|Das zum Bezahlen für die Reservierung verwendete Abonnement. Die Zahlungsmethode für das Abonnement wird mit Vorauszahlungen für die Reservierung belastet. Als Abonnementtyp muss „Enterprise Agreement“ (Angebotsnummer: MS-AZR-0017P) oder „Nutzungsbasierte Zahlung“ (Angebotsnummer: MS-AZR-0003P) festgelegt werden. Bei einem Enterprise-Abonnement werden die Gebühren vom Verpflichtungsguthaben der Reservierung abgezogen oder als Überschreitung belastet. Bei einem Abonnement mit nutzungsbasierter Zahlung wird die Kreditkarte mit den Gebühren belastet, oder die Gebühren werden für die Zahlung auf Rechnung in Rechnung gestellt.|    
    |Umfang       |Der Bereich der Reservierung kann ein Abonnement oder mehrere Abonnements (freigegebener Bereich) umfassen. Optionen: <ul><li>Einzelnes Abonnement: Der Reservierungsrabatt wird auf die virtuellen Computern in diesem Abonnement angewendet. </li><li>Freigegeben: Der Reservierungsrabatt wird auf virtuelle Computer angewendet, die in einem beliebigen Abonnement innerhalb des Abrechnungskontexts ausgeführt werden. Für Enterprise-Kunden stellt der freigegebene Bereich die Reservierung dar und umfasst alle Abonnements (mit Ausnahme von Dev/Test-Abonnements) innerhalb der Reservierung. Für Kunden mit nutzungsbasierter Zahlung stellt der freigegebene Bereich alle Abonnements mit nutzungsbasierter Zahlung dar, die vom Kontoadministrator erstellt wurden.</li></ul>|
    |Speicherort    |Die Azure-Region, die durch die Reservierung abgedeckt wird|    
    |Größe des virtuellen Computers     |Die Größe der VM-Instanzen|
    |Begriff        |Ein Jahr oder drei Jahre|
    |Menge    |Die Anzahl von Instanzen, die innerhalb der Reservierung erworben werden. Die Menge ist die Anzahl der ausgeführten VM-Instanzen, auf die der Abrechnungsrabatt angewendet werden kann. Beispiel: Wenn Sie zehn virtuelle Computer vom Typ „10 Standard_D2“ in der Region „USA, Osten“ ausführen, würden Sie als Menge zehn angeben, um den Vorteil für alle ausgeführten Computer zu maximieren. |
5. Sie können die Kosten für die Reservierung anzeigen, indem Sie auf **Kosten berechnen** klicken.

    ![Screenshot vor dem Tätigen des Reservierungskaufs](./media/virtual-machines-buy-compute-reservations/virtualmachines-reservedvminstance-purchase.png)

6. Wählen Sie die Option **Kaufen**.
7. Klicken Sie auf **Diese Reservierung anzeigen**, um den Status des Einkaufs anzuzeigen.

    ![Screenshot vor dem Tätigen des Reservierungskaufs](./media/virtual-machines-buy-compute-reservations/virtualmachines-reservedvmInstance-submit.png)

## <a name="next-steps-after-buying-a-reservation"></a>Weitere Schritte nach dem Kauf einer Reservierung
Der Reservierungsrabatt wird automatisch auf die Anzahl der ausgeführten virtuellen Computer angewendet, die dem Reservierungsbereich und den Reservierungsattributen entsprechen. Sie können den Bereich der Reservierung über das [Azure-Portal](https://portal.azure.com), PowerShell, die CLI oder die API aktualisieren. 

Informationen zum Verwalten einer Reservierung finden Sie unter [Reservierte Azure-VM-Instanzen (RIs)s](../articles/billing/billing-manage-reserved-vm-instance.md).

