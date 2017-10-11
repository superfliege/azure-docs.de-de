<!--author=alkohli last changed: 01/12/17-->

### <a name="to-take-a-backup"></a>So erstellen Sie eine Sicherung

1. Wechseln Sie zu Ihrer StorSimple-Geräte-Manager-Dienst. Aus der tabellarischen Auflistung von Geräten, auswählen, und klicken Sie auf Ihrem Gerät, und klicken Sie dann auf **alle Einstellungen**. In der **Einstellungen** wechseln Sie zum Blatt **Einstellungen > Verwalten > Sicherungsrichtlinie**.

    ![Backup-Richtlinie hinzufügen](./media/storsimple-8000-take-backup/step8takebu1.png)

2. In der **Sicherungsrichtlinie** Blatt, klicken Sie auf **+ Richtlinie hinzufügen**.

    ![Backup-Richtlinie hinzufügen](./media/storsimple-8000-take-backup/step8takebu2.png)

3. In der **Erstellen einer Sicherungsrichtlinie** Blatt geben einen Namen, die zwischen 3 und 150 Zeichen für Ihre Sicherungsrichtlinie enthält.

4. Wählen Sie die Volumes gesichert werden soll. Wenn Sie mehr als ein Volume auswählen, werden diese Volumes werden gruppiert, um eine ausfallsichere Sicherung zu erstellen.

    ![Backup-Richtlinie hinzufügen](./media/storsimple-8000-take-backup/step8takebu4.png)

5. Auf **erste Zeitplan hinzufügen** Blatt:

    1. Wählen Sie den Typ der Sicherung. Wählen Sie für schnellere Wiederherstellungen **lokale** Momentaufnahme. Wählen Sie für datenresilienz **Cloud** Momentaufnahme.
    2. Geben Sie die sicherungshäufigkeit in Minuten, Stunden, Tage oder Wochen an.
    3. Wählen Sie eine Aufbewahrungsdauer. Die Aufbewahrungsdauer hängt von den sicherungshäufigkeit ab. Beispielsweise kann für eine tägliche Richtlinie kann die Aufbewahrung in Wochen, angegeben werden, während die Aufbewahrungsdauer für eine monatliche Richtlinie Monate beträgt.
    4. Wählen Sie die Startzeit und das Datum für die Sicherungsrichtlinie.
    5. Klicken Sie auf **OK** die Sicherungsrichtlinie zu erstellen.

        ![Backup-Richtlinie hinzufügen](./media/storsimple-8000-take-backup/step8takebu5.png) 

6. Klicken Sie auf **erstellen** die Sicherungsrichtlinie-Erstellung starten. Sie werden benachrichtigt, wenn die Sicherungsrichtlinie erfolgreich erstellt wurde. Die Liste der Sicherungsrichtlinien wird ebenfalls aktualisiert.
      
      ![Backup-Richtlinie hinzufügen](./media/storsimple-8000-take-backup/step8takebu9.png)
      
      Sie haben jetzt eine Sicherungsrichtlinie, die geplante Sicherungen für Ihre Volumedaten erstellt.




