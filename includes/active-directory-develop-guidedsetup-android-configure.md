
## <a name="register-your-application"></a>Anwendung registrieren
Es gibt zwei Methoden für die Registrierung Ihrer Anwendung. Diese werden in den beiden folgenden Abschnitten beschrieben.

### <a name="option-1-express-mode"></a>Option 1: Expressmodus
Gehen Sie zur schnellen Registrierung Ihrer Anwendung wie folgt vor:
1. Navigieren Sie zum [Anwendungsregistrierungsportal von Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=android&step=configure).
2.  Geben Sie im Feld **Anwendungsname** einen Namen für Ihre Anwendung ein.

3. Vergewissern Sie sich, dass das Kontrollkästchen **Guided Setup** (Geführtes Setup) aktiviert ist, und klicken Sie anschließend auf **Erstellen**.

4. Befolgen Sie die Anweisungen zum Abrufen der Anwendungs-ID, und fügen Sie sie in Ihren Code ein.

### <a name="option-2-advanced-mode"></a>Option 2: Erweiterter Modus
Wenn Sie Ihre Anwendung registrieren und die Anwendungsregistrierungsinformationen Ihrer Projektmappe hinzufügen möchten, führen Sie folgende Schritte aus:
1. Falls Sie Ihre Anwendung noch nicht registriert haben, navigieren Sie zum [Anwendungsregistrierungsportal von Microsoft](https://apps.dev.microsoft.com/portal/register-app).
2. Geben Sie im Feld **Anwendungsname** einen Namen für Ihre Anwendung ein. 

3. Vergewissern Sie sich, dass das Kontrollkästchen **Guided Setup** (Geführtes Setup) deaktiviert ist, und klicken Sie anschließend auf **Erstellen**.

4. Klicken Sie auf **Plattform hinzufügen** > **Native Anwendung** und anschließend auf **Speichern**.

5. Öffnen Sie `MainActivity` unter **App** > **Java** > **{host}.{namespace}**. 

6.  Ersetzen Sie in der folgenden Zeile *[Enter the application Id here]* durch die Anwendungs-ID, die Sie gerade registriert haben:

    ```java
    final static String CLIENT_ID = "[Enter the application Id here]";
    ```
<!-- Workaround for Docs conversion bug -->
7. Öffnen Sie unter **App** > **Manifests** die Datei *AndroidManifest.xml*.

8. Fügen Sie im Knoten `manifest\application` die folgende Aktivität hinzu. Dadurch wird eine `BrowserTabActivity`-Aktivität hinzugefügt, die dem Betriebssystem das Fortsetzen der Anwendung nach Abschluss der Authentifizierung ermöglicht:

    ```xml
    <!--Intent filter to capture System Browser calling back to our app after sign-in-->
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
9. Ersetzen Sie im Knoten `BrowserTabActivity` die Zeichenfolge `[Enter the application Id here]` durch die Anwendungs-ID.
