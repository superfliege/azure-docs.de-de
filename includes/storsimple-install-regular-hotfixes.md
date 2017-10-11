<!--author=SharS last changed: 9/17/15-->

#### <a name="to-install-regular-hotfixes-via-windows-powershell-for-storsimple"></a>So installieren Sie reguläre Hotfixes mithilfe von Windows PowerShell für StorSimple
1. Verbinden Sie mit der seriellen Konsole des Geräts. Weitere Informationen finden Sie unter [Schritt 1: Herstellen einer Verbindung mit der seriellen Konsole](../articles/storsimple/storsimple-update-device.md#step1).
2. Wählen Sie im Menü seriellen Konsole Option 1, **Anmelden mit Vollzugriff**. Geben Sie das Kennwort ein. Das Standardkennwort lautet **Password1**.
3. Geben Sie in die Eingabeaufforderung Folgendes ein:
   
    ```
    Start-HcsHotfix
    ```
   
    > [!IMPORTANT]
    >
    > Dieser Befehl gilt nur für reguläre Hotfixes. Führen Sie diesen Befehl auf nur einen Controller, aber beide Controller werden aktualisiert.
    > Sie können ein Controllerfailover während des Aktualisierungsvorgangs feststellen; Das Failover wirkt nicht systemverfügbarkeit oder Vorgang sich jedoch.

4. Wenn Sie aufgefordert werden, geben Sie den Pfad für den freigegebenen Netzwerkordner, der die Hotfixdateien enthält.
5. Sie werden zur Bestätigung aufgefordert. Typ **Y** um die Hotfixinstallation fortzusetzen.

