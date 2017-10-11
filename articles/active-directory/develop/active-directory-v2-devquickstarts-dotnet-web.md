---
title: Azure AD v2. 0 .NET web-app Signieren in den ersten Schritten | Microsoft Docs
description: "Wie Sie eine .NET MVC-Web-App erstellen, die Benutzer sich mit beiden persönlichen Microsoft-Account \"und\" Geschäfts-oder schulkonten signiert."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: c8b97ac6-0a06-4367-81b6-7d1d98152b14
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: ba5bdf7daba6086b70aec54ebe25d4445fa708c3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="add-sign-in-to-an-net-mvc-web-app"></a>Anmelden an eine .NET MVC-Web-app hinzufügen
Mit dem Endpunkt v2. 0 können Sie schnell die Authentifizierung für Ihre Web-apps mit Unterstützung für beide persönlichen Microsoft-Konten und Arbeits-oder schulkonten hinzufügen.  In ASP.NET Web-apps können Sie dazu mithilfe des Microsoft OWIN-Middleware, die in .NET Framework 4.5 enthalten.

> [!NOTE]
> Nicht alle Azure Active Directory – Szenarien und Funktionen werden von der v2. 0-Endpunkt unterstützt.  Erfahren Sie, um zu bestimmen, ob die v2. 0-Endpunkt verwendet werden sollte, [v2. 0-Einschränkungen](active-directory-v2-limitations.md).
>
>

 Hier wird eine Web-app erstellen, mit denen OWIN den Benutzer anmelden, einige Informationen über den Benutzer anzuzeigen, und melden Sie den Benutzer aus einer app.

## <a name="download"></a>Herunterladen
Der Code für dieses Lernprogramm wird beibehalten, [auf GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet).  Um nachzuvollziehen, können Sie [Herunterladen der app Skeleton als eine ZIP-](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) oder Klonen Sie das Gerüst:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

Abgeschlossene Anwendung wird am Ende dieses Lernprogramms ebenfalls bereitgestellt.

## <a name="register-an-app"></a>Registrieren einer app
Erstellen Sie eine neue app am [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), oder befolgen Sie diese [Ausführliche Schritte](active-directory-v2-app-registration.md).  Stellen Sie sicher, dass:

* Kopieren Sie die **Anwendungs-Id** an Ihre app zugewiesen, Sie benötigen ihn schnell.
* Hinzufügen der **Web** Plattform für Ihre app.
* Geben Sie den richtigen **Umleitungs-URI**. Die umleitungs-Uri gibt an, in Azure AD, in denen Authentifizierungsantworten geleitet werden – der Standardwert für dieses Lernprogramm ist `https://localhost:44326/`.

## <a name="install--configure-owin-authentication"></a>Installieren und Konfigurieren der OWIN-Authentifizierung
Konfigurieren Sie hier, um die OpenID Connect-Authentifizierungsprotokoll verwenden die OWIN-Middleware.  OWIN wird zum an- und Abmelde Anforderungen ausstellen, verwalten die Sitzung des Benutzers und Abrufen von Informationen zum Benutzer, unter anderem verwendet werden.

1. Um zu beginnen, öffnen Sie die `web.config` Datei im Stammverzeichnis des Projekts, und geben Sie Ihre app Konfigurationswerte in die `<appSettings>` Abschnitt.

  * Die `ida:ClientId` ist die **Anwendungs-Id** an Ihre app im registrierungsportal zugewiesen.
  * Die `ida:RedirectUri` ist die **Umleitungs-Uri** im Portal eingegebene.

2. Fügen Sie anschließend die OWIN-Middleware NuGet-Pakete zum Projekt mit der Paket-Manager-Konsole.

        ```
        PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
        PM> Install-Package Microsoft.Owin.Security.Cookies
        PM> Install-Package Microsoft.Owin.Host.SystemWeb
        ```  

