---
title: 'Azure Active Directory B2C: Sichern Ihrer RESTful-Dienste unter Verwendung der HTTP-Standardauthentifizierung'
description: "Beispiel für die Sicherung Ihres benutzerdefinierten REST-API-Ansprücheaustauschs in Azure AD B2C unter Verwendung der HTTP-Standardauthentifizierung"
services: active-directory-b2c
documentationcenter: 
author: yoelhor
manager: joroja
editor: 
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 09/25/2017
ms.author: yoelh
ms.openlocfilehash: 226ecbe7e4d9d95fd4ba3255c95ec6baa1a86576
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="azure-active-directory-b2c-secure-your-restful-services-using-http-basic-authentication"></a>Azure Active Directory B2C: Sichern Ihrer RESTful-Dienste unter Verwendung der HTTP-Standardauthentifizierung
In einem [thematisch ähnlichen Artikel](active-directory-b2c-custom-rest-api-netfw.md) haben wir einen RESTful-Dienst (Web-API) erstellt, der sich ohne Authentifizierung in Azure AD B2C-User Journeys integriert. In diesem Artikel wird gezeigt, wie Sie die HTTP-Standardauthentifizierung zu Ihrem RESTful-Dienst hinzufügen können, sodass nur verifizierte Benutzer, einschließlich B2C, auf Ihre API zugreifen können. Mit der HTTP-Standardauthentifizierung legen Sie die Anmeldeinformationen des Benutzers (App-ID und App-Geheimnis) in Ihrer benutzerdefinierten Richtlinie fest, damit die Benutzer diese verwenden können. 

