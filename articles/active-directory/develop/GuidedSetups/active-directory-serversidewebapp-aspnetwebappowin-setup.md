---
title: 'Azure AD v2 ASP.NET-Web Server erste Setup wurde gestartet: | Microsoft Docs'
description: "Implementieren von Microsoft anmelden in einer ASP.NET-Projektmappe mit einer herkömmlichen Web browserbasierte Anwendung, die mit standardmäßigen OpenID Connect"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: ebf54f5a203adb7f0e5b0c47dcc07595e269e218
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
## <a name="set-up-your-project"></a>Richten Ihres Projekts ein

Dieser Abschnitt zeigt die Schritte zum Installieren und konfigurieren die authentifizierungspipeline über OWIN-Middleware auf einem ASP.NET-Projekt OpenID Connect verwenden. 

> Möchten Sie stattdessen Visual Studio-Projekt für dieses Beispiel herunterladen? [Herunterladen ein Projekts](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-DotNet/archive/master.zip) und fahren Sie mit der [Konfigurationsschritt](#create-an-application-express) im Codebeispiel wird vor der Ausführung zu konfigurieren.

<!--start-collapse-->
> ### <a name="create-your-aspnet-project"></a>Erstellen von Ihrem ASP.NET-Projekt

> 1. In Visual Studio:`File` > `New` > `Project`<br/>
> 2. Klicken Sie unter *Visual C# \Web*Option `ASP.NET Web Application (.NET Framework)`.
> 3. Benennen Sie Ihre Anwendung, und klicken Sie auf *OK*
> 4. Wählen Sie `Empty` und aktivieren Sie das Kontrollkästchen hinzuzufügende `MVC` Verweise
<!--end-collapse-->

## <a name="add-authentication-components"></a>Authentication-Komponenten hinzufügen

1. In Visual Studio:`Tools` > `Nuget Package Manager` > `Package Manager Console`
2. Hinzufügen *OWIN-Middleware NuGet-Pakete* Geben Sie Folgendes in der Paket-Manager-Konsole:

```powershell
Install-Package Microsoft.Owin.Security.OpenIdConnect
Install-Package Microsoft.Owin.Security.Cookies
Install-Package Microsoft.Owin.Host.SystemWeb
```

<!--start-collapse-->
> ### <a name="about-these-libraries"></a>Über diese Bibliotheken

>Die oben genannten Bibliotheken aktivieren Sie einmaliges Anmelden (SSO) über die Cookie-basierte Authentifizierung mithilfe der OpenID Connect. Nach der Authentifizierung abgeschlossen ist, und das Token, das den Benutzer darstellt, die für Ihre Anwendung gesendet wird, erstellt der OWIN-Middleware einen Sitzungscookie. Der Browser verwendet dieses Cookie dann bei nachfolgenden Anforderungen, damit der Benutzer sein Kennwort erneut eingeben muss und keine weitere Überprüfung erforderlich ist.
<!--end-collapse-->

## <a name="configure-the-authentication-pipeline"></a>Konfigurieren der authentifizierungspipeline
Die folgenden Schritte dienen zum Erstellen einer OWIN-Middleware Startklasse OpenID Connect-Authentifizierung konfigurieren. Diese Klasse wird automatisch ausgeführt werden, wenn die IIS-Prozess gestartet wird.

> Wenn das Projekt besitzt eine `Startup.cs` Datei im Stammordner:<br/>
> 1. Klicken Sie mit der rechten Maustaste auf den Stammordner des Projekts: >`Add` > `New Item...` > `OWIN Startup class`<br/>
> 2. Nennen Sie Sie`Startup.cs`

> Stellen Sie sicher, dass die ausgewählte Klasse eine OWIN-Startklasse und eine standardmäßige C#-Klasse nicht ist. Dies überprüfen, indem Sie überprüfen, ob Sie finden Sie unter `[assembly: OwinStartup(typeof({NameSpace}.Startup))]` über den Namespace.


1. Hinzufügen *OWIN* und *Microsoft.IdentityModel* Verweise auf `Startup.cs`:

```csharp
using Microsoft.Owin;
using Owin;
using Microsoft.IdentityModel.Protocols;
using Microsoft.Owin.Security;
using Microsoft.Owin.Security.Cookies;
using Microsoft.Owin.Security.OpenIdConnect;
using Microsoft.Owin.Security.Notifications;
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
Ersetzen Sie Startklasse, durch den folgenden Code:
</li>
</ol>

```csharp
public class Startup
{        
    // The Client ID is used by the application to uniquely identify itself to Azure AD.
    string clientId = System.Configuration.ConfigurationManager.AppSettings["ClientId"];

    // RedirectUri is the URL where the user will be redirected to after they sign in.
    string redirectUri = System.Configuration.ConfigurationManager.AppSettings["RedirectUri"];

    // Tenant is the tenant ID (e.g. contoso.onmicrosoft.com, or 'common' for multi-tenant)
    static string tenant = System.Configuration.ConfigurationManager.AppSettings["Tenant"];

    // Authority is the URL for authority, composed by Azure Active Directory v2 endpoint and the tenant name (e.g. https://login.microsoftonline.com/contoso.onmicrosoft.com/v2.0)
    string authority = String.Format(System.Globalization.CultureInfo.InvariantCulture, System.Configuration.ConfigurationManager.AppSettings["Authority"], tenant);

    /// <summary>
    /// Configure OWIN to use OpenIdConnect 
    /// </summary>
    /// <param name="app"></param>
    public void Configuration(IAppBuilder app)
    {
        app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

        app.UseCookieAuthentication(new CookieAuthenticationOptions());
            app.UseOpenIdConnectAuthentication(
            new OpenIdConnectAuthenticationOptions
            {
                // Sets the ClientId, authority, RedirectUri as obtained from web.config
                ClientId = clientId,
                Authority = authority,
                RedirectUri = redirectUri,
                // PostLogoutRedirectUri is the page that users will be redirected to after sign-out. In this case, it is using the home page
                PostLogoutRedirectUri = redirectUri,
                Scope = OpenIdConnectScopes.OpenIdProfile,
                // ResponseType is set to request the id_token - which contains basic information about the signed-in user
                ResponseType = OpenIdConnectResponseTypes.IdToken,
                // ValidateIssuer set to false to allow personal and work accounts from any organization to sign in to your application
                // To only allow users from a single organizations, set ValidateIssuer to true and 'tenant' setting in web.config to the tenant name
                // To allow users from only a list of specific organizations, set ValidateIssuer to true and use ValidIssuers parameter 
                TokenValidationParameters = new System.IdentityModel.Tokens.TokenValidationParameters() { ValidateIssuer = false },
                // OpenIdConnectAuthenticationNotifications configures OWIN to send notification of failed authentications to OnAuthenticationFailed method
                Notifications = new OpenIdConnectAuthenticationNotifications
                {
                    AuthenticationFailed = OnAuthenticationFailed
                }
            }
        );
    }

    /// <summary>
    /// Handle failed authentication requests by redirecting the user to the home page with an error in the query string
    /// </summary>
    /// <param name="context"></param>
    /// <returns></returns>
    private Task OnAuthenticationFailed(AuthenticationFailedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> context)
    {
        context.HandleResponse();
        context.Response.Redirect("/?errormessage=" + context.Exception.Message);
        return Task.FromResult(0);
    }
}

```
<!--start-collapse-->
> ### <a name="more-information"></a>Weitere Informationen

> Die Parameter, die Sie, in angeben *OpenIDConnectAuthenticationOptions* wie die Koordinaten für die Anwendung für die Kommunikation mit Azure AD dienen. Da die OpenID Connect-Middleware Cookies im Hintergrund verwendet wird, müssen Sie auch Cookieauthentifizierung einrichten, wie der Code oben zeigt. Die *ValidateIssuer* Wert weist OpenIdConnect nicht den Zugriff auf eine bestimmte Organisation einschränken.
<!--end-collapse-->

