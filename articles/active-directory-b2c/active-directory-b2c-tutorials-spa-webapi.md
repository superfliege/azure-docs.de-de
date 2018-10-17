---
title: 'Tutorial: Gewähren des Zugriffs auf eine ASP.NET Core-Web-API über eine einseitige App unter Verwendung von Azure Active Directory B2C | Microsoft-Dokumentation'
description: In diesem Tutorial erfahren Sie, wie Sie mit Active Directory B2C eine .NET Core-Web-API schützen und sie über eine einseitige App aufrufen.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.author: davidmu
ms.date: 3/02/2018
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.component: B2C
ms.openlocfilehash: efe975fa4f89a262faef82df3cc79820d393b60e
ms.sourcegitcommit: 616e63d6258f036a2863acd96b73770e35ff54f8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/14/2018
ms.locfileid: "45605758"
---
# <a name="tutorial-grant-access-to-an-aspnet-core-web-api-from-a-single-page-app-using-azure-active-directory-b2c"></a>Tutorial: Gewähren des Zugriffs auf eine ASP.NET Core-Web-API über eine einseitige App mithilfe von Azure Active Directory B2C

In diesem Tutorial erfahren Sie, wie Sie eine durch Azure Active Directory (Azure AD) B2C geschützte ASP.NET Core-Web-API-Ressource über eine einseitige App aufrufen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Registrieren einer Web-API bei Ihrem Azure AD B2C-Mandanten
> * Definieren und Konfigurieren von Bereichen für eine Web-API
> * Gewähren von App-Berechtigungen für die Web-API
> * Aktualisieren des Beispielcodes zur Verwendung von Azure AD B2C für den Schutz einer Web-API

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

