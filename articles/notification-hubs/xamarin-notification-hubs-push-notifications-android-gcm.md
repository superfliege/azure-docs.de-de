---
title: "Erste Schritte mit Notification Hubs für Xamarin.Android-Apps | Microsoft Docs"
description: In diesem Lernprogramm erfahren Sie, wie Sie mithilfe von Azure Notification Hubs eine Pushbenachrichtigung an eine Xamarin.Android-App senden.
author: jwhitedev
manager: kpiteira
editor: 
services: notification-hubs
documentationcenter: xamarin
ms.assetid: 0be600fe-d5f3-43a5-9e5e-3135c9743e54
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 12/22/2017
ms.author: jawh
ms.openlocfilehash: 1cb6fbc82c493e17815dc60ddcff183a47513bc6
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2018
---
# <a name="get-started-with-notification-hubs-for-xamarinandroid-apps"></a>Erste Schritte mit Notification Hubs für Xamarin.Android-Apps
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Übersicht
In diesem Lernprogramm erfahren Sie, wie Sie mithilfe von Azure Notification Hubs eine Pushbenachrichtigung an eine Xamarin.Android-App senden. Sie erstellen eine leere Xamarin.Android-App, die Pushbenachrichtigungen mithilfe von Firebase Cloud Messaging (FCM) empfängt. Sobald Sie dieses Lernprogramm abgeschlossen haben, können Sie über Ihren Notification Hub Pushbenachrichtigungen an alle Geräte senden, die Ihre App ausführen. Der komplette Code ist in der [NotificationHubs-Beispiel-App][GitHub] verfügbar.

Das Lernprogramm zeigt ein einfaches Übertragungsszenario mithilfe von Notification Hubs.

## <a name="before-you-begin"></a>Voraussetzungen
[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Den vollständigen Code für dieses Tutorial finden Sie [auf GitHub][GitHub].

## <a name="prerequisites"></a>Voraussetzungen
Für dieses Lernprogramm ist Folgendes erforderlich:

* [Visual Studio mit Xamarin] unter Windows oder [Visual Studio für Mac] unter OS X.
* Ein aktives Google-Konto

Das Abschließen dieses Lernprogramms ist eine Voraussetzung für alle anderen Notification Hubs-Lernprogramme für Xamarin.Android-Apps.

> [!IMPORTANT]
> Sie benötigen ein aktives Azure-Konto, um dieses Lernprogramm abzuschließen. Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A9C9624B5&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-android-get-started%2F).
> 
> 

## <a name="enable-firebase-cloud-messaging"></a>Aktivieren von Firebase Cloud Messaging
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-your-notification-hub"></a>Konfigurieren Ihres Notification Hub
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">

<li><p>Klicken Sie auf die Registerkarte <b>Konfigurieren</b>, geben Sie den Wert für den <b>API-Schlüssel</b> ein, den Sie im vorherigen Abschnitt erhalten haben, und klicken Sie auf <b>Speichern</b>.</p>
</li>
</ol>
&emsp;&emsp;![](./media/notification-hubs-android-get-started/notification-hubs-gcm-api.png)

Der Notification Hub ist für die Arbeit mit FCM konfiguriert, und Sie besitzen die Verbindungszeichenfolgen, um die App für den Empfang von Benachrichtigungen zu registrieren und Pushbenachrichtigungen zu versenden.

## <a name="connect-your-app-to-the-notification-hub"></a>Verbinden Ihrer App mit dem Notification Hub
Erstellen Sie zunächst ein neues Projekt. 

1. Klicken Sie in Visual Studio auf **Neue Projektmappe** > **Android-App** und anschließend auf **Weiter**.
   
      ![Visual Studio – Erstellen eines neuen Android-Projekts][22]

