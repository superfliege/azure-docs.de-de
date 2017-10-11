> [!IMPORTANT]
> Um Pushbenachrichtigungen von Mobile Engagement zu erhalten, müssen Sie aktivieren `Silent Remote Notifications` in Ihrer Anwendung. Sie müssen das UIBackgroundModes-Array, in der Datei "Info.plist" der Remote-Notification-Wert hinzu.
> 
> 

1. Open `info.plist` Datei im Projekt
2. Klicken Sie mit der rechten Maustaste auf das oberste Element in der Liste (`Information Property List`) und eine neue Zeile hinzufügen
   
    ![](./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push1.png)
3. Geben Sie in der neuen Zeile`Required background modes`
   
    ![](./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push2.png)
4. Klicken Sie auf den nach-links-Taste, um die Zeile zu erweitern.
5. Fügen Sie den folgenden Wert für das Element 0`App downloads content in response to push notifications`
   
    ![](./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push3.png)
6. Nachdem Sie die Änderung vornehmen, sollte die Datei "Info.plist" XML die folgenden Schlüssel und Wert enthalten:
   
        <key>UIBackgroundModes</key>
        <array>
        <string>remote-notification</string>
        </array>
7. Bei Verwendung von **Xcode 7 und höher** und **iOS 9 +**:
   
   * Aktivieren Sie **Pushbenachrichtigungen** in Ziele > den Zielnamen Ihres > Funktionen.

