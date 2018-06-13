---
title: Authentifizierung und Autorisierung in Azure App Service | Microsoft Docs
description: Eine grundlegende Übersicht über das Authentifizierungs-/Autorisierungsfeature für Azure App Service.
services: app-service
documentationcenter: ''
author: mattchenderson
manager: erikre
editor: ''
ms.assetid: b7151b57-09e5-4c77-a10c-375a262f17e5
ms.service: app-service
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 08/29/2016
ms.author: mahender
ms.openlocfilehash: c180dcf5d769245f3fa2485ccee2cbc18ecf5f67
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
ms.locfileid: "33763491"
---
# <a name="authentication-and-authorization-in-azure-app-service"></a>Authentifizierung und Autorisierung in Azure App Service

Azure App Service bietet integrierte Authentifizierungs- und Autorisierungsunterstützung, sodass Sie für das Anmelden von Benutzern und den Zugriff auf Daten nur wenig oder keinen Code in Ihrer Web-App, API und dem mobilen Back-End sowie in [Azure Functions](../azure-functions/functions-overview.md) schreiben müssen. In diesem Artikel wird beschrieben, wie App Service zur Vereinfachung der Authentifizierung und Autorisierung für Ihre App beiträgt. 

Eine sichere Authentifizierung und Autorisierung erfordert umfassende Sicherheitskenntnisse, u.a. zu Verbund, Verschlüsselung, Verwaltung von [JSON-Webtoken (JWT)](https://wikipedia.org/wiki/JSON_Web_Token) und [Gewährungstypen](https://oauth.net/2/grant-types/). App Service bietet diese Hilfsprogramme, damit Sie mehr Zeit und Energie für das Bereitstellen von geschäftlichem Nutzen für Ihre Kunden aufwenden können.

> [!NOTE]
> Die Nutzung von App Service für die Authentifizierung und Autorisierung ist nicht verpflichtend. Viele Webframeworks sind mit Sicherheitsfunktionen gebündelt und können bei Bedarf verwendet werden. Wenn Sie mehr Flexibilität benötigen, als App Service bietet, können Sie auch eigene Hilfsprogramme schreiben.  
>

Spezielle Informationen zu nativen mobilen Apps finden Sie unter [Authentifizierung und Autorisierung in Azure Mobile Apps](../app-service-mobile/app-service-mobile-auth.md).

## <a name="how-it-works"></a>So funktioniert's

Das Modul für Authentifizierung und Autorisierung wird in der gleichen Sandbox wie Ihr Anwendungscode ausgeführt. Wenn es aktiviert ist, wird es von allen eingehenden HTTP-Anforderungen durchlaufen, bevor diese von Ihrem Anwendungscode behandelt werden.

![](media/app-service-authentication-overview/architecture.png)

Dieses Modul erledigt einige Dinge für Ihre App:

- Authentifizierung von Benutzern mit dem angegebenen Anbieter
- Überprüfung, Speicherung und Aktualisierung von Token
- Verwaltung der authentifizierten Sitzung
- Einfügen von Identitätsinformationen in Anforderungsheader

Das Modul wird getrennt vom Anwendungscode ausgeführt und mithilfe von App-Einstellungen konfiguriert. Weder SDKs noch bestimmte Sprachen oder Änderungen am Anwendungscode sind erforderlich. 

### <a name="user-claims"></a>Benutzeransprüche

App Service stellt Ihrem Code für alle Sprachenframeworks die Ansprüche des Benutzers zur Verfügung, indem sie in die Anforderungsheader eingefügt werden. Für ASP.NET 4.6-Apps füllt App Service die [ClaimsPrincipal.Current](/dotnet/api/system.security.claims.claimsprincipal.current)-Eigenschaft mit Ansprüchen des authentifizierten Benutzers, sodass Sie dem standardmäßigen .NET Codemuster einschließlich des `[Authorize]`-Attributs folgen können. Auf ähnliche Weise füllt App Service die `_SERVER['REMOTE_USER']`-Variable für PHP-Apps.

Für [Azure Functions](../azure-functions/functions-overview.md) wird `ClaimsPrincipal.Current` nicht für .NET Code extrahiert, Sie können die Benutzeransprüche jedoch in den Anforderungsheadern finden.

Weitere Informationen finden Sie unter [Zugriff auf Benutzeransprüche](app-service-authentication-how-to.md#access-user-claims).

### <a name="token-store"></a>Tokenspeicher

App Service bietet einen integrierten Tokenspeicher. Dabei handelt es sich um ein Repository mit Token, die den Benutzern Ihrer Web-Apps, APIs oder nativen mobilen Apps zugeordnet sind. Wenn Sie die Authentifizierung für jeden Anbieter aktivieren, ist dieser Tokenspeicher sofort für Ihre App verfügbar. Wenn Ihr Anwendungscode im Auftrag des Benutzers auf Daten dieser Anbieter zugreifen muss, z.B um: 

- Beiträge in der Facebook-Chronik des authentifizierten Benutzers zu veröffentlichen
- Unternehmensdaten des authentifizierten Benutzers aus der Azure Active Directory Graph-API oder sogar Microsoft Graph zu lesen,

werden die ID-, Zugriffs- und Aktualisierungstoken für die authentifizierte Sitzung zwischengespeichert und sind nur für den zugehörigen Benutzer zugänglich.  

In der Regel müssen Sie Code schreiben, um diese Token in Ihrer Anwendung zu erfassen, speichern und aktualisieren. Mit dem Tokenspeicher [rufen Sie die Token bei Bedarf einfach ab](app-service-authentication-how-to.md#retrieve-tokens-in-app-code) und [weisen App Service an, sie zu aktualisieren](app-service-authentication-how-to.md#refresh-access-tokens), wenn sie ungültig werden. 

Wenn Sie in Ihrer App nicht mit Token arbeiten müssen, können Sie den Tokenspeicher deaktivieren.

### <a name="logging-and-tracing"></a>Protokollierung und Nachverfolgung

Wenn Sie die [Anwendungsprotokollierung](web-sites-enable-diagnostic-log.md) aktivieren, werden Ablaufverfolgungen für Authentifizierung und Autorisierung direkt in den Protokolldateien angezeigt. Wenn ein unerwarteter Authentifizierungsfehler angezeigt wird, können Sie alle Details dazu problemlos in den vorhandenen Anwendungsprotokollen finden. Wenn Sie die [Ablaufverfolgung für Anforderungsfehler](web-sites-enable-diagnostic-log.md) aktivieren, sehen Sie genau, welche Rolle das Modul für Authentifizierung und Autorisierung möglicherweise bei einer fehlgeschlagenen Anforderung gespielt hat. Suchen Sie in den Ablaufverfolgungsprotokollen nach Verweisen auf ein Modul namens `EasyAuthModule_32/64`. 

## <a name="identity-providers"></a>Identitätsanbieter

App Service nutzt die [Verbundidentität](https://en.wikipedia.org/wiki/Federated_identity). Dabei werden die Benutzeridentitäten und der Authentifizierungsablauf von einem externen Identitätsanbieter für Sie verwaltet. Fünf Identitätsanbieter sind standardmäßig verfügbar: 

| Anbieter | Anmeldungsendpunkt |
| - | - |
| [Azure Active Directory](../active-directory/active-directory-whatis.md) | `/.auth/login/aad` |
| [Microsoft-Konto](../active-directory/develop/active-directory-appmodel-v2-overview.md) | `/.auth/login/microsoft` |
| [Facebook](https://developers.facebook.com/docs/facebook-login) | `/.auth/login/facebook` |
| [Google](https://developers.google.com/+/web/api/rest/oauth) | `/.auth/login/google` |
| [Twitter](https://developer.twitter.com/docs/basics/authentication) | `/.auth/login/twitter` |

Wenn Sie die Authentifizierung und Autorisierung mit einem dieser Anbieter aktivieren, ist der entsprechende Anmeldungsendpunkt für die Benutzerauthentifizierung und die Überprüfung von Authentifizierungstoken vom Anbieter verfügbar. Sie können Ihren Benutzern problemlos eine beliebige Anzahl von diesen Anmeldeoptionen bereitstellen. Außerdem können Sie einen weiteren Identitätsanbieter oder eine [eigene benutzerdefinierte Identitätslösung][custom-auth] integrieren.

## <a name="authentication-flow"></a>Authentifizierungsfluss

Der Authentifizierungsablauf gilt für alle Anbieter, unterscheidet sich jedoch abhängig davon, ob die Anmeldung mit dem SDK des Anbieters erfolgen soll:

- Ohne die Anbieter-SDK: Die Anwendung delegiert die Verbundanmeldung an App Service. Dies ist normalerweise bei Browser-Apps der Fall, die dem Benutzer die Anmeldeseite des Anbieters anzeigen können. Der Servercode verwaltet den Anmeldevorgang, daher wird er auch als _servergesteuerter Datenfluss_ oder _Serverfluss_ bezeichnet. Dieser Fall gilt für Web-Apps. Es gilt auch für native Apps, die Benutzer mithilfe des Client SDK von Mobile Apps anmelden, da das SDK eine Webansicht zum Anmelden von Benutzern mit der App Service-Authentifizierung öffnet. 
- Mit Anbieter-SDK: Die Anwendung meldet den Benutzer manuell an und sendet dann das Authentifizierungstoken zur Überprüfung an App Service. Dies ist normalerweise bei Apps ohne Browser der Fall, die dem Benutzer die Anmeldeseite des Anbieters nicht anzeigen können. Der Anwendungscode verwaltet den Anmeldevorgang, daher wird er auch als _clientgesteuerter Datenfluss_ oder _Clientfluss_ bezeichnet. Dieser Fall gilt für REST-APIs [Azure Functions](../azure-functions/functions-overview.md) und JavaScript-Browserclients sowie Web-Apps, die eine höhere Flexibilität beim Anmeldevorgang erfordern. Er gilt auch für native mobile Apps, die Benutzer mithilfe des Anbieter-SDKs anmelden.

> [!NOTE]
> Bei Aufrufen aus einer vertrauenswürdigen Browser-App in App Service wird eine weitere REST-API in App Service aufgerufen, oder [Azure Functions](../azure-functions/functions-overview.md) kann über den servergesteuerten Datenfluss authentifiziert werden. Weitere Informationen finden Sie unter [Authentifizierung und Autorisierung in Azure App Service]().
>

Die folgende Tabelle zeigt die Schritte des Authentifizierungsablaufs.

| Schritt | Ohne Anbieter-SDK | Mit Anbieter-SDK |
| - | - | - |
| 1. Anmeldung des Benutzers | Client wird zu `/.auth/login/<provider>` umgeleitet. | Clientcode meldet Benutzer direkt mit dem Anbieter-SDK an und erhält ein Authentifizierungstoken. Informationen finden Sie in der Dokumentation des Anbieters. |
| 2. Nachauthentifizierung | Anbieter leitet Client zu `/.auth/login/<provider>/callback` um. | Clientcode sendet Token des Anbieters zur Überprüfung an `/.auth/login/<provider>`. |
| 3. Einrichten der authentifizierten Sitzung | App Service fügt der Antwort ein authentifiziertes Cookie hinzu. | App Service gibt das eigene Authentifizierungstoken an den Clientcode zurück. |
| 4. Bereitstellen von authentifiziertem Inhalt | Client schließt Authentifizierungscookie in nachfolgenden Anforderungen (die automatisch vom Browser verarbeitet werden) ein. | Clientcode stellt Authentifizierungstoken im `X-ZUMO-AUTH`-Header (der automatisch von Mobile Apps-Client-SDKs verarbeitet wird) bereit. |

Für Clientbrowser kann App Service alle nicht authentifizierten Benutzer automatisch an `/.auth/login/<provider>` weiterleiten. Sie können Benutzern auch einen oder mehrere `/.auth/login/<provider>`-Links zur Anmeldung bei Ihrer App mit dem Anbieter ihrer Wahl bereitstellen.

<a name="authorization"></a>

## <a name="authorization-behavior"></a>Autorisierungsverhalten

Im [Azure-Portal](https://portal.azure.com) können Sie die App Service-Autorisierung mit einer Reihe von Verhaltensweisen konfigurieren.

![](media/app-service-authentication-overview/authorization-flow.png)

Die folgenden Überschriften beschreiben die Optionen.

### <a name="allow-all-requests-default"></a>Alle Anforderungen zulassen (Standard)

Authentifizierung und Autorisierung werden nicht von App Service (deaktiviert) verwaltet. 

Wählen Sie diese Option, wenn Sie keine Authentifizierung und Autorisierung benötigen, oder wenn Sie einen eigenen Authentifizierungs- und Autorisierungscode schreiben möchten.

### <a name="allow-only-authenticated-requests"></a>Nur authentifizierte Anforderungen zulassen

Die Option lautet **Mit \<Anbieter> anmelden**. App Service leitet alle anonymen Anforderungen an `/.auth/login/<provider>` für den ausgewählten Anbieter um. Wenn die anonyme Anforderung von einer nativen mobilen App stammt, wird `HTTP 401 Unauthorized` als Antwort zurückgegeben.

Mit dieser Option müssen Sie in Ihrer App keinen Authentifizierungscode schreiben. Eine genauere Autorisierung, z.B. rollenspezifische Autorisierung, kann durch das Untersuchen der Ansprüche des Benutzers durchgeführt werden (siehe [Zugriff auf Benutzeransprüche](app-service-authentication-how-to.md#access-user-claims)).

### <a name="allow-all-requests-but-validate-authenticated-requests"></a>Alle Anforderungen zulassen, aber authentifizierte Anforderungen überprüfen

Die Option lautet **Anonyme Anforderungen zulassen**. Diese Option aktiviert die Authentifizierung und Autorisierung in App Service, überlässt Autorisierungsentscheidungen jedoch Ihrem Anwendungscode. Für authentifizierte Anforderungen übergibt App Service auch Authentifizierungsinformationen in den HTTP-Headern. 

Diese Option bietet mehr Flexibilität bei der Verarbeitung anonymer Anforderungen. Beispielsweise können Sie Ihren Benutzern [mehrere Anmeldeoptionen bereitstellen](app-service-authentication-how-to.md#configure-multiple-sign-in-options). Allerdings müssen Sie Code schreiben. 

## <a name="more-resources"></a>Weitere Ressourcen

[Tutorial: Umfassendes Authentifizieren und Autorisieren von Benutzern in Azure App Service (Windows)](app-service-web-tutorial-auth-aad.md)  
[Tutorial: Umfassendes Authentifizieren und Autorisieren von Benutzern in Azure App Service für Linux](containers/tutorial-auth-aad.md)  
[Anpassen der Authentifizierung und Autorisierung in App Service](app-service-authentication-how-to.md)

Anbieterspezifische Anleitungen:

* [Konfigurieren Ihrer App Service-Anwendung zur Verwendung der Azure Active Directory-Anmeldung][AAD]
* [Konfigurieren Ihrer App Service-Anwendung zur Verwendung der Facebook-Anmeldung][Facebook]
* [Konfigurieren Ihrer App Service-Anwendung zur Verwendung der Google-Anmeldung][Google]
* [Konfigurieren Ihrer App Service-Anwendung zur Verwendung der Microsoft-Kontoanmeldung][MSA]
* [Konfigurieren Ihrer App Service-Anwendung zur Verwendung der Twitter-Anmeldung][Twitter]
* [Vorgehensweise: Verwenden einer benutzerdefinierten Authentifizierung für Ihre Anwendung][custom-auth]

[AAD]: app-service-mobile-how-to-configure-active-directory-authentication.md
[Facebook]: app-service-mobile-how-to-configure-facebook-authentication.md
[Google]: app-service-mobile-how-to-configure-google-authentication.md
[MSA]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Twitter]: app-service-mobile-how-to-configure-twitter-authentication.md

[custom-auth]: ../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth

[ADAL-Android]: ../app-service-mobile/app-service-mobile-android-how-to-use-client-library.md#adal
[ADAL-iOS]: ../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#adal
[ADAL-dotnet]: ../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#adal
