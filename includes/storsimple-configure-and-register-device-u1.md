<!--author=alkohli last changed: 02/22/2016-->


### <a name="to-configure-and-register-the-device"></a>Zum Konfigurieren und Registrieren des Geräts
1. Zugriff auf die Windows PowerShell-Schnittstelle auf der seriellen Konsole des StorSimple-Geräts. Finden Sie unter [Verwenden von PuTTY zum Herstellen einer seriellen Konsole des Geräts](#use-putty-to-connect-to-the-device-serial-console) Anweisungen. **Achten Sie darauf, dass Sie die Vorgehensweise genau befolgen, oder Sie werden auf die Konsole zugreifen.**
2. Drücken Sie in der Sitzung, die geöffnet wird EINGABETASTE einmal auf eine Eingabeaufforderung einzublenden. 
3. Sie werden aufgefordert, die Sprache auszuwählen, die Sie für Ihr Gerät festlegen möchten. Geben Sie die Sprache, und drücken Sie dann die EINGABETASTE. 
   
    ![StorSimple konfigurieren und Registrieren des Geräts 1](./media/storsimple-configure-and-register-device-u1/HCS_RegisterYourDevice1-U1-include.png)
4. Wählen Sie in der Menü der seriellen Konsole, das angezeigt wird Option 1 aus, um sich mit Vollzugriff Anmelden aus. 
   
    ![Registrieren von Geräten StorSimple 2](./media/storsimple-configure-and-register-device-u1/HCS_RegisterYourDevice2_U1-include.png)
   
     Führen Sie die Schritte 5 bis 12, die mindestens erforderlichen Netzwerkeinstellungen für Ihr Gerät zu konfigurieren. **Diese Konfigurationsschritte müssen für den aktiven Controller des Geräts ausgeführt werden soll.** Menü der seriellen Konsole gibt den Zustand des Controllers in die bannermeldung an. Wenn Sie nicht mit dem aktiven Controller verbunden sind, trennen, und klicken Sie dann mit dem aktiven Controller verbinden.
5. Geben Sie an der Eingabeaufforderung Ihr Kennwort ein. Das Standardkennwort für das Gerät ist **Password1**.
6. Geben Sie den folgenden Befehl: `Invoke-HcsSetupWizard`. 
7. Ein Installations-Assistent wird angezeigt, können Sie die Netzwerkeinstellungen für das Gerät zu konfigurieren. Geben Sie die folgenden Informationen: 
   
   * IP-Adresse für die DATA 0-Netzwerkschnittstelle
   * Subnetzmaske
   * Gateway
   * IP-Adresse für den primären DNS-server
     
        Beachten Sie, dass das System nach jedem Schritt im Prozess Validierung Netzwerkeinstellungen ausführt.
     
     > [!NOTE]
     > Sie müssen möglicherweise warten einige Minuten, bis die Subnetzmaske und die DNS-Einstellungen angewendet werden. Wenn Sie eine Fehlermeldung "Überprüfen die Netzwerkverbindung mit Data 0" erhalten, überprüfen Sie die physische Netzwerkverbindung für die Netzwerkschnittstelle DATA 0 Ihres aktiven Controllers.
     > 
     > 
8. (Optional) Ihren Webproxyserver zu konfigurieren. Obwohl die webproxykonfiguration ist optional und **Beachten Sie, dass wenn Sie einen Webproxy verwenden, Sie nur hier konfiguriert werden können**. Weitere Informationen finden Sie unter [Konfigurieren des Webproxys für Ihr Gerät](../articles/storsimple/storsimple-configure-web-proxy.md).
9. Konfigurieren Sie einen primären NTP-Server für Ihr Gerät. NTP-Server sind erforderlich, wie die Zeit, damit er mit Ihrem clouddienstanbieter authentifizieren kann Ihr Gerät synchronisiert werden muss. Stellen Sie sicher, dass Ihr Netzwerk NTP-Datenverkehr vom Rechenzentrum mit dem Internet zulässt. Wenn dies nicht möglich ist, geben Sie einen internen NTP-Server. 
10. Aus Gründen der Sicherheit das geräteadministratorkennwort läuft nach der ersten Sitzung ab, und Sie es jetzt ändern müssen. Geben Sie bei Aufforderung ein Administratorkennwort für das Gerät. Ein gültiges geräteadministratorkennwort muss 8 bis 15 Zeichen lang sein. Das Kennwort muss drei der folgenden enthalten: Kleinbuchstaben, Großbuchstaben, numerische und Sonderzeichen.
    
    <br/>![Registrieren von Geräten StorSimple 5](./media/storsimple-configure-and-register-device-u1/HCS_RegisterYourDevice5_U1-include.png)
11. Der letzte Schritt im Setup-Assistenten wird Ihr Gerät beim StorSimple Manager-Dienst registriert. Hierzu benötigen Sie den Dienstregistrierungsschlüssel, den Sie in Schritt 2 erhalten haben. Nachdem Sie den Registrierungsschlüssel angeben, müssen Sie möglicherweise warten, 2 bis 3 Minuten, bevor das Gerät registriert ist.
    
    > [!NOTE]
    > Sie können zu einem beliebigen Zeitpunkt zum Beenden des Setupassistenten STRG + C drücken. Wenn Sie alle Einstellungen des Netzwerks (IP-Adresse für Data 0, Subnetzmaske und Gateway) eingegeben haben, werden Ihre Einträge beibehalten.
    > 
    > 
    
    ![Registrieren von Geräten StorSimple 6](./media/storsimple-configure-and-register-device-u1/HCS_RegisterYourDevice6_U1-include.png)
12. Nachdem das Gerät registriert ist, wird ein Schlüssel für die Dienstdatenverschlüsselung angezeigt. Kopieren Sie diesen Schlüssel, und speichern Sie sie an einem sicheren Ort. **Dieser Schlüssel wird mit dem Dienstregistrierungsschlüssel zum Registrieren zusätzlicher Geräte bei StorSimple Manager-Dienst erforderlich sein.** Verweisen auf [Sicherheit von StorSimple](../articles/storsimple/storsimple-security.md) für Weitere Informationen zu diesem Schlüssel.
    
    ![Registrieren von Geräten StorSimple 7](./media/storsimple-configure-and-register-device-u1/HCS_RegisterYourDevice7_U1-include.png)    
    
    > [!NOTE]
    > Wählen Sie den Text einfach, zum Kopieren des Texts aus dem Fenster der seriellen Konsole. Sie sollten klicken Sie dann in die Zwischenablage oder einem beliebigen Texteditor einfügen können. Verwenden Sie nicht STRG + C, um den Verschlüsselungsschlüssel für Dienstdaten zu kopieren. Verwenden STRG + C bewirkt zum Beenden des Setupassistenten. Daher wird das Kennwort nicht geändert werden, und das Gerät wird auf das Standardkennwort zurückgesetzt.
    > 
    > 
13. Beenden Sie die serielle Konsole.
14. Kehren Sie zum klassischen Azure-Portal zurück, und schließen Sie die folgenden Schritte aus:
    
    1. Doppelklicken Sie auf Ihrem StorSimple-Manager-Dienst den Zugriff auf die **Schnellstart** Seite.
    2. Klicken Sie auf **anzeigen verbundener Geräte**.
    3. Auf der **Geräte** Seite, stellen Sie sicher, dass das Gerät erfolgreich mit dem Dienst verbunden wurde, indem Sie seinen Status. Der Gerätestatus sollte **Online**.
       
        ![Seite "StorSimple-Geräte"](./media/storsimple-configure-and-register-device-u1/HCS_DevicesPageM_U1-include.png) 
       
        Wenn der Gerätestatus **Offline**, warten Sie einige Minuten, bis das Gerät online geschaltet werden kann. 
       
        Wenn das Gerät nach wenigen Minuten immer noch offline ist, müssen Sie sicherstellen, dass Ihre Firewall Netzwerk konfiguriert wurde, wie in beschrieben [netzwerkanforderungen für Ihr StorSimple-Gerät](../articles/storsimple/storsimple-system-requirements.md). 
       
        Stellen Sie sicher, dass Port 9354 für ausgehende Kommunikation geöffnet ist, da diese vom Servicebus für die Kommunikation von StorSimple Manager-Dienst-zu-Gerät verwendet werden.

