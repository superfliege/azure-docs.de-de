---
title: 'Schnellstart: Einrichten der Anmeldung für eine Desktop-App mit Azure Active Directory B2C | Microsoft-Dokumentation'
description: Führen Sie eine ASP.NET-Desktopbeispielanwendung aus, bei der die Kontoanmeldung über Azure Active Directory B2C erfolgt.
services: active-directory-b2c
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: quickstart
ms.custom: mvc
ms.date: 2/13/2018
ms.author: davidmu
ms.openlocfilehash: a312169217ac5476a503d5ad5a9126674f999eac
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2018
---
# <a name="quickstart-set-up-sign-in-for-a-desktop-app-using-azure-active-directory-b2c"></a>Schnellstart: Einrichten der Anmeldung für eine Desktop-App mit Azure Active Directory B2C 

Azure Active Directory (Azure AD) B2C ermöglicht die Cloudidentitätsverwaltung zum Schützen Ihrer Anwendung, Ihres Unternehmens und Ihrer Kunden. Mit Azure AD B2C können sich Ihre Apps über offene Standardprotokolle bei Konten für soziale Netzwerke und bei Unternehmenskonten authentifizieren.

In diesem Schnellstart verwenden Sie eine Azure AD B2C-fähige Windows Presentation Foundation-Desktop-App (WPF-Beispiel-App), indem Sie ein soziales Netzwerk als Identitätsanbieter verwenden und eine per Azure AD B2C geschützte Web-API aufrufen.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

