---
title: Mobile App, die Web-APIs aufruft – Übersicht | Microsoft Identity Platform
description: Erfahren Sie, wie Sie eine mobile App erstellen, die Web-APIs aufruft (Übersicht)
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: dadobali
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: eb0acd1534bab11eb57a7aa0e695f192b5999ed2
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65080316"
---
# <a name="scenario-mobile-application-that-calls-web-apis"></a>Szenario: Mobile App, die Web-APIs aufruft

Erfahren Sie, wie Sie eine mobile App erstellen, die Web-APIs aufruft.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [Prerequisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="getting-started"></a>Erste Schritte

Erstellen Sie Ihre erste mobile App und probieren Sie einen Schnellstart aus.

> [!div class="nextstepaction"]
> [Schnellstart: Abrufen eines Tokens und Aufrufen der Microsoft Graph-API von einer Android-App aus](./quickstart-v2-android.md)
>
> [Schnellstart: Abrufen eines Tokens und Aufrufen der Microsoft Graph-API von einer iOS-App aus](./quickstart-v2-ios.md)
>
> [Schnellstart: Abrufen eines Tokens und Aufrufen der Microsoft Graph-API von einer Xamarin.iOS- und Xamarin.Android-App aus](https://github.com/Azure-Samples/active-directory-xamarin-native-v2)

## <a name="overview"></a>Übersicht

Wenn Sie eine mobile App entwickeln, stellen Sie Ihren Endbenutzern eine personalisierte und übersichtliche Benutzeroberfläche bereit.  Microsoft Identity Platform ermöglicht mobilen Entwicklern, genau dies für iOS- und Android-Benutzer umzusetzen. Benutzer von Azure AD, einem persönlichen Microsoft-Konto und Azure AD B2C können sich bei Ihrer Anwendung anmelden und Token abrufen, um eine Web-API in ihrem Auftrag abzurufen. Mithilfe der Microsoft Authentication Library (MSAL), die den [Flow für den OAuth 2.0-Autorisierungscode](v2-oauth2-auth-code-flow.md) nach Branchenstandard implementiert, können Sie diese Flows implementieren.

![Daemon-Apps](./media/scenarios/mobile-app.svg)

Überlegungen zur mobilen App:

- ***Benutzerfreundlichkeit ist der Schlüssel:*** Zeigen Sie Benutzern, noch bevor diese zur Anmeldung aufgefordert werden, welche Vorteile Ihre App bietet, und fordern Sie nur die Berechtigungen an, die unbedingt erforderlich sind.
- ***Unterstützen aller Benutzerkonfigurationen:*** Viele Geschäftsbenutzer mobiler Apps haben nur bedingten Zugriff und müssen Gerätekonformitätsrichtlinien beachten. Stellen Sie sicher, dass diese wichtigen Szenarios unterstützt werden.
- ***Implementieren von einmaligem Anmelden (SSO):*** Mit MSAL und Microsoft Identity Platform ist es einfach, einmaliges Anmelden (SSO) über den Gerätebrowser oder Microsoft Authenticator (und das Intune-Unternehmensportal unter Android) zu aktivieren.

## <a name="specifics"></a>Besonderheiten

Beim Erstellen einer mobilen App mit Microsoft Identity Platform sollten Sie folgende Überlegungen berücksichtigen:

- Abhängig von der Plattform kann bei der ersten Anmeldung ein Eingreifen erforderlich sein. Unter iOS beispielsweise muss die App eine Benutzeraktion anzeigen, wenn einmaliges Anmelden (SSO) zum ersten Mal über Microsoft Authenticator (und das Intune-Unternehmensportal unter Android) abgerufen wird.
- Unter iOS und Android verwendet MSAL möglicherweise einen externen Browser zum Anmelden von Benutzern, zu dem Sie von Ihrer App weitergeleitet werden. Dies kann angepasst werden, um stattdessen In-App-Webansichten zu verwenden.
- Verwenden Sie niemals ein Geheimnis in einer mobilen App, da alle Benutzer darauf zugreifen könnten.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [App-Registrierung](scenario-mobile-app-registration.md)
