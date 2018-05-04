---
title: Erste Schritte mit Azure Notification Hubs für Android-Apps und Firebase Cloud Messaging | Microsoft-Dokumentation
description: In diesem Lernprogramm erfahren Sie, wie Sie mithilfe von Azure Notification Hubs und Firebase Cloud Messaging Pushbenachrichtigungen an Android-Geräte senden.
services: notification-hubs
documentationcenter: android
keywords: Pushbenachrichtigungen,Pushbenachrichtigung,Android-Pushbenachrichtigung,FCM,Firebase Cloud Messaging
author: jwhitedev
manager: kpiteira
editor: ''
ms.assetid: 02298560-da61-4bbb-b07c-e79bd520e420
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: hero-article
ms.date: 12/22/2017
ms.author: jawh
ms.openlocfilehash: 6768b3e55af5e497c9424bc09130c9dd0948968b
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
---
# <a name="get-started-with-azure-notification-hubs-for-android-apps-and-firebase-cloud-messaging"></a>Erste Schritte mit Azure Notification Hubs für Android-Apps und Firebase Cloud Messaging
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Übersicht
> [!IMPORTANT]
> In diesem Artikel werden Pushbenachrichtigungen mit Google Firebase Cloud Messaging (FCM) veranschaulicht. Falls Sie noch Google Cloud Messaging (GCM) verwenden, helfen Ihnen die Informationen unter [Sending push notifications to Android with Azure Notification Hubs and GCM](notification-hubs-android-push-notification-google-gcm-get-started.md)(Senden von Pushbenachrichtigungen an Android mit Azure Notification Hubs und GCM) weiter.
> 
> 

In diesem Tutorial erfahren Sie, wie Sie mithilfe von Azure Notification Hubs und Firebase Cloud Messaging Pushbenachrichtigungen an eine Android-Anwendung senden. In diesem Tutorial erstellen Sie eine leere Android-App, die Pushbenachrichtigungen mithilfe von Firebase Cloud Messaging (FCM) empfängt.

