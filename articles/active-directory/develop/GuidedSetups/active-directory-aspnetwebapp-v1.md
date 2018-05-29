---
title: 'Azure AD v1 – ASP.NET-Webserver: erste Schritte | Microsoft-Dokumentation'
description: Informationen zum Implementieren einer Microsoft-Anmeldung in einer ASP.NET-Projektmappe mittels einer herkömmlichen webbrowserbasierten Anwendung unter Verwendung des Standards OpenID Connect
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/09/2018
ms.author: andret
ms.openlocfilehash: 00fef27529dd1d69c13ceaf22e16d2b7cd47801f
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
ms.locfileid: "32156460"
---
<!--start-intro-->
# <a name="add-sign-in-with-microsoft-to-an-aspnet-web-app"></a>Hinzufügen von „Mit Microsoft anmelden“ zu einer ASP.NET-Web-App

Diese Anleitung veranschaulicht das Implementieren von „Mit Microsoft anmelden“ mithilfe einer ASP.NET MVC-Projektmappe mit einer herkömmlichen browserbasierten Anwendung mittels OpenID Connect. 

Am Ende dieses Handbuchs akzeptiert Ihre Anwendung Anmeldungen von Geschäfts-, Schul- oder Unikonten aus Organisationen, die in Azure Active Directory integriert sind.

> [!NOTE]
> Diese geführte Einrichtung hilft Ihnen bei der Aktivierung von Anmeldungen über Geschäfts-, Schul- oder Unikonten in Ihrer ASP.NET-Anwendung. Wenn Sie die Anmeldung über persönliche Konten neben Geschäfts-, Schul- oder Unikonten aktivieren möchten, verwenden Sie den [v2-Endpunkt](../active-directory-v2-compare.md). Lesen Sie [diese geführte ASP.NET-Einrichtung für den v2-Endpunkt](./active-directory-aspnetwebapp.md) sowie [dieses Dokument](../active-directory-v2-limitations.md), in dem die aktuellen Einschränkungen des v2-Endpunkts erläutert werden.
<br/><br/>

<!--separator-->

