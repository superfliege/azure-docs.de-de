---
title: 'Tutorial: Gewähren des Zugriffs auf eine ASP.NET-Web-API über eine Web-App unter Verwendung von Azure Active Directory B2C | Microsoft-Dokumentation'
description: In diesem Tutorial erfahren Sie, wie Sie mit Active Directory B2C eine ASP.NET-Web-API schützen und sie über eine ASP.NET-Web-App aufrufen.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.author: davidmu
ms.date: 01/23/2018
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.component: B2C
ms.openlocfilehash: 469a3662b5bc4db467dde3285d557ac8bbae368e
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2018
ms.locfileid: "39609088"
---
# <a name="tutorial-grant-access-to-an-aspnet-web-api-from-a-web-app-using-azure-active-directory-b2c"></a>Tutorial: Gewähren des Zugriffs auf eine ASP.NET-Web-API über eine Web-App unter Verwendung von Azure Active Directory B2C

In diesem Tutorial erfahren Sie, wie Sie eine durch Azure Active Directory (Azure AD) B2C geschützte Web-API-Ressource über eine ASP.NET-Web-App aufrufen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Registrieren einer Web-API bei Ihrem Azure AD B2C-Mandanten
> * Definieren und Konfigurieren von Bereichen für eine Web-API
> * Gewähren von App-Berechtigungen für die Web-API
> * Aktualisieren des Beispielcodes zur Verwendung von Azure AD B2C für den Schutz einer Web-API

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

