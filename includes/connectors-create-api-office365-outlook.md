#### <a name="prerequisites"></a>Voraussetzungen
* Ein Azure-Konto; Sie erstellen eine [Konto frei](https://azure.microsoft.com/free)
* Ein [Office 365](https://office365.com) Konto  

Autorisieren Sie, bevor Sie Ihre Office 365-Konto in eine Logik-app verwenden, die Logik-app zur Verbindung mit Ihrem Office 365-Konto. Sie können problemlos innerhalb Ihrer Logik-app auf Azure-Portal so vorgehen.  

Autorisieren Sie Ihre Logik-app zur Verbindung mit Ihrem Office 365-Konto mithilfe der folgenden Schritte:

1. Erstellen Sie eine Logik-app. Wählen Sie im Designer Logik-Apps **Anzeigen von Microsoft verwalteten APIs** in der Dropdownliste aus, und klicken Sie dann "Office 365" in das Suchfeld eingeben. Wählen Sie eine der Trigger oder Aktionen:  
    ![Erstellungsschritt für Office 365-Verbindung](./media/connectors-create-api-office365-outlook/office365-sendemail.png)  
2. Wenn Sie zuvor alle Verbindungen mit Office 365 erstellt haben, werden Sie aufgefordert, sich anzumelden, mit den Office 365-Anmeldeinformationen:  
    ![Erstellungsschritt für Office 365-Verbindung](./media/connectors-create-api-office365-outlook/office365-signin.png)  
3. Wählen Sie **anmelden**, und geben Sie Ihren Benutzernamen und Kennwort. Wählen Sie **anmelden**:  
    ![Erstellungsschritt für Office 365-Verbindung](./media/connectors-create-api-office365-outlook/office365-usernamepassword.png)
   
    Diese Anmeldeinformationen werden verwendet, um Ihre Logik-app zum Herstellen einer Verbindung mit und Zugreifen auf Ihr Office 365-Konto zu autorisieren. 
4. Beachten Sie, dass die Verbindung wurde erstellt. Fahren Sie mit den anderen Schritten in der Logik-app:   
    ![Erstellungsschritt für Office 365-Verbindung](./media/connectors-create-api-office365-outlook/office365-sendemailproperties.png)  

