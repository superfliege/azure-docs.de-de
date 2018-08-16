---
title: Grundlegendes zum Azure Active Directory-Anwendungsmanifest | Microsoft-Dokumentation
description: Sie erhalten ausführliche Informationen über das Azure Active Directory-Anwendungsmanifest, das die Identitätskonfiguration einer Anwendung in einem Azure AD-Mandanten darstellt, und für die OAuth-Autorisierung, den Zustimmungsprozess und andere Zwecke genutzt wird.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 4804f3d4-0ff1-4280-b663-f8f10d54d184
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/20/2017
ms.author: celested
ms.custom: aaddev
ms.reviewer: elisol, sureshja
ms.openlocfilehash: a0b39f5ac4347e48dc834bf5f5408c36df107aff
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2018
ms.locfileid: "39601074"
---
# <a name="azure-active-directory-application-manifest"></a>Azure Active Directory-Anwendungsmanifest
Apps, die in Azure AD integrieren werden, müssen bei einem Azure AD-Mandanten registriert werden. Eine solche App kann mit dem App-Manifest (unter dem Azure AD-Blatt) im [Azure-Portal](https://portal.azure.com) konfiguriert werden.

## <a name="manifest-reference"></a>Manifestreferenz

>[!div class="mx-tdBreakAll"]
>[!div class="mx-tdCol2BreakAll"]
|Schlüssel  |Werttyp |Beispielwert  |BESCHREIBUNG  |
|---------|---------|---------|---------|
|appID     |  Bezeichnerzeichenfolge       |""|  Der eindeutige Bezeichner für die Anwendung, die einer App von Azure AD zugewiesen wird.|
|appRoles     |    Arraytyp     |<code>[{<br>&emsp;"allowedMemberTypes": [<br>&emsp;&nbsp;&nbsp;&nbsp;"User"<br>&emsp;],<br>&emsp;"description":"Read-only access to device information",<br>&emsp;"displayName":"Read Only",<br>&emsp;"id":guid,<br>&emsp;"isEnabled":true,<br>&emsp;"value":"ReadOnly"<br>}]</code>|Die Auflistung der Rollen, die von einer Anwendung deklariert werden können. Diese Rollen können Benutzern, Gruppen oder Dienstprinzipalen zugewiesen werden.|
|availableToOtherTenants|boolean|`true`|Wenn dieser Wert auf „true“ festgelegt ist, ist die Anwendung für andere Mandanten verfügbar. Ist er auf „false“ festgelegt, ist die App nur für den Mandanten verfügbar, bei dem sie registriert ist. Weitere Informationen finden Sie unter [Anmelden von Azure Active Directory-Benutzern (AD) mit dem mehrinstanzenfähigen Anwendungsmuster](howto-convert-app-to-be-multi-tenant.md). |
|displayName     |Zeichenfolge         |`MyRegisteredApp`         |Der Anzeigename für die Anwendung. |
|errorURL     |Zeichenfolge         |`http://MyRegisteredAppError`         |Die URL für Fehler, die in einer Anwendung auftreten. |
|groupMembershipClaims     |    Zeichenfolge     |    `1`     |   Eine Bitmaske, die den in einem Benutzer- oder OAuth 2.0-Zugriffstoken ausgegebenen Anspruch „Groups“ konfiguriert, der von der Anwendung erwartet wird. Die Bitmaskenwerte sind: 0: keine, 1: Sicherheitsgruppen und Azure AD-Rollen, 2: reserviert und 4: reserviert. Durch das Festlegen der Bitmaske auf 7 werden alle Sicherheitsgruppen, Verteilergruppen und Azure AD-Verzeichnisrollen abgerufen, bei denen der angemeldete Benutzer ein Mitglied ist. |
|optionalClaims     |  Zeichenfolge       |     `null`    |    Die [optionalen Ansprüche](active-directory-optional-claims.md), die im Token vom Sicherheitstokendienst für diese spezifische App zurückgegeben werden. |
|acceptMappedClaims    |      boolean   | `true`        |    Wenn dieser Wert auf „true“ festgelegt ist, kann eine Anwendung die Anspruchszuordnung verwenden, ohne einen benutzerdefinierten Signaturschlüssel anzugeben.|
|homepage     |  Zeichenfolge       |`http://MyRegistererdApp`         |    Die URL zur Startseite der Anwendung. |
|identifierUris     |  Zeichenfolgenarray       | `http://MyRegistererdApp`        |   Benutzerdefinierte URIs, die eine Webanwendung innerhalb des zugehörigen Azure AD-Mandanten oder innerhalb einer überprüften benutzerdefinierten Domäne eindeutig identifizieren, wenn es sich um eine mehrinstanzenfähige Anwendung handelt. |
|keyCredentials     |   Arraytyp      | <code>[{<br>&nbsp;"customKeyIdentifier":null,<br>"endDate":"2018-09-13T00:00:00Z",<br>"keyId":"\<guid>",<br>"startDate":"2017-09-12T00:00:00Z",<br>"type":"AsymmetricX509Cert",<br>"usage":"Verify",<br>"value":null<br>}]</code>      |   Diese Eigenschaft enthält Verweise auf von der Anwendung zugewiesene Anmeldeinformationen, auf Zeichenfolgen basierende gemeinsame geheime Schlüssel und X.509-Zertifikate. Diese Anmeldeinformationen werden beim Anfordern von Zugriffstokens verwendet (wenn die App nicht als Ressource, sondern als Client agiert). |
|knownClientApplications     |     Arraytyp    |    [guid]     |     Dieser Wert wird zur Bündelung der Zustimmung verwendet, wenn Sie eine aus zwei Teilen bestehende Lösung haben (eine Clientanwendung und eine benutzerdefinierte Web-API-Anwendung). Wenn Sie hier die App-ID der Clientanwendung eingeben, muss der Benutzer nur einmal seine Zustimmung zur Verwendung der Clientanwendung geben. Azure AD weiß, dass die Zustimmung für die Clientanwendung implizit die Zustimmung zur Verwendung der Web-API beinhaltet, und stellt die Dienstprinzipale für die Client- und Web-API automatisch bereit. Die Client- und Web-API-Anwendung müssen beim selben Mandanten registriert sein.|
|logoutUrl     |   Zeichenfolge      |     `http://MyRegisteredAppLogout`    |   Die URL zur Abmeldung von der Anwendung. |
|oauth2AllowImplicitFlow     |   boolean      |  `false`       |       Dieser Wert gibt an, ob die Webanwendung implizite OAuth 2.0-Flowtoken anfordern kann. Die Standardeinstellung ist „false“. Dieses Flag wird für browserbasierte Apps wie JavaScript-basierte Single-Page-Webanwendungen verwendet. |
|oauth2AllowUrlPathMatching     |   boolean      |  `false`       |   Gibt an, ob Azure AD im Rahmen von OAuth 2.0-Tokenanforderungen den Pfadabgleich des Umleitungs-URI mit den replyUrls der Anwendung zulässt. Der Standardwert lautet „false“. |
|oauth2Permissions     | Arraytyp         |      <code>[{<br>"adminConsentDescription":"Allow the application to access resources on behalf of the signed-in user.",<br>"adminConsentDisplayName":"Access resource1",<br>"id":"\<guid>",<br>"isEnabled":true,<br>"type":"User",<br>"userConsentDescription":"Allow the application to access resource1 on your behalf.",<br>"userConsentDisplayName":"Access resources",<br>"value":"user_impersonation"<br>}]  </code> |  Die Sammlung von OAuth 2.0-Berechtigungsbereichen, die die Web-API-Anwendung (Ressource) für Clientanwendungen verfügbar macht. Diese Berechtigungsbereiche können Clientanwendungen im Zuge der Zustimmung gewährt werden. |
|oauth2RequiredPostResponse     | boolean        |    `false`     |      Gibt an, ob Azure AD im Rahmen von OAuth 2.0-Tokenanforderungen POST-Anforderungen zulässt (im Gengensatz zu GET-Anforderungen). Beim Standartwert „false“ sind nur GET-Anforderungen zulässig. 
|objectId     | Bezeichnerzeichenfolge        |     ""    |    Ein eindeutiger Bezeichner für die Anwendung im Verzeichnis. Diese ID ist nicht der Bezeichner, der verwendet wird, um die App in einer Protokolltransaktion zu bezeichnen. Der Bezeichner dient dazu, in Verzeichnisabfragen auf das Objekt zu verweisen.|
|passwordCredentials     | Arraytyp        |   <code>[{<br>"customKeyIdentifier":null,<br>"endDate":"2018-10-19T17:59:59.6521653Z",<br>"keyId":"\<guid>",<br>"startDate":"2016-10-19T17:59:59.6521653Z",<br>"value":null<br>}]  </code>    |    Informationen hierzu finden Sie in der Beschreibung der keyCredentials-Eigenschaft. |
|publicClient     |  boolean       |      `false`   | Gibt an, ob es sich bei einer Anwendung um einen öffentlichen Client handelt (beispielsweise eine installierte Anwendung auf einem mobilen Gerät). Die Standardeinstellung ist "false". |
|supportsConvergence     |  boolean       |   `false`      | Diese Eigenschaft darf nicht bearbeitet werden. Übernehmen Sie den Standardwert. |
|replyUrls     |  Zeichenfolgenarray       |   `http://localhost`     |  Diese Eigenschaft, die mehrere Werte zulässt, enthält die Liste der registrierten „redirect_uri“-Werte, die von Azure AD bei der Rückgabe von Tokens als Ziele akzeptiert werden. |
|requiredResourceAccess     |     Arraytyp    |    <code>[{<br>"resourceAppId":"00000002-0000-0000-c000-000000000000",<br>"resourceAccess":[{<br>&nbsp;&nbsp;&nbsp;&nbsp;"id":"311a71cc-e848-46a1-bdf8-97ff7156d8e6",<br>&nbsp;&nbsp;&nbsp;&nbsp;"type":"Scope"<br>&nbsp;&nbsp;}]<br>}]</code>    |   Gibt Ressourcen an, auf die diese Anwendung zugreifen muss, sowie den Satz von OAuth-Berechtigungsbereichen und -Anwendungsrollen, die unter den jeweiligen Ressourcen benötigt werden. Durch diese Vorkonfiguration des erforderlichen Ressourcenzugriffs wird die Zustimmungsoberfläche bestimmt.|
|resourceAppId     |    Bezeichnerzeichenfolge     |  ""      |   Der eindeutige Bezeichner für die Ressource, auf die die Anwendung zugreifen muss. Dieser Wert muss mit der „appId“ identisch sein, die für die Zielressourcenanwendung deklariert wurde. |
|resourceAccess     |  Arraytyp       | Siehe hierzu den Beispielwert für die RequiredResourceAccess-Eigenschaft. |   Die Liste der OAuth 2.0-Berechtigungsbereiche und -App-Rollen, die die Anwendung für die jeweilige Ressource erfordert (enthält die ID und die Typwerte der angegebenen Ressourcen)        |
|samlMetadataUrl    |Zeichenfolge| `http://MyRegisteredAppSAMLMetadata` |Die URL zu den SAML-Metadaten für die Anwendung.| 

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zu den Beziehungen zwischen den Anwendungs- und Dienstprinzipalobjekten einer Anwendung finden Sie unter [Anwendungs- und Dienstprinzipalobjekte in Azure AD](app-objects-and-service-principals.md).
* Unter [Azure AD-Entwicklerglossar](developer-glossary.md) finden Sie Definitionen für einige der wichtigsten Konzepte für Azure Active Directory-Entwickler (AD).

Bitte geben Sie uns über den folgenden Kommentarabschnitt Ihr Feedback, um uns bei der Verbesserung unserer Inhalte zu unterstützen.

<!--article references -->
[AAD-APP-OBJECTS]:app-objects-and-service-principals.md
[AAD-DEVELOPER-GLOSSARY]:../../../azure-ad-dev-glossary.md
[AAD-GROUPS-FOR-AUTHORIZATION]: http://www.dushyantgill.com/blog/2014/12/10/authorization-cloud-applications-using-ad-groups/
[ADD-UPD-RMV-APP]:quickstart-v1-integrate-apps-with-azure-ad.md
[APPLICATION-ENTITY]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[APPLICATION-ENTITY-APP-ROLE]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type
[APPLICATION-ENTITY-OAUTH2-PERMISSION]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type
[AZURE-PORTAL]: https://portal.azure.com
[DEV-GUIDE-TO-AUTH-WITH-ARM]: http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/
[GRAPH-API]: active-directory-graph-api.md
[IMPLICIT-GRANT]:v1-oauth2-implicit-grant-flow.md
[INTEGRATING-APPLICATIONS-AAD]: https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
[O365-PERM-DETAILS]: https://msdn.microsoft.com/office/office365/HowTo/application-manifest
[O365-SERVICE-DAEMON-APPS]: https://msdn.microsoft.com/office/office365/howto/building-service-apps-in-office-365
[RBAC-CLOUD-APPS-AZUREAD]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/

