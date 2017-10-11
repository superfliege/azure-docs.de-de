
1. In der [Azure-Portal](https://portal.azure.com/), klicken Sie auf **alle Durchsuchen** > **Anwendungsdienste**, und klicken Sie auf Ihre Mobile Apps-Back-End. Klicken Sie unter **Einstellungen**, klicken Sie auf **App Service-Push**, und klicken Sie dann auf Notification Hub-Name.
2. Wechseln Sie zu **Windows (WNS)**, geben Sie die **Sicherheitsschlüssel** (geheimer Clientschlüssel) und **Paket-SID** , die Sie aus der Live-Dienste-Website abgerufen, und klicken Sie dann auf **speichern**.

    ![Legen Sie den WNS-Schlüssel im portal](./media/app-service-mobile-configure-wns/mobile-push-wns-credentials.png)

Die Back-End ist jetzt für WNS zu verwenden, um das Senden von Pushbenachrichtigungen konfiguriert.
