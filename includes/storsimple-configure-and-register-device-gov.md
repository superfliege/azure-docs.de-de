<!--author=SharS last changed: 02/22/16-->

### <a name="to-configure-and-register-the-device"></a>Zum Konfigurieren und Registrieren des Geräts
1. Zugriff auf die Windows PowerShell-Schnittstelle auf der seriellen Konsole des StorSimple-Geräts. Finden Sie unter [Verwenden von PuTTY zum Herstellen einer seriellen Konsole des Geräts](#use-putty-to-connect-to-the-device-serial-console) Anweisungen. **Achten Sie darauf, dass Sie die Vorgehensweise genau befolgen, oder Sie werden auf die Konsole zugreifen.**
2. Drücken Sie in der Sitzung, die geöffnet wird EINGABETASTE einmal auf eine Eingabeaufforderung einzublenden. 
3. Sie werden aufgefordert, die Sprache auszuwählen, die Sie für Ihr Gerät festlegen möchten. Geben Sie die Sprache, und drücken Sie dann die EINGABETASTE. 
   
    ![StorSimple konfigurieren und Registrieren des Geräts 1](./media/storsimple-configure-and-register-device-gov/HCS_RegisterYourDevice1-gov-include.png)
4. Wählen Sie in der Menü der seriellen Konsole, das angezeigt wird Option 1 aus, um sich mit Vollzugriff Anmelden aus. 
   
    ![Registrieren von Geräten StorSimple 2](./media/storsimple-configure-and-register-device-gov/HCS_RegisterYourDevice2-gov-include.png)
5. Führen Sie die folgenden Schritte aus, um die mindestens erforderlichen Netzwerkeinstellungen für Ihr Gerät zu konfigurieren.
   
   > [!IMPORTANT]
   > Diese Konfigurationsschritte müssen für den aktiven Controller des Geräts ausgeführt werden soll. Menü der seriellen Konsole gibt den Zustand des Controllers in die bannermeldung an. Wenn Sie sind nicht mit dem aktiven Controller verbunden sind, trennen, und klicken Sie dann mit dem aktiven Controller verbinden.
   > 
   > 
   
   1. Geben Sie an der Eingabeaufforderung Ihr Kennwort ein. Das Standardkennwort für das Gerät ist **Password1**.
   2. Geben Sie den folgenden Befehl ein:
      
        `Invoke-HcsSetupWizard`
   3. Ein Installations-Assistent wird angezeigt, können Sie die Netzwerkeinstellungen für das Gerät zu konfigurieren. Geben Sie die folgenden Informationen an: 
      
      * IP-Adresse für DATA 0-Netzwerkschnittstelle
      * Subnetzmaske
      * Gateway
      * IP-Adresse für den primären DNS-server
      * IP-Adresse für den primären NTP-server
      
      > [!NOTE]
      > Sie müssen möglicherweise warten einige Minuten, bis die Subnetzmaske und die DNS-Einstellungen übernommen werden. 
      > 
      > 
   4. Konfigurieren Sie optional Ihren Webproxyserver ein.
      
      > [!IMPORTANT]
      > Webproxy-Konfiguration ist, jedoch optional Bedenken Sie, dass wenn Sie einen Webproxy verwenden, Sie nur hier konfiguriert werden können. Weitere Informationen finden Sie unter [Konfigurieren des Webproxys für Ihr Gerät](../articles/storsimple/storsimple-configure-web-proxy.md). 
      > 
      > 
6. Drücken Sie STRG + C, um den Setup-Assistenten zu beenden.
7. Installieren Sie die Updates wie folgt:
   
   1. Verwenden Sie das folgende Cmdlet, um IP-Adressen auf beide Controller festzulegen:
      
      `Set-HcsNetInterface -InterfaceAlias Data0 -Controller0IPv4Address <Controller0 IP> -Controller1IPv4Address <Controller1 IP>`
   2. Führen Sie an der Eingabeaufforderung `Get-HcsUpdateAvailability`. Sie sollten über benachrichtigt werden, dass Updates verfügbar sind.
   3. Führen Sie `Start-HcsUpdate` aus. Sie können diesen Befehl auf einem beliebigen Knoten ausführen. Updates auf dem ersten Controller angewendet werden, der Controller wird ein Failover, und klicken Sie dann die Updates auf den anderen Controller angewendet werden.
      
      Sie können den Fortschritt des Updates überwachen, indem ausführen `Get-HcsUpdateStatus`.    
      
      Die folgende Beispielausgabe zeigt das Update wird ausgeführt.
      
      ````
      Controller0>Get-HcsUpdateStatus
      RunInprogress       : True
      LastHotfixTimestamp : 4/13/2015 10:56:13 PM
      LastUpdateTimestamp : 4/13/2015 10:35:25 PM
      Controller0Events   :
      Controller1Events   : 
      ````
      
      Die folgende Beispielausgabe zeigt an, dass das Update abgeschlossen ist.
      
      ````
      Controller1>Get-HcsUpdateStatus
      
      RunInprogress       : False
      LastHotfixTimestamp : 4/13/2015 10:56:13 PM
      LastUpdateTimestamp : 4/13/2015 10:35:25 PM
      Controller0Events   :
      Controller1Events   :
      
      ````
      
      Dauert möglicherweise bis zu 11 Stunden alle Updates, einschließlich der Windows-Updates anwenden.

8. Nachdem alle Updates erfolgreich installiert wurden, führen Sie das folgende Cmdlet aus, um sicherzustellen, dass die Softwareupdates ordnungsgemäß angewendet wurden:
   
     `Get-HcsSystem`
   
    Daraufhin sollte die folgenden Versionen:
   
   * HcsSoftwareVersion: 6.3.9600.17491
   * CisAgentVersion: 1.0.9037.0
   * MdsAgentVersion: 26.0.4696.1433
9. Führen Sie das folgende Cmdlet aus, um sicherzustellen, dass die Firmware-Aktualisierung ordnungsgemäß angewendet wurde:
   
    `Start-HcsFirmwareCheck`.
   
     Der Status der Firmware **"uptodate"**.
10. Führen Sie das folgende Cmdlet aus, um das Gerät mit dem Microsoft Azure Government-Portal zeigen (da es die öffentlichen klassischen Azure-Portal standardmäßig verweist). Dies startet beide Controller neu. Es wird empfohlen, dass Sie zwei PuTTY Sitzungen verwenden, um gleichzeitig eine Verbindung mit beiden Controllern herstellen, sodass Sie erkennen können, wenn jeder Controller neu gestartet wird.
    
     `Set-CloudPlatform -AzureGovt_US`
    
    Sie werden eine bestätigungsmeldung angezeigt. Übernehmen Sie den Standardnamen (**Y**).
11. Führen Sie das folgende Cmdlet aus, um Setup fortzusetzen:
    
     `Invoke-HcsSetupWizard`
    
     ![Assistent zum Fortsetzen von setup](./media/storsimple-configure-and-register-device-gov/HCS_ResumeSetup-gov-include.png)
    
    Wenn Sie Setup fortsetzen, kann der Assistent das Update 1 ausgestattete Version werden (die Version 17469 entspricht). 
12. Akzeptieren Sie die Netzwerkeinstellungen an. Sie sehen eine validierungsmeldung angezeigt, nachdem Sie jede Einstellung akzeptiert.
13. Aus Gründen der Sicherheit das geräteadministratorkennwort läuft nach der ersten Sitzung ab, und Sie es jetzt ändern müssen. Geben Sie bei Aufforderung ein Administratorkennwort für das Gerät. Ein gültiges geräteadministratorkennwort muss 8 bis 15 Zeichen lang sein. Das Kennwort muss drei der folgenden enthalten: Kleinbuchstaben, Großbuchstaben, numerische und Sonderzeichen.
    
    <br/>![Registrieren von Geräten StorSimple 5](./media/storsimple-configure-and-register-device-gov/HCS_RegisterYourDevice5_gov-include.png)
14. Der letzte Schritt im Setup-Assistenten wird Ihr Gerät beim StorSimple Manager-Dienst registriert. Hierzu benötigen Sie den Dienstregistrierungsschlüssel, die Sie in erhalten [Schritt2: Abrufen des dienstregistrierungsschlüssels](#step-2-get-the-service-registration-key). Nachdem Sie den Registrierungsschlüssel angeben, müssen Sie möglicherweise warten, 2 bis 3 Minuten, bevor das Gerät registriert ist.
    
    > [!NOTE]
    > Sie können zu einem beliebigen Zeitpunkt zum Beenden des Setupassistenten STRG + C drücken. Wenn Sie alle Einstellungen des Netzwerks (IP-Adresse für Data 0, Subnetzmaske und Gateway) eingegeben haben, werden Ihre Einträge beibehalten.
    > 
    > 
    
    ![StorSimple-Registrierung ausgeführt](./media/storsimple-configure-and-register-device-gov/HCS_RegistrationProgress-gov-include.png)
15. Nachdem das Gerät registriert ist, wird ein Schlüssel für die Dienstdatenverschlüsselung angezeigt. Kopieren Sie diesen Schlüssel, und speichern Sie sie an einem sicheren Ort. **Dieser Schlüssel wird mit dem Dienstregistrierungsschlüssel zum Registrieren zusätzlicher Geräte bei StorSimple Manager-Dienst erforderlich sein.** Verweisen auf [Sicherheit von StorSimple](../articles/storsimple/storsimple-security.md) für Weitere Informationen zu diesem Schlüssel.
    
    ![Registrieren von Geräten StorSimple 7](./media/storsimple-configure-and-register-device-gov/HCS_RegisterYourDevice7_gov-include.png)    
    
    > [!IMPORTANT]
    > Wählen Sie den Text einfach, zum Kopieren des Texts aus dem Fenster der seriellen Konsole. Sie sollten klicken Sie dann in die Zwischenablage oder einem beliebigen Texteditor einfügen können. 
    > 
    > Verwenden Sie nicht STRG + C, um den Verschlüsselungsschlüssel für Dienstdaten zu kopieren. Verwenden STRG + C bewirkt zum Beenden des Setupassistenten. Daher wird das Kennwort nicht geändert werden, und das Gerät wird auf das Standardkennwort zurückgesetzt.
    > 
    > 
16. Beenden Sie die serielle Konsole.
17. In der Azure-Verwaltungsportal Government zurück, und führen die folgenden Schritte aus:
    
    1. Doppelklicken Sie auf Ihrem StorSimple-Manager-Dienst den Zugriff auf die **Schnellstart** Seite.
    2. Klicken Sie auf **anzeigen verbundener Geräte**.
    3. Auf der **Geräte** Seite, stellen Sie sicher, dass das Gerät erfolgreich mit dem Dienst verbunden wurde, indem Sie seinen Status. Der Gerätestatus sollte **Online**.
       
        ![Seite "StorSimple-Geräte"](./media/storsimple-configure-and-register-device-gov/HCS_DeviceOnline-gov-include.png) 
       
        Wenn der Gerätestatus **Offline**, warten Sie einige Minuten, bis das Gerät online geschaltet werden kann. 
       
        Wenn das Gerät nach wenigen Minuten immer noch offline ist, müssen Sie sicherstellen, dass Ihre Firewall Netzwerk konfiguriert wurde, wie in beschrieben [netzwerkanforderungen für Ihr StorSimple-Gerät](../articles/storsimple/storsimple-system-requirements.md). 
       
        Stellen Sie sicher, dass Port 9354 für ausgehende Kommunikation geöffnet ist, da diese vom Servicebus für die Kommunikation mit dem StorSimple Manager-Dienst-zu-Gerät verwendet werden.

