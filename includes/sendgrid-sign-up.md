Azure-Kunden können 25.000 kostenlose e-Mails monatlich entsperren. Diese 25.000 freien monatlichen e-Mails erhalten Sie Zugriff auf Erweiterter Berichterstattung und Analyse und [alle APIs] [ all APIs] (Web, SMTP, Ereignis, Analyse und mehr). Informationen über zusätzliche Dienste von SendGrid finden Sie auf der [SendGrid-Lösungen] [ SendGrid Solutions] Seite.

### <a name="to-sign-up-for-a-sendgrid-account"></a>Für eine SendGrid-Konto registrieren
1. Melden Sie sich auf die [Azure-Verwaltungsportal][Azure Management Portal].
2. Klicken Sie im Menü auf der linken Seite auf **neu**.

    ![Befehl-Leiste neu][command-bar-new]
3. Klicken Sie auf **Add-ons** und dann **SendGrid-e-Mail-Übermittlung**.

    ![Sendgrid-Speicher][sendgrid-store]
4. Füllen Sie das Formular für die Registrierung, und wählen Sie **erstellen**.

    ![Sendgrid-erstellen][sendgrid-create]
5. Geben Sie einen **Namen** zum Identifizieren Ihrer SendGrid-Diensts in Ihrem Azure-Einstellungen. Namen müssen zwischen 1 und 100 Zeichen lang sein und nur alphanumerische Zeichen, Bindestriche, Punkte und Unterstriche enthalten. Der Name muss in der Liste der abonnierte Speicherelemente in Azure eindeutig sein.
6. Eingabe und Bestätigung Ihrer **Kennwort**.
7. Wählen Sie Ihre **Abonnement**.
8. Erstellen Sie ein neues **Ressourcengruppe** oder eine vorhandene verwenden.
9. In der **Tarif** Abschnitt wählen Sie die SendGrid-Plan, die Sie registrieren möchten.

    ![Sendgrid-Preise][sendgrid-pricing]
10. Geben Sie einen **Promotioncode** sofern vorhanden.
11. Geben Sie Ihre **Kontaktinformationen**.
12. Lesen und akzeptieren Sie die **rechtliche Bedingungen**.
13. Nach dem Bestätigen des Kaufs sehen Sie eine **wurde erfolgreich bereitgestellt** Popupfenster angezeigt, und Sie sehen Ihre aufgeführte Konto die **alle Ressourcen** Abschnitt.

    ![alle Ressourcen][all-resources]

    Nachdem Sie Ihren Einkauf abgeschlossen haben, und auf die **verwalten** Schaltfläche, um die e-Mail-Überprüfung initiieren, Sie erhalten eine e-Mail von SendGrid werden Sie aufgefordert, Ihr Konto zu überprüfen. Wenn Sie nicht diese e-Mail empfangen oder Probleme mit Ihrem Konto haben, finden Sie unter diese häufig gestellten Fragen.

    ![Verwalten][manage]

    **Sie können nur bis zu 100-e-Mails/Tag senden, bis Sie Ihr Konto überprüft haben.**

    Um Ihr Abonnement zu ändern oder die SendGrid erhalten Sie die Einstellungen angezeigt wird, klicken Sie auf den Namen Ihres SendGrid-Diensts, um die SendGrid-Marketplace-Dashboard zu öffnen.

    ![Einstellungen][settings]

    Um eine e-Mail mit SendGrid senden zu können, müssen Sie Ihren API-Schlüssel angeben.

### <a name="to-find-your-sendgrid-api-key"></a>Ihren SendGrid-API-Schlüssel gefunden.
1. Klicken Sie auf **Verwalten**.

    ![Verwalten][manage]
2. Wählen Sie in Ihrem SendGrid-Dashboard **Einstellungen** und dann **API-Schlüssel** im Menü auf der linken Seite.

    ![API-Schlüssel][api-keys]

3. Klicken Sie auf die **API-Schlüssel erstellen** Dropdownliste, und wählen Sie **allgemeinen API-Schlüssel**.

    ![Allgemein-api-Schlüssel][general-api-key]
4. Geben Sie mindestens die **Name dieses Schlüssels** und Bereitstellen des vollen **Senden von E-Mails** , und wählen Sie **speichern**.

    ![Zugriff][access]
5. Ihre API wird an diesem Punkt nur einmal angezeigt. Sie unbedingt sicher zu speichern.

### <a name="to-find-your-sendgrid-credentials"></a>Mithilfe der Softwareoption Ihrer SendGrid-Anmeldeinformationen
1. Klicken Sie auf das Symbol "Taste" zu Ihrem **Benutzername**.

    ![key][key]
2. Das Kennwort ist diejenige, die Sie beim Setup ausgewählt haben. Sie können auswählen, **Change Password** oder **Zurücksetzen des Kennworts** Änderungen vornehmen.

Um Ihre e-Mail-zustellbarkeit dar-Einstellungen zu verwalten, klicken Sie auf die **Schaltfläche "verwalten"**. Dies wird zu Ihrem SendGrid-Dashboard umleiten.

    ![manage][manage]

    For more information on sending email through SendGrid, visit the [Email API Overview][Email API Overview].

<!--images-->

[command-bar-new]: ./media/sendgrid-sign-up/new-addon.png
[sendgrid-store]: ./media/sendgrid-sign-up/sendgrid-store.png
[sendgrid-create]: ./media/sendgrid-sign-up/sendgrid-create.png
[sendgrid-pricing]: ./media/sendgrid-sign-up/sendgrid-pricing.png
[all-resources]: ./media/sendgrid-sign-up/all-resources.png
[manage]: ./media/sendgrid-sign-up/manage.png
[settings]: ./media/sendgrid-sign-up/settings.png
[api-keys]: ./media/sendgrid-sign-up/api-keys.png
[general-api-key]: ./media/sendgrid-sign-up/general-api-key.png
[access]: ./media/sendgrid-sign-up/access.png
[key]: ./media/sendgrid-sign-up/key.png

<!--Links-->

[SendGrid Solutions]: https://sendgrid.com/solutions
[Azure Management Portal]: https://manage.windowsazure.com
[SendGrid Getting Started]: http://sendgrid.com/docs
[SendGrid Provisioning Process]: https://support.sendgrid.com/hc/articles/200181628-Why-is-my-account-being-provisioned-
[all APIs]: https://sendgrid.com/docs/API_Reference/index.html
[Email API Overview]: https://sendgrid.com/docs/API_Reference/Web_API_v3/Mail/index.html
