
### <a name="update-manifest-file-to-enable-notifications"></a>Aktualisieren der Manifestdatei zum Aktivieren von Benachrichtigungen
Kopieren Sie unten die in-app-messaging-Ressourcen in Ihrem "Manifest.xml" zwischen den `<application>` und `</application>` Tags.

        <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/plain" />
              </intent-filter>
        </activity>
        <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
            <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/html" />
            </intent-filter>
        </activity>
        <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity" android:theme="@android:style/Theme.Light" android:exported="false">
            <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT" />
            </intent-filter>
        </activity>
        <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity" android:theme="@android:style/Theme.Dialog" android:exported="false">
            <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
                <category android:name="android.intent.category.DEFAULT"/>
            </intent-filter>
        </activity>
        <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver" android:exported="false">
            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
            </intent-filter>
        </receiver>
        <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachDownloadReceiver">
            <intent-filter>
                <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
            </intent-filter>
        </receiver>

### <a name="specify-an-icon-for-notifications"></a>Geben Sie ein Symbol für Benachrichtigungen
Fügen Sie die folgenden XML-Ausschnitt in die "Manifest.xml" zwischen den `<application>` und `</application>` Tags.

        <meta-data android:name="engagement:reach:notification:icon" android:value="engagement_close"/>

Hiermit wird definiert, das Symbol, das sowohl in der System- und Benachrichtigungen in der app angezeigt wird. Ist er optional für app-Benachrichtigungen für systembenachrichtigungen jedoch obligatorisch. Android wird abgelehnt, systembenachrichtigungen mit ungültige Symbole.

Stellen Sie sicher, dass Sie ein Symbol, das in einem vorhanden ist die **zeichenbaren** Ordner (z. B. ``engagement_close.png``). **MipMap** Ordner wird nicht unterstützt.

> [!NOTE]
> Verwenden Sie nicht die **Startprogramm** Symbol. Es wurde eine andere Lösung und ist in der Regel in der Mipmap-Ordner, in denen nicht unterstützt wird.
> 
> 

Für real-apps können Sie ein Symbol, das für Benachrichtigungen pro geeignetes [Android Entwurfsrichtlinien](http://developer.android.com/design/patterns/notifications.html).

> [!TIP]
> Um sicherzustellen, dass auf das um Symbol für richtiges Lösungen zu verwenden, können Sie anzeigen [in diesen Beispielen](https://www.google.com/design/icons).
> Führen Sie einen Bildlauf nach unten, um die **Benachrichtigung** Abschnitt, klicken Sie auf ein Symbol, und klicken Sie dann auf `PNGS` zeichenbaren Symbolsatz herunterladen. Sie sehen zeichenbaren Ordner mit der Auflösung für jede Version des Symbols verwenden.
> 
> 

### <a name="enable-your-app-to-receive-gcm-push-notifications"></a>Konfigurieren Sie die Anwendung zum Empfangen von GCM-Pushbenachrichtigungen
1. Fügen Sie Folgendes in die "Manifest.xml" zwischen den `<application>` und `</application>` Tags, nach dem Ersetzen der **Absender-ID** aus Ihrer Firebase Projekt Konsole abgerufen. Die \n ist beabsichtigt Achten Sie darauf, dass Sie das Projekt mit Zahl enden.
   
        <meta-data android:name="engagement:gcm:sender" android:value="************\n" />
2. Fügen Sie den folgenden Code in Ihrem "Manifest.xml" zwischen den `<application>` und `</application>` Tags. Ersetzen Sie den Paketnamen <Your package name>.
   
        <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
        android:exported="false">
            <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
            </intent-filter>
        </receiver>
   
        <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver" android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<Your package name>" />
            </intent-filter>
        </receiver>
3. Den letzten Satz von Berechtigungen, die hervorgehoben werden, vor dem Hinzufügen der `<application>` Tag. Ersetzen Sie `<Your package name>` durch den tatsächlichen Paketnamen Ihrer Anwendung.
   
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
        <uses-permission android:name="<Your package name>.permission.C2D_MESSAGE" />
        <permission android:name="<Your package name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />

