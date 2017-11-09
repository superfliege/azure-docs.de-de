---
title: Testversion einer Azure AD B2C Webanwendung | Microsoft-Dokumentation
description: "User Journeys bei der Anmeldung, Registrierung, Profilbearbeitung und Kennwortzurücksetzung bei der Testversion einer Anwendung in einer Azure AD B2C-Testumgebung"
services: active-directory-b2c
documentationcenter: .net
author: saraford
manager: krassk
editor: PatAltimore
ms.assetid: 2ffb780d-2c51-4c2e-b8d6-39c40a81a77e
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 10/31/2017
ms.author: saraford
ms.openlocfilehash: 2a51f15fd38a901548dcf4c56922f9715c328463
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/01/2017
---
# <a name="test-drive-a-web-application-configured-with-azure-ad-b2c"></a>Testversion einer mit Azure AD B2C konfigurierten Webanwendung

Azure Active Directory B2C ermöglicht die Identitätsverwaltung in der Cloud, um Ihre Anwendung, Ihr Unternehmen und Ihre Kunden zu schützen.  In diesem Schnellstart wird eine Aufgabenlisten-Beispiel-App verwendet, um Folgendes zu veranschaulichen:

* Erstellen eines Kontos bei einem sozialen Netzwerk als Identitätsanbieter oder eines lokalen Kontos mit der E-Mail-Adresse über die Richtlinie **Registrieren oder anmelden** oder Anmelden bei einem solchen Konto 
* Aufrufen einer durch Azure AD B2C geschützten API zum Erstellen und Bearbeiten von Aufgaben

## <a name="prerequisites"></a>Voraussetzungen

