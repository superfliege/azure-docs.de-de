
1. Klicken Sie auf die **Anwendungsdienste** Schaltfläche, wählen Sie Ihre Mobile Apps-Back-End, wählen Sie **Schnellstart**, und wählen Sie dann Ihre Clientplattform (iOS, Android, Xamarin, Cordova).

    ![Azure-Portal mit Mobile Apps Schnellstart hervorgehoben][quickstart]

2. Wenn eine datenbankverbindung nicht konfiguriert ist, erstellen Sie dazu wie folgt vor:

    ![Azure-Portal mit Mobile Apps Connect auf Datenbank][connect]

    a. Erstellt eine neue SQL-Datenbank und Server.

    ![Azure-Portal für Mobile Apps erstellen, neue Datenbank und server][server]

    b. Warten Sie, bis die Erstellung der Datenverbindung erfolgreich abgeschlossen wurde.

    ![Azure-Portal Benachrichtigung der erfolgreichen Erstellung der Datenverbindung][notification]

    c. Die Herstellung der Datenverbindung muss erfolgreich sein.

    ![Azure Portal-Benachrichtigung "Sie haben bereits eine Datenverbindung"][already-connection]

3. Wählen Sie unter **2. Tabellen-API erstellen** als **Back-End-Sprache** die Option „Node.js“ aus. 
 
4. Akzeptieren Sie die Bestätigung, und wählen Sie dann **erstellen TodoItem-Tabelle**.  
    Diese Aktion erstellt eine neue Aufgabe Item-Tabelle in der Datenbank. 

    >[!IMPORTANT]
    > Ein vorhandenes Back-End in Node.js wechseln, werden alle Inhalt überschrieben. Um ein .NET Back-End stattdessen zu erstellen, finden Sie unter [mit dem .NET Back-End-Server SDK für Mobile Apps arbeiten][instructions].

<!-- Images. -->
[quickstart]: ./media/app-service-mobile-configure-new-backend/quickstart.png
[connect]: ./media/app-service-mobile-configure-new-backend/connect-to-bd.png
[notification]: ./media/app-service-mobile-configure-new-backend/notification-data-connection-create.png
[server]: ./media/app-service-mobile-configure-new-backend/create-new-server.png
[already-connection]: ./media/app-service-mobile-configure-new-backend/already-connection.png

<!-- URLs -->
[instructions]: ../articles/app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#create-app