2. Geben Sie die Werte für **App-Name** und **Bezeichner** ein. Wählen Sie die **Zielplattformen** aus, die Sie unterstützen möchten, und klicken Sie dann auf **Weiter** > **Erstellen**.
   
      ![Visual Studio – Android-App-Konfiguration][23]

    Dadurch wird ein neues Android-Projekt erstellt.

3. Klicken Sie in der Lösungsansicht mit der rechten Maustaste auf Ihr neues Projekt, und wählen Sie **Options**aus, um die Projekteigenschaften zu öffnen. Wählen Sie im Bereich **Build** das Element **Android Application** aus.
   
    Ihr **Package name** muss mit einem Kleinbuchstaben beginnen.
   
   > [!IMPORTANT]
   > Der erste Buchstabe im Paketnamen muss ein Kleinbuchstabe sein. Andernfalls erhalten Sie Anwendungsmanifestfehler, wenn Sie Ihre App weiter unten für Pushbenachrichtigungen registrieren.
   > 
   > 
   
      ![Visual Studio – Android-Projektoptionen][24]
4. Legen Sie optional die **Minimum Android version** auf einen anderen API Level fest.
5. Legen Sie optional die **Target Android version** auf Ihre gewünschte Zielversion der API fest (API Level 8 oder höher).
6. Klicken Sie auf **OK**, um das Dialogfeld mit den Projektoptionen zu schließen.

### <a name="add-the-required-packages-to-your-project"></a>Hinzufügen der erforderlichen Pakete zu Ihrem Projekt

1. Klicken Sie mit der rechten Maustaste auf Ihr Projekt, und wählen Sie **Hinzufügen** > **NuGet-Pakete hinzufügen**.
2. Suchen Sie nach **Xamarin.Azure.NotificationHubs.Android**, und fügen Sie das Element dem Projekt hinzu.
3. Suchen Sie nach **Xamarin.Firebase.Messaging**, und fügen Sie das Element dem Projekt hinzu.

### <a name="set-up-notification-hubs-in-your-project"></a>Einrichten von Notification Hubs in Ihrem Projekt
1. Erfassen Sie für Ihre Android-App und den Notification Hub folgende Informationen:
   
   * **Verbindungszeichenfolge für Überwachung**: Klicken Sie im Dashboard des [Azure-Portals] auf **Verbindungszeichenfolgen anzeigen**. Kopieren Sie die Verbindungszeichenfolge *DefaultListenSharedAccessSignature* für diesen Wert.
   * **Hub-Name**: Dies ist der Name des Hubs aus dem [Azure-Portals]. Beispiel: *mynotificationhub2*.
     
2. Erstellen Sie eine **Constants.cs** -Klasse für Ihr Xamarin-Projekt und definieren Sie die folgenden Konstantenwerte in der Klasse. Ersetzen Sie die Platzhalter durch Ihre Werte.
    
    ```csharp
        public static class Constants
        {
           public const string ListenConnectionString = "<Listen connection string>";
           public const string NotificationHubName = "<hub name>";
        }
    ```

3. Fügen Sie die folgenden using-Anweisungen zu **MainActivity.cs**hinzu:
   
    ```csharp
        using Android.Util;
    ```

4. Fügen Sie **MainActivity.cs** eine Instanzvariable hinzu, die bei laufender App zum Anzeigen eines Warndialogfelds verwendet wird:
   
    ```csharp
        public const string TAG = "MainActivity";
    ```

5. Fügen Sie `OnCreate` in **MainActivity.cs** nach dem Element `base.OnCreate(savedInstanceState)` den folgenden Code hinzu:

    ```csharp   
        if (Intent.Extras != null)
        {
            foreach (var key in Intent.Extras.KeySet())
            {
                if(key!=null)
                {
                    var value = Intent.Extras.GetString(key);
                    Log.Debug(TAG, "Key: {0} Value: {1}", key, value);
                }
            }
        }
    ```

