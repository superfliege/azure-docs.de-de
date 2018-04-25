---
title: 'Schnellstart: Einrichten der Anmeldung für eine ASP.NET-Anwendung mit Azure Active Directory B2C | Microsoft-Dokumentation'
description: Führen Sie eine Beispiel-ASP.NET-Web-App aus, bei der die Kontoanmeldung über Azure Active Directory B2C erfolgt
services: active-directory-b2c
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.topic: quickstart
ms.custom: mvc
ms.date: 2/13/2018
ms.author: davidmu
ms.openlocfilehash: 4342e8c58c9bb20580d8428a6c9869f9a3b893cb
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2018
---
# <a name="quickstart-set-up-sign-in-for-an-aspnet-application-using-azure-active-directory-b2c"></a>Schnellstart: Einrichten der Anmeldung für eine ASP.NET-Anwendung unter Verwendung von Azure Active Directory B2C

Azure Active Directory (Azure AD) B2C ermöglicht die Cloudidentitätsverwaltung zum Schützen Ihrer Anwendung, Ihres Unternehmens und Ihrer Kunden. Mit Azure AD B2C können sich Ihre Apps über offene Standardprotokolle bei Konten für soziale Netzwerke und bei Unternehmenskonten authentifizieren.

In diesem Schnellstart verwenden Sie eine Azure AD B2C-fähige ASP.NET-App, indem Sie ein soziales Netzwerk als Identitätsanbieter verwenden und eine per Azure AD B2C geschützte Web-API aufrufen.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

