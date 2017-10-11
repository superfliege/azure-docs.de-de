

1. Starten Sie auf Ihrem Mac **Schlüsselbundverwaltung**. Auf der linken Navigationsleiste unter **Kategorie**öffnen **eigene Zertifikate**. Suchen Sie das SSL-Zertifikat, das Sie im vorherigen Abschnitt heruntergeladen haben, und anzugeben Sie seinen Inhalt. Wählen Sie nur das Zertifikat (Aktivieren Sie nicht den privaten Schlüssel), und [exportieren](https://support.apple.com/kb/PH20122?locale=en_US).
2. In der [Azure-Portal](https://portal.azure.com/), klicken Sie auf **alle Durchsuchen** > **Anwendungsdienste**, und klicken Sie auf Ihre Mobile Apps-Back-End. Klicken Sie unter **Einstellungen**, klicken Sie auf **App Service-Push**, und klicken Sie dann auf Notification Hub-Name. Wechseln Sie zu **Apple Push Notification Services** > **Zertifikat hochladen**. Die Auswahl der richtigen P12-Datei hochladen **Modus** (je nachdem, ob Ihr Client SSL-Zertifikat aus zuvor ist Produktions- oder Sandkasten). Alle Änderungen zu speichern.

Der Dienst ist jetzt für die Zusammenarbeit mit Pushbenachrichtigungen in iOS konfiguriert.

[1]: ./media/app-service-mobile-apns-configure-push/mobile-push-notification-hub.png
