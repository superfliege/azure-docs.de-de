---
title: Benutzerflowtypen in Azure Active Directory B2C | Microsoft-Dokumentation
description: Ein Thema zum erweiterbaren Richtlinienframework von Azure Active Directory B2C und die Erstellung verschiedener Benutzerflowtypen.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: d4a93b04b8ad86a6a6d36a5bdaf3209b49e7a9dc
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52877081"
---
# <a name="azure-active-directory-b2c-user-flows"></a>Azure Active Directory B2C: Benutzerabläufe


Das erweiterbare Richtlinienframework von Azure Active Directory (Azure AD) B2C ist der Hauptvorteil des Diensts. Die Richtlinien beschreiben Benutzeroberflächen im Zusammenhang mit der Kundenidentität, z.B. Registrierung, Anmeldung oder Profilbearbeitung. Um Ihnen bei der Einrichtung der gängigsten Identitätsaufgaben zu helfen, enthält das Azure AD B2C-Portal vordefinierte, konfigurierbare Richtlinien mit dem Namen **Benutzerflows**. Ein Registrierungsbenutzerflow ermöglicht beispielsweise das Steuern des Verhaltens durch Konfigurieren der folgenden Einstellungen:

* Kontotypen (Konten sozialer Netzwerke wie Facebook oder lokale Konten wie die E-Mail-Adressen), die Kunden zum Registrieren für die Anwendung verwenden können
* Attribute (z.B. Vorname, Postleitzahl und Schuhgröße), die der Kunde während des Anmeldeprozesses angeben muss
* Verwendung der Azure Multi-Factor Authentication
* Das Aussehen und Verhalten aller Registrierungsseiten
* Informationen (als Ansprüche in Token ausgedrückt), die die Anwendung nach Abschluss der Benutzerflowausführung erhält

Sie können mehrere Benutzerflows verschiedener Typen in Ihrem Mandanten erstellen und bei Bedarf in Ihren Anwendungen verwenden. Benutzerflows können anwendungsübergreifend wiederverwendet werden. Durch diese Flexibilität können Entwickler Benutzeroberflächen im Zusammenhang mit der Kundenidentität mit minimalen oder ganz ohne Änderungen an ihrem Code definieren und ändern.

Benutzerflows können über eine einfache Entwicklerschnittstelle verwendet werden. Ihre Anwendung löst einen Benutzerflow mithilfe einer standardmäßigen HTTP-Authentifizierungsanforderung aus (übergibt einen Benutzerflowparameter in der Anforderung) und empfängt ein benutzerdefiniertes Token als Antwort. Der einzige Unterschied zwischen Anforderungen, die einen Registrierungsbenutzerflow aufrufen, und Anforderungen, die einen Anmeldebenutzerflow aufrufen, ist beispielsweise der Benutzerflowname, der im Parameter „p“ der Abfragezeichenfolge verwendet wird:

```

https://contosob2c.b2clogin.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siup                                       // Your sign-up user flow

```

```

https://contosob2c.b2clogin.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siin                                       // Your sign-in user flow

```

## <a name="create-a-sign-up-or-sign-in-user-flow"></a>Erstellen eines Registrierungs- oder Anmeldebenutzerflows

Mit diesem Benutzerflow werden die Registrierung und Anmeldung von Kunden in ein und derselben Konfiguration verarbeitet. Die Kunden werden je nach Kontext auf den jeweils entsprechenden Pfad geleitet (Registrierung oder Anmeldung). Außerdem wird mit der Richtlinie der Inhalt von Token beschrieben, die die Anwendung nach erfolgreichen Registrierungen oder Abmeldungen erhält.  Ein Codebeispiel für den Benutzerflow zur **Registrierung oder Anmeldung** ist [hier](active-directory-b2c-devquickstarts-web-dotnet-susi.md) verfügbar.  Sie sollten diesen Benutzerflow anstelle eines **Registrierungs-** oder **Anmeldebenutzerflows** verwenden.  

[!INCLUDE [active-directory-b2c-create-sign-in-sign-up-policy](../../includes/active-directory-b2c-create-sign-in-sign-up-policy.md)]

