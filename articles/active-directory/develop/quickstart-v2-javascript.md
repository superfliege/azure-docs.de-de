---
title: 'Schnellstart: Azure AD v2 JavaScript | Microsoft-Dokumentation'
description: Es wird beschrieben, wie JavaScript-Anwendungen, für die Zugriffstoken vom Azure Active Directory v2.0-Endpunkt erforderlich sind, eine API aufrufen können.
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/21/2018
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 4c64552ab23331755bf1d292bede61e78b339df0
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46987423"
---
# <a name="quickstart-sign-in-users-and-acquire-an-access-token-from-a-javascript-application"></a>Schnellstart: Anmelden von Benutzern und Beschaffen eines Zugriffstokens von einer JavaScript-Anwendung

[!INCLUDE [active-directory-develop-applies-v2-msal](../../../includes/active-directory-develop-applies-v2-msal.md)]

In dieser Schnellstartanleitung wird beschrieben, wie Sie ein Codebeispiel verwenden, mit dem veranschaulicht wird, wie eine Single-Page-JavaScript-Anwendung (SPA) die Anmeldung für persönliche Konten und Geschäfts-, Schul- und Unikonten durchführen, ein Zugriffstoken abrufen und die Microsoft Graph-API aufrufen kann.

![Funktionsweise der in dieser Schnellstartanleitung generierten Beispiel-App](media/quickstart-v2-javascript/javascriptspa-intro.png)

> [!div renderon="docs"]
> ## <a name="register-your-application-and-download-your-quickstart-app"></a>Registrieren Ihrer Anwendung und Herunterladen Ihrer Schnellstart-App
>
> ### <a name="step-1-register-your-application"></a>Schritt 1: Registrieren Ihrer Anwendung
>
> 1. Registrieren Sie Ihre Anwendung im [Microsoft-Anwendungsregistrierungsportal](https://apps.dev.microsoft.com/portal/register-app).
> 1. Geben Sie im Feld **Anwendungsname** einen Namen für Ihre Anwendung ein.
> 1. Vergewissern Sie sich, dass das Kontrollkästchen **Guided Setup** (Geführtes Setup) deaktiviert ist, und klicken Sie anschließend auf **Erstellen**.
> 1. Klicken Sie auf **Plattform hinzufügen**, und wählen Sie dann die Option **Web**.
> 1. Stellen Sie sicher, dass **Impliziten Fluss zulassen** *aktiviert* ist.
> 1. Fügen Sie unter **Umleitungs-URLs** die URL `http://localhost:30662/` hinzu.
> 1. Klicken Sie auf **Speichern**.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-azure-portal"></a>Schritt 1: Konfigurieren Ihrer Anwendung im Azure-Portal
> Damit das Codebeispiel für diese Schnellstartanleitung funktioniert, müssen Sie den Umleitungs-URI `http://localhost:30662/` hinzufügen und **Implizite Gewährung** aktivieren.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Make these changes for me]() (Diese Änderungen für mich vornehmen)
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Ist konfiguriert](media/quickstart-v2-javascript/green-check.png) Ihre Anwendung ist mit diesen Attributen konfiguriert.

#### <a name="step-2-download-the-project"></a>Schritt 2: Herunterladen des Projekts

Sie können eine dieser Optionen auswählen, die für Ihre Entwicklungsumgebung geeignet sind.
* [Herunterladen der Kernprojektdateien – für einen Webserver, z.B. Node.js](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/core.zip)
* [Laden Sie das Visual Studio-Projekt herunter](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/VisualStudio.zip)

Extrahieren Sie die ZIP-Datei in einen lokalen Ordner (z.B. **C:\Azure-Samples**).

#### <a name="step-3-configure-your-javascript-app"></a>Schritt 3: Konfigurieren Ihrer JavaScript-App

> [!div renderon="docs"]
> Bearbeiten Sie `msalconfig.js`, und ersetzen Sie `Enter_the_Application_Id_here` durch die Anwendungs-ID der App, die Sie gerade registriert haben. Sie finden die *Anwendungs-ID* auf der Seite *Übersicht*.

