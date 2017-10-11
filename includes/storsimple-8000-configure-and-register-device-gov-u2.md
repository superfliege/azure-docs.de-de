<!--author=SharS last changed: 06/22/2016-->

### <a name="to-configure-and-register-the-device"></a>Zum Konfigurieren und Registrieren des Geräts
1. Zugriff auf die Windows PowerShell-Schnittstelle auf der seriellen Konsole des StorSimple-Geräts. Finden Sie unter [Verwenden von PuTTY zum Herstellen einer seriellen Konsole des Geräts](../articles/storsimple/storsimple-8000-deployment-walkthrough-gov-u2.md#use-putty-to-connect-to-the-device-serial-console) Anweisungen. **Achten Sie darauf, dass Sie die Vorgehensweise genau befolgen, oder Sie werden auf die Konsole zugreifen.**
2. Drücken Sie im geöffneten Sitzungsfenster einrichten, **EINGABETASTE** einmal an eine Eingabeaufforderung einzublenden.
3. Sie werden aufgefordert, die Sprache auszuwählen, die Sie für Ihr Gerät festlegen möchten. Geben Sie die Sprache aus, und drücken Sie dann die **EINGABETASTE**.
   
    ![StorSimple konfigurieren und Registrieren des Geräts 1](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegisterYourDevice1-gov-include.png)
4. Wählen Sie in der Menü der seriellen Konsole, das angezeigt wird Option 1 aus, um sich mit Vollzugriff Anmelden aus.
   
    ![Registrieren von Geräten StorSimple 2](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegisterYourDevice2-gov-include.png)
5. Führen Sie die folgenden Schritte aus, um die mindestens erforderlichen Netzwerkeinstellungen für Ihr Gerät zu konfigurieren.
   
   > [!IMPORTANT]
   > Diese Konfigurationsschritte müssen für den aktiven Controller des Geräts ausgeführt werden soll. Menü der seriellen Konsole gibt den Zustand des Controllers in die bannermeldung an. Wenn Sie sind nicht mit dem aktiven Controller verbunden sind, trennen, und klicken Sie dann mit dem aktiven Controller verbinden.
   
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
    
   4. Konfigurieren Sie optional Ihren Webproxyserver ein.
      
      > [!IMPORTANT]
      > Webproxy-Konfiguration ist, jedoch optional Bedenken Sie, dass wenn Sie einen Webproxy verwenden, Sie nur hier konfiguriert werden können. Weitere Informationen finden Sie unter [Konfigurieren des Webproxys für Ihr Gerät](../articles/storsimple/storsimple-configure-web-proxy.md).
     
6. Drücken Sie STRG + C, um den Setup-Assistenten zu beenden.
8. Führen Sie das folgende Cmdlet aus, um das Gerät mit dem Microsoft Azure Government-Portal zeigen (da es die öffentlichen klassischen Azure-Portal standardmäßig verweist). Dies startet beide Controller neu. Es wird empfohlen, dass Sie zwei PuTTY Sitzungen verwenden, um gleichzeitig eine Verbindung mit beiden Controllern herstellen, sodass Sie erkennen können, wenn jeder Controller neu gestartet wird.
   
    `Set-CloudPlatform -AzureGovt_US`
   
   Sie werden eine bestätigungsmeldung angezeigt. Übernehmen Sie den Standardnamen (**Y**).
9. Führen Sie das folgende Cmdlet aus, um Setup fortzusetzen:
   
    `Invoke-HcsSetupWizard`
   
    ![Assistent zum Fortsetzen von setup](./media/storsimple-configure-and-register-device-gov-u2/HCS_ResumeSetup-gov-include.png)
   
10. Akzeptieren Sie die Netzwerkeinstellungen an. Sie sehen eine validierungsmeldung angezeigt, nachdem Sie jede Einstellung akzeptiert.
11. Aus Gründen der Sicherheit das geräteadministratorkennwort läuft nach der ersten Sitzung ab, und Sie es jetzt ändern müssen. Geben Sie bei Aufforderung ein Administratorkennwort für das Gerät. Ein gültiges geräteadministratorkennwort muss 8 bis 15 Zeichen lang sein. Das Kennwort muss drei der folgenden enthalten: Kleinbuchstaben, Großbuchstaben, numerische und Sonderzeichen.
    
    <br/>![Registrieren von Geräten StorSimple 5](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegisterYourDevice5_gov-include.png)
12. Der letzte Schritt im Setup-Assistenten wird Ihr Gerät beim StorSimple-Geräte-Manager registriert. Hierzu benötigen Sie den Dienstregistrierungsschlüssel, die Sie in erhalten [Schritt2: Abrufen des dienstregistrierungsschlüssels](../articles/storsimple/storsimple-8000-deployment-walkthrough-gov-u2.md#step-2-get-the-service-registration-key). Nachdem Sie den Registrierungsschlüssel angeben, müssen Sie möglicherweise warten, 2 bis 3 Minuten, bevor das Gerät registriert ist.
    
    > [!NOTE]
    > Sie können zu einem beliebigen Zeitpunkt zum Beenden des Setupassistenten STRG + C drücken. Wenn Sie alle Einstellungen des Netzwerks (IP-Adresse für Data 0, Subnetzmaske und Gateway) eingegeben haben, werden Ihre Einträge beibehalten.
    
    ![StorSimple-Registrierung ausgeführt](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegistrationProgress-gov-include.png)
13. Nachdem das Gerät registriert ist, wird ein Schlüssel für die Dienstdatenverschlüsselung angezeigt. Kopieren Sie diesen Schlüssel, und speichern Sie sie an einem sicheren Ort. **Dieser Schlüssel ist erforderlich, mit dem Dienstregistrierungsschlüssel zum Registrieren zusätzlicher Geräte mit dem StorSimple-Geräte-Manager-Dienst.** Verweisen auf [Sicherheit von StorSimple](../articles/storsimple/storsimple-8000-security.md) für Weitere Informationen zu diesem Schlüssel.
    
    ![Registrieren von Geräten StorSimple 7](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegisterYourDevice7_gov-include.png)
    > [!IMPORTANT]
    > Wählen Sie den Text einfach, zum Kopieren des Texts aus dem Fenster der seriellen Konsole. Sie sollten klicken Sie dann in die Zwischenablage oder einem beliebigen Texteditor einfügen können.
    > 
    > Verwenden Sie keine **STRG + C** So kopieren Sie den Verschlüsselungsschlüssel für Dienstdaten. Mit **STRG + C** bewirkt, dass Sie den Setupassistenten zu beenden. Daher wird das Kennwort nicht geändert werden, und das Gerät wird auf das Standardkennwort zurückgesetzt.
    
14. Beenden Sie die serielle Konsole.
15. In der Azure-Verwaltungsportal Government zurück, und führen die folgenden Schritte aus:
    
    1. Wechseln Sie zu Ihrer StorSimple-Geräte-Manager-Dienst.
    2. Klicken Sie auf **Geräte**. Identifizieren Sie aus der Liste der Geräte die Geräte an, die Sie Ddeploying sind. Stellen Sie sicher, dass das Gerät erfolgreich mit dem Dienst verbunden wurde, indem Sie seinen Status. Der Gerätestatus sollte **Online**.
            
        Wenn der Gerätestatus **Offline**, warten Sie einige Minuten, bis das Gerät online geschaltet werden kann.
       
        Wenn das Gerät nach wenigen Minuten immer noch offline ist, müssen Sie sicherstellen, dass Ihre Firewall Netzwerk konfiguriert wurde, wie in beschrieben [netzwerkanforderungen für Ihr StorSimple-Gerät](../articles/storsimple/storsimple-8000-system-requirements.md).
       
        Stellen Sie sicher, dass Port 9354 für ausgehende Kommunikation geöffnet ist, da diese vom Servicebus für die StorSimple-Geräte-Manager-Dienst-zu-Gerät-Kommunikation verwendet werden.

