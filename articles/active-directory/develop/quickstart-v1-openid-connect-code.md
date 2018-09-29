---
title: Erstellen einer Node.js Express-Web-App für das An- und Abmelden mit Azure Active Directory | Microsoft Docs
description: Hier erfahren Sie, wie eine Express-MVC-Web-App mit node.js erstellen, die für die Anmeldung in Azure AD integriert wird.
services: active-directory
documentationcenter: nodejs
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 81deecec-dbe2-4e75-8bc0-cf3788645f99
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: quickstart
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 19563f76c261fda1fca53babcb553f2dceeaa345
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46990092"
---
# <a name="quickstart-build-a-nodejs-express-web-app-for-sign-in-and-sign-out-with-azure-active-directory"></a>Schnellstart: Erstellen einer Node.js Express-Web-App für das An- und Abmelden mit Azure Active Directory

[!INCLUDE [active-directory-develop-applies-v1](../../../includes/active-directory-develop-applies-v1.md)]

Passport ist eine Authentifizierungs-Middleware für Node.js. Passport ist flexibel und modular und kann unauffällig in jede Express- oder Restify-basierte Webanwendung integriert werden. Ein umfassender Satz an Strategien unterstützt die Authentifizierung mittels eines Benutzernamens und Kennworts in Facebook, Twitter und anderen Anwendungen. Für Azure Active Directory (Azure AD) installieren wir dieses Modul und fügen dann das `passport-azure-ad`-Plug-In von Azure AD hinzu.

In diesem Schnellstart erfahren Sie, wie Sie mit Passport die folgenden Aufgaben ausführen können:

* Anmelden des Benutzers bei der App mit Azure AD.
* Anzeigen einiger Informationen zum Benutzer
* Abmelden des Benutzers von der App

Zur Entwicklung einer vollständigen, funktionstüchtigen Anwendung müssen Sie folgende Schritte ausführen:

1. Registrieren einer App
2. Legen Sie fest, dass Ihre App die Strategie `passport-azure-ad` verwendet.
3. Verwenden Sie Passport zur Ausgabe von An- und Abmeldeanforderungen für Azure AD.
4. Drucken Sie Informationen zum Benutzer aus.

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie zum Einstieg die folgenden erforderlichen Schritte aus:

* [App-Gerüst als ZIP-Datei herunterladen](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/skeleton.zip)
  
    * Klonen Sie das Gerüst:

        ```git clone --branch skeleton https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```

    Der Code für diesen Schnellstart wird [auf GitHub](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS)verwaltet. Die fertige Anwendung wird außerdem am Ende dieses Schnellstarts bereitgestellt.

* Stellen Sie einen Azure AD-Mandanten bereit, in dem Sie Benutzer erstellen und Ihre Anwendung registrieren können. Wenn Sie über noch keinen Mandanten verfügen, [erfahren Sie hier, wie Sie einen erhalten](quickstart-create-new-tenant.md).

## <a name="step-1-register-an-app"></a>Schritt 1: Registrieren einer App

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen im Menü oben auf der Seite Ihr Konto aus. Wählen Sie in der **Verzeichnisliste** den Active Directory-Mandanten aus, bei dem Sie Ihre Anwendung registrieren möchten.
1. Wählen Sie im Menü auf der linken Seite **Alle Dienste** und dann **Azure Active Directory**.
1. Wählen Sie **App-Registrierungen** und dann **Hinzufügen** aus.
1. Folgen Sie den Bildschirmaufforderungen, und erstellen Sie eine neue **Webanwendung** und/oder **Web-API**.

    * Der **Name** der Anwendung beschreibt Ihre Anwendung für Benutzer.
    * Die **Anmelde-URL** ist die Basis-URL Ihrer Anwendung. Der Standardwert des Gerüsts lautet `http://localhost:3000/auth/openid/return`.

1. Nach Abschluss der Registrierung weist Azure AD Ihrer App eine eindeutige Anwendungs-ID zu. Diesen Wert benötigen Sie in den nächsten Abschnitten. Daher sollten Sie ihn von der Anwendungsseite kopieren.
1. Aktualisieren Sie auf der Seite **Einstellungen > Eigenschaften** für Ihre Anwendung den App-ID-URI. 
    
    Die **App-ID-URI** ist eine eindeutige Kennung für die Anwendung. Üblicherweise wird das Format `https://<tenant-domain>/<app-name>` verwendet, z. B.: `https://contoso.onmicrosoft.com/my-first-aad-app`.

