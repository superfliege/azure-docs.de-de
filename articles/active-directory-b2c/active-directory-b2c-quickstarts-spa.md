---
title: Schnellstart – Einrichten der Anmeldung für eine Single-Page-Webanwendung mit Azure Active Directory B2C | Microsoft-Dokumentation
description: Führen Sie eine Single-Page-Beispielwebanwendung aus, bei der die Kontoanmeldung über Azure Active Directory B2C erfolgt.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: quickstart
ms.date: 7/13/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 155cdaf51ac5725a315259a0d809ba644f64110c
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/14/2018
ms.locfileid: "39048915"
---
# <a name="quickstart-set-up-sign-in-for-a-single-page-app-using-azure-active-directory-b2c"></a>Schnellstart: Einrichten der Anmeldung für eine Single-Page-Webanwendung mit Azure Active Directory B2C

Azure Active Directory (Azure AD) B2C ermöglicht die Cloudidentitätsverwaltung zum Schützen Ihrer Anwendung, Ihres Unternehmens und Ihrer Kunden. Mit Azure AD B2C können sich Ihre Apps über offene Standardprotokolle bei Konten für soziale Netzwerke und bei Unternehmenskonten authentifizieren.

In diesem Schnellstart verwenden Sie eine einseitige Azure AD B2C-fähige App, indem Sie ein soziales Netzwerk als Identitätsanbieter verwenden und eine per Azure AD B2C geschützte Web-API aufrufen.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

* [Visual Studio 2017](https://www.visualstudio.com/downloads/) mit der Workload **ASP.NET und Webentwicklung**
* Installieren von [Node.js](https://nodejs.org/en/download/)
* Ein Facebook-Konto

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

In der Beispielanwendung sollen mehrere Registrierungsoptionen unterstützt werden, z.B. die Erstellung eines lokalen Kontos per E-Mail-Adresse. Verwenden Sie für diesen Schnellstart ein Facebook-Konto. 

### <a name="sign-up-using-a-social-identity-provider"></a>Registrieren mit einem sozialen Netzwerk als Identitätsanbieter

Azure AD B2C zeigt für die Beispiel-Web-App eine benutzerdefinierte Anmeldeseite für die fiktive Marke „Wingtip Toys“ an. 

1. Um sich mit einem Konto bei einem sozialen Netzwerk als Identitätsanbieter zu registrieren, klicken Sie auf die Schaltfläche des Facebook-Identitätsanbieters.

    Sie authentifizieren sich mit den Anmeldeinformationen Ihres Social Media-Kontos und autorisieren die Anwendung dazu, Informationen von Ihrem Social Media-Konto zu lesen. Wenn Sie der Anwendung Zugriff auf diese gewähren, kann die Anwendung Profilinformationen aus dem Social Media-Konto abrufen, z.B. Ihren Namen und Ihre Stadt. 

2. Schließen Sie den Anmeldevorgang für den Identitätsanbieter ab, indem Sie Ihre Anmeldeinformationen eingeben.

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
> [Erstellen eines Azure Active Directory B2C-Mandanten im Azure-Portal](tutorial-create-tenant.md)