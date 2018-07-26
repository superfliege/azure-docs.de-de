---
title: In Azure Active Directory B2C verwendbare Anwendungstypen | Microsoft-Dokumentation
description: Erfahren Sie, welche Anwendungstypen in Azure Active Directory B2C verwendet werden können.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/13/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: f4b45c743c0efa1c9df665018b28a8b4ffb76f73
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/25/2018
ms.locfileid: "39238402"
---
# <a name="applications-types-that-can-be-used-in-active-directory-b2c"></a>In Azure Active Directory B2C verwendbare Anwendungstypen

Azure Active Directory (Azure AD) B2C unterstützt die Authentifizierung für eine Vielzahl von modernen Anwendungsarchitekturen. Diese basieren alle auf den branchenüblichen Protokollen [OAuth 2.0](active-directory-b2c-reference-protocols.md) oder [OpenID Connect](active-directory-b2c-reference-protocols.md). Dieses Dokument beschreibt die Anwendungstypen, die Sie unabhängig von der bevorzugten Sprache oder Plattform erstellen können. Außerdem verdeutlicht es die allgemeinen Szenarios, bevor Sie mit dem Erstellen von Anwendungen beginnen.

Jede Anwendung, die Azure AD B2C verwendet, muss über das [Azure-Portal](https://portal.azure.com/) in Ihrem [Azure AD B2C-Mandanten](active-directory-b2c-get-started.md) registriert werden. Während des Anwendungsregistrierungsprozesses werden Werte erfasst und zugewiesen, wie z.B.:

* Eine **Anwendungs-ID**, die Ihre Anwendung eindeutig ausweist.
* Ein **Umleitungs-URI**, der zum Umleiten von Antworten zurück an die Anwendung verwendet werden kann.

Für jede Anforderung, die an Azure AD B2C gesendet wird, wird eine **Richtlinie**angegeben. Eine Richtlinie steuert das Verhalten von Azure AD. Mit diesen Endpunkten können Sie auch einen extrem anpassbaren Satz von Benutzeroberflächen erstellen. Beispiele für allgemeine Richtlinien sind Anmelderichtlinien, Registrierungsrichtlinien und Profilbearbeitungsrichtlinien. Wenn Sie mit Richtlinien nicht vertraut sind, sollten Sie sich zuerst die Informationen zum [erweiterbaren Richtlinienframework](active-directory-b2c-reference-policies.md) von Azure AD B2C ansehen, bevor Sie weiterlesen.

Die Interaktion der einzelnen Anwendungen folgt einem ähnlichen allgemeinen Muster:

1. Die Anwendung leitet den Endbenutzer an den v2.0-Endpunkt weiter, um eine [Richtlinie](active-directory-b2c-reference-policies.md) auszuführen.
2. Der Benutzer schließt die Richtlinie gemäß Richtliniendefinition ab.
3. Die Anwendung empfängt ein Sicherheitstoken vom v2.0-Endpunkt.
4. Die Anwendung verwendet das Sicherheitstoken für den Zugriff auf geschützte Informationen oder eine geschützte Ressource.
5. Der Ressourcenserver überprüft das Sicherheitstoken, um sicherzustellen, dass der Zugriff gewährt werden kann.
6. Die Anwendung aktualisiert das Sicherheitstoken in regelmäßigen Abständen.

Diese Schritte können sich je nach erstelltem Anwendungstyp geringfügig unterscheiden.

## <a name="web-applications"></a>Webanwendungen

Für Webanwendungen (einschließlich .NET, PHP, Java, Ruby, Python und Node.js), die auf einem Server gehostet werden und über einen Browser zugänglich sind, unterstützt Azure AD B2C [OpenID Connect](active-directory-b2c-reference-protocols.md) für alle Benutzeroberflächen. Hierzu gehören die Anmeldung, Registrierung und Profilverwaltung. In der Azure AD B2C-Implementierung von OpenID Connect initiiert Ihre Webanwendung diese Benutzeroberflächen, indem Authentifizierungsanforderungen für Azure AD ausgegeben werden. Das Ergebnis der Anforderung ist ein `id_token`. Dieses Sicherheitstoken stellt die Identität des Benutzers dar. Darüber hinaus werden Informationen über den Benutzer in Form von Ansprüchen bereitgestellt:

```
// Partial raw id_token
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImtyaU1QZG1Cd...

// Partial content of a decoded id_token
{
    "name": "John Smith",
    "email": "john.smith@gmail.com",
    "oid": "d9674823-dffc-4e3f-a6eb-62fe4bd48a58"
    ...
}
```

Informationen zu allen Arten von Token und zu den für eine Anwendung verfügbaren Ansprüchen finden Sie in der [Azure AD B2C-Tokenreferenz](active-directory-b2c-reference-tokens.md).

In einer Webanwendung sind für jede Ausführung einer [Richtlinie](active-directory-b2c-reference-policies.md) im Allgemeinen folgende Schritte erforderlich:

![Abbildung der Web-App-Verantwortlichkeitsbereiche](./media/active-directory-b2c-apps/webapp.png)

Die Überprüfung von `id_token` mithilfe eines von Azure AD erhaltenen öffentlichen Signaturschlüssels ist ausreichend, um die Identität des Benutzers zu bestätigen. Dadurch wird außerdem ein Sitzungscookie festgelegt, das zur Identifizierung des Benutzers in nachfolgenden Seitenanforderungen verwendet werden kann.

Um dieses Szenario in Aktion zu sehen, können Sie eines der Webanwendungs-Codebeispiele für die Anmeldung im Abschnitt [Erste Schritte](active-directory-b2c-overview.md) testen.

Neben der Bereitstellung der einfachen Anmeldung benötigt eine Webserveranwendung unter Umständen auch Zugriff auf einen Back-End-Webdienst. In diesem Fall kann die Webanwendung einen etwas anderen [OpenID Connect-Ablauf](active-directory-b2c-reference-oidc.md) durchführen und Token anhand von Autorisierungscodes und Aktualisierungstoken beschaffen. Dieses Szenario ist im folgenden [Abschnitt zu Web-APIs](#web-apis)dargestellt.

## <a name="web-apis"></a>Web-APIs

Sie können Azure AD B2C zum Schützen von Webdiensten, wie z.B. der RESTful-Web-API Ihrer Anwendung, verwenden. Web-APIs können OAuth 2.0 verwenden, um ihre Daten zu schützen, indem eingehende HTTP-Anforderungen per Token authentifiziert werden. Der Aufrufer einer Web-API fügt ein Token im Autorisierungsheader einer HTTP-Anforderung an:

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

Die Web-API kann dann mit dem Token die Identität des API-Aufrufers überprüfen und Informationen über den Aufrufer aus Ansprüchen extrahieren, die im Token codiert sind. Informationen zu allen Arten von Token und zu den für eine App verfügbaren Ansprüchen finden Sie in der [Azure AD B2C-Tokenreferenz](active-directory-b2c-reference-tokens.md).

> [!NOTE]
> Azure AD B2C unterstützt derzeit nur Web-APIs, auf die von ihren eigenen bekannten Clients zugegriffen wird. Ihre vollständige Anwendung kann beispielsweise eine iOS-App, eine Android-Anwendung und eine Back-End-Web-API umfassen. Diese Architektur wird vollständig unterstützt. Das Erteilen des Zugriffs auf dieselbe Web-API an einen Partnerclient, beispielsweise eine andere iOS-Anwendung, wird derzeit nicht unterstützt. Alle Komponenten Ihrer vollständigen Anwendung müssen eine Anwendungs-ID gemeinsam verwenden.
>
>

Eine Web-API kann Token von vielen Clienttypen empfangen, einschließlich Webanwendungen, Desktop- und mobilen Anwendungen, Single-Page-Anwendungen, serverseitigen Daemons und anderen Web-APIs. Hier sehen Sie ein Beispiel für den vollständigen Ablauf für eine Webanwendung, die eine Web-API aufruft:

![Abbildung der Web-App-Web-API-Verantwortlichkeitsbereiche](./media/active-directory-b2c-apps/webapi.png)

Weitere Informationen zu Autorisierungscodes, Aktualisierungstoken und den Schritten zum Abrufen von Token finden Sie im [OAuth 2.0-Protokoll](active-directory-b2c-reference-oauth-code.md).

Weitere Informationen zum Schützen einer Web-API mit Azure AD B2C finden Sie in den Tutorials zur Web-API im [Abschnitt mit den ersten Schritten](active-directory-b2c-overview.md).

## <a name="mobile-and-native-applications"></a>Mobile und native Anwendungen

Auf Geräten installierte Anwendungen, z.B. mobile Anwendungen und Desktopanwendungen, benötigen häufig Zugriff auf Back-End-Dienste oder Web-APIs im Auftrag von Benutzern. Sie können Ihren nativen Anwendungen angepasste Oberflächen für die Identitätsverwaltung hinzufügen und Back-End-Dienste sicher aufrufen, indem Sie Azure AD B2C und den [Autorisierungscodeablauf von OAuth 2.0](active-directory-b2c-reference-oauth-code.md) verwenden.  

Bei diesem Ablauf führt die Anwendung [Richtlinien](active-directory-b2c-reference-policies.md) aus und empfängt einen `authorization_code` von Azure AD, nachdem der Benutzer die Richtlinie abgeschlossen hat. Der `authorization_code` stellt die Berechtigung der Anwendung zum Aufrufen von Back-End-Diensten im Namen des derzeit angemeldeten Benutzers dar. Die Anwendung kann dann den `authorization_code` im Hintergrund gegen ein `id_token` und ein `refresh_token` austauschen.  Mit dem `id_token` kann die Anwendung sich in HTTP-Anforderungen bei einer Back-End-Web-API authentifizieren. Sie kann auch das `refresh_token` zum Abrufen eines neuen `id_token` verwenden, wenn ein älteres abläuft.

> [!NOTE]
> Azure AD B2C unterstützt derzeit nur Token, die für den Zugriff auf den eigenen Back-End-Webdienst einer Anwendung verwendet werden. Ihre vollständige Anwendung kann beispielsweise eine iOS-Anwendung, eine Android-Anwendung und eine Back-End-Web-API umfassen. Diese Architektur wird vollständig unterstützt. Das Gewähren des Zugriffs auf eine Partner-Web-API über OAuth 2.0-Zugriffstoken wird für Ihre iOS-Anwendung derzeit nicht unterstützt. Alle Komponenten Ihrer vollständigen Anwendung müssen eine Anwendungs-ID gemeinsam verwenden.
>
>

![Abbildung der Verantwortlichkeitsbereiche systemeigener Apps](./media/active-directory-b2c-apps/native.png)

## <a name="current-limitations"></a>Aktuelle Einschränkungen

Azure AD B2C unterstützt derzeit nicht die folgenden Arten von Apps, die aber trotzdem Teil der Roadmap sind. 

### <a name="daemonsserver-side-applications"></a>Daemons/serverseitige Anwendungen

Anwendungen, die lang andauernde Prozesse enthalten oder ohne Benutzereingriff arbeiten, benötigen auch die Möglichkeit, auf sichere Ressourcen wie Web-APIs zuzugreifen. Diese Anwendungen können mithilfe der Identität der Anwendung (anstelle der delegierten Benutzeridentität) und mithilfe des Clientanmeldeinformations-Flows von OAuth 2.0 die Authentifizierung durchführen und Token abrufen. Ein Clientanmeldeinformations-Flow ist nicht gleich einem „Im Auftrag von“-Ablauf, und in der „Im Auftrag von“-Ablauf sollte nicht für die Server-zu-Server-Authentifizierung verwendet werden.

Obwohl der Clientanmeldeinformations-Flow zurzeit nicht von Azure AD B2C unterstützt wird, können Sie den Clientanmeldeinformations-Flow mithilfe von Azure AD einrichten. Ein Azure AD B2C-Mandant teilt sich einige Funktionen mit Azure AD-Unternehmensmandanten.  Der Clientanmeldeinformations-Flow wird mithilfe der Azure AD-Funktionen des Azure AD B2C-Mandanten unterstützt. 

Informationen über das Einrichten des Clientanmeldeinformations-Flows finden Sie unter [Azure Active Directory v2.0 und der OAuth 2.0-Clientanmeldeinformations-Flow](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-protocols-oauth-client-creds). Bei einer erfolgreichen Authentifizierung wird ein formatiertes Token empfangen, sodass es von Azure AD wie in [Azure AD-Tokenreferenz](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims) beschrieben verwendet werden kann.


### <a name="web-api-chains-on-behalf-of-flow"></a>Web-API-Ketten („Im Auftrag von“-Ablauf)

Viele Architekturen umfassen eine Web-API, von der eine andere Downstream-Web-API aufgerufen werden muss, wobei beide durch Azure AD B2C gesichert sind. Dieses Szenario wird häufig bei nativen Clients mit einem Web-API-Back-End eingesetzt. Dieses ruft dann einen Microsoft-Onlinedienst wie z. B. die Azure AD Graph-API auf.

Dieses Szenario der verketteten Web-API kann mithilfe der Berechtigung für Anmeldeinformationen über den OAuth 2.0-JWT-Bearer unterstützt werden, auch bekannt als „Im Auftrag von“-Ablauf.  Der „Im Auftrag von“-Ablauf ist in Azure AD B2C derzeit noch nicht implementiert.
