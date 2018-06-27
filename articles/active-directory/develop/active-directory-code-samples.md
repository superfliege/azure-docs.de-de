---
title: Azure Active Directory-Codebeispiele | Microsoft-Dokumentation
description: Dieser Artikel enthält einen Index der Azure Active Directory-Codebeispiele (V1-Endpunkt), geordnet nach Szenario.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: mtillman
editor: ''
ms.assetid: a242a5ff-7300-40c2-ba83-fb6035707433
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/24/2018
ms.author: celested
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 5d7f0d1fc32f18991be6614bb7661b63570a8700
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36264921"
---
# <a name="azure-active-directory-code-samples-v1-endpoint"></a>Azure Active Directory-Codebeispiele (V1-Endpunkt)

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Sie können Microsoft Azure Active Directory (Azure AD) verwenden, um Ihren Webanwendungen und Web-APIs Authentifizierung und Autorisierung hinzuzufügen.

Dieser Abschnitt enthält Links zu Beispielen, die Sie verwenden können, um weitere Informationen zum V1-Endpunkt von Azure AD zu erhalten. Anhand dieser Beispiele wird die Vorgehensweise erläutert, und die Codeausschnitte können Sie in Ihren Anwendungen verwenden. Auf der Codebeispielseite finden Sie ausführliche Infothemen mit hilfreichen Angaben zu Anforderungen, Installation und Einrichtung. Außerdem ist der Code kommentiert, um die wichtigsten Abschnitte besser verständlich zu machen.

> [!NOTE]
> Wenn Sie an Azure AD-Codebeispielen für den V2-Endpunkt interessiert sind, lesen Sie [V2.0-Codebeispiele nach Szenario](active-directory-v2-code-samples.md).

Informationen zum grundlegenden Szenario für jeden Beispieltyp finden Sie unter [Authentifizierungsszenarien für Azure AD](active-directory-authentication-scenarios.md).

