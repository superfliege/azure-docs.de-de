### <a name="grant-mobile-engagement-access-to-your-gcm-api-key"></a>Gewähren von Mobile Engagement-Zugriff auf Ihren GCM API-Schlüssel
Damit können Mobile Engagement Pushbenachrichtigungen in Ihrem Auftrag zu senden, müssen Sie Zugriff auf Ihren API-Schlüssel gewähren. Dies erfolgt durch Konfigurieren und Sie Ihren Schlüssel in das Portal für Mobile Einbindung eingeben.

1. Stellen Sie sicher aus Ihrem klassischen Azure-Portal, Sie sind in der app, die wir für dieses Projekt verwenden möchten, und klicken Sie dann auf die **einbeziehen** unten auf die Schaltfläche:
   
    ![](./media/mobile-engagement-android-send-push/engage-button.png)
2. Klicken Sie dann auf die **Einstellungen** -> **nativen Push** Abschnitt aus, um die GCM-Schlüssel eingeben:
   
    ![](./media/mobile-engagement-android-send-push/engagement-portal.png)
3. Klicken Sie auf die **bearbeiten** Symbol vor **API-Schlüssel** in der **GCM-Einstellungen** Abschnitt wie folgt:
   
    ![](./media/mobile-engagement-android-send-push/native-push-settings.png)
4. Im Popupfenster, fügen Sie die Schlüssel des GCM-Server abgerufen werden, bevor es aus, und klicken Sie dann auf **Ok**.
   
    ![](./media/mobile-engagement-android-send-push/api-key.png)

## <a id="send"></a>Eine Benachrichtigung an die app senden
Es wird nun eine einfache Push Notification-Kampagne erstellt, die eine Pushbenachrichtigung an unserer app sendet.

1. Navigieren Sie zu der **erreichen** Registerkarte in Ihr Portal für Mobile Einbindung.
2. Klicken Sie auf **neue Ankündigung** Ihre Benachrichtigung pushkampagne zu erstellen.
   
    ![](./media/mobile-engagement-android-send-push/new-announcement.png)
3. Richten Sie das erste Feld der Kampagne durch die folgenden Schritte aus:
   
    ![](./media/mobile-engagement-android-send-push/campaign-first-params.png)
   
    ein. Benennen Sie Ihre Kampagne.
   
    b. Wählen Sie die **Übermittlungstyp** als *systembenachrichtigung -> einfache*: Dies ist die einfache Android pushbenachrichtigungstyp, die einen Titel und eine kleine Textzeile.
   
    c. Wählen Sie **Übermittlungszeit** als *jederzeit* zum ermöglichen der Apps, die eine Benachrichtigung erhalten, ob die app oder nicht gestartet wird.
   
    d. In der Benachrichtigung Texttyp der **Titel** kann sich in in der Push fett formatiert.
   
    e. Geben Sie dann Ihre **Nachricht**
4. Führen Sie einen Bildlauf nach unten, und klicken Sie in der **Content** Abschnitt **Benachrichtigung nur**.
   
    ![](./media/mobile-engagement-android-send-push/campaign-content.png)
5. Sie haben die grundlegendste Kampagne möglich festlegen. Jetzt einen Bildlauf nach unten erneut aus, und klicken Sie auf die **erstellen** Schaltfläche, um Ihre Kampagne zu speichern.
6. Letzter Schritt: Klicken Sie auf **aktivieren** Ihre Kampagne zum Senden von Pushbenachrichtigungen zu aktivieren.
   
    ![](./media/mobile-engagement-android-send-push/campaign-activate.png)

