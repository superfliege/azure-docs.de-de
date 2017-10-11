<!--author=SharS last changed: 9/17/15-->

#### <a name="to-install-maintenance-mode-hotfixes-via-windows-powershell-for-storsimple"></a>So installieren Sie wartungsmodushotfixes mithilfe von Windows PowerShell für StorSimple
> [!IMPORTANT]
> Im Wartungsmodus befindet müssen Sie den Hotfix zuerst auf einen Controller und dann auf den anderen Controller angewendet.
> 
> 

1. Versetzen Sie das Gerät in den Wartungsmodus versetzt. Finden Sie unter [Schritt2: Geben Sie Wartungsmodus](../articles/storsimple/storsimple-update-device.md#step2) für Anweisungen zum Wartungsmodus versetzt.
2. Um das Hotfix angewendet haben, geben Sie Folgendes ein:
   
     `Start-HcsHotfix` 
3. Wenn Sie aufgefordert werden, geben Sie den Pfad für den freigegebenen Netzwerkordner, der die Hotfixdateien enthält.
4. Sie werden zur Bestätigung aufgefordert. Typ **Y** um die Hotfixinstallation fortzusetzen.
5. Nachdem Sie den Hotfix auf einen Controller angewendet haben, melden Sie sich mit dem anderen Controller. Wenden Sie den Hotfix, wie Sie für den vorherigen Controller ausgeführt haben.
6. Nachdem die Hotfixes angewendet werden, beenden Sie den Wartungsmodus. Finden Sie unter [Schritt 4: Wartungsmodus beenden](../articles/storsimple/storsimple-update-device.md#step4) Anweisungen.

