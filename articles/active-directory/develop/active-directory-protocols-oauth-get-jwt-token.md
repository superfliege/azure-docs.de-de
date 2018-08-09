---
title: Authentifizieren mit Azure AD und Abrufen eines JWT-Tokens mithilfe von OAuth 2.0
description: Der hier angegebene Beispielcode zeigt, wie Sie sich mit Azure Active Directory unter Verwendung von OAuth 2.0 authentifizieren, um auf geschützte Webanwendungen und Web-APIs in Ihrer Organisation zuzugreifen.
services: active-directory
author: rloutlaw
manager: angerobe
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2018
ms.author: routlaw
ms.custom: aaddev
ms.openlocfilehash: eb26101229ad60abae7a8a84f8dfa496488e84ba
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2018
ms.locfileid: "39579002"
---
# <a name="request-an-access-token-using-oauth-20-to-access-web-apis-and-applications-secured-by-azure-active-directory"></a>Anfordern eines Zugriffstokens mithilfe von OAuth 2.0 für den Zugriff auf Web-APIs und Webanwendungen, die durch Azure Active Directory geschützt werden

In diesem Artikel wird gezeigt, wie Sie ein JSON Web Token (JWT) abrufen, um auf durch Azure AD geschützte Ressourcen zuzugreifen. Es wird vorausgesetzt, dass Sie über ein [Autorisierungstoken](/azure/active-directory/develop/active-directory-protocols-oauth-code#request-an-authorization-code) verfügen. Mögliche Quellen eines solchen Tokens sind eine vom Benutzer gewährte Berechtigung oder ein [Dienstprinzipal](/azure/active-directory/develop/active-directory-application-objects).

## <a name="build-the-request"></a>Erstellen der Anforderung

Rufen Sie mithilfe der folgenden `POST`-HTTP-Anforderung ein JWT ab, um auf eine bestimmte Anwendung oder API zuzugreifen, die durch Azure AD geschützt wird.

```http
POST https://{tenant}/oauth2/v2.0/token?client_id={client-id}
&scope={scope}
&code=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq3n8b2JRLk4OxVXr...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=authorization_code
&client_secret=JqQX2PNo9bpM0uEihUPzyrh   
```

### <a name="request-headers"></a>Anforderungsheader

Die folgenden Header sind erforderlich:

|Anforderungsheader|BESCHREIBUNG|  
|--------------------|-----------------|  
| *Host:* | https://login.microsoftonline.com |
| *Inhaltstyp*:| application/x-www-form-urlencoded |
 

### <a name="uri-parameters"></a>URI-Parameter

| Parameter     |                       | BESCHREIBUNG                                                                                                                                                                                                                                                                                                                                                                                                                                |
|---------------|-----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Mandant        | required              | Mit dem `{tenant}` -Wert im Pfad der Anforderung kann festgelegt werden, welche Benutzer sich bei der Anwendung anmelden können. Zulässige Werte sind `common`, `organizations`, `consumers` und Mandantenbezeichner. Weitere Informationen finden Sie in den [Grundlagen zu Protokollen](active-directory-v2-protocols.md#endpoints).                                                                                                                                                   |
| client_id     | required              | Die Anwendungs-ID, die das Registrierungsportal ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) Ihrer App zugewiesen hat.                                                                                                                                                                                                                             |
| grant_type    | required              | Muss der `authorization_code` für den Autorisierungscodefluss sein.                                                                                                                                                                                                                                                                                                                                                                            |
| scope         | required              | Eine durch Leerzeichen getrennte Liste von Bereichen. Die in diesem Abschnitt angeforderten Bereiche müssen den Bereichen im Aufruf von `/authorize` entsprechen oder eine Teilmenge dieser Bereiche sein. Wenn die in dieser Anforderung angegebenen Bereiche mehrere Ressourcenserver umfassen, gibt der v2.0-Endpunkt ein Token für die im ersten Bereich angegebene Ressource zurück. Eine ausführlichere Erläuterung von Bereichen finden Sie in [Berechtigungen, Zustimmung und Bereiche](v2-permissions-and-consent.md). |
| code          | required              | Der Autorisierungscode, den Sie im Aufruf von `/authorize` erhalten haben                                                                                                                                                                                                                                                                                                                                                                |
| redirect_uri  | required              | Derselbe Wert für den Umleitungs-URI, der zum Abrufen des Autorisierungscodes verwendet wurde                                                                                                                                                                                                                                                                                                                                                             |
| client_secret | erforderlich für Web-Apps | Der geheime App-Schlüssel, den Sie im App-Registrierungsportal für Ihre App erstellt haben. Verwenden Sie ihn nicht in einer systemeigenen App, da geheime Clientschlüssel nicht zuverlässig auf Geräten gespeichert werden können. Er ist erforderlich für Web-Apps und Web-APIs, die die Möglichkeit haben, den geheimen Client-Schlüssel sicher auf dem Server zu speichern.  Die geheimen Clientschlüssel müssen vor dem Senden URL-codiert werden.                                                                                 |
| code_verifier | optional              | Derselbe code_verifier-Parameter, der auch zum Abrufen von „authorization_code“ verwendet wurde. Erforderlich, wenn PKCE bei der Anforderung für die Gewährung des Autorisierungscodes verwendet wurde. Weitere Informationen finden Sie unter [PKCE RFC](https://tools.ietf.org/html/rfc7636).                                                                                                                                                                                                                                                                                             |
## <a name="handle-the-response"></a>Verarbeiten der Antwort

Eine erfolgreiche Tokenantwort enthält ein JWT-Token und sieht ungefähr wie folgt aus:

```json
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```
| Parameter     | BESCHREIBUNG                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|---------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| access_token  | Das angeforderte Zugriffstoken. Die App kann dieses Token zur Authentifizierung bei geschützten Ressourcen verwenden, wie z.B. eine Web-API.                                                                                                                                                                                                                                                                                                                                    |
| token_type    | Gibt den Wert des Tokentyps an. Der Bearertyp ist der einzige Typ, den Azure AD unterstützt.                                                                                                                                                                                                                                                                                                                                                                           |
| expires_in    | Gibt an, wie lange das Zugriffstoken (in Sekunden) gültig ist.                                                                                                                                                                                                                                                                                                                                                                                                       |
| scope         | Die Bereiche, für die das Zugriffstoken gültig ist.                                                                                                                                                                                                                                                                                                                                                                                                         |
| refresh_token | Ein Aktualisierungstoken von OAuth 2.0. Die App kann dieses Token verwenden, um zusätzliche Zugriffstoken nach Ablauf der aktuellen Zugriffstoken zu erhalten. Aktualisierungstoken sind langlebig und können verwendet werden, um den Zugriff auf Ressourcen für längere Zeit beizubehalten. Weitere Details finden Sie in der [v2.0-Tokenreferenz](v2-id-and-access-tokens.md). <br> **Hinweis:** Wird nur bei Anforderung des `offline_access`-Bereichs bereitgestellt.                                               |
| id_token      | Ein unsigniertes JSON-Webtoken (JWT). Die App kann die Segmente dieses Tokens decodieren, um Informationen zum angemeldeten Benutzer abzurufen. Die App kann die Werte zwischenspeichern und sie anzeigen, sollte sich in Bezug auf Autorisierungs- und Sicherheitsgrenzen aber nicht darauf verlassen. Weitere Informationen zu ID-Token finden Sie in der [Tokenreferenz zum v2.0-Endpunkt](v2-id-and-access-tokens.md). <br> **Hinweis:** Wird nur bei Anforderung des `openid`-Bereichs bereitgestellt. |



