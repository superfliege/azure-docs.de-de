---
title: Azure Active Directory v2.0-Authentifizierungsbibliotheken | Microsoft Docs
description: Enthält eine Liste mit den kompatiblen Clientbibliotheken und Bibliotheken der Servermiddleware sowie Links zu verwandten Bibliotheken, Quellen und Beispielen für den Azure Active Directory v2.0-Endpunkt.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 19cec615-e51f-4141-9f8c-aaf38ff9f746
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/13/2018
ms.author: celested
ms.reviewer: jmprieur, saeeda
ms.custom: aaddev
ms.openlocfilehash: 1450cffca7a4cfa57856c75cdcc9e106958ea043
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/22/2018
ms.locfileid: "36319121"
---
# <a name="azure-active-directory-v20-authentication-libraries"></a>Azure Active Directory v2.0-Authentifizierungsbibliotheken

Der [Azure Active Directory (Azure AD) v2.0-Endpunkt](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-compare) unterstützt die Protokolle OAuth 2.0 und OpenID Connect 1.0 nach Branchenstandard. Die Microsoft Authentication Library (MSAL) ist für die Zusammenarbeit mit dem Azure AD v2.0-Endpunkt ausgelegt. Es ist auch möglich, Open-Source-Bibliotheken zu verwenden, die OAuth 2.0 und OpenID Connect 1.0 unterstützen.

Es wird empfohlen, von Protokollspezialisten geschriebene Bibliotheken zu verwenden, die sich an eine SDL-Methodik (Security Development Lifecycle) halten, z.B. [die SDL-Methodik von Microsoft][Microsoft-SDL]. Wenn Sie die Unterstützung für die Protokolle selbst schreiben möchten, sollten Sie sich nach einer Methodik wie Microsoft SDL richten und die Sicherheitsaspekte in den Standardspezifikationen für die einzelnen Protokolle sorgfältig beachten.

> [!NOTE]
> Suchen Sie nach den Azure AD v1.0-Bibliotheken (ADAL)? Sehen Sie den [Leitfaden zu den ADAL-Bibliotheken](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-libraries) ein.
>
>

## <a name="types-of-libraries"></a>Bibliothekstypen

Für den Azure AD v2.0-Endpunkt können zwei Arten von Bibliotheken verwendet werden:

* **Clientbibliotheken**. Für native Clients und Server werden Clientbibliotheken verwendet, um den Zugriff auf Token zum Aufrufen einer Ressource zu erhalten, z.B. Microsoft Graph.
* **Bibliotheken der Servermiddleware**. Web-Apps nutzen die Bibliotheken der Servermiddleware für die Benutzeranmeldung. Web-APIs nutzen die Bibliotheken der Servermiddleware zum Überprüfen von Token, die von nativen Clients oder anderen Servern gesendet werden.

## <a name="library-support"></a>Bibliotheksunterstützung