* Absolvieren Sie das Tutorial [Verwenden von Azure Active Directory B2C für die Benutzerauthentifizierung in einer ASP.NET-Web-App](active-directory-b2c-tutorials-web-app.md).
* Installieren Sie [Visual Studio 2017](https://www.visualstudio.com/downloads/) mit der Workload **ASP.NET und Webentwicklung**.

## <a name="register-web-api"></a>Registrieren der Web-API

Web-API-Ressourcen müssen bei Ihrem Mandanten registriert werden, damit sie [geschützte Ressourcenanforderungen](../active-directory/develop/developer-glossary.md#resource-server) von [Clientanwendungen](../active-directory/develop/developer-glossary.md#client-application), die über ein [Zugriffstoken](../active-directory/develop/developer-glossary.md#access-token) von Azure Active Directory verfügen, akzeptieren und darauf reagieren können. Durch die Registrierung werden in Ihrem Mandanten das [Anwendungs- und Dienstprinzipalobjekt](../active-directory/develop/developer-glossary.md#application-object) eingerichtet. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als globaler Administrator Ihres Azure AD B2C-Mandanten an.

2. Vergewissern Sie sich, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält, indem Sie oben rechts im Azure-Portal zu diesem Verzeichnis wechseln. Wählen Sie die Abonnementinformationen aus, und klicken Sie dann auf **Verzeichnis wechseln**.

    ![Wechseln von Verzeichnissen](./media/active-directory-b2c-tutorials-web-api/switch-directories.png)

3. Wählen Sie das Verzeichnis aus, das den Mandanten enthält.

    ![Auswählen des Verzeichnisses](./media/active-directory-b2c-tutorials-web-api/select-directory.png)

4. Klicken Sie links oben im Azure-Portal auf **Alle Dienste**, suchen Sie nach **Azure AD B2C**, und klicken Sie darauf. Sie sollten nun den Mandanten verwenden, den Sie im vorherigen Tutorial erstellt haben.

5. Wählen Sie **Anwendungen** und dann **Hinzufügen** aus.

    Verwenden Sie die folgenden Einstellungen, um die Beispiel-Web-API bei Ihrem Mandanten zu registrieren.
    
    ![Hinzufügen einer neuen API](./media/active-directory-b2c-tutorials-web-api/web-api-registration.png)
    
    | Einstellung      | Empfohlener Wert  | Beschreibung                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Name** | My Sample Web API | Geben Sie einen aussagekräftigen**Namen** für Ihre Web-API ein. |
    | **Web-App/Web-API einschließen** | JA | Wählen Sie für eine Web-API die Option **Ja** aus. |
    | **Impliziten Fluss zulassen** | JA | Diese API verwendet die [OpenID Connect-Anmeldung](active-directory-b2c-reference-oidc.md). Wählen Sie daher **Ja** aus. |
    | **Antwort-URL** | `https://localhost:44332` | Antwort-URLs sind Endpunkte, an denen Azure AD B2C von Ihrer API angeforderte Token zurückgibt. Die Beispiel-Web-API in diesem Tutorial wird lokal (localhost) ausgeführt und lauscht am Port 44332. |
    | **App-ID-URI** | myAPISample | Durch den URI wird die API eindeutig im Mandanten identifiziert. Dadurch können pro Mandant mehrere APIs registriert werden. [Bereiche](../active-directory/develop/developer-glossary.md#scopes) steuern den Zugriff auf die geschützte API-Ressource und werden auf der Grundlage des APP-ID-URI definiert. |
    | **Nativer Client** | Nein  | Da es sich hierbei um eine Web-API und nicht um einen nativen Client handelt, wählen Sie „Nein“ aus. |
    
6. Klicken Sie auf **Erstellen**, um Ihre API zu registrieren.

Registrierte APIs werden in der Anwendungsliste für den Azure AD B2C-Mandanten angezeigt. Wählen Sie Ihre Web-API aus der Liste aus. Der Eigenschaftenbereich der Web-API wird angezeigt.

![Web-API-Eigenschaften](./media/active-directory-b2c-tutorials-web-api/b2c-web-api-properties.png)

Notieren Sie sich den Wert der **Anwendungsclient-ID**. Die ID identifiziert die API eindeutig und wird im weiteren Verlauf des Tutorials beim Konfigurieren der API benötigt.

Durch die Registrierung der Web-API bei Azure AD B2C wird eine Vertrauensstellung definiert. Nachdem die API bei B2C registriert wurde, kann sie den B2C-Zugriffstoken vertrauen, die sie von anderen Anwendungen erhält.

## <a name="define-and-configure-scopes"></a>Definieren und Konfigurieren von Bereichen

[Bereiche](../active-directory/develop/developer-glossary.md#scopes) ermöglichen die Steuerung des Zugriffs auf geschützte Ressourcen. Bereiche werden von der Web-API verwendet, um eine bereichsbasierte Zugriffssteuerung zu implementieren. Benutzer der Web-API können beispielsweise über Lese- und Schreibzugriff oder nur über Lesezugriff verfügen. In diesem Tutorial verwenden Sie Bereiche zum Definieren von Lese- und Schreibberechtigungen für die Web-API.

### <a name="define-scopes-for-the-web-api"></a>Definieren von Bereichen für die Web-API

Registrierte APIs werden in der Anwendungsliste für den Azure AD B2C-Mandanten angezeigt. Wählen Sie Ihre Web-API aus der Liste aus. Der Eigenschaftenbereich der Web-API wird angezeigt.

Klicken Sie auf **Veröffentlichte Bereiche (Vorschau)**.

Fügen Sie folgende Einträge hinzu, um Bereiche für die API zu konfigurieren: 

![In der Web-API definierte Bereiche](media/active-directory-b2c-tutorials-web-api/scopes-defined-in-web-api.png)

| Einstellung      | Empfohlener Wert  | BESCHREIBUNG                                        |
| ------------ | ------- | -------------------------------------------------- |
| **Umfang** | Hello.Read | Lesezugriff auf „hello“ |
| **Umfang** | Hello.Write | Schreibzugriff auf „hello“ |

Klicken Sie auf **Speichern**.

Mit den veröffentlichten Bereichen können einer Client-App Berechtigungen für die Web-API gewährt werden.

### <a name="grant-app-permissions-to-web-api"></a>Erteilen von Web-API-Berechtigungen für die App

Wenn Sie über eine App eine geschützte Web-API aufrufen möchten, müssen Sie der App Berechtigungen für die API erteilen. In diesem Tutorial verwenden Sie die Web-App, die Sie unter [Verwenden von Azure Active Directory B2C für die Benutzerauthentifizierung in einer ASP.NET-Web-App](active-directory-b2c-tutorials-web-app.md) erstellt haben. 

1. Wählen Sie in der Dienstliste des Azure-Portals die Option **Azure AD B2C** aus, und klicken Sie auf **Anwendungen**, um die Liste mit den registrierten Apps anzuzeigen.

2. Wählen Sie in der App-Liste die App **My Sample Web App** aus, und klicken Sie auf **API-Zugriff (Vorschau)** und anschließend auf **Hinzufügen**.

3. Wählen Sie in der Dropdownliste **API auswählen** Ihre registrierte Web-API **My Sample Web API** aus.

4. Wählen Sie in der Dropdownliste **Bereiche auswählen** die Bereiche aus, die Sie in der Web-API-Registrierung definiert haben.

    ![Auswählen der Bereiche für die App](media/active-directory-b2c-tutorials-web-api/selecting-scopes-for-app.png)

5. Klicken Sie auf **OK**.

**My Sample Web App** wird zum Aufrufen der geschützten API **My Sample Web API** registriert. Ein Benutzer [authentifiziert](../active-directory/develop/developer-glossary.md#authentication) sich mit Azure AD B2C, um die Web-App zu verwenden. Die Web-App bezieht eine [Autorisierungsgewährung](../active-directory/develop/developer-glossary.md#authorization-grant) von Azure AD B2C, um auf die geschützte Web-API zuzugreifen.

## <a name="update-code"></a>Aktualisieren des Codes

Nachdem die Web-API registriert ist und Bereiche definiert wurden, müssen Sie den Web-API-Code für die Verwendung Ihres Azure AD B2C-Mandanten konfigurieren. In diesem Tutorial konfigurieren Sie eine Beispiel-Web-API. 

Die Beispiel-Web-API ist Teil des Projekts, das Sie im vorherigen Tutorial ([Verwenden von Azure Active Directory B2C für die Benutzerauthentifizierung in einer ASP.NET-Web-App](active-directory-b2c-tutorials-web-app.md)) heruntergeladen haben. Sollten Sie das vorherige Tutorial noch nicht absolviert haben, holen Sie dies nach, bevor Sie mit dem vorliegenden Tutorial fortfahren.

Die Beispielprojektmappe enthält zwei Projekte:

**Web-App-Beispiel-App (TaskWebApp):** Web-App zum Erstellen und Bearbeiten einer Aufgabenliste. Die Web-App verwendet die **Registrierungs- oder Anmelderichtlinie** für die Registrierung oder Anmeldung von Benutzern mit einer E-Mail-Adresse.

**Web-API-Beispiel-App (TaskService):** Web-API, die die Funktionen zum Erstellen, Lesen, Aktualisieren und Löschen der Aufgabenliste unterstützt. Die Web-API wird durch Azure AD B2C geschützt und von der Web-App aufgerufen.

Die Beispiel-Web-App und die Beispiel-Web-API definieren die Konfigurationswerte als App-Einstellungen in der Datei „Web.config“ des jeweiligen Projekts.

Öffnen Sie die Projektmappe **B2C-WebAPI-DotNet** in Visual Studio.

### <a name="configure-the-web-app"></a>Konfigurieren der Web-App

1. Öffnen Sie **Web.config** im Projekt **TaskWebApp**.

2. Verwenden Sie zum lokalen Ausführen der API die localhost-Einstellung für **api:TaskServiceUrl**. Nehmen Sie in der Datei „Web.config“ folgende Änderungen vor: 

    ```C#
    <add key="api:TaskServiceUrl" value="https://localhost:44332/"/>
    ```

3. Konfigurieren Sie den URI der API. Dieser URI wird von der Web-App für die API-Anforderung verwendet. Konfigurieren Sie außerdem die angeforderten Berechtigungen.

    ```C#
    <add key="api:ApiIdentifier" value="https://<Your tenant name>.onmicrosoft.com/myAPISample/" />
    <add key="api:ReadScope" value="Hello.Read" />
    <add key="api:WriteScope" value="Hello.Write" />
    ```

### <a name="configure-the-web-api"></a>Konfigurieren der Web-API

1. Öffnen Sie **Web.config** im Projekt **TaskService**.

2. Konfigurieren Sie die API für die Verwendung Ihres Mandanten.

    ```C#
    <add key="ida:Tenant" value="<Your tenant name>.onmicrosoft.com" />
    ```

3. Legen Sie die Client-ID auf die ID der registrierten Anwendung für Ihre API fest.

    ```C#
    <add key="ida:ClientId" value="<The Application ID for your web API obtained from the Azure portal>"/>
    ```

4. Aktualisieren Sie die Richtlinieneinstellung mit dem Namen, der bei der Erstellung der Registrierungs- und Anmelderichtlinie generiert wurde.

    ```C#
    <add key="ida:SignUpSignInPolicyId" value="B2C_1_SiUpIn" />
    ```

5. Konfigurieren Sie die Bereichseinstellung so, dass sie dem entspricht, was Sie im Portal erstellt haben.

    ```C#
    <add key="api:ReadScope" value="Hello.Read" />
    <add key="api:WriteScope" value="Hello.Write" />
    ```

## <a name="run-the-sample"></a>Ausführen des Beispiels

Führen Sie sowohl das Projekt **TaskWebApp** als auch das Projekt **TaskService** aus. 

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf Ihre Projektmappe, und wählen Sie **Startprojekte festlegen...** aus. 
2. Aktivieren Sie das Optionsfeld **Mehrere Startprojekte**.
3. Ändern Sie die **Aktion** für beide Projekte in **Start**.
4. Klicken Sie auf „OK“, um die Konfiguration zu speichern.
5. Drücken Sie**F5**, um beide Anwendungen auszuführen. Jede Anwendung wird auf einer eigenen Browserregisterkarte geöffnet. `https://localhost:44316/` ist die Web-App.
    `https://localhost:44332/` ist die Web-API.

6. Klicken Sie auf dem Menübanner der Web-App auf den Link für die Registrierung/Anmeldung, um sich für die Webanwendung zu registrieren. Verwenden Sie das Konto, das Sie im [Web-App-Tutorial](active-directory-b2c-tutorials-web-app.md) erstellt haben. 
7. Klicken Sie nach der Anmeldung auf den Link **Aufgabenliste**, und erstellen Sie ein Aufgabenlistenelement.

Wenn Sie ein Aufgabenlistenelement erstellen, richtet die Web-App eine Anforderung an die Web-API, um das Element zu generieren. Ihre geschützte Web-App ruft die geschützte Web-API in Ihrem Azure AD B2C-Mandanten auf.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Sie können Ihren Azure AD B2C-Mandanten für weitere Azure AD B2C-Tutorials verwenden. Wenn Sie ihn nicht mehr benötigt, können Sie [Ihren Azure AD B2C-Mandanten löschen](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurde erläutert, wie Sie eine ASP.NET-Web-API durch Registrieren und Definieren von Bereichen in Azure AD B2C schützen. Das nächste Tutorial enthält ausführlichere Informationen zur Weiterentwicklung dieses Szenarios sowie exemplarische Vorgehensweisen mit Code.

> [!div class="nextstepaction"]
> [Erstellen Sie mithilfe von Azure Active Directory B2C eine ASP.NET-Web-App mit Registrierung, Anmeldung, Profilbearbeitung und Kennwortzurücksetzung.](active-directory-b2c-devquickstarts-web-dotnet-susi.md)