* [Visual Studio 2017](https://www.visualstudio.com/downloads/) mit der Workload **ASP.NET und Webentwicklung** 
* Sie besitzen ein Konto bei einem sozialen Netzwerk, d.h. bei Facebook, Google, Microsoft oder Twitter.

## <a name="download-the-sample"></a>Herunterladen des Beispiels

[Laden Sie eine ZIP-Datei herunter](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop/archive/master.zip), oder klonen Sie die Beispiel-Web-App aus GitHub.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop.git
```

## <a name="run-the-app-in-visual-studio"></a>Ausführen der App in Visual Studio

Öffnen Sie in Visual Studio im Projektordner der Beispielanwendung die Projektmappe `active-directory-b2c-wpf.sln`.

Drücken Sie **F5**, um die Anwendung zu debuggen.

## <a name="create-an-account"></a>Erstellen eines Kontos

Klicken Sie auf **Anmelden**, um den Workflow **Registrieren oder anmelden** basierend auf einer Azure AD B2C-Richtlinie zu starten.

![Beispielanwendung](media/active-directory-b2c-quickstarts-desktop-app/wpf-sample-application.png)

In der Beispielanwendung werden mehrere Registrierungsoptionen unterstützt, z.B. die Nutzung eines sozialen Netzwerks als Identitätsanbieter oder die Erstellung eines lokalen Kontos per E-Mail-Adresse. Verwenden Sie für diese Schnellstartanleitung ein Konto bei einem sozialen Netzwerk als Identitätsanbieter (etwa Facebook, Google, Microsoft oder Twitter). 

### <a name="sign-up-using-a-social-identity-provider"></a>Registrieren mit einem sozialen Netzwerk als Identitätsanbieter

Azure AD B2C zeigt für die Beispiel-Web-App eine benutzerdefinierte Anmeldeseite für die fiktive Marke „Wingtip Toys“ an. 

1. Um sich mit einem Social Media-Konto als Identitätsanbieter zu registrieren, klicken Sie auf die Schaltfläche des Identitätsanbieters, den Sie verwenden möchten. 

    ![Anbieter für Registrierung oder Anmeldung](media/active-directory-b2c-quickstarts-desktop-app/sign-in-or-sign-up-wpf.png)

    Sie authentifizieren sich mit den Anmeldeinformationen Ihres Social Media-Kontos und autorisieren die Anwendung dazu, Informationen von Ihrem Social Media-Konto zu lesen. Wenn Sie der Anwendung Zugriff auf diese gewähren, kann die Anwendung Profilinformationen aus dem Social Media-Konto abrufen, z.B. Ihren Namen und Ihre Stadt. 

2. Schließen Sie den Anmeldevorgang für den Identitätsanbieter ab. Beispiel: Wenn Sie Twitter ausgewählt haben, geben Sie Ihre Twitter-Anmeldeinformationen ein, und klicken Sie auf **Anmelden**.

    ![Authentifizierung und Autorisierung mit einem Social Media-Konto](media/active-directory-b2c-quickstarts-desktop-app/twitter-authenticate-authorize-wpf.png)

    Die Profildetails für Ihr neues Konto werden mit Informationen aus Ihrem Social Media-Konto aufgefüllt. 

3. Passen Sie die Informationen bei Bedarf an, und klicken Sie auf **Fortfahren**. Die von Ihnen eingegebenen Werte werden für das Profil Ihres Azure AD B2C-Benutzerkontos verwendet.

    ![Profildetails für das neue Konto beim Registrieren](media/active-directory-b2c-quickstarts-desktop-app/new-account-sign-up-profile-details-wpf.png)

    Sie haben erfolgreich ein neues Azure AD B2C-Benutzerkonto über einen Identitätsanbieter erstellt. Nach der Anmeldung wird das Zugriffstoken im Textfeld *Tokeninformationen* angezeigt. Das Zugriffstoken wird für den Zugriff auf die API-Ressource verwendet.

## <a name="edit-your-profile"></a>Bearbeiten des Profils

Azure Active Directory B2C bietet Funktionen, mit denen Benutzer ihre Profile aktualisieren können.  Die Beispiel-Web-App nutzt eine Azure AD B2C-Bearbeitungsprofilrichtlinie für den Workflow. 

1. Klicken Sie auf **Profil bearbeiten**, um das erstellte Profil zu bearbeiten.

    ![Bearbeiten eines Profils](media/active-directory-b2c-quickstarts-desktop-app/edit-profile-wpf.png)

2. Wählen Sie den Identitätsanbieter aus, der dem erstellten Konto zugeordnet ist. Wenn Sie beim Erstellen Ihres Kontos z.B. Twitter als Identitätsanbieter verwendet haben, wählen Sie Twitter aus, um die Details des verknüpften Profils zu ändern.

3. Ändern Sie Ihren **Anzeigenamen** oder den **Ort**, und klicken Sie auf **Weiter**.

    Im Textfeld *Tokeninformationen* wird ein neues Zugriffstoken angezeigt. Wenn Sie die Änderungen an Ihrem Profil überprüfen möchten, kopieren Sie das Zugriffstoken und fügen es in den Tokendecoder https://jwt.ms ein.

## <a name="access-a-protected-web-api-resource"></a>Zugreifen auf eine geschützte Web-API-Ressource

Klicken Sie auf **Call API** (API aufrufen), um eine Anforderung an die per Azure AD B2C geschützte Ressource https://fabrikamb2chello.azurewebsites.net/hello zu übermitteln. 

![API aufrufen](media/active-directory-b2c-quickstarts-desktop-app/call-api-wpf.png)

Die Anwendung enthält das Azure AD-Zugriffstoken in der Anforderung, die an die geschützte Web-API-Ressource gesendet wird. Die Web-API sendet den Anzeigenamen zurück, der im Zugriffstoken enthalten ist.

Sie haben Ihr Azure AD B2C-Benutzerkonto verwendet, um einen autorisierten Aufruf einer geschützten Azure AD B2C-Web-API durchzuführen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Sie können Ihren Azure AD B2C-Mandanten für weitere Azure AD B2C-Schnellstarts oder -Tutorials verwenden. Wenn Sie ihn nicht mehr benötigt, können Sie [Ihren Azure AD B2C-Mandanten löschen](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Nächste Schritte

Der nächste Schritt besteht darin, einen eigenen Azure AD B2C-Mandanten zu erstellen und das Beispiel für die Ausführung über Ihren Mandanten zu konfigurieren. 

> [!div class="nextstepaction"]
> [Erstellen eines Azure Active Directory B2C-Mandanten im Azure-Portal](active-directory-b2c-get-started.md)
