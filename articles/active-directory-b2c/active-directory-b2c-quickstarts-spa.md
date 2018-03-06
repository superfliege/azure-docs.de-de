---
title: "Testen einer einseitigen Azure AD B2C-fähigen App"
description: "Schnellstart zum Testen einer einseitigen Beispiel-App, für die Azure Active Directory B2C zum Authentifizieren und Registrieren von Benutzern verwendet wird."
services: active-directory-b2c
documentationcenter: 
author: PatAltimore
manager: mtillman
ms.reviewer: saraford
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: quickstart
ms.date: 2/13/2018
ms.author: patricka
ms.openlocfilehash: e659fd228c2294313a62b331c8e530b7d34073ac
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="quickstart-test-drive-an-azure-ad-b2c-enabled-single-page-app"></a>Schnellstart: Testen einer einseitigen Azure AD B2C-fähigen App

Azure Active Directory (Azure AD) B2C ermöglicht die Cloudidentitätsverwaltung zum Schützen Ihrer Anwendung, Ihres Unternehmens und Ihrer Kunden. Mit Azure AD B2C können sich Ihre Apps über offene Standardprotokolle bei Konten für soziale Netzwerke und bei Unternehmenskonten authentifizieren.

In diesem Schnellstart verwenden Sie eine einseitige Azure AD B2C-fähige App, indem Sie ein soziales Netzwerk als Identitätsanbieter verwenden und eine per Azure AD B2C geschützte Web-API aufrufen.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

* [Visual Studio 2017](https://www.visualstudio.com/downloads/) mit der Workload **ASP.NET und Webentwicklung**
* Installieren von [Node.js](https://nodejs.org/en/download/)
* Sie besitzen ein Konto bei einem sozialen Netzwerk, d.h. bei Facebook, Google, Microsoft oder Twitter.

## <a name="download-the-sample"></a>Herunterladen des Beispiels

[Laden Sie eine ZIP-Datei herunter](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp/archive/master.zip), oder klonen Sie die Beispiel-Web-App aus GitHub.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp.git
```

## <a name="run-the-sample-application"></a>Ausführen der Beispielanwendung

So führen Sie dieses Beispiel über die Node.js-Eingabeaufforderung aus 

```
cd active-directory-b2c-javascript-msal-singlepageapp
npm install && npm update
node server.js
```

Die Node.js-App gibt die Nummer des Ports aus, über den sie unter „localhost“ lauscht.

```
Listening on port 6420...
```

Navigieren Sie in einem Webbrowser zur URL `http://localhost:6420` der App.

![Beispielanwendung im Browser](media/active-directory-b2c-quickstarts-spa/sample-app-spa.png)

## <a name="create-an-account"></a>Erstellen eines Kontos

Klicken Sie auf die Schaltfläche **Login** (Anmelden), um den Azure AD B2C-Workflow zum **Registrieren oder Anmelden** basierend auf einer Azure AD B2C-Richtlinie zu starten. 

In der Beispielanwendung werden mehrere Registrierungsoptionen unterstützt, z.B. die Nutzung eines sozialen Netzwerks als Identitätsanbieter oder die Erstellung eines lokalen Kontos per E-Mail-Adresse. Verwenden Sie für diese Schnellstartanleitung ein Konto bei einem sozialen Netzwerk als Identitätsanbieter (etwa Facebook, Google, Microsoft oder Twitter). 

### <a name="sign-up-using-a-social-identity-provider"></a>Registrieren mit einem sozialen Netzwerk als Identitätsanbieter

Azure AD B2C zeigt eine benutzerdefinierte Anmeldeseite für eine fiktive Marke „Wingtip Toys“ für die Beispiel-Web-App an. 

1. Um sich mit einem Social Media-Konto als Identitätsanbieter zu registrieren, klicken Sie auf die Schaltfläche des Identitätsanbieters, den Sie verwenden möchten.

    ![Anbieter für Registrierung oder Anmeldung](media/active-directory-b2c-quickstarts-spa/sign-in-or-sign-up-spa.png)

    Sie authentifizieren sich mit den Anmeldeinformationen Ihres Social Media-Kontos und autorisieren die Anwendung dazu, Informationen von Ihrem Social Media-Konto zu lesen. Wenn Sie der Anwendung Zugriff auf diese gewähren, kann die Anwendung Profilinformationen aus dem Social Media-Konto abrufen, z.B. Ihren Namen und Ihre Stadt. 

2. Schließen Sie den Anmeldevorgang für den Identitätsanbieter ab. Beispiel: Wenn Sie Twitter ausgewählt haben, geben Sie Ihre Twitter-Anmeldeinformationen ein, und klicken Sie auf **Anmelden**.

    ![Authentifizierung und Autorisierung mit einem Social Media-Konto](media/active-directory-b2c-quickstarts-spa/twitter-authenticate-authorize-spa.png)

    Die Profildetails für Ihr neues Konto werden mit Informationen aus Ihrem Social Media-Konto aufgefüllt. 

3. Aktualisieren Sie die Felder „Anzeigename“, „Position“ und „Stadt“, und klicken Sie auf **Fortfahren**.  Die von Ihnen eingegebenen Werte werden für das Profil Ihres Azure AD B2C-Benutzerkontos verwendet.

    Sie haben erfolgreich ein neues Azure AD B2C-Benutzerkonto über einen Identitätsanbieter erstellt. 

## <a name="access-a-protected-web-api-resource"></a>Zugreifen auf eine geschützte Web-API-Ressource

Klicken Sie auf die Schaltfläche **Call Web API** (Web-API aufrufen), um Ihren Anzeigenamen des Web-API-Aufrufs als JSON-Objekt zurückzugeben. 

![Web-API-Antwort](media/active-directory-b2c-quickstarts-spa/call-api-spa.png)

Die einseitige Beispiel-App enthält ein Azure AD-Zugriffstoken in der Anforderung, die an die geschützte Web-API-Ressource gesendet wird, um den Vorgang zum Zurückgeben des JSON-Objekts durchzuführen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Sie können Ihren Azure AD B2C-Mandanten für weitere Azure AD B2C-Schnellstarts oder -Tutorials verwenden. Wenn Sie ihn nicht mehr benötigt, können Sie [Ihren Azure AD B2C-Mandanten löschen](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie eine Azure AD B2C-fähige ASP.NET-Beispiel-App verwendet, um sich über eine benutzerdefinierte Anmeldeseite anzumelden, sich mit einem sozialen Netzwerk als Identitätsanbieter anzumelden, ein Azure AD B2C-Konto zu erstellen und eine per Azure AD B2C geschützte Web-API aufzurufen. 

Der nächste Schritt besteht darin, einen eigenen Azure AD B2C-Mandanten zu erstellen und das Beispiel für die Ausführung über Ihren Mandanten zu konfigurieren. 

> [!div class="nextstepaction"]
> [Erstellen eines Azure Active Directory B2C-Mandanten im Azure-Portal](active-directory-b2c-get-started.md)