---
title: Weitere Informationen zu den von Azure AD v2.0 unterstützten Autorisierungsprotokollen | Microsoft-Dokumentation
description: Anleitung für Protokolle, die vom Azure AD v2.0-Endpunkt unterstützt werden.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 5fb4fa1b-8fc4-438e-b3b0-258d8c145f22
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: 4c9e3c689a7b61e10d63272dd80815b11cacc71e
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2019
ms.locfileid: "55075418"
---
# <a name="v20-protocols---oauth-20-and-openid-connect"></a>v2.0-Protokolle – OAuth 2.0- und OpenID Connect

Der v2.0-Endpunkt kann Azure Active Directory (Azure AD) als Identity-as-a-Service-Lösung mit den Industriestandardprotokollen OpenID Connect und OAuth 2.0 verwenden. Auch wenn der Dienst den Standard entspricht, kann es feine Unterschiede zwischen zwei Implementierungen dieser Protokolle geben. Die hier bereitgestellten Informationen sind hilfreich, wenn Sie Code direkt durch Senden und Verarbeiten von HTTP-Anforderungen schreiben oder eine Open Source-Bibliothek eines Drittanbieters verwenden, anstatt eine unserer [Open-Source-Bibliotheken](reference-v2-libraries.md) zu nutzen.

> [!NOTE]
> Nicht alle Szenarien und Features von Azure AD werden vom v2.0-Endpunkt unterstützt. Lesen Sie die Informationen zu den [Einschränkungen des v2.0-Endpunkts](active-directory-v2-limitations.md), um zu bestimmen, ob Sie den v2.0-Endpunkt verwenden sollten.

## <a name="the-basics"></a>Grundlagen

In fast allen OAuth 2.0- und OpenID Connect-Vorgängen sind vier Beteiligte am Austausch involviert:

![OAuth 2.0-Rollen](../../media/active-directory-v2-flows/protocols_roles.png)

* Der **Autorisierungsserver** ist der v2.0-Endpunkt und verantwortlich für das Sicherstellen der Identität des Benutzers, das Erteilen und Widerrufen des Zugriffs auf Ressourcen und das Ausstellen von Token. Der Autorisierungsserver ist auch als Identitätsanbieter bekannt und verarbeitet auf sichere Weise alles im Zusammenhang mit den Informationen des Benutzers, dessen Zugriff und den Vertrauensstellungen zwischen den Beteiligten in einem Vorgang.
* Beim **Ressourcenbesitzer** handelt es sich normalerweise um den Endbenutzer. Diese Person besitzt die Daten und hat die Möglichkeit, Dritten den Zugriff auf die Daten oder die Ressource zu gewähren.
* Der **OAuth-Client** ist Ihre App, die durch ihre Anwendungs-ID identifiziert wird. Der OAuth-Client ist normalerweise der Beteiligte, mit dem der Endbenutzer interagiert, und der Token vom Autorisierungsserver anfordert. Der Client muss vom Besitzer der Ressource die Berechtigung zum Zugriff darauf erhalten.
* Der **Ressourcenserver** ist der Ort, an dem die Ressource oder die Daten abgelegt sind. Er vertraut dem Autorisierungsserver, dass der OAuth-Client sicher authentifiziert und autorisiert wird, und verwendet Bearerzugriffstoken, um sicherzustellen, dass der Zugriff auf eine Ressource gewährt werden kann.

