1. Navigieren Sie in Ihrem Browser zu Ihrem virtuellen Jenkins-Computer. Da das Jenkins-Dashboard über ungesichertes HTTP nicht zugänglich ist, wird eine Meldung angezeigt, die darauf hinweist, dass Sie mithilfe von SSH einen Tunnel zum virtuellen Computer erstellen müssen.

    ![Jenkins stellt Informationen zur Verfügung, die erläutern, wie Sie mit SSH eine Verbindung mit dem virtuellen Jenkins-Computer herstellen können.](./media/jenkins-connect-to-jenkins-server-running-on-azure/jenkins-ssh-instructions.png)

1. Öffnen Sie ein Bash- oder Terminalfenster mit Zugriff auf SSH.

1. Geben Sie den folgenden Befehl `ssh` ein, und ersetzen Sie die Platzhalter **&lt;Benutzername>** und **&lt;Domäne>** durch die Werte, die Sie beim Erstellen des virtuellen Jenkins-Computers angegebenen haben:

    ```bash
    ssh -L 127.0.0.1:8080:localhost:8080 <username>@<domain>.eastus.cloudapp.azure.com
    ```

1. Befolgen Sie die `ssh`-Aufforderungen, um sich am virtuellen Jenkins-Computer anzumelden.

1. Geben Sie den folgenden Befehl ein, um das anfängliche Kennwort zum Entsperren von Jenkins abzurufen.

    ```bash
    sudo cat /var/lib/jenkins/secrets/initialAdminPassword
    ```

1. Kopieren Sie das Kennwort, das im Bash- oder Terminalfenster angezeigt wird, in die Zwischenablage.

1. Navigieren Sie in Ihrem Browser zu `http://localhost:8080`.

1. Fügen Sie das zuvor abgerufene Kennwort in das Feld **Administratorkennwort** ein, und wählen Sie **Weiter** aus.

    ![Jenkins speichert ein anfängliches Kennwort, mit dem Sie den virtuellen Jenkins-Computer entsperren müssen, wenn Sie das erste Mal eine Verbindung herstellen.](./media/jenkins-connect-to-jenkins-server-running-on-azure/jenkins-unlock.png)

1. Wählen Sie **Vorgeschlagene Plug-Ins installieren** aus.

    ![Jenkins ermöglicht es Ihnen, eine Sammlung von vorgeschlagenen Plug-Ins bei der ersten Eingabe zu installieren.](./media/jenkins-connect-to-jenkins-server-running-on-azure/jenkins-customize.png)

1. Erstellen Sie auf der Seite für die **Erstellung des ersten Administratorbenutzers** den administrativen Benutzer und das Kennwort für das Jenkins-Dashboard, und wählen Sie **Speichern und Beenden** aus.

1. Wählen Sie auf der Seite **Jenkins is ready!** (Jenkins ist bereit) die Option **Start using Jenkins** (Jenkins verwenden) aus.

    ![Nachdem Jenkins installiert und für den Zugriff konfiguriert wurde, können Sie damit beginnen, Aufträge zu erstellen.](./media/jenkins-connect-to-jenkins-server-running-on-azure/jenkins-ready.png)