
1. Navigieren Sie zu der [Konsole der Google Cloud](https://console.developers.google.com/project), melden Sie sich mit Ihren Google-Kontoanmeldeinformationen. 
2. Klicken Sie auf **Projekt erstellen**, geben Sie einen Projektnamen ein, und klicken Sie auf **erstellen**. Wenn angefordert, die SMS-Überprüfung ausführen, und klicken Sie auf **erstellen** erneut aus.
   
    ![Erstellen eines neuen Projekts](./media/mobile-services-enable-google-cloud-messaging/mobile-services-google-new-project.png)   
   
     Geben Sie Ihre neue **Projektname** , und klicken Sie auf **Projekt erstellen**.
3. Klicken Sie auf die **Hilfsprogramme und vieles mehr** Schaltfläche, und klicken Sie dann auf **Projektinformationen**. Notieren Sie sich die **Projektnummer**. Sie müssen diesen Wert festlegen die `SenderId` -Variable in der Client-app.
   
    ![Dienstprogramme und vieles mehr](./media/mobile-services-enable-google-cloud-messaging/notification-hubs-utilities-and-more.png)
4. In dem Projektdashboard unter **Mobile-APIs**, klicken Sie auf **Google Cloud Messaging**, klicken Sie dann auf der nächsten Seite auf **API aktivieren** und akzeptieren die Nutzungsbedingungen. 
   
    ![Aktivieren von GCM](./media/mobile-services-enable-google-cloud-messaging/enable-GCM.png)
   
    ![Aktivieren von GCM](./media/mobile-services-enable-google-cloud-messaging/enable-gcm-2.png) 
5. Klicken Sie in dem Projektdashboard auf **Anmeldeinformationen** > **Create Credential** > **API-Schlüssel**. 
   
    ![](./media/mobile-services-enable-google-cloud-messaging/mobile-services-google-create-server-key.png)
6. In **Erstellen eines neuen Schlüssels**, klicken Sie auf **Serverschlüssel**, geben Sie einen Namen für den Schlüssel, und klicken Sie auf **erstellen**.
7. Notieren Sie sich die **API-Schlüssel** Wert.
   
    Sie verwenden diese API-Schlüsselwert zum Aktivieren von Azure mit GCM authentifizieren und Senden von Pushbenachrichtigungen für Ihre app.