> [!div class="sxs-lookup" renderon="portal"]
> Bearbeiten Sie `msalconfig.js`, und ersetzen Sie „msalconfig“ durch Folgendes:

```javascript
var msalconfig = {
    clientID: "Enter_the_Application_Id_here",
    redirectUri: location.origin
};
```
> [!NOTE]
> Bei Verwendung von [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) wird der Umleitungs-URI auf `http://localhost:30662/` festgelegt (gemäß Konfiguration im Projekt des Codebeispiels). Wenn Sie [Node.js](https://nodejs.org/en/download/) oder einen anderen Webserver nutzen, können Sie den Umleitungs-URI auf `http://localhost:30662/` festlegen und den Server dann für das Lauschen über diesen Port konfigurieren.
>

#### <a name="step-4-run-the-project"></a>Schritt 4: Ausführen des Projekts

Drücken Sie bei Verwendung von Visual Studio **F5**, um Ihr Projekt auszuführen.

Bei Verwendung von Node.js können Sie über eine Befehlszeile im Verzeichnis des Projekts Folgendes ausführen, um den Server zu starten:
 ```batch
 npm install
 node server.js
 ```
Öffnen Sie einen Webbrowser, und navigieren Sie zu `http://localhost:30662/`. Klicken Sie auf die Schaltfläche **Microsoft Graph-API aufrufen**, um die Anmeldung zu starten.


## <a name="more-information"></a>Weitere Informationen

### <a name="msaljs"></a>*msal.js*

MSAL ist die Bibliothek zum Anmelden von Benutzern und Anfordern von Token, die für den Zugriff auf eine per Microsoft Azure Active Directory (Azure AD) geschützte API verwendet werden. Die Datei *index.html* der Schnellstartanleitung enthält einen Verweis auf die Bibliothek:

```html
<script src="https://secure.aadcdn.microsoftonline-p.com/lib/0.2.3/js/msal.min.js"></script>
````

Falls Sie Node installiert haben, ist der Download per npm möglich:

```batch
npm install msal
```

### <a name="msal-initialization"></a>MSAL-Initialisierung

Im Code der Schnellstartanleitung wird auch veranschaulicht, wie Sie die Bibliothek initialisieren:

```javascript
var userAgentApplication = new Msal.UserAgentApplication(msalconfig.clientID, null, loginCallback, {
    redirectUri: msalconfig.redirectUri
});
```

> |Hierbei gilt:  |  |
> |---------|---------|
> |`ClientId`     |Anwendungs-ID der im Azure-Portal registrierten Anwendung|
> |`authority`    |Dies ist die Autoritäts-URL. Durch das Übergeben von *null* wird die Standardautorität auf `https://login.microsoftonline.com/common` festgelegt. Legen Sie diesen Wert auf `https://login.microsoftonline.com/<tenant name or ID>` fest, wenn Ihre App auf einen Mandanten beschränkt ist (also nur für Konten in einem Verzeichnis gilt).|
> |`loginCallBack`| Aufgerufene Rückrufmethode nach der Umleitung der Authentifizierung zurück an die App|
> |`redirectUri`  |URL, an die Benutzer nach der Authentifizierung für Azure AD zurückgeleitet werden|

### <a name="sign-in-users"></a>Anmelden von Benutzern

Der folgende Codeausschnitt veranschaulicht, wie Sie Benutzer anmelden:

```javascript
userAgentApplication.loginRedirect(graphAPIScopes);
```

> |Hierbei gilt:  |  |
> |---------|---------|
> | `graphAPIScopes`   | (Optional) Enthält Bereiche, die beim Anmelden für die Zustimmung durch die Benutzer angefordert werden (Beispiel: `[ "user.read" ]` für Microsoft Graph oder `[ "api://<Application ID>/access_as_user" ]` für benutzerdefinierte Web-APIs). |

> [!TIP]
> Alternativ hierzu können Sie auch die `loginPopup`-Methode verwenden, um ein Popupfenster zum Anmelden des Benutzers anzuzeigen.

### <a name="request-tokens"></a>Anfordern von Token

MSAL verfügt über drei Methoden, die zum Beschaffen von Token verwendet werden: `acquireTokenRedirect`, `acquireTokenPopup` und `acquireTokenSilent`:

#### <a name="get-a-user-token-silently"></a>Automatisches Abrufen eines Benutzertokens

Die Methode `acquireTokenSilent` verwaltet das Abrufen und Erneuern von Token ohne Eingreifen des Benutzers. Nachdem die Methode `loginRedirect` oder `loginPopup` zum ersten Mal ausgeführt wurde, wird normalerweise die `acquireTokenSilent`-Methode zum Abrufen der Token genutzt, die für den Zugriff auf geschützte Ressourcen für nachfolgende Aufrufe verwendet werden. Aufrufe zum Anfordern oder Verlängern von Token werden im Hintergrund ausgeführt.

```javascript
// Try to acquire the token used to query Graph API silently first:
userAgentApplication.acquireTokenSilent(graphAPIScopes)
```

> |Hierbei gilt:  |  |
> |---------|---------|
> | `graphAPIScopes`   | Enthält Bereiche, die für die Rückgabe im Zugriffstoken für die API angefordert werden (Beispiel: `[ "user.read" ]` für Microsoft Graph oder `[ "api://<Application ID>/access_as_user" ]` für benutzerdefinierte Web-APIs). |

#### <a name="get-a-user-token-interactively"></a>Interaktives Abrufen eines Benutzertokens

 Es gibt Situationen, in denen Sie die Interaktion mit dem Azure AD v2.0-Endpunkt erzwingen müssen. Beispiel: 
* Benutzer müssen ihre Anmeldeinformationen ggf. erneut eingeben, weil ihr Kennwort abgelaufen ist.
* Ihre Anwendung fordert Zugriff auf zusätzliche Ressourcenbereiche an, für die der Benutzer seine Zustimmung erteilen muss.
* Die zweistufige Authentifizierung ist erforderlich.

Das übliche empfohlene Muster für die meisten Anwendungen ist zuerst das Aufrufen von `acquireTokenSilent`, das anschließende Abfangen der Ausnahme und dann das Aufrufen von `acquireTokenRedirect` (oder `acquireTokenPopup`), um eine interaktive Anforderung zu starten.

Der Aufruf von `acquireTokenRedirect(scope)` führt dazu, dass Benutzer an den Azure AD v2.0-Endpunkt umgeleitet werden (`acquireTokenPopup(scope)` führt zu einem Popupfenster), mit dem Benutzer interagieren müssen. Entweder bestätigen sie ihre Anmeldeinformationen, indem sie ihre Zustimmung zur erforderlichen Ressource erteilen, oder sie führen die zweistufige Authentifizierung durch.

```javascript
userAgentApplication.acquireTokenRedirect(graphAPIScopes);
```

## <a name="next-steps"></a>Nächste Schritte

Eine ausführlichere Schritt-für-Schritt-Anleitung zur Erstellung der Anwendung für diese Schnellstartanleitung finden Sie im unten angegebenen JavaScript-Tutorial.

### <a name="learn-the-steps-to-create-the-application-for-this-quickstart"></a>Informieren Sie sich über die Schritte zum Erstellen der Anwendung für diese Schnellstartanleitung.

> [!div class="nextstepaction"]
> [Aufrufen der Microsoft Graph-API aus einer JavaScript-Einzelseitenanwendung](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-javascriptspa)

### <a name="browse-the-msal-repo-for-documentation-faq-issues-and-more"></a>Suchen Sie im MSAL-Repository nach Dokumentation, häufig gestellten Fragen, Problemen und mehr.

> [!div class="nextstepaction"]
> [msal.js – GitHub-Repository](https://github.com/AzureAD/microsoft-authentication-library-for-js)


[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