6. Klicken Sie mit der rechten Maustaste auf Ihr Projekt, und fügen Sie die Datei `google-services.json` hinzu, die Sie zuvor aus Ihrem Firebase-Projekt heruntergeladen haben. Klicken Sie mit der rechten Maustaste auf die hinzugefügte Datei, und legen Sie den Buildvorgang auf `GoogleServicesJson` fest.

    ![Visual Studio – Konfigurieren von „google-services.json“][25]

7. Erstellen Sie die neue Klasse **MyFirebaseIIDService**.

8. Fügen Sie **MyFirebaseIIDService.cs** die folgenden using-Anweisungen hinzu:
   
    ```csharp
        using System;
        using Android.App;
        using Firebase.Iid;
        using Android.Util;
        using WindowsAzure.Messaging;
        using System.Collections.Generic;
    ```

9. Fügen Sie in **MyFirebaseIIDService.cs** oberhalb der **class**-Deklaration Folgendes hinzu, und legen Sie fest, dass Ihre Klasse von **FirebaseInstanceIdService** erbt:
   
    ```csharp
        [Service]
        [IntentFilter(new[] { "com.google.firebase.INSTANCE_ID_EVENT" })]
        public class MyFirebaseIIDService : FirebaseInstanceIdService
    ```

10. Fügen Sie in **MyFirebaseIIDService.cs** den folgenden Code hinzu:
   
    ```csharp
        const string TAG = "MyFirebaseIIDService";
        NotificationHub hub;

        public override void OnTokenRefresh()
        {
            var refreshedToken = FirebaseInstanceId.Instance.Token;
            Log.Debug(TAG, "FCM token: " + refreshedToken);
            SendRegistrationToServer(refreshedToken);
        }

        void SendRegistrationToServer(string token)
        {
            // Register with Notification Hubs
            hub = new NotificationHub(Constants.NotificationHubName,
                                      Constants.ListenConnectionString, this);

            var tags = new List<string>() { };
            var regID = hub.Register(token, tags.ToArray()).RegistrationId;

            Log.Debug(TAG, $"Successful registration of ID {regID}");
        }
    ```

11. Erstellen Sie eine weitere neue Klasse für Ihr Projekt, und geben Sie ihr den Namen **MyFirebaseMessagingService**.

12. Fügen Sie **MyFirebaseMessagingService.cs** die folgenden using-Anweisungen hinzu.
    
    ```csharp
        using System;
        using System.Linq;
        using Android;
        using Android.App;
        using Android.Content;
        using Android.Util;
        using Firebase.Messaging;
    ```

13. Fügen Sie oberhalb Ihrer Klassendeklaration Folgendes hinzu, und legen Sie fest, dass Ihre Klasse von **FirebaseMessagingService** erbt:
    
    ```csharp
        [Service]
        [IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
        public class MyFirebaseMessagingService : FirebaseMessagingService
    ```
    
14. Fügen Sie **MyFirebaseMessagingService.cs** den folgenden Code hinzu:
    
    ```csharp
        const string TAG = "MyFirebaseMsgService";
        public override void OnMessageReceived(RemoteMessage message)
        {
            Log.Debug(TAG, "From: " + message.From);
            if(message.GetNotification()!= null)
            {
                //These is how most messages will be received
                Log.Debug(TAG, "Notification Message Body: " + message.GetNotification().Body);
                SendNotification(message.GetNotification().Body);
            }
            else 
            {
                //Only used for debugging payloads sent from the Azure portal
                SendNotification(message.Data.Values.First());

            }

        }

        void SendNotification(string messageBody)
        {
            var intent = new Intent(this, typeof(MainActivity));
            intent.AddFlags(ActivityFlags.ClearTop);
            var pendingIntent = PendingIntent.GetActivity(this, 0, intent, PendingIntentFlags.OneShot);

            var notificationBuilder = new Notification.Builder(this)
                        .SetContentTitle("FCM Message")
                        .SetSmallIcon(Resource.Drawable.ic_launcher)
                        .SetContentText(messageBody)
                        .SetAutoCancel(true)
                        .SetContentIntent(pendingIntent);

            var notificationManager = NotificationManager.FromContext(this);

            notificationManager.Notify(0, notificationBuilder.Build());
        }
    ```

