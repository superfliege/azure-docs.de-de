---
title: Azure Active Directory für Entwickler | Microsoft-Dokumentation
description: Dieser Artikel enthält eine Übersicht über die Anmeldung bei Geschäfts-, Schul- oder Unikonten von Microsoft mit Azure Active Directory.
services: active-directory
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 5c872c89-ef04-4f4c-98de-bc0c7460c7c2
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/30/2018
ms.author: celested
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: f0f577e2ff18645cfa93763f2345ef4230907e09
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/14/2018
---
# <a name="azure-active-directory-for-developers"></a>Azure Active Directory für Entwickler

Azure Active Directory (Azure AD) ist ein Cloudidentitätsdienst, mit dem Entwickler Apps erstellen können, über die Benutzer sicher mit einem Geschäfts-, Schul- oder Unikonto von Microsoft angemeldet werden. Azure AD unterstützt sowohl Entwickler, die Branchen-Apps mit einem einzelnen Mandanten erstellen, als auch Entwickler, die mehrinstanzenfähige Apps erstellen möchten. Zusätzlich zur einfachen Anmeldung ermöglicht Azure AD Apps auch das Aufrufen von Microsoft-APIs, z.B. [Microsoft Graph](https://developer.microsoft.com/en-us/graph/docs/concepts/overview), und von benutzerdefinierten APIs, die basierend auf der Azure AD-Plattform erstellt werden. In dieser Dokumentation erfahren Sie, wie Sie Ihrer Anwendung Azure AD-Unterstützung mit branchenüblichen Authentifizierungsprotokollen wie OAuth2.0 und OpenID Connect hinzufügen.

> [!NOTE]
> Auf dieser Seite geht es hauptsächlich um den Azure AD v1.0-Endpunkt, der nur Geschäfts-, Schul- oder Unikonten von Microsoft unterstützt. Wenn Sie die Anmeldung für Endverbraucher oder persönliche Microsoft-Konten durchführen möchten, lesen Sie die Informationen zum [Azure AD v2.0-Endpunkt](active-directory-appmodel-v2-overview.md). Der Azure AD v2.0-Endpunkt stellt eine einheitliche Entwickleroberfläche für Apps bereit, bei denen sowohl Benutzer mit Azure AD-Konten (Geschäfts-, Schul- und Unikonten) als auch Benutzer mit persönlichen Microsoft-Konten angemeldet werden sollen.

| | |
| --- | --- |
|[Authentifizierungsszenarien für Azure AD](active-directory-authentication-scenarios.md) | Eine Einführung in die Authentifizierung mit Azure AD. |
|[Anwendungsarten](active-directory-authentication-scenarios.md#application-types-and-scenarios) | Eine Übersicht über die von Azure AD unterstützten Authentifizierungsszenarien. |      
| | |

## <a name="get-started"></a>Erste Schritte
In den folgenden Anleitungen wird Schritt für Schritt die Erstellung einer App auf Ihrer bevorzugten Plattform mit dem ADAL SDK (Azure AD Authentication Library) beschrieben. Informationen zur Verwendung der Microsoft Authentication Library (MSAL) finden Sie in der Dokumentation zum [Azure AD v2.0-Endpunkt](active-directory-appmodel-v2-overview.md).

|  |  |  |  |
| --- | --- | --- | --- |
| <center>![Mobile Apps und Desktop-Apps](./media/active-directory-developers-guide/NativeApp_Icon.png)<br />Mobile Apps und Desktop-Apps</center> | [Übersicht](active-directory-authentication-scenarios.md#native-application-to-web-api)<br /><br />[iOS](active-directory-devquickstarts-ios.md)<br /><br />[Android](active-directory-devquickstarts-android.md) | [.NET (WPF)](active-directory-devquickstarts-dotnet.md)<br /><br />[Xamarin](active-directory-devquickstarts-xamarin.md) | [Cordova](active-directory-devquickstarts-cordova.md) |
| <center>![Web-Apps](./media/active-directory-developers-guide/Web_app.png)<br />Web-Apps</center> | [Übersicht](active-directory-authentication-scenarios.md#web-browser-to-web-application)<br /><br />[ASP.NET](active-directory-devquickstarts-webapp-dotnet.md)<br /><br />[Java](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect) | [Python](https://github.com/Azure-Samples/active-directory-python-webapp-graphapi)<br/><br/> [Node.js](active-directory-devquickstarts-openidconnect-nodejs.md) | |
| <center>![Einzelseiten-Apps](./media/active-directory-developers-guide/SPA.png)<br />Einzelseiten-Apps</center> | [Übersicht](active-directory-authentication-scenarios.md#single-page-application-spa)<br /><br />[AngularJS](active-directory-devquickstarts-angular.md)<br /><br />[JavaScript](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) |  |  |
| <center>![Web-APIs](./media/active-directory-developers-guide/Web_API.png)<br />Web-APIs</center> | [Übersicht](active-directory-authentication-scenarios.md#web-application-to-web-api)<br /><br />[ASP.NET](active-directory-devquickstarts-webapi-dotnet.md)<br /><br />[Node.js](active-directory-devquickstarts-webapi-nodejs.md) | &nbsp; |
| <center>![Dienst-zu-Dienst](./media/active-directory-developers-guide/Service_App.png)<br />Dienst-zu-Dienst</center> | [Übersicht](active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api)<br /><br />[.NET](active-directory-code-samples.md#daemon-applications-accessing-web-apis-with-the-applications-identity)|  |
|  |  |  |  |  |

## <a name="how-to-guides"></a>Anleitungen
In den folgenden Anleitungen wird die Vorgehensweise für die gängigsten Aufgaben in Azure AD beschrieben.

|                                                                           |  |
|---------------------------------------------------------------------------| --- |
|[Anwendungsregistrierung](active-directory-integrating-applications.md)           | Enthält Informationen zum Registrieren einer Anwendung in Azure AD. |
|[Mehrinstanzenfähige Anwendungen](active-directory-devhowto-multi-tenant-overview.md)    | Enthält Informationen zum Anmelden bei einem Microsoft-Geschäftskonto. |
|[OAuth- und OpenID Connect-Protokoll](active-directory-protocols-openid-connect-code.md)| Enthält Informationen zum Anmelden von Benutzern und zum Aufrufen von Web-APIs unter Verwendung der Microsoft-Authentifizierungsprotokolle. |
|[Weitere Anleitungen](active-directory-developers-guide-index.md#guides)        |  Eine Liste mit verfügbaren Anleitungen für Azure AD. |
|  |  |

## <a name="reference-topics"></a>Referenzthemen
Die folgenden Artikel enthalten ausführliche Informationen zu APIs, Protokollmeldungen und Begriffen, die in Azure AD verwendet werden.

|                                                                                   | |
| ----------------------------------------------------------------------------------| --- |
| [Authentifizierungsbibliotheken (ADAL)](active-directory-authentication-libraries.md)   | Eine Übersicht über die von Azure AD bereitgestellten Bibliotheken und SDKs. |
| [Codebeispiele](active-directory-code-samples.md)                                  | Eine Liste mit allen Azure AD-Codebeispielen. |
| [Glossar](active-directory-dev-glossary.md)                                      | Begriffe und Definitionen von Wörtern, die in dieser Dokumentation verwendet werden. |
| [Weitere Referenzthemen](active-directory-developers-guide-index.md#reference)| Eine Liste mit verfügbaren Referenzthemen für Azure AD. |
|  |  |


[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
