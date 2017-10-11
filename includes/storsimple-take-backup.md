<!--author=alkohli last changed: 9/17/15-->

### <a name="to-take-a-backup"></a>So erstellen Sie eine Sicherung
1. Auf dem Gerät **Schnellstart** auf **Hinzufügen einer Sicherungsrichtlinie**. Dadurch wird das Hinzufügen einer Sicherungsrichtlinie-Assistent gestartet. 
2. Auf der **Sicherungsrichtlinie definieren** Seite:
   
   1. Geben Sie einen Namen, der zwischen 3 und 150 Zeichen für Ihre Sicherungsrichtlinie enthält.
   2. Wählen Sie die Volumes gesichert werden soll. Wenn Sie mehr als ein Volume auswählen, werden diese Volumes werden gruppiert werden, um eine ausfallsichere Sicherung zu erstellen.
   3. Klicken Sie auf das Symbol "Pfeil" ![Pfeilsymbol](./media/storsimple-take-backup/HCS_ArrowIcon-include.png). 
      
      ![Backup-Richtlinie hinzufügen](./media/storsimple-take-backup/HCS_AddBackupPolicyWizard1M-include.png)
3. Auf der **definieren Sie einen Zeitplan** Seite:
   
   1. Wählen Sie aus der Dropdown-Liste den Typ der Sicherung. Wählen Sie für schnellere Wiederherstellungen **lokale Momentaufnahme**. Wählen Sie für datenresilienz **Cloudmomentaufnahme**.
   2. Geben Sie die sicherungshäufigkeit in Minuten, Stunden, Tage oder Wochen an.
   3. Wählen Sie eine Aufbewahrungsdauer. Die Aufbewahrungsdauer hängt von den sicherungshäufigkeit ab. Beispielsweise kann für eine tägliche Richtlinie kann die Aufbewahrung in Wochen, angegeben werden, während die Aufbewahrungsdauer für eine monatliche Richtlinie Monate beträgt.
   4. Wählen Sie die Startzeit und das Datum für die Sicherungsrichtlinie.
   5. Wählen Sie die **aktivieren** Kontrollkästchen, um die Sicherungsrichtlinie zu aktivieren. 
   6. Klicken Sie auf das Häkchensymbol ![Kontrollkästchen-Symbol](./media/storsimple-take-backup/HCS_CheckIcon-include.png) um die Richtlinie zu speichern.
      
      ![Backup-Richtlinie hinzufügen](./media/storsimple-take-backup/HCS_AddBackupPolicyWizard2M-include.png)
      
      Sie haben jetzt eine Sicherungsrichtlinie, die geplante Sicherungen für Ihre Volumedaten erstellt.

Sie haben die Gerätekonfiguration abgeschlossen. 

![Video zur Verfügung](./media/storsimple-take-backup/Video_icon.png) **Video verfügbar**

Um ein Video zu beobachten, die veranschaulicht, wie Sie einen StorSimple Sicherung erstellen, klicken Sie auf [hier](https://azure.microsoft.com/documentation/videos/take-a-storsimple-backup/).

