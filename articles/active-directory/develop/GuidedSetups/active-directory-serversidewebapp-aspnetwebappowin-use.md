---
title: 'Azure AD v2 ASP.NET-Web Server erste verwenden wurde gestartet: | Microsoft Docs'
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
ms.openlocfilehash: 3b7d29e48c91f40e8782a5e32a52998b815fe331
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
## <a name="add-a-controller-to-handle-sign-in-and-sign-out-requests"></a>Fügen Sie einen Controller zum an- und Abmelde Anforderungen zu verarbeiten

Dieser Schritt veranschaulicht das Erstellen eines neuen Controllers zum an- und Abmelde Methoden verfügbar zu machen.

1.  Klicken Sie mit der rechten Maustaste auf die `Controllers` Ordner, und wählen`Add` > `Controller`
2.  Wählen Sie `MVC (.NET version) Controller – Empty`.
3.  Klicken Sie auf *hinzufügen*
4.  Nennen Sie sie `HomeController` , und klicken Sie auf *hinzufügen*
5.  Hinzufügen *OWIN* Verweise auf die Klasse:

```csharp
using Microsoft.Owin.Security;
using Microsoft.Owin.Security.Cookies;
using Microsoft.Owin.Security.OpenIdConnect;
```
<!-- Workaround for Docs conversion bug -->
<ol start="6">
<li>
Fügen Sie die beiden Methoden aus, um die Anmeldeseite zu behandeln und Abmeldung initiiert wird, eine authentifizierungsaufforderung über Code an den Controller hinzu:
</li>
</ol>

```csharp
/// <summary>
/// Send an OpenID Connect sign-in request.
/// Alternatively, you can just decorate the SignIn method with the [Authorize] attribute
/// </summary>
public void SignIn()
{
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(
            new AuthenticationProperties{ RedirectUri = "/" },
            OpenIdConnectAuthenticationDefaults.AuthenticationType);
    }
}

/// <summary>
/// Send an OpenID Connect sign-out request.
/// </summary>
public void SignOut()
{
    HttpContext.GetOwinContext().Authentication.SignOut(
            OpenIdConnectAuthenticationDefaults.AuthenticationType,
            CookieAuthenticationDefaults.AuthenticationType);
}
```

## <a name="create-the-apps-home-page-to-sign-in-users-via-a-sign-in-button"></a>Erstellen Sie die app-Startseite zur Anmeldung der Benutzer über eine Schaltfläche "Anmelden"

Erstellen Sie in Visual Studio eine neue Ansicht, um die Schaltfläche "Anmelden" hinzufügen und Anzeigen von Benutzerinformationen nach der Authentifizierung:

1.  Klicken Sie mit der rechten Maustaste auf die `Views\Home` Ordner, und wählen`Add View`
2.  Nennen Sie sie `Index`.
3.  Fügen Sie folgenden HTML-Code, der die Schaltfläche "Anmelden" enthält, in die Datei ein:

```html
<html>
<head>
    <meta name="viewport" content="width=device-width" />
    <title>Sign-In with Microsoft Guide</title>
</head>
<body>
@if (!Request.IsAuthenticated)
{
    <!-- If the user is not authenticated, display the sign-in button -->
    <a href="@Url.Action("SignIn", "Home")" style="text-decoration: none;">
        <svg xmlns="http://www.w3.org/2000/svg" xml:space="preserve" width="300px" height="50px" viewBox="0 0 3278 522" class="SignInButton">
        <style type="text/css">.fil0:hover {fill: #4B4B4B;} .fnt0 {font-size: 260px;font-family: 'Segoe UI Semibold', 'Segoe UI'; text-decoration: none;}</style>
        <rect class="fil0" x="2" y="2" width="3174" height="517" fill="black" />
        <rect x="150" y="129" width="122" height="122" fill="#F35325" />
        <rect x="284" y="129" width="122" height="122" fill="#81BC06" />
        <rect x="150" y="263" width="122" height="122" fill="#05A6F0" />
        <rect x="284" y="263" width="122" height="122" fill="#FFBA08" />
        <text x="470" y="357" fill="white" class="fnt0">Sign in with Microsoft</text>
        </svg>
    </a>
}
else
{
    <span><br/>Hello @System.Security.Claims.ClaimsPrincipal.Current.FindFirst("name").Value;</span>
    <br /><br />
    @Html.ActionLink("See Your Claims", "Index", "Claims")
    <br /><br />
    @Html.ActionLink("Sign out", "SignOut", "Home")
}
@if (!string.IsNullOrWhiteSpace(Request.QueryString["errormessage"]))
{
    <div style="background-color:red;color:white;font-weight: bold;">Error: @Request.QueryString["errormessage"]</div>
}
</body>
</html>
```
<!--start-collapse-->
### <a name="more-information"></a>Weitere Informationen
> Auf dieser Seite wird eine Anmeldeschaltfläche in SVG-Format mit schwarzem Hintergrund hinzugefügt:<br/>![Melden Sie sich mit Microsoft](media/active-directory-serversidewebapp-aspnetwebappowin-use/aspnetsigninbuttonsample.png)<br/> Weitere Anmeldeschaltflächen, wechseln Sie zu der [auf dieser Seite](https://docs.microsoft.com/azure/active-directory/develop/active-directory-branding-guidelines "Branding guidelines").
<!--end-collapse-->

## <a name="add-a-controller-to-display-users-claims"></a>Fügen Sie einen Controller zum Anzeigen von Ansprüchen des Benutzers
Dieser Controller veranschaulicht die Verwendungen der der `[Authorize]` Attribut einen Controller zu schützen. Dieses Attribut beschränkt den Zugriff auf den Controller nur zulassen, dass authentifizierte Benutzer. Der folgende Code verwendet das Attribut Benutzeransprüche, die abgerufen wurden als Teil der Anmeldeseite angezeigt wird.

1.  Klicken Sie mit der rechten Maustaste auf die `Controllers` Ordner:`Add` > `Controller`
2.  Wählen Sie `MVC {version} Controller – Empty`.
3.  Klicken Sie auf *hinzufügen*
4.  Nennen Sie Sie`ClaimsController`
5.  Ersetzen Sie den Code Ihrer Controller-Klasse mit den folgenden - Code Dies fügt die `[Authorize]` -Attribut der Klasse:

```csharp
[Authorize]
public class ClaimsController : Controller
{
    /// <summary>
    /// Add user's claims to viewbag
    /// </summary>
    /// <returns></returns>
    public ActionResult Index()
    {
        var claimsPrincipalCurrent = System.Security.Claims.ClaimsPrincipal.Current;
        //You get the user’s first and last name below:
        ViewBag.Name = claimsPrincipalCurrent.FindFirst("name").Value;

        // The 'preferred_username' claim can be used for showing the username
        ViewBag.Username = claimsPrincipalCurrent.FindFirst("preferred_username").Value;

        // The subject claim can be used to uniquely identify the user across the web
        ViewBag.Subject = claimsPrincipalCurrent.FindFirst(System.Security.Claims.ClaimTypes.NameIdentifier).Value;

        // TenantId is the unique Tenant Id - which represents an organization in Azure AD
        ViewBag.TenantId = claimsPrincipalCurrent.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;

        return View();
    }
}
```

<!--start-collapse-->
### <a name="more-information"></a>Weitere Informationen
> Aufgrund der Verwendung von der `[Authorize]` alle Methoden dieses Controllers-Attribut kann nur ausgeführt werden, wenn der Benutzer authentifiziert ist. Wenn der Benutzer nicht authentifiziert ist und versucht, den Controller zugreifen, wird OWIN initiieren eine authentifizierungsaufforderung und Authentifizierung des Benutzers zu erzwingen. Der Code oben untersucht die Auflistung der Ansprüche von der `ClaimsPrincipal.Current` Instanz für bestimmte Benutzerattribute, die in das Zugriffstoken des Benutzers enthalten. Diese Attribute enthalten die vollständigen Namen des Benutzers und Benutzername als auch den Betreff der globale User-Bezeichner. Es enthält auch die *Mandanten-ID*, der die ID für die Organisation des Benutzers darstellt. 
<!--end-collapse-->

## <a name="create-a-view-to-display-the-users-claims"></a>Erstellen Sie eine Ansicht zum Anzeigen von den Ansprüchen des Benutzers

Erstellen Sie in Visual Studio eine neue Ansicht zum Anzeigen von den Ansprüchen des Benutzers auf einer Webseite ein:

1.  Klicken Sie mit der rechten Maustaste auf die `Views\Claims` Ordner und:`Add View`
2.  Nennen Sie sie `Index`.
3.  Fügen Sie folgenden HTML-Code in die Datei ein:

```html
<html>
<head>
    <meta name="viewport" content="width=device-width" />
    <title>Sign-In with Microsoft Sample</title>
    <link href="@Url.Content("~/Content/bootstrap.min.css")" rel="stylesheet" type="text/css" />
</head>
<body style="padding:50px">
    <h3>Main Claims:</h3>
    <table class="table table-striped table-bordered table-hover">
        <tr><td>Name</td><td>@ViewBag.Name</td></tr>
        <tr><td>Username</td><td>@ViewBag.Username</td></tr>
        <tr><td>Subject</td><td>@ViewBag.Subject</td></tr>
        <tr><td>TenantId</td><td>@ViewBag.TenantId</td></tr>
    </table>
    <br />
    <h3>All Claims:</h3>
    <table class="table table-striped table-bordered table-hover table-condensed">
    @foreach (var claim in System.Security.Claims.ClaimsPrincipal.Current.Claims)
    {
        <tr><td>@claim.Type</td><td>@claim.Value</td></tr>
    }
</table>
    <br />
    <br />
    @Html.ActionLink("Sign out", "SignOut", "Home", null, new { @class = "btn btn-primary" })
</body>
</html>
```
