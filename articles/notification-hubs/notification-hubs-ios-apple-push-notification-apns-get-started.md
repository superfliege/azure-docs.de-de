---
title: "Erste Schritte mit Azure Notification Hubs für iOS-Apps | Microsoft-Dokumentation"
description: In diesem Tutorial erfahren Sie, wie Sie mithilfe von Azure Notification Hubs Pushbenachrichtigungen an iOS-Anwendungen senden.
services: notification-hubs
documentationcenter: ios
keywords: Pushbenachrichtigung,Pushbenachrichtigungen,iOS-Pushbenachrichtigungen
author: jwhitedev
manager: kpiteira
editor: 
ms.assetid: b7fcd916-8db8-41a6-ae88-fc02d57cb914
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: hero-article
ms.date: 12/22/2017
ms.author: jawh
ms.openlocfilehash: 0e9e7ab196eef790b74074be319cd8122cf3ff5c
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/02/2018
---
# <a name="get-started-with-azure-notification-hubs-for-ios-apps"></a>Erste Schritte mit Azure Notification Hubs für iOS-Apps
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Übersicht
> [!NOTE]
> Sie benötigen ein aktives Azure-Konto, um dieses Lernprogramm abzuschließen. Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter [Einen Monat kostenlos testen](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-ios-get-started).
> 
> 

