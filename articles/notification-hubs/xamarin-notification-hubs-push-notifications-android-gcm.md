---
title: Senden von Pushbenachrichtigungen an Xamarin.Android-Apps mit Azure Notification Hubs | Microsoft-Dokumentation
description: In diesem Lernprogramm erfahren Sie, wie Sie mithilfe von Azure Notification Hubs eine Pushbenachrichtigung an eine Xamarin.Android-App senden.
author: dimazaid
manager: kpiteira
editor: spelluru
services: notification-hubs
documentationcenter: xamarin
ms.assetid: 0be600fe-d5f3-43a5-9e5e-3135c9743e54
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: tutorial
ms.custom: mvc
ms.date: 04/14/2018
ms.author: dimazaid
ms.openlocfilehash: 4c537b06c4ff50d90d9fd5a847b378038f252790
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38972273"
---
# <a name="tutorial-push-notifications-to-xamarinandroid-apps-using-azure-notification-hubs"></a>Tutorial: Senden von Pushbenachrichtigungen an Xamarin.Android-Apps mit Azure Notification Hubs
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Übersicht
In diesem Lernprogramm erfahren Sie, wie Sie mithilfe von Azure Notification Hubs eine Pushbenachrichtigung an eine Xamarin.Android-App senden. Sie erstellen eine leere Xamarin.Android-App, die Pushbenachrichtigungen mithilfe von Firebase Cloud Messaging (FCM) empfängt. Sie können über Ihren Notification Hub Pushbenachrichtigungen an alle Geräte senden, die Ihre App ausführen. Der komplette Code ist in der [NotificationHubs-Beispiel-App][GitHub] verfügbar.

In diesem Tutorial führen Sie die folgenden Schritte aus:

> [!div class="checklist"]
> * Erstellen eines Firebase-Projekts und Aktivieren von Firebase Cloud Messaging
> * Erstellen eines Notification Hubs
> * Erstellen einer Xamarin.Android-App und Herstellen einer Verbindung mit dem Notification Hub
> * Senden von Testbenachrichtigungen im Azure-Portal

## <a name="prerequisites"></a>Voraussetzungen

- **Azure-Abonnement**. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.
- [Visual Studio mit Xamarin] unter Windows oder [Visual Studio für Mac] unter OS X.
- Ein aktives Google-Konto

## <a name="create-a-firebase-project-and-enable-firebase-cloud-messaging"></a>Erstellen eines Firebase-Projekts und Aktivieren von Firebase Cloud Messaging
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="create-a-notification-hub"></a>Erstellen eines Notification Hubs
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

### <a name="configure-gcm-settings-for-the-notification-hub"></a>Konfigurieren von GCM-Einstellungen für den Notification Hub

1. Wählen Sie **Google (GCM)** im Abschnitt **BENACHRICHTIGUNGSEINSTELLUNGEN** aus. 
2. Geben Sie den **Legacy-Serverschlüssel** ein, den Sie in der Google Firebase-Konsole notiert haben. 
3. Wählen Sie auf der Symbolleiste **Speichern** aus. 

    ![](./media/notification-hubs-android-get-started/notification-hubs-gcm-api.png)

Der Notification Hub ist für die Arbeit mit FCM konfiguriert, und Sie besitzen die Verbindungszeichenfolgen, um die App für den Empfang von Benachrichtigungen zu registrieren und Pushbenachrichtigungen zu versenden.

## <a name="create-xamarinandroid-app-and-connect-it-to-notification-hub"></a>Erstellen einer Xamarin.Android-App und Herstellen einer Verbindung mit dem Notification Hub

### <a name="create-visual-studio-project-and-add-nuget-packages"></a>Erstellen eines Visual Studio-Projekts und Hinzufügen von NuGet-Paketen
1. Zeigen Sie in Visual Studio auf **Datei**, wählen Sie **Neu** und anschließend **Projekt** aus. 
   
      ![Visual Studio – Erstellen eines neuen Android-Projekts](./media/partner-xamarin-notification-hubs-android-get-started/new-project-dialog.png)
2. Erweitern Sie im **Projektmappen-Explorer**-Fenster **Eigenschaften**, und klicken Sie auf **AndroidManifest.xml**. Aktualisieren Sie den Paketnamen entsprechend dem Paketnamen, den Sie eingegeben haben, als Sie Ihrem Projekt in der Google Firebase-Konsole Firebase Cloud Messaging hinzugefügt haben. 

      ![Paketname in GCM](./media/partner-xamarin-notification-hubs-android-get-started/package-name-gcm.png)
3. Klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie **NuGet-Pakete verwalten...** aus. 
4. Wählen Sie die Registerkarte **Durchsuchen** aus. Suchen Sie nach **Xamarin.GooglePlayServices.Base**. Wählen Sie **Xamarin.GooglePlayServices.Base** in der Ergebnisliste aus. Wählen Sie dann **Installieren** aus. 

      ![Google Play Services NuGet](./media/partner-xamarin-notification-hubs-android-get-started/google-play-services-nuget.png)
5. Suchen Sie im Fenster **NuGet-Paket-Manager** nach **Xamarin.Firebase.Messaging**. Wählen Sie **Xamarin.Firebase.Messaging** in der Ergebnisliste aus. Wählen Sie dann **Installieren** aus. 
6. Suchen Sie jetzt nach **Xamarin.Azure.NotificationHubs.Android**. Wählen Sie **Xamarin.Azure.NotificationHubs.Android** in der Ergebnisliste aus. Wählen Sie dann **Installieren** aus. 

