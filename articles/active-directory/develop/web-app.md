---
title: Web-Apps in Azure Active Directory
description: Beschreibt, worum es sich bei Web-Apps handelt, und stellt die Grundlagen des Protokollflusses, der Registrierung und des Tokenablaufs für diesen App-Typ vor.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: saeeda, jmprieur, andret
ms.custom: aaddev
ms.openlocfilehash: 0eec1100c5246bb9f5c281971453ac60abc0339f
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47225017"
---
# <a name="web-apps"></a>Web-Apps

Web-Apps sind Anwendungen, die einen Benutzer in einem Webbrowser gegenüber einer Webanwendung authentifizieren. In diesem Szenario weist die Webanwendung den Browser des Benutzers an, diesen in Azure AD anzumelden. Azure AD gibt über den Browser des Benutzers eine Anmeldeantwort mit Benutzeransprüchen in einem Sicherheitstoken zurück. Dieses Szenario unterstützt Anmeldungen über OpenID Connect, SAML 2.0 und WS-Verbundprotokolle.

## <a name="diagram"></a>Diagramm

![Authentifizierungsfluss für „Browser zu Webanwendung“](./media/authentication-scenarios/web_browser_to_web_api.png)

## <a name="protocol-flow"></a>Protokollfluss

1. Wenn ein Benutzer die Anwendung aufruft und sich anmelden muss, wird er über eine Anmeldeanforderung an den Authentifizierungsendpunkt in Azure AD umgeleitet.
1. Der Benutzer meldet sich auf der Anmeldeseite an.
1. Nach erfolgreicher Authentifizierung erstellt Azure AD ein Authentifizierungstoken und gibt eine Anmeldeantwort an die im Azure-Portal konfigurierte Antwort-URL der Anwendung zurück. Bei Produktionsanwendungen empfiehlt sich die Verwendung einer Antwort-URL mit HTTPS. Das zurückgegebene Token enthält Ansprüche für den Benutzer und Azure AD, die die Anwendung zum Überprüfen des Tokens benötigt.
1. Die Anwendung überprüft das Token mithilfe eines öffentlichen Signaturschlüssels und der Ausstellerinformationen aus dem Verbundmetadatendokument für Azure AD. Nach der Überprüfung des Tokens durch die Anwendung startet Azure AD eine neue Sitzung mit dem Benutzer. Anschließend kann der Benutzer bis zum Ablauf der Sitzung auf die Anwendung zugreifen.

## <a name="code-samples"></a>Codebeispiele

Sehen Sie sich die Codebeispiele für Szenarien vom Typ „Webbrowser zu Webanwendung“ an. Schauen Sie außerdem regelmäßig vorbei: Es kommen immer wieder neue Beispiele hinzu.

## <a name="app-registration"></a>App-Registrierung

Wenn Sie eine Web-App registrieren möchten, helfen Ihnen die Informationen unter [Registrieren einer App mit dem Azure AD v1.0-Endpunkt](quickstart-v1-add-azure-ad-app.md) weiter.

* Einzelinstanzenfähige Anwendung: Wenn Sie eine Anwendung nur für Ihre Organisation erstellen, muss diese über das Azure-Portal im Verzeichnis Ihres Unternehmens registriert werden.
* Mehrinstanzenfähige Anwendung: Wenn Sie eine Anwendung erstellen, die von Benutzern außerhalb Ihrer Organisation verwendet werden kann, muss diese ebenfalls im Verzeichnis Ihres Unternehmens registriert werden. Darüber hinaus muss sie aber auch im Verzeichnis jeder anderen Organisation registriert werden, von der die Anwendung verwendet wird. Um Ihre Anwendung in deren Verzeichnis verfügbar zu machen, können Sie einen Registrierungsprozess für Ihre Kunden einschließen, über den sie Ihrer Anwendung zustimmen können. Bei der Registrierung für Ihre Anwendung erscheint ein Dialogfeld mit den erforderlichen Berechtigungen für die Anwendung sowie mit einer Zustimmungsoption. Je nachdem, welche Berechtigungen erforderlich sind, wird unter Umständen die Zustimmung eines Administrators aus der anderen Organisation benötigt. Wenn der Benutzer oder Administrator seine Zustimmung gibt, wird die Anwendung in dessen Verzeichnis registriert.

## <a name="token-expiration"></a>Tokenablauf

Die Sitzung des Benutzers läuft ab, wenn die Gültigkeitsdauer des von Azure AD ausgestellten Tokens abläuft. Ihre Anwendung kann diesen Zeitraum bei Bedarf verkürzen und Benutzer beispielsweise bei zu langer Inaktivität abmelden. Nach Ablauf der Sitzung wird der Benutzer aufgefordert, sich erneut anzumelden.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu anderen [Anwendungstypen und -szenarien](app-types.md)
* Weitere Informationen zu den [Authentifizierungsgrundlagen](authentication-scenarios.md) von Azure AD