> Für diese Anleitung ist Visual Studio 2015 Update 3 oder Visual Studio 2017 erforderlich.  Sie haben beides nicht?  [Laden Sie Visual Studio 2017 kostenlos herunter](https://www.visualstudio.com/downloads/)

## <a name="how-this-guide-works"></a>Funktionsweise dieser Anleitung

![Funktionsweise dieser Anleitung](media/active-directory-aspnetwebapp-v1/aspnet-intro.png)

Diese Anleitung basiert auf dem Szenario, in dem ein Browser auf eine ASP.NET-Website zugreift und ein Benutzer aufgefordert wird, sich über die Schaltfläche „Anmelden“ anzumelden. In diesem Szenario wird ein Großteil der Arbeit zum Rendern der Webseite auf dem Server erledigt.

> [!NOTE]
> Diese geführte Einrichtung veranschaulicht die Anmeldung von Benutzern bei einer ASP.NET-Webanwendung – beginnend mit einer leeren Vorlage. Sie enthält Schritte wie das Hinzufügen einer Anmeldeschaltfläche sowie sämtliche Controller und Methoden und Erläuterungen zu einigen Konzepten. Alternativ können Sie auch ein Projekt für die Anmeldung von Azure Active Directory-Benutzern (Geschäfts-, Schul- oder Unikonten) mithilfe der [Visual Studio-Webvorlage](https://docs.microsoft.com/aspnet/visual-studio/overview/2013/creating-web-projects-in-visual-studio#organizational-account-authentication-options) erstellen und *Organisationskonten* und dann eine der Cloudoptionen auswählen. Bei dieser Option wird eine umfangreichere Vorlage mit zusätzlichen Controllern, Methoden und Ansichten verwendet.

## <a name="libraries"></a>Bibliotheken

In dieser Anleitung werden die folgenden Pakete verwendet:

|Bibliothek|BESCHREIBUNG|
|---|---|
|[Microsoft.Owin.Security.OpenIdConnect](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/)|Middleware, die einer Anwendung das Verwenden von OpenIDConnect für die Authentifizierung ermöglicht|
|[Microsoft.Owin.Security.Cookies](https://www.nuget.org/packages/Microsoft.Owin.Security.Cookies)|Middleware, die einer Anwendung das Beibehalten der Benutzersitzung mithilfe von Cookies ermöglicht|
|[Microsoft.Owin.Host.SystemWeb](https://www.nuget.org/packages/Microsoft.Owin.Host.SystemWeb)|Ermöglicht OWIN-basierten Anwendungen die Ausführung in IIS mithilfe der ASP.NET-Anforderungspipeline|


<!--end-intro-->

<!--start-setup-->

## <a name="set-up-your-project"></a>Einrichten des Projekts

Dieser Abschnitt zeigt die Schritte zum Installieren und Konfigurieren der Authentifizierungspipeline über OWIN-Middleware für ein ASP.NET-Projekt mithilfe von OpenID Connect. 

> Möchten Sie stattdessen das Visual Studio-Projekt dieses Beispiels herunterladen? [Laden Sie ein Projekt herunter](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/GuidedSetup.zip), und fahren Sie mit dem Schritt [Konfiguration](#configure-your-webconfig-and-register-an-application) fort, um das Codebeispiel vor der Ausführung zu konfigurieren.

## <a name="create-your-aspnet-project"></a>Erstellen des ASP.NET-Projekts
1. In Visual Studio: `File` > `New` > `Project`<br/>
2. Wählen Sie `ASP.NET Web Application (.NET Framework)` unter *Visual C# \Web* aus.
3. Geben Sie der Anwendung einen Namen, und klicken Sie auf *OK*.
4. Wählen Sie `Empty` aus, und aktivieren Sie dann das Kontrollkästchen, um `MVC`-Verweise hinzuzufügen.

## <a name="add-authentication-components"></a>Hinzufügen von Authentifizierungskomponenten

1. In Visual Studio: `Tools` > `Nuget Package Manager` > `Package Manager Console`
2. Fügen Sie *NuGet-Pakete für OWIN-Middleware* hinzu, indem Sie im Fenster „Paket-Manager-Konsole“ Folgendes eingeben:

    ```powershell
    Install-Package Microsoft.Owin.Security.OpenIdConnect
    Install-Package Microsoft.Owin.Security.Cookies
    Install-Package Microsoft.Owin.Host.SystemWeb
    ```

<!--start-collapse-->
> ### <a name="about-these-packages"></a>Zu diesen Paketen
>Die oben genannten Bibliotheken ermöglichen einmaliges Anmelden (Single Sign-On, SSO) mithilfe von OpenID Connect über die cookiebasierte Authentifizierung. Nachdem die Authentifizierung abgeschlossen ist und das Token, das den Benutzer darstellt, an Ihre Anwendung gesendet wurde, erstellt die OWIN-Middleware ein Sitzungscookie. Der Browser verwendet dann dieses Cookie bei nachfolgenden Anforderungen, damit der Benutzer keine erneute Authentifizierung durchführen muss. Es ist außerdem keine weitere Überprüfung erforderlich.
<!--end-collapse-->

## <a name="configure-the-authentication-pipeline"></a>Konfigurieren der Authentifizierungspipeline
Die folgenden Schritte dienen zum Erstellen der OWIN-Middleware-Klasse *Startup*, um die OpenID Connect-Authentifizierung zu konfigurieren. Diese Klasse wird automatisch ausgeführt.

> [!TIP]
> Wenn das Projekt nicht die Datei `Startup.cs` im Stammordner enthält:<br/>
> 1. Klicken Sie mit der rechten Maustaste auf den Stammordner des Projekts: >    `Add` > `New Item...` > `OWIN Startup class`<br/>
> 2. Vergeben Sie den Namen `Startup.cs`.<br/>
>
>> Stellen Sie sicher, dass die ausgewählte Klasse eine OWIN-Klasse des Typs „Startup“ und keine C#-Standardklasse ist. Bestätigen Sie dies, indem Sie überprüfen, ob `[assembly: OwinStartup(typeof({NameSpace}.Startup))]` über dem Namespace angezeigt wird.


1. Fügen Sie `Startup.cs` die Namespaces *OWIN* und *Microsoft.IdentityModel* hinzu:

    [!code-csharp[main](../../../../WebApp-OpenIDConnect-DotNet/WebApp-OpenIDConnect-DotNet\Startup.cs?name=AddedNameSpaces "Startup.cs")]

2. Ersetzen Sie die Startup-Klasse durch den folgenden Code:

    [!code-csharp[main](../../../../WebApp-OpenIDConnect-DotNet/WebApp-OpenIDConnect-DotNet\Startup.cs?name=Startup "Startup.cs")]
    
<!--start-collapse-->
> [!NOTE]
> Die Parameter, die Sie in *OpenIDConnectAuthenticationOptions* bereitstellen, dienen als Koordinaten für die Anwendung zur Kommunikation mit Azure AD. Da die OpenID Connect-Middleware Cookies verwendet, müssen Sie außerdem die Cookieauthentifizierung einrichten, wie im Code oben beschrieben. Der *ValidateIssuer*-Wert weist OpenIdConnect an, den Zugriff nicht auf eine bestimmte Organisation zu beschränken.
<!--end-collapse-->

<!--end-setup-->

<!--start-use-->

## <a name="add-a-controller-to-handle-sign-in-and-sign-out-requests"></a>Hinzufügen eines Controllers zum Verarbeiten von An- und Abmeldeanforderungen

Dieser Schritt veranschaulicht das Erstellen eines neuen Controllers, um An- und Abmeldeverfahren verfügbar zu machen.

1.  Klicken Sie mit der rechten Maustaste auf den Ordner `Controllers`, und wählen Sie `Add` > `Controller` aus.
2.  Wählen Sie `MVC (.NET version) Controller – Empty` aus.
3.  Klicken Sie auf *Hinzufügen*.
4.  Vergeben Sie den Namen `HomeController`, und klicken Sie auf *Hinzufügen*.
5.  Fügen Sie der Klasse *OWIN*-Namespaces hinzu:

    [!code-csharp[main](../../../../WebApp-OpenIDConnect-DotNet/WebApp-OpenIDConnect-DotNet\Controllers\HomeController.cs?name=AddedNameSpaces "HomeController.cs")]

6. Fügen Sie die folgenden Methoden hinzu, um die An- und Abmeldung bei Ihrem Controller zu verarbeiten, indem Sie eine Authentifizierungsaufforderung per Code auslösen:

    [!code-csharp[main](../../../../WebApp-OpenIDConnect-DotNet/WebApp-OpenIDConnect-DotNet\Controllers\HomeController.cs?name=SigInAndSignOut "HomeController.cs")]
    
## <a name="create-the-apps-home-page-to-sign-in-users-via-a-sign-in-button"></a>Erstellen der Startseite der App zum Anmelden von Benutzern über eine Anmeldeschaltfläche

Erstellen Sie in Visual Studio eine neue Ansicht, um die Schaltfläche „Anmelden“ hinzufügen und Benutzerinformationen nach der Authentifizierung anzuzeigen:

1.  Klicken Sie mit der rechten Maustaste auf den Ordner `Views\Home`, und wählen Sie `Add View` aus.
2.  Vergeben Sie den Namen `Index`.
3.  Fügen Sie der Datei den folgenden HTML-Code hinzu, der die Schaltfläche „Anmelden“ enthält:

    [!code-html[main](../../../../WebApp-OpenIDConnect-DotNet/WebApp-OpenIDConnect-DotNet/Views/Home/Index.cshtml "Index.cshtml")]

<!--start-collapse-->
> [!NOTE]
> Auf dieser Seite wird eine Anmeldeschaltfläche im SVG-Format mit schwarzem Hintergrund hinzugefügt:<br/>![Mit Microsoft anmelden](media/active-directory-aspnetwebapp-v1/aspnetsigninbuttonsample.png)<br/> Weitere Anmeldeschaltflächen finden Sie auf [dieser Seite](https://docs.microsoft.com/azure/active-directory/develop/active-directory-branding-guidelines "Brandingrichtlinien").
<!--end-collapse-->

## <a name="display-users-claims-by-adding-a-controller"></a>Anzeigen von Ansprüchen des Benutzers durch Hinzufügen eines Controllers
Dieser Controller veranschaulicht die Verwendungen des `[Authorize]`-Attributs, um einen Controller zu schützen. Dieses Attribut beschränkt den Zugriff auf den Controller, indem nur authentifizierte Benutzer zugelassen werden. Der folgende Code verwendet das Attribut zum Anzeigen von Benutzeransprüchen, die als Teil der Anmeldung abgerufen wurden.

1.  Klicken Sie mit der rechten Maustaste auf den Ordner `Controllers`: `Add` > `Controller`
2.  Wählen Sie `MVC {version} Controller – Empty` aus.
3.  Klicken Sie auf *Hinzufügen*.
4.  Vergeben Sie den Namen `ClaimsController`.
5.  Ersetzen Sie den Code Ihrer Controller-Klasse durch den folgenden Code. Dadurch wird der Klasse das `[Authorize]`-Attribut hinzugefügt:

    [!code-csharp[main](../../../../WebApp-OpenIDConnect-DotNet/WebApp-OpenIDConnect-DotNet\Controllers\ClaimsController.cs?name=ClaimsController "ClaimsController.cs")]

<!--start-collapse-->
> [!NOTE]
> Aufgrund der Verwendung des `[Authorize]`-Attributs können alle Methoden dieses Controllers nur ausgeführt werden, wenn der Benutzer authentifiziert ist. Wenn der Benutzer nicht authentifiziert ist und versucht, auf den Controller zuzugreifen, löst OWIN eine Authentifizierungsaufforderung aus und zwingt den Benutzer, sich zu authentifizieren. Der vorherige Code untersucht die Anspruchssammlung des Benutzers auf bestimmte Attribute, die im Token des Benutzers enthalten sind. Diesen Attribute enthalten den vollständigen Namen des Benutzers und den Benutzername sowie die GUID des Antragstellers. Sie enthalten auch die *Mandanten-ID*, die die ID der Organisation des Benutzers darstellt. 
<!--end-collapse-->

## <a name="create-a-view-to-display-the-users-claims"></a>Erstellen einer Ansicht zum Anzeigen der Ansprüche des Benutzers

Erstellen Sie in Visual Studio eine neue Ansicht zum Anzeigen der Ansprüche des Benutzers auf einer Webseite:

1.  Klicken Sie mit der rechten Maustaste auf den Ordner `Views\Claims`, und wählen Sie `Add View` aus.
2.  Vergeben Sie den Namen `Index`.
3.  Fügen Sie der Datei den folgenden HTML-Code hinzu:

    [!code-html[main](../../../../WebApp-OpenIDConnect-DotNet/WebApp-OpenIDConnect-DotNet/Views/Claims/Index.cshtml "Index.cshtml")]
    
<!--end-use-->

<!--start-configure-->
## <a name="configure-your-webconfig-and-register-an-application"></a>Konfigurieren der Datei *web.config* und Registrieren einer Anwendung

1. Fügen Sie in Visual Studio der Datei `web.config` (im Stammordner) im Abschnitt `configuration\appSettings` Folgendes hinzu:

    ```xml
    <add key="ClientId" value="Enter_the_Application_Id_here" />
    <add key="RedirectUrl" value="Enter_the_Redirect_Url_here" />
    <add key="Tenant" value="common" />
    <add key="Authority" value="https://login.microsoftonline.com/{0}" /> 
    ```
2. Wählen Sie im Projektmappen-Explorer das Projekt aus. Untersuchen Sie das Fenster <i>Eigenschaften</i> (wenn es nicht angezeigt wird, drücken Sie F4).
3. Ändern Sie „SSL-aktiviert“ in <code>True</code>.
4. Kopieren Sie die SSL-URL des Projekts in die Zwischenablage:<br/><br/>![Projekteigenschaften](media/active-directory-aspnetwebapp-v1/visual-studio-project-properties.png)<br />
5. Ersetzen Sie in <code>web.config</code> den Wert von <code>Enter_the_Redirect_URL_here</code> durch die SSL-URL Ihres Projekts. 

### <a name="register-your-application-in-the-azure-portal-then-add-its-information-to-webconfig"></a>Registrieren Ihrer Anwendung im Azure-Portal und Hinzufügen ihrer Informationen in *web.config*

1. Wechseln Sie zu [Microsoft Azure-Portal – App-Registrierungen](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps), um eine Anwendung zu registrieren.
2. Wählen Sie `New application registration`.
3. Geben Sie einen Namen für Ihre Anwendung ein.
4. Fügen Sie die *SSL-URL* des Visual Studio-Projekts in `Sign-on URL` ein. (Diese URL wird auch automatisch der Liste der Antwort-URLs für die Anwendung, die Sie registrieren, hinzugefügt.)
5. Klicken Sie auf `Create`, um die Anwendung zu registrieren. Dadurch gelangen Sie zurück zur Liste der Anwendungen.
6. Suchen Sie nun nach der Anwendung, die Sie gerade erstellt haben, und wählen Sie sie aus, um ihre Eigenschaften anzuzeigen.
7. Kopieren Sie die GUID unter `Application ID` in die Zwischenablage.
8. Wechseln Sie zurück zu Visual Studio, und ersetzen Sie in `web.config` den Wert von `Enter_the_Application_Id_here` durch die Anwendungs-ID der Anwendung, die Sie gerade registriert haben.

> [!TIP]
> Wenn Ihr Konto für den Zugriff auf mehrere Verzeichnisse konfiguriert ist, müssen Sie das richtige Verzeichnis für die Organisation, für die Sie die Anwendung registrieren möchten, auswählen. Klicken Sie dazu rechts oben im Azure-Portal auf Ihren Kontonamen, und überprüfen Sie das angegebene Verzeichnis:<br/>![Auswählen des richtigen Verzeichnisses](media/active-directory-aspnetwebapp-v1/tenantselector.png)

## <a name="configure-sign-in-options"></a>Konfigurieren von Anmeldeoptionen

Sie können Ihre Anwendung so konfigurieren, dass sich nur Benutzer anmelden dürfen, die zu einer Azure Active Directory-Instanz einer Organisation gehören, oder dass Anmeldungen von Benutzern beliebiger Organisation erlaubt sind. Führen Sie die Anweisungen für eine der folgenden Optionen aus:

### <a name="configure-your-application-to-allow-sign-ins-of-work-and-school-accounts-from-any-company-or-organization-multi-tenant"></a>Konfigurieren der Anwendung, damit Anmeldungen von Geschäfts-, Schul- oder Unikonten aller Unternehmen oder Organisationen (mehrere Mandanten) zulässig sind

Führen Sie die folgenden Schritte aus, wenn Sie Anmeldungen von Geschäfts-, Schul- oder Unikonten beliebiger Unternehmen oder Organisationen, die in Azure Active Directory integriert sind, zulassen möchten. Dies ist ein gängiges Szenario für *SaaS-Anwendungen*:

1. Wechseln Sie zurück zu [Microsoft Azure-Portal – App-Registrierungen](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps), und suchen Sie die Anwendung, die Sie gerade registriert haben.
2. Wählen Sie unter `All Settings` die Option `Properties` aus.
3. Ändern Sie die `Multi-tenanted`-Eigenschaft in `Yes`, und klicken Sie auf `Save`.

Weitere Informationen zu dieser Einstellung und zum Konzept von Anwendungen mit mehreren Mandanten finden Sie in [diesem Artikel](../active-directory-devhowto-multi-tenant-overview.md "Übersicht über mehrinstanzenfähige Anwendungen").

### <a name="restrict-users-from-only-one-organizations-active-directory-instance-to-sign-in-to-your-application-single-tenant"></a>Beschränken der Benutzer auf die Active Directory-Instanz nur einer Organisation für die Anmeldung bei der Anwendung (Einzelmandant)

Diese Option ist ein gängiges Szenario für *Branchenanwendungen*: Wenn Ihre Anwendung Anmeldungen nur von Konten akzeptieren soll, die einer bestimmten Azure Active Directory-Instanz angehören (einschließlich *Gastkonten* dieser Instanz), ersetzen Sie den `Tenant`-Parameter in *web.config* von `Common` durch den Mandantennamen der Organisation – z.B. *contoso.onmicrosoft.com*. Ändern Sie danach das `ValidateIssuer`-Argument in Ihrer [*OWIN-Klasse „Startup“*](#configure-the-authentication-pipeline) in `true`.

Um nur Benutzer in einer Liste angegebener Organisationen zuzulassen, legen Sie `ValidateIssuer` auf TRUE fest, und legen Sie mithilfe des `ValidIssuers`-Parameters eine Liste mit Organisationen fest.

Eine andere Möglichkeit ist das Implementieren einer benutzerdefinierten Methode, um mit dem *IssuerValidator*-Parameter Aussteller zu überprüfen. Weitere Informationen zu `TokenValidationParameters` finden Sie in [diesem MSDN-Artikel](https://msdn.microsoft.com/library/system.identitymodel.tokens.tokenvalidationparameters.aspx "MSDN-Artikel zu TokenValidationParameters").

<!--end-configure-->

<!--start-configure-arp-->
<!--
## Configure your ASP.NET Web App with the application's registration information

In this step, you will configure your project to use SSL, and then use the SSL URL to configure your application’s registration information. After this, add the application’ registration information to your solution via *web.config*.

1.  In Solution Explorer, select the project and look at the `Properties` window (if you don’t see a Properties window, press F4)
2.  Change `SSL Enabled` to `True`
3.  Copy the value from `SSL URL` above and paste it in the `Redirect URL` field on the top of this page, then click *Update*:<br/><br/>![Project properties](media/active-directory-aspnetwebapp-v1/vsprojectproperties.png)<br />
4.  Add the following in `web.config` file located in root’s folder, under section `configuration\appSettings`:

```xml
<add key="ClientId" value="[Enter the application Id here]" />
<add key="RedirectUri" value="[Enter the Redirect URL here]" />
<add key="Tenant" value="common" />
<add key="Authority" value="https://login.microsoftonline.com/{0}" /> 
```
-->
<!--end-configure-arp-->
<!--start-test-->
## <a name="test-your-code"></a>Testen Ihres Codes

Drücken Sie `F5`, um das Projekt in Visual Studio auszuführen. Der Browser wird geöffnet und leitet Sie zu *http://localhost:{port}* weiter, wo die Schaltfläche *Mit Microsoft anmelden* angezeigt wird. Fahren Sie fort, indem Sie darauf klicken, um sich anzumelden.

Wenn Sie einen Test durchführen möchten, verwenden Sie ein Geschäftskonto (Azure Active Directory), um sich anzumelden. 

![„Mit Microsoft anmelden“ im Browserfenster](media/active-directory-aspnetwebapp-v1/aspnetbrowsersignin.png)

![„Mit Microsoft anmelden“ im Browserfenster](media/active-directory-aspnetwebapp-v1/aspnetbrowsersignin2.png)

#### <a name="expected-results"></a>Erwartete Ergebnisse
Nach der Anmeldung wird der Benutzer zur Startseite Ihrer Website umgeleitet. Dabei handelt es sich um die HTTPS-URL, die im Microsoft-Anwendungsregistrierungsportal in Ihre Anwendungsregistrierungsinformationen eingegeben wurde. Diese Seite zeigt jetzt *Hello {Benutzer}*, einen Link zum Abmelden und einen Link zum Anzeigen der Ansprüche des Benutzers. Hierbei handelt es sich um einen Link zum Authorize-Controller, den Sie zuvor erstellt haben.

### <a name="see-users-claims"></a>Anzeigen der Ansprüche des Benutzers
Klicken Sie auf den Link, um die Ansprüche des Benutzers anzuzeigen. Dadurch gelangen Sie zum Controller und zur Ansicht, die nur Benutzern zur Verfügung steht, die authentifiziert wurden.

#### <a name="expected-results"></a>Erwartete Ergebnisse
 Es sollte eine Tabelle angezeigt werden, die die Standardeigenschaften des angemeldeten Benutzers enthält:

| Eigenschaft | Wert | BESCHREIBUNG|
|---|---|---|
| NAME | {Vollständiger Name des Benutzers} | Vor- und Nachname des Benutzers
|Username | <span>user@domain.com</span>| Benutzername zur Identifizierung des angemeldeten Benutzers
| Antragsteller| {Antragsteller}|Zeichenfolge zur eindeutigen Identifizierung der Benutzeranmeldung über das Web|
| Mandanten-ID| {GUID}| Eine *GUID*, die die Azure Active Directory-Organisation des Benutzers eindeutig darstellt.|

Darüber hinaus sehen Sie eine Tabelle mit allen Benutzeransprüchen, die in der Authentifizierungsanforderung enthalten sind. Eine Liste aller Ansprüche in einem ID-Token mit einer Erläuterung finden Sie in [diesem Artikel](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims "Liste der Ansprüche im ID-Token").


### <a name="test-accessing-a-method-that-has-an-authorize-attribute-optional"></a>Testen des Zugriffs auf eine Methode mit einem *[Authorize]*-Attribut (optional)
In diesem Schritt testen Sie den Zugriff auf den Claims-Controller als anonymer Benutzer:<br/>
Klicken Sie auf den Link zum Abmelden des Benutzers, und schließen Sie den Abmeldevorgang ab.<br/>
Geben Sie nun http://localhost:{port}/claims im Browser ein, um auf Ihren Controller zuzugreifen, der durch das `[Authorize]`-Attribut geschützt ist.

#### <a name="expected-results"></a>Erwartete Ergebnisse
Sie sollten zum Anmelden aufgefordert werden, um die Ansicht anzuzeigen.

## <a name="additional-information"></a>Zusätzliche Informationen

<!--start-collapse-->
### <a name="protect-your-entire-web-site"></a>Schützen der gesamten Website
Zum Schutz der gesamten Website fügen Sie in der Methode `Global.asax` `Application_Start` `AuthorizeAttribute` zu `GlobalFilters` hinzu:

```csharp
GlobalFilters.Filters.Add(new AuthorizeAttribute());
```
<!--end-collapse-->

<div></div>
<br/>

<!--end-test-->