<!--author=SharS last changed: 9/17/15-->

#### <a name="to-install-maintenance-mode-updates-via-windows-powershell-for-storsimple"></a>So installieren Sie wartungsmodusupdates mithilfe von Windows PowerShell für StorSimple
1. Wenn dies bereits geschehen ist, Zugriff auf die serielle Konsole des Geräts, und wählen Sie Option 1, **Anmelden mit Vollzugriff**. 
2. Geben Sie das Kennwort ein. Das Standardkennwort lautet **Password1**.
3. Geben Sie in die Eingabeaufforderung Folgendes ein:
   
     `Get-HcsUpdateAvailability` 
4. Sie werden benachrichtigt, wenn Updates verfügbar sind und ob die Updates Unterbrechung oder unterbrechungsfreie sind. Um störende Updates zu installieren, müssen Sie das Gerät in den Wartungsmodus zu versetzen. Finden Sie unter [Schritt2: Geben Sie Wartungsmodus](../articles/storsimple/storsimple-update-device.md#step2) Anweisungen.
5. Wenn Ihr Gerät im Wartungsmodus befindet, ist an der Eingabeaufforderung eingeben:`Start-HcsUpdate`
6. Sie werden zur Bestätigung aufgefordert. Nachdem Sie die Updates bestätigt haben, werden sie auf dem Controller installiert, die Sie zurzeit zugreifen. Nachdem die Updates installiert wurden, wird der Controller neu gestartet. 
7. Überwachen des Status von Updates. Type:
   
    `Get-HcsUpdateStatus`
   
    Wenn die `RunInProgress` ist `True`, das Update wird noch ausgeführt. Wenn `RunInProgress` ist `False`, bedeutet dies, dass das Update abgeschlossen wurde.  
8. Wenn das Update auf dem aktuellen Controller installiert ist, und er neu gestartet wurde, Herstellen einer Verbindung mit dem anderen Controller, und führen Sie die Schritte 1 bis 6.
9. Nachdem beide Controller aktualisiert werden, beenden Sie den Wartungsmodus. Finden Sie unter [Schritt 4: Wartungsmodus beenden](../articles/storsimple/storsimple-update-device.md#step4) Anweisungen.