1. Fügen Sie auf der Seite **Einstellungen > Antwort-URLs** für Ihre Anwendung die URL hinzu, die in Schritt 5 der Anmelde-URL hinzugefügt wurde, und wählen Sie dann **Speichern** aus.
1. Führen Sie zum Erstellen eines geheimen Schlüssels Schritt 4 unter [So fügen Sie Anmeldeinformationen für Anwendungen oder Zugriffsberechtigungen für Web-APIs hinzu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#to-add-application-credentials-or-permissions-to-access-web-apis) aus.

   > [!IMPORTANT]
   > Kopieren Sie den Wert des Anwendungsschlüssels. Dies ist der Wert für `clientSecret` und wird weiter unten in **Schritt 3** benötigt. 

## <a name="step-2-add-prerequisites-to-your-directory"></a>Schritt 2: Hinzufügen von Voraussetzungen zu Ihrem Verzeichnis

1. Wechseln Sie über die Befehlszeile vom Verzeichnis auf Ihren Stammordner, wenn dies noch nicht der Fall ist, und führen Sie dann die folgenden Befehle aus:

    * `npm install express`
    * `npm install ejs`
    * `npm install ejs-locals`
    * `npm install restify`
    * `npm install mongoose`
    * `npm install bunyan`
    * `npm install assert-plus`
    * `npm install passport`

1. Sie benötigen auch `passport-azure-ad`, also führen Sie den folgenden Befehl aus:

    * `npm install passport-azure-ad`

Dadurch werden die Bibliotheken installiert, von denen `passport-azure-ad` abhängt.

## <a name="step-3-set-up-your-app-to-use-the-passport-node-js-strategy"></a>Schritt 3: Einrichten Ihrer App zur Nutzung der „passport-node-js“-Strategie

Hier konfigurieren Sie Express für die Verwendung des OpenID Connect-Authentifizierungsprotokolls. Passport wird für verschiedene Zwecke verwendet, unter anderem für die Ausgabe von Anmelde- und Abmeldeanforderungen, für die Verwaltung der Benutzerssitzungen und für das Abrufen der Benutzerinformationen.

1. Öffnen Sie die Datei `config.js` im Stammverzeichnis des Projekts, und geben Sie dann die Konfigurationswerte Ihrer App im Abschnitt `exports.creds` ein.

    * `clientID` ist die **Anwendungs-ID** , die Ihrer App im Registrierungsportal zugewiesen ist.
    * `returnURL` ist die **Antwort-URL**, die Sie im Portal eingegeben haben.
    * `clientSecret` ist der geheime Schlüssel, den Sie im Portal generiert haben.

1. Öffnen Sie als Nächstes die Datei `app.js` im Stammverzeichnis des Projekts. Fügen Sie den folgenden Aufruf hinzu, um die Strategie `OIDCStrategy` aufzurufen, die zu `passport-azure-ad` gehört.

    ```JavaScript
    var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

    // add a logger

    var log = bunyan.createLogger({
        name: 'Microsoft OIDC Example Web Application'
    });
    ```

1. Verwenden Sie danach die Strategie, auf die gerade verwiesen wurde, um die Anmeldeanforderungen zu verarbeiten.

    ```JavaScript
    // Use the OIDCStrategy within Passport. (Section 2)
    //
    //   Strategies in passport require a `validate` function that accepts
    //   credentials (in this case, an OpenID identifier), and invokes a callback
    //   with a user object.
    passport.use(new OIDCStrategy({
        callbackURL: config.creds.returnURL,
        realm: config.creds.realm,
        clientID: config.creds.clientID,
        clientSecret: config.creds.clientSecret,
        oidcIssuer: config.creds.issuer,
        identityMetadata: config.creds.identityMetadata,
        skipUserProfile: config.creds.skipUserProfile,
        responseType: config.creds.responseType,
        responseMode: config.creds.responseMode
    },
    function(iss, sub, profile, accessToken, refreshToken, done) {
        if (!profile.email) {
        return done(new Error("No email found"), null);
        }
        // asynchronous verification, for effect...
        process.nextTick(function () {
        findByEmail(profile.email, function(err, user) {
            if (err) {
            return done(err);
            }
            if (!user) {
            // "Auto-registration"
            users.push(profile);
            return done(null, profile);
            }
            return done(null, user);
        });
        });
    }
    ));
    ```
   Passport verwendet ein ähnliches Muster für alle Strategien (Twitter, Facebook usw.), das beim Schreiben aller Strategien eingehalten wird. Beim Betrachten der Strategie sehen Sie, dass ein function-Element übergeben wird, das Token- und done-Elemente als Parameter aufweist. Die Strategie kehrt wieder an den Ausgangspunkt zurück, sobald die Arbeit abgeschlossen ist. Dann speichern wir den Benutzer und das Token, damit wir beides nicht mehr erfragen müssen.

   > [!IMPORTANT]
   > Der obige Code erfasst alle Benutzer, die sich an unserem Server authentifizieren. Dies wird als automatische Registrierung bezeichnet. Es wird empfohlen, die Authentifizierung bei einem Produktionsserver erst erlauben zu lassen, wenn dieser über einen von Ihnen festgelegten Vorgang registriert wurde. Dies ist normalerweise das Muster, das Sie bei Consumer-Apps vorfinden. Es ermöglicht Ihnen, sich bei Facebook zu registrieren, aber dann werden Sie aufgefordert, zusätzliche Informationen anzugeben. Wenn dies nicht eine Beispielanwendung wäre, könnten wir die E-Mail-Adresse des Benutzers einfach aus dem Tokenobjekt extrahieren, das zurückgegeben wird, und den Benutzer dann dazu auffordern, zusätzliche Informationen einzugeben. Da es sich um einen Testserver handelt, fügen wir sie der In-Memory Database hinzu.

1. Fügen Sie die Methoden hinzu, mit denen die angemeldeten Benutzer nachverfolgt werden können, wie es für Passport erforderlich ist. Diese Methoden umfassen die Serialisierung und Deserialisierung von Informationen des Benutzers.

    ```JavaScript
    // Passport session setup. (Section 2)

    //   To support persistent sign-in sessions, Passport needs to be able to
    //   serialize users into the session and deserialize them out of the session. Typically,
    //   this is done simply by storing the user ID when serializing and finding
    //   the user by ID when deserializing.
    passport.serializeUser(function(user, done) {
        done(null, user.email);
    });

    passport.deserializeUser(function(id, done) {
        findByEmail(id, function (err, user) {
            done(err, user);
        });
    });

    // array to hold signed-in users
    var users = [];

    var findByEmail = function(email, fn) {
        for (var i = 0, len = users.length; i < len; i++) {
            var user = users[i];
            log.info('we are using user: ', user);
            if (user.email === email) {
                return fn(null, user);
            }
        }
        return fn(null, null);
    };
    ```

1. Fügen Sie den Code hinzu, um die Express-Engine zu laden. Hier verwend wir die standardmäßigen Muster für „/views“ und „/routes“, die Express bietet.

    ```JavaScript
    // configure Express (section 2)

      var app = express();
      app.configure(function() {
      app.set('views', __dirname + '/views');
      app.set('view engine', 'ejs');
      app.use(express.logger());
      app.use(express.methodOverride());
      app.use(cookieParser());
      app.use(expressSession({ secret: 'keyboard cat', resave: true, saveUninitialized: false }));
      app.use(bodyParser.urlencoded({ extended : true }));
      // Initialize Passport!  Also use passport.session() middleware, to support
      // persistent login sessions (recommended).
      app.use(passport.initialize());
      app.use(passport.session());
      app.use(app.router);
      app.use(express.static(__dirname + '/../../public'));
    });
    ```

1. Fügen Sie schließlich die Routen hinzu, die die eigentlichen Anmeldeanforderungen an die `passport-azure-ad`-Engine übergeben:

    ```JavaScript
    // Our Auth routes (section 3)

    // GET /auth/openid
    //   Use passport.authenticate() as route middleware to authenticate the
    //   request. The first step in OpenID authentication involves redirecting
    //   the user to their OpenID provider. After authenticating, the OpenID
    //   provider redirects the user back to this application at
    //   /auth/openid/return.
    app.get('/auth/openid',
    passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
    function(req, res) {
        log.info('Authentication was called in the Sample');
        res.redirect('/');
    });

    // GET /auth/openid/return
    //   Use passport.authenticate() as route middleware to authenticate the
    //   request. If authentication fails, the user is redirected back to the
    //   sign-in page. Otherwise, the primary route function is called,
    //   which, in this example, redirects the user to the home page.
    app.get('/auth/openid/return',
      passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
      function(req, res) {
        log.info('We received a return from AzureAD.');
        res.redirect('/');
      });

    // POST /auth/openid/return
    //   Use passport.authenticate() as route middleware to authenticate the
    //   request. If authentication fails, the user is redirected back to the
    //   sign-in page. Otherwise, the primary route function is called,
    //   which, in this example, redirects the user to the home page.
    app.post('/auth/openid/return',
      passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
      function(req, res) {
        log.info('We received a return from AzureAD.');
        res.redirect('/');
      });
    ```

## <a name="step-4-use-passport-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a>Schritt 4: Verwenden von Passport zum Übergeben von An- und Abmeldeanforderungen an Azure AD

Ihre Anwendung ist nun ordnungsgemäß für die Kommunikation mit dem Endpunkt über das Authentifizierungsprotokoll OpenID Connect konfiguriert. `passport-azure-ad` kümmert sich um alle Details der Erstellung von Authentifizierungsnachrichten, die Überprüfung der Azure AD-Token und Verwaltung von Benutzersitzungen. Sie müssen Ihren Benutzern nur noch die An- und Abmeldung ermöglichen und zusätzliche Informationen zu den angemeldeten Benutzern sammeln.

1. Fügen Sie Ihrer Datei `app.js` die Standard-, Anmelde-, Konto- und Abmeldemethoden hinzu:

    ```JavaScript
    //Routes (section 4)

    app.get('/', function(req, res){
      res.render('index', { user: req.user });
    });

    app.get('/account', ensureAuthenticated, function(req, res){
      res.render('account', { user: req.user });
    });

    app.get('/login',
      passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
      function(req, res) {
        log.info('Login was called in the Sample');
        res.redirect('/');
    });

    app.get('/logout', function(req, res){
      req.logout();
      res.redirect('/');
    });
    ```

1. Überprüfen Sie diese im Detail:

    * Die Route `/` leitet an die Ansicht „index.ejs“ weiter und übergibt den Benutzer in der Anforderung (falls vorhanden).
    * Die Route `/account` *stellt sicher, dass wir authentifiziert sind* (wird im folgenden Beispiel implementiert) und übergibt dann den Benutzer in der Anforderung, damit wir zusätzliche Informationen zum Benutzer abrufen können.
    * Die Route `/login` ruft unsere azuread-openidconnect-Authentifizierung von `passport-azuread` auf. Wenn dies nicht erfolgreich ist, leitet die Route den Benutzer wieder an „/login“ zurück.
    * `/logout` ruft einfach „logout.ejs“ (und die Route) auf, um Cookies zu löschen, und leitet dann den Benutzer zu „index.ejs“ zurück.

1. Fügen Sie im letzten Teil von `app.js` die **EnsureAuthenticated**-Methode hinzu, die in `/account` wie oben gezeigt verwendet wird.

    ```JavaScript
    // Simple route middleware to ensure user is authenticated. (section 4)

    //   Use this route middleware on any resource that needs to be protected. If
    //   the request is authenticated (typically via a persistent sign-in session),
    //   the request proceeds. Otherwise, the user is redirected to the
    //   sign-in page.
    function ensureAuthenticated(req, res, next) {
      if (req.isAuthenticated()) { return next(); }
      res.redirect('/login')
    }
    ```

1. Erstellen Sie als schließlich den Server selbst in `app.js`:

    ```JavaScript
    app.listen(3000);
    ```

## <a name="step-5-to-display-our-user-in-the-website-create-the-views-and-routes-in-express"></a>Schritt 5: Erstellen von Ansichten und Routen in Express, um Benutzer auf der Website anzuzeigen

`app.js` ist jetzt vollständig. Sie müssen einfach nur die Routen und Ansichten hinzufügen, die die Informationen für die Benutzer anzeigen und die erstellten Routen `/logout` und `/login` verarbeiten.

1. Erstellen der Route `/routes/index.js` im Stammverzeichnis

    ```JavaScript
    /*
     * GET home page.
     */

    exports.index = function(req, res){
      res.render('index', { title: 'Express' });
    };
    ```

1. Erstellen der Route `/routes/user.js` im Stammverzeichnis

    ```JavaScript
    /*
     * GET users listing.
     */

    exports.list = function(req, res){
      res.send("respond with a resource");
    };
    ```

    Diese übergeben lediglich die Anforderung an die Ansichten, einschließlich des Benutzers, falls vorhanden.

1. Erstellen Sie die Ansicht `/views/index.ejs` im Stammverzeichnis. Dies ist eine einfache Seite, die die Anmelde- und Abmeldemethoden aufruft und uns ermöglicht, Kontoinformationen zu erfassen. Beachten Sie, dass eine bedingte `if (!user)`-Anweisung verwendet werden kann, da das Übergeben des Benutzers in der Anforderung belegt, dass er ein angemeldeter Benutzer ist.

    ```JavaScript
    <% if (!user) { %>
        <h2>Welcome! Please log in.</h2>
        <a href="/login">Log In</a>
    <% } else { %>
        <h2>Hello, <%= user.displayName %>.</h2>
        <a href="/account">Account Info</a></br>
        <a href="/logout">Log Out</a>
    <% } %>
    ```

1. Erstellen Sie die Ansicht `/views/account.ejs` im Stammverzeichnis, sodass zusätzliche Informationen angezeigt werden können, die von `passport-azure-ad` in die Benutzeranforderung eingefügt wurden.

    ```Javascript
    <% if (!user) { %>
        <h2>Welcome! Please log in.</h2>
        <a href="/login">Log In</a>
    <% } else { %>
    <p>displayName: <%= user.displayName %></p>
    <p>givenName: <%= user.name.givenName %></p>
    <p>familyName: <%= user.name.familyName %></p>
    <p>UPN: <%= user._json.upn %></p>
    <p>Profile ID: <%= user.id %></p>
    <p>Full Claimes</p>
    <%- JSON.stringify(user) %>
    <p></p>
    <a href="/logout">Log Out</a>
    <% } %>
    ```

1. Fügen Sie ein Layout hinzu, um das Erscheinungsbild der Seite zu optimieren. Erstellen Sie die Ansicht `/views/layout.ejs` im Stammverzeichnis.

    ```HTML

    <!DOCTYPE html>
    <html>
        <head>
            <title>Passport-OpenID Example</title>
        </head>
        <body>
            <% if (!user) { %>
                <p>
                <a href="/">Home</a> |
                <a href="/login">Log In</a>
                </p>
            <% } else { %>
                <p>
                <a href="/">Home</a> |
                <a href="/account">Account</a> |
                <a href="/logout">Log Out</a>
                </p>
            <% } %>
            <%- body %>
        </body>
    </html>
    ```

## <a name="step-6-build-and-run-your-app"></a>Schritt 6: Erstellen und Ausführen der App

1. Führen Sie `node app.js` aus, und navigieren Sie zu `http://localhost:3000`.
1. Melden Sie sich entweder mit einem persönlichen Microsoft-Konto oder einem Geschäfts-, Schul- oder Unikonto an.

    Beachten Sie, wie die Identität des Benutzers in der Liste „/account“ angezeigt wird. Sie verfügen jetzt über eine mit branchenüblichen Protokollen gesicherte Web-App, die Benutzer mit ihren persönlichen Konten oder ihren Geschäfts- oder Schulkonten authentifizieren kann.

    Als Referenz stellen wir das vollständige Beispiel (ohne Ihre Konfigurationswerte) [als ZIP-Datei](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/master.zip)bereit. Sie können es alternativ aus GitHub klonen:

    ```git clone --branch master https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```

## <a name="next-steps"></a>Nächste Schritte

Sie können sich nun anderen Szenarien zuwenden und diese ausprobieren:

> [!div class="nextstepaction"]
> [Schützen einer Web-API mit Azure AD](quickstart-v1-nodejs-webapi.md)