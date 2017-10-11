## <a name="defining-a-backup-policy"></a>Definieren eine Sicherungsrichtlinie
Eine Sicherungsrichtlinie definiert einen Überblick darüber, wann die datenmomentaufnahmen erstellt werden und wie lange diese Momentaufnahmen beibehalten werden. Wenn Sie eine Richtlinie für die Sicherung einer virtuellen Maschine zu definieren, können Sie ein Sicherungsauftrag ausgelöst *einmal pro Tag*. Wenn Sie eine neue Richtlinie erstellen, wird er in den Tresor angewendet. Die Sicherungsrichtlinie Schnittstelle sieht wie folgt:

![Sicherungsrichtlinie](./media/backup-create-policy-for-vms/backup-policy.png)

So erstellen Sie eine Richtlinie

1. Geben Sie einen Namen für die **Richtlinienname**.
2. Momentaufnahmen der Daten können täglich oder wöchentlich Intervallen erstellt werden. Verwenden der **Sicherungshäufigkeit** Dropdown-Menü auswählen, ob datenmomentaufnahmen täglich ausgeführt werden oder alle zwei Wochen.
   
   * Wenn Sie ein tägliches Intervall auswählen, verwenden Sie das markierte Steuerelement die Tageszeit für die Momentaufnahme auswählen. Klicken Sie zum Ändern der Stunde Aufheben der elementgruppierung der Stunde, und wählen Sie die neue Stunde.
     
     ![Tägliche Sicherungsrichtlinie](./media/backup-create-policy-for-vms/backup-policy-daily.png) <br/>
   * Wenn Sie einem wöchentlichen Intervall auswählen, verwenden Sie die hervorgehobenen Steuerelemente können Sie die Wochentage und die Uhrzeit für die Momentaufnahme auswählen. Wählen Sie einen oder mehrere Tage aus, klicken Sie im Tag. Wählen Sie im Menü Stunde eine Stunde. Klicken Sie zum Ändern der Stunde, heben Sie die ausgewählten Stunde, und wählen Sie die neue Stunde.
     
     ![Wöchentliche Sicherungsrichtlinie](./media/backup-create-policy-for-vms/backup-policy-weekly.png)
3. Standardmäßig alle **Beibehaltungsdauer** Optionen ausgewählt werden. Deaktivieren Sie alle Aufbewahrung Bereichsgrenzen, die Sie nicht verwenden möchten. Geben Sie dann die Interval(s) verwenden.
   
    Monatliche und jährliche Beibehaltungsdauern ermöglichen Ihnen das Festlegen der Momentaufnahmen, die basierend auf einem Inkrement wöchentlich oder täglich.
   
   > [!NOTE]
   > Wenn Sie einen virtuellen Computer zu schützen, wird ein Sicherungsauftrag einmal täglich ausgeführt. Die Zeit, wenn die Sicherung ausgeführt wird, entspricht dem für jede Beibehaltungsdauer.
   > 
   > 
4. Klicken Sie nach dem Festlegen aller Optionen für die Richtlinie, oben auf dem Blatt auf **speichern**.
   
    Die neue Richtlinie wird sofort in den Tresor angewendet.