In diesem Lernprogramm erfahren Sie, wie Sie mithilfe von Azure Notification Hubs Pushbenachrichtigungen an eine iOS-App senden. Sie erstellen eine leere iOS-App, die Pushbenachrichtigungen mithilfe des Apple-Pushbenachrichtigungsdiensts ( [Apple Push Notification Service, APNs](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html#//apple_ref/doc/uid/TP40008194-CH8-SW1)) empfängt. 

Nach Abschluss dieses Tutorials können Sie über Ihren Notification Hub Pushbenachrichtigungen an alle Geräte senden, auf denen Ihre App ausgeführt wird.

## <a name="before-you-begin"></a>Voraussetzungen
[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Den vollständigen Code für dieses Tutorial finden Sie [auf GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/iOS/GetStartedNH/GetStarted). 

## <a name="prerequisites"></a>Voraussetzungen
Für dieses Lernprogramm ist Folgendes erforderlich:

* [Microsoft Azure-Messaging-Framework]
* Neueste Version von [Xcode]
* Für iOS 10 (oder eine neuere Version) geeignetes Gerät
* [Apple-Entwicklerprogramm-Mitgliedschaft](https://developer.apple.com/programs/)
  
  > [!NOTE]
  > Pushbenachrichtigungen müssen aufgrund von Konfigurationsanforderungen auf einem physischen iOS-Gerät (iPhone oder iPad) anstatt im iOS-Simulator bereitgestellt und getestet werden.
  > 
  > 

Das Abschließen dieses Lernprogramms ist eine Voraussetzung für alle anderen Notification Hubs-Lernprogramme für iOS-Apps.

[!INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

## <a name="configure-your-notification-hub-for-ios-push-notifications"></a>Konfigurieren Ihres Notification Hub für iOS-Pushbenachrichtigungen
Dieser Abschnitt führt Sie durch die Schritte zum Erstellen eines neuen Notification Hubs sowie zum Konfigurieren der Authentifizierung mit APNs unter Verwendung des zuvor erstellten Pushzertifikats vom Typ **.p12**. Wenn Sie einen bereits erstellten Notification Hub verwenden möchten, können Sie zu Schritt 5 springen.

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">

<li>

<p>Wählen Sie unter <b>Notification Services</b> die Option <b>Apple (APNS)</b> aus. Wählen Sie <b>Zertifikat</b> aus, klicken Sie auf das Dateisymbol, und wählen Sie die Datei vom Typ <b>.p12</b> aus, die Sie zuvor exportiert haben. Achten Sie auch darauf, dass Sie das richtige Kennwort angeben.</p>

<p>Wählen Sie unbedingt den Modus <b>Sandkasten</b> aus, da Sie in einer Entwicklungsumgebung arbeiten. Wählen Sie den Modus <b>Produktion</b> nur dann, wenn Sie Pushbenachrichtigungen an Benutzer senden möchten, die Ihre App im Store erworben haben.</p>
</li>
</ol>
&emsp;&emsp;&emsp;&emsp;![Konfigurieren von APNS im Azure-Portal][6]

&emsp;&emsp;&emsp;&emsp;![Konfigurieren der APNS-Zertifizierung im Azure-Portal][7]

Sie haben nun Ihren Notification Hub mit APNS konfiguriert und verfügen über die Verbindungszeichenfolge zum Registrieren Ihrer App sowie zum Versenden von Pushbenachrichtigungen.

## <a name="connect-your-ios-app-to-notification-hubs"></a>Verbinden der iOS-App mit Notification Hubs
1. Erstellen Sie in Xcode ein neues iOS-Projekt, und wählen Sie die Vorlage **Single View Application** aus.
   
    ![Xcode – Single View Application][8]
    
2. Achten Sie beim Festlegen der Optionen für das neue Projekt darauf, dass Sie den gleichen **Produktnamen** und die gleiche **Organisations-ID** verwenden wie beim Festlegen der Paket-ID im Apple-Entwicklerportal.
   
    ![Xcode – Projektoptionen][11]
    
3. Klicken Sie in der Projektnavigation auf Ihren Projektnamen, klicken Sie auf die Registerkarte **Allgemein**, und suchen Sie nach **Signierung**. Wählen Sie das passende Team für Ihr Apple-Entwicklerkonto aus. Xcode sollte auf der Grundlage Ihrer Paket-ID automatisch das Bereitstellungsprofil abrufen, das Sie zuvor erstellt haben.
   
    Wenn das neue in Xcode erstellte Bereitstellungsprofil nicht angezeigt wird, versuchen Sie, die Profile für Ihre Signaturidentität zu aktualisieren. Klicken Sie in der Menüleiste auf **Xcode**, dann auf **Voreinstellungen**, auf die Registerkarte **Konto**, auf die Schaltfläche **Details anzeigen**, auf die Signaturidentität und anschließend rechts unten auf die Schaltfläche „Aktualisieren“.

    ![Xcode – Bereitstellungsprofil][9]

4. Vergewissern Sie sich auf der Registerkarte **Funktionen**, dass Pushbenachrichtigungen aktiviert sind.

    ![Xcode – Pushfunktionen][12]
   
5. Laden Sie das [Microsoft Azure-Messaging-Framework] herunter, und entzippen Sie die Datei. Klicken Sie in Xcode mit der rechten Maustaste auf Ihr Projekt, und klicken Sie dann auf die Option **Add Files to** zum Hinzufügen des Ordners **WindowsAzureMessaging.framework** zum Xcode-Projekt. Klicken Sie auf **Optionen**, vergewissern Sie sich, dass die Option **Copy items if needed** (Elemente kopieren, wenn nötig) aktiviert ist, und klicken Sie dann auf **Hinzufügen**.

    ![Azure SDK entzippen][10]

6. Fügen Sie Ihrem Projekt eine neue Headerdatei namens **HubInfo.h** hinzu. Diese Datei enthält die Konstanten für Ihren Notification Hub. Fügen Sie die folgenden Definitionen hinzu, und ersetzen Sie die Platzhalter für Zeichenfolgenliterale durch Ihren *Hubnamen* und den zuvor notierten Wert für *DefaultListenSharedAccessSignature*.

    ```obj-c
        #ifndef HubInfo_h
        #define HubInfo_h
   
            #define HUBNAME @"<Enter the name of your hub>"
            #define HUBLISTENACCESS @"<Enter your DefaultListenSharedAccess connection string"
   
        #endif /* HubInfo_h */
    ```
    
7. Öffnen Sie die Datei **AppDelegate.h**, und fügen Sie die folgenden Importdirektiven hinzu:

    ```obj-c
        #import <WindowsAzureMessaging/WindowsAzureMessaging.h>
        #import <UserNotifications/UserNotifications.h> 
        #import "HubInfo.h"
    ```
8. Fügen Sie in der Datei **AppDelegate.m** der Methode **didFinishLaunchingWithOptions** die folgenden Codezeilen hinzu (abhängig von Ihrer iOS-Version). Dieser Code registriert Ihr Gerätehandle bei APNS:

    ```obj-c
        UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
            UIUserNotificationTypeAlert | UIUserNotificationTypeBadge categories:nil];
   
        [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
        [[UIApplication sharedApplication] registerForRemoteNotifications];
    ```
   
9. Fügen Sie in der gleichen Datei die folgenden Methoden hinzu:

    ```obj-c
         - (void) application:(UIApplication *) application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *) deviceToken {
           SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:HUBLISTENACCESS
                                        notificationHubPath:HUBNAME];
   
            [hub registerNativeWithDeviceToken:deviceToken tags:nil completion:^(NSError* error) {
               if (error != nil) {
                   NSLog(@"Error registering for notifications: %@", error);
                }
                else {
                   [self MessageBox:@"Registration Status" message:@"Registered"];
              }
          }];
         }
   
        -(void)MessageBox:(NSString *) title message:(NSString *)messageText
        {
         UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }
    ```

    Mit diesem Code wird eine Verbindung mit dem Notification Hub mithilfe der in „HubInfo.h“ angegebenen Verbindungsinformationen hergestellt. Dieser Code übergibt das Gerätetoken an den Notification Hub, sodass dieser Benachrichtigungen senden kann.

10. Fügen Sie in derselben Datei folgende Methode hinzu, um einen **UIAlert** anzuzeigen, wenn die Benachrichtigung empfangen wird, während die App aktiv ist:

    ```obj-c
            - (void)application:(UIApplication *)application didReceiveRemoteNotification: (NSDictionary *)userInfo {
               NSLog(@"%@", userInfo);
               [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
           }
    ```

11. Erstellen Sie die App auf Ihrem Gerät, und führen Sie sie dort aus, um Fehler auszuschließen.

## <a name="send-test-push-notifications"></a>Senden von Test-Pushbenachrichtigungen
Mit der Option *Testsendevorgang* im [Azure-Portal] können Sie den Empfang von Benachrichtigungen in Ihrer App testen. Diese Option sendet zu Testzwecken eine Pushbenachrichtigung an Ihr Gerät.

![Azure-Portal: Testsendevorgang][30]

[!INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]


## <a name="checking-if-your-app-can-receive-push-notifications"></a>Überprüfen, ob die App Pushbenachrichtigungen empfangen kann
Zum Testen von Pushbenachrichtigungen unter iOS müssen Sie die App auf einem physischen iOS-Gerät bereitstellen. Sie können keine Apple-Pushbenachrichtigungen mit dem iOS-Simulator senden.

1. Führen Sie die App aus, und überprüfen Sie, ob die Registrierung erfolgreich ist. Drücken Sie dann **OK**.
   
    ![Registrierungstest für iOS-App-Pushbenachrichtigungen][33]
2. Als Nächstes senden Sie eine Pushbenachrichtigung zu Testzwecken über das [Azure-Portal] (wie oben beschrieben). 

3. Die Pushbenachrichtigung wird an alle Geräte gesendet, die dafür registriert sind, die Benachrichtigungen vom jeweiligen Notification Hub zu erhalten.
   
    ![iOS-App-Pushbenachrichtigung – Testempfang][35]

## <a name="next-steps"></a>Nächste Schritte
In diesem einfachen Beispiel haben Sie Pushbenachrichtigungen an alle Ihre registrierten iOS-Geräte übertragen. Wir empfehlen, als Nächstes mit dem Tutorial [Azure Notification Hubs – Benachrichtigen von iOS-Benutzern über .NET-Back-End] zu beginnen. Darin erfahren Sie Schritt für Schritt, wie Sie ein Back-End zum Senden von Pushbenachrichtigungen unter Verwendung von Tags konfigurieren. 

Wenn Sie Ihre Benutzer in Interessengruppen einteilen möchten, finden Sie im Tutorial [Verwenden von Notification Hubs zum Übermitteln von Nachrichten] hierzu weitere Informationen. 

Allgemeine Informationen zu Notification Hubs finden Sie im [Notification Hubs-Leitfaden].

<!-- Images. -->

[6]: ./media/notification-hubs-ios-get-started/notification-hubs-apple-config.png
[7]: ./media/notification-hubs-ios-get-started/notification-hubs-apple-config-cert.png
[8]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app.png
[9]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app2.png
[10]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app3.png
[11]: ./media/notification-hubs-ios-get-started/notification-hubs-xcode-product-name.png
[12]: ./media/notification-hubs-ios-get-started/notification-hubs-enable-push.png

[30]: ./media/notification-hubs-ios-get-started/notification-hubs-test-send.png

[31]: ./media/notification-hubs-ios-get-started/notification-hubs-ios-ui.png
[32]: ./media/notification-hubs-ios-get-started/notification-hubs-storyboard-view.png
[33]: ./media/notification-hubs-ios-get-started/notification-hubs-test1.png
[35]: ./media/notification-hubs-ios-get-started/notification-hubs-test3.png



<!-- URLs. -->
[Microsoft Azure-Messaging-Framework]: http://go.microsoft.com/fwlink/?LinkID=799698&clcid=0x409
[Mobile Services iOS SDK]: http://go.microsoft.com/fwLink/?LinkID=266533
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Notification Hubs-Leitfaden]: http://msdn.microsoft.com/library/jj927170.aspx
[Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456

[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-ios-get-started-push.md
[Azure Notification Hubs – Benachrichtigen von iOS-Benutzern über .NET-Back-End]: notification-hubs-aspnet-backend-ios-apple-apns-notification.md
[Verwenden von Notification Hubs zum Übermitteln von Nachrichten]: notification-hubs-ios-xplat-segmented-apns-push-notification.md

[Local and Push Notification Programming Guide]: http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW1
[Azure-Portal]: https://portal.azure.com