## <a name="create-a-sign-up-user-flow"></a>Erstellen eines Benutzerflows für die Registrierung

[!INCLUDE [active-directory-b2c-create-sign-up-policy](../../includes/active-directory-b2c-create-sign-up-policy.md)]

## <a name="create-a-sign-in-user-flow"></a>Erstellen eines Benutzerflows für die Anmeldung

[!INCLUDE [active-directory-b2c-create-sign-in-policy](../../includes/active-directory-b2c-create-sign-in-policy.md)]

## <a name="create-a-profile-editing-user-flow"></a>Erstellen eines Benutzerflows für die Profilbearbeitung

[!INCLUDE [active-directory-b2c-create-profile-editing-policy](../../includes/active-directory-b2c-create-profile-editing-policy.md)]

## <a name="create-a-password-reset-user-flow"></a>Erstellen eines Benutzerflows zur Kennwortrücksetzung

[!INCLUDE [active-directory-b2c-create-password-reset-policy](../../includes/active-directory-b2c-create-password-reset-policy.md)]

## <a name="preview-user-flows"></a>Vorschau für Benutzerflows

Wenn wir neue Features herausgeben, sind einige davon vielleicht nicht mit vorhandenen Richtlinien oder Benutzerflows verfügbar.  Wir planen, ältere Versionen mit den neuesten desselben Typs zu ersetzen, sobald diese Richtlinien oder Benutzerflows allgemein verfügbar sind.  Die vorhandenen Richtlinien oder Benutzerflows werden nicht geändert, und um diese neuen Funktionen nutzen zu können, müssen Sie neue Benutzerflows erstellen.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="how-do-i-link-a-sign-up-or-sign-in-user-flow-with-a-password-reset-user-flow"></a>Wie wird ein Registrierungs- oder Anmeldebenutzerflow mit einem Benutzerflow zum Zurücksetzen von Kennwörtern verknüpft?
Beim Erstellen einen **Registrierungs-** oder Anmeldebenutzerflow (für lokale Konten) wird auf der ersten Seite der Oberfläche der Link **Kennwort vergessen?** angezeigt. Durch Klicken auf diesen Link wird nicht automatisch ein Benutzerflow zum Zurücksetzen des Kennworts ausgelöst. 

Stattdessen wird der Fehlercode **`AADB2C90118`** an die App zurückgegeben. Ihre App muss mit diesen Fehlercode verarbeiten, indem sie einen bestimmten Benutzerflow zur Kennwortzurücksetzung aufruft. Weitere Informationen finden Sie in dem [Beispiel, das das Verknüpfen von Benutzerflows veranschaulicht](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-DotNet-SUSI).

### <a name="should-i-use-a-sign-up-or-sign-in-user-flow-or-a-sign-up-user-flow-and-a-sign-in-user-flow"></a>Sollte ich einen Registrierungs- oder einen Anmeldebenutzerflow oder einen Benutzerflow für die Registrierung und einen Benutzerflow für die Anmeldung verwenden?
Wir empfehlen, einen **Registrierungs- oder einen Anmeldebenutzerflow** einem Benutzerflow für die **Registrierung** und einem Benutzerflow für die **Anmeldung** vorzuziehen.  

Der **Registrierungs- oder Anmeldebenutzerflow** weist mehr Funktionen als der **Anmeldebenutzerflow** auf. Außerdem ermöglicht sie Ihnen, die Seite für die Benutzeroberflächenanpassung zu nutzen, und sie hat eine bessere Unterstützung für die Lokalisierung. 

Der **Anmeldebenutzerflow** wird empfohlen, wenn Sie Ihren Benutzerflow nicht lokalisieren müssen, nur geringfügige Anpassungsfunktionen für das Branding benötigen und eine integrierte Kennwortzurücksetzung möchten.

## <a name="next-steps"></a>Nächste Schritte
* [Token, Sitzung und einmaliges Anmelden – Konfiguration](active-directory-b2c-token-session-sso.md)
* [Deaktivieren der E-Mail-Überprüfung während der Registrierung von Endbenutzern](active-directory-b2c-reference-disable-ev.md)

