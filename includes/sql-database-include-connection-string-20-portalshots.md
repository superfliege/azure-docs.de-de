
<!--
includes/sql-database-include-connection-string-20-portalshots.md

Latest Freshness check:  2015-09-02 , GeneMi.

## Connection string
-->


### <a name="obtain-the-connection-string-from-the-azure-portal"></a>Abrufen der Verbindungszeichenfolge aus dem Azure-Portal
Nutzen Sie das [Azure-Portal](https://portal.azure.com/) zum Abrufen der Verbindungszeichenfolge, die für die Interaktion des Clientprogramms mit Azure SQL-Datenbank benötigt wird. 

1. Wählen Sie **Alle Dienste** > **SQL-Datenbanken** aus.

2. Geben Sie in das Textfeld „Filter“ nahe der oberen linken Ecke des Blattes **SQL-Datenbanken** den Namen der Datenbank ein.

3. Wählen Sie die Zeile für die Datenbank aus.

4. Nachdem das Blatt für die Datenbank angezeigt wird, können Sie der visuellen Einfachheit halber die Schaltflächen zum **Minimieren** auswählen, um die Blätter auszublenden, die Sie zum Durchsuchen und Filtern verwendet haben. 
   
5. Wählen Sie auf dem Blatt für die Datenbank **Datenbankverbindungszeichenfolgen anzeigen** aus.

6. Wenn Sie die ADO.NET-Verbindungsbibliothek verwenden wollen, kopieren Sie die Zeichenfolge mit der Bezeichnung **ADO**. 
   
    ![Kopieren der ADO-Verbindungszeichenfolge für die Datenbank][20-CopyAdoConnectionString]
7. Fügen Sie in dem ein oder anderen Format die Informationen der Verbindungszeichenfolge in den Clientcode für die Anwendung ein.

Weitere Informationen finden Sie unter [Verbindungszeichenfolgen und Konfigurationsdateien](http://msdn.microsoft.com/library/ms254494.aspx).

<!-- Image references. -->



[20-CopyAdoConnectionString]: ./media/sql-database-include-connection-string-20-portalshots/connqry-connstr-b.png


<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->
