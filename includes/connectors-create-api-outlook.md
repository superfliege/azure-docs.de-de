## <a name="connect-to-outlookcom"></a>Herstellen einer Verbindung mit Outlook.com
### <a name="prerequisites"></a>Voraussetzungen
* Eine Outlook.com-Konto

Bevor Sie Ihre Outlook.com-Konto in einer Logik-app verwenden können, müssen Sie die Logik-app für die Verbindung mit Ihrem Outlook.com-Konto autorisieren. Glücklicherweise können Sie problemlos aus in der Logik-app im Azure-Portal vorgehen. 

Es folgen die Schritte zum Autorisieren der Logik-app für die Verbindung mit Ihrem Outlook.com-Konto:

1. Alle Logik-apps müssen durch einen Trigger gestartet werden, damit nach der Erstellung der Logik-app, und der Designer wird geöffnet und zeigt eine Liste von ausgelöst werden, die Sie verwenden können, die Logik-app zu starten:
   
   ![](./media/connectors-create-api-outlook/office365-outlook-0.png)
2. Geben Sie "Outlook" in das Suchfeld ein. Beachten Sie, dass die Liste gefiltert wird, um alle Trigger für "Outlook" im Namen aufzulisten:![](./media/connectors-create-api-outlook/office365-outlook-0-5.png)
3. Wählen Sie **Office 365 Outlook - auf neue e-Mails**.   
   Wenn Sie alle Verbindungen mit Outlook, bevor Sie erstellt haben, werden Sie aufgefordert Abrufen Ihrer Outlook.com-Anmeldeinformationen angeben. Diese Anmeldeinformationen werden verwendet, um Ihre Logik-app für die Verbindung autorisieren und einfacher Zugriff auf Ihre Outlook.com-Konto:![](./media/connectors-create-api-outlook/office365-outlook-1.png)
4. Geben Sie Ihre Anmeldeinformationen für Outlook, und melden Sie sich:![](./media/connectors-create-api-outlook/office365-outlook-2.png)  
   Das wars. Sie haben jetzt eine Verbindung mit Outlook erstellt. Diese Verbindung wird für die Verwendung in einer beliebigen anderen Logik-app verfügbar sein, die Sie erstellen.