* Schließen Sie das Tutorial [Aktivieren der Authentifizierung einseitiger Apps mit Konten unter Verwendung von Azure Active Directory B2C](active-directory-b2c-tutorials-spa.md) ab.
* Installieren Sie [Visual Studio 2017](https://www.visualstudio.com/downloads/) mit der Workload **ASP.NET und Webentwicklung**.
* [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) oder eine höhere Version
* Installieren von [Node.js](https://nodejs.org/en/download/)

## <a name="register-web-api"></a>Registrieren der Web-API

Web-API-Ressourcen müssen bei Ihrem Mandanten registriert werden, damit sie [geschützte Ressourcenanforderungen](../active-directory/develop/developer-glossary.md#resource-server) von [Clientanwendungen](../active-directory/develop/developer-glossary.md#client-application), die über ein [Zugriffstoken](../active-directory/develop/developer-glossary.md#access-token) von Azure Active Directory verfügen, akzeptieren und darauf reagieren können. Durch die Registrierung werden in Ihrem Mandanten das [Anwendungs- und Dienstprinzipalobjekt](../active-directory/develop/developer-glossary.md#application-object) eingerichtet. 

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als globaler Administrator Ihres Azure AD B2C-Mandanten an.

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

1. Klicken Sie links oben im Azure-Portal auf **Alle Dienste**, suchen Sie nach **Azure AD B2C**, und klicken Sie darauf. Sie sollten nun den Mandanten verwenden, den Sie im vorherigen Tutorial erstellt haben.

2. Wählen Sie **Anwendungen** und dann **Hinzufügen** aus.

    Verwenden Sie die folgenden Einstellungen, um die Beispiel-Web-API bei Ihrem Mandanten zu registrieren.
    
    ![Hinzufügen einer neuen API](media/active-directory-b2c-tutorials-spa-webapi/web-api-registration.png)
    
    | Einstellung      | Empfohlener Wert  | Beschreibung                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Name** | Hello Core API | Geben Sie einen aussagekräftigen**Namen** für Ihre Web-API ein. |
    | **Web-App/Web-API einschließen** | JA | Wählen Sie für eine Web-API die Option **Ja** aus. |
    | **Impliziten Fluss zulassen** | JA | Diese API verwendet die [OpenID Connect-Anmeldung](active-directory-b2c-reference-oidc.md). Wählen Sie daher **Ja** aus. |
    | **Antwort-URL** | `http://localhost:5000` | Antwort-URLs sind Endpunkte, an denen Azure AD B2C von Ihrer API angeforderte Token zurückgibt. Die Beispiel-Web-API in diesem Tutorial wird lokal (Localhost) ausgeführt und lauscht am Port 5000 (sobald dies weiter unten in diesem Tutorial konfiguriert ist). |
    | **App-ID-URI** | HelloCoreAPI | Durch den URI wird die API eindeutig im Mandanten identifiziert. Dadurch können pro Mandant mehrere APIs registriert werden. [Bereiche](../active-directory/develop/developer-glossary.md#scopes) steuern den Zugriff auf die geschützte API-Ressource und werden auf der Grundlage des APP-ID-URI definiert. |
    | **Nativer Client** | Nein  | Da es sich hierbei um eine Web-API und nicht um einen nativen Client handelt, wählen Sie „Nein“ aus. |
    
3. Klicken Sie auf **Erstellen**, um Ihre API zu registrieren.

Registrierte APIs werden in der Anwendungsliste für den Azure AD B2C-Mandanten angezeigt. Wählen Sie Ihre Web-API aus der Liste aus. Der Eigenschaftenbereich der Web-API wird angezeigt.

![Web-API-Eigenschaften](./media/active-directory-b2c-tutorials-spa-webapi/b2c-web-api-properties.png)

Notieren Sie sich den Wert der **Anwendungsclient-ID**. Die ID identifiziert die API eindeutig und wird im weiteren Verlauf des Tutorials beim Konfigurieren der API benötigt.

Durch die Registrierung der Web-API bei Azure AD B2C wird eine Vertrauensstellung definiert. Nachdem die API bei B2C registriert wurde, kann sie den B2C-Zugriffstoken vertrauen, die sie von anderen Anwendungen erhält.

## <a name="define-and-configure-scopes"></a>Definieren und Konfigurieren von Bereichen

[Bereiche](../active-directory/develop/developer-glossary.md#scopes) ermöglichen die Steuerung des Zugriffs auf geschützte Ressourcen. Bereiche werden von der Web-API verwendet, um eine bereichsbasierte Zugriffssteuerung zu implementieren. So können beispielsweise einige Benutzer Lese- und Schreibzugriff haben, während andere Benutzer nur über Leseberechtigungen verfügen. In diesem Tutorial definieren Sie Leseberechtigungen für die Web-API.

### <a name="define-scopes-for-the-web-api"></a>Definieren von Bereichen für die Web-API

Registrierte APIs werden in der Anwendungsliste für den Azure AD B2C-Mandanten angezeigt. Wählen Sie Ihre Web-API aus der Liste aus. Der Eigenschaftenbereich der Web-API wird angezeigt.

Klicken Sie auf **Veröffentlichte Bereiche (Vorschau)**.

Fügen Sie folgende Einträge hinzu, um Bereiche für die API zu konfigurieren: 

![In der Web-API definierte Bereiche](media/active-directory-b2c-tutorials-spa-webapi/scopes-web-api.png)

| Einstellung      | Empfohlener Wert  | BESCHREIBUNG                                        |
| ------------ | ------- | -------------------------------------------------- |
| **Umfang** | demo.read | Lesezugriff auf die Demo-API |

Klicken Sie auf **Speichern**.

Mit den veröffentlichten Bereichen können einer Client-App Berechtigungen für die Web-API gewährt werden.

### <a name="grant-app-permissions-to-web-api"></a>Erteilen von Web-API-Berechtigungen für die App

Wenn Sie über eine App eine geschützte Web-API aufrufen möchten, müssen Sie der App Berechtigungen für die API erteilen. Verwenden Sie in diesem Tutorial die einseitige App, die Sie im Tutorial [Aktivieren der Authentifizierung einseitiger Apps mit Konten unter Verwendung von Azure Active Directory B2C](active-directory-b2c-tutorials-spa.md) erstellt haben.

1. Wählen Sie in der Dienstliste des Azure-Portals die Option **Azure AD B2C** aus, und klicken Sie auf **Anwendungen**, um die Liste mit den registrierten Apps anzuzeigen.

2. Wählen Sie in der App-Liste die App **My sample single page app** aus, und klicken Sie auf **API-Zugriff (Vorschau)** und anschließend auf **Hinzufügen**.

3. Wählen Sie in der Dropdownliste **API auswählen** Ihre registrierte Web-API **Hello Core API** aus.

4. Wählen Sie in der Dropdownliste **Bereiche auswählen** die Bereiche aus, die Sie in der Web-API-Registrierung definiert haben.

    ![Auswählen der Bereiche für die App](media/active-directory-b2c-tutorials-spa-webapi/selecting-scopes-for-app.png)

5. Klicken Sie auf **OK**.

**My sample single page app** wird zum Aufrufen der geschützten API **Hello Core API** registriert. Ein Benutzer [authentifiziert](../active-directory/develop/developer-glossary.md#authentication) sich mit Azure AD B2C, um die Single-Page-Webanwendung zu verwenden. Die Single-Page-Webanwendung holt eine [Autorisierungsgewährung](../active-directory/develop/developer-glossary.md#authorization-grant) von Azure AD B2C ein, um auf die geschützte Web-API zuzugreifen.

## <a name="update-code"></a>Aktualisieren des Codes

Nachdem die Web-API registriert ist und Bereiche definiert wurden, müssen Sie den Web-API-Code für die Verwendung Ihres Azure AD B2C-Mandanten konfigurieren. In diesem Tutorial konfigurieren Sie ein Beispiel für eine .NET Core-Web-App, die Sie von GitHub herunterladen können. 

[Laden Sie eine ZIP-Datei herunter](https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapi/archive/master.zip), oder klonen Sie die Beispiel-Web-App aus GitHub.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapi.git
```

### <a name="configure-the-web-api"></a>Konfigurieren der Web-API

1. Öffnen Sie die Projektmappe **B2C-WebAPI.sln** in Visual Studio.

2. Öffnen Sie die Datei **appsettings.json**. Aktualisieren Sie die folgenden Werte, um die Web-API zur Verwendung Ihres Mandanten zu konfigurieren:

    ```javascript
    "AzureAdB2C": 
      {
        "Tenant": "<your tenant name>.onmicrosoft.com", 
        "ClientId": "<The Application ID for your web API obtained from the Azure portal>",
        "Policy": "<Your sign up sign in policy e.g. B2C_1_SiUpIn>",
        "ScopeRead": "demo.read"  
      },
    ```

#### <a name="enable-cors"></a>Aktivieren von CORS

Damit Ihre einseitige App die ASP.NET Core-Web-API aufrufen kann, müssen Sie [CORS](https://docs.microsoft.com/aspnet/core/security/cors) aktivieren.

1. Fügen Sie in **Startup.cs** CORS zur `ConfigureServices()`-Methode hinzu.

    ```C#
    public void ConfigureServices(IServiceCollection services) {
      services.AddCors();
    ```

2. Konfigurieren Sie in **Startup.cs** CORS in der `Configure()`-Methode.

    ```C#
    public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory) {
      app.UseCors(builder =>
        builder.WithOrigins("http://localhost:6420").AllowAnyHeader().AllowAnyMethod());
    ```

3. Öffnen Sie die Datei **launchSettings.json** unter **Eigenschaften**, suchen Sie nach der Einstellung **iisSettings** *applicationURL*, und legen Sie die Portnummer auf die Nummer fest, die für die API-Antwort-URL `http://localhost:5000` registriert ist.

### <a name="configure-the-single-page-app"></a>Konfigurieren der einseitigen App

Die einseitige App verwendet Azure AD B2C für die Benutzerregistrierung und -anmeldung und ruft die geschützte ASP.NET Core-Web-API auf. Sie müssen die einseitige App aktualisieren, sodass sie die .NET Core-Web-API aufruft.
Gehen Sie zum Ändern der App-Einstellungen wie folgt vor:

1. Öffnen Sie die Datei `index.html` im Beispiel für die einseitige Node.js-App.
2. Konfigurieren Sie das Beispiel mit den Registrierungsinformationen für den Azure AD B2C-Mandanten. Fügen Sie im folgenden Code Ihren Mandantennamen zu **b2cScopes** hinzu, und ändern Sie den **webApi**-Wert in den zuvor verzeichneten *applicationURL*-Wert:

    ```javascript
    // The current application coordinates were pre-registered in a B2C tenant.
    var applicationConfig = {
        clientID: '<Application ID for your SPA obtained from portal app registration>',
        authority: "https://<your-tenant-name>.b2clogin.com/tfp/<your-tenant-name>.onmicrosoft.com/B2C_1_SiUpIn",
        b2cScopes: ["https://<Your tenant name>.onmicrosoft.com/HelloCoreAPI/demo.read"],
        webApi: 'http://localhost:5000/api/values',
    };
    ```

## <a name="run-the-spa-app-and-web-api"></a>Ausführen der SPA-App und der Web-API

Sie müssen die einseitige Node.js-App und die .NET Core-Web-API ausführen.

### <a name="run-the-aspnet-core-web-api"></a>Ausführen der ASP.NET Core-Web-API 

Drücken Sie **F5**, um die Projektmappe **B2C-WebAPI.sln** in Visual Studio zu debuggen.

Beim Starten des Projekts wird in Ihrem Standardbrowser eine Webseite mit dem Hinweis angezeigt, dass die Web-API für Anforderungen zur Verfügung steht.

### <a name="run-the-single-page-app"></a>Ausführen der einseitigen App

1. Starten Sie eine Node.js-Eingabeaufforderung.
2. Wechseln Sie zum Verzeichnis mit dem Node.js-Beispiel. Beispiel: `cd c:\active-directory-b2c-javascript-msal-singlepageapp`
3. Führen Sie die folgenden Befehle aus:
    ```
    npm install && npm update
    node server.js
    ```

    Im Konsolenfenster wird die Portnummer angezeigt, unter der die App gehostet wird.
    
    ```
    Listening on port 6420...
    ```

4. Navigieren Sie in einem Browser zur Adresse `http://localhost:6420`, um die App anzuzeigen.
5. Melden Sie sich mit der E-Mail-Adresse und dem Kennwort an, die Sie im Tutorial [Aktivieren der Authentifizierung einseitiger Apps mit Konten unter Verwendung von Azure Active Directory B2C](active-directory-b2c-tutorials-spa.md) verwendet haben.
6. Klicken Sie auf die Schaltfläche **API aufrufen**.

Nach dem Registrieren oder Anmelden mit einem Benutzerkonto ruft das Beispiel die geschützte Web-API auf und gibt ein Ergebnis zurück.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Sie können Ihren Azure AD B2C-Mandanten für weitere Azure AD B2C-Tutorials verwenden. Wenn Sie ihn nicht mehr benötigt, können Sie [Ihren Azure AD B2C-Mandanten löschen](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurde erläutert, wie Sie eine Web-API durch Registrieren und Definieren von Bereichen in Azure AD B2C schützen. Wenn Sie weitere Informationen benötigen, sehen Sie sich die verfügbaren Azure AD B2C-Codebeispiele an.

> [!div class="nextstepaction"]
> [Azure-Codebeispiele](https://azure.microsoft.com/resources/samples/?service=active-directory-b2c&sort=0)
