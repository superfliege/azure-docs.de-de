---
title: 'Tutorial: Gewähren des Zugriffs auf eine Node.js-Web-API über eine Desktop-App unter Verwendung von Azure Active Directory B2C | Microsoft-Dokumentation'
description: In diesem Tutorial erfahren Sie, wie Sie mit Active Directory B2C eine Node.js-Web-API schützen und sie über eine .NET-Desktop-App aufrufen.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.author: davidmu
ms.date: 3/01/2018
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.component: B2C
ms.openlocfilehash: 01c13b214d40fba278ce788047e2b158adc20287
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34711595"
---
# <a name="tutorial-grant-access-to-a-nodejs-web-api-from-a-desktop-app-using-azure-active-directory-b2c"></a>Tutorial: Gewähren des Zugriffs auf eine Node.js-Web-API über eine Desktop-App unter Verwendung von Azure Active Directory B2C

In diesem Tutorial erfahren Sie, wie Sie eine durch Azure Active Directory (Azure AD) B2C geschützte Node.js-Web-API-Ressource über eine WPF-Desktop-App (Windows Presentation Foundation) aufrufen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Registrieren einer Web-API bei Ihrem Azure AD B2C-Mandanten
> * Definieren und Konfigurieren von Bereichen für eine Web-API
> * Gewähren von App-Berechtigungen für die Web-API
> * Aktualisieren des Beispielcodes zur Verwendung von Azure AD B2C für den Schutz einer Web-API

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