## <a name="app-registration"></a>App-Registrierung
Jede App, die den v2.0-Endpunkt verwendet, muss in [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) oder durch die neue **App-Registrierungen (Vorschauversion)** im [Azure-Portal](https://portal.azure.com/?Microsoft_AAD_RegisteredApps=true#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade) registriert sein, bevor sie über OAuth oder OpenID Connect interagieren kann. Der Registrierungsprozess für die App sammelt einige Werte und weist ihr einige Werte zu:

* Eine **Anwendungs-ID**, die Ihre App eindeutig identifiziert
* Einen **Umleitungs-URI** oder **Paketbezeichner**, der zum Umleiten von Antworten zurück an die App verwendet werden kann
* Einige andere szenariospezifische Werte.

Weitere Informationen finden Sie im Artikel zum [Registrieren von Apps](quickstart-v2-register-an-app.md).

## <a name="endpoints"></a>Endpunkte

Nach dem Registrieren kommuniziert eine App mit Azure AD, indem Anforderungen an den v2.0-Endpunkt gesendet werden:

```
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/token
```

Dabei ist für `{tenant}` einer von vier verschiedenen Werten möglich:

| Wert | BESCHREIBUNG |
| --- | --- |
| `common` | Ermöglicht Benutzern mit persönlichen Microsoft-Konten und Geschäfts-, Schul- oder Unikonten aus Azure AD die Anmeldung bei der Anwendung. |
| `organizations` | Ermöglicht nur Benutzern mit Geschäfts-, Schul- oder Unikonten aus Azure AD die Anmeldung bei der Anwendung. |
| `consumers` | Ermöglicht nur Benutzern mit persönlichen Microsoft-Konten (MSA) die Anmeldung bei der Anwendung. |
| `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` oder `contoso.onmicrosoft.com` | Ermöglicht nur Benutzern mit Geschäfts-, Schul- oder Unikonten eines bestimmten Azure AD-Mandanten die Anmeldung bei der Anwendung. Dabei kann entweder der Anzeigename der Domäne des Azure AD-Mandanten oder der GUID-Bezeichner des Mandanten verwendet werden. |

Um zu erfahren, wie Sie mit diesen Endpunkten interagieren, wählen Sie im Abschnitt [Protokolle](#protocols) einen bestimmten App-Typ aus, und folgen Sie den Links für weitere Informationen.

## <a name="tokens"></a>Token

Die v2.0-Implementierung von OAuth 2.0 und OpenID Connect macht ausgiebig Gebrauch von Bearertoken (auch in Form von JWTs). Ein Trägertoken ist ein einfaches Sicherheitstoken, das dem „Träger“ den Zugriff auf eine geschützte Ressource ermöglicht. In diesem Kontext ist der „Träger“ jede beliebige Partei, die das Token vorweisen kann. Um das Trägertoken zu erhalten, muss sich die Partei zwar zunächst bei Azure AD authentifizieren, falls jedoch keine Maßnahmen ergriffen werden, um das Token bei der Übertragung und Speicherung zu schützen, kann das Token von einer fremden Partei abgefangen und verwendet werden. Einige Sicherheitstoken verfügen über einen integrierten Mechanismus, der eine unbefugte Verwendung durch nicht autorisierte Parteien verhindert. Trägertoken besitzen dagegen keinen solchen Mechanismus und müssen über einen sicheren Kanal wie etwa Transport Layer Security (HTTPS) übertragen werden. Wird ein Bearertoken als Klartext gesendet, kann eine böswillige Partei das Token mithilfe eines Man-in-the-Middle-Angriffs abfangen und damit unautorisiert auf eine geschützte Ressource zugreifen. Die gleichen Sicherheitsprinzipien gelten für die (Zwischen-)Speicherung von Trägertoken zur späteren Verwendung. Stellen Sie daher sicher, dass Ihre App Bearertoken stets auf sichere Weise überträgt und speichert. Weitere Sicherheitsüberlegungen zu Bearertoken finden Sie unter [RFC 6750, Abschnitt 5](https://tools.ietf.org/html/rfc6750).

Weitere Informationen zu verschiedenen Tokentypen, die im v2.0-Endpunkt verwendet werden, finden Sie in der [Tokenreferenz zum v2.0-Endpunkt](v2-id-and-access-tokens.md).

## <a name="protocols"></a>Protokolle

Wenn Sie einige Beispielanforderungen sehen möchten, beginnen Sie mit einem der folgenden Lernprogramme. Jedes Lernprogramm entspricht einem bestimmten Szenario. Wenn Sie Hilfe bei der Bestimmung des richtigen Arbeitsablaufs benötigen, informieren Sie sich über die [App-Typen, die mit dem v2.0-Endpunkt erstellt werden können](v2-app-types.md).

* [Erstellen von mobilen und nativen Anwendungen mit OAuth 2.0](v2-oauth2-auth-code-flow.md)
* [Erstellen von Web-Apps mit OpenID Connect](v2-protocols-oidc.md)
* [Erstellen von Single-Page-Apps mit dem impliziten OAuth 2.0-Datenfluss](v2-oauth2-implicit-grant-flow.md)
* [Erstellen von Daemons oder serverseitigen Prozessen mit dem OAuth 2.0-Clientanmeldeinformations-Datenfluss](v2-oauth2-client-creds-grant-flow.md)
* [Abrufen von Token in einer Web-API mit dem „Im Namen von“-Datenfluss von OAuth 2.0](v2-oauth2-on-behalf-of-flow.md)