### <a name="add-the-google-services-json-file"></a>Hinzufügen der JSON-Datei von Google Services

1. Kopieren Sie die Datei **google-services.json**, die Sie aus der Google Firebase-Konsole in den Projektordner heruntergeladen haben.
2. Fügen Sie **google-services.json** dem Projekt hinzu.
3. Wählen Sie **google-services.json** im **Projektmappen-Explorer**-Fenster aus.
4. Legen Sie im Bereich **Eigenschaften** die Buildaktion auf **GoogleServicesJson** fest. Wenn **GoogleServicesJson** nicht angezeigt wird, schließen Sie Visual Studio, starten Sie es neu, öffnen Sie das Projekt erneut, und versuchen Sie es erneut. 

      ![Buildaktion GoogleServicesJson](./media/partner-xamarin-notification-hubs-android-get-started/google-services-json-build-action.png)

### <a name="set-up-notification-hubs-in-your-project"></a>Einrichten von Notification Hubs in Ihrem Projekt

#### <a name="registering-with-firebase-cloud-messaging"></a>Registrieren bei Firebase Cloud Messaging

Öffnen Sie die Datei **AndroidManifest.xml**, und fügen Sie die folgenden `<receiver>`-Elemente in das `<application>`-Element ein:

        <receiver android:name="com.google.firebase.iid.FirebaseInstanceIdInternalReceiver" android:exported="false" />
        <receiver android:name="com.google.firebase.iid.FirebaseInstanceIdReceiver" android:exported="true" android:permission="com.google.android.c2dm.permission.SEND">
          <intent-filter>
            <action android:name="com.google.android.c2dm.intent.RECEIVE" />
            <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
            <category android:name="${applicationId}" />
          </intent-filter>
        </receiver>

1. Erfassen Sie für Ihre Android-App und den Notification Hub folgende Informationen:
   
   * **Verbindungszeichenfolge für Überwachung**: Klicken Sie im Dashboard des [Azure-Portal] auf **Verbindungszeichenfolgen anzeigen**. Kopieren Sie die Verbindungszeichenfolge *DefaultListenSharedAccessSignature* für diesen Wert.
   * **Hub-Name**: Der Name des Hubs aus dem [Azure-Portal]. Beispiel: *mynotificationhub2*.
     
2. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das **Projekt**, zeigen Sie auf **Hinzufügen**, und wählen Sie **Klasse** aus. 
4. Erstellen Sie eine **Constants.cs** -Klasse für Ihr Xamarin-Projekt und definieren Sie die folgenden Konstantenwerte in der Klasse. Ersetzen Sie die Platzhalter durch Ihre Werte.
    
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
7. Erstellen Sie eine neue Klasse **MyFirebaseIIDService**, wie Sie die **Constants**-Klasse erstellt haben. 
8. Fügen Sie **MyFirebaseIIDService.cs** die folgenden using-Anweisungen hinzu:
   
    ```csharp
    using Android.App;
    using Android.Util;
    using WindowsAzure.Messaging;
    using Firebase.Iid;
    ```

9. Fügen Sie in **MyFirebaseIIDService.cs** die folgende **Klassen**-Deklaration hinzu, und legen Sie fest, dass Ihre Klasse von **FirebaseInstanceIdService** erbt:
   
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
        using Android.App;
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
15. **Erstellen** Sie Ihr Projekt. 
16. **Ausführen** Ihrer App auf Ihrem Gerät oder im geladenen Emulator

## <a name="send-test-notification-from-the-azure-portal"></a>Senden einer Testbenachrichtigung im Azure-Portal
Mit der Option *Testsendevorgang* im [Azure-Portal] können Sie den Empfang von Benachrichtigungen in Ihrer App testen. Diese Option sendet zu Testzwecken eine Pushbenachrichtigung an Ihr Gerät.

![Azure-Portal – Testsendung](media/partner-xamarin-notification-hubs-android-get-started/send-test-notification.png)

Pushbenachrichtigungen werden normalerweise in einem Back-End-Dienst, z.B. Mobile Services oder ASP.NET, über eine kompatible Bibliothek gesendet. Falls für Ihr Back-End keine Bibliothek verfügbar ist, können Sie Benachrichtigungsmeldungen auch direkt über die REST-API senden.

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie Broadcastbenachrichtigungen an alle Android-Geräte gesendet, die beim Back-End registriert sind. Um zu erfahren, wie Sie Pushbenachrichtigungen an bestimmte Android-Geräte senden, fahren Sie mit dem folgenden Tutorial fort: 

> [!div class="nextstepaction"]
>[Senden von Pushbenachrichtigungen an bestimmte Geräte](notification-hubs-aspnet-backend-android-xplat-segmented-gcm-push-notification.md)


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

[Azure-Portal]: https://portal.azure.com/
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Android]: http://msdn.microsoft.com/library/dn282661.aspx

[Use Notification Hubs to push notifications to users]: /manage/services/notification-hubs/notify-users-aspnet
[Use Notification Hubs to send breaking news]: /manage/services/notification-hubs/breaking-news-dotnet
[GitHub]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/Xamarin/GetStartedXamarinAndroid
