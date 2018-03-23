## <a name="add-the-applications-registration-information-to-your-app"></a>Hinzufügen von Registrierungsinformationen für die Anwendung zu Ihrer App

In diesem Schritt müssen Sie die Umleitungs-URL der Registrierungsinformationen für die Anwendung konfigurieren und dann die Anwendungs-ID zu Ihrer JavaScript-Einzelseitenanwendung hinzufügen.

### <a name="configure-redirect-url"></a>Konfigurieren der Umleitungs-URL

Konfigurieren Sie das Feld `Redirect URL` mit der URL für Ihre Seite „index.html“ basierend auf Ihrem Webserver, und klicken Sie dann auf *Aktualisieren*.


> #### <a name="visual-studio-instructions-for-obtaining-redirect-url"></a>Visual Studio-Anweisungen zum Abrufen der Umleitungs-URL
> So erhalten Sie die Umleitungs-URL:
> 1.    Wählen Sie im *Projektmappen-Explorer* das Projekt aus. Untersuchen Sie das Fenster `Properties` (wenn es nicht angezeigt wird, drücken Sie `F4`).
> 2.    Kopieren Sie den Wert von `URL` in die Zwischenablage:<br/> ![Projekteigenschaften](media/active-directory-develop-guidedsetup-javascriptspa-configure/vs-project-properties-screenshot.png)<br />
> 3.    Fügen Sie den Wert als `Redirect URL` oben auf dieser Seite ein, und klicken Sie dann auf `Update`.

<p/>

> #### <a name="setting-redirect-url-for-python"></a>Festlegen der Umleitungs-URL für Python
> Für Python können Sie den Webserverport über die Befehlszeile festlegen. Diese ausführliche Anleitung verwendet den Port 8080. Sie können aber auch beliebige andere verfügbare Ports verwenden. Befolgen Sie in jedem Fall die nachstehenden Anweisungen zum Einrichten einer Umleitungs-URL in den Registrierungsinformationen der Anwendung:<br/>
> Legen Sie `http://localhost:8080/` oben auf dieser Seite als eine `Redirect URL` fest, oder verwenden Sie `http://localhost:[port]/` bei einem benutzerdefinierten TCP-Port (wobei *[port]* die benutzerdefinierte TCP-Portnummer ist). Klicken Sie anschließend auf „Aktualisieren“.

### <a name="configure-your-javascript-spa-application"></a>Konfigurieren Ihrer JavaScript-Einzelseitenanwendung

1.  Erstellen Sie eine Datei namens `msalconfig.js` mit den Informationen für die Anwendungsregistrierung. Wenn Sie Visual Studio verwenden, wählen Sie das Projekt aus (Stammordner des Projekts), klicken Sie mit der rechten Maustaste, und wählen Sie Folgendes aus: `Add` > `New Item` > `JavaScript File`. Vergeben Sie den Namen `msalconfig.js`.
2.  Fügen Sie Ihrer Datei `msalconfig.js` den folgenden Code hinzu:

```javascript
var msalconfig = {
    clientID: "[Enter the application Id here]",
    redirectUri: location.origin
};
``` 
