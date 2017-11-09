---
title: Testen einer Azure AD B2C-Desktopanwendung | Microsoft-Dokumentation
description: "Testen von Registrierung, Anmeldung, Profilbearbeitung und Kennwortrücksetzung bei User Journeys in einer Azure AD B2C-Testumgebung"
services: active-directory-b2c
documentationcenter: .net
author: saraford
manager: krassk
editor: PatAltimore
ms.assetid: 86293627-26fb-4e96-a76b-f263f9a945bd
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 10/31/2017
ms.author: saraford
ms.openlocfilehash: 9653d86dd5635016512bf6e6fafbf7195145ed07
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/01/2017
---
# <a name="test-drive-a-desktop-application-configured-with-azure-ad-b2c"></a>Testen einer mit Azure AD B2C konfigurierten Desktopanwendung

Azure Active Directory B2C ermöglicht die Cloudidentitätsverwaltung, um Ihre Anwendung, Ihr Unternehmen und Ihre Kunden zu schützen.  Dieser Schnellstart veranschaulicht anhand einer Beispiel-WPF-Desktop-App (Windows Presentation Foundation) Folgendes:

* Erstellen eines Kontos bei einem sozialen Netzwerk als Identitätsanbieter oder eines lokalen Kontos mit der E-Mail-Adresse über die Richtlinie **Registrieren oder anmelden** oder Anmelden bei einem solchen Konto 
* **Aufrufen einer API** zum Abrufen Ihres Anzeigenamens aus einer mit Azure AD B2C geschützten Ressource

## <a name="prerequisites"></a>Voraussetzungen

