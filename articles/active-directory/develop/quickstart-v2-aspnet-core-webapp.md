---
title: 'Schnellstart: Azure AD v2.0 ASP.NET Core-Web-App | Microsoft Docs'
description: Erfahren Sie, wie „Bei Microsoft anmelden“ für eine ASP.NET Core-Web-App mithilfe von OpenID Connect implementiert wird.
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/25/2018
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: f57dc1707a9e25e4ea308142e804cdbc80d4308a
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46984941"
---
# <a name="add-sign-in-with-microsoft-to-an-aspnet-core-web-app"></a>Hinzufügen von „Bei Microsoft anmelden“ zu einer ASP.NET Core-Web-App

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

Dieser Schnellstart enthält ein Codebeispiel, das zeigt, wie eine ASP.NET Core-Web-App persönliche (hotmail.com, live.com, andere) und Geschäfts-, Schul- oder Unikonten aus einer beliebigen Azure Active Directory-Instanz anmelden kann.

![Funktionsweise der in diesem Schnellstart generierten Beispiel-App](media/quickstart-v2-aspnet-core-webapp/aspnetcorewebapp-intro.png)


> [!div renderon="docs"]
> ## <a name="register-your-application-and-download-your-quickstart-app"></a>Registrieren Ihrer Anwendung und Herunterladen Ihrer Schnellstart-App
>
> ### <a name="register-and-configure-your-application-and-code-sample"></a>Registrieren und Konfigurieren Ihrer Anwendung und des Codebeispiels
> #### <a name="step-1-register-your-application"></a>Schritt 1: Registrieren Ihrer Anwendung
> 
> 1. Navigieren Sie zum [Anwendungsregistrierungsportal von Microsoft](https://apps.dev.microsoft.com/portal/register-app).
> 1. Geben Sie einen Namen für Ihre Anwendung ein, stellen Sie sicher, dass die Option **Angeleitetes Setup** deaktiviert ist, und klicken Sie auf **Erstellen**.
> 1. Klicken Sie auf `Add Platform`, und wählen Sie dann `Web` aus.
> 1. Stellen Sie sicher, dass **Impliziten Fluss zulassen** *aktiviert* ist.
> 1. Geben Sie `https://localhost:3110/` unter **Umleitungs-URLs** ein.
> 1. Scrollen Sie auf der Seite nach unten, und klicken Sie auf **Speichern**.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-azure-portal"></a>Schritt 1: Konfigurieren Ihrer Anwendung im Azure-Portal
> Damit das Codebeispiel für diesen Schnellstart funktioniert, müssen Sie einen Antwort-URI als `http://localhost:3110/` hinzufügen.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Diese Änderung für mich vornehmen]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Bereits konfiguriert](media/quickstart-v2-aspnet-core-webapp/green-check.png): Ihre Anwendung ist mit diesem Attribut konfiguriert.

#### <a name="step-2-download-your-aspnet-core-project"></a>Schritt 2: Herunterladen des ASP.NET Core-Projekts

- [ASP.NET Core-Projekt herunterladen](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/archive/master.zip)

#### <a name="step-3-configure-your-project"></a>Schritt 3: Konfigurieren des Projekts

1. Extrahieren Sie die ZIP-Datei in einen lokalen Ordner (z.B. **C:\Azure-Samples**).
1. Wenn Sie Visual Studio 2017 verwenden, öffnen Sie das Projekt in Visual Studio (optional).
1. Bearbeiten Sie **appsettings.json**, und ersetzen Sie den Wert für `ClientId` durch die Anwendungs-ID aus der Anwendung, die Sie soeben registriert haben:

    ```json
    "ClientId": "Enter_the_Application_Id_here"
    "TenantId": "common"
    ```
1. Wenn Ihre Anwendung eine *Einzelmandantenanwendung* (nur für Konten im aktuellen Verzeichnis) ist, suchen Sie in Ihrer Datei **appsettings.json** nach dem Wert für `TenantId`, und ersetzen Sie `common` durch Ihre **Mandanten-ID** oder den **Mandantennamen** (z.B. contoso.microsoft.com). Sie können den Namen des Mandanten auf der **Seite „Übersicht“** abrufen.

## <a name="more-information"></a>Weitere Informationen

Dieser Abschnitt gibt einen Überblick über den Code, der für die Anmeldung von Benutzern erforderlich ist. Dies kann hilfreich sein, um die Funktionsweise des Codes und die Hauptargumente zu verstehen und zu ermitteln, ob Sie einer vorhandenen ASP.NET Core-Anwendung Anmeldung hinzufügen möchten.

### <a name="startup-class"></a>Startklasse

