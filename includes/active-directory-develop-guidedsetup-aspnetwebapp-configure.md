
## <a name="create-an-application-express"></a>Erstellen einer Anwendung (Express)
Nun müssen Sie Ihre Anwendung im *Microsoft-Anwendungsregistrierungsportal* registrieren:
1. Registrieren Sie Ihre Anwendung im [Microsoft-Anwendungsregistrierungsportal](https://apps.dev.microsoft.com/portal/register-app?appType=serverSideWebApp&appTech=aspNetWebAppOwin&step=configure).
2.  Geben Sie einen Namen für Ihre Anwendung und Ihre E-Mail-Adresse ein.
3.  Stellen Sie sicher, dass die Option für geführtes Setup aktiviert ist.
4.  Befolgen Sie die Anweisungen, um Ihrer Anwendung eine Umleitungs-URL hinzufügen.

## <a name="add-your-application-registration-information-to-your-solution-advanced"></a>Hinzufügen der Registrierungsinformationen Ihrer Anwendung zur Projektmappe (für Fortgeschrittene)
Nun müssen Sie Ihre Anwendung im *Microsoft-Anwendungsregistrierungsportal* registrieren:
1. Registrieren Sie Ihre Anwendung im [Microsoft-Anwendungsregistrierungsportal](https://apps.dev.microsoft.com/portal/register-app).
2. Geben Sie einen Namen für Ihre Anwendung und Ihre E-Mail-Adresse ein. 
3.  Stellen Sie sicher, dass die Option für geführtes Setup deaktiviert ist.
4.  Klicken Sie auf `Add Platform`, und wählen Sie dann `Web` aus.
5.  Kehren Sie zurück zu Visual Studio, und wählen Sie im Projektmappen-Explorer das Projekt aus. Untersuchen Sie das Fenster „Eigenschaften“ (wenn es nicht angezeigt wird, drücken Sie F4).
6.  Ändern Sie „SSL-aktiviert“ in `True`.
7.  Kopieren Sie die SSL-URL, und fügen Sie diese URL zur Liste der Umleitungs-URLs im Registrierungsportal hinzu:<br/><br/>![Projekteigenschaften](media/active-directory-develop-guidedsetup-aspnetwebapp-configure/vsprojectproperties.png)<br />
8.  Fügen Sie im Stammordner der Datei `web.config` im Abschnitt `configuration\appSettings` Folgendes hinzu:

```xml
<add key="ClientId" value="Enter_the_Application_Id_here" />
<add key="redirectUri" value="Enter_the_Redirect_URL_here" />
<add key="Tenant" value="common" />
<add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" /> 
```
<!-- Workaround for Docs conversion bug -->
<ol start="9">
<li>
Ersetzen Sie `ClientId` durch die Anwendungs-ID, die Sie gerade registriert haben.
</li>
<li>
Ersetzen Sie `redirectUri` durch die SSL-URL Ihres Projekts.
</li>
</ol>
<!-- End Docs -->
