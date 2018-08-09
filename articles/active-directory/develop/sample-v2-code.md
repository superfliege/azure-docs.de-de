---
title: Azure Active Directory-Codebeispiele | Microsoft-Dokumentation
description: Dieser Artikel enthält einen Index der verfügbaren Azure Active Directory-Codebeispiele (v2-Endpunkt), geordnet nach Szenario.
services: active-directory
documentationcenter: dev-center-name
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: a242a5ff-7300-40c2-ba83-fb6035707433
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/26/2018
ms.author: celested
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 0be97bfbbaafd6045fd36be57d85e917f0325779
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2018
ms.locfileid: "39600651"
---
# <a name="azure-active-directory-code-samples-v2-endpoint"></a>Azure Active Directory-Codebeispiele (v2-Endpunkt)

Sie können Microsoft Azure Active Directory (Azure AD) für Folgendes verwenden:

- Hinzufügen von Authentifizierungs- und Autorisierungsfeatures zu Ihren Webanwendungen und Web-APIs
- Anfordern eines Zugriffstokens für den Zugriff auf eine geschützte Web-API

In diesem Artikel wird der Azure AD v2-Endpunkt kurz erläutert, und es werden Links zu Beispielen angegeben. Anhand dieser Beispiele wird die Vorgehensweise erläutert, und die Codeausschnitte können Sie in Ihren Anwendungen verwenden. Auf der Codebeispielseite finden Sie ausführliche Infothemen mit hilfreichen Angaben zu Anforderungen, Installation und Einrichtung. Außerdem enthält der Code Kommentare, die die wichtigsten Abschnitte besser verständlich machen.

> [!NOTE]
> Wenn Sie an Beispielen zu v1 interessiert sind, lesen Sie [Azure Active Directory-Codebeispiele](sample-v1-code.md).

Informationen zum grundlegenden Szenario für die einzelnen Beispieltypen finden Sie unter [App-Typen für den Azure Active Directory v2.0-Endpunkt](v2-app-types.md).

