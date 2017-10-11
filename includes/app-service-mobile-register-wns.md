
1. Klicken Sie im Visual Studio-Projektmappen-Explorer mit der rechten Maustaste in den Windows Store-app-Projekt, und klicken Sie auf **Store** > **App mit Store verknüpfen**.

    ![Windows Store app zuordnen](./media/app-service-mobile-register-wns/notification-hub-associate-win8-app.png)
2. Klicken Sie im Assistenten auf **Weiter**, und melden Sie sich mit Ihrem Microsoft-Konto. Geben Sie einen Namen für Ihre app im **einen neuen app-Namen reservieren**, und klicken Sie dann auf **reservieren**.
3. Nachdem die app-Registrierung wurde erfolgreich erstellt wurde, wählen Sie den neuen Namen für die app aus, klicken Sie auf **Weiter**, und klicken Sie dann auf **zuordnen**. Dies fügt die erforderlichen Registrierungsinformationen für Windows Store, auf das Anwendungsmanifest.
4. Wiederholen Sie die Schritte 1 und 3 für Windows Phone Store-app-Projekt mithilfe der zuvor erstellten Registrierungs für Windows Store-app.  
5. Navigieren Sie zu der [Windows Dev Center](https://dev.windows.com/en-us/overview), und melden Sie sich mit Ihrem Microsoft-Konto. Klicken Sie auf die neuen app-Registrierung in **Meine apps**, und erweitern Sie dann **Services** > **Pushbenachrichtigungen**.
6. Auf der **Pushbenachrichtigungen** auf **Live-Dienste-Website** unter **Windows Push Notification Services (WNS) und Microsoft Azure-Mobile Apps**. Notieren Sie sich die Werte der **Paket-SID** und *aktuelle* Wert in **Anwendungsgeheimnis**. 

    ![App-Einstellung im Developer center](./media/app-service-mobile-register-wns/mobile-services-win8-app-push-auth.png)

   > [!IMPORTANT]
   > Die geheimen Anwendungsschlüssel und Paket-SID sind wichtige Sicherheitsmerkmale auf. Freigeben Sie diese Werte für jede Person oder verteilen Sie Ihre app nicht.
   >
   >
