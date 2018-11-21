---
title: Token-, Sitzungs- und SSO-Konfiguration in Azure Active Directory B2C | Microsoft-Dokumentation
description: Token, Sitzung und einmaliges Anmelden – Konfiguration in Azure Active Directory B2C
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/16/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 15064e90690064e67b296e7a46749f27773c0814
ms.sourcegitcommit: 542964c196a08b83dd18efe2e0cbfb21a34558aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2018
ms.locfileid: "51636899"
---
# <a name="token-session-and-single-sign-on-configuration-in-azure-active-directory-b2c"></a>Azure Active Directory B2C: Token, Sitzung und einmaliges Anmelden – Konfiguration

Diese Funktion ermöglicht Ihnen eine präzisere Steuerung der folgenden Einstellungen auf [Basis einer Richtlinie](active-directory-b2c-reference-policies.md):

- Lebensdauer von Sicherheitstoken, die von Azure Active Directory (Azure AD) B2C ausgegeben werden.
- Lebensdauer von Webanwendungssitzungen, die von Azure AD B2C verwaltet werden.
- Formate wichtiger Ansprüche in den von Azure AD B2C ausgegebenen Sicherheitstoken
- App- und richtlinienübergreifendes SSO-Verhalten in Ihrem Azure AD B2C-Mandanten

Sie können diese Funktion für jeden Richtlinientyp verwenden. In diesem Beispiel wird veranschaulicht, wie Sie sie mit einer Registrierungs- oder Anmelderichtlinie nutzen. Für integrierte Richtlinien können Sie dieses Feature in Ihrem Azure AD B2C-Verzeichnis wie folgt verwenden:

1. Klicken Sie auf **Registrierungs- oder Anmelderichtlinien**.
2. Öffnen Sie eine Richtlinie, indem Sie darauf klicken. Klicken Sie z.B. auf **B2C_1_SiUpIn**.
3. Klicken Sie oben im Menü auf **Bearbeiten**.
4. Klicken Sie auf **Token, Sitzung und einmaliges Anmelden**.
5. Nehmen Sie die gewünschten Änderungen vor. Informieren Sie sich in den folgenden Abschnitten zu verfügbaren Eigenschaften.
6. Klicken Sie auf **OK**.
7. Klicken Sie oben im Menü auf **Speichern**.

## <a name="token-lifetimes-configuration"></a>Konfiguration der Tokengültigkeitsdauer

Azure AD B2C unterstützt das [OAuth 2.0-Autorisierungsprotokoll](active-directory-b2c-reference-protocols.md) zum Aktivieren des sicheren Zugriffs auf geschützte Ressourcen. Um diese Unterstützung zu implementieren, gibt Azure AD B2C verschiedene [Sicherheitstoken](active-directory-b2c-reference-tokens.md) aus. 

Die folgenden Eigenschaften werden zum Verwalten der Gültigkeitsdauer von Sicherheitstoken verwendet, die von Azure AD B2C ausgegeben werden:

- **Lebensdauer von Zugriffs- und ID-Token (Minuten)**: Die Gültigkeitsdauer des OAuth 2.0-Bearertokens, das für den Zugriff auf eine geschützte Ressource verwendet wird
    - Standardwert: 60 Minuten.
    - Minimum (inklusive): 5 Minuten.
    - Maximum (inklusive): 1.440 Minuten.
- **Lebensdauer des Aktualisierungstokens (Tage)**: Der maximale Zeitraum, vor dessen Verstreichen ein Aktualisierungstoken verwendet werden kann, um ein neues Zugriffs- oder ID-Token zu erwerben (und optional ein neues Aktualisierungstoken, wenn Ihrer Anwendung der `offline_access`-Bereich gewährt wurde)
    - Standardwert: 14 Tage.
    - Minimum (inklusive): 1 Tag.
    - Minimum (inklusive): 90 Tage.
- **Lebensdauer für gleitendes Fenster des Aktualisierungstokens (Tage)**: Nach Ablauf dieses Zeitraums muss der Benutzer sich erneut authentifizieren, unabhängig von der Gültigkeitsdauer des zuletzt von der Anwendung abgerufenen Aktualisierungstokens. Es kann nur bereitgestellt werden, wenn die Option auf **Begrenzt** festgelegt ist. Sein Wert muss mindestens dem Wert **Lebensdauer des Aktualisierungstokens (Tage)** entsprechen. Wenn die Option auf **Unbegrenzt** festgelegt ist, können Sie keinen bestimmten Wert angeben.
    - Standardwert: 90 Tage.
    - Minimum (inklusive): 1 Tag.
    - Maximum (inklusive): 365 Tage.

Mit diesen Eigenschaften werden die folgenden Anwendungsfälle ermöglicht:

- Ermöglichen Sie einem Benutzer, unbegrenzt bei einer mobilen Anwendung angemeldet zu bleiben, solange er ständig in der Anwendung aktiv ist. Sie können die Option **Lebensdauer für gleitendes Fenster des Aktualisierungstokens (Tage)** in Ihrer Anmelderichtlinie auf **Unbegrenzt** festlegen.
- Erfüllen Sie die Sicherheits- und Complianceanforderungen Ihrer Branche durch Festlegung der entsprechenden Zugriffstoken-Gültigkeitsdauer.

Diese Einstellungen sind für Richtlinien zur Kennwortzurücksetzung nicht verfügbar. 

## <a name="token-compatibility-settings"></a>Tokenkompatibilitätseinstellungen

Die folgenden Eigenschaften ermöglichen Kunden die Aktivierung nach Bedarf:

