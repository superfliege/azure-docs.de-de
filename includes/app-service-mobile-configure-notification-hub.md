Die Mobile Apps-Funktion von Azure App Service verwendet [Azure Notification Hubs] Push-Vorgänge, senden, damit Sie einen benachrichtigungs-Hub für die mobile app konfigurieren.

1. In der [Azure-Portal], wechseln Sie zu **Anwendungsdienste**, und klicken Sie dann auf Ihre app-Back-End. Klicken Sie unter **Einstellungen**, klicken Sie auf **Push**.
2. Klicken Sie auf **verbinden** der app einen Notification Hub-Ressource hinzu. Sie können entweder erstellen Sie einen Hub oder zum Verbinden mit einer vorhandenen Arbeitsaufgabe.

    ![](./media/app-service-mobile-create-notification-hub/configure-hub-flow.png)

Sie haben nun einen benachrichtigungs-Hub zu Ihrem Mobile Apps-Back-End-Projekt verbunden. Konfigurieren Sie später dieser Notification Hub zur Verbindung mit einem plattformbenachrichtigungssystem (PNS) mithilfe von Push auf Geräte übertragen.

[Azure-Portal]: https://portal.azure.com/
[Azure Notification Hubs]: https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-push-notification-overview/