15. Führen Sie Ihre App auf Ihrem Gerät oder im geladenen Emulator aus.

## <a name="send-notifications-from-the-portal"></a>Senden von Benachrichtigungen über das Portal
Mit der Option *Testsendevorgang* im [Azure-Portals] können Sie den Empfang von Benachrichtigungen in Ihrer App testen. Diese Option sendet zu Testzwecken eine Pushbenachrichtigung an Ihr Gerät.

![Azure-Portal – Testsendung][30]

Pushbenachrichtigungen werden normalerweise in einem Back-End-Dienst, z.B. Mobile Services oder ASP.NET, über eine kompatible Bibliothek gesendet. Falls für Ihr Back-End keine Bibliothek verfügbar ist, können Sie Benachrichtigungsmeldungen auch direkt über die REST-API senden.

Es folgen einige andere Lernprogramme, die Informationen zum Senden von Benachrichtigungen enthalten:

* ASP.NET: Siehe [Benachrichtigen von Benutzern mit Benachrichtigungshubs].
* Azure Notification Hubs Java-SDK: Informationen zum Senden von Benachrichtigungen über Java finden Sie unter [Verwenden von Notification Hubs von Java aus](notification-hubs-java-push-notification-tutorial.md) . Dies wurde für die Android-Entwicklung in Eclipse getestet.
* PHP: Siehe [Verwenden von Notification Hubs von PHP aus](notification-hubs-php-push-notification-tutorial.md).

## <a name="next-steps"></a>Nächste Schritte
In diesem einfachen Beispiel haben Sie Benachrichtigungen an alle Android-Geräte versendet. Informationen zum Adressieren bestimmter Benutzer finden Sie in dem Lernprogramm [Benachrichtigen von Benutzern mit Benachrichtigungshubs]. Wenn Sie Ihre Benutzer in Interessengruppen einteilen möchten, finden Sie unter [Verwenden von Benachrichtigungshubs zum Übermitteln von Nachrichten]weitere Informationen. Mehr zur Verwendung von Notification Hubs erfahren Sie im [Notification Hubs-Leitfaden] und unter [Notification Hubs für Android].

<!-- Anchors. -->
[Enable Google Cloud Messaging]: #register
[Configure your Notification Hub]: #configure-hub
[Connecting your app to the Notification Hub]: #connecting-app
[Run your app with the emulator]: #run-app
[Send notifications from your back-end]: #send
[Next steps]:#next-steps

<!-- Images. -->

[11]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-configure-android.png

[13]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-app1.png
[15]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-app3.png

[18]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-android-app7.png
[19]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-android-app8.png

[20]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-console-app.png
[21]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-android-toast.png
[22]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project1.png
[23]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project2.png
[24]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-xamarin-android-app-options.png
[25]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-google-services-json.png

[30]: ./media/notification-hubs-android-get-started/notification-hubs-test-send.png


<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-xamarin-android/#create-new-service
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js
[Visual Studio mit Xamarin]: https://docs.microsoft.com/visualstudio/install/install-visual-studio
[Visual Studio für Mac]: https://www.visualstudio.com/vs/visual-studio-mac/

[Azure-Portals]: https://portal.azure.com/
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs-Leitfaden]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs für Android]: http://msdn.microsoft.com/library/dn282661.aspx

[Benachrichtigen von Benutzern mit Benachrichtigungshubs]: /manage/services/notification-hubs/notify-users-aspnet
[Verwenden von Benachrichtigungshubs zum Übermitteln von Nachrichten]: /manage/services/notification-hubs/breaking-news-dotnet
[GitHub]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/Xamarin/GetStartedXamarinAndroid
