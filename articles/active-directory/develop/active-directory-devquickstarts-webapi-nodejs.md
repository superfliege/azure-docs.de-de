---
title: Erste Schritte mit Azure Active Directory Node.js | Microsoft-Dokumentation
description: "In diesem Thema erfahren Sie, wie Sie eine Node.js-REST-Web-API erstellen, die für die Authentifizierung in Azure AD integriert wird."
services: active-directory
documentationcenter: nodejs
author: craigshoemaker
manager: routlaw
ms.assetid: 7654ab4c-4489-4ea5-aba9-d7cdc256e42a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 10/17/2017
ms.author: cshoe
ms.custom: aaddev
ms.openlocfilehash: 6f67a2cf2baabfa10c6a8e81b085ca6991b981dd
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2017
---
# <a name="secure-nodejs-web-api-with-azure-active-directory"></a>Schützen einer Node.JS-Web-API mit Azure Active Directory

In diesem Artikel wird das Schützen eines [Restify](http://restify.com/)-API-Endpunkts mit [Passport](http://passportjs.org/) mithilfe des [passport-azure-ad](https://github.com/AzureAD/passport-azure-ad)-Moduls zur Behandlung der Kommunikation mit Azure Active Directory (AAD) veranschaulicht. 

Dieses Tutorial behandelt die Aspekte des Schützens von API-Endpunkten. Die Aspekte der Anmeldung und Aufbewahrung von Authentifizierungstoken sind hier nicht implementiert und gehören zur Verantwortlichkeit einer Clientanwendung. Näheres zu Clientimplementierungen erfahren Sie in [An- und Abmeldung bei NodeJS-Web-Apps mit Azure AD](active-directory-devquickstarts-openidconnect-nodejs.md).

Das vollständige Codebeispiel zu diesem Artikel ist auf [GitHub](https://github.com/Azure-Samples/active-directory-node-webapi-basic) verfügbar.

## <a name="create-the-sample-project"></a>Erstellen des Beispielprojekts
Diese Serveranwendung erfordert einige Paketabhängigkeiten zur Unterstützung von Restify und Passport sowie Kontoinformationen, die AAD übergeben werden.

Kopieren Sie zu Anfang den folgenden Code in eine Datei namens `package.json`:

```Shell
{
  "name": "node-aad-demo",
  "version": "0.0.1",
  "scripts": {
    "start": "node app.js"
  },
  "dependencies": {
    "passport": "0.4.0",
    "passport-azure-ad": "3.0.8",
    "restify": "6.0.1",
    "restify-plugins": "1.6.0"
  }
}
```

Sobald `package.json` erstellt ist, führen Sie `npm install` in der Befehlszeile aus, um die Paketabhängigkeiten zu installieren. 

### <a name="configure-the-project-to-use-active-directory"></a>Konfigurieren des Projekts zur Verwendung von Active Directory

Um mit der Konfiguration der Anwendung zu beginnen, rufen Sie einige kontospezifische Werte mit der Azure CLI ab. Die einfachste Möglichkeit zum Einstieg in die CLI ist die Verwendung der Azure Cloud Shell.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Geben Sie in der Cloud Shell-Instanz folgenden Befehl ein: 

```azurecli-interactive
az ad app create --display-name node-aad-demo --homepage http://localhost --identifier-uris http://node-aad-demo
```

Zu den [Argumenten](/cli/azure/ad/app?view=azure-cli-latest#az_ad_app_create) des `create`-Befehls zählen:

| Argument  | Beschreibung |
|---------|---------|
|`display-name` | Ein Anzeigename der Registrierung |
|`homepage` | Die URL, unter der sich Benutzer bei Ihrer Anwendung anmelden und sie nutzen können |
|`identifier-uris` | Durch Leerzeichen getrennte eindeutige URIs, die Azure AD für diese Anwendung verwenden kann |

Bevor Sie eine Verbindung mit Azure Active Directory herstellen können, benötigen Sie die folgenden Informationen:

| Name  | Beschreibung | Variablenname in der Config-Datei |
| ------------- | ------------- | ------------- |
| Mandantenname  | Der [Mandantenname](active-directory-howto-tenant.md), den Sie für die Authentifizierung verwenden möchten. | `tenantName`  |
| Client-ID  | Client-ID ist der OAuth-Begriff, der für die _AAD-Anwendungs-ID_ verwendet wird. |  `clientID`  |

Kopieren Sie aus der Registrierungsantwort in der Azure Cloud Shell den Wert `appId`, und erstellen Sie eine neue Datei namens `config.js`. Fügen Sie als Nächstes den folgenden Code hinzu, und ersetzen Sie ihre Werte durch die in Klammern gesetzten Token:

```JavaScript
const tenantName    = //<YOUR_TENANT_NAME>;
const clientID      = //<YOUR_APP_ID_FROM_CLOUD_SHELL>;
const serverPort    = 3000;

module.exports.serverPort = serverPort;

module.exports.credentials = {
  identityMetadata: `https://login.microsoftonline.com/${tenantName}.onmicrosoft.com/.well-known/openid-configuration`, 
  clientID: clientID
};
```
Um weitere Informationen zu den einzelnen Konfigurationseinstellungen zu erhalten, lesen Sie die Dokumentation zum [passport-azure-ad](https://github.com/AzureAD/passport-azure-ad#5-usage)-Modul.

## <a name="implement-the-server"></a>Implementieren des Servers
Das [passport-azure-ad](https://github.com/AzureAD/passport-azure-ad#5-usage)-Modul verfügt über zwei Authentifizierungsstrategien: [OIDC](https://github.com/AzureAD/passport-azure-ad#51-oidcstrategy) und [Bearer](https://github.com/AzureAD/passport-azure-ad#52-bearerstrategy). Der in diesem Artikel implementierte Server verwendet die Bearer-Strategie zum Schützen des API-Endpunkts.

### <a name="step-1-import-dependencies"></a>Schritt 1: Importieren von Abhängigkeiten
Erstellen Sie eine neue Datei namens `app.js`, und fügen Sie folgenden Text ein:

```JavaScript
const
      restify = require('restify')
    , restifyPlugins = require('restify-plugins')
    , passport = require('passport')
    , BearerStrategy = require('passport-azure-ad').BearerStrategy
    , config = require('./config')
    , authenticatedUserTokens = []
    , serverPort = process.env.PORT || config.serverPort
;
```

In diesem Codeabschnitt:

- Auf die Module `restify` und `restify-plugins` wird verwiesen, um einen Restify-Server einzurichten.

- Die Module `passport` und `passport-azure-ad` sind für die Kommunikation mit AAD verantwortlich.

- Die Variable `config` wird mit Werten aus der im vorherigen Schritt erstellten Datei `config.js` initialisiert.

- Für `authenticatedUserTokens` wird ein Array erstellt, um Benutzertoken zu speichern, wenn sie an geschützte Endpunkte übergeben werden.

- Der `serverPort` wird entweder vom Port der Prozessumgebung oder der Konfigurationsdatei aus definiert.

### <a name="step-2-instantiate-an-authentication-strategy"></a>Schritt 2: Instanziieren einer Authentifizierungsstrategie
Beim Schützen eines Endpunkts müssen Sie eine Strategie bereitstellen, um zu ermitteln, ob die aktuelle Anforderung von einem authentifizierten Benutzer stammt. Hier ist die Variable `authenticatonStrategy` eine Instanz der `passport-azure-ad` `BearerStrategy`-Klasse. Fügen Sie nach den `require`-Anweisungen den folgenden Code hinzu.

```JavaScript
const authenticationStrategy = new BearerStrategy(config.credentials, (token, done) => {
    let userToken = authenticatedUserTokens.find((user) => user.sub === token.sub);

    if(!userToken) {
        authenticatedUserTokens.push(token);
    }

    return done(null, user, token);
});
```
Diese Implementierung verwendet die automatische Registrierung durch Hinzufügen von Authentifizierungstoken im `authenticatedUserTokens`-Array, wenn sie nicht bereits vorhanden sind.

Sobald eine neue Instanz der Strategie erstellt wurde, müssen Sie sie mit der `use`-Methode in Passport übergeben. Fügen Sie `app.js` folgenden Code zum Verwenden der Strategie in Passport hinzu.

```JavaScript
passport.use(authenticationStrategy);
```

### <a name="step-3-server-configuration"></a>Schritt 3: Serverkonfiguration
Sobald die Authentifizierungsstrategie definiert ist, können Sie den Restify-Server mithilfe einiger Grundeinstellungen einrichten und festlegen, dass er Passport für die Sicherheit verwendet.

```JavaScript
const server = restify.createServer({ name: 'Azure Active Directroy with Node.js Demo' });
server.use(restifyPlugins.authorizationParser());
server.use(passport.initialize());
server.use(passport.session());
```
Dieser Server wird zum Analysieren von Autorisierungsheadern initialisiert und konfiguriert und dann zur Verwendung von Passport eingerichtet.


### <a name="step-4-define-routes"></a>Schritt 4: Definieren von Routen
Jetzt können Sie Routen definieren und entscheiden, welche mit AAD geschützt werden. Dieses Projekt enthält zwei Routen, in denen die Stammebene geöffnet ist, und für die `/api`-Route ist festgelegt, dass eine Authentifizierung erforderlich ist.

Fügen Sie in `app.js` den folgenden Code für die Stammebenenroute hinzu:

```JavaScript
server.get('/', (req, res, next) => {
    res.send(200, 'Try: curl -isS -X GET http://127.0.0.1:3000/api');
    next();
});
```

Alle Anforderungen können über die Stammroute geleitet werden, und sie gibt eine Meldung zurück, die einen Befehl zum Testen der `/api`-Route enthält. Im Gegensatz dazu ist die `/api`-Route mit [`passport.authenticate` ](http://passportjs.org/docs/authenticate) gesperrt. Fügen Sie nach der Stammroute den folgenden Code hinzu.

```JavaScript
server.get('/api', passport.authenticate('oauth-bearer', { session: false }), (req, res, next) => {
    res.json({ message: 'response from API endpoint' });
    return next();
});
```

Diese Konfiguration lässt nur authentifizierte Anforderungen zu, die einen Bearer-Token-Zugriff auf `/api` enthalten. Die Option `session: false` soll verhindern, dass Sitzungen fordern, dass mit jeder Anforderung an die API ein Token übergeben wird.

Schließlich wird durch Aufrufen der `listen`-Methode festgelegt, dass der Server auf den konfigurierten Port lauscht.

```JavaScript
server.listen(serverPort);
```

## <a name="run-the-sample"></a>Ausführen des Beispiels

Jetzt ist der Server implementiert, und Sie können ihn starten, indem Sie eine Eingabeaufforderung öffnen und Folgendes eingeben:

```Shell
npm start
```

Wenn der Server ausgeführt wird, können Sie eine Anforderung an ihn senden, um die Ergebnisse zu testen. Um die Antwort der Stammroute anzuzeigen, öffnen Sie eine Bash-Shell, und geben Sie den folgenden Code ein:

```Shell 
curl -isS -X GET http://127.0.0.1:3000/
```

Wenn Sie Ihren Server ordnungsgemäß konfiguriert haben, sollte die Antwort folgendermaßen aussehen:

```Shell
HTTP/1.1 200 OK
Server: Azure Active Directroy with Node.js Demo
Content-Type: application/json
Content-Length: 49
Date: Tue, 10 Oct 2017 18:35:13 GMT
Connection: keep-alive

Try: curl -isS -X GET http://127.0.0.1:3000/api
```

Als Nächstes können Sie die Route testen, die Authentifizierung erfordert, indem Sie den folgenden Befehl in der Bash-Shell eingeben:

```Shell 
curl -isS -X GET http://127.0.0.1:3000/api
```

Wenn Sie den Server ordnungsgemäß konfiguriert haben, sollte er mit dem Status `Unauthorized` antworten.

```Shell
HTTP/1.1 401 Unauthorized
Server: Azure Active Directroy with Node.js Demo
WWW-Authenticate: token is not found
Date: Tue, 10 Oct 2017 16:22:03 GMT
Connection: keep-alive
Content-Length: 12

Unauthorized
```
Nun haben Sie eine sichere API erstellt und können einen Client implementieren, der Authentifizierungstoken an die API übergeben kann.

## <a name="next-steps"></a>Nächste Schritte
Wie in der Einführung erwähnt, müssen Sie ein Clientgegenstück zum Herstellen der Verbindung mit dem Server implementieren, das Anmelden, Abmelden und Verwalten von Token behandelt. Codebasierte Beispiele finden Sie in den Clientanwendungen in [iOS](https://github.com/MSOpenTech/azure-activedirectory-library-for-ios) und [Android](https://github.com/MSOpenTech/azure-activedirectory-library-for-android). Ein Tutorial mit schrittweisem Vorgehen finden Sie im folgenden Artikel:

> [!div class="nextstepaction"]
> [An- und Abmeldung bei NodeJS-Web-Apps mit Azure AD](active-directory-devquickstarts-openidconnect-nodejs.md)