Schreiben Sie auch Beiträge zu unseren Beispielen auf GitHub. Wie das geht, erfahren Sie unter [Microsoft Azure Active Directory – Beispiele und Dokumentation](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="desktop-and-mobile-public-client-applications-calling-microsoft-graph-or-a-web-api"></a>Desktop und mobile öffentliche Clientanwendungen, die Microsoft Graph oder eine Web-API aufrufen

Die folgenden Beispiele veranschaulichen öffentliche Client-Anwendungen (Desktop/mobile Anwendungen), die im Namen eines Benutzers auf Microsoft Graph oder eine Web-API zugreifen.

Clientanwendung | Plattform | Ablauf/Erteilung | Aufrufen von Microsoft Graph | Aufrufen einer ASP.NET- oder ASP.NET Core 2.0-Web-API
------------------ | -------- | ---------- | -------------------- | -------------------------
Desktop (WPF)           | .NET/C# | Interactive | [dotnet-native-multitarget](https://github.com/azure-samples/active-directory-dotnet-native-multitarget) | [Dotnet-native-desktop](https://github.com/Azure-Samples/active-directory-dotnet-native-desktop) </p> [dotnet-native-aspnetcore](https://azure.microsoft.com/resources/samples/active-directory-dotnet-native-aspnetcore/)</p> [dotnet-webapi-manual-jwt-validation](https://github.com/azure-samples/active-directory-dotnet-webapi-manual-jwt-validation)
Mobil (UWP)            | .NET/C#  | Interactive | [dotnet-native-uwp-wam](https://github.com/azure-samples/active-directory-dotnet-native-uwp-wam) |  [dotnet-windows-store](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) (Web-API für einzelnen Mandanten) </p> [dotnet-webapi-multitenant-windows-store](https://github.com/Azure-Samples/active-directory-dotnet-webapi-multitenant-windows-store) (mehrinstanzenfähige Web-API)|
Mobil (Android, iOS, UWP)   | .NET/C# (Xamarin) | Interactive | [dotnet-native-multitarget](https://github.com/azure-samples/active-directory-dotnet-native-multitarget) |
Mobil (Android)           | Android/Java | Interactive |   [android](https://github.com/Azure-Samples/active-directory-android) |
Mobil (iOS)           | iOS/Objective C | Interactive |   [nativeClient-iOS](https://github.com/azureadquickstarts/nativeclient-ios) |
Desktop (Konsole)          | .NET/C# | Benutzername und Kennwort </p> Integrierte Windows-Authentifizierung | | [dotnet-native-headless](https://github.com/azure-samples/active-directory-dotnet-native-headless)
Desktop (Konsole)           | .NET Core/C# | Geräteprofil | | [dotnet-deviceprofile](https://github.com/Azure-Samples/active-directory-dotnet-deviceprofile)

## <a name="web-applications"></a>Webanwendungen

### <a name="web-applications-signing-in-users-calling-microsoft-graph-or-a-web-api-with-the-users-identity"></a>Webanwendungen, die Benutzer anmelden und Microsoft Graph oder eine Web-API mit der Identität eines Benutzers aufrufen

 Plattform | Nur Benutzeranmeldung | Aufrufen von Microsoft Graph oder AAD Graph| Aufrufen einer anderen ASP.NET- oder ASP.NET Core 2.0-Web-API
 -------- | ------------------- | --------------------- | -------------------------
ASP.NET 4.5 | [webApp-openidconnect-dotnet](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-aspnetwebapp-v1) </p> [webapp-WSFederation-dotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-wsfederation) | [dotnet-webapp-multitenant-openidconnect](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect) (AAD Graph) |
ASP.NET Core 2.0 | [dotnet-webapp-openidconnect-aspnetcore](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore) | [webapp-webapi-multitenant-openidconnect-aspnetcore](https://github.com/Azure-Samples/active-directory-webapp-webapi-multitenant-openidconnect-aspnetcore/) (AAD Graph) | [dotnet-webapp-webapi-openidconnect-aspnetcore](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore)
ASP.NET 4.5 | [dotnet-webapp-webapi-oauth2-useridentity](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity) | |
Python | | [python-webapp-graphapi](https://github.com/Azure-Samples/active-directory-python-webapp-graphapi)  |
Java | | [java-webapp-openidconnect](https://github.com/azure-samples/active-directory-java-webapp-openidconnect)  |
PHP | | [php-graphapi-web](https://github.com/Azure-Samples/active-directory-php-graphapi-web)  |

### <a name="web-applications-demonstrating-role-based-access-control-authorization"></a>Webanwendungen mit rollenbasierter Zugriffssteuerung (Autorisierung)

Die folgenden Beispiele zeigen die Implementierung der rollenbasierten Zugriffssteuerung, die zum Einschränken der Berechtigungen bestimmter Funktionen einer Webanwendung auf bestimmte Benutzer verwendet wird. Die Benutzerberechtigung hängt von der Zugehörigkeit zu einer Azure AD-Gruppe oder -Rolle ab.

Plattform | Beispiel | BESCHREIBUNG
 -------- | ------------------- | ---------------------
ASP.NET 4.5 | [dotnet-webapp-groupclaims](https://github.com/Azure-Samples/active-directory-dotnet-webapp-groupclaims) | Eine .NET 4.5 MVC-Web-App, die Azure AD-**Gruppen** für die Autorisierung verwendet
ASP.NET 4.5 | [dotnet-webapp-roleclaims](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) | Eine .NET 4.5 MVC-Web-App, die Azure AD-**Rollen** für die Autorisierung verwendet

## <a name="daemon-applications-accessing-web-apis-with-the-applications-identity"></a>Daemon-Anwendungen (Zugriff auf Web-APIs mit der Identität der Anwendung)

Die folgenden Beispiele veranschaulichen Desktop- oder Web-Anwendungen, die nicht mit einem Benutzer, sondern mit der Identität der Anwendung auf Microsoft Graph oder eine Web-API zugreifen.

Clientanwendung | Plattform | Ablauf/Erteilung | Aufrufen von Microsoft Graph | Aufrufen einer ASP.NET- oder ASP.NET Core 2.0-Web-API
------------------ | -------- | ---------- | -------------------- | -------------------------
Daemon-App (Konsole)          | .NET/C#  | Clientanmeldeinformationen mit App-Geheimnis oder Zertifikat | | [dotnet-daemon](https://github.com/azure-samples/active-directory-dotnet-daemon)</p> [dotnet-daemon-certificate-credential](https://github.com/azure-samples/active-directory-dotnet-daemon-certificate-credential)
Daemon-App (Konsole)         | .NET Core/C# | Clientanmeldeinformationen mit Zertifikat| | [dotnetcore-daemon-certificate-credential](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-certificate-credential)
Desktop            | Java | Client credentials (Clientanmeldeinformationen) |   [java-native-headless](https://github.com/azure-samples/active-directory-java-native-headless) |
ASP.NET-Web-App  | .NET/C# | Client credentials (Clientanmeldeinformationen) |    | [dotnet-webapp-webapi-oauth2-appidentity](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-appidentity)

## <a name="web-apis"></a>Web-APIs

### <a name="web-api-protected-by-azure-active-directory"></a>Durch Azure Active Directory geschützte Web-API

Im folgende Beispiel wird gezeigt, wie eine NodeJS-Web-API mit Azure AD geschützt wird.

Plattform | Beispiel | BESCHREIBUNG
 -------- | ------------------- | ---------------------
Node.js | [node-webapi](https://github.com/Azure-Samples/active-directory-node-webapi) |  NodeJS-Web-API, die mit Azure AD- und OAuth-2.0-Zugriffstoken gesichert ist.

### <a name="web-api-calling-microsoft-graph-or-another-web-api"></a>Web-API ruft Microsoft Graph oder eine andere Web-API auf

Die folgenden Beispiele zeigen eine Web-API, die eine andere Web-API aufruft. Im zweiten Beispiel wird gezeigt, wie der bedingte Zugriff behandelt wird.

 Plattform |  Aufrufen von Microsoft Graph | Aufrufen einer anderen ASP.NET- oder ASP.NET Core 2.0-Web-API
 -------- |  --------------------- | -------------------------
ASP.NET 4.5 | [dotnet-webapi-onbehalfof](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof) |[dotnet-webapi-onbehalfof](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof)
ASP.NET 4.5 | [dotnet-webapi-onbehalfof-ca](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof-ca) |[dotnet-webapi-onbehalfof-ca](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof-ca)

## <a name="single-page-applications"></a>Single-Page-Anwendungen

Dieses Beispiel zeigt, wie Sie eine Single-Page-Anwendung schreiben, die mit Azure AD gesichert ist.

 Plattform |  Aufrufen von Microsoft Graph | Aufrufen der eigenen API | Aufrufen einer anderen Web-API
 -------- |  --------------------- | ------------------ | ----------------
JavaScript / ASP.NET 4.x |  | [javascript-singlepageapp](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) |
JavaScript (AngularJS) / ASP.NET 4.x |  | [angularjs-singlepageapp](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp) |
JavaScript (AngularJS) / ASP.NET 4.x |  |  | [angularjs-singlepageapp-cors](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp-dotnet-webapi)

## <a name="other-microsoft-graph-samples"></a>Weitere Microsoft Graph-Beispiele

Beispiele und Lernprogramme, die verschiedene Verwendungsmuster für die Microsoft Graph-API (einschließlich Authentifizierung mit Azure AD) veranschaulichen, finden Sie unter [Microsoft Graph Community Samples & Tutorials](https://github.com/microsoftgraph/msgraph-community-samples).

## <a name="see-also"></a>Weitere Informationen

[Entwicklerhandbuch zu Azure Active Directory](active-directory-developers-guide.md)

[Azure AD-Graph-API – Konzepte und Referenz](https://msdn.microsoft.com/library/azure/hh974476.aspx)

[Bibliothek für das Azure AD Graph-API-Hilfsprogramm](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)
