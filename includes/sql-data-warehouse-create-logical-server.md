### <a name="create-a-new-logical-sql-server-in-the-azure-portal"></a>Erstellen eines neuen logischen SQL-Servers im Azure-portal

1. Klicken Sie auf **neu**, Suche **logischen Server**, und drücken Sie dann **EINGABETASTE**.

    ![logische Suchserver](./media/sql-data-warehouse-create-logical-server/search-logical-server.png)
2. Wählen Sie **SQLServer (logische Server)** 

    ![Wählen Sie die logischen server](./media/sql-data-warehouse-create-logical-server/select-logical-server.png)
  
3. Klicken Sie auf **erstellen** auf die neue SQL Server (logische Server) Blatt geöffnet.

   <kbd>![öffnen logischen Server Blatt](./media/sql-data-warehouse-create-logical-server/open-logical-server-blade.png) </kbd> <kbd> ![logischen Server Blatt](./media/sql-data-warehouse-create-logical-server/logical-server-blade.png)</kbd>
  
3. Geben Sie im Textfeld für das SQL Server (logische Server) Blade Server Name einen gültigen Namen für den neuen logischen Server ein. Ein grünes Häkchen zeigt an, dass Sie einen gültigen Namen eingegeben haben.
    
    ![neuen Namen des Servers](./media/sql-data-warehouse-create-logical-server/new-name-logical-server.png)

    > [!IMPORTANT]
    > Der vollqualifizierte Name für den neuen Server werden < Name Ihres Servers >. database.windows.net.
    >
    
4. Geben Sie im Textfeld Server Admin Anmeldung einen Benutzernamen für die Anmeldung der SQL-Authentifizierung für diesen Server ein. Diese Anmeldung ist als der prinzipalanmeldung auf Server bezeichnet. Ein grünes Häkchen zeigt an, dass Sie einen gültigen Namen eingegeben haben.
    
    ![SQL-Admin-Anmeldung](./media/sql-data-warehouse-create-logical-server/sql-admin-login.png)
5. In der **Kennwort** und **Kennwort bestätigen** Textfelder, geben Sie ein Kennwort für den Server principal Anmeldekonto. Ein grünes Häkchen zeigt an, dass Sie ein gültiges Kennwort eingegeben haben.
    
    ![SQL-Administratorkennwort](./media/sql-data-warehouse-create-logical-server/sql-admin-password.png)
6. Wählen Sie ein Abonnement, in dem Sie die Berechtigung zum Erstellen von Objekten besitzen.

    ![Abonnement](./media/sql-data-warehouse-create-logical-server/subscription.png)
7. Wählen Sie in das Textfeld Ressource Gruppe **neu erstellen** und klicken Sie dann im Textfeld Ressource Gruppe geben Sie einen gültigen Namen für die neue Ressourcengruppe (Sie können auch eine vorhandene Ressourcengruppe, wenn Sie bereits einen eigenen erstellt haben). Ein grünes Häkchen zeigt an, dass Sie einen gültigen Namen eingegeben haben.

    ![neue Ressourcengruppe.](./media/sql-data-warehouse-create-logical-server/new-resource-group.png)

8. In der **Speicherort** Textfeld, wählen Sie die Daten zu zentrieren, die Ihrem Standort – z. B. "East Australia".
    
    ![Server-Speicherort](./media/sql-data-warehouse-create-logical-server/server-location.png)
    
    > [!TIP]
    > Das Kontrollkästchen für **Azure-Diensten Zugriff auf den Server erlauben** kann nicht auf diesem Blatt geändert werden. Sie können diese Einstellung auf dem Blatt "Server-Firewall" ändern. Weitere Informationen finden Sie unter [erste Schritte mit Sicherheit](../articles/sql-database/sql-database-manage-servers-portal.md).
    >
    
9. Klicken Sie auf **Erstellen**.

    ![Schaltfläche "erstellen"](./media/sql-data-warehouse-create-logical-server/create.png)