3. Hinzufügen einer "OWIN Startklasse" zum Projekt aufgerufen `Startup.cs` rechts auf das Projekt-- **hinzufügen** --> **neues Element** --> Suchen Sie nach "OWIN".  Ruft die owin-Middleware die `Configuration(...)` Methode, wenn die app gestartet wird.
4. Ändern Sie die Klassendeklaration `public partial class Startup` -wir haben bereits Teil dieser Klasse für Sie implementiert, in einer anderen Datei.  In der `Configuration(...)` stellen einen Aufruf von ConfigureAuth(...) zum Einrichten der Authentifizierung für Ihre Web-app-Methode  

        ```C#
        [assembly: OwinStartup(typeof(Startup))]
        
        namespace TodoList_WebApp
        {
            public partial class Startup
            {
                public void Configuration(IAppBuilder app)
                {
                    ConfigureAuth(app);
                }
            }
        }
        ```

5. Öffnen Sie die Datei `App_Start\Startup.Auth.cs` und Implementieren der `ConfigureAuth(...)` Methode.  Die Parameter, die Sie, in angeben `OpenIdConnectAuthenticationOptions` dient als Koordinaten für die app für die Kommunikation mit Azure AD.  Sie müssen auch Cookieauthentifizierung einrichten – die OpenID Connect-Middleware verwendet Cookies im Hintergrund.

        ```C#
        public void ConfigureAuth(IAppBuilder app)
                     {
                             app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);
        
                             app.UseCookieAuthentication(new CookieAuthenticationOptions());
        
                             app.UseOpenIdConnectAuthentication(
                                     new OpenIdConnectAuthenticationOptions
                                     {
                                             // The `Authority` represents the v2.0 endpoint - https://login.microsoftonline.com/common/v2.0
                                             // The `Scope` describes the permissions that your app will need.  See https://azure.microsoft.com/documentation/articles/active-directory-v2-scopes/
                                             // In a real application you could use issuer validation for additional checks, like making sure the user's organization has signed up for your app, for instance.
        
                                             ClientId = clientId,
                                             Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, "common", "/v2.0"),
                                             RedirectUri = redirectUri,
                                             Scope = "openid email profile",
                                             ResponseType = "id_token",
                                             PostLogoutRedirectUri = redirectUri,
                                             TokenValidationParameters = new TokenValidationParameters
                                             {
                                                     ValidateIssuer = false,
                                             },
                                             Notifications = new OpenIdConnectAuthenticationNotifications
                                             {
                                                     AuthenticationFailed = OnAuthenticationFailed,
                                             }
                                     });
                     }
        ```

## <a name="send-authentication-requests"></a>Senden von authentifizierungsanforderungen
Ihre app ist jetzt ordnungsgemäß konfiguriert, um die Kommunikation mit dem v2. 0-Endpunkt mithilfe des OpenID Connect-Authentifizierungsprotokolls.  OWIN verfügt über alle problematischen Details zum Objekt Authentifizierungsnachrichten, Token von Azure AD überprüfen und Verwalten der benutzersitzung durchgeführt.  Alle bleibt besteht darin, Ihre Benutzer eine Möglichkeit zum Anmelden und Abmelden.

- Sie können Tags in Ihrer Domänencontroller erforderlich ist, diesen Benutzer anmeldet, bevor der Zugriff auf eine bestimmte Seite zu autorisieren.  Open `Controllers\HomeController.cs`, und fügen die `[Authorize]` -Tag, um die Info-Controller.
        
        ```C#
        [Authorize]
        public ActionResult About()
        {
          ...
        ```

- OWIN können auch direkt Authentifizierungsanfragen von innerhalb des Codes ausgeben.  Open `Controllers\AccountController.cs`.  Geben Sie in den Aktionen SignIn() und SignOut() Herausforderung OpenID Connect und Anforderungen für einmaliges abmelden.

        ```C#
        public void SignIn()
        {
            // Send an OpenID Connect sign-in request.
            if (!Request.IsAuthenticated)
            {
                HttpContext.GetOwinContext().Authentication.Challenge(new AuthenticationProperties { RedirectUri = "/" }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
            }
        }
        
        // BUGBUG: Ending a session with the v2.0 endpoint is not yet supported.  Here, we just end the session with the web app.  
        public void SignOut()
        {
            // Send an OpenID Connect sign-out request.
            HttpContext.GetOwinContext().Authentication.SignOut(CookieAuthenticationDefaults.AuthenticationType);
            Response.Redirect("/");
        }
        ```

