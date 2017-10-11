<!--author=SharS last changed: 11/18/16-->

#### <a name="to-install-regular-updates-via-windows-powershell-for-storsimple"></a>So installieren Sie reguläre Updates über Windows PowerShell für StorSimple
1. Öffnen Sie die serielle Konsole des Geräts und wählen Sie Option 1, **Anmelden mit Vollzugriff**. Geben Sie das Kennwort ein. Das Standardkennwort lautet *Password1*. 
2. Geben Sie in die Eingabeaufforderung Folgendes ein:
   
     `Get-HcsUpdateAvailability`
   
    Sie werden benachrichtigt, wenn Updates verfügbar sind und ob die Updates Unterbrechung oder unterbrechungsfreie sind.
3. Geben Sie in die Eingabeaufforderung Folgendes ein:
   
     `Start-HcsUpdate`
   
    Der Aktualisierungsvorgang wird gestartet.

> [!IMPORTANT]
> * Dieser Befehl gilt nur für reguläre Updates. Führen Sie diesen Befehl auf nur einen Controller, aber beide Controller werden aktualisiert. 
> * Sie können ein Controllerfailover während des Aktualisierungsvorgangs feststellen; Das Failover wirkt nicht systemverfügbarkeit oder Vorgang sich jedoch.
> 
> 

