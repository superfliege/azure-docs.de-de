## <a name="using-vault-credentials-to-authenticate-with-the-azure-backup-service"></a>Tresor-Anmeldeinformationen verwenden für die Authentifizierung bei Azure Backup-Dienst
Der lokale Server (Windows-Client oder Windows Server oder Data Protection Manager-Server) muss bei einem sicherungstresor authentifiziert werden, bevor sie Daten in Azure sichern kann. Die Authentifizierung erfolgt mithilfe von "Tresor-Anmeldeinformationen". Das Konzept der tresoranmeldeinformationen ist ähnelt dem Konzept einer "veröffentlichungseinstellungen"-Datei die in Azure PowerShell verwendet wird.

### <a name="what-is-the-vault-credential-file"></a>Was ist die Datei mit tresoranmeldeinformationen?
Die Datei mit tresoranmeldeinformationen ist ein Zertifikat vom Portal für jede backup-Tresor generiert. Das Portal hochgeladen klicken Sie dann den öffentlichen Schlüssel, Access Control Service (ACS). Der private Schlüssel des Zertifikats für dem Benutzer als Teil des Workflows zur Verfügung, denen als Eingabe in der Registrierung zustandsautomatenworkflow zugewiesen ist. Dadurch wird den Computer zum Senden von Sicherungsdaten an einem identifizierten-Tresor im Azure Backup-Dienst authentifiziert.

Die tresoranmeldeinformationen wird nur während des Workflows für die Registrierung verwendet. Es ist Aufgabe des Benutzers, um sicherzustellen, dass die Datei mit tresoranmeldeinformationen nicht gefährdet sind. Fällt in die Hände der alle nicht autorisierten Benutzer, kann die Datei mit tresoranmeldeinformationen verwendet werden, auf anderen Computern mit demselben Tresor zu registrieren. Jedoch, wie die zu sichernden Daten verschlüsselt ist, verwenden eine Passphrase an den Kunden gehört, können nicht vorhandene Sicherungsdaten gefährdet werden. Um dieses Problem zu minimieren, werden Tresor-Anmeldeinformationen festgelegt, in 48 Std. abläuft. Sie können den tresoranmeldeinformationen des backup-Tresor oft – herunterladen, aber während des Workflows für die Registrierung gilt nur der neueste Vault-Anmeldedatei.

### <a name="download-the-vault-credential-file"></a>Die Datei mit tresoranmeldeinformationen herunterladen
Die Datei mit tresoranmeldeinformationen ist über einen sicheren Kanal vom Azure-Portal heruntergeladen. Azure Backup-Dienst ist nicht bekannt, des privaten Schlüssels des Zertifikats und des privaten Schlüssels wird nicht im Verwaltungsportal oder den Dienst beibehalten. Verwenden Sie die folgenden Schritte aus, um die Datei mit den tresoranmeldeinformationen auf einem lokalen Computer herunterladen.

1. Melden Sie sich bei der [-Verwaltungsportal](https://manage.windowsazure.com/)
2. Klicken Sie auf **Wiederherstellungsdienste** im linken Navigationsbereich und wählen Sie den sicherungstresor, in dem Sie erstellt haben. Klicken Sie auf das Symbol "Cloud", um die Schnellstart-Ansicht des sicherungstresors zu erhalten.
   
   ![Die Schnellansicht](./media/backup-download-credentials/quickview.png)
3. Klicken Sie auf der Seite "Schnellstart" auf **tresoranmeldeinformationen herunterladen**. Das Portal generiert die Datei mit Tresor-Anmeldeinformationen, zum Download zur Verfügung gestellt werden.
   
   ![Herunterladen](./media/backup-download-credentials/downloadvc.png)
4. Das Portal wird mithilfe einer Kombination aus den tresornamen und dem aktuellen Datum tresoranmeldeinformationen generiert. Klicken Sie auf **speichern** Herunterladen der tresoranmeldeinformationen zum Ordner "Downloads" das lokale Konto, oder wählen speichern, aus speichern, um einen Speicherort für den Tresor-Anmeldeinformationen angeben.

### <a name="note"></a>Hinweis
* Stellen Sie sicher, dass die tresoranmeldeinformationen wird gespeichert, in einen Speicherort an, die von Ihrem Computer zugegriffen werden kann. Wenn sie in einer Freigabe/SMB-Datei gespeichert ist, überprüfen Sie für die Zugriffsberechtigungen.
* Die Datei mit tresoranmeldeinformationen ist nur während des Workflows für die Registrierung verwendet.
* Die Datei mit tresoranmeldeinformationen läuft ab nach 48 Std. und kann über das Portal heruntergeladen werden.
* Finden Sie in der Azure-Sicherung [– häufig gestellte Fragen](../articles/backup/backup-azure-backup-faq.md) für alle Fragen im Workflow.

