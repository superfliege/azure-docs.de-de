<!--author=SharS last changed: 03/17/2016-->

#### <a name="to-download-hotfixes"></a>Zum Herunterladen von Updates
Führen Sie die folgenden Schritte aus, um das Softwareupdate herunterladen.

1. Starten Sie Internet Explorer, und navigieren Sie zu [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).
2. Wenn dies zum ersten Mal verwenden Microsoft Update-Katalog auf diesem Computer ist, klicken Sie auf **installieren** bei der Aufforderung zum Installieren von Microsoft Update-Katalog-Add-On.
    ![Katalog installieren](./media/storsimple-install-update-option-1/HCS_InstallCatalog-include.png)
3. Geben Sie in das Suchfeld des Microsoft Update-Katalogs, die Anzahl von Knowledge Base (KB) für den Hotfix heruntergeladen werden können, z. B. sollen **3063418**, und klicken Sie dann auf **Suche**.
4. Daraufhin werden die **StorSimple Update 1.2 Appliance Update** Paket. Klicken Sie auf **Hinzufügen**. Das Update wird in den Warenkorb hinzugefügt werden.
5. Suchen Sie nach zusätzlichen Hotfixes in der obigen Tabelle aufgeführten (**3043005** und **3063416**), und jede Warenkorb hinzufügen.
6. Klicken Sie auf **Auswahlkorb anzeigen**.
   
    ![Auswahlkorb anzeigen](./media/storsimple-install-update-option-1/HCS_InstallBasket-include.png)
7. Klicken Sie auf **Herunterladen**. Geben oder **Durchsuchen** in einen lokalen Speicherort, der die Downloads angezeigt werden soll. Die Updates werden am angegebenen Speicherort heruntergeladen und in einen Unterordner mit dem gleichen Namen wie das Update platziert. Der Ordner kann auch auf eine Netzwerkfreigabe kopiert werden, der vom Gerät erreichbar ist.

> [!NOTE]
> Die Hotfixes muss von beiden Controllern erkennen alle möglichen Fehlermeldungen über den peercontroller zugänglich sein.
> 
> 