* Installieren Sie [Visual Studio 2017](https://www.visualstudio.com/downloads/) mit den folgenden Workloads:
    - **.NET-Desktopentwicklung**

* Sie besitzen ein Konto bei einem sozialen Netzwerk, d.h. bei Facebook, Google, Microsoft oder Twitter. Falls dies nicht der Fall sein sollte, ist eine gültige E-Mail-Adresse erforderlich.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Herunterladen des Beispiels

[Laden Sie die Beispielanwendung von GitHub herunter, oder klonen Sie sie](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop).

## <a name="run-the-app-in-visual-studio"></a>Ausführen der App in Visual Studio

Öffnen Sie in Visual Studio im Projektordner der Beispielanwendung die Projektmappe `active-directory-b2c-wpf.sln`. 

Wählen Sie **Debuggen > Debuggen starten** aus, um die Anwendung zu erstellen und auszuführen. 

## <a name="create-an-account"></a>Erstellen eines Kontos

Klicken Sie auf **Anmelden**, um den Workflow für das **Registrieren oder Anmelden** zu starten. Bei der Erstellung eines Kontos können Sie ein vorhandenes Konto bei einem sozialen Netzwerk als Identitätsanbieter oder ein E-Mail-Konto verwenden.

![Beispielanwendung](media/active-directory-b2c-quickstarts-desktop-app/wpf-sample-application.png)

### <a name="sign-up-using-a-social-identity-provider"></a>Registrieren mit einem sozialen Netzwerk als Identitätsanbieter

Um sich mit einem Social Media-Konto als Identitätsanbieter zu registrieren, klicken Sie auf die Schaltfläche des Identitätsanbieters, den Sie verwenden möchten. Wenn Sie es vorziehen, eine E-Mail-Adresse zu verwenden, wechseln Sie zum Abschnitt [Registrieren mit einer E-Mail-Adresse](#sign-up-using-an-email-address).

![Anbieter für Registrierung oder Anmeldung](media/active-directory-b2c-quickstarts-desktop-app/sign-in-or-sign-up-wpf.png)

Sie müssen sich mit den Anmeldeinformationen Ihres Social Media-Kontos authentifizieren (anmelden) und die Anwendung dazu autorisieren, Informationen von Ihrem Social Media-Konto zu lesen. Wenn Sie der Anwendung Zugriff auf diese gewähren, kann die Anwendung Profilinformationen aus dem Social Media-Konto abrufen, z.B. Ihren Namen und Ihre Stadt. 

![Authentifizierung und Autorisierung mit einem Social Media-Konto](media/active-directory-b2c-quickstarts-desktop-app/twitter-authenticate-authorize-wpf.png)

Die Profildetails für Ihr neues Konto werden mit Informationen aus Ihrem Social Media-Konto aufgefüllt. Passen Sie die Informationen bei Bedarf an, und klicken Sie auf **Fortfahren**.

![Profildetails für das neue Konto beim Registrieren](media/active-directory-b2c-quickstarts-desktop-app/new-account-sign-up-profile-details-wpf.png)

Sie haben erfolgreich ein neues Azure AD B2C-Benutzerkonto über einen Identitätsanbieter erstellt. Nach der Anmeldung wird das Zugriffstoken im Textfeld *Tokeninformationen* angezeigt. Das Zugriffstoken wird für den Zugriff auf die API-Ressource verwendet.

![Autorisierungstoken](media/active-directory-b2c-quickstarts-desktop-app/twitter-auth-token.png)

Nächster Schritt: Fahren Sie direkt mit dem Abschnitt [Bearbeiten des Profils](#edit-your-profile) fort.

### <a name="sign-up-using-an-email-address"></a>Registrieren mit einer E-Mail-Adresse

Wenn Sie nicht ein Social Media-Konto zur Authentifizierung verwenden möchten, können Sie mit einer gültigen E-Mail-Adresse ein Azure AD B2C-Benutzerkonto erstellen. Ein lokales Azure AD B2C-Benutzerkonto verwendet Azure Active Directory als Identitätsanbieter. Um Ihre E-Mail-Adresse zu verwenden, klicken Sie auf den Link **Sie haben noch kein Konto? Jetzt registrieren**.

![Anmelden oder Registrieren mit einer E-Mail-Adresse](media/active-directory-b2c-quickstarts-desktop-app/sign-in-or-sign-up-email-wpf.png)

Geben Sie eine gültige E-Mail-Adresse ein, und klicken Sie auf **Überprüfungscode senden**. Um den Überprüfungscode von Azure AD B2C zu erhalten, ist eine gültige E-Mail-Adresse erforderlich.

Geben Sie den Überprüfungscode ein, den Sie per E-Mail erhalten, und klicken Sie auf **Code überprüfen**.

Fügen Sie Ihre Profilinformationen hinzu, und klicken Sie auf **Erstellen**.

![Registrieren für ein neues Konto mit einer E-Mail-Adresse](media/active-directory-b2c-quickstarts-desktop-app/sign-up-new-account-profile-email-wpf.png)

Sie haben erfolgreich ein neues lokales Azure AD B2C-Benutzerkonto erstellt. Nach der Anmeldung wird das Zugriffstoken im Textfeld *Tokeninformationen* angezeigt. Das Zugriffstoken wird für den Zugriff auf die API-Ressource verwendet.

![Autorisierungstoken](media/active-directory-b2c-quickstarts-desktop-app/twitter-auth-token.png)

## <a name="edit-your-profile"></a>Bearbeiten des Profils

Azure Active Directory B2C bietet Funktionen, mit denen Benutzer ihre Profile aktualisieren können. Klicken Sie auf **Profil bearbeiten**, um das erstellte Profil zu bearbeiten.

![Bearbeiten eines Profils](media/active-directory-b2c-quickstarts-desktop-app/edit-profile-wpf.png)

Wählen Sie den Identitätsanbieter aus, der dem erstellten Konto zugeordnet ist. Wenn Sie beim Erstellen Ihres Kontos z.B. Twitter als Identitätsanbieter verwendet haben, wählen Sie Twitter aus, um die Details des verknüpften Profils zu ändern.

![Anbieter des Profils zur Bearbeitung auswählen](media/active-directory-b2c-quickstarts-desktop-app/edit-account-choose-provider-wpf.png)

Ändern Sie Ihren **Anzeigenamen** und Ihre **Stadt**. 

![Update profile (Profil aktualisieren)](media/active-directory-b2c-quickstarts-desktop-app/update-profile-wpf.png)

Im Textfeld *Tokeninformationen* wird ein neues Zugriffstoken angezeigt. Wenn Sie die Änderungen an Ihrem Profil überprüfen möchten, kopieren Sie das Zugriffstoken und fügen es in den Tokendecoder https://jwt.ms ein.

![Autorisierungstoken](media/active-directory-b2c-quickstarts-desktop-app/twitter-auth-token.png)

## <a name="access-a-resource"></a>Zugreifen auf eine Ressource

Klicken Sie auf **Call API** (API aufrufen), um eine Anforderung an die durch Azure AD B2C geschützte Ressource https://fabrikamb2chello.azurewebsites.net/hello zu übermitteln. 

![API aufrufen](media/active-directory-b2c-quickstarts-desktop-app/call-api-wpf.png)

Die Anwendung enthält das Zugriffstoken, das im Textfeld *Tokeninformationen* in der Anforderung angezeigt wird. Die API sendet den Anzeigenamen zurück, der im Zugriffstoken enthalten ist.

## <a name="next-steps"></a>Nächste Schritte

Der nächste Schritt besteht darin, einen eigenen Azure AD B2C-Mandanten zu erstellen und das Beispiel für die Ausführung über Ihren Mandanten zu konfigurieren. 

> [!div class="nextstepaction"]
> [Erstellen eines Azure Active Directory B2C-Mandanten im Azure-Portal](active-directory-b2c-get-started.md)
