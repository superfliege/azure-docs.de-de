
## <a name="create-an-application-express"></a>Erstellen einer Anwendung (Express)
Nun müssen Sie Ihre Anwendung im *Microsoft-Anwendungsregistrierungsportal* registrieren:
1. Registrieren Sie Ihre Anwendung im [Microsoft-Anwendungsregistrierungsportal](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=android&step=configure).
2.  Geben Sie einen Namen für Ihre Anwendung und Ihre E-Mail-Adresse ein.
3.  Stellen Sie sicher, dass die Option für geführtes Setup aktiviert ist.
4.  Befolgen Sie die Anweisungen zum Abrufen der Anwendungs-ID, und fügen Sie sie in Ihren Code ein.

### <a name="add-your-application-registration-information-to-your-solution-advanced"></a>Hinzufügen der Registrierungsinformationen Ihrer Anwendung zur Projektmappe (für Fortgeschrittene)
Nun müssen Sie Ihre Anwendung im *Microsoft-Anwendungsregistrierungsportal* registrieren:
1. Registrieren Sie Ihre Anwendung im [Microsoft-Anwendungsregistrierungsportal](https://apps.dev.microsoft.com/portal/register-app).
2. Geben Sie einen Namen für Ihre Anwendung und Ihre E-Mail-Adresse ein. 
3. Stellen Sie sicher, dass die Option für geführtes Setup deaktiviert ist.
4. Klicken Sie auf `Add Platform`, wählen Sie `Native Application` aus, und klicken Sie auf „Speichern“.
5.  Öffnen Sie `MainActivity` (unter `app` > `java` > *`{host}.{namespace}`*).
6.  Ersetzen Sie die *[hier die Anwendung-ID eingeben]* in der Zeile, die mit `final static String CLIENT_ID` beginnt, durch die Anwendungs-ID, die Sie gerade registriert haben:

```java
final static String CLIENT_ID = "[Enter the application Id here]";
```
<!-- Workaround for Docs conversion bug -->
<ol start="7">
<li>
Öffnen Sie `AndroidManifest.xml` (unter `app` > `manifests`). Fügen Sie die folgende Aktivität dem Knoten `manifest\application` hinzu. Hierdurch wird `BrowserTabActivity` registriert, um dem Betriebssystem das Fortsetzen der Anwendung nach Abschluss der Authentifizierung zu ermöglichen:
</li>
</ol>

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
<!-- Workaround for Docs conversion bug -->
<ol start="8">
<li>
Ersetzen Sie `[Enter the application Id here]` in `BrowserTabActivity` durch die Anwendungs-ID.
</li>
</ol>