[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Den vollständigen Code für dieses Tutorial können Sie [hier](https://github.com/Azure/azure-notificationhubs-samples/tree/master/Android/GetStartedFirebase)bei GitHub herunterladen.

## <a name="prerequisites"></a>Voraussetzungen
> [!IMPORTANT]
> Sie benötigen ein aktives Azure-Konto, um dieses Lernprogramm abzuschließen. Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-android-get-started).
> 
> 

* Zusätzlich zum oben erwähnten aktiven Azure-Konto ist für dieses Tutorial die aktuelle Version von [Android Studio](http://go.microsoft.com/fwlink/?LinkId=389797)erforderlich.
* Android 2.3 oder höher für Firebase Cloud Messaging.
* Google Repository Version 27 oder höher ist für Firebase Cloud Messaging erforderlich.
* Google Play Services 9.0.2 oder höher für Firebase Cloud Messaging.
* Das Abschließen dieses Tutorial ist eine Voraussetzung für alle anderen Notification Hubs-Tutorials für Android-Apps.

## <a name="create-a-new-android-studio-project"></a>Erstellen eines neuen Android Studio-Projekts
1. Starten Sie in Android Studio ein neues Android Studio-Projekt.
   
    ![Android Studio – Neues Projekt](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-new-project.png)
2. Wählen Sie den Formfaktor **Phone and Tablet** und das **Minimum SDK** aus, das unterstützt werden soll. Klicken Sie auf **Weiter**.
   
    ![Android Studio – Projekterstellungsworkflow](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-choose-form-factor.png)
3. Wählen Sie **Empty Activity** als Hauptaktivität aus, und klicken Sie auf **Weiter** und anschließend auf **Fertig stellen**.

## <a name="create-a-project-that-supports-firebase-cloud-messaging"></a>Erstellen eines Projekts, das Firebase Cloud Messaging unterstützt
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-new-notification-hub"></a>Konfigurieren eines neuen Notification Hubs
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

&emsp;&emsp;6. Wählen Sie unter **Notification Services** die Option **Google (GCM)** aus. Geben Sie den FCM-Serverschlüssel ein, den Sie zuvor in der [Firebase-Konsole](https://firebase.google.com/console/) kopiert haben, und klicken Sie auf **Save**.

&emsp;&emsp;![Azure Notification Hubs – Google (GCM)](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-gcm-api.png)

Der Notification Hub ist jetzt für die Arbeit mit Firebase Cloud Messaging konfiguriert, und Sie besitzen die Verbindungszeichenfolge, um die App für den Empfang und das Senden von Pushbenachrichtigungen zu registrieren.

## <a id="connecting-app"></a>Verbinden Ihrer App mit dem Notification Hub
### <a name="add-google-play-services-to-the-project"></a>Hinzufügen von Google Play Services zum Projekt
[!INCLUDE [Add Play Services](../../includes/notification-hubs-android-studio-add-google-play-services.md)]

### <a name="adding-azure-notification-hubs-libraries"></a>Hinzufügen von Azure Notification Hubs-Bibliotheken
1. Fügen Sie in der Datei `Build.Gradle` für die **App** die folgenden Zeilen im Abschnitt **dependencies** hinzu.
   
    ```java
        compile 'com.microsoft.azure:notification-hubs-android-sdk:0.4@aar'
        compile 'com.microsoft.azure:azure-notifications-handler:1.0.1@aar'
    ```

2. Fügen Sie das folgende Repository nach dem Abschnitt **dependencies** hinzu.
   
    ```java
        repositories {
            maven {
                url "http://dl.bintray.com/microsoftazuremobile/SDK"
            }
        }
    ```

### <a name="updating-the-androidmanifestxml"></a>Aktualisieren Sie die Datei „AndroidManifest.xml“.
1. Für die Unterstützung von FCM müssen Sie in Ihrem Code einen Instanz-ID-Listenerdienst implementieren, der zum [Beschaffen von Registrierungstoken](https://firebase.google.com/docs/cloud-messaging/android/client#sample-register) mit der [FirebaseInstanceId-API von Google](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId) verwendet wird. In diesem Tutorial lautet der Name der Klasse `MyInstanceIDService`. 
   
    Fügen Sie der Datei „AndroidManifest.xml“ im Tag `<application>` die unten angegebene Dienstdefinition hinzu. 
   
    ```xml
        <service android:name=".MyInstanceIDService">
            <intent-filter>
                <action android:name="com.google.firebase.INSTANCE_ID_EVENT"/>
            </intent-filter>
        </service>
    ```

2. Nachdem Sie Ihr FCM-Registrierungstoken von der FirebaseInstanceId-API erhalten haben, verwenden Sie es zum [Registrieren beim Azure Notification Hub](notification-hubs-push-notification-registration-management.md). Sie unterstützen diese Registrierung im Hintergrund mit einem `IntentService`-Element mit dem Namen `RegistrationIntentService`. Dieser Dienst ist auch für die Aktualisierung Ihres FCM-Registrierungstokens zuständig.
   
    Fügen Sie der Datei „AndroidManifest.xml“ im Tag `<application>` die unten angegebene Dienstdefinition hinzu. 
   
    ```xml
        <service
            android:name=".RegistrationIntentService"
            android:exported="false">
        </service>
    ```

3. Außerdem müssen Sie einen Empfänger für die Benachrichtigungen definieren. Fügen Sie der Datei „AndroidManifest.xml“ im Tag `<application>` die unten angegebene Empfängerdefinition hinzu. Ersetzen Sie den Platzhalter `<your package>` durch Ihren tatsächlichen Namen des Pakets, der oben in der Datei `AndroidManifest.xml` angegeben ist.

    ```xml
        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
            android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<your package name>" />
            </intent-filter>
        </receiver>
    ```

4. Fügen Sie unterhalb des Tags `</application>` die folgenden erforderlichen FCM-bezogenen Berechtigungen hinzu. Ersetzen Sie dabei `<your package>` durch den oben in der Datei `AndroidManifest.xml` angegebenen Paketnamen.
   
    Weitere Informationen zu diesen Berechtigungen finden Sie unter [Setup a GCM Client app for Android](https://developers.google.com/cloud-messaging/android/client#manifest) (Einrichten einer GCM-Client-App für Android) und [Migrate a GCM Client App for Android to Firebase Cloud Messaging](https://developers.google.com/cloud-messaging/android/android-migrate-fcm#remove_the_permissions_required_by_gcm) (Migrieren einer GCM-Client-App für Android zu Firebase Cloud Messaging).
   
    ```xml
        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
    ```

### <a name="adding-code"></a>Hinzufügen von Code
1. Erweitern Sie in der Projektansicht die Knoten **app** > **src** > **main** > **java**. Klicken Sie mit der rechten Maustaste auf Ihren Paketordner unter **java**, klicken Sie auf **Neu** und dann auf **Java Class**. Fügen Sie eine neue Klasse namens `NotificationSettings`hinzu. 
   
    ![Android Studio – Neue Java-Klasse](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hub-android-new-class.png)
   
    Aktualisieren Sie diese drei Platzhalter im weiter unten angegebenen Code für die `NotificationSettings`-Klasse:
   
   * **SenderId**: Dies ist die Absender-ID, die Sie zuvor in den Projekteinstellungen der [Firebase-Konsole](https://firebase.google.com/console/) über die Registerkarte **Cloud Messaging** ermittelt haben.
   * **HubListenConnectionString**: Dies ist die **DefaultListenAccessSignature**-Verbindungszeichenfolge für Ihren Hub. Sie können diese Verbindungszeichenfolge kopieren, indem Sie im [Azure-Portal] für Ihren Hub auf **Zugriffsrichtlinien** klicken.
   * **HubName**: Verwenden Sie den Namen Ihres Notification Hub, der im [Azure-Portal] auf dem Hub-Blatt angezeigt wird.
     
     `NotificationSettings` -Code:
     
    ```java
       public class NotificationSettings {
     
           public static String SenderId = "<Your project number>";
           public static String HubName = "<Your HubName>";
           public static String HubListenConnectionString = "<Enter your DefaultListenSharedAccessSignature connection string>";
       }
    ```

2. Fügen Sie mit den obigen Schritten eine weitere neue Klasse mit dem Namen `MyInstanceIDService` hinzu. Das ist Ihre Implementierung des Instanz-ID-Listenerdiensts.
   
    Mit dem Code für diese Klasse wird Ihr `IntentService`-Element aufgerufen, um im Hintergrund [das FCM-Token zu aktualisieren](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens).
   
    ```java
        import android.content.Intent;
        import android.util.Log;
        import com.google.firebase.iid.FirebaseInstanceIdService;

        public class MyInstanceIDService extends FirebaseInstanceIdService {

            private static final String TAG = "MyInstanceIDService";

            @Override
            public void onTokenRefresh() {

                Log.d(TAG, "Refreshing GCM Registration Token");

                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        };
    ```

1. Fügen Sie dem Projekt eine weitere neue Klasse namens `RegistrationIntentService`hinzu. Dies ist die Implementierung für Ihr `IntentService`-Element zum [Aktualisieren des FCM-Tokens](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) und [Registrieren beim Notification Hub](notification-hubs-push-notification-registration-management.md).
   
    Verwenden Sie für diese Klasse den folgenden Code.
   
    ```java
        import android.app.IntentService;
        import android.content.Intent;
        import android.content.SharedPreferences;
        import android.preference.PreferenceManager;
        import android.util.Log;        
        import com.google.firebase.iid.FirebaseInstanceId;
        import com.microsoft.windowsazure.messaging.NotificationHub;
   
        public class RegistrationIntentService extends IntentService {
   
            private static final String TAG = "RegIntentService";
   
            private NotificationHub hub;
   
            public RegistrationIntentService() {
                super(TAG);
            }
   
            @Override
            protected void onHandleIntent(Intent intent) {
   
                SharedPreferences sharedPreferences = PreferenceManager.getDefaultSharedPreferences(this);
                String resultString = null;
                String regID = null;
                String storedToken = null;
   
                try {
                    String FCM_token = FirebaseInstanceId.getInstance().getToken();
                    Log.d(TAG, "FCM Registration Token: " + FCM_token);
   
                    // Storing the registration ID that indicates whether the generated token has been
                    // sent to your server. If it is not stored, send the token to your server,
                    // otherwise your server should have already received the token.
                    if (((regID=sharedPreferences.getString("registrationID", null)) == null)){
   
                        NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
                                NotificationSettings.HubListenConnectionString, this);
                        Log.d(TAG, "Attempting a new registration with NH using FCM token : " + FCM_token);
                        regID = hub.register(FCM_token).getRegistrationId();
   
                        // If you want to use tags...
                        // Refer to : https://azure.microsoft.com/documentation/articles/notification-hubs-routing-tag-expressions/
                        // regID = hub.register(token, "tag1,tag2").getRegistrationId();
   
                        resultString = "New NH Registration Successfully - RegId : " + regID;
                        Log.d(TAG, resultString);
   
                        sharedPreferences.edit().putString("registrationID", regID ).apply();
                        sharedPreferences.edit().putString("FCMtoken", FCM_token ).apply();
                    }
   
                    // Check if the token may have been compromised and needs refreshing.
                    else if ((storedToken=sharedPreferences.getString("FCMtoken", "")) != FCM_token) {
   
                        NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
                                NotificationSettings.HubListenConnectionString, this);
                        Log.d(TAG, "NH Registration refreshing with token : " + FCM_token);
                        regID = hub.register(FCM_token).getRegistrationId();
   
                        // If you want to use tags...
                        // Refer to : https://azure.microsoft.com/documentation/articles/notification-hubs-routing-tag-expressions/
                        // regID = hub.register(token, "tag1,tag2").getRegistrationId();
   
                        resultString = "New NH Registration Successfully - RegId : " + regID;
                        Log.d(TAG, resultString);
   
                        sharedPreferences.edit().putString("registrationID", regID ).apply();
                        sharedPreferences.edit().putString("FCMtoken", FCM_token ).apply();
                    }
   
                    else {
                        resultString = "Previously Registered Successfully - RegId : " + regID;
                    }
                } catch (Exception e) {
                    Log.e(TAG, resultString="Failed to complete registration", e);
                    // If an exception happens while fetching the new token or updating our registration data
                    // on a third-party server, this ensures that we'll attempt the update at a later time.
                }
   
                // Notify UI that registration has completed.
                if (MainActivity.isVisible) {
                    MainActivity.mainActivity.ToastNotify(resultString);
                }
            }
        }
    ```

2. Fügen Sie in der `MainActivity`-Klasse oberhalb der Klassendeklaration die folgenden `import`-Anweisungen hinzu.
   
    ```java
        import com.google.android.gms.common.ConnectionResult;
        import com.google.android.gms.common.GoogleApiAvailability;
        import com.microsoft.windowsazure.notifications.NotificationsManager;
        import android.content.Intent;
        import android.util.Log;
        import android.widget.TextView;
        import android.widget.Toast;
    ```
3. Fügen Sie folgende private Member oben in der Klasse hinzu. Sie verwenden diese Member zum [Überprüfen der Verfügbarkeit von Google Play Services gemäß Empfehlung von Google](https://developers.google.com/android/guides/setup#ensure_devices_have_the_google_play_services_apk).
   
    ```java
        public static MainActivity mainActivity;
        public static Boolean isVisible = false;    
        private static final String TAG = "MainActivity";
        private static final int PLAY_SERVICES_RESOLUTION_REQUEST = 9000;
    ```

4. Fügen Sie in der `MainActivity` -Klasse die folgende Methode zur Verfügbarkeit von Google Play Services hinzu. 
   
    ```java
        /**
        * Check the device to make sure it has the Google Play Services APK. If
        * it doesn't, display a dialog that allows users to download the APK from
        * the Google Play Store or enable it in the device's system settings.
        */
    
        private boolean checkPlayServices() {
            GoogleApiAvailability apiAvailability = GoogleApiAvailability.getInstance();
            int resultCode = apiAvailability.isGooglePlayServicesAvailable(this);
            if (resultCode != ConnectionResult.SUCCESS) {
                if (apiAvailability.isUserResolvableError(resultCode)) {
                    apiAvailability.getErrorDialog(this, resultCode, PLAY_SERVICES_RESOLUTION_REQUEST)
                            .show();
                } else {
                    Log.i(TAG, "This device is not supported by Google Play Services.");
                    ToastNotify("This device is not supported by Google Play Services.");
                    finish();
                }
                return false;
            }
            return true;
        }
    ```

5. Fügen Sie in der `MainActivity`-Klasse den folgenden Code hinzu, mit dem eine Überprüfung auf Google Play Services durchgeführt wird, bevor das `IntentService`-Element aufgerufen wird, um Ihr FCM-Registrierungstoken abzurufen und die Registrierung beim Notification Hub vorzunehmen.
   
    ```java
        public void registerWithNotificationHubs()
        {
            if (checkPlayServices()) {
                // Start IntentService to register this application with FCM.
                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        }
    ```

6. Fügen Sie in der `OnCreate`-Methode der `MainActivity`-Klasse den folgenden Code hinzu, um den Registrierungsprozess zu starten, wenn die Aktivität erstellt wird.
   
    ```java
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
   
            mainActivity = this;
            NotificationsManager.handleNotifications(this, NotificationSettings.SenderId, MyHandler.class);
            registerWithNotificationHubs();
        }
    ```

7. Fügen Sie dem `MainActivity`-Element diese zusätzlichen Methoden hinzu, um den App-Zustand zu überprüfen und den Status in Ihrer App zu melden.
   
    ```java
        @Override
        protected void onStart() {
            super.onStart();
            isVisible = true;
        }
   
        @Override
        protected void onPause() {
            super.onPause();
            isVisible = false;
        }
   
        @Override
        protected void onResume() {
            super.onResume();
            isVisible = true;
        }
   
        @Override
        protected void onStop() {
            super.onStop();
            isVisible = false;
        }
   
        public void ToastNotify(final String notificationMessage) {
            runOnUiThread(new Runnable() {
                @Override
                public void run() {
                    Toast.makeText(MainActivity.this, notificationMessage, Toast.LENGTH_LONG).show();
                    TextView helloText = (TextView) findViewById(R.id.text_hello);
                    helloText.setText(notificationMessage);
                }
            });
        }
    ```

8. Für die `ToastNotify`-Methode wird das `TextView`-Steuerelement *„Hello World“* verwendet, um Status und Benachrichtigungen dauerhaft in der App zu melden. Fügen Sie im Layout von „activity_main.xml“ die folgende ID für dieses Steuerelement hinzu.
   
    ```java
       android:id="@+id/text_hello"
    ```

9. Als Nächstes fügen Sie eine Unterklasse für Ihren Empfänger hinzu, den Sie in „AndroidManifest.xml“ definiert haben. Fügen Sie dem Projekt eine weitere neue Klasse namens `MyHandler`hinzu.
10. Fügen Sie am Anfang von `MyHandler.java` die folgenden Importanweisungen hinzu:
    
    ```java
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.media.RingtoneManager;
        import android.net.Uri;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;
        import com.microsoft.windowsazure.notifications.NotificationsHandler;
    ```

11. Fügen Sie für die `MyHandler`-Klasse den folgenden Code hinzu, um sie `com.microsoft.windowsazure.notifications.NotificationsHandler` unterzuordnen.
    
    Mit diesem Code wird die `OnReceive`-Methode überschrieben, sodass der Handler empfangene Benachrichtigungen meldet. Der Handler sendet die Pushbenachrichtigung mit der `sendNotification()` -Methode auch an den Android-Benachrichtigungs-Manager. Die `sendNotification()` -Methode muss ausgeführt werden, wenn die App nicht ausgeführt wird und eine Benachrichtigung eingeht.
    
    ```java
        public class MyHandler extends NotificationsHandler {
            public static final int NOTIFICATION_ID = 1;
            private NotificationManager mNotificationManager;
            NotificationCompat.Builder builder;
            Context ctx;
    
            @Override
            public void onReceive(Context context, Bundle bundle) {
                ctx = context;
                String nhMessage = bundle.getString("message");
                sendNotification(nhMessage);
                if (MainActivity.isVisible) {
                    MainActivity.mainActivity.ToastNotify(nhMessage);
                }
            }
    
            private void sendNotification(String msg) {
    
                Intent intent = new Intent(ctx, MainActivity.class);
                intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP);
    
                mNotificationManager = (NotificationManager)
                        ctx.getSystemService(Context.NOTIFICATION_SERVICE);
    
                PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
                        intent, PendingIntent.FLAG_ONE_SHOT);
    
                Uri defaultSoundUri = RingtoneManager.getDefaultUri(RingtoneManager.TYPE_NOTIFICATION);
                NotificationCompat.Builder mBuilder =
                        new NotificationCompat.Builder(ctx)
                                .setSmallIcon(R.mipmap.ic_launcher)
                                .setContentTitle("Notification Hub Demo")
                                .setStyle(new NotificationCompat.BigTextStyle()
                                        .bigText(msg))
                                .setSound(defaultSoundUri)
                                .setContentText(msg);
    
                mBuilder.setContentIntent(contentIntent);
                mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
            }
        }
    ```

12. Klicken Sie in Android Studio auf der Menüleiste auf **Build** > **Rebuild Project**, um sicherzustellen, dass im Code keine Fehler enthalten sind.
13. Führen Sie die App auf Ihrem Gerät aus, und vergewissern Sie sich, dass sie richtig beim Notification Hub registriert wird. 
    
    > [!NOTE]
    > Die Registrierung schlägt beim ersten Starten unter Umständen fehl, bis die `onTokenRefresh()`-Methode des Instanz-ID-Diensts aufgerufen wird. Durch die Aktualisierung sollte eine erfolgreiche Registrierung beim Notification Hub initiiert werden.
    > 
    > 

## <a name="sending-push-notifications"></a>Senden von Pushbenachrichtigungen
Sie können Pushbenachrichtigungen in Ihrer App testen, indem Sie sie über das [Azure-Portal] senden. Suchen Sie im Hub wie unten gezeigt nach dem Abschnitt **Problembehandlung**.

![Azure Notification Hubs – Testsendung](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-test-send.png)

[!INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]


## <a name="testing-your-app"></a>Testen der App
#### <a name="push-notifications-in-the-emulator"></a>Pushbenachrichtigungen im Emulator
Wenn Sie Pushbenachrichtigungen in einem Emulator testen möchten, müssen Sie sicherstellen, dass das Emulatorabbild die Google-API-Ebene unterstützt, die Sie für die App ausgewählt haben. Wenn das Image keine nativen Google-APIs unterstützt, wird die Ausnahme **SERVICE\_NOT\_AVAILABLE** ausgegeben.

Stellen Sie zusätzlich zu den obigen Punkten sicher, dass Ihr Google-Konto dem ausgeführten Emulator unter **Azure-Portal** > **Konten**(Senden von Pushbenachrichtigungen an Android mit Azure Notification Hubs und FCM) weiter. Andernfalls führt die Registrierung bei GCM möglicherweise zur Ausnahme **AUTHENTICATION\_FAILED**.

#### <a name="running-the-application"></a>Ausführen der Anwendung
1. Führen Sie die App aus, und prüfen Sie, ob die Registrierungs-ID als erfolgreiche Registrierung gemeldet wird.
   
    ![Testen unter Android – Kanalregistrierung](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-registered.png)
2. Geben Sie eine Benachrichtigungsmeldung ein, die an alle für den Hub registrierten Android-Geräte gesendet werden soll.
   
    ![Testen unter Android – Senden einer Nachricht](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-set-message.png)
3. Tippen Sie auf **Send Notification**. Auf allen Geräten, auf denen die App ausgeführt wird, wird eine `AlertDialog`-Instanz mit der Pushbenachrichtigungsnachricht angezeigt. Geräte, auf denen die App nicht ausgeführt wird, die aber zuvor für Pushbenachrichtigungen registriert wurden, erhalten im Android-Benachrichtigungs-Manager eine Benachrichtigung. Diese können durch Wischen von der linken oberen Ecke nach unten angezeigt werden.
   
    ![Testen unter Android – Benachrichtigungen](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-received-message.png)

## <a name="next-steps"></a>Nächste Schritte
Als nächster Schritt wird das Tutorial [Verwenden von Notification Hubs für Pushbenachrichtigungen an Benutzer] empfohlen. Darin erfahren Sie, wie Sie Benachrichtigungen über ein ASP.NET-Back-End mithilfe von Tags an bestimmte Benutzer senden.


Wenn Sie Ihre Benutzer nach Interessengruppen unterteilen möchten, helfen Ihnen die Informationen im Tutorial [Verwenden von Notification Hubs zum Übermitteln von aktuellen Nachrichten] weiter.

Weitere allgemeine Informationen zu Notification Hubs finden Sie in unserem [Notification Hubs-Leitfaden].

<!-- Images. -->



<!-- URLs. -->
[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-android-get-started-push.md  
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Referencing a library project]: http://go.microsoft.com/fwlink/?LinkId=389800
[Notification Hubs-Leitfaden]: notification-hubs-push-notification-overview.md
[Verwenden von Notification Hubs für Pushbenachrichtigungen an Benutzer]: notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md
[Verwenden von Notification Hubs zum Übermitteln von aktuellen Nachrichten]: notification-hubs-aspnet-backend-android-xplat-segmented-gcm-push-notification.md
[Azure-Portal]: https://portal.azure.com
