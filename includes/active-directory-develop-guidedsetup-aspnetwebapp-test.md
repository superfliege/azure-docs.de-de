## <a name="test-your-code"></a>Testen Ihres Codes

Um Ihre Anwendung in Visual Studio zu testen, drücken Sie **F5**, um das Projekt auszuführen. Der Browser wird geöffnet und leitet Sie zu „http://<span></span>localhost:{Port}“ weiter, wo die Schaltfläche **Bei Microsoft anmelden** angezeigt wird. Wählen Sie die Schaltfläche aus, um den Anmeldeprozess zu starten.

Wenn Sie Ihren Test ausführen möchten, verwenden Sie für die Anmeldung ein Microsoft Azure Active Directory-Konto (Azure AD) (Geschäfts-, Schul- oder Unikonto) oder ein persönliches Microsoft-Konto (<span>live.</span>com oder <span>outlook.</span>com).

![Bei Microsoft anmelden](media/active-directory-develop-guidedsetup-aspnetwebapp-test/aspnetbrowsersignin.png)
<br/><br/>
![Mit einem Microsoft-Konto anmelden](media/active-directory-develop-guidedsetup-aspnetwebapp-test/aspnetbrowsersignin2.png)

#### <a name="view-application-results"></a>Anzeigen von Anwendungsergebnissen
Nachdem Sie sich angemeldet haben, wird der Benutzer zur Startseite Ihrer Website umgeleitet. Die Startseite ist die HTTPS-URL, die im Microsoft-Anwendungsregistrierungsportal in Ihre Anwendungsregistrierungsinformationen eingegeben wurde. Auf der Startseite wird die Willkommensnachricht „Hallo \<Benutzer>“, ein Link zum Abmelden und ein Link zum Anzeigen der Benutzeransprüche angezeigt. Der Link für die Benutzeransprüche durchsucht den **Authorize**-Controller, den Sie zuvor erstellt haben.

### <a name="browse-to-see-the-users-claims"></a>Navigieren zu den Benutzeransprüchen
Um die Benutzeransprüche anzuzeigen, klicken Sie auf den Link zum Navigieren zur Controlleransicht. Diese ist nur für authentifizierte Benutzer verfügbar.

#### <a name="view-the-claims-results"></a>Anzeigen der Anspruchsergebnisse
Nachdem Sie die Controlleransicht aufgerufen haben, sollten Sie eine Tabelle mit den grundlegenden Eigenschaften des Benutzers sehen:

|Eigenschaft |Wert |BESCHREIBUNG |
|---|---|---|
|**Name** |Vollständiger Name des Benutzers | Vor- und Nachname des Benutzers
|**Benutzername** |Benutzer<span>@domain.com</span> | Der zur Identifizierung des Benutzers verwendete Benutzername
|**Betreff** |Antragsteller |Eine Zeichenfolge, die den Benutzer im Internet eindeutig identifiziert|
|**Tenant ID** |Guid | Eine **GUID**, die die Azure AD-Organisation des Benutzers eindeutig identifiziert|

Darüber hinaus sollten Sie eine Tabelle aller Ansprüche sehen, die in der Authentifizierungsanforderung enthalten sind. Weitere Informationen finden Sie in der [Liste der im Azure AD-ID-Token enthaltenen Ansprüche](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims).


### <a name="test-access-to-a-method-that-has-an-authorize-attribute-optional"></a>Testen des Zugriffs auf eine Methode mit einem Authorize-Attribut (optional)
So testen Sie als anonymer Benutzer den Zugriff auf den **Authorize**-Controller für die Benutzeransprüche
1. Klicken Sie auf den Link zum Abmelden des Benutzers, und schließen Sie den Abmeldevorgang ab.
2. Geben Sie in Ihren Browser „http://<span></span>localhost:{Port}/authenticated“ ein, um auf Ihren Controller zuzugreifen, der durch das **Authorize**-Attribut geschützt ist.

#### <a name="expected-results-after-access-to-a-protected-controller"></a>Erwartete Ergebnisse nach dem Zugriff auf einen geschützten Controller
Sie werden für die Verwendung der geschützten Controlleransicht zur Authentifizierung aufgefordert.

## <a name="additional-information"></a>Zusätzliche Informationen

<!--start-collapse-->
### <a name="protect-your-entire-website"></a>Schutz der gesamten Website
Um Ihre gesamte Website zu schützen, fügen Sie in der Datei **Global.asax** das Attribut **AuthorizeAttribute** zum Filter **GlobalFilters** in der Methode **Application_Start** hinzu:

```csharp
GlobalFilters.Filters.Add(new AuthorizeAttribute());
```
<!--end-collapse-->

### <a name="restrict-sign-in-access-to-your-application"></a>Beschränken des Anmeldezugriffs auf Ihre Anwendung
Standardmäßig können sich persönliche Konten wie solche bei outlook.com und live.com sowie andere Konten bei Ihrer Anwendung anmelden. Geschäfts-, Schul- oder Unikonten in Organisationen, die in Azure AD integriert sind, können sich standardmäßig ebenfalls anmelden.

Zum Beschränken des benutzerseitigen Anmeldezugriffs für Ihre Anwendung stehen mehrere Optionen zur Verfügung.

#### <a name="restrict-access-to-a-single-organization"></a>Beschränken des Zugriffs auf eine einzelne Organisation
Sie können den Anmeldezugriff für Ihre Anwendung auf bestimmte Benutzerkonten, die in einer einzelnen Azure AD-Organisation vorhanden sind, beschränken:
1. Ändern Sie in der Datei **Web.config** den Wert für den Parameter **Tenant**. Ändern Sie den Wert von **Common** in den Mandantennamen der Organisation, z.B. **contoso.onmicrosoft.com**.
2. Legen Sie in Ihrer OWIN-**Startup**-Klasse das **ValidateIssuer**-Argument auf **true** fest.

#### <a name="restrict-access-to-a-list-of-organizations"></a>Beschränken des Zugriffs auf eine Liste von Organisationen
Sie können den Anmeldezugriff auf bestimmte Benutzerkonten, die in einer Azure AD-Organisation in der Liste der zulässigen Organisationen enthalten sind, beschränken:
1. Legen Sie in der Datei **Web.config** in Ihrer **OWIN-Startup**-Klasse das **ValidateIssuer**-Argument auf **true** fest.
2. Legen Sie den Wert des Parameters **ValidIssuers** auf die Liste der zulässigen Organisationen fest.

#### <a name="use-a-custom-method-to-validate-issuers"></a>Überprüfen von Ausstellern mithilfe einer benutzerdefinierten Methode
Sie können eine benutzerdefinierte Methode implementieren, um Aussteller mit dem **IssuerValidator**-Parameter zu überprüfen. Weitere Informationen zur Verwendung dieses Parameters finden Sie auf MSDN unter [TokenValidationParameters-Klasse](https://msdn.microsoft.com/library/system.identitymodel.tokens.tokenvalidationparameters.aspx).

[!INCLUDE [Help and support](./active-directory-develop-help-support-include.md)]
