## <a name="test-your-code"></a>Testen Ihres Codes

### <a name="test-with-visual-studio"></a>Testen mit Visual Studio
Wenn Sie Visual Studio verwenden, drücken Sie **F5**, um Ihr Projekt auszuführen. Der Browser wird geöffnet und leitet Sie zu „http://<span></span>localhost:{Port}“ weiter, wo die Schaltfläche **Microsoft Graph-API aufrufen** angezeigt wird.

<p/><!-- --> 

### <a name="test-with-python-or-other-web-server"></a>Testen mit Python oder einem anderen Webserver
Wenn Sie nicht Visual Studio verwenden, stellen Sie sicher, dass Ihr Webserver gestartet wurde. Konfigurieren Sie den Server zum Lauschen am TCP-Port, der auf dem Speicherort Ihrer Datei **index.html** basiert. Beginnen Sie bei Python mit dem Lauschen am Port, indem Sie im Anwendungsordner den Befehl über die Eingabeaufforderung oder das Terminal ausführen:
 
```bash
python -m http.server 8080
```
Öffnen Sie den Browser, und geben Sie „http://<span></span>localhost:8080“ oder „http://<span></span>localhost:{Port}“ ein. Hierbei entspricht **Port** dem Port, an dem Ihr Webserver lauscht. Über die Schaltfläche **Microsoft Graph-API aufrufen** sollten Sie den Inhalt Ihrer Datei „index.html“ sehen.

## <a name="test-your-application"></a>Testen Ihrer Anwendung

Nachdem der Browser Ihre Datei „index.html“ geladen hat, klicken Sie auf **Microsoft Graph-API aufrufen**. Beim ersten Mal, bei dem Sie Ihre Anwendung ausführen, leitet der Browser Sie zum Microsoft Azure Active Directory v2.0-Endpunkt (Azure AD) um. Sie werden aufgefordert, sich anzumelden:
 
![Anmelden bei Ihrem JavaScript SPA-Konto](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspascreenshot1.png)


### <a name="provide-consent-for-application-access"></a>Zustimmen zum Anwendungszugriff

Bei der ersten Anmeldung bei Ihrer Anwendung werden Sie aufgefordert, Ihre Zustimmung zu geben, dass die Anwendung auf Ihr Profil zugreifen und Sie anmelden darf:

![Zustimmen zum Anwendungszugriff](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspaconsent.png)

### <a name="view-application-results"></a>Anzeigen von Anwendungsergebnissen
Nachdem Sie sich angemeldet haben, sollten Sie im Feld **Antwort auf Graph-API-Aufruf** die Benutzerprofilinformationen sehen.
 
![Erwartete Ergebnisse aus dem Microsoft Graph-API-Aufruf](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptsparesults.png)

Außerdem sollten in den Feldern **Zugriffstoken** und **ID-Tokenansprüche** grundlegende Informationen zum abgerufenen Token angezeigt werden.

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Weitere Informationen zu Bereichen und delegierten Berechtigungen

Die Microsoft Graph-API benötigt den Bereich **user.read**, um das Benutzerprofil zu lesen. Dieser Bereich wird standardmäßig jeder Anwendung automatisch hinzugefügt, die im Registrierungsportal registriert ist. Andere Microsoft Graph-APIs sowie benutzerdefinierte APIs für Ihren Back-End-Server erfordern unter Umständen zusätzliche Bereiche. Die Microsoft Graph-API benötigt den Bereich **Calendars.Read**, um die Kalender des Benutzers aufzuführen.

Um auf die Kalender des Benutzers im Kontext einer Anwendung zugreifen zu können, müssen Sie den Informationen für die Anwendungsregistrierung die delegierte Berechtigung **Calendars.Read** hinzufügen. Fügen Sie dann dem Aufruf von **acquireTokenSilent** den Bereich **Calendars.Read** hinzu. 

>[!NOTE]
>Wenn Sie die Anzahl der Bereiche erhöhen, werden Benutzer ggf. zu weiteren Genehmigungen aufgefordert.

Wenn eine Back-End-API keinen Bereich erfordert (nicht empfohlen), können Sie **clientId** als Bereich in den Aufrufen **acquireTokenSilent** und **acquireTokenRedirect** verwenden.

<!--end-collapse-->

[!INCLUDE [Help and support](./active-directory-develop-help-support-include.md)]
