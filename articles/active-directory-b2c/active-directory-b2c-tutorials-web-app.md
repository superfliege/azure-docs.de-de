---
title: 'Tutorial: Aktivieren der Authentifizierung von Web-Apps mit Konten unter Verwendung von Azure Active Directory B2C | Microsoft-Dokumentation'
description: Tutorial zur Verwendung von Azure Active Directory B2C zum Bereitstellen einer Benutzeranmeldung für eine ASP.NET-Webanwendung.
services: active-directory-b2c
author: davidmu1
ms.author: davidmu
ms.date: 1/23/2018
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory-b2c
ms.openlocfilehash: 59e23344d235bac8f69bba76cfff2922bc41fd0f
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2018
---
# <a name="tutorial-enable-a-web-application-to-authenticate-with-accounts-using-azure-active-directory-b2c"></a>Tutorial: Aktivieren der Authentifizierung von Web-Apps mit Konten unter Verwendung von Azure Active Directory B2C

In diesem Tutorial erfahren Sie, wie Sie Azure Active Directory (Azure AD) B2C für die Anmeldung und Registrierung von Benutzern in einer ASP.NET-Web-App verwenden. Mit Azure AD B2C können sich Ihre Apps über offene Standardprotokolle bei Konten für soziale Netzwerke sowie bei Unternehmenskonten und Azure Active Directory-Konten authentifizieren.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Registrieren einer ASP.NET-Beispiel-Web-App bei Ihrem Azure AD B2C-Mandanten
> * Erstellen von Richtlinien für Benutzerregistrierung, Benutzeranmeldung, Profilbearbeitung und Kennwortzurücksetzung
> * Konfigurieren der Beispiel-Web-App für die Verwendung Ihres Azure AD B2C-Mandanten 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

