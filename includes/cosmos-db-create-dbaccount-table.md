1. Melden Sie sich in einem neuen Browserfenster beim [Azure-Portal](https://portal.azure.com/) an.
2. Klicken Sie im linken Menü auf **Neu**, dann auf **Datenbanken** und anschließend unter **Azure Cosmos DB** auf **Erstellen**. 
   
   ![Screenshot des Azure-Portals mit Hervorhebung von „Weitere Dienste“ und „Azure Cosmos DB“](./media/cosmos-db-create-dbaccount-table/create-nosql-db-databases-json-tutorial-1.png)

3. Geben Sie auf der Seite **Neues Konto** die Einstellungen für das neue Azure Cosmos DB-Konto ein. 
 
    Einstellung|Empfohlener Wert|Beschreibung
    ---|---|---
    ID|*Ein eindeutiger Name*|Geben Sie einen eindeutigen Namen ein, der das Azure Cosmos DB-Konto identifiziert. Da *documents.azure.com* an die ID angefügt wird, die Sie bereitstellen, um Ihren URI zu erstellen, sollten Sie eine eindeutige, aber identifizierbare ID verwenden.<br><br>Die ID darf nur Kleinbuchstaben, Zahlen und den Bindestrich (-) enthalten, und sie muss zwischen 3 und 50 Zeichen lang sein.
    API|Azure Table|Die API bestimmt den Typ des zu erstellenden Kontos. Azure Cosmos DB stellt fünf APIs bereit, die Sie für Ihre Anwendung auswählen können: SQL (Dokumentendatenbank), Gremlin (Diagrammdatenbank), MongoDB (Dokumentendatenbank), Azure Table und Cassandra. Für jede ist derzeit ein separates Konto erforderlich.<br><br>Wählen Sie **Azure Table** aus, da Sie mit dieser Schnellstartanleitung eine Tabelle erstellen, die mit der Table-API verwendet werden kann.<br><br>[Weitere Informationen zur Table-API](../articles/cosmos-db/table-introduction.md) |
    Abonnement|*Derselbe eindeutige Name wie oben für die ID*|Wählen Sie das Azure-Abonnement, das Sie für dieses Azure Cosmos DB-Konto verwenden möchten, aus. 
    Ressourcengruppe|*Derselbe Wert wie die ID*|Geben Sie einen neuen Ressourcengruppenname für Ihr Konto ein. Der Einfachheit halber können Sie denselben Namen wie bei Ihrer ID verwenden. 
    Standort|*Die Region, die Ihren Benutzern am nächsten liegt*|Wählen Sie den geografischen Standort, an dem Ihr Azure Cosmos DB-Konto gehostet werden soll, aus. Verwenden Sie einen Standort, der Ihren Benutzern am nächsten liegt, um ihnen einen schnellen Zugriff auf die Daten zu gewähren.
    Georedundanz aktivieren| Nicht ausfüllen | Dadurch wird eine replizierte Version Ihrer Datenbank in einer zweiten (zugeordneten) Region erstellt. Lassen Sie diese Einstellung leer.  
    An Dashboard anheften | Select | Aktivieren Sie dieses Kontrollkästchen, damit Ihr neues Datenbankkonto für den einfacheren Zugriff Ihrem Portaldashboard hinzugefügt wird.

    Klicken Sie dann auf **Erstellen**.  

    ![Screenshot des Blatts „Neue Azure Cosmos DB“](./media/cosmos-db-create-dbaccount-table/create-nosql-db-databases-json-tutorial-2.png)

4. Die Kontoerstellung dauert einige Minuten. Während der Kontoerstellung zeigt das Portal die Kachel **Azure Cosmos DB wird bereitgestellt** an.

    ![Der Bereich „Benachrichtigungen“ im Azure-Portal](./media/cosmos-db-create-dbaccount-table/deploying-cosmos-db.png)

    Nach der Erstellung des Kontos wird die Seite **Herzlichen Glückwunsch! Ihr Azure Cosmos DB-Konto wurde erstellt** angezeigt.
