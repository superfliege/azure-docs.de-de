### <a name="prerequisites"></a>Voraussetzungen
Sie benötigen eine [Service Bus](https://azure.microsoft.com/services/service-bus/) Konto.  

Bevor Sie Ihr Azure Service Bus-Konto in einer Logik-app verwenden können, müssen Sie die Logik-app zur Verbindung mit Ihrem Servicebus-Konto autorisieren. Glücklicherweise können Sie problemlos innerhalb Ihrer Logik-app auf Azure-Portal vorgehen.  

Es folgen die Schritte zum Autorisieren der Logik-app für die Verbindung zu Ihrem Service Bus-Konto:  

1. Wählen Sie zum Erstellen einer Verbindungs mit Service Bus in der Logik-app-Designer **Anzeigen von Microsoft verwalteten APIs** in der Dropdown-Liste. Geben Sie dann **service Bus-** in das Suchfeld. Wählen Sie den Trigger oder die Aktion, die Sie verwenden möchten.  
    ![Bild: datenbankverbindung Service Bus 1](./media/connectors-create-api-servicebus/servicebus-1.png)  
2. Wenn Sie alle Verbindungen mit Service Bus vor erstellt haben, werden Sie aufgefordert, Ihren Service Bus-Anmeldeinformationen. Diese Anmeldeinformationen werden verwendet, um Ihre Logik-app eine Verbindung herstellen und Zugriff auf Daten in Ihrem Service Bus-Konto zu autorisieren. Der Service Bus-Connector benötigt die Verbindungszeichenfolge für den Service Bus-Namespace. Darüber hinaus müssen **verwalten** Berechtigungen. Eine gute Möglichkeit, Sie wissen, ob Ihre Verbindungszeichenfolge bezieht sich auf den Namespace oder eine bestimmte Entität ist, wenn es enthält die `EntityPath` Parameter. Wenn dies der Fall ist, ist es nicht die richtige Verbindungszeichenfolge für eine Logik-app.  
    ![Service Bus-Verbindungszeichenfolge](./media/connectors-create-api-servicebus/connectionstring.png)
3. Nachdem Sie die Verbindungszeichenfolge für den Namespace erhalten haben, können Sie es für die API-Verbindung im Logik-Apps verwenden.  
    ![Service Bus-Verbindung Bild 2](./media/connectors-create-api-servicebus/servicebus-2.png)  
4. Beachten Sie die Verbindung erstellt wurde, und Sie sind nun mit den anderen Schritten in der Logik-app zu fortfahren.  
    ![Service Bus-Verbindung Bild 3](./media/connectors-create-api-servicebus/servicebus-3.png)   

