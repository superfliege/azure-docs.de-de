
Standardmäßig können anonym APIs im Back-End für Mobile Apps aufgerufen werden. Als Nächstes müssen Sie den Zugriff auf nur authentifizierte Clients einschränken.  

* **Node.js zurück (über das Azure-Portal) enden** :  

    Klicken Sie in Ihrer Einstellungen für Mobile Apps können auf **einfache Tabellen** , und wählen Sie die Tabelle. Klicken Sie auf **Berechtigungen ändern**Option **authentifizierter Zugriff nur** für alle Berechtigungen, und klicken Sie dann auf **speichern**.
* **.NET back-End (c#)**:  

    Navigieren Sie im Server-Projekt zu **Controller** > **TodoItemController.cs**. Hinzufügen der `[Authorize]` -Attribut auf die **TodoItemController** -Klasse wie folgt. Um den Zugriff auf bestimmte Methoden zu beschränken, können Sie auch dieses Attribut nur auf diese Methoden anstelle der Klasse anwenden. Veröffentlichen Sie das Server-Projekt.

        [Authorize]
        public class TodoItemController : TableController<TodoItem>

* **Node.js-Back-End (über Node.js-Code)** :  

    Um die Authentifizierung für den Zugriff auf Tabellen benötigen, fügen Sie die folgende Zeile am Node.js-Server-Skript:

        table.access = 'authenticated';

    Weitere Informationen finden Sie unter [Vorgehensweise: Authentifizierung für den Zugriff auf Tabellen](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-auth). Zum Herunterladen von Schnellstart Codeprojekt aus Ihrer Website finden Sie unter [Vorgehensweise: Herunterladen von Node.js Back-End-Schnellstart Codeprojekt mithilfe von Git](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart).