- **Ausstelleranspruch (iss)**: Diese Eigenschaft gibt den Azure AD B2C-Mandanten an, der das Token ausgestellt hat.
    - `https://<domain>/{B2C tenant GUID}/v2.0/`: Dies ist der Standardwert.
    - `https://<domain>/tfp/{B2C tenant GUID}/{Policy ID}/v2.0/`: Dieser Wert enthält IDs für den B2C-Mandanten und die in der Tokenanforderung verwendete Richtlinie. Verwenden Sie diesen Wert, wenn für Ihre App oder Bibliothek Azure AD B2C die [OpenID Connect Discovery 1.0-Spezifikationen](http://openid.net/specs/openid-connect-discovery-1_0.html) erfüllen muss.
- **Antragstelleranspruch (sub)**: Diese Eigenschaft gibt die Entität an, für die das Token Informationen bestätigt.
    - **ObjectID**: Diese Eigenschaft ist der Standardwert. Er wird als Objekt-ID des Benutzers im Verzeichnis in den `sub`-Anspruch des Tokens eingefügt.
    - **Not supported** (Nicht unterstützt): Diese Eigenschaft wird nur zur Abwärtskompatibilität angegeben. Es wird empfohlen, baldmöglichst auf **ObjectID** umzustellen.
- **Anspruch zur Darstellung der Richtlinien-ID**: Mit dieser Eigenschaft wird der Anspruchstyp angegeben, in den die in der Tokenanforderung verwendete Richtlinien-ID eingefügt wird.
    - **tfp**: Diese Eigenschaft ist der Standardwert.
    - **acr**: Diese Eigenschaft wird nur zur Abwärtskompatibilität angegeben.

## <a name="session-behavior"></a>Sitzungsverhalten

Azure AD B2C unterstützt das [OpenID Connect-Authentifizierungsprotokoll](active-directory-b2c-reference-oidc.md) zum Aktivieren der sicheren Anmeldung bei Webanwendungen. Sie können die folgenden Eigenschaften zum Verwalten von Webanwendungssitzungen verwenden:

- **Lebensdauer der Web-App-Sitzung (Minuten)**: Die Gültigkeitsdauer von Azure AD B2C-Sitzungscookies, die nach erfolgreicher Authentifizierung des Benutzers im Browser gespeichert werden
    - Standardwert: 1.440 Minuten.
    - Minimum (inklusive): 15 Minuten.
    - Maximum (inklusive): 1.440 Minuten.
- **Timeout für Web-App-Sitzung**: Wenn diese Option auf **Absolut** festgelegt ist, muss der Benutzer sich nach dem in **Lebensdauer der Web-App-Sitzung (Minuten)** angegebenen Zeitraum erneut authentifizieren. Wenn diese Option auf **Rollen** festgelegt ist (Standardeinstellung), bleibt der Benutzer angemeldet, solange er ständig in Ihrer Webanwendung aktiv ist.

Mit diesen Eigenschaften werden die folgenden Anwendungsfälle ermöglicht:

- Erfüllen Sie die Sicherheits- und Complianceanforderungen Ihrer Branche durch Festlegung der entsprechenden Lebensdauer für Webanwendungssitzungen.
- Erzwingen Sie die Authentifizierung nach einem festgelegten Zeitraum während der Interaktion eines Benutzers mit einer Komponente Ihrer Webanwendung, die eine hohe Sicherheit erfordert. 

Diese Einstellungen sind für Richtlinien zur Kennwortzurücksetzung nicht verfügbar.

## <a name="single-sign-on-sso-configuration"></a>Einmaliges Anmelden (Single Sign-on, SSO) – Konfiguration

Wenn Sie in Ihrem B2C-Mandanten über mehrere Anwendungen und Richtlinien verfügen, können Sie die Interaktionen der Benutzer mithilfe der Eigenschaft **Konfiguration des einmaligen Anmeldens** übergreifend verwalten. Sie können die Eigenschaft auf eine der folgenden Optionen festlegen:

- **Mandant**: Dies ist die Standardeinstellung. Mit dieser Einstellung können mehrere Anwendungen und Richtlinien in Ihrem B2C-Mandanten die gleiche Benutzersitzung gemeinsam nutzen. Beispiel: Sobald sich ein Benutzer bei einer Anwendung angemeldet hat, erfolgt die Anmeldung bei einer anderen Anwendung (etwa Contoso Pharmacy) nahtlos beim Zugriff auf die Anwendung.
- **Anwendung**: Mit dieser Einstellung können Sie eine Benutzersitzung ausschließlich für eine Anwendung beibehalten, unabhängig von anderen Anwendungen. Verwenden Sie diese Einstellung beispielsweise, wenn Sie möchten, dass sich der Benutzer (mit denselben Anmeldeinformationen) bei Contoso Pharmacy anmeldet – auch wenn der Benutzer bereits bei Contoso Shopping angemeldet ist, einer anderen Anwendung im selben B2C-Mandanten. 
- **Richtlinie**: Mit dieser Einstellung können Sie eine Benutzersitzung ausschließlich für eine Richtlinie beibehalten, unabhängig von den Anwendungen, die sie verwenden. Wenn der Benutzer sich beispielsweise bereits angemeldet und einen Multi-Factor Authentication-Schritt (MFA) abgeschlossen hat, kann er in mehreren Anwendungen Zugriff auf Komponenten mit höherer Sicherheit erhalten, solange die an die Richtlinie gebundene Sitzung nicht abläuft.
- **Deaktiviert**: Diese Einstellung erzwingt, dass der Benutzer bei jeder Ausführung der Richtlinie die gesamte User Journey ausführen muss. Dies erlaubt beispielsweise mehreren Benutzern, sich bei der Anwendung anzumelden (in einem Szenario mit freigegebenem Desktop), auch während ein einzelner Benutzer während der gesamten Zeit angemeldet bleibt.

Diese Einstellungen sind für Richtlinien zur Kennwortzurücksetzung nicht verfügbar. 