- Öffnen Sie jetzt `Views\Shared\_LoginPartial.cshtml`.  Dies ist, in dem Sie die Benutzer Ihre app an- und Abmeldung Links anzeigen und drucken Sie den Namen des Benutzers in einer Ansicht.

        ```HTML
        @if (Request.IsAuthenticated)
        {
            <text>
                <ul class="nav navbar-nav navbar-right">
                    <li class="navbar-text">
        
                        @*The 'preferred_username' claim can be used for showing the user's primary way of identifying themselves.*@
        
                        Hello, @(System.Security.Claims.ClaimsPrincipal.Current.FindFirst("preferred_username").Value)!
                    </li>
                    <li>
                        @Html.ActionLink("Sign out", "SignOut", "Account")
                    </li>
                </ul>
            </text>
        }
        else
        {
            <ul class="nav navbar-nav navbar-right">
                <li>@Html.ActionLink("Sign in", "SignIn", "Account", routeValues: null, htmlAttributes: new { id = "loginLink" })</li>
            </ul>
        }
        ```

## <a name="display-user-information"></a>Anzeigen von Benutzerinformationen
Beim Authentifizieren von Benutzern mit OpenID Connect gibt der v2. 0-Endpunkt ein Id_token der App, die Ansprüche oder Assertionen bezüglich des Benutzers enthält.  Sie können diese Ansprüche Personalisieren Sie Ihre app verwenden:

- Öffnen der `Controllers\HomeController.cs` Datei.  Sie erreichen den Ansprüchen des Benutzers in Ihren Domänencontrollern über die `ClaimsPrincipal.Current` Security principal-Objekt.

        ```C#
        [Authorize]
        public ActionResult About()
        {
            ViewBag.Name = ClaimsPrincipal.Current.FindFirst("name").Value;
        
            // The object ID claim will only be emitted for work or school accounts at this time.
            Claim oid = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier");
            ViewBag.ObjectId = oid == null ? string.Empty : oid.Value;
        
            // The 'preferred_username' claim can be used for showing the user's primary way of identifying themselves
            ViewBag.Username = ClaimsPrincipal.Current.FindFirst("preferred_username").Value;
        
            // The subject or nameidentifier claim can be used to uniquely identify the user
            ViewBag.Subject = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
        
            return View();
        }
        ```

## <a name="run"></a>Ausführen
Schließlich erstellen Sie und führen Sie Ihrer app aus!   Melden Sie sich mit einem persönlichen Microsoft-Account oder ein Geschäfts- bzw. schulkonto ein, und beachten Sie, wie die Identität des Benutzers in der oberen Navigationsleiste wiedergegeben werden.  Sie haben jetzt eine Web-app mithilfe branchenüblicher Protokolle, die Benutzer mit ihren persönlichen und Arbeit/UNI-Konten authentifizieren können gesichert werden.

Zu Referenzzwecken im abgeschlossenen Beispiel (ohne die Konfigurationswerte) [dient als eine ZIP hier](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/complete.zip), oder Sie können von GitHub Klonen:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

## <a name="next-steps"></a>Nächste Schritte
Sie können jetzt auf Fortgeschrittene Themen verschieben.  Möglicherweise möchten versuchen:

[Sichern einer Web-API, mit der der Endpunkt v2. 0 >>](active-directory-devquickstarts-webapi-dotnet.md)

Weitere Ressourcen finden Sie:

* [Entwicklerhandbuch v2. 0 >>](active-directory-appmodel-v2-overview.md)
* [StackOverflow "Azure Active Directory"-Tag >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Sicherheitsupdates für unsere Produkte abrufen
Wir empfehlen Ihnen, Abrufen von Benachrichtigungen auf Sicherheitsvorfälle auftreten [auf dieser Seite](https://technet.microsoft.com/security/dd252948) und Sicherheit Advise-Benachrichtigungen abonnieren.
