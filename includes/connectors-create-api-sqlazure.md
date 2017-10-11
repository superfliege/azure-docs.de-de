### <a name="prerequisites"></a>Voraussetzungen
* Ein Azure-Konto; Sie erstellen eine [Konto frei](https://azure.microsoft.com/free)
* Ein [Azure SQL-Datenbank](../articles/sql-database/sql-database-get-started.md) mit seiner Verbindungsinformationen, einschließlich Servername, Datenbankname und Benutzername/Kennwort. Diese Informationen sind in der SQL-Datenbank-Verbindungszeichenfolge enthalten:
  
    Server = Tcp:*Yoursqlservername*. database.windows.net,1433;Initial Catalog =*Yourqldbname*; Persist Security Info = false ergibt; Benutzer-ID = {Ihr_Benutzername}; Kennwort = {Your_password}; MultipleActiveResultSets = "false"; Verschlüsseln = True; "TrustServerCertificate" = "false"; Verbindungstimeout = 30;
  
    Erfahren Sie mehr über [Azure SQL-Datenbanken](https://azure.microsoft.com/services/sql-database).

> [!NOTE]
> Wenn Sie eine Azure SQL-Datenbank erstellen, können Sie auch die Beispieldatenbanken enthaltene SQL erstellen. 
> 
> 

Verbinden Sie bevor Sie Azure SQL-Datenbank in eine Logik-app verwenden zur SQL-Datenbank. Sie können problemlos innerhalb Ihrer Logik-app auf Azure-Portal so vorgehen.  

Verbinden Sie mit Ihrer Azure SQL-Datenbank mithilfe der folgenden Schritte:  

1. Erstellen Sie eine Logik-app. Fügen Sie in der Logik-Apps-Designer einen Trigger, und fügen Sie eine Aktion. Wählen Sie **Anzeigen von Microsoft verwalteten APIs** in der Dropdownliste aus, und klicken Sie dann "Sql" in das Suchfeld eingeben. Wählen Sie eine der Aktionen aus:  
   
    ![Erstellungsschritt für SQL Azure-Verbindung](./media/connectors-create-api-sqlazure/sql-actions.png)
2. Wenn Sie zuvor alle Verbindungen mit SQL-Datenbank erstellt haben, werden Sie aufgefordert, die Verbindungsdetails:  
   
    ![Erstellungsschritt für SQL Azure-Verbindung](./media/connectors-create-api-sqlazure/connection-details.png) 
3. Geben Sie die Details für die SQL-Datenbank aus. Eigenschaften mit einem Sternchen sind erforderlich.
   
   | Eigenschaft | Details |
   | --- | --- |
   | Verbinden über Gateway |Lassen Sie dieses nicht aktiviert. Wird verwendet, wenn eine Verbindung mit einer lokalen SQL Server herstellen. |
   | Verbindungsname * |Geben Sie einen Namen für die Verbindung aus. |
   | Name der SQL Server * |Geben Sie den Namen des Servers ein. Dies ist etwa *servername.database.windows.net*. Der Servername ist in den Eigenschaften der SQL-Datenbank im Azure-Portal angezeigt und auch in der Verbindungszeichenfolge angezeigt. |
   | Name der SQL-Datenbank * |Geben Sie den Namen, die Sie Ihre SQL-Datenbank beim Anfügen erhalten hat. Diese wird in den Eigenschaften der SQL-Datenbank in der Verbindungszeichenfolge aufgelistet: Initial Catalog =*Yoursqldbname*. |
   | Benutzername * |Geben Sie den Benutzernamen ein, den Sie bei der Erstellung der SQL-Datenbank erstellt haben. Dies wird in den Eigenschaften der SQL-Datenbank im Azure-Portal aufgeführt. |
   | Kennwort * |Geben Sie das Kennwort, das Sie bei der Erstellung der SQL-Datenbank erstellt haben. |
   
    Diese Anmeldeinformationen werden verwendet, um Ihre Logik-app eine Verbindung herstellen und den Zugriff auf Ihre SQL-Daten zu autorisieren. Sobald der Vorgang abgeschlossen ist, suchen die Verbindungsdetails folgt oder ähnlich:  
   
    ![Erstellungsschritt für SQL Azure-Verbindung](./media/connectors-create-api-sqlazure/sample-connection.png) 
4. Wählen Sie **Erstellen** aus. 
5. Beachten Sie, dass die Verbindung wurde erstellt. Fahren Sie mit den anderen Schritten in der Logik-app: 
   
    ![Erstellungsschritt für SQL Azure-Verbindung](./media/connectors-create-api-sqlazure/table.png)

