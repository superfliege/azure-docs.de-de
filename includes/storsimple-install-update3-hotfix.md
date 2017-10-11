<!--author=alkohli last changed: 09/01/16-->

#### <a name="to-download-hotfixes"></a>Zum Herunterladen von Updates
Führen Sie die folgenden Schritte aus, um das Softwareupdate von Microsoft Update-Katalog herunterladen.

1. Starten Sie Internet Explorer, und navigieren Sie zu [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).
2. Wenn dies zum ersten Mal verwenden Microsoft Update-Katalog auf diesem Computer ist, klicken Sie auf **installieren** bei der Aufforderung zum Installieren von Microsoft Update-Katalog-Add-On.
    ![Katalog installieren](./media/storsimple-install-update2-hotfix/HCS_InstallCatalog-include.png)
3. Geben Sie in das Suchfeld des Microsoft Update-Katalogs, die Anzahl von Knowledge Base (KB) für den Hotfix heruntergeladen werden können, z. B. sollen **3186843**, und klicken Sie dann auf **Suche**.
   
    Der Hotfix Liste angezeigt wird, z. B. **kumulative Software Bundle-Update 3.0 für StorSimple 8000-Serie**.
   
    ![Search-Katalog](./media/storsimple-install-update2-hotfix/HCS_SearchCatalog1-include.png)
4. Klicken Sie auf **Hinzufügen**. Das Update wird in den Warenkorb hinzugefügt.
5. Suchen Sie nach zusätzlichen Hotfixes in der obigen Tabelle aufgeführten (**3186859**), und fügen Sie jedes Element der Warenkorb hinzu.
6. Klicken Sie auf **Auswahlkorb anzeigen**.
7. Klicken Sie auf **Herunterladen**. Geben oder **Durchsuchen** in einen lokalen Speicherort, der die Downloads angezeigt werden soll. Die Updates werden am angegebenen Speicherort heruntergeladen und in einen Unterordner mit dem gleichen Namen wie das Update platziert. Der Ordner kann auch auf eine Netzwerkfreigabe kopiert werden, der vom Gerät erreichbar ist.

> [!NOTE]
> Die Hotfixes muss von beiden Controllern erkennen alle möglichen Fehlermeldungen über den peercontroller zugänglich sein.
> 
> 

