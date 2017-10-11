### <a name="server-auth"></a>Vorgehensweise: Authentifizierung mit einem Anbieter (Server Flow)
Damit Mobile Apps im Authentifizierungsprozess sorgt in Ihrer app zu verwalten, müssen Sie Ihre app mit Ihrem Identitätsanbieter registrieren. Dann müssen in Ihren Azure App Service konfigurieren Sie den Anwendungs-ID und den Schlüssel, die von Ihrem Anbieter bereitgestellt.
Weitere Informationen finden Sie im Lernprogramm [Hinzufügen von Authentifizierung zu Ihrer app](../articles/app-service-mobile/app-service-mobile-cordova-get-started-users.md).

Sobald Sie Ihren Identitätsanbieter registriert haben, rufen Sie die `.login()` Methode mit dem Namen des Anbieters. Geben Sie beispielsweise Folgendes ein, um die Anmeldung mit Facebook Verwenden des folgenden Codes:

```
client.login("facebook").done(function (results) {
     alert("You are now logged in as: " + results.userId);
}, function (err) {
     alert("Error: " + err);
});
```

Die gültigen Werte für den Anbieter sind "Aad", "Facebook", "Google", "Microsoftaccount" und "twitter".

> [!NOTE]
> Google-Authentifizierung funktioniert zurzeit nicht über den Server übertragen.  Sie müssen zum Authentifizieren mit Google verwenden eine [Client-Flow-Methode](#client-auth).

In diesem Fall wird in Azure App Service OAuth 2.0-authentifizierungsfluss verwaltet.  Es zeigt die Anmeldeseite des ausgewählten Anbieters und ein App Service-Authentifizierungstoken nach der erfolgreichen Anmeldung mit dem Identitätsanbieter generiert. Die Anmeldefunktion gibt nach Abschluss des Vorgangs ein JSON-Objekt, das den Benutzer-ID und die App-Dienst verfügbar macht Authentifizierungstoken in den Feldern UserId und AuthenticationToken bzw.. Dieses Token kann zwischengespeichert und wiederverwendet, bis sie abläuft werden soll.

###<a name="client-auth"></a>Vorgehensweise: Authentifizierung mit einem Anbieter (Client Flow)

Ihre app auch unabhängig wenden Sie sich an den Identitätsanbieter und klicken Sie dann das zurückgegebene Token zur Authentifizierung auf Ihren App Service angeben. Dieser Ablauf der-Client können Sie eine einmalige Anmeldung für Benutzer zu ermöglichen oder zusätzliche Benutzerdaten vom Identitätsanbieter abgerufen.

#### <a name="social-authentication-basic-example"></a>Einfaches Beispiel mit sozialen Authentifizierung

In diesem Beispiel verwendet die Facebook-Client SDK für die Authentifizierung:

```
client.login(
     "facebook",
     {"access_token": token})
.done(function (results) {
     alert("You are now logged in as: " + results.userId);
}, function (err) {
     alert("Error: " + err);
});

```
In diesem Beispiel wird davon ausgegangen, dass das vom jeweiligen Anbieter SDK bereitgestellte Token in der token-Variable gespeichert ist.

#### <a name="microsoft-account-example"></a>Microsoft-Account-Beispiel

Im folgenden Beispiel wird das Live SDK, die Single-Sign-on für Windows Store-apps unterstützt, mithilfe von Microsoft-Account:

```
WL.login({ scope: "wl.basic"}).then(function (result) {
      client.login(
            "microsoftaccount",
            {"authenticationToken": result.session.authentication_token})
      .done(function(results){
            alert("You are now logged in as: " + results.userId);
      },
      function(error){
            alert("Error: " + err);
      });
});

```

In diesem Beispiel ruft ein Token von Live Connect, die mit dem App-Dienst bereitgestellt wird, durch Aufrufen der Funktion für die Anmeldung ab.

###<a name="auth-getinfo"></a>Vorgehensweise: Abrufen von Informationen zu den authentifizierten Benutzer

Die Authentifizierungsinformationen kann abgerufen werden, aus der `/.auth/me` über einen HTTP-Endpunkt mit einer AJAX-Bibliothek aufrufen.  Stellen Sie sicher, Sie legen die `X-ZUMO-AUTH` Header Authentifizierungstoken.  Das Authentifizierungstoken befindet sich in `client.currentUser.mobileServiceAuthenticationToken`.  Geben Sie beispielsweise Folgendes ein, um die API zum Abrufen von Daten verwenden:

```
var url = client.applicationUrl + '/.auth/me';
var headers = new Headers();
headers.append('X-ZUMO-AUTH', client.currentUser.mobileServiceAuthenticationToken);
fetch(url, { headers: headers })
    .then(function (data) {
        return data.json()
    }).then(function (user) {
        // The user object contains the claims for the authenticated user
    });
```

Abrufen von Daten steht als [Npm-Paket](https://www.npmjs.com/package/whatwg-fetch) oder für den Download Browser [CDNJS](https://cdnjs.com/libraries/fetch). JQuery oder einer anderen AJAX-API können auch die Informationen abzurufen.  Daten werden als JSON-Objekt empfangen.