Da Sie sich bei Verwendung des v2.0-Endpunkts für eine beliebige standardkonforme Bibliothek entscheiden können, ist es wichtig zu wissen, wo Sie Hilfe erhalten. Wenden Sie sich bei Problemen und Featureanfragen in Bezug auf den Bibliothekscode an den Bibliotheksbesitzer. Wenden Sie sich bei Problemen und Featureanfragen in Bezug auf die dienstseitige Protokollimplementierung an Microsoft. [Erstellen Sie eine Featureanfrage](https://feedback.azure.com/forums/169401-azure-active-directory) zu zusätzlichen Features, die Sie sich für das Protokoll wünschen. [Erstellen Sie eine Supportanfrage](https://docs.microsoft.com/en-us/azure/azure-supportability/how-to-create-azure-support-request), wenn Sie ein Problem finden, bei dem der Azure AD v2.0-Endpunkt nicht mit OAuth 2.0 oder OpenID Connect 1.0 konform ist.

Bibliotheken werden in zwei Kategorien unterteilt:

* **Von Microsoft unterstützt**. Microsoft bietet Fixes für diese Bibliotheken an und hat sie in Bezug auf die SDL-Anforderungen umfassend geprüft.
* **Kompatibel**. Microsoft hat diese Bibliotheken in grundlegenden Szenarien getestet und bestätigt, dass sie mit dem v2.0-Endpunkt funktionieren. Microsoft stellt keine Fehlerbehebungen für diese Bibliotheken bereit und hat diese Prüfungen keinerlei Prüfungen unterzogen. Bei Problemen und Featureanfragen sollten Sie sich an die Community für das Open Source-Projekt der Bibliothek wenden.

In den nächsten Abschnitten dieses Artikels finden Sie eine Liste mit den Bibliotheken, die mit dem v2.0-Endpunkt verwendet werden können.

## <a name="microsoft-supported-client-libraries"></a>Von Microsoft unterstützte Clientbibliotheken

> [!IMPORTANT]
> Die MSAL-Vorschaubibliotheken eignen sich für den Einsatz in einer Produktionsumgebung. Microsoft stellt die gleiche Art von Produktionsunterstützung für diese Bibliotheken wie für die aktuellen Produktionsbibliotheken (ADAL) bereit. In der Vorschauphase sollten Sie damit rechnen, dass ohne vorherige Ankündigung Änderungen an der MSAL-API, am internen Cacheformat und an anderen Mechanismen dieser Bibliotheken vorgenommen werden, die neben Fehlerkorrekturen und Verbesserungen von Features erfolgen. Dies kann sich auf Ihre Anwendung auswirken. Für eine Änderung des Cacheformats kann es beispielsweise erforderlich sein, dass Benutzer sich erneut anmelden. Eine API-Änderung kann das Aktualisieren Ihres Codes erforderlich machen. Wenn das Release für allgemeine Verfügbarkeit (General Availability, GA) erhältlich ist, müssen alle Anwendungen, für die eine Vorschauversion der Bibliothek verwendet wird, innerhalb von sechs Monaten aktualisiert werden, da sie sonst ggf. nicht mehr funktionieren.

| Plattform | Bibliothek | Download | Quellcode | Beispiel | Verweis
| --- | --- | --- | --- | --- | --- |
| .NET-Client, Windows Store, UWP, Xamarin iOS und Android | MSAL .NET (Vorschau) |[NuGet](https://www.nuget.org/packages/Microsoft.Identity.Client) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) | [Desktop-App](guidedsetups/active-directory-mobileanddesktopapp-windowsdesktop-intro.md) |  |
| JavaScript | MSAL.js (Vorschau) | [GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-js) | [GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-js) | [Einzelseiten-App](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2) |  |
| iOS, macOS | MSAL (Vorschau) | [GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-objc) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-objc) | [iOS-App](https://github.com/Azure-Samples/active-directory-msal-ios-swift) |  |
| Android | MSAL (Vorschau) | [Das zentrale Repository](https://repo1.maven.org/maven2/com/microsoft/identity/client/msal/) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-android) | [Android-App](guidedsetups/active-directory-mobileanddesktopapp-android-intro.md) | [JavaDocs](http://javadoc.io/doc/com.microsoft.identity.client/msal) |

## <a name="microsoft-supported-server-middleware-libraries"></a>Von Microsoft unterstützte Bibliotheken der Servermiddleware

| Plattform | Bibliothek | Download | Quellcode | Beispiel | Verweis
| --- | --- | --- | --- | --- | --- |
| .NET 4.x | OWIN OpenID Connect-Middleware |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect) |[GitHub](https://github.com/aspnet/AspNetKatana/) |[MVC-App](guidedsetups/active-directory-serversidewebapp-aspnetwebappowin-intro.md) | |
| .NET 4.x | OWIN OAuth Bearer-Middleware für Azure AD |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.ActiveDirectory/) |[GitHub](https://github.com/aspnet/AspNetKatana/) |  | |
| .NET 4.x | JWT-Handler für .NET 4.5 | [NuGet](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt/4.0.4.403061554) | [GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |
| .NET Core | ASP.NET OpenID Connect-Middleware |[Microsoft.AspNetCore.Authentication.OpenIdConnect (NuGet)][ServerLib-NetCore-Owin-Oidc-Lib] |[ASP.NET Security (GitHub)][ServerLib-NetCore-Owin-Oidc-Repo] |[MVC-App](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-v2) |
| .NET Core | ASP.NET OAuth Bearer-Middleware |[Microsoft.AspNetCore.Authentication.OAuth (NuGet)][ServerLib-NetCore-Owin-Oauth-Lib] |[ASP.NET Security (GitHub)][ServerLib-NetCore-Owin-Oauth-Repo] |  |
| .NET Core | JWT-Handler für .NET Core  |[NuGet](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt) |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |
| Node.js |Azure AD Passport |[npm](https://www.npmjs.com/package/passport-azure-ad) |[GitHub](https://github.com/AzureAD/passport-azure-ad) | [Web-App](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs)| |

## <a name="compatible-client-libraries"></a>Kompatible Clientbibliotheken

| Plattform | Name der Bibliothek | Getestete Version | Quellcode | Beispiel |
|:---:|:---:|:---:|:---:|:---:|
| Android |[OIDCAndroidLib](https://github.com/kalemontes/OIDCAndroidLib/) |0.2.1 |[OIDCAndroidLib](https://github.com/kalemontes/OIDCAndroidLib) |[Beispiel mit nativer App](active-directory-v2-devquickstarts-android.md) |
| iOS |[NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) |1.2.8 |[NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) |[Beispiel mit nativer App](active-directory-v2-devquickstarts-ios.md) |
| JavaScript |[Hello.js](https://adodson.com/hello.js/) |1.13.5 |[Hello.js](https://github.com/MrSwitch/hello.js) |[SPA](https://github.com/Azure-Samples/active-directory-javascript-graphapi-web-v2) |
| Java | [Scribe Java scribejava](https://github.com/scribejava/scribejava) | [Version 3.2.0](https://github.com/scribejava/scribejava/releases/tag/scribejava-3.2.0) | [ScribeJava](https://github.com/scribejava/scribejava/) | |
| PHP | [The PHP League oauth2-Client](https://github.com/thephpleague/oauth2-client) | [Version 1.4.2](https://github.com/thephpleague/oauth2-client/releases/tag/1.4.2) | [oauth2-Client](https://github.com/thephpleague/oauth2-client/) | |
| Ruby |[OmniAuth](https://github.com/omniauth/omniauth/wiki) |omniauth:1.3.1</br>omniauth-oauth2:1.4.0 |[OmniAuth](https://github.com/omniauth/omniauth)</br>[OmniAuth OAuth2](https://github.com/intridea/omniauth-oauth2) |  |

## <a name="related-content"></a>Verwandte Inhalte
Weitere Informationen zum Azure AD v2.0-Endpunkt finden Sie in der [Übersicht über das Azure AD-App-Modell (v2.0)][AAD-App-Model-V2-Overview].

<!--Image references-->

<!--Reference style links -->
[AAD-App-Model-V2-Overview]: ../active-directory-appmodel-v2-overview.md
[ClientLib-NET-Lib]: http://www.nuget.org/packages/Microsoft.Identity.Client
[ClientLib-NET-Repo]: https://github.com/AzureAD/microsoft-authentication-library-for-dotnet
[ClientLib-NET-Sample]: active-directory-v2-devquickstarts-wpf.md
[ClientLib-Node-Lib]: https://www.npmjs.com/package/passport-azure-ad
[ClientLib-Node-Repo]: https://github.com/AzureAD/passport-azure-ad
[ClientLib-Node-Sample]:/
[ClientLib-Iosmac-Lib]:/
[ClientLib-Iosmac-Repo]:/
[ClientLib-Iosmac-Sample]:/
[ClientLib-Android-Lib]:/
[ClientLib-Android-Repo]:/
[ClientLib-Android-Sample]:/
[ClientLib-Js-Lib]:/
[ClientLib-Js-Repo]:/
[ClientLib-Js-Sample]:/

[Microsoft-SDL]: http://www.microsoft.com/sdl/default.aspx
[ServerLib-Net4-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/
[ServerLib-Net4-Owin-Oidc-Repo]: http://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oidc-Sample]: active-directory-v2-devquickstarts-dotnet-web.md
[ServerLib-Net4-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OAuth/
[ServerLib-Net4-Owin-Oauth-Repo]: http://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oauth-Sample]: https://azure.microsoft.com/documentation/articles/active-directory-v2-devquickstarts-dotnet-api/
[ServerLib-Net-Jwt-Lib]: https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt
[ServerLib-Net-Jwt-Repo]: https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet
[ServerLib-Net-Jwt-Sample]:/
[ServerLib-NetCore-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OpenIdConnect/
[ServerLib-NetCore-Owin-Oidc-Repo]: https://github.com/aspnet/Security
[ServerLib-NetCore-Owin-Oidc-Sample]: https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-v2
[ServerLib-NetCore-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OAuth/
[ServerLib-NetCore-Owin-Oauth-Repo]: https://github.com/aspnet/Security
[ServerLib-NetCore-Owin-Oauth-Sample]:/
[ServerLib-Node-Lib]: https://www.npmjs.com/package/passport-azure-ad
[ServerLib-Node-Repo]: https://github.com/AzureAD/passport-azure-ad/
[ServerLib-Node-Sample]: https://azure.microsoft.com/documentation/articles/active-directory-v2-devquickstarts-node-web/
