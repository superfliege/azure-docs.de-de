
<!--
includes/sql-database-include-connection-string-20-portalshots.md

Latest Freshness check:  2015-09-02 , GeneMi.

## Connection string
-->


### <a name="obtain-the-connection-string-from-the-azure-portal"></a>Abrufen der Verbindungszeichenfolge vom Azure-portal
Verwenden der [Azure-Portal](https://portal.azure.com/) zum Abrufen der Verbindungszeichenfolge, die für Ihr Clientprogramm für die Interaktion mit Azure SQL-Datenbank erforderlich sind: 

1. Klicken Sie auf **Durchsuchen** > **SQL-Datenbanken**.
2. Geben Sie den Namen der Datenbank in das Textfeld "Filter" in der Nähe der oberen linken Ecke des der **SQL-Datenbanken** Blatt.
3. Klicken Sie auf die Zeile für die Datenbank.
4. Nachdem das Blatt für Ihre Datenbank angezeigt wird, können visual halber Sie klicken Sie auf die standard minimieren-Steuerelemente, um die Blades zu reduzieren, die Sie durchsuchen und Filtern der Datenbank verwendet. 
   
    ![Filtern Sie Ihre Datenbank zu isolieren][10-FilterDatabase]
5. Klicken Sie auf das Blatt für Ihre Datenbank auf **Anzeigen von Datenbank-Verbindungszeichenfolgen**.
6. Wenn Sie beabsichtigen, die ADO.NET Connection-Bibliothek verwenden, kopieren Sie die Zeichenfolge, die mit der Bezeichnung **ADO**. 
   
    ![Kopieren Sie die ADO-Verbindungszeichenfolge für die Datenbank][20-CopyAdoConnectionString]
7. Fügen Sie in einem Format oder einer anderen die Verbindungszeichenfolgeninformationen in Ihren Clientcode für die Anwendung ein.

Weitere Informationen finden Sie in folgenden Quellen:<br/>[Verbindungszeichenfolgen und Konfigurationsdateien](http://msdn.microsoft.com/library/ms254494.aspx).

<!-- Image references. -->

[10-FilterDatabase]: ./media/sql-database-include-connection-string-20-portalshots/connqry-connstr-a.png

[20-CopyAdoConnectionString]: ./media/sql-database-include-connection-string-20-portalshots/connqry-connstr-b.png


<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->