#### <a name="to-install-and-verify-regular-mode-hotfixes"></a>Installieren und den normalen Modus Hotfixes überprüfen
Führen Sie die folgenden Schritte zum Installieren und Überprüfen von Hotfixes im normalen Modus. Wenn Sie bereits mit der Azure-Portal installiert haben, fahren Sie mit [installieren und Überprüfen von Hotfixes im Wartungsmodus](#to-install-and-verify-maintenance-mode-hotfixes).

1. Zugriff auf die Windows PowerShell-Schnittstelle auf der seriellen Konsole des StorSimple-Geräts, um die Hotfixes zu installieren. Führen Sie die detaillierten Anweisungen im [PuTTy verwenden, für die Verbindung mit der seriellen Konsole](../articles/storsimple/storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console). Drücken Sie die an der Eingabeaufforderung **EINGABETASTE**.
2. Wählen Sie **Option 1** , melden Sie sich bei dem Gerät mit Vollzugriff. Es wird empfohlen, dass Sie den Hotfix auf dem passiven Controller installiert.
3. Geben Sie Folgendes ein, um das Hotfix, an der Eingabeaufforderung zu installieren:
   
    `Start-HcsHotfix -Path <path to update file> -Credential <credentials in domain\username format>`
   
    Verwenden Sie IP-anstelle von DNS im Freigabepfad in der oben aufgeführten Befehl an. Die Credential-Parameter wird verwendet, nur, wenn Sie eine authentifizierte Freigabe zugreifen.
   
    Es wird empfohlen, dass Sie die Credential-Parameter verwenden, zum Zugreifen auf Freigaben. Sogar Freigaben, die auf "Jeder" geöffnet sind sind in der Regel nicht für nicht authentifizierte Benutzer geöffnet.
   
    Geben Sie das Kennwort, wenn Sie aufgefordert werden.
   
    Nachfolgend finden Sie eine Beispielausgabe.
   
        ````
        Controller0>Start-HcsHotfix -Path \\10.100.100.100\share
        \hcsmdssoftwareupdate.exe -Credential contoso\John
   
        Confirm
   
        This operation starts the hotfix installation and could reboot one or
        both of the controllers. If the device is serving I/Os, these will not
        be disrupted. Are you sure you want to continue?
        [Y] Yes [N] No [?] Help (default is "Y"): Y
   
        ````
4. Typ **Y** bei der Aufforderung zum Installieren des Hotfixes zu bestätigen.
5. Überwachen Sie das Update mithilfe der `Get-HcsUpdateStatus` Cmdlet. Das Update wird zunächst auf dem passiven Controller ausführen. Sobald der passive Controller aktualisiert wird, wird ein Failover und das Update wird dann auf den anderen Controller angewendet. Die Aktualisierung ist abgeschlossen, wenn beide Controller aktualisiert werden.
   
    Die folgende Beispielausgabe zeigt das Update wird ausgeführt. Die `RunInprogress` werden `True` Wenn das Update wird ausgeführt.

    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : True
    LastHotfixTimestamp :
    LastUpdateTimestamp : 8/29/2016 2:04:02 AM
    Controller0Events   :
    Controller1Events   :
    ```
   
     Die folgende Beispielausgabe zeigt an, dass das Update abgeschlossen ist. Die `RunInProgress` werden `False` nachdem das Update abgeschlossen wurde.
   
    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : False
    LastHotfixTimestamp : 8/30/2016 9:15:55 AM
    LastUpdateTimestamp : 8/30/2016 9:06:07 AM
    Controller0Events   :
    Controller1Events   :
    ```

    > [!NOTE] 
    > In einigen Fällen die Cmdlet-Berichte `False` Wenn das Update wird noch ausgeführt. Um sicherzustellen, dass das Update abgeschlossen ist, warten Sie einige Minuten, führen Sie diesen Befehl erneut aus, und überprüfen Sie, ob die `RunInProgress` ist `False`. Wenn dies der Fall, wurde das Update abgeschlossen.

1. Wenn die Software Update abgeschlossen ist, überprüfen Sie die System-Software-Versionen. Type:
   
    `Get-HcsSystem`
   
    Daraufhin sollte die folgenden Versionen:
   
   * `HcsSoftwareVersion: 6.3.9600.17759`
   * `CisAgentVersion:  1.0.9343.0`
   * `MdsAgentVersion: 30.0.4698.16`
     
     Wenn die Versionsnummern nach dem Anwenden des Updates nicht ändern, bedeutet dies, dass es sich bei der Hotfix nicht angewendet werden konnte. Sollte es jedoch angezeigt, wenden Sie sich an [Microsoft-Support](../articles/storsimple/storsimple-contact-microsoft-support.md) für weitere Unterstützung zu erhalten.
     
     > [!IMPORTANT]
     > Sie müssen über den aktiven Controller neu starten, die `Restart-HcsController` Cmdlet vor dem Anwenden der verbleibenden aktualisiert. 
     > 
     > 
2. Wiederholen Sie die Schritte 3 bis 5, um die LSI-Treiber und Firmware-Hotfix installieren **KB3186859**. Nachdem der Hotfix installiert ist, verwenden Sie die `Get-HcsSystem` Cmdlet. Die LSI-Version muss sein:
   
   * `Lsisas2Version: 2.0.78.00`
3. Wiederholen Sie die Schritte 3 bis 5, um die Updateinstallation Storport und Spaceport **KB3121261**.
4. Wenn Sie von Update 2 oder eine frühere Version aktualisieren, müssen Sie auch herunterladen:
   
   * iSCSI-Lösung mit KB3146621
   * WMI-Lösung mit KB3103616

#### <a name="to-install-and-verify-maintenance-mode-hotfixes"></a>Installieren und Überprüfen von Hotfixes im Wartungsmodus
Verwenden von KB3121899, Datenträger-Firmware-Updates zu installieren. Diese sind störende Updates und ca. 30 Minuten dauern, um abzuschließen. Sie können diese in eines geplanten Wartungsfensters durch Herstellen einer Verbindung mit der seriellen Konsole des Geräts installieren.

Beachten Sie, wenn Ihre datenträgerfirmware bereits auf dem neuesten Stand ist, müssen Sie wird nicht auf diese Updates zu installieren. Führen Sie die `Get-HcsUpdateAvailability` Cmdlet über die serielle Konsole des Geräts zum Überprüfen, ob Updates verfügbar sind und ob die Updates störende sind (Wartungsmodus) oder Updates unterbrechen den Betrieb nicht (normalen Modus).

Um die Datenträger-Firmware-Updates zu installieren, müssen befolgen Sie die nachstehenden Anweisungen.

1. Versetzen Sie das Gerät befindet sich im Wartungsmodus. Beachten Sie, dass Sie nicht die Windows PowerShell-Remoting, beim Verbinden mit einem Gerät im Wartungsmodus befindet verwenden soll. Führen Sie stattdessen dieses Cmdlet auf dem Gerätecontroller über die serielle Konsole des Geräts verbunden. Type:
   
    `Enter-HcsMaintenanceMode`
   
    Nachfolgend finden Sie eine Beispielausgabe.
   
        Controller0>Enter-HcsMaintenanceMode
        Checking device state...
   
        In maintenance mode, your device will not service IOs and will be disconnected from the Microsoft Azure StorSimple Manager service. Entering maintenance mode will end the current session and reboot both controllers, which takes a few minutes to complete. Are you sure you want to enter maintenance mode?
        [Y] Yes [N] No (Default is "Y"): Y
   
        -----------------------MAINTENANCE MODE------------------------
        Microsoft Azure StorSimple Appliance Model 8100
        Name: Update2-8100-SHG0997879L76673
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
        This operation starts a hotfix installation and could reboot one or both of the controllers. By installing new updates you agree to, and accept any additional terms associated with, the new functionality listed in the release notes (https://go.microsoft.com/fwLink/?LinkID=613790). Are you sure you want to continue?
        [Y] Yes [N] No (Default is "Y"): Y
        WARNING: Installation is currently in progress. This operation can take several minutes to complete.
3. Überwachen der Installation von teamstatusinformationen mit `Get-HcsUpdateStatus` Befehl. Das Update ist abgeschlossen, wenn die `RunInProgress` ändert sich in `False`.
4. Nachdem die Installation abgeschlossen ist, installiert der Controller, auf dem der Wartungsmodus-Hotfix wurde, neu gestartet wird. Melden Sie sich als Option 1 mit Vollzugriff, und überprüfen Sie die Firmwareversion des Datenträgers. Type:
   
   `Get-HcsFirmwareVersion`
   
   Die erwartete Datenträger-Firmware-Versionen sind:
   
   `XMGG, XGEG, KZ50, F6C2, VR08`
   
   Nachfolgend finden Sie eine Beispielausgabe.
   
       -----------------------MAINTENANCE MODE------------------------
       Microsoft Azure StorSimple Appliance Model 8100
       Name: Update2-8100-SHG0997879L76YD
       Software Version: 6.3.9600.17705
       Copyright (C) 2014 Microsoft Corporation. All rights reserved.
       You are connected to Controller1
       ---------------------------------------------------------------
   
       Controller1>Get-HcsFirmwareVersion
   
       Controller0 : TalladegaFirmware
         ActiveBIOS:0.45.0006
         BackupBIOS:0.45.0008
         MainCPLD:17.0.0005
         ActiveBMCRoot:2.0.000E
         BackupBMCRoot:2.0.000E
         BMCBoot:2.0.0001
         LsiFirmware:19.00.00.00
         LsiBios:07.37.00.00
         Battery1Firmware:06.29
         Battery2Firmware:06.29
         DomFirmware:X231600
         CanisterFirmware:3.5.0.32
         CanisterBootloader:5.03
         CanisterConfigCRC:0xD1B030A4
         CanisterVPDStructure:0x06
         CanisterGEMCPLD:0x17
         CanisterVPDCRC:0xEE3504B4
         MidplaneVPDStructure:0x0C
         MidplaneVPDCRC:0xA6BD4F64
         MidplaneCPLD:0x10
         PCM1Firmware:1.00|1.05
         PCM1VPDStructure:0x05
         PCM1VPDCRC:0x41BEF99C
         PCM2Firmware:1.00|1.05
         PCM2VPDStructure:0x05
         PCM2VPDCRC:0x41BEF99C
   
         DisksFirmware
         SEAGATE:ST400FM0073:XGEG
         SEAGATE:ST400FM0073:XGEG
         SEAGATE:ST400FM0073:XGEG
         SEAGATE:ST400FM0073:XGEG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
   
    Führen Sie die `Get-HcsFirmwareVersion` Befehl auf dem zweiten Controller aus, um sicherzustellen, dass die Version der Software aktualisiert wurde. Dann können Sie den Wartungsmodus beenden. Geben Sie hierzu den folgenden Befehl für jeden Gerätecontroller aus:
   
   `Exit-HcsMaintenanceMode`
5. Beim Beenden des Wartungsmodus starten Sie die Controller neu. Nach der datenträgerfirmware Updates erfolgreich angewendet werden, und das Gerät im Wartungsmodus können zurück zum klassischen Azure-Portal beendet wurde. Beachten Sie, dass das Portal nicht angezeigt, dass Sie die Updates für den Wartungsmodus für 24 Stunden installiert.

