---
title: 'Tutorial: Aktivieren der Authentifizierung von Desktop-Apps mit Konten unter Verwendung von Azure Active Directory B2C | Microsoft-Dokumentation'
description: Tutorial zur Verwendung von Azure Active Directory B2C zum Bereitstellen einer Benutzeranmeldung für eine .NET-Desktop-App.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.author: davidmu
ms.date: 2/28/2018
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.component: B2C
ms.openlocfilehash: 40adcc7280a279e066c9e22f3252972cf07d070d
ms.sourcegitcommit: 616e63d6258f036a2863acd96b73770e35ff54f8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/14/2018
ms.locfileid: "45604075"
---
# <a name="tutorial-enable-desktop-app-authentication-with-accounts-using-azure-active-directory-b2c"></a>Tutorial: Aktivieren der Authentifizierung von Desktop-Apps mit Konten unter Verwendung von Azure Active Directory B2C

In diesem Tutorial erfahren Sie, wie Sie Azure Active Directory (Azure AD) B2C für die Anmeldung und Registrierung von Benutzern in einer WPF-Desktop-Anwendung (Windows Presentation Foundation) verwenden. Mit Azure AD B2C können sich Ihre Apps über offene Standardprotokolle bei Konten für soziale Netzwerke sowie bei Unternehmenskonten und Azure Active Directory-Konten authentifizieren.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Registrieren der Beispiel-Desktop-App bei Ihrem Azure AD B2C-Mandanten
> * Erstellen von Richtlinien für Benutzerregistrierung, Benutzeranmeldung, Profilbearbeitung und Kennwortzurücksetzung
> * Konfigurieren der Beispielanwendung für die Verwendung Ihres Azure AD B2C-Mandanten

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

