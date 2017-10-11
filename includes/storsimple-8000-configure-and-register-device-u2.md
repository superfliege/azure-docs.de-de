<!--author=alkohli last changed: 01/18/2017-->


#### <a name="to-configure-and-register-the-device"></a>Zum Konfigurieren und Registrieren des Geräts

1. Zugriff auf die Windows PowerShell-Schnittstelle auf der seriellen Konsole des StorSimple-Geräts. Finden Sie unter [Verwenden von PuTTY zum Herstellen einer seriellen Konsole des Geräts](#use-putty-to-connect-to-the-device-serial-console) Anweisungen. **Achten Sie darauf, dass Sie die Vorgehensweise genau befolgen, oder Sie werden auf die Konsole zugreifen.**

2. Drücken Sie im geöffneten Sitzungsfenster einrichten, **EINGABETASTE** einmal an eine Eingabeaufforderung einzublenden.

3. Sie werden aufgefordert, die Sprache auszuwählen, die Sie für Ihr Gerät festlegen möchten. Geben Sie die Sprache aus, und drücken Sie dann die **EINGABETASTE**.

4. In der Menü der seriellen Konsole, die angezeigt wird, wählen Sie Option 1 aus, um **Anmelden mit Vollzugriff**.
     Führen Sie die Schritte 5 bis 12, die mindestens erforderlichen Netzwerkeinstellungen für Ihr Gerät zu konfigurieren. **Diese Konfigurationsschritte müssen für den aktiven Controller des Geräts ausgeführt werden soll.** Menü der seriellen Konsole gibt den Zustand des Controllers in die bannermeldung an. Wenn Sie nicht mit dem aktiven Controller verbunden sind, trennen, und klicken Sie dann mit dem aktiven Controller verbinden.

5. Geben Sie an der Eingabeaufforderung Ihr Kennwort ein. Das Standardkennwort für das Gerät ist **Password1**.

6. Geben Sie den folgenden Befehl: `Invoke-HcsSetupWizard`.

7. Ein Installations-Assistent wird angezeigt, können Sie die Netzwerkeinstellungen für das Gerät zu konfigurieren. Geben Sie die folgenden Informationen:
   
   * IP-Adresse für die DATA 0-Netzwerkschnittstelle
   * Subnetzmaske
   * Gateway
   * IP-Adresse für den primären DNS-server

   Beispiel für eine Ausgabe wird nachfolgend dargestellt.

    ```
        ---------------------------------------------------------------
        Microsoft Azure StorSimple Appliance Model 8100
        Name: 8100-SHX0991003G44MT
        Software Version: 6.3.9600.17759
        Copyright (C) 2014 Microsoft Corporation. All rights reserved.
        You are connected to Controller0 - Active
        ---------------------------------------------------------------

        Your device needs to be registered with the Microsoft Azure StorSimple Manager service. Please run 'Invoke-HcsSetupWizard' to set up your device.

        Controller0>Invoke-HcsSetupWizard

        Which IP address family would you like to configure on interface Data0?
        [4] IPv4 [6] IPv6 [B] Both (Default is "4"): 4

        Data0 IPv4 address:10.111.111.00
        Data0 IPv4 subnet: 255.255.252.0
        Data0 IPv4 gateway: 10.111.111.11

        IPv4 primary DNS server [10.222.118.154]:10.222.222.111
    ```

    <br>
    In der obigen Beispielausgabe sehen Sie sich, dass das System nach jedem Schritt im Prozess Validierung Netzwerkeinstellungen ausführt.

     > [!NOTE]
     > Sie müssen möglicherweise warten einige Minuten, bis die Subnetzmaske und die DNS-Einstellungen angewendet werden. Wenn Sie eine Fehlermeldung "Überprüfen die Netzwerkverbindung mit Data 0" erhalten, überprüfen Sie die physische Netzwerkverbindung für die Netzwerkschnittstelle DATA 0 Ihres aktiven Controllers.

8. (Optional) Ihren Webproxyserver zu konfigurieren. Obwohl die webproxykonfiguration ist optional und **Beachten Sie, dass wenn Sie einen Webproxy verwenden, Sie nur hier konfiguriert werden können**. Weitere Informationen finden Sie unter [Konfigurieren des Webproxys für Ihr Gerät](../articles/storsimple/storsimple-8000-configure-web-proxy.md).
9. Konfigurieren Sie einen primären NTP-Server für Ihr Gerät. NTP-Server sind erforderlich, wie die Zeit, damit er mit Ihrem clouddienstanbieter authentifizieren kann Ihr Gerät synchronisiert werden muss. Stellen Sie sicher, dass Ihr Netzwerk NTP-Datenverkehr vom Rechenzentrum mit dem Internet zulässt. Wenn dies nicht möglich ist, geben Sie einen internen NTP-Server.

    Nachfolgend finden Sie eine Beispielausgabe.

    ```
        Would you like to configure a web proxy?
        [Y] Yes [N] No (Default is "N"):N

        Primary NTP server [time.windows.com]:time.windows.com

    ```

10. Aus Gründen der Sicherheit das geräteadministratorkennwort läuft nach der ersten Sitzung ab, und Sie es jetzt ändern müssen. Geben Sie bei Aufforderung ein Administratorkennwort für das Gerät. Ein gültiges geräteadministratorkennwort muss 8 bis 15 Zeichen lang sein. Das Kennwort muss drei der folgenden enthalten: Kleinbuchstaben, Großbuchstaben, numerische und Sonderzeichen.

    ```
        The device administrator password must be between 8 and 15 characters. The password must contain a combination of uppercase letters, lowercase letters, numbers and special characters.
        Administrator Password:********
        Confirm Administrator Password:********
    ```
11. Der letzte Schritt im Setup-Assistenten wird Ihr Gerät beim StorSimple-Geräte-Manager registriert. Hierzu benötigen Sie den Dienstregistrierungsschlüssel, den Sie in Schritt 2 erhalten haben. Nachdem Sie den Registrierungsschlüssel angeben, müssen Sie möglicherweise warten, 2 bis 3 Minuten, bevor das Gerät registriert ist.
    
    > [!NOTE]
    > Sie können zu einem beliebigen Zeitpunkt zum Beenden des Setupassistenten STRG + C drücken. Wenn Sie alle Einstellungen des Netzwerks (IP-Adresse für Data 0, Subnetzmaske und Gateway) eingegeben haben, werden Ihre Einträge beibehalten.
    
    Nachfolgend finden Sie eine Beispielausgabe.

    ```
        The service registration key is available in the StorSimple Manager service.
        Enter service registration key:**************************************
        Device registration is in progress. Please wait.

    ```

12. Nachdem das Gerät registriert ist, wird ein Schlüssel für die Dienstdatenverschlüsselung angezeigt. Kopieren Sie diesen Schlüssel, und speichern Sie sie an einem sicheren Ort. **Dieser Schlüssel wird mit dem Dienstregistrierungsschlüssel zum Registrieren zusätzlicher Geräte mit dem StorSimple-Geräte-Manager-Dienst erforderlich sein.** Verweisen auf [Sicherheit von StorSimple](../articles/storsimple/storsimple-security.md) für Weitere Informationen zu diesem Schlüssel.
    
    ![Registrieren von Geräten StorSimple 7](./media/storsimple-8000-configure-and-register-device-u2/step3pssetup1.png)
    
    > [!NOTE]
    > Wählen Sie den Text einfach, zum Kopieren des Texts aus dem Fenster der seriellen Konsole. Sie sollten klicken Sie dann in die Zwischenablage oder einem beliebigen Texteditor einfügen können. Verwenden Sie nicht STRG + C, um den Verschlüsselungsschlüssel für Dienstdaten zu kopieren. Verwenden STRG + C bewirkt zum Beenden des Setupassistenten. Daher wird das Kennwort nicht geändert werden, und das Gerät wird auf das Standardkennwort zurückgesetzt.
    
13. Beenden Sie die serielle Konsole.
14. In der Azure-Portal zurück, und führen die folgenden Schritte aus:
    
    1. Wechseln Sie zu Ihrer StorSimple-Geräte-Manager-Dienst.
    2. Klicken Sie auf **Geräte**.
    3. In der tabellarischen Auflistung von Geräten, stellen Sie sicher, dass das Gerät erfolgreich mit dem Dienst verbunden wurde, indem Sie seinen Status. Der Gerätestatus sollte **bereit, richten Sie**.
       
        ![Seite "StorSimple-Geräte"](./media/storsimple-8000-configure-and-register-device-u2/step3pssetup2.png)
       
        Sie müssen möglicherweise warten einige Minuten, für den Gerätestatus so ändern Sie in **bereit, richten Sie**.
       
        Wenn das Gerät wird nicht in dieser Liste angezeigt wird, dann müssen Sie sicherstellen, dass Ihre Firewall Netzwerk konfiguriert wurde, wie in beschrieben [netzwerkanforderungen für Ihr StorSimple-Gerät](../articles/storsimple/storsimple-8000-system-requirements.md). Stellen Sie sicher, dass Port 9354 für ausgehende Kommunikation geöffnet ist, da diese vom Servicebus für die StorSimple-Geräte-Manager-Dienst-zu-Gerät-Kommunikation verwendet werden.

