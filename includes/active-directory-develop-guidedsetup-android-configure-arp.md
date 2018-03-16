
## <a name="add-the-applications-registration-information-to-your-app"></a>Hinzufügen der Registrierungsinformationen der Anwendung zu Ihrer App

In diesem Schritt müssen Sie die Client-ID Ihrem Projekt hinzufügen.

1.  Öffnen Sie `MainActivity` (unter `app` > `java` > *`{host}.{namespace}`*).
2.  Ersetzen Sie die Zeile, die mit `final static String CLIENT_ID` beginnt, durch:
```java
final static String CLIENT_ID = "[Enter the application Id here]";
```
3. Öffnen Sie: `app` > `manifests` > `AndroidManifest.xml`
4. Fügen Sie die folgende Aktivität dem Knoten `manifest\application` hinzu. Hierdurch wird `BrowserTabActivity` registriert, um dem Betriebssystem das Fortsetzen der Anwendung nach Abschluss der Authentifizierung zu ermöglichen:

```xml
<!--Intent filter to capture System Browser calling back to our app after Sign In-->
<activity
    android:name="com.microsoft.identity.client.BrowserTabActivity">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />

        <!--Add in your scheme/host from registered redirect URI-->
        <!--By default, the scheme should be similar to 'msal[appId]' -->
        <data android:scheme="msal[Enter the application Id here]"
            android:host="auth" />
    </intent-filter>
</activity>
```

