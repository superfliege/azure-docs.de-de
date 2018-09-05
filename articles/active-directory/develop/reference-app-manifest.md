---
title: Grundlegendes zum Azure Active Directory-App-Manifest | Microsoft-Dokumentation
description: Sie erhalten ausführliche Informationen über das Azure Active Directory-App-Manifest, das die Identitätskonfiguration einer Anwendung in einem Azure AD-Mandanten darstellt, und für die OAuth-Autorisierung, den Zustimmungsprozess und andere Zwecke genutzt wird.
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
ms.date: 08/27/2018
ms.author: celested
ms.custom: aaddev
ms.reviewer: sureshja, justhu
ms.openlocfilehash: f336771da334ffd964ecd032654b8549b7a5e847
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2018
ms.locfileid: "43127019"
---
# <a name="azure-active-directory-app-manifest"></a>Azure Active Directory-App-Manifest

Apps, die in Azure Active Directory (Azure AD) integriert werden, müssen bei einem Azure AD-Mandanten registriert werden. Sie können die App im [Azure-Portal](https://portal.azure.com) konfigurieren, indem Sie **Azure Active Directory** aus, die zu konfigurierende App wählen und dann **Manifest** auswählen.

## <a name="manifest-reference"></a>Manifestreferenz

> [!NOTE]
> Wenn Sie die Beschreibungen nicht sehen können, maximieren Sie das Browserfenster, oder Scrollen/wischen Sie, um die Beschreibungen anzuzeigen.

>[!div class="mx-tdBreakAll"]
>[!div class="mx-tdCol2BreakAll"]

| Schlüssel  | Werttyp | Beispielwert | BESCHREIBUNG  |
|---------|---------|---------|---------|
| `appID` | Bezeichnerzeichenfolge | `"601790de-b632-4f57-9523-ee7cb6ceba95"` | Gibt den eindeutigen Bezeichner für die App an, die einer App von Azure AD zugewiesen wird. |
| `appRoles` | Arraytyp | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"allowedMemberTypes": [<br>&emsp;&nbsp;&nbsp;&nbsp;"User"<br>&nbsp;&nbsp;&nbsp;],<br>&nbsp;&nbsp;&nbsp;"description":"Read-only access to device information",<br>&nbsp;&nbsp;&nbsp;"displayName":"Read Only",<br>&nbsp;&nbsp;&nbsp;"id":guid,<br>&nbsp;&nbsp;&nbsp;"isEnabled":true,<br>&nbsp;&nbsp;&nbsp;"value":"ReadOnly"<br>&nbsp;&nbsp;}<br>]</code> | Gibt Auflistung der Rollen an, die von einer App deklariert werden können. Diese Rollen können Benutzern, Gruppen oder Dienstprinzipalen zugewiesen werden. |
| `availableToOtherTenants`| boolean | `true` | Wenn dieser Wert auf „true“ festgelegt ist, ist die App für andere Mandanten verfügbar. Ist er auf „false“ festgelegt, ist die App nur für den Mandanten verfügbar, bei dem sie registriert ist. Weitere Informationen finden Sie unter [Anmelden von Azure Active Directory-Benutzern mit dem mehrinstanzenfähigen Anwendungsmuster](howto-convert-app-to-be-multi-tenant.md). |
| `displayName` | Zeichenfolge | `MyRegisteredApp` | Der Anzeigename für die App. |
| `errorURL` | Zeichenfolge | `http://MyRegisteredAppError` | Die URL für Fehler, die in einer App auftreten. |
| `groupMembershipClaims` | Zeichenfolge | `1` | Eine Bitmaske, die den in einem Benutzer- oder OAuth 2.0-Zugriffstoken ausgegebenen Anspruch `groups` konfiguriert, der von der App erwartet wird. Die Bitmaskenwerte sind:<br>0: Keine<br>1: Sicherheitsgruppen und Azure AD-Rollen<br>2: Reserviert<br>4: Reserviert<br>Durch das Festlegen der Bitmaske auf 7 werden alle Sicherheitsgruppen, Verteilergruppen und Azure AD-Verzeichnisrollen abgerufen, bei denen der angemeldete Benutzer ein Mitglied ist. |
| `optionalClaims` | Zeichenfolge | `null` | Die optionalen Ansprüche, die im Token vom Sicherheitstokendienst für diese spezifische App zurückgegeben werden. Weitere Informationen finden Sie unter [Optionale Ansprüche](active-directory-optional-claims.md). |
| `acceptMappedClaims` | boolean | `true` | Wenn dieser Wert auf `true` festgelegt ist, kann eine App die Anspruchszuordnung verwenden, ohne einen benutzerdefinierten Signaturschlüssel anzugeben. |
| `homepage` | Zeichenfolge | `http://MyRegisteredApp` | Gibt die URL zur Startseite der App an. |
| `informationalUrls` | Zeichenfolge | <code>{<br>&nbsp;&nbsp;&nbsp;"privacy":"http://MyRegisteredApp/privacystatement",<br>&nbsp;&nbsp;&nbsp;"termsOfService":"http://MyRegisteredApp/termsofservice"<br>}</code> | Gibt die Links zu den Nutzungsbedingungen und Datenschutzbestimmungen der App an. Die Nutzungsbedingungen und Datenschutzbestimmungen werden auf der Oberfläche für die Benutzerzustimmung angezeigt. Weitere Informationen finden Sie unter [Nutzungsbedingungen und Datenschutzerklärung für registrierte Azure Active Directory-Apps](howto-add-terms-of-service-privacy-statement.md). |
| `identifierUris` | Zeichenfolgenarray | `http://MyRegistererdApp` | Benutzerdefinierte URIs, die eine Web-App innerhalb des zugehörigen Azure AD-Mandanten oder innerhalb einer überprüften benutzerdefinierten Domäne eindeutig identifizieren, wenn es sich um eine mehrinstanzenfähige App handelt. |
| `keyCredentials` | Arraytyp | <code>[<br>&nbsp;{<br>&nbsp;&nbsp;&nbsp;"customKeyIdentifier":null,<br>&nbsp;&nbsp;&nbsp;"endDate":"2018-09-13T00:00:00Z",<br>&nbsp;&nbsp;&nbsp;"keyId":"\<guid>",<br>&nbsp;&nbsp;&nbsp;"startDate":"2017-09-12T00:00:00Z",<br>&nbsp;&nbsp;&nbsp;"type":"AsymmetricX509Cert",<br>&nbsp;&nbsp;&nbsp;"usage":"Verify",<br>&nbsp;&nbsp;&nbsp;"value":null<br>&nbsp;&nbsp;}<br>]</code> | Enthält Verweise auf von der App zugewiesene Anmeldeinformationen, auf Zeichenfolgen basierende gemeinsame geheime Schlüssel und X.509-Zertifikate. Diese Anmeldeinformationen werden beim Anfordern von Zugriffstokens verwendet (wenn die App nicht als Ressource, sondern als Client agiert). |
| `knownClientApplications` | Arraytyp | `[GUID]` | Wird zur Bündelung der Zustimmung verwendet, wenn Sie eine aus zwei Teilen bestehende Lösung haben (eine Client-App und eine benutzerdefinierte Web-API-App). Wenn Sie hier die App-ID der Client-App eingeben, muss der Benutzer nur einmal seine Zustimmung zur Verwendung der Client-App geben. Azure AD weiß, dass die Zustimmung für die Clientanwendung implizit die Zustimmung zur Verwendung der Web-API beinhaltet, und stellt die Dienstprinzipale für die Client- und Web-API automatisch bereit. Die Client- und Web-API-App müssen beim selben Mandanten registriert sein. |
| `logoutUrl` | Zeichenfolge | `http://MyRegisteredAppLogout` | Die URL zum Abmelden von der App. |
| `oauth2AllowImplicitFlow` | boolean | `false` | Dieser Wert gibt an, ob die Web-App implizite OAuth 2.0-Flowtoken anfordern kann. Die Standardeinstellung ist „false“. Dieses Flag wird für browserbasierte Apps wie JavaScript-basierte Single-Page-Webanwendungen verwendet. |
| `oauth2AllowUrlPathMatching` | boolean | `false` | Gibt an, ob Azure AD im Rahmen von OAuth 2.0-Tokenanforderungen den Pfadabgleich des Umleitungs-URI mit den replyUrls der App zulässt. Die Standardeinstellung ist „false“. |
| `oauth2Permissions` | Arraytyp | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"adminConsentDescription":"Allow the app to access resources on behalf of the signed-in user.",<br>&nbsp;&nbsp;&nbsp;"adminConsentDisplayName":"Access resource1",<br>&nbsp;&nbsp;&nbsp;"id":"\<guid>",<br>&nbsp;&nbsp;&nbsp;"isEnabled":true,<br>&nbsp;&nbsp;&nbsp;"type":"User",<br>&nbsp;&nbsp;&nbsp;"userConsentDescription":"Allow the app to access resource1 on your behalf.",<br>&nbsp;&nbsp;&nbsp;"userConsentDisplayName":"Access resources",<br>&nbsp;&nbsp;&nbsp;"value":"user_impersonation"<br>&nbsp;&nbsp;}<br>]</code> | Gibt die Sammlung von OAuth 2.0-Berechtigungsbereichen an, die die Web-API-App (Ressource) für Client-Apps verfügbar macht. Diese Berechtigungsbereiche können Client-Apps im Zuge der Zustimmung gewährt werden. |
| `oauth2RequiredPostResponse` | boolean | `false` | Gibt an, ob Azure AD im Rahmen von OAuth 2.0-Tokenanforderungen POST-Anforderungen zulässt (im Gengensatz zu GET-Anforderungen). Beim Standartwert „false“ sind nur GET-Anforderungen zulässig. |
| `objectId` | Bezeichnerzeichenfolge | `"f7f9acfc-ae0c-4d6c-b489-0a81dc1652dd"` | Ein eindeutiger Bezeichner für die App im Verzeichnis. Diese ID ist nicht der Bezeichner, der verwendet wird, um die App in einer Protokolltransaktion zu bezeichnen. Sie dient dazu, in Verzeichnisabfragen auf das Objekt zu verweisen. |
| `parentalControlSettings` | Zeichenfolge | <code>{<br>&nbsp;&nbsp;&nbsp;"countriesBlockedForMinors":[],<br>&nbsp;&nbsp;&nbsp;"legalAgeGroupRule":"Allow"<br>}</code> | `countriesBlockedForMinors` gibt an, die Länder/Regionen an, in denen die App für Minderjährige blockiert wird.<br>`legalAgeGroupRule` gibt die Regel für die rechtliche Altersgruppe an, die für Benutzer der App gilt. Dieser Wert kann auf `Allow`, `RequireConsentForPrivacyServices`, `RequireConsentForMinors`, `RequireConsentForKids` oder `BlockMinors` festgelegt werden.  |
| `passwordCredentials` | Arraytyp | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"customKeyIdentifier":null,<br>&nbsp;&nbsp;&nbsp;"endDate":"2018-10-19T17:59:59.6521653Z",<br>&nbsp;&nbsp;&nbsp;"keyId":"\<guid>",<br>&nbsp;&nbsp;&nbsp;"startDate":"2016-10-19T17:59:59.6521653Z",<br>&nbsp;&nbsp;&nbsp;"value":null<br>&nbsp;&nbsp;&nbsp;}<br>]</code> | Siehe Beschreibung für die `keyCredentials`-Eigenschaft. |
| `publicClient` | boolean | `false` | Gibt an, ob es sich bei einer App um einen öffentlichen Client handelt (beispielsweise eine installierte App auf einem mobilen Gerät). Der Standardwert ist „false“. |
| `replyUrls` | Zeichenfolgenarray | `"http://localhost"` | Diese Eigenschaft, die mehrere Werte zulässt, enthält die Liste der registrierten „redirect_uri“-Werte, die von Azure AD bei der Rückgabe von Token als Ziele akzeptiert werden. |
| `requiredResourceAccess` | Arraytyp | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"resourceAppId":"00000002-0000-0000-c000-000000000000",<br>&nbsp;&nbsp;&nbsp;"resourceAccess":[<br>&nbsp;&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"id":"311a71cc-e848-46a1-bdf8-97ff7156d8e6",<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"type":"Scope"<br>&nbsp;&nbsp;&nbsp;}<br>&nbsp;&nbsp;]<br>&nbsp;}<br>]</code> | Mit dynamischer Einwilligung steuert `requiredResourceAccess` die Einwilligungsoberfläche für Administratoren und Benutzer, die statische Einwilligung verwenden. Die Oberfläche für die Benutzerzustimmung für den Allgemeinfall wird jedoch nicht gesteuert.<br>`resourceAppId` ist der eindeutige Bezeichner für die Ressource, auf die die App zugreifen muss. Dieser Wert muss mit der „appId“ identisch sein, die für die Zielressourcen-App deklariert wurde.<br>`resourceAccess` ist ein Array, das die OAuth 2.0-Berechtigungsbereiche und App-Rollen auflistet, welche die App für die jeweilige Ressource erfordert. Enthält die `id`- und `type`-Werte der jeweiligen Ressourcen. |
| `samlMetadataUrl` | Zeichenfolge | `http://MyRegisteredAppSAMLMetadata` | Die URL zu den SAML-Metadaten für die App. |

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu den Beziehungen zwischen den App- und Dienstprinzipalobjekten einer App finden Sie unter [Anwendungs- und Dienstprinzipalobjekte in Azure Active Directory (Azure AD)](app-objects-and-service-principals.md).
* Unter [Azure AD-Entwicklerglossar](developer-glossary.md) finden Sie Definitionen für einige der wichtigsten Konzepte für Azure Active Directory-Entwickler (AD).

Bitte geben Sie uns über den folgenden Kommentarabschnitt Ihr Feedback, um uns bei der Verbesserung unserer Inhalte zu unterstützen.

<!--article references -->
[AAD-APP-OBJECTS]:app-objects-and-service-principals.md
[AAD-DEVELOPER-GLOSSARY]:developer-glossary.md
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

