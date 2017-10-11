### <a name="grant-access-to-your-push-certificate-to-mobile-engagement"></a>Gewähren des Zugriffs auf Ihr Push-Zertifikat, Mobile Engagement
Um Mobile Engagement zum Senden von Pushbenachrichtigungen in Ihrem Auftrag zu ermöglichen, müssen Sie den Zugriff auf das Zertifikat gewährt. Dies erfolgt durch Konfigurieren und Sie Ihr Zertifikat in das Portal für Mobile Einbindung eingeben. Sicherstellen, dass Sie Ihre P12-Zertifikat anfordern, wie in beschrieben [Apple Dokumentation](https://developer.apple.com/library/prerelease/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)

1. Navigieren Sie zu Ihrem Portal für Mobile Einbindung. Stellen Sie sicher sind, in der richtigen und klicken Sie dann auf die **einbeziehen** unten auf die Schaltfläche:
   
    ![](./media/mobile-engagement-ios-send-push/engage-button.png)
2. Klicken Sie auf die **Einstellungen** Seite im Portal für die Einbindung. Von dort auf die **nativen Push** Abschnitt aus, um Ihr p12-Zertifikat hochzuladen:
   
    ![](./media/mobile-engagement-ios-send-push/engagement-portal.png)
3. Wählen Sie Ihre p12, laden Sie es hoch, und geben Ihr Kennwort ein:
   
    ![](./media/mobile-engagement-ios-send-push/native-push-settings.png)

## <a id="send"></a>Eine Benachrichtigung an die app senden
Es wird nun eine einfache Pushbenachrichtigung Kampagne erstellt, die einen Push an unserer app sendet:

1. Navigieren Sie zu der **erreichen** Registerkarte in Ihr Portal für Mobile Einbindung.
2. Klicken Sie auf **neue Ankündigung** um Ihre pushkampagne zu erstellen.
   
    ![](./media/mobile-engagement-ios-send-push/new-announcement.png)
3. Setup für die erste Felder der Kampagne:
   
    ![](./media/mobile-engagement-ios-send-push/campaign-first-params.png)
   
   * Geben Sie einen **Namen** für Ihre Kampagne 
   * Wählen Sie die **Übermittlungszeit** als **außerhalb der app nur**: Dies ist der einfache Apple Push Notification-Typ, der Funktionen von Text.
   * Geben Sie in den Benachrichtigungstext zuerst die **Titel** wird die erste Zeile in der Push-Vorgang sein.
   * Geben Sie dann Ihre **Nachricht** der ist die zweite Zeile
4. Führen Sie einen Bildlauf nach unten und wählen Sie im Abschnitt Inhalt **ausschließlichen Benachrichtigung**
   
    ![](./media/mobile-engagement-ios-send-push/campaign-content.png)
5. Sie haben die meisten grundlegende Kampagne festlegen. Jetzt einen Bildlauf nach unten, und klicken Sie auf **erstellen** Schaltfläche, um Ihre Benachrichtigung pushkampagne zu speichern. 
6. Klicken Sie abschließend - auf **aktivieren** Pushbenachrichtigung zu senden. 
   
    ![](./media/mobile-engagement-ios-send-push/campaign-activate.png)
7. Sie werden in der Lage, empfangen die Benachrichtigung auf Ihrem iOS-Gerät die mitteilungszentrale wie folgt:
   
    ![](./media/mobile-engagement-ios-send-push/iphone-notification.png)
8. Wenn Sie ein Apple Watch-mit diesem iOS-Gerät verbunden haben, werden Sie die Benachrichtigung auf Ihrer Apple Watch-angezeigt:
   
    ![](./media/mobile-engagement-ios-send-push/apple-watch.png)

