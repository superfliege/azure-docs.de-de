

1. Melden Sie sich bei der [Firebase Konsole](https://firebase.google.com/console/). Erstellen Sie ein neues Firebase-Projekt aus, wenn Sie noch keins besitzen.
2. Nachdem das Projekt erstellt wurde, klicken Sie auf **Firebase hinzufügen, um Ihre Android-app** und befolgen Sie die Anweisungen.

    ![](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-add-firebase-to-android-app.png)
3. Klicken Sie in der Konsole Firebase klicken Sie auf das Zahnrad für das Projekt, und klicken Sie dann auf **Projekteinstellungen**.

    ![](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-firebase-console-project-settings.png)
4. Klicken Sie auf die **Cloud Messaging** Registerkarte in den projekteinstellungen und kopieren Sie den Wert, der die **Serverschlüssel** und **Absender-ID**. Diese Werte werden später verwendet werden, die Zugriffsrichtlinie für Notification Hub und den Benachrichtigungshandler in der app zu konfigurieren.
