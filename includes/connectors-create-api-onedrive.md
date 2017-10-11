#### <a name="prerequisites"></a>Voraussetzungen
* Ein Azure-Konto; Sie erstellen eine [Konto frei](https://azure.microsoft.com/free)
* Ein [OneDrive](https://www.microsoft.com/store/apps/onedrive/9wzdncrfj1p3) Konto 

Bevor Sie Ihr OneDrive-Konto in einer Logik-app verwenden können, autorisieren Sie die Logik-app zur Verbindung mit Ihrem OneDrive-Konto.  Sie können problemlos innerhalb Ihrer Logik-app auf Azure-Portal so vorgehen. 

Autorisieren Sie Ihre Logik-app zur Verbindung mit Ihrem OneDrive-Konto mithilfe der folgenden Schritte:

1. Erstellen Sie eine Logik-app. Wählen Sie im Designer Logik-Apps **Anzeigen von Microsoft verwalteten APIs** in der Dropdownliste aus, und klicken Sie dann "Onedrive" in das Suchfeld eingeben. Wählen Sie eine der Trigger oder Aktionen:  
   ![](./media/connectors-create-api-onedrive/onedrive-1.png)
2. Wenn Sie alle Verbindungen mit OneDrive zuvor erstellt haben, werden Sie aufgefordert, melden Sie sich mit Ihrem OneDrive-Anmeldeinformationen:  
   ![](./media/connectors-create-api-onedrive/onedrive-2.png)
3. Wählen Sie **anmelden**, und geben Sie Ihren Benutzernamen und Kennwort. Wählen Sie **anmelden**:  
   ![](./media/connectors-create-api-onedrive/onedrive-3.png)   
   
    Diese Anmeldeinformationen werden verwendet, um Ihre Logik-app zum Herstellen einer Verbindung mit und Zugreifen auf die Daten in Ihrem OneDrive-Konto zu autorisieren. 
4. Wählen Sie **Ja** zum Autorisieren der Logik-app auf Ihrem OneDrive-Konto verwenden:  
   ![](./media/connectors-create-api-onedrive/onedrive-4.png)   
5. Beachten Sie, dass die Verbindung wurde erstellt. Fahren Sie mit den anderen Schritten in der Logik-app:  
   ![](./media/connectors-create-api-onedrive/onedrive-5.png)

