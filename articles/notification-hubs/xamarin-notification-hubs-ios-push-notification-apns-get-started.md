---
title: "Erste Schritte mit Azure Notification Hubs für Xamarin.iOS-Apps | Microsoft-Dokumentation"
description: In diesem Lernprogramm erfahren Sie, wie Sie mithilfe von Azure Notification Hubs eine Pushbenachrichtigung an eine Xamarin.iOS-Anwendung senden.
services: notification-hubs
keywords: iOS-Pushbenachrichtigungen,Pushnachrichten,Pushbenachrichtigungen,Pushnachricht
documentationcenter: xamarin
author: jwhiteDev
manager: kpiteira
editor: 
ms.assetid: 4d4dfd42-c5a5-4360-9d70-7812f96924d2
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 12/22/2017
ms.author: jawh
ms.openlocfilehash: 38ad8a15fcc4077926e735e01f877a4ee66718ef
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2018
---
# <a name="get-started-with-azure-notification-hubs-for-xamarinios-apps"></a>Erste Schritte mit Azure Notification Hubs für Xamarin.iOS-Apps
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Übersicht
> [!NOTE]
> Sie benötigen ein aktives Azure-Konto, um dieses Lernprogramm abzuschließen. Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter [Einen Monat kostenlos testen](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).
> 
> 

In diesem Lernprogramm erfahren Sie, wie Sie mithilfe von Azure Notification Hubs Pushbenachrichtigungen an eine iOS-App senden. Sie erstellen eine leere Xamarin.iOS-App, die Pushbenachrichtigungen mithilfe des [Apple-Pushbenachrichtigungsdiensts (Apple Push Notification Service, APNs)](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html)empfängt. 

Sobald Sie dieses Lernprogramm abgeschlossen haben, können Sie über Ihren Notification Hub Pushbenachrichtigungen an alle Geräte senden, die Ihre App ausführen. Der komplette Code ist in der [NotificationHubs-Beispiel-App][GitHub] verfügbar.

Im Lernprogramm wird ein einfaches Übertragungsszenario für Pushbenachrichtigungen mithilfe von Notification Hubs veranschaulicht.

## <a name="prerequisites"></a>Voraussetzungen
Für dieses Lernprogramm ist Folgendes erforderlich:

* Neueste Version von [Xcode][Install Xcode]
* Mit iOS 10 (oder einer neueren Version) kompatibles Gerät
* [Apple-Entwicklerprogramm-Mitgliedschaft](https://developer.apple.com/programs/)
* [Visual Studio für Mac]
  
  > [!NOTE]
  > Die Beispielanwendung muss aufgrund von Konfigurationsanforderungen für iOS-Pushbenachrichtigungen auf einem physischem iOS-Gerät (iPhone oder iPad) bereitgestellt und getestet werden, anstatt im Simulator.
  > 
  > 

Die Absolvierung dieses Tutorials ist eine Voraussetzung für alle anderen Notification Hubs-Tutorials für Xamarin.iOS-Apps.

[!INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

## <a name="configure-your-notification-hub-for-ios-push-notifications"></a>Konfigurieren Ihres Notification Hub für iOS-Pushbenachrichtigungen
Dieser Abschnitt führt Sie durch die Schritte zum Erstellen eines neuen Notification Hubs sowie zum Konfigurieren der Authentifizierung mit APNs unter Verwendung des zuvor erstellten Pushzertifikats vom Typ **.p12**. Wenn Sie einen bereits erstellten Notification Hub verwenden möchten, können Sie zu Schritt 5 springen.

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">

<li>

<p>Klicken Sie auf die Schaltfläche <b>Notification Services</b>, und wählen Sie <b>Apple (APNS)</b> aus. Wählen Sie <b>Zertifikat</b> aus, klicken Sie auf das Dateisymbol, und wählen Sie die Datei vom Typ <b>.p12</b> aus, die Sie zuvor exportiert haben. Achten Sie auch darauf, dass Sie das richtige Kennwort angeben.</p>

<p>Wählen Sie unbedingt den Modus <b>Sandkasten</b> aus, da Sie in einer Entwicklungsumgebung arbeiten. Wählen Sie den Modus <b>Produktion</b> nur dann, wenn Sie Pushbenachrichtigungen an Benutzer senden möchten, die Ihre App im Store erworben haben.</p>
</li>
</ol>

&emsp;&emsp;&emsp;&emsp;![Konfigurieren von APNS im Azure-Portal][6]

&emsp;&emsp;&emsp;&emsp;![Konfigurieren der APNS-Zertifizierung im Azure-Portal][7]

Der Notification Hub ist jetzt für die Arbeit mit APNS konfiguriert, und Sie besitzen die Verbindungszeichenfolge, um die App zu registrieren und Pushbenachrichtigungen zu versenden.

## <a name="connect-your-app-to-the-notification-hub"></a>Verbinden Ihrer App mit dem Notification Hub
#### <a name="create-a-new-project"></a>Erstellen eines neuen Projekts
1. Erstellen Sie in Visual Studio ein neues iOS-Projekt, wählen Sie die Vorlage **Einzelansicht-App** aus, und klicken Sie auf **Weiter**.
   
     ![Visual Studio: Auswählen des Anwendungstyps][31]

2. Geben Sie Ihren App-Namen und die Organisations-ID ein, und klicken Sie anschließend auf **Weiter** > **Erstellen**

3. Doppelklicken Sie in der Projektmappenansicht auf *Info.plist*, und vergewissern Sie sich unter **Identität**, dass Ihre Paket-ID der ID entspricht, die beim Erstellen Ihres Bereitstellungsprofils verwendet wurde. Vergewissern Sie sich unter **Signierung**, dass unter **Team** Ihr Entwicklerkonto ausgewählt ist. Vergewissern Sie sich außerdem, dass das Kontrollkästchen für die automatische Signierungsverwaltung aktiviert ist und dass Ihr Signaturzertifikat und Ihr Bereitstellungsprofil automatisch ausgewählt wurden.

    ![Visual Studio: iOS-App-Konfiguration][32]

4. Fügen Sie das Azure-Messaging-Paket hinzu. Klicken Sie in der Projektmappenansicht mit der rechten Maustaste auf das Projekt, und wählen Sie **Hinzufügen** > **NuGet-Pakete hinzufügen** aus. Suchen Sie nach **Xamarin.Azure.NotificationHubs.iOS**, und fügen Sie das Paket dem Projekt hinzu.

5. Fügen Sie Ihrer Klasse eine neue Datei hinzu, und nennen Sie sie **Constants.cs**. Fügen Sie anschließend die folgenden Variablen hinzu, und ersetzen Sie die Platzhalter für Zeichenfolgenliterale durch Ihren *Hubnamen* und den zuvor notierten Wert für *DefaultListenSharedAccessSignature*.
   
    ```csharp
        // Azure app-specific connection string and hub path
        public const string ConnectionString = "<Azure connection string>";
        public const string NotificationHubPath = "<Azure hub path>";
    ```

6. Fügen Sie die folgende Anweisung zur Datei **AppDelegate.cs**hinzu:
   
    ```csharp
        using WindowsAzure.Messaging;
    ```

7. Erstellen Sie eine Instanz von **SBNotificationHub**:
   
    ```csharp
        private SBNotificationHub Hub { get; set; }
    ```

8. Ändern Sie **FinishedLaunching()** in **AppDelegate.cs** wie folgt:
   
    ```csharp
        public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
        {
            if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
                var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                       UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound,
                       new NSSet ());
   
                UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
                UIApplication.SharedApplication.RegisterForRemoteNotifications ();
            } else {
                UIRemoteNotificationType notificationTypes = UIRemoteNotificationType.Alert | UIRemoteNotificationType.Badge | UIRemoteNotificationType.Sound;
                UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (notificationTypes);
            }
   
            return true;
        }
    ```

9. Überschreiben Sie die **RegisteredForRemoteNotifications()**-Methode in **AppDelegate.cs**:
   
    ```csharp
        public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
        {
            Hub = new SBNotificationHub(Constants.ConnectionString, Constants.NotificationHubPath);
   
            Hub.UnregisterAllAsync (deviceToken, (error) => {
                if (error != null)
                {
                    Console.WriteLine("Error calling Unregister: {0}", error.ToString());
                    return;
                }
   
                NSSet tags = null; // create tags if you want
                Hub.RegisterNativeAsync(deviceToken, tags, (errorCallback) => {
                    if (errorCallback != null)
                        Console.WriteLine("RegisterNativeAsync error: " + errorCallback.ToString());
                });
            });
        }
    ```

10. Überschreiben Sie die **ReceivedRemoteNotification()**-Methode in **AppDelegate.cs**:
   
    ```csharp
        public override void ReceivedRemoteNotification(UIApplication application, NSDictionary userInfo)
        {
            ProcessNotification(userInfo, false);
        }
    ```

11. Erstellen Sie die folgende **ProcessNotification()**-Methode in **AppDelegate.cs**:
   
    ```csharp
        void ProcessNotification(NSDictionary options, bool fromFinishedLaunching)
        {
            // Check to see if the dictionary has the aps key.  This is the notification payload you would have sent
            if (null != options && options.ContainsKey(new NSString("aps")))
            {
                //Get the aps dictionary
                NSDictionary aps = options.ObjectForKey(new NSString("aps")) as NSDictionary;
   
                string alert = string.Empty;
   
                //Extract the alert text
                // NOTE: If you're using the simple alert by just specifying
                // "  aps:{alert:"alert msg here"}  ", this will work fine.
                // But if you're using a complex alert with Localization keys, etc.,
                // your "alert" object from the aps dictionary will be another NSDictionary.
                // Basically the JSON gets dumped right into a NSDictionary,
                // so keep that in mind.
                if (aps.ContainsKey(new NSString("alert")))
                    alert = (aps [new NSString("alert")] as NSString).ToString();
   
                //If this came from the ReceivedRemoteNotification while the app was running,
                // we of course need to manually process things like the sound, badge, and alert.
                if (!fromFinishedLaunching)
                {
                    //Manually show an alert
                    if (!string.IsNullOrEmpty(alert))
                    {
                        UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                        avAlert.Show();
                    }
                }
            }
        }
    ```
   > [!NOTE]
   > Sie können **FailedToRegisterForRemoteNotifications()** überschreiben, um Situationen wie beispielsweise eine fehlende Netzwerkverbindung zu behandeln. Dies ist besonders wichtig, wenn Benutzer Ihre Anwendung ggf. im Offlinemodus starten (beispielsweise im Flugzeug) und Sie spezifische Pushnachrichtenszenarien für Ihre App behandeln möchten.
  

12. Führen Sie die App auf Ihrem Gerät aus.

## <a name="sending-test-push-notifications"></a>Senden von Pushbenachrichtigungen zu Testzwecken
Mit der Option *Testsendevorgang* im [Azure-Portal] können Sie den Empfang von Benachrichtigungen in Ihrer App testen. Diese Option sendet zu Testzwecken eine Pushbenachrichtigung an Ihr Gerät.

![Azure-Portal – Testsendung][30]

Pushbenachrichtigungen werden normalerweise mithilfe einer kompatiblen Bibliothek über einen Back-End-Dienst wie Mobile Apps oder ASP.NET gesendet. Falls für Ihr Back-End keine Bibliothek verfügbar ist, können Sie Benachrichtigungsmeldungen auch direkt über die REST-API senden.

Als nächster Schritt zum Senden von Benachrichtigungen über ein ASP.NET-Back-End empfiehlt sich das Tutorial [Verwenden von Notification Hubs für Pushbenachrichtigungen an Benutzer](notification-hubs-aspnet-backend-ios-apple-apns-notification.md). Sie können aber die folgenden Vorgehensweisen zum Senden von Benachrichtigungen verwenden:

Es folgen einige andere Lernprogramme, die Informationen zum Senden von Benachrichtigungen enthalten:
* REST-Schnittstelle: Sie können Pushbenachrichtigungen auf allen Back-End-Plattformen unterstützen, indem Sie die [REST-Schnittstelle](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx) verwenden.
* **Microsoft Azure Notification Hubs .NET SDK**: Führen Sie im Nuget-Paket-Manager für Visual Studio die Option [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)aus.
* Node.js: [Verwenden von Notification Hubs mit Node.js](notification-hubs-nodejs-push-notification-tutorial.md).
* Java/PHP**: Ein Beispiel zum Senden von Pushbenachrichtigungen über die REST-APIs finden Sie unter „Verwenden von Notification Hubs von Java/PHP“ ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).

## <a name="next-steps"></a>Nächste Schritte
In diesem einfachen Beispiel haben Sie Pushbenachrichtigungen an alle Ihre iOS-Geräte übertragen. Informationen zum Adressieren bestimmter Benutzer finden Sie in dem Lernprogramm [Benachrichtigen von Benutzern mit Benachrichtigungshubs]. Wenn Sie Ihre Benutzer in Interessengruppen einteilen möchten, finden Sie unter [Verwenden von Benachrichtigungshubs zum Übermitteln von Nachrichten]weitere Informationen. Weitere Informationen zur Verwendung von Notification Hubs finden Sie unter [Notification Hubs-Leitfaden] und [Notification Hubs-Informationen für iOS].

<!-- Images. -->
[6]: ./media/notification-hubs-ios-get-started/notification-hubs-apple-config.png
[7]: ./media/notification-hubs-ios-get-started/notification-hubs-apple-config-cert.png
[213]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-console-app.png

[215]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler1.png
[216]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler2.png

[30]: ./media/notification-hubs-ios-get-started/notification-hubs-test-send.png
[31]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-ios-app.png
[32]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-app-settings.png



<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Windows Azure Messaging Framework]: http://go.microsoft.com/fwlink/?LinkID=799698&clcid=0x409
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-xamarin-ios
[Notification Hubs-Leitfaden]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs-Informationen für iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456
[Visual Studio für Mac]: https://www.visualstudio.com/vs/visual-studio-mac/
[Benachrichtigen von Benutzern mit Benachrichtigungshubs]: /manage/services/notification-hubs/notify-users-aspnet
[Verwenden von Benachrichtigungshubs zum Übermitteln von Nachrichten]: /manage/services/notification-hubs/breaking-news-dotnet

[Local and Push Notification Programming Guide]:https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/HandlingRemoteNotifications.html#//apple_ref/doc/uid/TP40008194-CH6-SW1
[Apple Push Notification Service]: http://go.microsoft.com/fwlink/p/?LinkId=272584

[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/
[GitHub]: https://github.com/xamarin/mobile-samples/tree/master/Azure/NotificationHubs
[Zugriffsrichtlinien]: https://portal.azure.com