#### <a name="to-install-and-verify-regular-mode-hotfixes"></a>Installieren und den normalen Modus Hotfixes überprüfen
Führen Sie die folgenden Schritte zum Installieren und überprüfen die Hotfixes im normalen Modus. Wenn Sie bereits mit der Azure-Portal installiert haben, fahren Sie mit [installieren und Überprüfen von Hotfixes im Wartungsmodus](#to-install-and-verify-maintenance-mode-hotfixes).

1. Zugriff auf die Windows PowerShell-Schnittstelle auf der seriellen Konsole des StorSimple-Geräts, um das Softwareupdate zu installieren. Führen Sie die detaillierten Anweisungen im [PuTTy verwenden, für die Verbindung mit der seriellen Konsole](../articles/storsimple/storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console). Drücken Sie die an der Eingabeaufforderung **EINGABETASTE**.
2. Wählen Sie **Option 1** , melden Sie sich bei dem Gerät mit Vollzugriff.
3. Um die Installation des Updatepakets, an der Eingabeaufforderung, geben Sie Folgendes ein:
   
    `Start-HcsHotfix -Path <path to update file> -Credential <credentials in domain\username format>`
   
    Verwenden Sie IP-anstelle von DNS im Freigabepfad in der oben aufgeführten Befehl an. Die Credential-Parameter wird verwendet, nur, wenn Sie eine authentifizierte Freigabe zugreifen.
   
    Es wird empfohlen, dass Sie die Credential-Parameter verwenden, zum Zugreifen auf Freigaben. Sogar Freigaben, die auf "Jeder" geöffnet sind sind in der Regel nicht für nicht authentifizierte Benutzer geöffnet.
   
    Nachfolgend finden Sie eine Beispielausgabe.
   
    ```
    Controller0>Start-HcsHotfix -Path \\10.100.100.100\share
    \hcsmdssoftwareupdate.exe -Credential contoso\John

    Confirm

    This operation starts the hotfix installation and could reboot one or
    both of the controllers. If the device is serving I/Os, these will not
    be disrupted. Are you sure you want to continue?
    [Y] Yes [N] No [?] Help (default is "Y"): Y
    ```

4. Typ **Y** bei der Aufforderung zum Installieren des Hotfixes zu bestätigen.
5. Überwachen Sie das Update mithilfe der `Get-HcsUpdateStatus` Cmdlet.
   
    Die folgende Beispielausgabe zeigt das Update wird ausgeführt. Die `RunInprogress` werden `True` Wenn das Update wird ausgeführt.
   
    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : True
    LastHotfixTimestamp : 9/02/2015 10:36:13 PM
    LastUpdateTimestamp : 9/02/2015 10:35:25 PM
    Controller0Events   :
    Controller1Events   :
    ```
   
     Die folgende Beispielausgabe zeigt an, dass das Update abgeschlossen ist. Die `RunInProgress` werden `False` nachdem das Update abgeschlossen wurde.

    ```
    Controller1>Get-HcsUpdateStatus

    RunInprogress       : False
    LastHotfixTimestamp : 9/02/2015 10:56:13 PM
    LastUpdateTimestamp : 9/02/2015 10:35:25 PM
    Controller0Events   :
    Controller1Events   :
    ```
   
   > [!NOTE]
   > In einigen Fällen die Cmdlet-Berichte `False` Wenn das Update wird noch ausgeführt. Um sicherzustellen, dass das Update abgeschlossen ist, warten Sie einige Minuten, führen Sie diesen Befehl erneut aus, und überprüfen Sie, ob die `RunInProgress` ist `False`. Wenn dies der Fall, wurde das Update abgeschlossen.
   > 
   > 
6. Wenn die Software Update abgeschlossen ist, überprüfen Sie die System-Software-Versionen. Geben Sie den folgenden Befehl ein:
   
    `Get-HcsSystem`
   
    Daraufhin sollte die folgenden Versionen:
   
   * HcsSoftwareVersion: 6.3.9600.17584
   * CisAgentVersion: 1.0.9049.0
   * MdsAgentVersion: 26.0.4696.1433
     
     Wenn die Versionsnummern nach dem Anwenden des Updates nicht ändern, bedeutet dies, dass es sich bei der Hotfix nicht angewendet werden konnte. Sollte es jedoch angezeigt, wenden Sie sich an [Microsoft-Support](../articles/storsimple/storsimple-contact-microsoft-support.md) für weitere Unterstützung zu erhalten.
7. Wiederholen Sie Schritte 3 bis 5, um den verbleibenden im normalen Modus Hotfix (KB3043005) installieren.

#### <a name="to-install-and-verify-maintenance-mode-hotfixes"></a>Installieren und Überprüfen von Hotfixes im Wartungsmodus
Verwenden von KB3063416, Datenträger-Firmware-Updates zu installieren. Diese sind störende Updates und ca. 30 bis 45 Minuten dauern, bis zum Abschließen. Sie können diese in eines geplanten Wartungsfensters durch Herstellen einer Verbindung mit der seriellen Konsole des Geräts installieren.

Um die Datenträger-Firmware-Updates zu installieren, müssen befolgen Sie die nachstehenden Anweisungen.

1. Versetzen Sie das Gerät im Wartungsmodus befindet. Beachten Sie, dass Sie nicht die Windows PowerShell-Remoting, beim Verbinden mit einem Gerät im Wartungsmodus befindet verwenden soll. Sie müssen dieses Cmdlet auf der Gerätecontroller, die bei der Verbindung über die serielle Konsole des Geräts ausführen. Type:
   
    `Enter-HcsMaintenanceMode`
   
    Nachfolgend finden Sie eine Beispielausgabe.
   
        Controller0>Enter-HcsMaintenanceMode
        Checking device state...
   
        In maintenance mode, your device will not service IOs and will be disconnected from the Microsoft Azure StorSimple Manager service. Entering maintenance mode will end the current session and reboot both controllers, which takes a few minutes to complete. Are you sure you want to enter maintenance mode?
        [Y] Yes [N] No (Default is "Y"): Y
   
        -----------------------MAINTENANCE MODE------------------------
        Microsoft Azure StorSimple Appliance Model 8100
        Name: Update1-8100-SHG0997879L76YD
        Software Version: 6.3.9600.17584
        Copyright (C) 2014 Microsoft Corporation. All rights reserved.
        You are connected to Controller0 - Passive
        ---------------------------------------------------------------
        Serial Console Menu
        [1] Log in with full access
        [2] Log into peer controller with full access
        [3] Connect with limited access
        [4] Change language
        Please enter your choice>
   
    Beide Controller neu starten klicken Sie dann in den Wartungsmodus versetzt.
2. Um die Aktualisierung der Firmware zu installieren, geben Sie Folgendes ein:
   
    `Start-HcsHotfix -Path <path to update file> -Credential <credentials in domain\username format>`
   
    Nachfolgend finden Sie eine Beispielausgabe.
   
        Controller1>Start-HcsHotfix -Path \\10.100.100.100\share\DiskFirmwarePackage.exe -Credential contoso\john
        Enter Password:
        WARNING: In maintenance mode, hotfixes should be installed on each controller sequentially. After the hotfix is installed on this controller, install it on the peer controller.
        Confirm
        This operation starts a hotfix installation and could reboot one or both of the controllers. Are you sure you want to continue?
        [Y] Yes [N] No (Default is "Y"): Y
        WARNING: Installation is currently in progress. This operation can take several minutes to complete.
3. Überwachen der Installation von teamstatusinformationen mit `Get-HcsUpdateStatus` Befehl. Das Update ist abgeschlossen, wenn die `RunInProgress` ändert sich in `False`.
4. Nachdem die Installation abgeschlossen ist, wird der Controller, auf dem der Wartungsmodus-Hotfix installiert wurde, neu gestartet werden. Melden Sie sich als Option 1 mit Vollzugriff, und überprüfen Sie die Firmwareversion des Datenträgers. Type:
   
   `Get-HcsFirmwareVersion`
   
   Die erwartete Datenträger-Firmware-Versionen sind:
   
   `XMGG, XGEE, KZ50, F6C2, VR08`
   
   Führen Sie die `Get-HcsFirmwareVersion` Befehl auf dem zweiten Controller aus, um sicherzustellen, dass die Version der Software aktualisiert wurde. Dann können Sie den Wartungsmodus beenden. Geben Sie den folgenden Befehl für jeden Gerätecontroller an:
   
   `Exit-HcsMaintenanceMode`
5. Beim Beenden des Wartungsmodus starten Sie die Controller neu. Nach der datenträgerfirmware Updates erfolgreich angewendet werden, und das Gerät im Wartungsmodus können zurück zum klassischen Azure-Portal beendet wurde. Beachten Sie, dass das Portal nicht angezeigt, dass Sie die Updates für den Wartungsmodus für 24 Stunden installiert.

