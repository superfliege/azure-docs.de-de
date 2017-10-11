## <a name="download-install-and-register-the-azure-backup-agent"></a>Herunterladen Sie, installieren Sie und registrieren Sie den Azure Backup-agent
Nach dem Erstellen der Azure Backup-Tresor, muss ein Agent installiert sein, auf jedem Ihrer Windows-Computer (Windows Server, Windows-Client, System Center Data Protection Manager-Server oder Azure Backup Server-Computer), das Sichern von Daten und Anwendungen in Azure ermöglicht.

1. Melden Sie sich bei der [-Verwaltungsportal](https://manage.windowsazure.com/)
2. Klicken Sie auf **Wiederherstellungsdienste**, wählen Sie die backup-Tresor, die Sie mit einem Server registrieren möchten. Die Seite "Schnellstart" für backup-Tresor wird angezeigt.
   
    ![Schnellstart](./media/backup-install-agent/quickstart.png)
3. Klicken Sie auf der Seite "Schnellstart", auf die **für Windows Server oder System Center Data Protection Manager oder Windows Client** unter option **-Agent herunterladen**. Klicken Sie auf **speichern** , um auf den lokalen Computer zu kopieren.
   
    ![Speichern von Agents](./media/backup-install-agent/agent.png)
4. Sobald der Agent installiert ist, einen Doppelklick MARSAgentInstaller.exe um die Installation des Azure Backup-Agents zu starten. Wählen Sie den Installationsordner und Ablageordner für den Agent erforderlich. Der Speicherort des Schemacaches angegeben muss freien Speicherplatz verfügen, mindestens 5 % der Sicherungsdaten.
5. Wenn Sie einen Proxyserver verwenden, um mit dem Internet Verbinden der **Proxykonfiguration** Bildschirm, geben Sie die proxyserverdetails. Wenn Sie einen authentifizierten Proxy verwenden, geben Sie die Benutzerdetails für Name und Kennwort auf diesem Bildschirm.
6. Azure Backup-Agent installiert .NET Framework 4.5 und Windows PowerShell (falls es nicht bereits vorliegt) um die Installation abzuschließen.
7. Nachdem der Agent installiert ist, klicken Sie auf die **mit Registrierung Fortfahren** um mit dem Workflow den Vorgang fortzusetzen.
   
   ![Registrieren](./media/backup-install-agent/register.png)
8. Navigieren Sie zu, und wählen Sie die Vault-Anmeldedatei die zuvor heruntergeladen wurde, auf dem Bildschirm Tresor-Anmeldeinformationen.
   
    ![Tresor-Anmeldeinformationen](./media/backup-install-agent/vc.png)
   
    Die Datei mit tresoranmeldeinformationen gilt nur für 48 Stunden (nach dem Herunterladen vom Portal wird). Wenn Sie alle Fehler in diesem Bildschirm (z. B. "provided Vault-Anmeldedatei ist abgelaufen"), Anmeldung beim Azure-Portal auftritt und die Datei mit tresoranmeldeinformationen erneut herunterzuladen.
   
    Stellen Sie sicher, dass die Datei mit tresoranmeldeinformationen an einem Ort zugreifen können, von der setupanwendung verfügbar ist. Treten Sie Zugriff auf verwandte Fehlermeldungen, kopieren Sie die Datei mit tresoranmeldeinformationen in ein temporäres Verzeichnis auf diesem Computer, und wiederholen Sie den Vorgang.
   
    Treten einen ungültige Tresor-Anmeldeinformationen-Fehler (z. B. "Ungültige tresoranmeldeinformationen angegeben") die Datei ist entweder beschädigt oder enthält nicht die aktuellen Anmeldeinformationen verknüpft haben mit der Recovery-Dienst. Wiederholen Sie den Vorgang nach dem Herunterladen einer neuen Vault-Anmeldedatei vom Portal aus. Dieser Fehler wird in der Regel angezeigt, wenn der Benutzer klickt der **tresoranmeldeinformationen herunterladen** -Option in der Azure-Portal in schneller Folge. In diesem Fall ist nur die zweite Anmeldeinformationsdatei gültig.
9. In der **verschlüsselungseinstellung** Bildschirm können Sie eine Passphrase zu generieren oder geben Sie eine Passphrase (mindestens 16 Zeichen). Denken Sie daran, um die Passphrase an einem sicheren Ort zu speichern.
   
    ![Verschlüsselung](./media/backup-install-agent/encryption.png)
   
   > [!WARNING]
   > Wenn die Passphrase verloren geht oder vergessen; Microsoft kann nicht helfen bei der Wiederherstellung der zu sichernden Daten. Der Benutzer besitzt die verschlüsselungspassphrase, und Microsoft keinen Einblick in die Passphrase, die vom Endbenutzer verwendet. Speichern Sie die Datei an einem sicheren Ort, wie bei einem Wiederherstellungsvorgang erforderlich ist.
   > 
   > 
10. Nach dem Klicken auf die **Fertig stellen** Schaltfläche, der Computer wurde erfolgreich in den Tresor registriert ist und Sie sind jetzt bereit, um Sichern in Microsoft Azure zu starten.
11. Bei Verwendung von eigenständigen Microsoft Azure Backup kann, ändern Sie die Einstellungen, die während des Workflows für die Registrierung angegeben werden, indem Sie auf die **Change Properties** -Option in der Azure Backup-Mmc-Snap-in.
    
    ![Ändern von Eigenschaften](./media/backup-install-agent/change.png)
    
    Auch wenn Sie Data Protection Manager zu verwenden, können Sie die Einstellungen ändern während der Registrierung Workflow angegeben werden, indem Sie auf die **konfigurieren** Option dazu **Online** unter der **Management** Registerkarte.
    
    ![Konfigurieren von Azure Backup](./media/backup-install-agent/configure.png)

