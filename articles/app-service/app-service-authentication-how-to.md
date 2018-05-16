---
title: Anpassen der Authentifizierung und Autorisierung in Azure App Service | Microsoft-Dokumentation
description: Veranschaulicht die Anpassung der Authentifizierung und Autorisierung in App Service und das Abrufen von Benutzeransprüchen und verschiedenen Token.
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.service: app-service
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 03/14/2018
ms.author: cephalin
ms.openlocfilehash: c41cb3ef2939fe7271b1f8738fcf0cb95c4b1111
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="customize-authentication-and-authorization-in-azure-app-service"></a>Anpassen der Authentifizierung und Autorisierung in Azure App Service

Diesem Artikel erfahren Sie, wie die [Authentifizierung und Autorisierung in App Service](app-service-authentication-overview.md) angepasst und Identitäten über Ihre Anwendung verwaltet werden. 

Sehen Sie sich eines der folgenden Tutorials an, um sofort loszulegen:

* [Tutorial: Umfassendes Authentifizieren und Autorisieren von Benutzern in Azure App Service (Windows)](app-service-web-tutorial-auth-aad.md)
* [Tutorial: Umfassendes Authentifizieren und Autorisieren von Benutzern in Azure App Service für Linux](containers/tutorial-auth-aad.md)
* [Konfigurieren Ihrer App Service-Anwendung zur Verwendung der Azure Active Directory-Anmeldung](app-service-mobile-how-to-configure-active-directory-authentication.md)
* [Konfigurieren Ihrer App Service-Anwendung zur Verwendung der Facebook-Anmeldung](app-service-mobile-how-to-configure-facebook-authentication.md)
* [Konfigurieren Ihrer App Service-Anwendung zur Verwendung der Google-Anmeldung](app-service-mobile-how-to-configure-google-authentication.md)
* [Konfigurieren Ihrer App Service-Anwendung zur Verwendung der Microsoft-Kontoanmeldung](app-service-mobile-how-to-configure-microsoft-authentication.md)
* [Konfigurieren Ihrer App Service-Anwendung zur Verwendung der Twitter-Anmeldung](app-service-mobile-how-to-configure-twitter-authentication.md)

## <a name="configure-multiple-sign-in-options"></a>Konfigurieren mehrerer Anmeldeoptionen

Die Portalkonfiguration bietet keine einfache Möglichkeit, Ihren Benutzern mehrere Anmeldeoptionen (z.B. sowohl Facebook und Twitter) bereitzustellen. Allerdings lässt sich diese Funktionalität Ihrer Web-App problemlos hinzufügen. Dazu sind folgende Schritte erforderlich:

Zunächst konfigurieren Sie im Azure-Portal auf der Seite **Authentifizierung/Autorisierung** alle Identitätsanbieter, die Sie aktivieren möchten.

Wählen Sie für **Die auszuführende Aktion, wenn die Anforderung nicht authentifiziert ist** die Option **Anonyme Anforderungen zulassen (keine Aktion)** aus.

Fügen Sie auf der Anmeldeseite, der Navigationsleiste oder an einer anderen Stelle in Ihrer Web-App einen Anmeldelink für alle Anbieter hinzu, die Sie aktiviert haben (`/.auth/login/<provider>`). Beispiel: 

```HTML
<a href="/.auth/login/aad">Log in with Azure AD</a>
<a href="/.auth/login/microsoftaccount">Log in with Microsoft Account</a>
<a href="/.auth/login/facebook">Log in with Facebook</a>
<a href="/.auth/login/google">Log in with Google</a>
<a href="/.auth/login/twitter">Log in with Twitter</a>
```

Wenn der Benutzer auf einen der Links klickt, wird die entsprechende Anmeldeseite geöffnet, um den Benutzer anzumelden.

## <a name="access-user-claims"></a>Zugriff auf Benutzeransprüche

App Service übergibt Benutzeransprüche mithilfe spezieller Header an Ihre Anwendung. Externe Anforderungen sind nicht zum Festlegen dieser Header berechtigt, sie sind also nur vorhanden, wenn sie von App Service festgelegt wurden. Beispiele für solche Header wären etwa:

* X-MS-CLIENT-PRINCIPAL-NAME
* X-MS-CLIENT-PRINCIPAL-ID

Anhand dieser Header kann jeder Code unabhängig von Programmiersprache oder Framework die benötigten Informationen ermitteln. Bei ASP.NET 4.6-Apps wird **ClaimsPrincipal** automatisch mit entsprechenden Werten festgelegt.