* Installieren Sie [Visual Studio 2017](https://www.visualstudio.com/downloads/) mit den folgenden Workloads:
    - **ASP.NET und Webentwicklung**

* Sie besitzen ein Konto bei einem sozialen Netzwerk, d.h. bei Facebook, Google, Microsoft oder Twitter. Falls dies nicht der Fall sein sollte, ist eine gültige E-Mail-Adresse erforderlich.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Herunterladen des Beispiels

[Laden Sie die Beispielanwendung von GitHub herunter, oder klonen Sie sie](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi).

## <a name="run-the-app-in-visual-studio"></a>Ausführen der App in Visual Studio

Öffnen Sie in Visual Studio im Beispiel-Anwendungsprojektordner die Projektmappe `B2C-WebAPI-DotNet.sln`. 

Die Projektmappe besteht aus zwei Projekten:

* **TaskWebApp** – Eine ASP.NET MVC-Webanwendung, in der ein Benutzer die Elemente seiner Aufgabenliste verwalten kann.  
* **TaskService** – Ein ASP.NET-Web-API-Back-End, das alle in den Aufgabenlistenelementen eines Benutzers ausgeführte CRUD-Vorgänge verwaltet. Die Web-App ruft diese API auf und zeigt die Ergebnisse an.

Für diesen Schnellstart müssen die Projekte `TaskWebApp` und `TaskService` gleichzeitig ausgeführt werden. 

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf Ihre Projektmappe, und wählen Sie **Startprojekte festlegen...** aus. 
2. Aktivieren Sie das Optionsfeld **Mehrere Startprojekte**.
3. Ändern Sie die **Aktion** für beide Projekte in **Start**. Klicken Sie auf **OK**.

![Festlegen der Startseite in Visual Studio](media/active-directory-b2c-quickstarts-web-app/setup-startup-projects.png)

Wählen Sie **Debuggen > Debuggen starten** aus, um beide Anwendungen zu erstellen und auszuführen. Jede Anwendung wird in einer eigenen Browserregisterkarte geöffnet:

* `https://localhost:44316/` – Dies ist die Seite der ASP.NET-Webanwendung. Im Rahmen des Schnellstarts interagieren Sie direkt mit dieser Anwendung.
* `https://localhost:44332/` – Dies ist die Seite der Web-API, die von der ASP.NET-Webanwendung aufgerufen wird.

## <a name="create-an-account"></a>Erstellen eines Kontos

Klicken Sie in der ASP.NET-Webanwendung auf den Link **Registrieren/Anmelden**, um den Workflow **Registrieren oder anmelden** zu starten. Bei der Erstellung eines Kontos können Sie ein vorhandenes Konto bei einem sozialen Netzwerk als Identitätsanbieter oder ein E-Mail-Konto verwenden.

![ASP.NET-Web-App – Beispiel](media/active-directory-b2c-quickstarts-web-app/web-app-sign-in.png)

### <a name="sign-up-using-a-social-identity-provider"></a>Registrieren mit einem Konto bei einem sozialen Netzwerk als Identitätsanbieter

Um sich mit einem Konto bei einem sozialen Netzwerk als Identitätsanbieter zu registrieren, klicken Sie auf die Schaltfläche des Identitätsanbieters, den Sie verwenden möchten. Wenn Sie es vorziehen, eine E-Mail-Adresse zu verwenden, wechseln Sie zum Abschnitt [Registrieren mit einer E-Mail-Adresse](#sign-up-using-an-email-address).

![Anmelden oder Registrieren mit einem Konto bei einem Anbieter](media/active-directory-b2c-quickstarts-web-app/sign-in-or-sign-up-web.png)

Sie müssen sich mit den Anmeldeinformationen Ihres Social Media-Kontos authentifizieren (anmelden) und die Anwendung dazu autorisieren, Informationen von Ihrem Social Media-Konto zu lesen. Wenn Sie der Anwendung Zugriff auf diese gewähren, kann die Anwendung Profilinformationen aus dem Social Media-Konto abrufen, z.B. Ihren Namen und Ihre Stadt. 

![Authentifizierung und Autorisierung mit einem Social Media-Konto](media/active-directory-b2c-quickstarts-web-app/twitter-authenticate-authorize-web.png)

Schließen Sie den Anmeldevorgang für den Identitätsanbieter ab. Klicken Sie beispielsweise bei Twitter auf die Schaltfläche **Anmelden**.

Die Profildetails für Ihr neues Konto werden mit Informationen aus Ihrem Social Media-Konto aufgefüllt.

![Profildetails für das neue Konto beim Registrieren](media/active-directory-b2c-quickstarts-web-app/new-account-sign-up-profile-details-web.png)

Aktualisieren Sie die Felder „Anzeigename“, „Position“ und „Stadt“, und klicken Sie auf **Fortfahren**.  Die von Ihnen eingegebenen Werte werden für das Profil Ihres Azure AD B2C-Benutzerkontos verwendet.

Sie haben erfolgreich ein neues Azure AD B2C-Benutzerkonto über einen Identitätsanbieter erstellt. 

Nächster Schritt: Wechseln Sie zum Abschnitt [Anzeigen von Ansprüchen](#view-your-claims).

### <a name="sign-up-using-an-email-address"></a>Registrieren mit einer E-Mail-Adresse

Wenn Sie nicht ein Social Media-Konto zur Authentifizierung verwenden möchten, können Sie mit einer gültigen E-Mail-Adresse ein Azure AD B2C-Benutzerkonto erstellen. Ein lokales Azure AD B2C-Benutzerkonto verwendet Azure Active Directory als Identitätsanbieter. Um Ihre E-Mail-Adresse zu verwenden, klicken Sie auf den Link **Sie haben noch kein Konto? Jetzt registrieren**.

![Anmelden oder Registrieren mit einer E-Mail-Adresse](media/active-directory-b2c-quickstarts-web-app/sign-in-or-sign-up-email-web.png)

Geben Sie eine gültige E-Mail-Adresse ein, und klicken Sie auf **Überprüfungscode senden**. Um den Überprüfungscode von Azure AD B2C zu erhalten, ist eine gültige E-Mail-Adresse erforderlich. 

Geben Sie den Überprüfungscode ein, den Sie per E-Mail erhalten, und klicken Sie auf **Code überprüfen**.

Fügen Sie Ihre Profilinformationen hinzu, und klicken Sie auf **Erstellen**.

![Registrieren mit der E-Mail-Adresse für ein neues Konto](media/active-directory-b2c-quickstarts-web-app/sign-up-new-account-profile-email-web.png)

Sie haben erfolgreich ein neues lokales Azure AD B2C-Benutzerkonto erstellt.

## <a name="reset-your-password"></a>Zurücksetzen Ihres Kennworts

Wenn Sie Ihr Konto mithilfe einer E-Mail-Adresse erstellt haben, können Sie die Funktionen von Azure AD B2C verwenden, um Benutzern das Zurücksetzen ihres Kennworts zu ermöglichen. Um das von Ihnen erstellte Profil zu bearbeiten, klicken Sie in der Menüleiste auf Ihren Profilnamen, und wählen Sie **Kennwort zurücksetzen** aus.

Überprüfen Sie Ihre E-Mail-Adresse, indem Sie sie eingeben und auf **Überprüfungscode senden** klicken. Ein Überprüfungscode wird an Ihre E-Mail-Adresse gesendet.

Geben Sie den Überprüfungscode ein, den Sie per E-Mail erhalten haben, und klicken Sie auf **Code überprüfen**.

Nachdem Ihre E-Mail-Adresse verifiziert wurde, klicken Sie auf **Fortfahren**.

Geben Sie Ihr neues Kennwort ein, und klicken Sie auf **Fortfahren**.

## <a name="view-your-claims"></a>Anzeigen von Ansprüchen

Klicken Sie in der Menüleiste der Webanwendung auf **Ansprüche**, um die mit Ihrer letzten Aktion verknüpften Ansprüche anzuzeigen. 

![Registrieren mit der E-Mail-Adresse für ein neues Konto](media/active-directory-b2c-quickstarts-web-app/view-claims-sign-up-web.png)

In diesem Beispiel wurde bei der letzten Aktion die Funktion *Anmelden oder Registrieren* verwendet. Beachten Sie, dass der **Anspruchstyp** `http://schemas.microsoft.com/claims/authnclassreference` `b2c_1_susi` lautet, was darauf hinweist, dass es sich bei der letzten Aktion um eine Registrierung oder Anmeldung handelt. Wenn bei der letzten Aktion ein Kennwort zurückgesetzt worden wäre, würde der **Anspruchstyp** `b2c_1_reset` lauten.

## <a name="edit-your-profile"></a>Bearbeiten des Profils

Azure Active Directory B2C bietet Funktionen, mit denen Benutzer ihre Profile aktualisieren können. Klicken Sie in der Menüleiste der Webanwendung auf Ihren Profilnamen, und wählen Sie **Profil bearbeiten** aus, um das von Ihnen erstellte Profil zu bearbeiten.

![Bearbeiten eines Profils](media/active-directory-b2c-quickstarts-web-app/edit-profile-web.png)

Ändern Sie Ihren **Anzeigenamen** und Ihre **Stadt**.  Klicken Sie auf **Fortfahren**, um Ihr Profil zu aktualisieren.

![Update profile (Profil aktualisieren)](media/active-directory-b2c-quickstarts-web-app/update-profile-web.png)

Beachten Sie, dass Ihr Anzeigename oben rechts auf der Seite aktualisiert wird, nachdem Sie Ihren Namen geändert haben. 

Klicken Sie auf **Ansprüche**. Änderungen, die Sie am **Anzeigenamen** und an der **Stadt** vorgenommen haben, werden auch in den Ansprüchen übernommen.

![Anzeigen von Ansprüchen](media/active-directory-b2c-quickstarts-web-app/view-claims-update-web.png)

 Beachten Sie, dass der **Anspruchstyp** `http://schemas.microsoft.com/claims/authnclassreference` in `b2c_1_edit_profile` geändert wurde, was darauf hinweist, dass es sich bei der zuletzt ausgeführten Aktion um eine Profilbearbeitung handelt. Beachten Sie außerdem, dass der Name und die Stadt nun die Werte *Sara S.* bzw. *Seattle* enthalten.

## <a name="access-a-resource"></a>Zugreifen auf eine Ressource

Klicken Sie auf **Aufgabenliste**, um Ihre Aufgabenlistenelemente einzugeben und zu ändern. Die ASP.NET-Webanwendung schließt ein Zugriffstoken in die Anforderung an die Web-API-Ressource ein, das die Berechtigung zum Ausführen von Vorgängen bezüglich der Aufgabenlistenelemente des Benutzers anfordert. 

Geben Sie Text in das Textfeld **Neues Element** ein. Klicken Sie auf **Hinzufügen**, um die mit Azure AD B2C geschützte Web-API aufzurufen, die ein Aufgabenlistenelement hinzufügt.

![Hinzufügen eines Aufgabenlistenelements](media/active-directory-b2c-quickstarts-web-app/add-todo-item-web.png)

## <a name="other-scenarios"></a>Andere Szenarien

Zu anderen Szenarien im Zusammenhang mit der Testversion zählen Folgende:

* Melden Sie sich von der Anwendung ab, und klicken Sie auf **Aufgabenliste**. Beachten Sie, dass Sie zur Anmeldung aufgefordert werden und Ihre Listenelemente beibehalten werden. 
* Erstellen Sie ein neues Konto mit einem anderen Kontotyp. Wenn Sie zuvor ein Konto mit einer E-Mail-Adresse erstellt haben, verwenden Sie beispielsweise ein Konto bei einem sozialen Netzwerk als Identitätsanbieter.

## <a name="next-steps"></a>Nächste Schritte

Der nächste Schritt besteht darin, einen eigenen Azure AD B2C-Mandanten zu erstellen und das Beispiel für die Ausführung über Ihren Mandanten zu konfigurieren. 

> [!div class="nextstepaction"]
> [Erstellen eines Azure Active Directory B2C-Mandanten im Azure-Portal](active-directory-b2c-get-started.md)