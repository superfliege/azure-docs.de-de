### <a name="prepare-for-a-push-installation-on-a-windows-computer"></a>Vorbereiten einer Push-Installations auf einem Windows-computer

1. Stellen Sie sicher, dass die Netzwerkverbindung zwischen dem Windows-Computer und dem Prozessserver besteht.
2. Erstellen Sie ein Konto an, die der Prozessserver verwenden können, auf den Computer zugreifen. Das Konto muss über Administratorrechte (lokales oder Domänenbenutzerkonto) verfügen. (Verwenden Sie dieses Konto nur für die Clientpushinstallation und für Agent-Updates.)

   > [!NOTE]
   > Wenn Sie nicht über ein Domänenkonto verwenden, deaktivieren Sie die RAS-Benutzer-Steuerelement auf dem lokalen Computer. Deaktivieren die RAS-Benutzer-Steuerelement, unter dem Registrierungsschlüssel HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System einen neuen DWORD-Wert hinzufügen: **LocalAccountTokenFilterPolicy**. Legen Sie den Wert auf **1**. Führen Sie dazu an der Eingabeaufforderung den folgenden Befehl ein:  
   `REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`
   >
   >
2. Wählen Sie im Windows-Firewall auf dem Computer, die Sie schützen möchten, **eine app oder Feature durch die Firewall zulassen**. Aktivieren Sie **Datei- und Druckerfreigabe** und **Windows-Verwaltungsinstrumentation (WMI)**. Bei Computern, die zu einer Domäne gehören, können Sie die Firewall-Einstellungen konfigurieren, mit ein Gruppenrichtlinienobjekt (GPO).

   ![Firewalleinstellungen](./media/site-recovery-prepare-push-install-mob-svc-win/mobility1.png)

3. Fügen Sie das Konto, das Sie in CSPSConfigtool erstellt.
    1.  Melden Sie sich an Ihren Konfigurationsserver.
    2.  Open **cspsconfigtool.exe**. (Es ist als eine Verknüpfung auf dem Desktop und im Ordner "%ProgramData%\home\svsystems\bin" verfügbar.)
    3.  Auf der **Manage Accounts** Registerkarte **Konto hinzufügen**.
    4.  Fügen Sie das Konto, das Sie erstellt haben.
    5.  Geben Sie die Anmeldeinformationen, die Sie verwenden, wenn Sie die Replikation für einen Computer aktivieren.
