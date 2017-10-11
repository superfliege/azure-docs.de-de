1. In Ihrem **app** Projekt, öffnen Sie die Datei `AndroidManifest.xml`. Ersetzen Sie in den Code in den nächsten beiden Schritten  *`**my_app_package**`*  mit dem Namen des app-Pakets für das Projekt. Dies ist der Wert, der die `package` Attribut von der `manifest` Tag.
2. Fügen Sie die folgenden neuen Berechtigungen nach den vorhandenen `uses-permission` Element:

        <permission android:name="**my_app_package**.permission.C2D_MESSAGE"
            android:protectionLevel="signature" />
        <uses-permission android:name="**my_app_package**.permission.C2D_MESSAGE" />
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
        <uses-permission android:name="android.permission.GET_ACCOUNTS" />
        <uses-permission android:name="android.permission.WAKE_LOCK" />
3. Fügen Sie den folgenden Code nach der `application` Starttag:

        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
                                         android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="**my_app_package**" />
            </intent-filter>
        </receiver>
4. Öffnen Sie die Datei *ToDoActivity.java*, und fügen Sie die folgende importanweisung hinzu:

        import com.microsoft.windowsazure.notifications.NotificationsManager;
5. Fügen Sie die folgende private Variable, auf die Klasse. Ersetzen Sie  *`<PROJECT_NUMBER>`*  mit der Projektnummer, die Ihre app aus dem vorhergehenden Verfahren von Google zugewiesen.

        public static final String SENDER_ID = "<PROJECT_NUMBER>";
6. Ändern Sie die Definition der *MobileServiceClient* aus **private** auf **öffentlichen statischen**, sodass es nun wie folgt aussieht:

        public static MobileServiceClient mClient;
7. Fügen Sie eine neue Klasse, um Benachrichtigungen zu behandeln. Öffnen Sie im Projekt-Explorer die **Src** > **main** > **Java** Knoten aus, und mit der rechten Maustaste den Paketknoten-Name. Klicken Sie auf **neu**, und klicken Sie dann auf **Java-Klasse**.
8. In **Namen**, Typ `MyHandler`, und klicken Sie dann auf **OK**.

    ![](./media/app-service-mobile-android-configure-push/android-studio-create-class.png)

9. Ersetzen Sie in der Datei MyHandler der Klassendeklaration mit:

        public class MyHandler extends NotificationsHandler {
10. Fügen Sie die folgende importanweisungen für die `MyHandler` Klasse:

        import com.microsoft.windowsazure.notifications.NotificationsHandler;
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.os.AsyncTask;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;
11. Fügen Sie dieses Element die `MyHandler` Klasse:

        public static final int NOTIFICATION_ID = 1;
12. In der `MyHandler` -Klasse verwenden, fügen Sie den folgenden Code hinzu, überschreiben die **OnRegistered** -Methode, die Ihr Gerät mit dem mobilen Dienst Notification Hub registriert.

        @Override
        public void onRegistered(Context context,  final String gcmRegistrationId) {
           super.onRegistered(context, gcmRegistrationId);

           new AsyncTask<Void, Void, Void>() {

               protected Void doInBackground(Void... params) {
                   try {
                       ToDoActivity.mClient.getPush().register(gcmRegistrationId);
                       return null;
                   }
                   catch(Exception e) {
                       // handle error                
                   }
                   return null;              
               }
           }.execute();
       }
13. In der `MyHandler` -Klasse verwenden, fügen Sie den folgenden Code hinzu, überschreiben die **OnReceive** -Methode, die bewirkt, dass die Benachrichtigung angezeigt, wenn sie empfangen werden.

        @Override
        public void onReceive(Context context, Bundle bundle) {
               String msg = bundle.getString("message");

               PendingIntent contentIntent = PendingIntent.getActivity(context,
                       0, // requestCode
                       new Intent(context, ToDoActivity.class),
                       0); // flags

               Notification notification = new NotificationCompat.Builder(context)
                       .setSmallIcon(R.drawable.ic_launcher)
                       .setContentTitle("Notification Hub Demo")
                       .setStyle(new NotificationCompat.BigTextStyle().bigText(msg))
                       .setContentText(msg)
                       .setContentIntent(contentIntent)
                       .build();

               NotificationManager notificationManager = (NotificationManager)
                       context.getSystemService(Context.NOTIFICATION_SERVICE);
               notificationManager.notify(NOTIFICATION_ID, notification);
       }
14. Wieder in der Datei TodoActivity.java Aktualisieren der **OnCreate** Methode der *ToDoActivity* Klasse zum Registrieren der Benachrichtigung Handler-Klasse. Stellen Sie sicher, dass dieser Code nach dem Hinzufügen der *MobileServiceClient* instanziiert wird.

        NotificationsManager.handleNotifications(this, SENDER_ID, MyHandler.class);

    Ihre app wird jetzt aktualisiert, um Pushbenachrichtigungen zu unterstützen.