* Erstellen Sie Ihren eigenen [Azure AD B2C-Mandanten](active-directory-b2c-get-started.md).
* Installieren Sie [Visual Studio 2017](https://www.visualstudio.com/downloads/) mit den Workloads **.NET-Desktopentwicklung** und **ASP.NET und Webentwicklung**.

## <a name="register-desktop-app"></a>Registrieren der Desktop-App

Anwendungen müssen bei Ihrem Mandanten [registriert](../active-directory/develop/developer-glossary.md#application-registration) werden, um [Zugriffstoken](../active-directory/develop/developer-glossary.md#access-token) aus Azure Active Directory erhalten zu können. Bei der App-Registrierung wird für die App eine [Anwendungs-ID](../active-directory/develop/developer-glossary.md#application-id-client-id) in Ihrem Mandanten erstellt. 

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als globaler Administrator Ihres Azure AD B2C-Mandanten an.

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

1. Wählen Sie **Azure AD B2C** aus der Dienstliste im Azure-Portal aus. 

2. Klicken Sie in den B2C-Einstellungen auf **Anwendungen** und anschließend auf **Hinzufügen**. 

    Verwenden Sie die folgenden Einstellungen, um die Beispiel-Web-App bei Ihrem Mandanten zu registrieren:
    
    ![Hinzufügen einer neuen App](media/active-directory-b2c-tutorials-desktop-app/desktop-app-registration.png)
    
    | Einstellung      | Empfohlener Wert  | Beschreibung                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Name** | My Sample WPF App | Geben Sie einen aussagekräftigen **App-Namen**ein. | 
    | **Web-App/Web-API einschließen** | Nein  | Wählen Sie bei einer Desktop-App **Nein** aus. |
    | **Nativen Client einschließen** | JA | Dies ist eine Desktop-App, die als nativer Client betrachtet wird. |
    | **Umleitungs-URI** | Standardwerte | Eindeutiger Bezeichner, an den Azure AD B2C den Benutzer-Agent in einer OAuth 2.0-Antwort umleitet |
    | **Benutzerdefinierter Umleitungs-URI** | `com.onmicrosoft.contoso.appname://redirect/path` | Geben Sie `com.onmicrosoft.<your tenant name>.<any app name>://redirect/path` ein. Richtlinien senden Token an diesen URI. |
    
3. Klicken Sie auf **Erstellen**, um Ihre App zu registrieren.

Registrierte Apps werden in der Anwendungsliste für den Azure AD B2C-Mandanten angezeigt. Wählen Sie Ihre Desktop-App aus der Liste aus. Der Eigenschaftenbereich der registrierten Desktop-App wird angezeigt.

![Eigenschaften der Desktop-App](./media/active-directory-b2c-tutorials-desktop-app/b2c-desktop-app-properties.png)

Notieren Sie sich den Wert der **Anwendungsclient-ID**. Die ID identifiziert die App eindeutig und wird im weiteren Verlauf des Tutorials beim Konfigurieren der App benötigt.

## <a name="create-policies"></a>Erstellen von Richtlinien

Eine Azure AD B2C-Richtlinie dient zum Definieren von Benutzerworkflows. Gängige Workflows sind beispielsweise Anmelden, Registrieren, Ändern von Kennwörtern und Bearbeiten von Profilen.

### <a name="create-a-sign-up-or-sign-in-policy"></a>Erstellen einer Registrierungs- oder Anmelderichtlinie

Erstellen Sie eine **Registrierungs- oder Anmelderichtlinie**, um Benutzer für den Zugriff auf die Desktop-App und die anschließende Anmeldung zu registrieren.

1. Wählen Sie auf der Azure AD B2C-Portalseite die Option **Registrierungs- oder Anmelderichtlinien** aus, und klicken Sie auf **Hinzufügen**.

    Konfigurieren Sie Ihre Richtlinie mit den folgenden Einstellungen:

    ![Hinzufügen einer Registrierungs- oder Anmelderichtlinie](media/active-directory-b2c-tutorials-desktop-app/add-susi-policy.png)

    | Einstellung      | Empfohlener Wert  | Beschreibung                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Name** | SiUpIn | Geben Sie einen **Namen** für die Richtlinie ein. Der Richtlinienname wird mit dem Präfix **B2C_1_** versehen. Im Beispielcode wird der vollständige Richtlinienname **B2C_1_SiUpIn** verwendet. | 
    | **Identitätsanbieter** | E-Mail-Registrierung | Der Identitätsanbieter zur eindeutigen Identifizierung des Benutzers. |
    | **Registrierungsattribute** | „Anzeigename“ und „Postleitzahl“ | Wählen Sie Attribute aus, die im Rahmen der Benutzerregistrierung erfasst werden sollen. |
    | **Anwendungsansprüche** | „Anzeigename“, „Postleitzahl“, „User is new“ (Benutzer ist neu), „User's Object ID“ (Objekt-ID des Benutzers) | Wählen Sie [Ansprüche](../active-directory/develop/developer-glossary.md#claim) aus, die im [Zugriffstoken](../active-directory/develop/developer-glossary.md#access-token) enthalten sein sollen. |

2. Klicken Sie auf **Erstellen**, um die Richtlinie zu erstellen. 

### <a name="create-a-profile-editing-policy"></a>Erstellen einer Richtlinie für die Profilbearbeitung

Erstellen Sie eine **Richtlinie zur Profilbearbeitung**, um Benutzern das eigenständige Zurücksetzen ihrer Benutzerprofilinformationen zu ermöglichen.

1. Wählen Sie auf der Azure AD B2C-Portalseite die Option **Richtlinien zur Profilbearbeitung** aus, und klicken Sie auf **Hinzufügen**.

    Konfigurieren Sie Ihre Richtlinie mit den folgenden Einstellungen:

    | Einstellung      | Empfohlener Wert  | Beschreibung                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Name** | SiPe | Geben Sie einen **Namen** für die Richtlinie ein. Der Richtlinienname wird mit dem Präfix **B2C_1_** versehen. Im Beispielcode wird der vollständige Richtlinienname **B2C_1_SiPe** verwendet. | 
    | **Identitätsanbieter** | Anmeldung mit lokalem Konto | Der Identitätsanbieter zur eindeutigen Identifizierung des Benutzers. |
    | **Profilattribute** | „Anzeigename“ und „Postleitzahl“ | Wählen Sie Attribute aus, die Benutzer bei der Profilbearbeitung ändern können. |
    | **Anwendungsansprüche** | „Anzeigename“, „Postleitzahl“, „User's Object ID“ (Objekt-ID des Benutzers) | Wählen Sie [Ansprüche](../active-directory/develop/developer-glossary.md#claim) aus, die nach erfolgreicher Profilbearbeitung im [Zugriffstoken](../active-directory/develop/developer-glossary.md#access-token) enthalten sein sollen. |

2. Klicken Sie auf **Erstellen**, um die Richtlinie zu erstellen. 

### <a name="create-a-password-reset-policy"></a>Erstellen einer Richtlinie zur Kennwortrücksetzung

Wenn Sie in Ihrer Anwendung das Zurücksetzen des Kennworts ermöglichen möchten, müssen Sie eine **Richtlinie für die Kennwortzurücksetzung** erstellen. Diese Richtlinie beschreibt die Kundenerfahrung während der Kennwortzurücksetzung und den Inhalt der Token, die die Anwendung bei erfolgreichem Abschluss erhält.

1. Wählen Sie auf der Azure AD B2C-Portalseite die Option **Richtlinien für die Kennwortzurücksetzung** aus, und klicken Sie auf **Hinzufügen**.

    Konfigurieren Sie Ihre Richtlinie mit den folgenden Einstellungen:

    | Einstellung      | Empfohlener Wert  | Beschreibung                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Name** | SSPR | Geben Sie einen **Namen** für die Richtlinie ein. Der Richtlinienname wird mit dem Präfix **B2C_1_** versehen. Im Beispielcode wird der vollständige Richtlinienname **B2C_1_SSPR** verwendet. | 
    | **Identitätsanbieter** | Kennwort mittels E-Mail-Adresse zurücksetzen | Dies ist der Identitätsanbieter zur eindeutigen Identifizierung des Benutzers. |
    | **Anwendungsansprüche** | User's Object ID (Objekt-ID des Benutzers) | Wählen Sie [Ansprüche](../active-directory/develop/developer-glossary.md#claim) aus, die nach erfolgreicher Kennwortzurücksetzung im [Zugriffstoken](../active-directory/develop/developer-glossary.md#access-token) enthalten sein sollen. |

2. Klicken Sie auf **Erstellen**, um die Richtlinie zu erstellen. 

## <a name="update-desktop-app-code"></a>Aktualisieren des Codes der Desktop-App

Nachdem Sie eine Desktop-App registriert und Richtlinien erstellt haben, müssen Sie Ihre App für die Verwendung Ihres Azure AD B2C-Mandanten konfigurieren. In diesem Tutorial konfigurieren Sie eine Beispiel-Desktop-App. 

[Laden Sie eine ZIP-Datei herunter](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop/archive/master.zip), oder klonen Sie das Beispiel aus GitHub.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop.git
```

Das Beispiel für die WPF-Desktop-App veranschaulicht, wie eine Desktop-App Azure AD B2C für die Benutzerregistrierung und -anmeldung sowie zum Aufrufen einer geschützten Web-API verwenden kann.

Sie müssen die App ändern, damit sie die App-Registrierung in Ihrem Mandanten verwendet, und die von Ihnen erstellten Richtlinien konfigurieren. 

Gehen Sie zum Ändern der App-Einstellungen wie folgt vor:

1. Öffnen Sie die Projektmappe `active-directory-b2c-wpf` in Visual Studio.

2. Öffnen Sie im Projekt `active-directory-b2c-wpf` die Datei **App.xaml.cs**, und nehmen Sie die folgenden Änderungen vor:

    ```C#
    private static string Tenant = "<your-tenant-name>.onmicrosoft.com";
    private static string ClientId = "The Application ID for your desktop app registered in your tenant";
    ```

3. Aktualisieren Sie die Variable **PolicySignUpSignIn** mit dem Namen der im vorherigen Schritt erstellten *Registrierungs- und Anmelderichtlinie*. Denken Sie daran, das Präfix *B2C_1_* einzufügen.

    ```C#
    public static string PolicySignUpSignIn = "B2C_1_SiUpIn";
    ```

## <a name="run-the-sample-desktop-application"></a>Ausführen der Beispiel-Desktop-App

Drücken Sie **F5**, um die Desktop-App zu erstellen und auszuführen. 

Die Beispiel-App unterstützt Registrierung, Anmeldung, Profilbearbeitung und Kennwortzurücksetzung. In diesem Tutorial wird beschrieben, wie ein Benutzer sich mit einer E-Mail-Adresse für die Nutzung der App anmeldet. Die anderen Szenarien können Sie selbstständig erkunden.

### <a name="sign-up-using-an-email-address"></a>Registrieren mit einer E-Mail-Adresse

1. Klicken Sie auf die Schaltfläche **Anmelden**, um sich als Benutzer der Desktop-App zu registrieren. Hierbei wird die zuvor definierte Richtlinie **B2C_1_SiUpIn** verwendet.

2. Azure AD B2C zeigt eine Anmeldeseite mit einem Registrierungslink an. Da Sie noch nicht über ein Konto verfügen, klicken Sie auf den Link **Jetzt registrieren**. 

3. Der Registrierungsworkflow zeigt eine Seite an, über die die Identität des Benutzers mithilfe einer E-Mail-Adresse erfasst und überprüft wird. Darüber hinaus werden im Rahmen des Registrierungsworkflows auch das Kennwort des Benutzers sowie die angeforderten Attribute erfasst, die in der Richtlinie definiert wurden.

    Verwenden Sie eine gültige E-Mail-Adresse, und bestätigen Sie sie mithilfe des Prüfcodes. Legen Sie ein Kennwort fest. Geben Sie Werte für die angeforderten Attribute ein. 

    ![Registrierungsworkflow](media/active-directory-b2c-tutorials-desktop-app/sign-up-workflow.png)

4. Klicken Sie auf **Erstellen**, um im Azure AD B2C-Mandanten ein lokales Konto zu erstellen.

Nun kann sich der Benutzer mithilfe seiner E-Mail-Adresse anmelden und die Desktop-App verwenden.

> [!NOTE]
> Wenn Sie auf die Schaltfläche **API aufrufen** klicken, wird der Fehler „Nicht autorisiert“ angezeigt. Der Fehler wird angezeigt, da Sie versuchen, über den Demomandanten auf eine Ressource zuzugreifen. Da Ihr Zugriffstoken nur für Ihren Azure AD-Mandanten gilt, ist der API-Aufruf nicht autorisiert. Fahren Sie mit dem nächsten Tutorial fort, um eine geschützte Web-API für Ihren Mandanten zu erstellen. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Sie können Ihren Azure AD B2C-Mandanten für weitere Azure AD B2C-Tutorials verwenden. Wenn Sie ihn nicht mehr benötigt, können Sie [Ihren Azure AD B2C-Mandanten löschen](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie einen Azure AD B2C-Mandanten und Richtlinien erstellen und wie Sie die Beispiel-Desktop-App für die Verwendung Ihres Azure AD B2C-Mandanten aktualisieren. Im nächsten Tutorial erfahren Sie, wie Sie eine geschützte Web-API registrieren, konfigurieren und über eine Desktop-App aufrufen.

> [!div class="nextstepaction"]
> 