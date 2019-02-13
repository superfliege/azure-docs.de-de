---
title: Senden von Pushbenachrichtigungen an Android-Apps mit Azure Notification Hubs und Firebase Cloud Messaging | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie mithilfe von Azure Notification Hubs und Google Firebase Cloud Messaging Pushbenachrichtigungen an Android-Geräte senden.
services: notification-hubs
documentationcenter: android
keywords: Pushbenachrichtigungen,Pushbenachrichtigung,Android-Pushbenachrichtigung,FCM,Firebase Cloud Messaging
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: 02298560-da61-4bbb-b07c-e79bd520e420
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: tutorial
ms.custom: mvc
ms.date: 02/05/2019
ms.author: jowargo
ms.openlocfilehash: 9467197715d79527699eac0acf9c23f204b0e639
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55814896"
---
# <a name="tutorial-push-notifications-to-android-devices-by-using-azure-notification-hubs-and-google-firebase-cloud-messaging"></a>Tutorial: Senden von Pushbenachrichtigungen an Android-Geräte mit Azure Notification Hubs und Google Firebase Cloud Messaging

[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

In diesem Tutorial erfahren Sie, wie Sie mithilfe von Azure Notification Hubs und Firebase Cloud Messaging (FCM) Pushbenachrichtigungen an eine Android-Anwendung senden. In diesem Tutorial erstellen Sie eine leere Android-App, die Pushbenachrichtigungen mithilfe von Firebase Cloud Messaging (FCM) empfängt.

Den vollständigen Code für dieses Tutorial können Sie [hier](https://github.com/Azure/azure-notificationhubs-samples/tree/master/Android/GetStartedFirebase)bei GitHub herunterladen.

In diesem Tutorial führen Sie die folgenden Schritte aus:

> [!div class="checklist"]
> * Erstellen eines Android Studio-Projekts.
> * Erstellen eines Firebase-Projekts, das Firebase Cloud Messaging unterstützt.
> * Erstellen eines Notification Hubs.
> * Herstellen einer Verbindung Ihrer App mit dem Notification Hub.
> * Testen der App.

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen ein aktives Azure-Konto, um dieses Lernprogramm abzuschließen. Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter [Kostenlose Azure-Testversion](https://azure.microsoft.com/free/).

* Zusätzlich zum oben erwähnten aktiven Azure-Konto ist für dieses Tutorial die aktuelle Version von [Android Studio](https://go.microsoft.com/fwlink/?LinkId=389797)erforderlich.
* Android 2.3 oder höher für Firebase Cloud Messaging.
* Google Repository Version 27 oder höher ist für Firebase Cloud Messaging erforderlich.
* Google Play Services 9.0.2 oder höher für Firebase Cloud Messaging.
* Das Abschließen dieses Tutorial ist eine Voraussetzung für alle anderen Notification Hubs-Tutorials für Android-Apps.

## <a name="create-an-android-studio-project"></a>Erstellen eines Android Studio-Projekts

1. Starten Sie Android Studio.
2. Wählen Sie im Menü die Option **File** (Datei), zeigen Sie auf **New** (Neu), und wählen Sie **New Project** (Neues Projekt). 
2. Wählen Sie auf der Seite **Choose your project** (Projekt auswählen) die Option **Empty Activity** (Leere Aktivität) und dann **Next** (Weiter) aus. 
3. Führen Sie auf der Seite **Configure your project** (Projekt konfigurieren) die folgenden Schritte aus: 
    1. Geben Sie einen **Namen** für die Anwendung ein.
    2. Geben Sie einen Speicherort zum Speichern der Projektdateien an. 
    3. Wählen Sie **Fertig stellen** aus. 

        ![Konfigurieren des Projekts](./media/notification-hubs-android-push-notification-google-fcm-get-started/configure-project.png)

## <a name="create-a-firebase-project-that-supports-fcm"></a>Erstellen eines Firebase-Projekts, das FCM unterstützt

[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-notification-hub"></a>Konfigurieren eines Notification Hubs

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

### <a name="configure-firebase-cloud-messaging-settings-for-the-hub"></a>Konfigurieren von Firebase Cloud Messaging-Einstellungen für den Hub

1. Wählen Sie im linken Menü unter **Settings** (Einstellungen) die Option **Google (GCM/FCM)** aus. 
2. Fügen Sie den **Serverschlüssel** für das FCM-Projekt ein, den Sie zuvor gespeichert haben. 
3. Wählen Sie auf der Symbolleiste **Save** (Speichern) aus. 

    ![Azure Notification Hubs – Google (FCM)](./media/notification-hubs-android-push-notification-google-fcm-get-started/fcm-server-key.png)
4. In den Warnungen wird eine Meldung angezeigt, dass die Notification Hubs aktualisiert wurden. Die Schaltfläche **Save** (Speichern) ist deaktiviert. 

Der Notification Hub ist jetzt für die Arbeit mit Firebase Cloud Messaging konfiguriert, und Sie besitzen die Verbindungszeichenfolge, um die App für den Empfang und das Senden von Pushbenachrichtigungen zu registrieren.

## <a id="connecting-app"></a>Verbinden Ihrer App mit dem Notification Hub

### <a name="add-google-play-services-to-the-project"></a>Hinzufügen von Google Play Services zum Projekt

[!INCLUDE [Add Play Services](../../includes/notification-hubs-android-studio-add-google-play-services.md)]

### <a name="adding-azure-notification-hubs-libraries"></a>Hinzufügen von Azure Notification Hubs-Bibliotheken

1. Fügen Sie in der Datei `Build.Gradle` für die **App** die folgenden Zeilen im Abschnitt **dependencies** hinzu.

    ```text
    implementation 'com.microsoft.azure:notification-hubs-android-sdk:0.4@aar'
    implementation 'com.microsoft.azure:azure-notifications-handler:1.0.1@aar'
    ```

2. Fügen Sie das folgende Repository nach dem Abschnitt **dependencies** hinzu.

    ```text
    repositories {
        maven {
            url "http://dl.bintray.com/microsoftazuremobile/SDK"
        }
    }
    ```

### <a name="add-google-firebase-support"></a>Hinzufügen von Unterstützung für Google Firebase

1. Fügen Sie in der Datei `Build.Gradle` für die **App** im Abschnitt **dependencies** die folgende Zeile hinzu, sofern sie noch nicht vorhanden ist: 

    ```text
    implementation 'com.google.firebase:firebase-core:16.0.0'
    ```

2. Fügen Sie am Ende der Datei folgendes Plug-In hinzu, sofern es noch nicht vorhanden ist: 

    ```text
    apply plugin: 'com.google.gms.google-services'
    ```

### <a name="updating-the-androidmanifestxml"></a>Aktualisieren von „AndroidManifest.xml“

1. Für die Unterstützung von FCM müssen Sie in Ihrem Code einen Instanz-ID-Listenerdienst implementieren, der zum [Beschaffen von Registrierungstoken](https://firebase.google.com/docs/cloud-messaging/android/client#sample-register) mit der [FirebaseInstanceId-API von Google](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId) verwendet wird. In diesem Tutorial lautet der Name der Klasse `MyInstanceIDService`.

    Fügen Sie der Datei „AndroidManifest.xml“ im Tag `<application>` die unten angegebene Dienstdefinition hinzu.

    ```xml
    <service android:name=".MyInstanceIDService">
        <intent-filter>
            <action android:name="com.google.firebase.INSTANCE_ID_EVENT"/>
        </intent-filter>
    </service>
    ```

2. Nachdem Sie Ihr FCM-Registrierungstoken von der FirebaseInstanceId-API erhalten haben, verwenden Sie es zum [Registrieren beim Azure Notification Hub](notification-hubs-push-notification-registration-management.md). Sie unterstützen diese Registrierung im Hintergrund mit einem `IntentService` mit dem Namen `RegistrationIntentService`. Dieser Dienst ist auch für die Aktualisierung Ihres FCM-Registrierungstokens zuständig.

    Fügen Sie der Datei „AndroidManifest.xml“ im Tag `<application>` die unten angegebene Dienstdefinition hinzu.

    ```xml
    <service
        android:name=".RegistrationIntentService"
        android:exported="false">
    </service>
    ```

3. Außerdem müssen Sie einen Empfänger für die Benachrichtigungen definieren. Fügen Sie der Datei „AndroidManifest.xml“ im Tag `<application>` die unten angegebene Empfängerdefinition hinzu. 

    ```xml
    <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
        android:permission="com.google.android.c2dm.permission.SEND">
        <intent-filter>
            <action android:name="com.google.android.c2dm.intent.RECEIVE" />
            <category android:name="<your package name>" />
        </intent-filter>
    </receiver>
    ```

    > [!IMPORTANT]
    > Ersetzen Sie den Platzhalter `<your package NAME>` durch Ihren tatsächlichen Namen des Pakets, der oben in der Datei `AndroidManifest.xml` angegeben ist.
4. Wählen Sie auf der Symbolleiste **Sync Now** (Jetzt synchronisieren) aus.
5. Fügen Sie **unterhalb** des Tags `</application>` die folgenden erforderlichen FCM-bezogenen Berechtigungen hinzu.

    ```xml
    <uses-permission android:name="android.permission.INTERNET"/>
    <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
    <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
    ```

### <a name="adding-code"></a>Hinzufügen von Code

1. Erweitern Sie in der Projektansicht die Knoten **app** > **src** > **main** > **java**. Klicken Sie mit der rechten Maustaste auf Ihren Paketordner unter **java**, klicken Sie auf **Neu** und dann auf **Java Class**. Geben Sie als Name `NotificationSettings` ein, und wählen Sie **OK**.

    Aktualisieren Sie diese drei Platzhalter im weiter unten angegebenen Code für die `NotificationSettings`-Klasse:

   * **HubListenConnectionString:** Die **DefaultListenAccessSignature**-Verbindungszeichenfolge für Ihren Hub. Sie können diese Verbindungszeichenfolge kopieren, indem Sie im [Azure-Portal] für Ihren Hub auf **Zugriffsrichtlinien** klicken.
   * **HubName:** Verwenden Sie den Namen Ihres Benachrichtigungs-Hubs, der im [Azure-Portal] auf der Hub-Seite angezeigt wird.

     `NotificationSettings` -Code:

        ```java
        public class NotificationSettings {
            public static String HubName = "<Your HubName>";
            public static String HubListenConnectionString = "<Enter your DefaultListenSharedAccessSignature connection string>";
        }
        ```

    > [!IMPORTANT]
    > Geben Sie für Ihren Notification Hub den **Namen** und einen Wert für **DefaultListenSharedAccessSignature** ein, bevor Sie fortfahren. 
2. Fügen Sie eine weitere Klasse namens `MyInstanceIDService`hinzu. Diese Klasse ist Ihre Implementierung des Instanz-ID-Listenerdiensts.

    Mit dem Code für diese Klasse wird Ihr `IntentService`-Element aufgerufen, um im Hintergrund [das FCM-Token zu aktualisieren](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens).

    ```java
    import android.content.Intent;
    import android.util.Log;
    import com.google.firebase.iid.FirebaseInstanceIdService;

    public class MyInstanceIDService extends FirebaseInstanceIdService {

        private static final String TAG = "MyInstanceIDService";

        @Override
        public void onTokenRefresh() {

            Log.d(TAG, "Refreshing FCM Registration Token");

            Intent intent = new Intent(this, RegistrationIntentService.class);
            startService(intent);
        }
    };
    ```

3. Fügen Sie dem Projekt eine weitere neue Klasse namens `RegistrationIntentService`hinzu. Diese Klasse implementiert die `IntentService`-Schnittstelle und ist für das [Aktualisieren des GCM-Tokens](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) und [Registrieren beim Benachrichtigungshub](notification-hubs-push-notification-registration-management.md) zuständig.

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

4. Fügen Sie in der `MainActivity`-Klasse oberhalb der Klassendeklaration die folgenden `import`-Anweisungen hinzu.

    ```java
    import com.google.android.gms.common.ConnectionResult;
    import com.google.android.gms.common.GoogleApiAvailability;
    import com.microsoft.windowsazure.notifications.NotificationsManager;
    import android.content.Intent;
    import android.util.Log;
    import android.widget.TextView;
    import android.widget.Toast;
    ```

5. Fügen Sie im oberen Bereich der Klasse die folgenden Member hinzu. Sie verwenden diese Felder zum [Überprüfen der Verfügbarkeit von Google Play Services gemäß Empfehlung von Google](https://developers.google.com/android/guides/setup#ensure_devices_have_the_google_play_services_apk).

    ```java
    public static MainActivity mainActivity;
    public static Boolean isVisible = false;
    private static final String TAG = "MainActivity";
    private static final int PLAY_SERVICES_RESOLUTION_REQUEST = 9000;
    ```

6. Fügen Sie in der Klasse `MainActivity` die folgende Methode hinzu, um die Verfügbarkeit von Google Play Services zu überprüfen.

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

7. Fügen Sie in der `MainActivity`-Klasse den folgenden Code hinzu, mit dem eine Überprüfung auf Google Play Services durchgeführt wird, bevor das `IntentService`-Element aufgerufen wird, um Ihr FCM-Registrierungstoken abzurufen und die Registrierung beim Notification Hub vorzunehmen.

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

8. Fügen Sie in der `OnCreate`-Methode der `MainActivity`-Klasse den folgenden Code hinzu, um den Registrierungsprozess zu starten, wenn die Aktivität erstellt wird.

    ```java
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        mainActivity = this;
        registerWithNotificationHubs();
        MyHandler.createChannelAndHandleNotifications(getApplicationContext());
    }
    ```

9. Fügen Sie dem `MainActivity`-Element diese zusätzlichen Methoden hinzu, um den App-Zustand zu überprüfen und den Status in Ihrer App zu melden.

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

10. Für die `ToastNotify`-Methode wird das `TextView`-Steuerelement *„Hello World“* verwendet, um Status und Benachrichtigungen dauerhaft in der App zu melden. Fügen Sie im Layout von **res** -> **layout** -> **activity_main.xml** die folgende ID für dieses Steuerelement hinzu.

    ```java
    android:id="@+id/text_hello"
    ```

11. Als Nächstes fügen Sie eine Unterklasse für Ihren Empfänger hinzu, den Sie in „AndroidManifest.xml“ definiert haben. Fügen Sie dem Projekt eine weitere neue Klasse namens `MyHandler`hinzu.

12. Fügen Sie am Anfang von `MyHandler.java` die folgenden Importanweisungen hinzu:

    ```java
    import android.app.NotificationChannel;
    import android.app.NotificationManager;
    import android.app.PendingIntent;
    import android.content.Context;
    import android.content.Intent;
    import android.media.RingtoneManager;
    import android.net.Uri;
    import android.os.Build;
    import android.os.Bundle;
    import android.support.v4.app.NotificationCompat;
    import com.microsoft.windowsazure.notifications.NotificationsHandler;    
    import com.microsoft.windowsazure.notifications.NotificationsManager;
    ```

13. Fügen Sie für die `MyHandler`-Klasse den folgenden Code hinzu, um sie `com.microsoft.windowsazure.notifications.NotificationsHandler` unterzuordnen.

    Mit diesem Code wird die `OnReceive`-Methode überschrieben, sodass der Handler empfangene Benachrichtigungen meldet. Der Handler sendet die Pushbenachrichtigung mit der `sendNotification()` -Methode auch an den Android-Benachrichtigungs-Manager. Die `sendNotification()` -Methode muss ausgeführt werden, wenn die App nicht ausgeführt wird und eine Benachrichtigung eingeht.

    ```java
    public class MyHandler extends NotificationsHandler {
        public static final String NOTIFICATION_CHANNEL_ID = "nh-demo-channel-id";
        public static final String NOTIFICATION_CHANNEL_NAME = "Notification Hubs Demo Channel";
        public static final String NOTIFICATION_CHANNEL_DESCRIPTION = "Notification Hubs Demo Channel";
    
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
            NotificationCompat.Builder notificationBuilder = new NotificationCompat.Builder(
                    ctx,
                    NOTIFICATION_CHANNEL_ID)
                    .setContentText(msg)
                    .setPriority(NotificationCompat.PRIORITY_HIGH)
                    .setSmallIcon(android.R.drawable.ic_popup_reminder)
                    .setBadgeIconType(NotificationCompat.BADGE_ICON_SMALL);
    
            notificationBuilder.setContentIntent(contentIntent);
            mNotificationManager.notify(NOTIFICATION_ID, notificationBuilder.build());
        }
    
        public static void createChannelAndHandleNotifications(Context context) {
            if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
                NotificationChannel channel = new NotificationChannel(
                        NOTIFICATION_CHANNEL_ID,
                        NOTIFICATION_CHANNEL_NAME,
                        NotificationManager.IMPORTANCE_HIGH);
                channel.setDescription(NOTIFICATION_CHANNEL_DESCRIPTION);
                channel.setShowBadge(true);
    
                NotificationManager notificationManager = context.getSystemService(NotificationManager.class);
                notificationManager.createNotificationChannel(channel);
                NotificationsManager.handleNotifications(context, "", MyHandler.class);
            }
        }
    }
    ```

14. Klicken Sie in Android Studio auf der Menüleiste auf **Build** > **Rebuild Project**, um sicherzustellen, dass im Code keine Fehler enthalten sind. Wird eine Fehlermeldung zum Symbol `ic_launcher` angezeigt, entfernen Sie die folgende Anweisung aus der Datei „AndroidManifest.xml“. 

    ```
        android:icon="@mipmap/ic_launcher"
    ```
15. Führen Sie die App auf Ihrem Gerät aus, und vergewissern Sie sich, dass sie richtig beim Notification Hub registriert wird.

    > [!NOTE]
    > Die Registrierung schlägt beim ersten Starten unter Umständen fehl, bis die `onTokenRefresh()`-Methode des Instanz-ID-Diensts aufgerufen wird. Durch die Aktualisierung sollte eine erfolgreiche Registrierung beim Notification Hub initiiert werden.

    ![Erfolgreiche Geräteregistrierung](./media/notification-hubs-android-push-notification-google-fcm-get-started/device-registration.png)

## <a name="test-send-notification-from-the-notification-hub"></a>Testsendung der Benachrichtigung über den Notification Hub

Sie können Pushbenachrichtigungen mit folgenden Aktionen über das [Azure-Portal] senden:

1. Wählen Sie im Azure-Portal auf der Seite **Notification Hub** für Ihren Notification Hub im Abschnitt **Problembehandlung** die Option **Testsendevorgang** aus.
3. Wählen Sie für **Plattformen** die Option **Android** aus.
4. Wählen Sie **Senden** aus.  Es wird noch keine Benachrichtigung auf dem Android-Gerät angezeigt, da Sie die mobile App nicht darauf ausgeführt haben. Wählen Sie nach dem Ausführen der mobilen App erneut die Schaltfläche **Senden** aus, um die Benachrichtigung anzuzeigen.
5. Sie sehen das **Ergebnis** des Vorgangs in der Liste am unteren Rand des Fensters.

    ![Azure Notification Hubs – Testsendung](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-test-send.png)
6. Die Benachrichtigung wird auf Ihrem Gerät angezeigt. 

    ![Benachrichtigung auf Gerät](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-on-device.png)
    

[!INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]

### <a name="run-the-mobile-app-on-emulator"></a>Ausführen der mobilen App im Emulator
Wenn Sie Pushbenachrichtigungen in einem Emulator testen möchten, müssen Sie sicherstellen, dass das Emulatorabbild die Google-API-Ebene unterstützt, die Sie für die App ausgewählt haben. Wenn das Image keine nativen Google-APIs unterstützt, wird unter Umständen die Ausnahme **SERVICE\_NOT\_AVAILABLE** angezeigt.

Stellen Sie zusätzlich sicher, dass Ihr Google-Konto dem ausgeführten Emulator unter **Einstellungen** > **Konten** hinzugefügt wurde. Andernfalls führt die Registrierung bei GCM möglicherweise zur Ausnahme **AUTHENTICATION\_FAILED**.

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie mit Firebase Cloud Messaging Pushbenachrichtigungen an Android-Geräte gesendet. Um zu erfahren, wie Sie Pushbenachrichtigungen mit Google Cloud Messaging senden, fahren Sie mit dem folgenden Tutorial fort:

> [!div class="nextstepaction"]
>[Senden von Pushbenachrichtigungen an Android mit Azure Notification Hubs](notification-hubs-android-push-notification-google-gcm-get-started.md)

<!-- Images. -->

<!-- URLs. -->
[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-android-get-started-push.md  
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Referencing a library project]: http://go.microsoft.com/fwlink/?LinkId=389800
[Notification Hubs Guidance]: notification-hubs-push-notification-overview.md
[Use Notification Hubs to push notifications to users]: notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md
[Use Notification Hubs to send breaking news]: notification-hubs-aspnet-backend-android-xplat-segmented-gcm-push-notification.md
[Azure-Portal]: https://portal.azure.com
