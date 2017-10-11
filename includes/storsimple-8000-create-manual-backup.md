
<!--author=alkohli last changed: 01/20/2017-->

#### <a name="to-create-a-manual-backup"></a>Zum Erstellen einer manuellen Sicherung

1. Wechseln Sie zu Ihrer StorSimple-Geräte-Manager-Dienst, und klicken Sie dann auf **Geräte**. Wählen Sie aus der tabellarischen Auflistung von Geräten, Ihr Gerät aus. Wechseln Sie zu **Einstellungen > Verwalten > Sicherungsrichtlinien**.

2. Die **Sicherungsrichtlinien** Blatt Listet alle Sicherungsrichtlinien in einem tabellarischen Format, einschließlich der Richtlinie für das Volume, die Sie sichern möchten. Wählen Sie die Richtlinie, die dem Volume, die Sie sichern möchten, und mit der rechten Maustaste das Kontextmenü aufrufen. Wählen Sie aus der Dropdown-Liste **jetzt sichern**.

    ![Erstellen Sie eine manuelle Sicherung](./media/storsimple-8000-create-manual-backup/createmanualbu1.png)

3. In der **jetzt sichern** Blatt die folgenden Schritte aus:

    1. Wählen Sie die entsprechende **Snapshot-Typ** aus der Dropdownliste aus: **lokale** Momentaufnahme oder **Cloud** Momentaufnahme. Wählen Sie lokale Momentaufnahme für schnelle Sicherungen oder Wiederherstellungen und Cloud-Momentaufnahme für die resilienz von Daten.

        ![Erstellen Sie eine manuelle Sicherung](./media/storsimple-8000-create-manual-backup/createmanualbu2.png)

    2. Klicken Sie auf **OK** zum Starten eines Auftrags zum Erstellen einer Momentaufnahme. Eine Benachrichtigung am oberen Rand der Seite wird angezeigt, nachdem der Auftrag erfolgreich erstellt wurde.

        ![Erstellen Sie eine manuelle Sicherung](./media/storsimple-8000-create-manual-backup/createmanualbu4.png)

    3. Klicken Sie auf die Benachrichtigung, um den Auftrag zu überwachen. Dadurch gelangen Sie zu der **Aufträge** Blade, in dem Sie den Status des Auftrags anzeigen können.


5. Nachdem der Sicherungsauftrag abgeschlossen ist, navigieren Sie zu der **Sicherungskatalog** Registerkarte.

6. Legen Sie die Filterauswahl auf die entsprechenden Gerätegruppen, Sicherungsrichtlinie und Zeitraum. Die Sicherung sollte in der Liste der Sicherungssätze angezeigt werden, die im Katalog angezeigt wird.

