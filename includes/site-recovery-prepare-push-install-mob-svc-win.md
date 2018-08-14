### <a name="prepare-for-a-push-installation-on-a-windows-computer"></a>Vorbereiten einer Pushinstallation auf einem Windows-Computer

1. Stellen Sie sicher, dass zwischen dem Windows-Computer und dem Prozessserver eine Netzwerkkonnektivität besteht.
1. Erstellen Sie ein Konto, das vom Prozessserver zum Zugreifen auf den Computer verwendet werden kann. Das Konto muss über Administratorrechte verfügen (entweder lokal oder für die Domäne). Verwenden Sie dieses Konto nur für die Pushinstallation und für Agent-Updates.

   > [!NOTE]
   > Deaktivieren Sie die Remote-Benutzerzugriffssteuerung auf dem lokalen Computer, wenn Sie kein Domänenkonto verwenden. Fügen Sie zum Deaktivieren der Remote-Benutzerzugriffssteuerung unter dem Registrierungsschlüssel „HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System“ einen neuen DWORD-Eintrag hinzu: **LocalAccountTokenFilterPolicy**. Legen Sie den Wert auf **1** fest. Führen Sie den folgenden Befehl aus, wenn Sie dafür eine Eingabeaufforderung verwenden möchten:  
   `REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`
   >
   >
1. Wählen Sie in der Windows-Firewall des Computers, den Sie schützen möchten, die Option **Eine App oder ein Feature durch die Windows-Firewall zulassen**. Aktivieren Sie **Datei- und Druckerfreigabe** und **Windows-Verwaltungsinstrumentation (WMI)**. Für Computer, die zu einer Domäne gehören, können Sie die Firewalleinstellungen konfigurieren, indem Sie ein Gruppenrichtlinienobjekt (Group Policy Object, GPO) verwenden.

   ![Firewalleinstellungen](./media/site-recovery-prepare-push-install-mob-svc-win/mobility1.png)

1. Fügen Sie das Konto hinzu, das Sie im CSPSConfigtool erstellt haben. Folgen Sie diesen Schritten:

    a. Melden Sie sich am Konfigurationsserver an.

    b. Öffnen Sie die Datei **cspsconfigtool.exe**. Sie ist als Verknüpfung auf dem Desktop und im Ordner „%ProgramData%\home\svsystems\bin“ verfügbar.

    c. Wählen Sie auf der Registerkarte **Konten verwalten** die Option **Konto hinzufügen**.

    d. Fügen Sie das von Ihnen erstellte Konto hinzu.

    e. Geben Sie die Anmeldeinformationen ein, die Sie verwenden, wenn Sie die Replikation für einen Computer aktivieren.
