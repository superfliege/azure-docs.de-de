
1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) die Option **Alle durchsuchen** > **App Services** aus. Wählen Sie dann Ihr Mobile Apps-Back-End aus. Wählen Sie unter **Einstellungen** die Option **App Service Push** aus. Wählen Sie dann den Namen Ihres Notification Hub aus.
2. Wechseln Sie zu **Windows (WNS)**. Geben Sie nun den **Sicherheitsschlüssel** (Clientgeheimnis) und die **Paket-SID** von der Live Services-Website ein. Wählen Sie dann **Speichern** aus.

    ![Festlegen des WNS-Schlüssels im Portal](./media/app-service-mobile-configure-wns/mobile-push-wns-credentials.png)

Das Back-End ist jetzt für die Verwendung von WNS zum Senden von Pushbenachrichtigungen konfiguriert.
