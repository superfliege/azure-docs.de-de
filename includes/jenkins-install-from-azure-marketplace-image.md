1. Öffnen Sie in Ihrem Browser das [Azure Marketplace-Image für Jenkins](https://azuremarketplace.microsoft.com/marketplace/apps/azure-oss.jenkins?tab=Overview).

1. Wählen Sie **Jetzt herunterladen** aus.

    ![Wählen Sie „Jetzt herunterladen“ aus, um den Installationsprozess für das Jenkins Marketplace-Image zu starten.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-get-it-now.png)

1. Nachdem Sie die Preisdetails und die Nutzungsbedingungen überprüft haben, wählen Sie **Weiter** aus.

    ![Preise und Nutzungsbedingungen zum Jenkins Marketplace-Image.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-pricing-and-terms.png)

1. Wählen Sie **Erstellen** aus, um den Jenkins-Server im Azure-Portal zu konfigurieren. 

    ![Installieren Sie das Jenkins Marketplace-Image.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-create.png)

1. Geben Sie auf der Registerkarte **Grundlagen** die folgenden Werte an:

    - **Name**: Geben Sie `Jenkins` ein.
    - **Benutzer**: Geben Sie den Benutzernamen ein, der für die Anmeldung auf dem virtuellen Computer verwendet wird, auf dem Jenkins ausgeführt wird.
    - **Authentifizierungstyp**: Wählen Sie **Kennwort** aus.
    - **Kennwort**: Geben Sie das Kennwort ein, das für die Anmeldung auf dem virtuellen Computer verwendet wird, auf dem Jenkins ausgeführt wird.
    - **Kennwort bestätigen**: Geben Sie das Kennwort erneut ein, das für die Anmeldung auf dem virtuellen Computer verwendet wird, auf dem Jenkins ausgeführt wird.
    - **Jenkins-Releasetyp**: Wählen Sie **LTS** aus.
    - **Abonnement**: Wählen Sie das Azure-Abonnement aus, für das Jenkins installiert werden soll.
    - **Ressourcengruppe**: Wählen Sie **Neu erstellen** aus, und geben Sie einen Namen für die Ressourcengruppe ein, die als logischer Container für die Sammlung von Ressourcen dient, aus denen Ihre Jenkins-Installation besteht.
    - **Standort**: Wählen Sie **USA, Osten** aus.

    ![Geben Sie Authentifizierungs- und Ressourcengruppeninformationen für Jenkins auf der Registerkarte „Grundlagen“ ein.](./media/jenkins-install-from-azure-marketplace-image/jenkins-configure-basic.png)

1. Wählen Sie **OK** aus, um mit der Registerkarte **Einstellungen** fortzufahren. 

1. Geben Sie auf der Registerkarte **Einstellungen** die folgenden Werte an:

    - **Größe**: Wählen Sie die geeignete Option für die Größenanpassung für den virtuellen Jenkins-Computer aus.
    - **VM-Datenträgertyp**: Geben Sie entweder HDD (Festplattenlaufwerk) oder SSD (Solid-State Drive) als den Speichertyp an, der für den virtuellen Jenkins-Computer zulässig ist.
    - **Öffentliche IP-Adresse**: Der IP-Adressname ist standardmäßig der Jenkins-Name, den Sie auf der vorherigen Seite mit dem Suffix „-IP“ angegeben haben. Sie können die Option auswählen, um diesen Standardwert zu ändern.
    - **Domänennamenbezeichnung**: Geben Sie den Wert für die vollqualifizierte URL zum virtuellen Jenkins-Computer an.

    ![Geben Sie die Einstellungen für virtuelle Computer für Jenkins auf der Registerkarte „Einstellungen“ ein.](./media/jenkins-install-from-azure-marketplace-image/jenkins-configure-settings.png)

1. Wählen Sie **OK** aus, um mit der Registerkarte **Zusammenfassung** fortzufahren.

1. Wenn die Registerkarte **Zusammenfassung** angezeigt wird, werden die eingegebenen Informationen überprüft. Wenn die Meldung **Überprüfung erfolgreich** angezeigt wird, wählen Sie **OK** aus. 

    ![Auf der Registerkarte „Zusammenfassung“ werden die ausgewählten Optionen angezeigt und überprüft.](./media/jenkins-install-from-azure-marketplace-image/jenkins-configure-summary.png)

1. Wenn die Registerkarte **Erstellen** angezeigt wird, wählen Sie **Erstellen** aus, um den virtuellen Computer für Jenkins zu erstellen. Wenn Ihr Server bereit ist, wird im Azure-Portal eine entsprechende Benachrichtigung angezeigt.

    ![Bereitschaftsbenachrichtigung für Jenkins](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-notification.png)