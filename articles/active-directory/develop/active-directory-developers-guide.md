---
title: "Azure Active Directory für Entwickler | Microsoft-Dokumentation"
description: "Dieser Artikel enthält eine Übersicht über die Anmeldung bei Geschäfts-, Schul- oder Unikonten von Microsoft mit Azure Active Directory."
services: active-directory
author: dstrockis
manager: mtillman
editor: 
ms.assetid: 5c872c89-ef04-4f4c-98de-bc0c7460c7c2
ms.service: active-directory
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 89a232af6387f6403e6e341cced16d06e9979dae
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/10/2018
---
# <a name="azure-active-directory-for-developers"></a>Azure Active Directory für Entwickler
Azure Active Directory (Azure AD) ist ein Cloudidentitätsdienst, mit dem Entwickler eine sichere Anmeldung aller Benutzer umsetzen können, die über ein Geschäfts-, Schul- oder Unikonto von Microsoft verfügen. In dieser Dokumentation erfahren Sie, wie Sie Ihrer Anwendung Azure AD-Unterstützung mit branchenüblichen Authentifizierungsprotokollen (OAuth2.0 und OpenID Connect) hinzufügen.

| | |
| --- | --- |
|[Authentifizierungsszenarien für Azure AD](active-directory-authentication-scenarios.md) | Eine Einführung in die Authentifizierung mit Azure AD. |
|[Anwendungsarten](active-directory-authentication-scenarios.md#application-types-and-scenarios) | Eine Übersicht über die von Azure AD unterstützten Authentifizierungsszenarien. |                                
                                                                              
## <a name="get-started"></a>Erste Schritte
In den folgenden Einrichtungsanleitungen wird Schritt für Schritt die Anmeldung von Azure Active Directory-Benutzern unter Verwendung der Authentifizierungsbibliotheken von Microsoft beschrieben.

|  |  |  |  |
| --- | --- | --- | --- |
| <center>![Mobile Apps und Desktop-Apps](./media/active-directory-developers-guide/NativeApp_Icon.png)<br />Mobile Apps und Desktop-Apps</center> | [Übersicht](active-directory-authentication-scenarios.md#native-application-to-web-api)<br /><br />[iOS](active-directory-devquickstarts-ios.md)<br /><br />[Android](active-directory-devquickstarts-android.md) | [.NET (WPF)](active-directory-devquickstarts-dotnet.md)<br /><br />[.NET (UWP)](active-directory-devquickstarts-windowsstore.md)<br /><br />[Xamarin](active-directory-devquickstarts-xamarin.md) | [Cordova](active-directory-devquickstarts-cordova.md) |
| <center>![Web-Apps](./media/active-directory-developers-guide/Web_app.png)<br />Web-Apps</center> | [Übersicht](active-directory-authentication-scenarios.md#web-browser-to-web-application)<br /><br />[ASP.NET](active-directory-devquickstarts-webapp-dotnet.md)<br /><br />[Java](active-directory-devquickstarts-webapp-java.md) | [Node.js](active-directory-devquickstarts-openidconnect-nodejs.md) |  |
| <center>![Einzelseiten-Apps](./media/active-directory-developers-guide/SPA.png)<br />Einzelseiten-Apps</center> | [Übersicht](active-directory-authentication-scenarios.md#single-page-application-spa)<br /><br />[AngularJS](active-directory-devquickstarts-angular.md)<br /><br />[JavaScript](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) |  |  |
| <center>![Web-APIs](./media/active-directory-developers-guide/Web_API.png)<br />Web-APIs</center> | [Übersicht](active-directory-authentication-scenarios.md#web-application-to-web-api)<br /><br />[ASP.NET](active-directory-devquickstarts-webapi-dotnet.md)<br /><br />[Node.js](active-directory-devquickstarts-webapi-nodejs.md) | &nbsp; |
| <center>![Dienst-zu-Dienst](./media/active-directory-developers-guide/Service_App.png)<br />Dienst-zu-Dienst</center> | [Übersicht](active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api)<br /><br />[.NET](active-directory-code-samples.md#server-or-daemon-application-to-web-api)|  |

## <a name="how-to-guides"></a>Anleitungen
In den folgenden Anleitungen erfahren Sie, wie Sie mit Azure AD häufig anfallende Aufgaben erledigen.

|                                                                           |  |
|---------------------------------------------------------------------------| --- |
|[Anwendungsregistrierung](active-directory-integrating-applications.md)           | Enthält Informationen zum Registrieren einer Anwendung in Azure AD. |
|[Mehrinstanzenfähige Anwendungen](active-directory-devhowto-multi-tenant-overview.md)    | Enthält Informationen zum Anmelden bei einem Microsoft-Geschäftskonto. |
|[OAuth- und OpenID Connect-Protokoll](active-directory-protocols-openid-connect-code.md)| Enthält Informationen zum Anmelden von Benutzern und zum Aufrufen von Web-APIs unter Verwendung der Microsoft-Authentifizierungsprotokolle. |
|[Weitere Anleitungen](active-directory-developers-guide-index.md#guides)        |  Eine Liste mit verfügbaren Anleitungen für Azure AD.   |

## <a name="reference-topics"></a>Referenzthemen
Die folgenden Artikel enthalten ausführliche Informationen zu APIs, Protokollmeldungen und Begriffen, die in Azure AD verwendet werden.

|                                                                                   | |
| ----------------------------------------------------------------------------------| --- |
| [Authentifizierungsbibliotheken (ADAL)](active-directory-authentication-libraries.md)   | Eine Übersicht über die von Azure AD bereitgestellten Bibliotheken und SDKs. |
| [Codebeispiele](active-directory-code-samples.md)                                  | Eine Liste mit allen Azure AD-Codebeispielen. |
| [Glossar](active-directory-dev-glossary.md)                                      | Begriffe und Definitionen von Wörtern, die in dieser Dokumentation verwendet werden. |
| [Weitere Referenzthemen](active-directory-developers-guide-index.md#reference)| Eine Liste mit verfügbaren Referenzthemen für Azure AD.   |


> [!NOTE]
> Wenn Sie die Anmeldung mit persönlichen Microsoft-Konten durchführen möchten, empfiehlt sich unter Umständen die Verwendung des [Azure AD v2.0-Endpunkts](active-directory-appmodel-v2-overview.md). Der Azure AD v2.0-Endpunkt wird für die Zusammenführung von persönlichen Microsoft-Konten und Microsoft-Geschäftskonten (aus Azure AD) in einem gemeinsamen Authentifizierungssystem genutzt.


[!INCLUDE  [Help and support](../../../includes/active-directory-develop-help-support-include.md)]