* Schließen Sie das Tutorial [Aktivieren der Authentifizierung von Desktop-Apps mit Konten unter Verwendung von Azure Active Directory B2C](active-directory-b2c-tutorials-desktop-app.md) ab.
* Installieren Sie [Visual Studio 2017](https://www.visualstudio.com/downloads/) mit den Workloads **.NET-Desktopentwicklung** und **ASP.NET und Webentwicklung**.
* Installieren von [Node.js](https://nodejs.org/en/download/)

## <a name="register-web-api"></a>Registrieren der Web-API

Web-API-Ressourcen müssen bei Ihrem Mandanten registriert werden, damit sie [geschützte Ressourcenanforderungen](../active-directory/develop/active-directory-dev-glossary.md#resource-server) von [Clientanwendungen](../active-directory/develop/active-directory-dev-glossary.md#client-application), die über ein [Zugriffstoken](../active-directory/develop/active-directory-dev-glossary.md#access-token) von Azure Active Directory verfügen, akzeptieren und darauf reagieren können. Durch die Registrierung werden in Ihrem Mandanten das [Anwendungs- und Dienstprinzipalobjekt](../active-directory/develop/active-directory-dev-glossary.md#application-object) eingerichtet. 

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als globaler Administrator Ihres Azure AD B2C-Mandanten an.

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

1. Wählen Sie **Azure AD B2C** aus der Dienstliste im Azure-Portal aus.

2. Klicken Sie in den B2C-Einstellungen auf **Anwendungen** und anschließend auf **Hinzufügen**.

    Verwenden Sie die folgenden Einstellungen, um die Beispiel-Web-API bei Ihrem Mandanten zu registrieren.
    
    ![Hinzufügen einer neuen API](media/active-directory-b2c-tutorials-desktop-app-webapi/web-api-registration.png)
    
    | Einstellung      | Empfohlener Wert  | Beschreibung                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Name** | My sample Node.js web API | Geben Sie einen aussagekräftigen**Namen** für Ihre Web-API ein. |
    | **Web-App/Web-API einschließen** | Ja | Wählen Sie für eine Web-API die Option **Ja** aus. |
    | **Impliziten Fluss zulassen** | Ja | Diese API verwendet die [OpenID Connect-Anmeldung](active-directory-b2c-reference-oidc.md). Wählen Sie daher **Ja** aus. |
    | **Antwort-URL** | `http://localhost:5000` | Antwort-URLs sind Endpunkte, an denen Azure AD B2C von Ihrer API angeforderte Token zurückgibt. Die Beispiel-Web-API in diesem Tutorial wird lokal (localhost) ausgeführt und lauscht am Port 5000. |
    | **App-ID-URI** | demoapi | Durch den URI wird die API eindeutig im Mandanten identifiziert. Dadurch können pro Mandant mehrere APIs registriert werden. [Bereiche](../active-directory/develop/active-directory-dev-glossary.md#scopes) steuern den Zugriff auf die geschützte API-Ressource und werden auf der Grundlage des APP-ID-URI definiert. |
    | **Nativer Client** | Nein  | Da es sich hierbei um eine Web-API und nicht um einen nativen Client handelt, wählen Sie „Nein“ aus. |
    
3. Klicken Sie auf **Erstellen**, um Ihre API zu registrieren.

Registrierte APIs werden in der Anwendungsliste für den Azure AD B2C-Mandanten angezeigt. Wählen Sie Ihre Web-API aus der Liste aus. Der Eigenschaftenbereich der Web-API wird angezeigt.

![Web-API-Eigenschaften](./media/active-directory-b2c-tutorials-web-api/b2c-web-api-properties.png)

Notieren Sie sich den Wert der **Anwendungsclient-ID**. Die ID identifiziert die API eindeutig und wird im weiteren Verlauf des Tutorials beim Konfigurieren der API benötigt.

Durch die Registrierung der Web-API bei Azure AD B2C wird eine Vertrauensstellung definiert. Nachdem die API bei B2C registriert wurde, kann sie den B2C-Zugriffstoken vertrauen, die sie von anderen Anwendungen erhält.

## <a name="define-and-configure-scopes"></a>Definieren und Konfigurieren von Bereichen

[Bereiche](../active-directory/develop/active-directory-dev-glossary.md#scopes) ermöglichen die Steuerung des Zugriffs auf geschützte Ressourcen. Bereiche werden von der Web-API verwendet, um eine bereichsbasierte Zugriffssteuerung zu implementieren. So können beispielsweise einige Benutzer Lese- und Schreibzugriff haben, während andere Benutzer nur über Leseberechtigungen verfügen. In diesem Tutorial definieren Sie Lese- und Schreibberechtigungen für die Web-API.

### <a name="define-scopes-for-the-web-api"></a>Definieren von Bereichen für die Web-API

Registrierte APIs werden in der Anwendungsliste für den Azure AD B2C-Mandanten angezeigt. Wählen Sie Ihre Web-API aus der Liste aus. Der Eigenschaftenbereich der Web-API wird angezeigt.

Klicken Sie auf **Veröffentlichte Bereiche (Vorschau)**.

Fügen Sie folgende Einträge hinzu, um Bereiche für die API zu konfigurieren: 

![In der Web-API definierte Bereiche](media/active-directory-b2c-tutorials-web-api/scopes-defined-in-web-api.png)

| Einstellung      | Empfohlener Wert  | BESCHREIBUNG                                        |
| ------------ | ------- | -------------------------------------------------- |
| **Umfang** | demo.read | Lesezugriff auf die Demo-API|

Klicken Sie auf **Speichern**.

Mit den veröffentlichten Bereichen können einer Client-App Berechtigungen für die Web-API gewährt werden.

### <a name="grant-app-permissions-to-web-api"></a>Erteilen von Web-API-Berechtigungen für die App

Wenn Sie über eine App eine geschützte Web-API aufrufen möchten, müssen Sie der App Berechtigungen für die API erteilen. Verwenden Sie in diesem Tutorial die Desktop-App, die Sie im Tutorial [Aktivieren der Authentifizierung von Desktop-Apps mit Konten unter Verwendung von Azure Active Directory B2C](active-directory-b2c-tutorials-desktop-app.md) erstellt haben.

1. Wählen Sie in der Dienstliste des Azure-Portals die Option **Azure AD B2C** aus, und klicken Sie auf **Anwendungen**, um die Liste mit den registrierten Apps anzuzeigen.

2. Wählen Sie in der App-Liste die App **My Sample WPF App** aus, und klicken Sie auf **API-Zugriff (Vorschau)** und anschließend auf **Hinzufügen**.

3. Wählen Sie in der Dropdownliste **API auswählen** Ihre registrierte Web-API **My sample Node.js web API** aus.

4. Wählen Sie in der Dropdownliste **Bereiche auswählen** die Bereiche aus, die Sie in der Web-API-Registrierung definiert haben.

    ![Auswählen der Bereiche für die App](media/active-directory-b2c-tutorials-web-api/selecting-scopes-for-app.png)

5. Klicken Sie auf **OK**.

**My Sample WPF App** wird zum Aufrufen der geschützten API **My sample Node.js web API** registriert. Ein Benutzer [authentifiziert](../active-directory/develop/active-directory-dev-glossary.md#authentication) sich mit Azure AD B2C, um die WPF-Desktop-App zu verwenden. Die Desktop-App bezieht eine [Autorisierungsgewährung](../active-directory/develop/active-directory-dev-glossary.md#authorization-grant) von Azure AD B2C, um auf die geschützte Web-API zuzugreifen.

## <a name="update-web-api-code"></a>Aktualisieren des Web-API-Codes

Nachdem die Web-API registriert ist und Bereiche definiert wurden, müssen Sie den Web-API-Code für die Verwendung Ihres Azure AD B2C-Mandanten konfigurieren. In diesem Tutorial konfigurieren Sie ein Beispiel für eine Node.js-Web-App, die Sie von GitHub herunterladen können. 

[Laden Sie eine ZIP-Datei herunter](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi/archive/master.zip), oder klonen Sie die Beispiel-Web-App aus GitHub.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi.git
```
Das Node.js-Web-API-Beispiel verwendet die Bibliothek „Passport.js“, um Azure AD B2C zum Schützen von API-Aufrufen zu aktivieren. 

### <a name="configure-the-web-api"></a>Konfigurieren der Web-API

1. Öffnen Sie im Node.js-Web-API-Beispiel die Datei `index.html`.
2. Konfigurieren Sie das Beispiel mit den Registrierungsinformationen für den Azure AD B2C-Mandanten. Ändern Sie die folgenden Codezeilen:

```nodejs
var tenantID = "<your-tenant-name>.onmicrosoft.com";
var clientID = "<Application ID for your Node.js Web API>";
var policyName = "B2C_1_SiUpIn";  // Sign-in / sign-up policy name
```

### <a name="configure-the-desktop-app"></a>Konfigurieren der Desktop-App

1. Öffnen Sie die Projektmappe `active-directory-b2c-wpf` aus [Aktivieren der Authentifizierung von Desktop-Apps mit Konten unter Verwendung von Azure Active Directory B2C](active-directory-b2c-tutorials-desktop-app.md) in Visual Studio.

## <a name="run-the-sample"></a>Ausführen des Beispiels

Führen Sie die Node.js-Web-API aus:

1. Starten Sie eine Node.js-Eingabeaufforderung.
2. Wechseln Sie zum Verzeichnis mit dem Node.js-Beispiel. Beispiel: `cd c:\active-directory-b2c-javascript-nodejs-webapi`
3. Führen Sie die folgenden Befehle aus:
    ```
    npm install && npm update
    ```
    ```
    node index.js
    ```
Führen Sie die Desktop-App aus:

1. Drücken Sie **F5**, um die Desktop-App auszuführen.
2. Melden Sie sich mit der E-Mail-Adresse und dem Kennwort an, die Sie im Tutorial [Aktivieren der Authentifizierung von Desktop-Apps mit Konten unter Verwendung von Azure Active Directory B2C](active-directory-b2c-tutorials-desktop-app.md) verwendet haben.
3. Klicken Sie auf die Schaltfläche **API aufrufen**. 

Die Desktop-App sendet eine Anforderung an die Web-API und erhält eine Antwort mit dem Anzeigenamen des angemeldeten Benutzers. Ihre geschützte Desktop-App ruft die geschützte Web-API in Ihrem Azure AD B2C-Mandanten auf.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Sie können Ihren Azure AD B2C-Mandanten für weitere Azure AD B2C-Tutorials verwenden. Wenn Sie ihn nicht mehr benötigt, können Sie [Ihren Azure AD B2C-Mandanten löschen](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurde erläutert, wie Sie eine ASP.NET-Web-API durch Registrieren und Definieren von Bereichen in Azure AD B2C schützen. Wenn Sie weitere Informationen benötigen, sehen Sie sich die verfügbaren Azure AD B2C-Codebeispiele an.

> [!div class="nextstepaction"]
> [Azure-Codebeispiele](https://azure.microsoft.com/resources/samples/?service=active-directory-b2c&sort=0)