* Erstellen Sie Ihren eigenen [Azure AD B2C-Mandanten](active-directory-b2c-get-started.md).
* Installieren Sie [Visual Studio 2017](https://www.visualstudio.com/downloads/) mit der Workload **ASP.NET und Webentwicklung**.

## <a name="register-web-app"></a>Registrieren einer Web-App

Anwendungen müssen bei Ihrem Mandanten [registriert](../active-directory/develop/active-directory-dev-glossary.md#application-registration) werden, um [Zugriffstoken](../active-directory/develop/active-directory-dev-glossary.md#access-token) aus Azure Active Directory erhalten zu können. Bei der App-Registrierung wird für die App eine [Anwendungs-ID](../active-directory/develop/active-directory-dev-glossary.md#application-id-client-id) in Ihrem Mandanten erstellt. 

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als globaler Administrator Ihres Azure AD B2C-Mandanten an.

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

1. Wählen Sie **Azure AD B2C** aus der Dienstliste im Azure-Portal aus. 

2. Klicken Sie in den B2C-Einstellungen auf **Anwendungen** und anschließend auf **Hinzufügen**. 

    Verwenden Sie die folgenden Einstellungen, um die Beispiel-Web-App bei Ihrem Mandanten zu registrieren:

    ![Hinzufügen einer neuen App](media/active-directory-b2c-tutorials-web-app/web-app-registration.png)
    
    | Einstellung      | Empfohlener Wert  | Beschreibung                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Name** | My Sample Web App | Geben Sie einen aussagekräftigen **App-Namen**ein. | 
    | **Web-App/Web-API einschließen** | Ja | Wählen Sie für eine Web-App die Option **Ja** aus. |
    | **Impliziten Fluss zulassen** | Ja | Diese App verwendet die [OpenID Connect-Anmeldung](active-directory-b2c-reference-oidc.md). Wählen Sie daher **Ja** aus. |
    | **Antwort-URL** | `https://localhost:44316` | Antwort-URLs sind Endpunkte, an denen Azure AD B2C von Ihrer App angeforderte Token zurückgibt. Das Beispiel in diesem Tutorial wird lokal (localhost) ausgeführt und lauscht am Port 44316. |
    | **Nativen Client einschließen** | Nein  | Da es sich hierbei um eine Web-App und nicht um einen nativen Client handelt, wählen Sie „Nein“ aus. |
    
3. Klicken Sie auf **Erstellen**, um Ihre App zu registrieren.

Registrierte Apps werden in der Anwendungsliste für den Azure AD B2C-Mandanten angezeigt. Wählen Sie Ihre Web-App aus der Liste aus. Der Eigenschaftenbereich der Web-App wird angezeigt.

![Web-App-Eigenschaften](./media/active-directory-b2c-tutorials-web-app/b2c-web-app-properties.png)

Notieren Sie sich den Wert der **Anwendungsclient-ID**. Die ID identifiziert die App eindeutig und wird im weiteren Verlauf des Tutorials beim Konfigurieren der App benötigt.

### <a name="create-a-client-password"></a>Erstellen eines Clientkennworts

Azure AD B2C verwendet die OAuth2-Autorisierung für [Clientanwendungen](../active-directory/develop/active-directory-dev-glossary.md#client-application). Web-Apps sind [vertrauliche Clients](../active-directory/develop/active-directory-dev-glossary.md#web-client) und erfordern eine Client-ID oder Anwendungs-ID und ein Clientgeheimnis, ein Clientkennwort oder einen Anwendungsschlüssel.

1. Klicken Sie auf der Seite „Schlüssel“ für die registrierte Web-App auf **Schlüssel generieren**.

2. Klicken Sie auf **Speichern**, um den App-Schlüssel anzuzeigen.

    ![Allgemeine Schlüsselseite für die App](media/active-directory-b2c-tutorials-web-app/app-general-keys-page.png)

Der Schlüssel wird einmal im Portal angezeigt. Wichtig: Kopieren und speichern Sie den Schlüsselwert. Er wird zum Konfigurieren der App benötigt. Bewahren Sie den Schlüssel an einem sicheren Ort auf. Veröffentlichen Sie den Schlüssel nicht.

## <a name="create-policies"></a>Erstellen von Richtlinien

Eine Azure AD B2C-Richtlinie dient zum Definieren von Benutzerworkflows. Gängige Workflows sind beispielsweise Anmelden, Registrieren, Ändern von Kennwörtern und Bearbeiten von Profilen.

### <a name="create-a-sign-up-or-sign-in-policy"></a>Erstellen einer Registrierungs- oder Anmelderichtlinie

Erstellen Sie eine **Registrierungs- oder Anmelderichtlinie**, um Benutzer für den Zugriff und die anschließende Anmeldung zu registrieren.

1. Wählen Sie auf der Azure AD B2C-Portalseite die Option **Registrierungs- oder Anmelderichtlinien** aus, und klicken Sie auf **Hinzufügen**.

    Konfigurieren Sie Ihre Richtlinie mit den folgenden Einstellungen:

    ![Hinzufügen einer Registrierungs- oder Anmelderichtlinie](media/active-directory-b2c-tutorials-web-app/add-susi-policy.png)

    | Einstellung      | Empfohlener Wert  | Beschreibung                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Name** | SiUpIn | Geben Sie einen **Namen**für die Richtlinie ein. Der Richtlinienname wird mit dem Präfix **b2c_1_** versehen. Im Beispielcode wird der vollständige Richtlinienname **b2c_1_SiUpIn** verwendet. | 
    | **Identitätsanbieter** | E-Mail-Registrierung | Der Identitätsanbieter zur eindeutigen Identifizierung des Benutzers. |
    | **Registrierungsattribute** | „Anzeigename“ und „Postleitzahl“ | Wählen Sie Attribute aus, die im Rahmen der Benutzerregistrierung erfasst werden sollen. |
    | **Anwendungsansprüche** | „Anzeigename“, „Postleitzahl“, „User is new“ (Benutzer ist neu), „User's Object ID“ (Objekt-ID des Benutzers) | Wählen Sie [Ansprüche](../active-directory/develop/active-directory-dev-glossary.md#claim) aus, die im [Zugriffstoken](../active-directory/develop/active-directory-dev-glossary.md#access-token) enthalten sein sollen. |

2. Klicken Sie auf **Erstellen**, um die Richtlinie zu erstellen. 

### <a name="create-a-profile-editing-policy"></a>Erstellen einer Richtlinie für die Profilbearbeitung

Erstellen Sie eine **Richtlinie zur Profilbearbeitung**, um Benutzern das eigenständige Zurücksetzen ihrer Benutzerprofilinformationen zu ermöglichen.

1. Wählen Sie auf der Azure AD B2C-Portalseite die Option **Richtlinien zur Profilbearbeitung** aus, und klicken Sie auf **Hinzufügen**.

    Konfigurieren Sie Ihre Richtlinie mit den folgenden Einstellungen:

    | Einstellung      | Empfohlener Wert  | Beschreibung                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Name** | SiPe | Geben Sie einen **Namen**für die Richtlinie ein. Der Richtlinienname wird mit dem Präfix **b2c_1_** versehen. Im Beispielcode wird der vollständige Richtlinienname **b2c_1_SiPe** verwendet. | 
    | **Identitätsanbieter** | Anmeldung mit lokalem Konto | Der Identitätsanbieter zur eindeutigen Identifizierung des Benutzers. |
    | **Profilattribute** | „Anzeigename“ und „Postleitzahl“ | Wählen Sie Attribute aus, die Benutzer bei der Profilbearbeitung ändern können. |
    | **Anwendungsansprüche** | „Anzeigename“, „Postleitzahl“, „User's Object ID“ (Objekt-ID des Benutzers) | Wählen Sie [Ansprüche](../active-directory/develop/active-directory-dev-glossary.md#claim) aus, die nach erfolgreicher Profilbearbeitung im [Zugriffstoken](../active-directory/develop/active-directory-dev-glossary.md#access-token) enthalten sein sollen. |

2. Klicken Sie auf **Erstellen**, um die Richtlinie zu erstellen. 

### <a name="create-a-password-reset-policy"></a>Erstellen einer Richtlinie zur Kennwortrücksetzung

Wenn Sie in Ihrer Anwendung das Zurücksetzen des Kennworts ermöglichen möchten, müssen Sie eine **Richtlinie für die Kennwortzurücksetzung** erstellen. Diese Richtlinie beschreibt die Kundenerfahrung während der Kennwortzurücksetzung und den Inhalt der Token, die die Anwendung bei erfolgreichem Abschluss erhält.

1. Wählen Sie auf der Azure AD B2C-Portalseite die Option **Richtlinien für die Kennwortzurücksetzung** aus, und klicken Sie auf **Hinzufügen**.

    Konfigurieren Sie Ihre Richtlinie mit den folgenden Einstellungen:

    | Einstellung      | Empfohlener Wert  | Beschreibung                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Name** | SSPR | Geben Sie einen **Namen**für die Richtlinie ein. Der Richtlinienname wird mit dem Präfix **b2c_1_** versehen. Im Beispielcode wird der vollständige Richtlinienname **b2c_1_SSPR** verwendet. | 
    | **Identitätsanbieter** | Kennwort mittels E-Mail-Adresse zurücksetzen | Dies ist der Identitätsanbieter zur eindeutigen Identifizierung des Benutzers. |
    | **Anwendungsansprüche** | User's Object ID (Objekt-ID des Benutzers) | Wählen Sie [Ansprüche](../active-directory/develop/active-directory-dev-glossary.md#claim) aus, die nach erfolgreicher Kennwortzurücksetzung im [Zugriffstoken](../active-directory/develop/active-directory-dev-glossary.md#access-token) enthalten sein sollen. |

2. Klicken Sie auf **Erstellen**, um die Richtlinie zu erstellen. 

## <a name="update-web-app-code"></a>Aktualisieren des Web-App-Codes

Nachdem Sie eine Web-App registriert und Richtlinien erstellt haben, müssen Sie Ihre App für die Verwendung Ihres Azure AD B2C-Mandanten konfigurieren. In diesem Tutorial konfigurieren Sie eine Beispiel-Web-App, die Sie von GitHub herunterladen können. 

[Laden Sie eine ZIP-Datei herunter](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi/archive/master.zip), oder klonen Sie die Beispiel-Web-App aus GitHub.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

Bei der ASP.NET-Beispiel-Web-App handelt es sich um eine einfache App zum Erstellen und Aktualisieren einer Aufgabenliste. Die App verwendet [Microsoft OWIN-Middleware-Komponenten](https://docs.microsoft.com/en-us/aspnet/aspnet/overview/owin-and-katana/), um Benutzern die Registrierung für die Verwendung der App in Ihrem Azure AD B2C-Mandanten zu ermöglichen. Durch Erstellung einer Azure AD B2C-Richtlinie können Benutzer ein Konto für ein soziales Netzwerk verwenden oder ein Konto erstellen und dieses als Identität für den App-Zugriff verwenden. 

Die Beispielprojektmappe enthält zwei Projekte:

**Web-App-Beispiel-App (TaskWebApp):** Web-App zum Erstellen und Bearbeiten einer Aufgabenliste. Die Web-App verwendet die **Registrierungs- oder Anmelderichtlinie** für die Registrierung oder Anmeldung von Benutzern.

**Web-API-Beispiel-App (TaskService):** Web-API, die die Funktionen zum Erstellen, Lesen, Aktualisieren und Löschen der Aufgabenliste unterstützt. Die Web-API wird per Azure AD B2C geschützt und von der Web-App aufgerufen.

Sie müssen die App ändern, sodass die App-Registrierung in Ihrem Mandanten verwendet wird. Sie beinhaltet die Client-ID oder Anwendungs-ID und das Clientkennwort oder den Anwendungsschlüssel. Außerdem müssen die von Ihnen erstellten Richtlinien konfiguriert werden. Die Beispiel-Web-App definiert die Konfigurationswerte als App-Einstellungen in der Datei „Web.config“. Gehen Sie zum Ändern der App-Einstellungen wie folgt vor:

1. Öffnen Sie die Projektmappe **B2C-WebAPI-DotNet** in Visual Studio.

2. Öffnen Sie im Web-App-Projekt **TaskWebApp** die Datei **Web.config**, und nehmen Sie folgende Änderungen an den vorhandenen Schlüsseln vor:

    ```C#
    <add key="ida:Tenant" value="<Your tenant name>.onmicrosoft.com" />
    
    <add key="ida:ClientId" value="The Application ID for your web app registered in your tenant" />
    
    <add key="ida:ClientSecret" value="Client password (client secret or app key)" />
    ```
3. Aktualisieren Sie die vorhandenen Schlüssel mit den Werten der im vorherigen Schritt erstellten Richtliniennamen. Denken Sie daran, das Präfix *b2c_1_* einzufügen.

    ```C#
    <add key="ida:SignUpSignInPolicyId" value="b2c_1_SiUpIn" />
    <add key="ida:EditProfilePolicyId" value="b2c_1_SiPe" />
    <add key="ida:ResetPasswordPolicyId" value="b2c_1_SSPR" />
    ```

## <a name="run-the-sample-web-app"></a>Ausführen der Beispiel-Web-App

Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **TaskWebApp**, und klicken Sie anschließend auf **Als Startprojekt festlegen**.

Drücken Sie**F5**, um die Web-App zu starten. Der Standardbrowser wird mit der lokalen Websiteadresse `https://localhost:44316/` geöffnet. 

Die Beispiel-App unterstützt Registrierung, Anmeldung, Profilbearbeitung und Kennwortzurücksetzung. In diesem Tutorial wird beschrieben, wie ein Benutzer sich mit einer E-Mail-Adresse für die Nutzung der App anmeldet. Die anderen Szenarien können Sie selbstständig erkunden.

### <a name="sign-up-using-an-email-address"></a>Registrieren mit einer E-Mail-Adresse

1. Klicken Sie auf dem oberen Banner auf den Link **Registrieren/Anmelden**, um sich als Benutzer der Web-App zu registrieren. Hierbei wird die zuvor definierte Richtlinie **b2c_1_SiUpIn** verwendet.

2. Azure AD B2C zeigt eine Anmeldeseite mit einem Registrierungslink an. Da Sie noch nicht über ein Konto verfügen, klicken Sie auf den Link **Jetzt registrieren**. 

3. Der Registrierungsworkflow zeigt eine Seite an, über die die Identität des Benutzers mithilfe einer E-Mail-Adresse erfasst und überprüft wird. Darüber hinaus werden im Rahmen des Registrierungsworkflows auch das Kennwort des Benutzers sowie die angeforderten Attribute erfasst, die in der Richtlinie definiert wurden.

    Verwenden Sie eine gültige E-Mail-Adresse, und bestätigen Sie sie mithilfe des Prüfcodes. Legen Sie ein Kennwort fest. Geben Sie Werte für die angeforderten Attribute ein. 

    ![Registrierungsworkflow](media/active-directory-b2c-tutorials-web-app/sign-up-workflow.png)

4. Klicken Sie auf **Erstellen**, um im Azure AD B2C-Mandanten ein lokales Konto zu erstellen.

Nun kann sich der Benutzer mithilfe seiner E-Mail-Adresse anmelden und die Web-App verwenden.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Sie können Ihren Azure AD B2C-Mandanten für weitere Azure AD B2C-Tutorials verwenden. Wenn Sie ihn nicht mehr benötigt, können Sie [Ihren Azure AD B2C-Mandanten löschen](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie einen Azure AD B2C-Mandanten und Richtlinien erstellen und wie Sie die Beispiel-Web-App für die Verwendung Ihres Azure AD B2C-Mandanten aktualisieren. Im nächsten Tutorial erfahren Sie, wie Sie eine durch Ihren Azure AD B2C-Mandanten geschützte ASP.NET-Web-API registrieren, konfigurieren und aufrufen.

> [!div class="nextstepaction"]
> [Tutorial: Schützen einer ASP.NET-Web-API mithilfe von Azure Active Directory B2C](active-directory-b2c-tutorials-web-api.md)