Sie können auch Beiträge zu den Beispielen auf GitHub schreiben. Wie das geht, erfahren Sie unter [Microsoft Azure Active Directory – Beispiele und Dokumentation](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="desktop-and-mobile-public-client-apps"></a>Öffentliche Desktop- und mobile Client-Apps

Die folgenden Beispiele veranschaulichen öffentliche Clientanwendungen (Desktop-/mobile Anwendungen), die im Namen eines Benutzers auf Microsoft Graph oder eine Web-API zugreifen.

Clientanwendung | Plattform | Ablauf/Erteilung | Aufrufen von Microsoft Graph | Aufrufen einer ASP.NET Core 2.0-Web-API
------------------ | -------- | ---------- | -------------------- | -------------------------
Desktop (WPF)      | .NET/C#  | Interactive | [dotnet-desktop-msgraph-v2](http://github.com/azure-samples/active-directory-dotnet-desktop-msgraph-v2) <p/> [dotnet-admin-restricted-scopes-v2](https://github.com/azure-samples/active-directory-dotnet-admin-restricted-scopes-v2) | [dotnet-native-aspnetcore-v2](https://GitHub.com/azure-samples/active-directory-dotnet-native-aspnetcore-v2)
Mobil (UWP)   | .NET/C# (UWP) | Interactive | [dotnet-native-uwp-v2](https://github.com/azure-samples/active-directory-dotnet-native-uwp-v2) |
Mobil (Android, iOS, UWP)   | .NET/C# (Xamarin) | Interactive | [xamarin-native-v2](https://Github.com/azure-samples/active-directory-xamarin-native-v2) |
Mobil (iOS)       | iOS/Objective-C/Swift | Interactive | [ios-swift-native-v2](https://github.com/azure-samples/active-directory-ios-swift-native-v2) <p/> [ios-native-nxoauth2-v2](https://github.com/azure-samples/active-directory-ios-native-nxoauth2-v2) |
Mobil (Android)   | Android/Java | Interactive |   [android-native-v2](https://github.com/azure-samples/active-directory-android-native-v2 ) |

## <a name="web-applications"></a>Webanwendungen

In den folgenden Beispielen werden Webanwendungen veranschaulicht, die Benutzer anmelden und Microsoft Graph oder eine Web-API mit der Identität des Benutzers aufrufen.

 Plattform | Nur Benutzeranmeldung | Benutzeranmeldung und Aufruf von Microsoft Graph 
 -------- | ------------------- | --------------------------------- 
ASP.NET 4.x | [appmodelv2-webapp-openIDConnect-dotNet](https://GitHub.com/AzureAdQuickstarts/AppModelv2-WebApp-OpenIDConnect-DotNet) <p/> [dotnet-webapp-openidconnect-v2](https://GitHub.com/azure-samples/active-directory-dotnet-webapp-openidconnect-v2)  |              [aspnet-connect-rest-sample](https://github.com/microsoftgraph/aspnet-connect-rest-sample)   
ASP.NET Core 2.0 | [aspnetcore-webapp-openidconnect-v2](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2) |              [aspnetcore-connect-sample](https://github.com/microsoftgraph/aspnetcore-connect-sample)   
Node.js      |                   | [AppModelv2-WebApp-OpenIDConnect-nodejs](https://github.com/azureadquickstarts/appmodelv2-webapp-openidconnect-nodejs)     
Ruby      |                   | [ruby-connect-rest-sample](https://github.com/microsoftgraph/ruby-connect-rest-sample)     

## <a name="daemon-applications"></a>Daemonanwendungen

Die folgenden Beispiele veranschaulichen Desktop- oder Webanwendungen, die mit der Identität der Anwendung (ohne Benutzer) auf Microsoft Graph oder eine Web-API zugreifen.

Clientanwendung | Plattform | Ablauf/Erteilung | Aufrufen von Microsoft Graph 
------------------ | -------- | ---------- | -------------------- 
Web-App | .NET/C#  | Clientanmeldeinformationen | [dotnet-daemon-v2](https://github.com/azure-samples/active-directory-dotnet-daemon-v2) 

## <a name="single-page-applications-spa"></a>Single-Page-Webanwendungen (SPA)

Dieses Beispiel zeigt, wie Sie eine Single-Page-Anwendung schreiben, die mit Azure AD gesichert ist.

 Plattform |  Aufrufen von Microsoft Graph 
 -------- |  --------------------- 
JavaScript (msal.js)  | [javascript-graphapi-v2](https://github.com/azure-samples/active-directory-javascript-graphapi-v2) 
JavaScript (msal.js + AngularJS) | [angular-connect-rest-sample](https://github.com/microsoftgraph/angular-connect-rest-sample) 
JavaScript (Hello.JS)  | [javascript-graphapi-web-v2](https://github.com/azure-samples/active-directory-javascript-graphapi-web-v2) 
JavaScript (hello.js + Angular 4) | [angular4-connect-sample](https://github.com/microsoftgraph/angular4-connect-sample) 

## <a name="web-apis"></a>Web-APIs

### <a name="web-api-protected-by-azure-ad"></a>Durch Azure AD geschützte Web-API

Im folgenden Beispiel wird gezeigt, wie eine Web-API mit Azure AD v2-Endpunkt geschützt wird.

Plattform | Beispiel | BESCHREIBUNG
 -------- | ------------------- | ---------------------
Node.js | [dotnet-native-aspnetcore-v2](https://GitHub.com/azure-samples/active-directory-dotnet-native-aspnetcore-v2) | Ruft eine ASP.NET Core-Web-API aus einer WPF-Anwendung mithilfe von Azure AD v2 auf.

### <a name="web-api-calling-microsoft-graph-or-another-web-api"></a>Web-API ruft Microsoft Graph oder eine andere Web-API auf

Dieses Beispiel ist noch nicht verfügbar.

## <a name="other-microsoft-graph-samples"></a>Weitere Microsoft Graph-Beispiele

[Beispiele](https://github.com/microsoftgraph/msgraph-community-samples/tree/master/samples#aspnet) und Tutorials, die verschiedene Verwendungsmuster für die Microsoft Graph-API (einschließlich Authentifizierung mit Azure AD) veranschaulichen, finden Sie unter [Beispiele und Tutorials der Microsoft Graph-Community](https://github.com/microsoftgraph/msgraph-community-samples).

## <a name="see-also"></a>Weitere Informationen

[Entwicklerhandbuch zu Azure Active Directory](azure-ad-developers-guide.md)

[Azure AD-Graph-API – Konzepte und Referenz](https://msdn.microsoft.com/library/azure/hh974476.aspx)

[Bibliothek für das Azure AD Graph-API-Hilfsprogramm](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)
