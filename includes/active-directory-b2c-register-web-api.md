---
author: PatAltimore
ms.service: active-directory-b2c
ms.topic: include
ms.date: 11/03/2016
ms.author: patricka
ms.openlocfilehash: 9d5af21fb3b329623b14cb8742d9ec9c5d1bad46
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50133543"
---
[!INCLUDE [active-directory-b2c-portal-add-application](active-directory-b2c-portal-add-application.md)]

Verwenden Sie zum Registrieren Ihrer Web-API die in der Tabelle angegebenen Einstellungen.

![Beispielregistrierungseinstellungen für die neue Web-API](./media/active-directory-b2c-register-web-api/b2c-new-web-api-settings.png)

| Einstellung      | Beispielwert  | BESCHREIBUNG                                        |
| ------------ | ------- | -------------------------------------------------- |
| **Name** | Contoso B2C-API | Geben Sie für die Anwendung einen **Namen** ein, der Ihre API für Kunden beschreibt. | 
| **Web-App/Web-API einschließen** | JA | Wählen Sie für eine Web-API die Option **Ja** aus. |
| **Impliziten Fluss zulassen** | JA | Wählen Sie **Ja** aus, wenn Ihre Anwendung [OpenID Connect-Anmeldung](../articles/active-directory-b2c/active-directory-b2c-reference-oidc.md) verwendet. |
| **Antwort-URL** | `https://localhost:44316/` | Antwort-URLs sind Endpunkte, an denen Azure AD B2C von Ihrer Anwendung angeforderte Token zurückgibt. Geben Sie [eine passende](../articles/active-directory-b2c/active-directory-b2c-app-registration.md#choosing-a-web-app-or-api-reply-url) **Antwort-URL** ein. In diesem Beispiel ist Ihre Web-API lokal und lauscht an Port 44316. |
| **App-ID-URI** | api | Der App-ID-URI ist der Bezeichner für Ihre Web-API. Der vollständige Bezeichner-URI (einschließlich der Domäne) wird für Sie generiert. |

Klicken Sie auf **Erstellen** , um Ihre Anwendung zu registrieren.

Die neu registrierte Anwendung wird in der Anwendungsliste für den B2C-Mandanten angezeigt. Wählen Sie Ihre Web-API aus der Liste aus. Der Eigenschaftenbereich der API wird angezeigt.

![Web-API-Eigenschaften](./media/active-directory-b2c-register-web-api/b2c-web-api-properties.png)

Notieren Sie sich die global eindeutige **Anwendungsclient-ID**. Die ID wird im Code Ihrer Anwendung verwendet.