Ihre Anwendung kann durch Aufrufen von `/.auth/me` auch zusätzliche Details zum authentifizierten Benutzer abrufen. Die Mobile Apps-Server-SDKs enthalten Hilfsmethoden für die Verwendung dieser Daten. Weitere Informationen finden Sie unter [Verwenden des Azure Mobile Apps SDK für Node.js](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-getidentity) sowie unter [Arbeiten mit dem .NET-Back-End-Server-SDK für Azure Mobile Apps](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#user-info).

## <a name="retrieve-tokens-in-app-code"></a>Abrufen von Token in App-Code

Die anbieterspezifischen Token werden aus dem Servercode in den Anforderungsheader eingefügt, sodass Sie problemlos darauf zugreifen können. Die folgende Tabelle zeigt mögliche Namen von Tokenheadern:

| | |
|-|-|
| Azure Active Directory | `X-MS-TOKEN-AAD-ID-TOKEN` <br/> `X-MS-TOKEN-AAD-ACCESS-TOKEN` <br/> `X-MS-TOKEN-AAD-EXPIRES-ON`  <br/> `X-MS-TOKEN-AAD-REFRESH-TOKEN` |
| Facebook-Token | `X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN` <br/> `X-MS-TOKEN-FACEBOOK-EXPIRES-ON` |
| Google | `X-MS-TOKEN-GOOGLE-ID-TOKEN` <br/> `X-MS-TOKEN-GOOGLE-ACCESS-TOKEN` <br/> `X-MS-TOKEN-GOOGLE-EXPIRES-ON` <br/> `X-MS-TOKEN-GOOGLE-REFRESH-TOKEN` |
| Microsoft-Konto | `X-MS-TOKEN-MICROSOFTACCOUNT-ACCESS-TOKEN` <br/> `X-MS-TOKEN-MICROSOFTACCOUNT-EXPIRES-ON` <br/> `X-MS-TOKEN-MICROSOFTACCOUNT-AUTHENTICATION-TOKEN` <br/> `X-MS-TOKEN-MICROSOFTACCOUNT-REFRESH-TOKEN` |
| Twitter | `X-MS-TOKEN-TWITTER-ACCESS-TOKEN` <br/> `X-MS-TOKEN-TWITTER-ACCESS-TOKEN-SECRET` |
|||

Senden Sie von Ihrem Clientcode (z.B. einer mobilen App oder JavaScript im Browser) eine HTTP-`GET`-Anforderung an `/.auth/me`. Der zurückgegebene JSON-Code enthält die anbieterspezifischen-Token.

> [!NOTE]
> Zugriffstoken sind für den Zugriff auf Anbieterressourcen vorgesehen, daher sind sie nur vorhanden, wenn Sie den Anbieter mit einem geheimen Clientschlüssel konfigurieren. Informationen zum Abrufen von Aktualisierungstoken finden Sie unter [Zugriffstoken für die Aktualisierung](#refresh-access-tokens).

## <a name="refresh-access-tokens"></a>Zugriffstoken für die Aktualisierung

Wenn das Zugriffstoken Ihres Anbieters abläuft, müssen Sie den Benutzer erneut authentifizieren. Sie können den Tokenablauf vermeiden, indem Sie einen `GET`-Aufruf an den `/.auth/refresh`-Endpunkt Ihrer Anwendung durchführen. Bei einem Aufruf aktualisiert App Service automatisch die Zugriffstoken im Tokenspeicher für den authentifizierten Benutzer. Bei nachfolgenden Anforderungen von Token durch Ihren App-Code werden die aktualisierten Token abgerufen. Damit die Tokenaktualisierung funktioniert, muss der Tokenspeicher jedoch [Aktualisierungstoken](https://auth0.com/learn/refresh-tokens/) für Ihren Anbieter enthalten. Die jeweilige Methode zum Abrufen von Aktualisierungstoken ist von den einzelnen Anbietern dokumentiert, die folgende Liste stellt jedoch eine kurze Zusammenfassung dar:

- **Google**: Fügen Sie Ihrem `/.auth/login/google`-API-Aufruf einen Abfragezeichenfolgen-Parameter vom Typ `access_type=offline` an. Bei Verwendung des Mobile Apps SDK können Sie den Parameter einer der `LogicAsync`-Überladungen hinzufügen (siehe [Google-Aktualisierungstoken](https://developers.google.com/identity/protocols/OpenIDConnect#refresh-tokens)).
- **Facebook**: Stellt keine Aktualisierungstoken bereit. Langlebige Token laufen nach 60 ab (siehe [Verlängern von Zugriffsschlüsseln für Seiten](https://developers.facebook.com/docs/facebook-login/access-tokens/expiration-and-extension)).
- **Twitter-**: Zugriffstoken laufen nicht ab (siehe [Häufig gestellte Fragen zu OAuth für Twitter](https://developer.twitter.com/docs/basics/authentication/guides/oauth-faq)).
- **Microsoft-Konto**: Wählen Sie beim [Konfigurieren der Authentifizierungseinstellungen für das Microsoft-Konto](app-service-mobile-how-to-configure-microsoft-authentication.md) den Bereich `wl.offline_access` aus.
- **Azure Active Directory**: Führen Sie in [https://resources.azure.com](https://resources.azure.com) die folgenden Schritte aus:
    1. Wählen Sie am oberen Seitenrand die Option **Lesen/Schreiben** aus.
    1. Navigieren Sie im linken Browser zu **subscriptions** > **_\<Name des\_Abonnements_** > **resourceGroups** > _**\<Name\_der\_Ressourcengruppe>**_ > **providers** > **Microsoft.Web** > **sites** > _**\<App\_Name>**_ > **config** > **authsettings**. 
    1. Klicken Sie auf **Edit**.
    1. Ändern Sie die folgende Eigenschaft. Ersetzen Sie _\<app\_id>_ mit der ID der Azure Active Directory-Anwendung des Diensts, auf den Sie zugreifen möchten.

        ```json
        "additionalLoginParams": ["response_type=code id_token", "resource=<app_id>"]
        ```

    1. Klicken Sie auf **Put**. 

Sobald Ihr Anbieter konfiguriert ist, können Sie anzeigen, ob sich Aktualisierungstoken im Tokenspeicher befinden, indem Sie `/.auth/me` aufrufen. 

Um das Zugriffstoken jederzeit zu aktualisieren, rufen Sie einfach `/.auth/refresh` in einer beliebigen Sprache auf. Im folgenden Codeausschnitt wird jQuery verwendet, um Ihre Zugriffstoken aus einem JavaScript-Client zu aktualisieren.

```JavaScript
function refreshTokens() {
  var refreshUrl = "/.auth/refresh";
  $.ajax(refreshUrl) .done(function() {
    console.log("Token refresh completed successfully.");
  }) .fail(function() {
    console.log("Token refresh failed. See application logs for details.");
  });
}
```

Wenn ein Benutzer die Ihrer App gewährten Berechtigungen widerruft, schlägt Ihr Aufruf von `/.auth/me` möglicherweise mit der Antwort `403 Forbidden` fehl. Überprüfen Sie Ihre Anwendungsprotokolle auf Details, um Fehler zu diagnostizieren.

## <a name="extend-session-expiration-grace-period"></a>Verlängern der Toleranzperiode für den Sitzungsablauf

Nachdem eine authentifizierte Sitzung abgelaufen ist, gilt standardmäßig eine Toleranzperiode von 72 Stunden. Innerhalb dieser Toleranzperiode sind Sie berechtigt, das Sitzungscookie oder Sitzungstoken mit App Service zu aktualisieren, ohne den Benutzer erneut zu authentifizieren. Rufen Sie einfach `/.auth/refresh` auf, wenn Ihr Sitzungscookie oder -token ungültig wird. Sie müssen den Tokenablauf nicht selbst nachverfolgen. Nach Ablauf der 72-stündigen Toleranzperiode muss sich der Benutzer neu anmelden, um ein gültiges Sitzungscookie oder Sitzungstoken zu erhalten.

Wenn 72 Stunden für Sie nicht ausreichend sind, können Sie dieses Ablauffenster erweitern. Die Erweiterung des Ablauffensters über einen langen Zeitraum kann sich erheblich auf die Sicherheit auswirken (z.B. wenn ein Authentifizierungstoken kompromittiert oder gestohlen wird). Daher wird empfohlen, den Standardwert von 72 Stunden beizubehalten oder den Ablaufzeitraum auf den kleinsten Wert festzulegen.

Zum Erweitern des standardmäßigen Ablauffensters führen Sie den folgenden Befehl in [Cloud Shell](../cloud-shell/overview.md) aus.

```azurecli-interactive
az webapp auth update --resource-group <group_name> --name <app_name> --token-refresh-extension-hours <hours>
```

> [!NOTE]
> Die Toleranzperiode gilt nur für die authentifizierte App Service-Sitzung, nicht jedoch für die Token von den Identitätsanbietern. Es gibt keine Toleranzperiode für die abgelaufenen Anbietertoken. 
>

## <a name="limit-the-domain-of-sign-in-accounts"></a>Beschränken der Domäne von Anmeldekonten

Die Anmeldung über mehrere Domänen ist sowohl bei Microsoft-Konten als auch bei Azure Active Directory möglich. Microsoft-Konten lassen beispielsweise _outlook.com_-, _live.com_- und _hotmail.com_-Konten zu. Azure Active Directory lässt eine beliebige Anzahl von benutzerdefinierten Domänen für die Anmeldekonten zu. Dieses Verhalten ist für eine interne App möglicherweise nicht wünschenswert, wenn nicht jeder Benutzer mit einem _outlook.com_-Konto über Zugriff verfügen soll. Gehen Sie folgendermaßen vor, um den Domänennamen der Anmeldekonten zu beschränken.

Navigieren Sie in [https://resources.azure.com](https://resources.azure.com) zu **subscriptions** > **_\<Name des\_Abonnements_** > **resourceGroups** > _**\<Name\_der\_Ressourcengruppe>**_ > **providers** > **Microsoft.Web** > **sites** > _**\<App\_Name>**_ > **config** > **authsettings**. 

Klicken Sie auf **Bearbeiten**, ändern Sie die folgende Eigenschaft, und klicken Sie dann auf **Put**. Achten Sie darauf, _\<Domänen\_Name>_ durch die gewünschte Domäne zu ersetzen.

```json
"additionalLoginParams": ["domain_hint=<domain_name>"]
```
## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Tutorial: Umfassendes Authentifizieren und Autorisieren von Benutzern in Azure App Service (Windows)](app-service-web-tutorial-auth-aad.md)
> [Tutorial: Umfassendes Authentifizieren und Autorisieren von Benutzern in Azure App Service (Linux)](containers/tutorial-auth-aad.md)