> [!NOTE]
>
>Im folgenden Artikel wird die [Standardauthentifizierung in der ASP.NET-Web-API](https://docs.microsoft.com/en-us/aspnet/web-api/overview/security/basic-authentication) eingehender beschrieben.

## <a name="prerequisites"></a>Voraussetzungen
Führen Sie die vorherigen Schritte im Artikel zum Thema [Integrieren von Anspruchsaustauschvorgängen der REST-API in Ihre Azure AD B2C User Journey](active-directory-b2c-custom-rest-api-netfw.md) durch.

## <a name="step-1-add-authentication-support"></a>1: Hinzufügen von Authentifizierungssupport

### <a name="step-11-add-application-settings-to-projects-webconfig-file"></a>1.1 Hinzufügen von Anwendungseinstellungen zur Datei „web.config“ des Projekts
1. Öffnen Sie das Visual Studio-Projekt, das Sie zuvor erstellt haben. 
2. Fügen Sie der Datei „web.config“ über das `appSettings`-Element folgende Anwendungseinstellungen hinzu:

    ```XML
    <add key="WebApp:ClientId" value="B2CServiceUserAccount" />
    <add key="WebApp:ClientSecret" value="your secret" />
    ```

3. Erstellen Sie ein Kennwort, und legen Sie den `WebApp:ClientSecret`-Wert fest.

    Um ein komplexes Kennwort zu generieren, können Sie PowerShell wie folgt ausführen. Sie können alternativ jeden beliebigen Wert verwenden.

    ```PowerShell
    $bytes = New-Object Byte[] 32
    $rand = [System.Security.Cryptography.RandomNumberGenerator]::Create()
    $rand.GetBytes($bytes)
    $rand.Dispose()
    [System.Convert]::ToBase64String($bytes)
    ```

### <a name="step-12-install-owin-libraries"></a>1.2 Installieren von OWIN-Bibliotheken
Fügen Sie dem Projekt zunächst über die Visual Studio-Paket-Manager-Konsole die NuGet-Pakete der OWIN-Middleware hinzu:

```
PM> Install-Package Microsoft.Owin
PM> Install-Package Owin
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

### <a name="step-13-add-authentication-middleware-class"></a>1.3 Hinzufügen der Authentifizierungs-Middleware-Klasse
Fügen Sie im `App_Start`-Ordner die `ClientAuthMiddleware.cs`-Klasse hinzu. Klicken Sie mit der rechten Maustaste auf den Ordner `App_Start`, wählen Sie **Hinzufügen**, und klicken Sie dann auf **Klasse**.

![Fügen Sie im Ordner „App_Start“ die Klasse „ClientAuthMiddleware.cs“ hinzu.](media/aadb2c-ief-rest-api-netfw-secure-basic/rest-api-netfw-secure-basic-OWIN-startup-auth1.png)

Geben Sie `ClientAuthMiddleware.cs` als Namen der Klasse ein.

![Erstellen Sie eine neue C#-Klasse.](media/aadb2c-ief-rest-api-netfw-secure-basic/rest-api-netfw-secure-basic-OWIN-startup-auth2.png)

Öffnen Sie die Datei `App_Start\ClientAuthMiddleware.cs`, und ersetzen Sie den Inhalt der Datei durch folgenden Code:


```C#

using Microsoft.Owin;
using System;
using System.Collections.Generic;
using System.Configuration;
using System.Linq;
using System.Security.Principal;
using System.Text;
using System.Threading.Tasks;
using System.Web;

namespace Contoso.AADB2C.API
{
    /// <summary>
    /// Class to create a custom owin middleware to check for client authentication
    /// </summary>
    public class ClientAuthMiddleware
    {
        private static readonly string ClientID = ConfigurationManager.AppSettings["WebApp:ClientId"];
        private static readonly string ClientSecret = ConfigurationManager.AppSettings["WebApp:ClientSecret"];

        /// <summary>
        /// Gets or sets the next owin middleware
        /// </summary>
        private Func<IDictionary<string, object>, Task> Next { get; set; }

        /// <summary>
        /// Initializes a new instance of the <see cref="ClientAuthMiddleware"/> class.
        /// </summary>
        /// <param name="next"></param>
        public ClientAuthMiddleware(Func<IDictionary<string, object>, Task> next)
        {
            this.Next = next;
        }

        /// <summary>
        /// Invoke client authentication middleware during each request.
        /// </summary>
        /// <param name="environment">Owin environment</param>
        /// <returns></returns>
        public Task Invoke(IDictionary<string, object> environment)
        {
            // Get wrapper class for the environment
            var context = new OwinContext(environment);

            // Check whether the authorization header is available. This contains the credentials.
            var authzValue = context.Request.Headers.Get("Authorization");
            if (string.IsNullOrEmpty(authzValue) || !authzValue.StartsWith("Basic ", StringComparison.OrdinalIgnoreCase))
            {
                // Process next middleware
                return Next(environment);
            }

            // Get credentials
            var creds = authzValue.Substring("Basic ".Length).Trim();
            string clientId;
            string clientSecret;

            if (RetrieveCreds(creds, out clientId, out clientSecret))
            {
                // Set transaction authenticated as client
                context.Request.User = new GenericPrincipal(new GenericIdentity(clientId, "client"), new string[] { "client" });
            }

            return Next(environment);
        }

        /// <summary>
        /// Retrieve credentials from header
        /// </summary>
        /// <param name="credentials">Authorization header</param>
        /// <param name="clientId">Client identifier</param>
        /// <param name="clientSecret">Client secret</param>
        /// <returns>True if valid credentials were presented</returns>
        private bool RetrieveCreds(string credentials, out string clientId, out string clientSecret)
        {
            string pair;
            clientId = clientSecret = string.Empty;

            try
            {
                pair = Encoding.UTF8.GetString(Convert.FromBase64String(credentials));
            }
            catch (FormatException)
            {
                return false;
            }
            catch (ArgumentException)
            {
                return false;
            }

            var ix = pair.IndexOf(':');
            if (ix == -1)
            {
                return false;
            }

            clientId = pair.Substring(0, ix);
            clientSecret = pair.Substring(ix + 1);

            // Return whether credentials are valid
            return (string.Compare(clientId, ClientAuthMiddleware.ClientID) == 0 &&
                string.Compare(clientSecret, ClientAuthMiddleware.ClientSecret) == 0);
        }
    }
}
```

### <a name="step-14-add-an-owin-startup-class"></a>1.4 Hinzufügen einer OWIN-Startklasse
Fügen Sie der API eine OWIN-Startklasse mit dem Namen `Startup.cs` hinzu. Klicken Sie mit der rechten Maustaste auf das Projekt, wählen Sie **Hinzufügen** und **Neues Element** aus, und suchen Sie dann nach „OWIN“.

![Hinzufügen einer OWIN-Startklasse](media/aadb2c-ief-rest-api-netfw-secure-basic/rest-api-netfw-secure-basic-OWIN-startup.png)

Öffnen Sie die Datei `Startup.cs`, und ersetzen Sie den Inhalt der Datei durch folgenden Code:

```C#
using Microsoft.Owin;
using Owin;

[assembly: OwinStartup(typeof(Contoso.AADB2C.API.Startup))]
namespace Contoso.AADB2C.API
{
    public class Startup
    {
        public void Configuration(IAppBuilder app)
        {
                app.Use<ClientAuthMiddleware>();
        }
    }
}
```

### <a name="step-15-protect-identity-api-class"></a>1.5 Schützen der Identity-API-Klasse
Öffnen Sie „Controllers\IdentityController.cs“, und fügen Sie der Controllerklasse das Tag `[Authorize]` hinzu. Das Tag `[Authorize]` schränkt den Zugriff auf den Controller für die Benutzer ein, die die Autorisierungsanforderung erfüllen.

![Fügen Sie das [Authorize]-Tag zum Controller hinzu.](media/aadb2c-ief-rest-api-netfw-secure-basic/rest-api-netfw-secure-basic-authorize.png)

## <a name="step-2-publish-to-azure"></a>2: Veröffentlichen in Azure
So veröffentlichen Sie Ihr Projekt: Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **Contoso.AADB2C.API**, und wählen Sie **Veröffentlichen** aus.

## <a name="step-3-add-the-restful-services-app-id--app-secret-to-azure-ad-b2c"></a>3: Hinzufügen von App-ID und App-Geheimnis für RESTful-Dienste in Azure AD B2C
Sobald Ihr RESTful-Dienst mithilfe der Client-ID (Benutzername) und einem geheimen Clientschlüssel geschützt ist, müssen Sie die Anmeldeinformationen in Ihrem Azure AD B2C-Mandanten speichern. Wenn Sie Ihre RESTful-Dienste aufrufen, stellt Ihre benutzerdefinierte Richtlinie die Anmeldeinformationen.  

### <a name="step-31-add-restful-services-client-id"></a>3.1 Hinzufügen von RESTful-Dienste-Client-ID
1.  Wechseln Sie zu Ihrem Azure AD B2C-Mandanten, und wählen Sie **B2C-Einstellungen** > **Framework für die Identitätsfunktion**.
2.  Wählen Sie **Policy Keys** (Richtlinienschlüssel), um die in Ihrem Mandanten verfügbaren Schlüssel anzuzeigen.
3.  Klicken Sie auf **+Hinzufügen**.
4.  Verwenden Sie für **Optionen** die Option **Manuell**.
5.  Verwenden Sie für **Name** den Wert `B2cRestClientId`.  
    Das Präfix `B2C_1A_` wird möglicherweise automatisch hinzugefügt.
6.  Geben Sie im Feld **Geheimnis** Ihre zuvor definierte App-ID ein.
7.  Verwenden Sie für **Schlüsselverwendung** die Option **Geheimnis**.
8.  Klicken Sie auf **Erstellen**
9.  Vergewissern Sie sich, dass Sie den Schlüssel `B2C_1A_B2cRestClientId` erstellt haben.

### <a name="step-32-add-restful-services-client-secret"></a>3.2 Hinzufügen des geheimen Clientschlüssels für RESTful-Dienste
1.  Wechseln Sie zu Ihrem Azure AD B2C-Mandanten, und wählen Sie **B2C-Einstellungen** > **Framework für die Identitätsfunktion**.
2.  Wählen Sie **Policy Keys** (Richtlinienschlüssel), um die in Ihrem Mandanten verfügbaren Schlüssel anzuzeigen.
3.  Klicken Sie auf **+Hinzufügen**.
4.  Verwenden Sie für **Optionen** die Option **Manuell**.
5.  Verwenden Sie für **Name** den Wert `B2cRestClientSecret`.  
    Das Präfix `B2C_1A_` wird möglicherweise automatisch hinzugefügt.
6.  Geben Sie im Feld **Geheimnis** Ihr zuvor definiertes App-Geheimnis ein.
7.  Verwenden Sie für **Schlüsselverwendung** die Option **Geheimnis**.
8.  Klicken Sie auf **Erstellen**
9.  Vergewissern Sie sich, dass Sie den Schlüssel `B2C_1A_B2cRestClientSecret` erstellt haben.

## <a name="step-4-change-the-technicalprofile-to-support-basic-authentication-in-your-extension-policy"></a>4: Ändern Sie `TechnicalProfile`, um die Standardauthentifizierung in Ihrer Erweiterungsrichtlinie zu unterstützen.
1.  Öffnen Sie die Erweiterungsrichtliniendatei (TrustFrameworkExtensions.xml) von Ihrem Arbeitsverzeichnis aus.
2.  Suchen Sie den `<TechnicalProfile>`-Knoten, der `Id="REST-API-SignUp"` enthält.
3.  Suchen Sie das `<Metadata>`-Element.
4.  Ändern Sie `AuthenticationType` in `Basic`.
```xml
<Item Key="AuthenticationType">Basic</Item>
```
5.  Fügen Sie den folgenden XML-Codeausschnitt unmittelbar nach dem Schließen des `<Metadata>`-Elements hinzu:  

    ```xml
    <CryptographicKeys>
        <Key Id="BasicAuthenticationUsername" StorageReferenceId="B2C_1A_B2cRestClientId" />
        <Key Id="BasicAuthenticationPassword" StorageReferenceId="B2C_1A_B2cRestClientSecret" />
    </CryptographicKeys>
    ```
Nach dem Hinzufügen der XML-Codeausschnitte sollte Ihr `TechnicalProfile` wie folgt aussehen:

![Standardauthentifizierungs-XML-Elemente hinzufügen](media/aadb2c-ief-rest-api-netfw-secure-basic/rest-api-netfw-secure-basic-add-1.png)

## <a name="step-5-upload-the-policy-to-your-tenant"></a>Schritt 5: Hochladen der Richtlinie in Ihren Mandanten

1.  Wechseln Sie im [Azure-Portal](https://portal.azure.com) in den [Kontext Ihres Azure AD B2C-Mandanten](active-directory-b2c-navigate-to-b2c-context.md), und klicken Sie auf **Azure AD B2C**.
2.  Wählen Sie **Framework für die Identitätsfunktion** aus.
3.  Klicken Sie auf **All Policies** (Alle Richtlinien).
4.  Wählen Sie **Richtlinie hochladen** aus.
5.  Aktivieren Sie das Kontrollkästchen **Richtlinie überschreiben, sofern vorhanden**.
6.  **Laden** Sie „TrustFrameworkExtensions.xml“ hoch, und stellen Sie sicher, dass kein Fehler bei der Validierung auftritt.

## <a name="step-6-test-the-custom-policy-by-using-run-now"></a>Schritt 6: Testen der benutzerdefinierten Richtlinie mit „Jetzt ausführen“
1.  Öffnen Sie die **Azure AD B2C-Einstellungen**, und navigieren Sie zu **Framework für die Identitätsfunktion**.

    >[!NOTE]
    >
    >    **Jetzt ausführen** macht erforderlich, dass mindestens eine Anwendung im Mandanten vorab registriert wird. 
    >    Informationen zum Registrieren von Anwendungen finden Sie in den Artikeln [Erste Schritte](active-directory-b2c-get-started.md) bzw. [Anwendungsregistrierung](active-directory-b2c-app-registration.md) zu Azure AD B2C.

2.  Öffnen Sie **B2C_1A_signup_signin** – die benutzerdefinierte Richtlinie der vertrauenden Seite (Relying Party, RP), die Sie hochgeladen haben. Wählen Sie **Jetzt ausführen** aus.
3.  Versuchen Sie es damit, „Test“ in das Feld **Vorname** einzugeben. B2C zeigt die Fehlermeldung am oberen Rand der Seite an.

    ![Testen Ihrer Identitäts-API](media/aadb2c-ief-rest-api-netfw-secure-basic/rest-api-netfw-test.png)

4.  Versuchen Sie es damit, einen Namen (nicht „Test“) in das Feld **Vorname** einzugeben. B2C meldet den Benutzer an und sendet dann „loyaltyNumber“ an Ihre Anwendung. Beachten Sie die Nummer in diesem JWT in diesem Beispiel.

```
{
  "typ": "JWT",
  "alg": "RS256",
  "kid": "X5eXk4xyojNFum1kl2Ytv8dlNP4-c57dO6QGTVBwaNk"
}.{
  "exp": 1507125903,
  "nbf": 1507122303,
  "ver": "1.0",
  "iss": "https://login.microsoftonline.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
  "aud": "e1d2612f-c2bc-4599-8e7b-d874eaca1ee1",
  "acr": "b2c_1a_signup_signin",
  "nonce": "defaultNonce",
  "iat": 1507122303,
  "auth_time": 1507122303,
  "loyaltyNumber": "290",
  "given_name": "Emily",
  "emails": ["B2cdemo@outlook.com"]
}
```

## <a name="next-steps"></a>Nächste Schritte
* [Verwenden Sie zum Sichern Ihrer RESTful-API Clientzertifikate.](active-directory-b2c-custom-rest-api-netfw-secure-cert.md)

## <a name="optional-download-the-complete-policy-files-and-code"></a>(Optional) Laden Sie die vollständige Richtliniendateien und den Code herunter
* Sie sollten nach Abschluss der exemplarischen Vorgehensweise „Erste Schritte mit benutzerdefinierten Richtlinien“ Ihr Szenario mithilfe Ihrer eigenen Dateien mit benutzerdefinierten Richtlinien erstellen, anstatt diese Beispieldateien zu verwenden.  [Beispielrichtliniendateien zur Referenz](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-rest-api-netfw-secure-basic)
* Sie können den vollständigen Code hier herunterladen: [Visual Studio-Beispiellösung zur Referenz](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-rest-api-netfw/Contoso.AADB2C.API).