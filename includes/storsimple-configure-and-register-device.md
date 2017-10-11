<!--author=alkohli last changed: 12/01/15-->


#### <a name="to-configure-and-register-the-device"></a>Zum Konfigurieren und Registrieren des Geräts
1. Zugriff auf die Windows PowerShell-Schnittstelle auf der seriellen Konsole des StorSimple-Geräts. Finden Sie unter [Verwenden von PuTTY zum Herstellen einer seriellen Konsole des Geräts](#use-putty-to-connect-to-the-device-serial-console) Anweisungen. **Achten Sie darauf, dass Sie die Vorgehensweise genau befolgen, oder Sie werden auf die Konsole zugreifen.**
2. Drücken Sie in der Sitzung, die geöffnet wird EINGABETASTE einmal auf eine Eingabeaufforderung einzublenden. 
3. Sie werden aufgefordert, die Sprache auszuwählen, die Sie für Ihr Gerät festlegen möchten. Geben Sie die Sprache, und drücken Sie dann die EINGABETASTE. 
   
    ![StorSimple konfigurieren und Registrieren des Geräts 1](./media/storsimple-configure-and-register-device/HCS_RegisterYourDevice1-include.png)
4. Wählen Sie in der Menü der seriellen Konsole, das angezeigt wird Option 1 aus, um sich mit Vollzugriff Anmelden aus. 
   
    ![Registrieren von Geräten StorSimple 2](./media/storsimple-configure-and-register-device/HCS_RegisterYourDevice2-include.png)
   
     Führen Sie die Schritte 5 bis 12, die mindestens erforderlichen Netzwerkeinstellungen für Ihr Gerät zu konfigurieren. **Diese Konfigurationsschritte müssen für den aktiven Controller des Geräts ausgeführt werden soll.** Menü der seriellen Konsole gibt den Zustand des Controllers in die bannermeldung an. Wenn Sie nicht mit dem aktiven Controller verbunden sind, trennen, und klicken Sie dann mit dem aktiven Controller verbinden.
5. Geben Sie an der Eingabeaufforderung Ihr Kennwort ein. Das Standardkennwort für das Gerät ist **Password1**.
6. Geben Sie den folgenden Befehl ein:
   
     `Invoke-HcsSetupWizard` 
7. Ein Installations-Assistent wird angezeigt, können Sie die Netzwerkeinstellungen für das Gerät zu konfigurieren. Geben Sie die folgenden Informationen: 
   
   * IP-Adresse für die DATA 0-Netzwerkschnittstelle
   * Subnetzmaske
   * Gateway
   * IP-Adresse für den primären DNS-server
   * IP-Adresse für den primären NTP-server
     
     > [!NOTE]
     > Sie müssen möglicherweise warten einige Minuten, bis die Subnetzmaske und die DNS-Einstellungen angewendet werden. Wenn Sie erhalten eine "das Gerät ist nicht bereit." Fehler angezeigt wird, überprüfen Sie die physische Netzwerkverbindung für die Netzwerkschnittstelle DATA 0 Ihres aktiven Controllers.
     > 
     > 
8. (Optional) Ihren Webproxyserver zu konfigurieren. Obwohl die webproxykonfiguration ist optional und **Beachten Sie, dass wenn Sie einen Webproxy verwenden, Sie nur hier konfiguriert werden können**. Weitere Informationen finden Sie unter [Konfigurieren des Webproxys für Ihr Gerät](../articles/storsimple/storsimple-configure-web-proxy.md). Wenn Sie während dieses Schritts Probleme auftreten, finden Sie Hinweise zur Problembehandlung [Fehler während der webproxykonfiguration](../articles/storsimple/storsimple-troubleshoot-deployment.md#errors-during-the-optional-web-proxy-settings).

     > [!NOTE]
     > Sie können zu einem beliebigen Zeitpunkt zum Beenden des Setupassistenten STRG + C drücken. Alle Einstellungen, die Sie vor dem Ausgeben dieses Befehls angewendet werden beibehalten.

1. Aus Gründen der Sicherheit das geräteadministratorkennwort läuft nach der ersten Sitzung ab, und Sie müssen es für die nachfolgenden Sitzungen ändern. Geben Sie bei Aufforderung ein Administratorkennwort für das Gerät. Ein gültiges geräteadministratorkennwort muss 8 bis 15 Zeichen lang sein. Das Kennwort muss eine Kombination von Kleinbuchstaben, Großbuchstaben, Zahlen und Sonderzeichen enthalten.
2. Kennwort des StorSimple-Momentaufnahme-Managers wird ebenfalls hier festgelegt. Verwenden Sie dieses Kennwort, wenn Sie ein Gerät mit StorSimple Snapshot Manager mit dem Windows-Host authentifizieren. Geben Sie bei Aufforderung ein Kennwort 14 bis 15 Zeichen. Das Kennwort muss eine Kombination aus drei der folgenden enthalten: Kleinbuchstaben, Großbuchstaben, numerische und Sonderzeichen. 
   
   ![Registrieren von Geräten StorSimple 4](./media/storsimple-configure-and-register-device/HCS_RegisterYourDevice4-include.png)
   
   Sie können den StorSimple Snapshot Manager-Kennwort in der StorSimple Manager Dienstschnittstelle zurücksetzen. Ausführliche Schritte finden Sie unter [ändern Sie die StorSimple-Kennwörter, die mit dem StorSimple Manager-Dienstes](../articles/storsimple/storsimple-change-passwords.md).
   
   Um die Behandlung von Problemen während dieses Schritts finden Sie Hinweise zur Problembehandlung [Fehler im Zusammenhang mit Kennwörtern](../articles/storsimple/storsimple-troubleshoot-deployment.md#errors-related-to-device-administrator-and-storsimple-snapshot-manager-passwords).
3. Der letzte Schritt im Setup-Assistenten wird Ihr Gerät beim StorSimple Manager-Dienst registriert. Hierzu benötigen Sie den Dienstregistrierungsschlüssel, den Sie in Schritt 2 erhalten haben. Nachdem Sie den Registrierungsschlüssel angeben, müssen Sie möglicherweise warten, 2 bis 3 Minuten, bevor das Gerät registriert ist.
   
   Um alle möglichen Gerät Registrierung Fehler zu beheben, finden Sie unter [Fehler während der geräteregistrierung](../articles/storsimple/storsimple-troubleshoot-deployment.md#errors-during-device-registration). Detaillierte Problembehandlung finden Sie auch auf [Beispiel zur schrittweisen Problembehandlung](../articles/storsimple/storsimple-troubleshoot-deployment.md#step-by-step-storsimple-troubleshooting-example).
4. Nachdem das Gerät registriert ist, wird ein Schlüssel für die Dienstdatenverschlüsselung angezeigt. Kopieren Sie diesen Schlüssel, und speichern Sie sie an einem sicheren Ort.
   
   > [!WARNING]
   > Dieser Schlüssel wird mit dem Dienstregistrierungsschlüssel zum Registrieren zusätzlicher Geräte bei StorSimple Manager-Dienst erforderlich sein. Verweisen auf [Sicherheit von StorSimple](../articles/storsimple/storsimple-security.md) für Weitere Informationen zu diesem Schlüssel.
   > 
   > 
   
    ![Registrieren von Geräten StorSimple 6](./media/storsimple-configure-and-register-device/HCS_RegisterYourDevice6-include.png)
   
    Wählen Sie den Text einfach, zum Kopieren des Texts aus dem Fenster der seriellen Konsole. Sie sollten klicken Sie dann in die Zwischenablage oder einem beliebigen Texteditor einfügen können. Verwenden Sie nicht STRG + C, um den Verschlüsselungsschlüssel für Dienstdaten zu kopieren. Verwenden STRG + C bewirkt zum Beenden des Setupassistenten. Daher das geräteadministratorkennwort und das Kennwort für StorSimple Snapshot Manager werden nicht geändert werden, und das Gerät wird auf die Standardkennwörter zurückgesetzt.
5. Beenden Sie die serielle Konsole.
6. Kehren Sie zum klassischen Azure-Portal zurück, und schließen Sie die folgenden Schritte aus:
   
   1. Doppelklicken Sie auf Ihrem StorSimple-Manager-Dienst den Zugriff auf die **Schnellstart** Seite.
   2. Klicken Sie auf **anzeigen verbundener Geräte**.
   3. Auf der **Geräte** Seite, stellen Sie sicher, dass das Gerät erfolgreich mit dem Dienst verbunden wurde, indem Sie seinen Status. Der Gerätestatus sollte **Online**. Wenn der Gerätestatus **Offline**, warten Sie einige Minuten, bis das Gerät online geschaltet werden kann.
   
   ![Seite "StorSimple-Geräte"](./media/storsimple-configure-and-register-device/HCS_DevicesPageM-include.png) 
   
   > [!IMPORTANT]
   > Nachdem das Gerät online ist, schließen Sie das Netzwerkkabel, die Sie am Anfang dieses Schritts getrennt wurde.
   > 
   > 

Nachdem das Gerät erfolgreich registriert und ist nicht online geschaltet werden, können Sie Ausführen den `Test-HcsmConnection -Verbose` um sicherzustellen, dass die Netzwerkkonnektivität fehlerfrei ist. Die ausführliche Informationen zur Nutzung des Cmdlets finden Sie unter [Cmdlet-Referenz für den Test-HcsmConnection](https://technet.microsoft.com/library/dn715782.aspx).

![Video zur Verfügung](./media/storsimple-configure-and-register-device/Video_icon.png) **Video verfügbar**

Um ein Video zu beobachten, die veranschaulicht, wie konfigurieren und registrieren Ihr Gerät über Windows PowerShell für StorSimple, klicken Sie auf [hier](https://azure.microsoft.com/documentation/videos/initialize-the-storsimple-appliance/).