* [Visual Studio 2017](https://www.visualstudio.com/downloads/) mit der Workload **ASP.NET und Webentwicklung** 
* Sie besitzen ein Konto bei einem sozialen Netzwerk, d.h. bei Facebook, Google, Microsoft oder Twitter.

## <a name="download-the-sample"></a>Herunterladen des Beispiels

[Laden Sie eine ZIP-Datei herunter](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi/archive/master.zip), oder klonen Sie die Beispiel-Web-App aus GitHub.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

## <a name="run-the-app-in-visual-studio"></a>Ausführen der App in Visual Studio

Öffnen Sie in Visual Studio im Projektordner der Beispielanwendung die Projektmappe `B2C-WebAPI-DotNet.sln`.

Die Beispielprojektmappe enthält zwei Projekte:

**Web-App-Beispiel-App (TaskWebApp):** Web-App zum Erstellen und Bearbeiten einer Aufgabenliste. Die Web-App verwendet die **Registrierungs- oder Anmelderichtlinie** für die Registrierung oder Anmeldung von Benutzern.

**Web-API-Beispiel-App (TaskService):** Web-API, die die Funktionen zum Erstellen, Lesen, Aktualisieren und Löschen der Aufgabenliste unterstützt. Die Web-API wird per Azure AD B2C geschützt und von der Web-App aufgerufen.

Für diesen Schnellstart führen Sie die Projekte `TaskWebApp` und `TaskService` gleichzeitig aus. 

1. Wählen Sie im Projektmappen-Explorer die Projektmappe `B2C-WebAPI-DotNet` aus.
2. Wählen Sie im Visual Studio-Menü die Option **Projekt > Startprojekte festlegen...**. 
3. Aktivieren Sie das Optionsfeld **Mehrere Startprojekte**.
4. Ändern Sie die **Aktion** für beide Projekte in **Start**. Klicken Sie auf **OK**.

Drücken Sie **F5**, um beide Anwendungen zu debuggen. Jede Anwendung wird in einer eigenen Browserregisterkarte geöffnet:

`https://localhost:44316/` – Dies ist die Seite der ASP.NET-Webanwendung. Im Rahmen des Schnellstarts interagieren Sie direkt mit dieser Anwendung.
`https://localhost:44332/` – Dies ist die Seite der Web-API, die von der ASP.NET-Webanwendung aufgerufen wird.

## <a name="create-an-account"></a>Erstellen eines Kontos

Klicken Sie in der ASP.NET-Webanwendung auf den Link **Registrieren/Anmelden**, um den Workflow **Registrieren oder anmelden** basierend auf einer Azure AD B2C-Richtlinie zu starten.

![ASP.NET-Web-App – Beispiel](media/active-directory-b2c-quickstarts-web-app/web-app-sign-in.png)

In der Beispielanwendung werden mehrere Registrierungsoptionen unterstützt, z.B. die Nutzung eines sozialen Netzwerks als Identitätsanbieter oder die Erstellung eines lokalen Kontos per E-Mail-Adresse. Verwenden Sie für diese Schnellstartanleitung ein Konto bei einem sozialen Netzwerk als Identitätsanbieter (etwa Facebook, Google, Microsoft oder Twitter). 

### <a name="sign-up-using-a-social-identity-provider"></a>Registrieren mit einem sozialen Netzwerk als Identitätsanbieter

Azure AD B2C zeigt für die Beispiel-Web-App eine benutzerdefinierte Anmeldeseite für die fiktive Marke „Wingtip Toys“ an. 

1. Um sich mit einem Social Media-Konto als Identitätsanbieter zu registrieren, klicken Sie auf die Schaltfläche des Identitätsanbieters, den Sie verwenden möchten.

    ![Anbieter für Registrierung oder Anmeldung](media/active-directory-b2c-quickstarts-web-app/sign-in-or-sign-up-web.png)

    Sie authentifizieren sich mit den Anmeldeinformationen Ihres Social Media-Kontos und autorisieren die Anwendung dazu, Informationen von Ihrem Social Media-Konto zu lesen. Wenn Sie der Anwendung Zugriff auf diese gewähren, kann die Anwendung Profilinformationen aus dem Social Media-Konto abrufen, z.B. Ihren Namen und Ihre Stadt. 

2. Schließen Sie den Anmeldevorgang für den Identitätsanbieter ab. Beispiel: Wenn Sie Twitter ausgewählt haben, geben Sie Ihre Twitter-Anmeldeinformationen ein, und klicken Sie auf **Anmelden**.

    ![Authentifizierung und Autorisierung mit einem Social Media-Konto](media/active-directory-b2c-quickstarts-web-app/twitter-authenticate-authorize-web.png)

    Die Profildetails für Ihr neues Azure AD B2C-Konto werden mit Informationen aus Ihrem Social Media-Konto aufgefüllt.

3. Aktualisieren Sie die Felder „Anzeigename“, „Position“ und „Stadt“, und klicken Sie auf **Fortfahren**.  Die von Ihnen eingegebenen Werte werden für das Profil Ihres Azure AD B2C-Benutzerkontos verwendet.

    ![Profildetails für das neue Konto beim Registrieren](media/active-directory-b2c-quickstarts-web-app/new-account-sign-up-profile-details-web.png)

    Sie haben erfolgreich die Beispiel-Web-App verwendet, für die eine Azure AD B2C-Richtlinie zum Authentifizieren per Identitätsanbieter genutzt wird, und ein Azure AD B2C-Benutzerkonto erstellt. 

## <a name="edit-your-profile"></a>Bearbeiten des Profils

Azure Active Directory B2C bietet Funktionen, mit denen Benutzer ihre Profile aktualisieren können. Die Beispiel-Web-App nutzt eine Azure AD B2C-Bearbeitungsprofilrichtlinie für den Workflow. 

1. Klicken Sie in der Menüleiste der Webanwendung auf Ihren Profilnamen, und wählen Sie **Profil bearbeiten** aus, um das von Ihnen erstellte Profil zu bearbeiten.

    ![Bearbeiten eines Profils](media/active-directory-b2c-quickstarts-web-app/edit-profile-web.png)

2. Ändern Sie Ihren **Anzeigenamen** und Ihre **Stadt**.  
3. Klicken Sie auf **Fortfahren**, um Ihr Profil zu aktualisieren. Der neue Anzeigename wird oben rechts auf der Startseite der Web-App angezeigt.

## <a name="access-a-protected-web-api-resource"></a>Zugreifen auf eine geschützte Web-API-Ressource

1. Klicken Sie auf **Aufgabenliste**, um Ihre Aufgabenlistenelemente einzugeben und zu ändern. 

2. Geben Sie Text in das Textfeld **Neues Element** ein. Klicken Sie auf **Hinzufügen**, um die mit Azure AD B2C geschützte Web-API aufzurufen, die ein Aufgabenlistenelement hinzufügt.

    ![Hinzufügen eines Aufgabenlistenelements](media/active-directory-b2c-quickstarts-web-app/add-todo-item-web.png)

    Die ASP.NET-Webanwendung schließt ein Azure AD-Zugriffstoken in die Anforderung an die geschützte Web-API-Ressource ein, um Vorgänge für die Aufgabenlistenelemente des Benutzers durchzuführen.

Sie haben Ihr Azure AD B2C-Benutzerkonto verwendet, um einen autorisierten Aufruf einer geschützten Azure AD B2C-Web-API durchzuführen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Sie können Ihren Azure AD B2C-Mandanten für weitere Azure AD B2C-Schnellstarts oder -Tutorials verwenden. Wenn Sie ihn nicht mehr benötigt, können Sie [Ihren Azure AD B2C-Mandanten löschen](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie eine Azure AD B2C-fähige ASP.NET-Beispiel-App verwendet, um sich über eine benutzerdefinierte Anmeldeseite anzumelden, sich mit einem sozialen Netzwerk als Identitätsanbieter anzumelden, ein Azure AD B2C-Konto zu erstellen und eine per Azure AD B2C geschützte Web-API aufzurufen. 

Fahren Sie mit dem Tutorial fort, um zu erfahren, wie Sie die ASP.NET-Beispiel-App so konfigurieren, dass Ihr eigener Azure AD B2C-Mandant verwendet wird.

> [!div class="nextstepaction"]
> [Tutorial: Authentifizieren von Benutzern mit Azure Active Directory B2C in einer ASP.NET-Web-App](active-directory-b2c-tutorials-web-app.md)