

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Klicken Sie auf **Ressourcen erstellen** > **Web + Mobil** > **Notification Hub**.
   
      ![Azure-Portal – Erstellen von Notification Hubs](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-create.png)
      
3. Geben Sie im Feld **Notification Hub** einen eindeutigen Namen ein. Wählen Sie **Region**, **Abonnement** und **Ressourcengruppe** (sofern bereits vorhanden). 
   
      Wenn Sie noch nicht über einen Service Bus-Namespace verfügen, können Sie den Standardnamen verwenden, der basierend auf dem Hub-Namen erstellt wird (sofern der Namespace-Name verfügbar ist).
    
      Gehen Sie wie folgt vor, wenn Sie bereits über einen Service Bus-Namespace verfügen, in dem Sie den Hub erstellen möchten:

    a. Wählen Sie im Bereich **Namespace** den Link **Vorhandene auswählen**. 
   
    b. Klicken Sie auf **Erstellen**.
   
      ![Azure-Portal – Festlegen von Eigenschaften für den Notification Hub](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-settings.png)

4. Klicken Sie nach dem Erstellen des Namespace und Ihrer Notification Hub-Instanz auf **Alle Ressourcen**, und wählen Sie in der Liste die erstellte Notification Hub-Instanz aus, um sie zu öffnen. 
   
      ![Azure-Portal – Portalseite für den Notification Hub](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-resources.png)

5. Wählen Sie in der Liste die Option **Zugriffsrichtlinien** aus. Notieren Sie sich die beiden Verbindungszeichenfolgen, die für Sie verfügbar sind. Sie werden später für die Behandlung von Pushbenachrichtigungen benötigt.

      >[!IMPORTANT]
      >Verwenden Sie **NICHT** „DefaultFullSharedAccessSignature“ in Ihrer Anwendung. Diese ist nur für die Verwendung in Ihrem Back-End vorgesehen.
      >
   
      ![Azure-Portal – Verbindungszeichenfolgen für den Notification Hub](./media/notification-hubs-portal-create-new-hub/notification-hubs-connection-strings-portal.png)