Die *Microsoft.AspNetCore.Authentication*-Middleware verwendet eine Startklasse, die ausgeführt wird, wenn der Hostprozess initialisiert wird:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(sharedOptions =>
    {
        sharedOptions.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        sharedOptions.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
    })
    .AddAzureAd(options => Configuration.Bind("AzureAd", options))
    .AddCookie();

    services.AddMvc();
}
```

Die Methode `AddAuthentication` konfiguriert den Dienst zum Hinzufügen von cookiebasierter Authentifizierung. Dies wird in Browserszenarien verwendet als sowie zum Festlegen der Aufforderung auf OpenIdConnect. 

Die Zeile, die `.AddAzureAd` enthält, fügt Ihrer Anwendung Azure Active Directory-Authentifizierung hinzu.

Außerdem fügt die Datei **AzureAdAuthenticationBuilderExtensions.cs** der AzureAd-Authentifizierungspipeline eine Erweiterungsmethode hinzu. Diese Erweiterungsmethode konfiguriert die Attribute, die für Azure AD-Authentifizierung erforderlich sind. Die `Configure`-Methode in der `IConfigureNamedOptions`-Schnittstelle enthält Folgendes:

```csharp
public void Configure(string name, OpenIdConnectOptions options)
{
    options.ClientId = _azureOptions.ClientId;
    options.Authority = $"{_azureOptions.Instance}common/v2.0";   // V2 specific
    options.UseTokenLifetime = true;
    options.RequireHttpsMetadata = false;
    options.TokenValidationParameters.ValidateIssuer = false;     // accept several tenants (here simplified)
}
```
> |Hierbei gilt:  |  |
> |---------|---------|
> |ClientId     |Die Anwendungs-ID der im Azure-Portal registrierten Anwendung.|
> |Authority | Der STS-Endpunkt für den zu authentifizierenden Benutzer. Normalerweise https://login.microsoftonline.com/{tenant}/v2.0 für die öffentliche Cloud, wobei {tenant} der Name Ihres Mandanten, Ihre Mandanten-ID oder *common* für einen Verweis auf den allgemeinen Endpunkt (verwendet für mehrinstanzenfähige Anwendungen) ist.|
> |UseTokenLifetime |Gibt an, dass das Authentifizierungscookie mit dem Authentifizierungscookie des Authentifizierungstokens übereinstimmen sollte.|
> |RequireHttpsMetadata     |Erfordern von HTTPS für die Metadatenadresse oder Zertifizierungsstelle. Es wird empfohlen, diesen Wert in `True` zu ändern.|
> |TokenValidationParameters     | Eine Liste von Parametern für die Tokenüberprüfung. In diesem Fall wird `ValidateIssuer` auf `false` festgelegt, um anzugeben, dass Anmeldungen von beliebigen persönlichen oder Geschäfts-, Schul- oder Unikonten akzeptiert werden können.|

### <a name="initiate-an-authentication-challenge"></a>Initiieren einer Authentifizierungsaufforderung

Sie können den Benutzer zur Anmeldung zwingen, indem Sie eine Authentifizierungsaufforderung in Ihrem Controller anfordern, wie in **AccountController.cs**:

```csharp
public IActionResult SignIn()
{
    var redirectUrl = Url.Action(nameof(HomeController.Index), "Home");
    return Challenge(
        new AuthenticationProperties { RedirectUri = redirectUrl },
        OpenIdConnectDefaults.AuthenticationScheme);
}
```

> [!TIP]
> Das Anfordern einer Authentifizierungsaufforderung mit der Methode oben ist optional und wird häufig verwendet, wenn Sie möchten, dass eine Ansicht sowohl für authentifizierte als auch für nicht authentifizierte Benutzer verfügbar ist. Sie können Controller mit der im nächsten Abschnitt beschriebenen Methode schützen.

### <a name="protect-a-controller-or-a-controllers-method"></a>Schützen eines Controllers oder der Methode eines Controllers

Sie können einen Controller oder Controllermethoden mithilfe des `[Authorize]`-Attributs schützen. Dieses Attribut schränkt den Zugriff auf den Controller oder seine Methoden ein, indem es nur authentifizierte Benutzer zulässt. Dies bedeutet, dass eine Authentifizierungsaufforderung für den Zugriff auf den Controller gestartet werden kann, wenn der Benutzer nicht authentifiziert ist.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie im GitHub-Repository für diesen ASP.NET Core-Schnellstart, einschließlich Anleitungen zum Hinzufügen von Authentifizierung zu einer brandneuen ASP.NET Core-Webanwendung:

> [!div class="nextstepaction"]
> [ASP.NET Core-Web-App-Codebeispiel](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]