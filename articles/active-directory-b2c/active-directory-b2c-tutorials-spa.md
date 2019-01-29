---
title: 'Tutorial: Aktivieren der Authentifizierung einseitiger Apps mit Konten unter Verwendung von Azure Active Directory B2C | Microsoft-Dokumentation'
description: Tutorial zur Verwendung von Azure Active Directory B2C zum Bereitstellen einer Benutzeranmeldung für eine einseitige Anwendung (JavaScript)
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.author: davidmu
ms.date: 11/30/2018
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.component: B2C
ms.openlocfilehash: 08372700e9740ca69ba73dfba49f9d120dfabc6d
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2019
ms.locfileid: "54850330"
---
# <a name="tutorial-enable-single-page-app-authentication-with-accounts-using-azure-active-directory-b2c"></a>Tutorial: Aktivieren der Authentifizierung einseitiger Apps mit Konten unter Verwendung von Azure Active Directory B2C

In diesem Tutorial erfahren Sie, wie Sie Azure Active Directory (Azure AD) B2C für die Anmeldung und Registrierung von Benutzern in einer einseitigen App (Single-Page App, SPA) verwenden. Mit Azure AD B2C können sich Ihre Apps über offene Standardprotokolle bei Konten für soziale Netzwerke sowie bei Unternehmenskonten und Azure Active Directory-Konten authentifizieren.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Registrieren einer einseitigen Beispiel-App in Ihrem Azure AD B2C-Verzeichnis
> * Erstellen von Benutzerflows für Benutzerregistrierung, Benutzeranmeldung, Profilbearbeitung und Kennwortzurücksetzung
> * Konfigurieren der Beispielanwendung für die Verwendung Ihres Azure AD B2C-Verzeichnisses

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

* Erstellen Sie Ihr eigenes [Azure AD B2C-Verzeichnis](active-directory-b2c-get-started.md).
* Installieren Sie [Visual Studio 2017](https://www.visualstudio.com/downloads/) mit der Workload **ASP.NET und Webentwicklung**.
* [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) oder eine höhere Version
* Installieren von [Node.js](https://nodejs.org/en/download/)

## <a name="register-single-page-app"></a>Registrieren der einseitigen App

Anwendungen müssen in Ihrem Verzeichnis [registriert](../active-directory/develop/developer-glossary.md#application-registration) werden, um [Zugriffstoken](../active-directory/develop/developer-glossary.md#access-token) aus Azure Active Directory erhalten zu können. Bei der App-Registrierung wird für die App eine [Anwendungs-ID](../active-directory/develop/developer-glossary.md#application-id-client-id) in Ihrem Verzeichnis erstellt. 

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als globaler Administrator Ihres Azure AD B2C-Verzeichnisses an.

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

1. Wählen Sie **Azure AD B2C** aus der Dienstliste im Azure-Portal aus. 

2. Klicken Sie in den B2C-Einstellungen auf **Anwendungen** und anschließend auf **Hinzufügen**. 

    Verwenden Sie die folgenden Einstellungen, um die Beispiel-Web-App in Ihrem Verzeichnis zu registrieren:
    
    ![Hinzufügen einer neuen App](media/active-directory-b2c-tutorials-spa/spa-registration.png)
    
    | Einstellung      | Empfohlener Wert  | Beschreibung                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Name** | My sample single page app | Geben Sie einen aussagekräftigen **App-Namen**ein. | 
    | **Web-App/Web-API einschließen** | JA | Wählen Sie bei einer einseitigen App **Ja** aus. |
    | **Impliziten Fluss zulassen** | JA | Diese App verwendet die [OpenID Connect-Anmeldung](active-directory-b2c-reference-oidc.md). Wählen Sie daher **Ja** aus. |
    | **Antwort-URL** | `http://localhost:6420` | Antwort-URLs sind Endpunkte, an denen Azure AD B2C von Ihrer App angeforderte Token zurückgibt. Das Beispiel in diesem Tutorial wird lokal (localhost) ausgeführt und lauscht am Port 6420. |
    | **Nativen Client einschließen** | Nein  | Da es sich hierbei um eine einseitige App und nicht um einen nativen Client handelt, wählen Sie „Nein“ aus. |
    
3. Klicken Sie auf **Erstellen**, um Ihre App zu registrieren.

Registrierte Apps werden in der Anwendungsliste für das Azure AD B2C-Verzeichnis angezeigt. Wählen Sie Ihre einseitige App aus der Liste aus. Der Eigenschaftenbereich der registrierten einseitigen App wird angezeigt.

![Eigenschaften der einseitigen App](./media/active-directory-b2c-tutorials-spa/b2c-spa-properties.png)

Notieren Sie sich den Wert der **Anwendungsclient-ID**. Die ID identifiziert die App eindeutig und wird im weiteren Verlauf des Tutorials beim Konfigurieren der App benötigt.

## <a name="create-user-flows"></a>Erstellen von Benutzerflows

Ein Azure AD B2C-Benutzerflow definiert die Umgebung für eine Identitätsaufgabe. Gängige Benutzerflows sind beispielsweise Anmelden, Registrieren, Ändern von Kennwörtern und Bearbeiten von Profilen.

### <a name="create-a-sign-up-or-sign-in-user-flow"></a>Erstellen eines Registrierungs- oder Anmeldebenutzerflows

Erstellen Sie einen **Registrierungs- oder Anmeldebenutzerflow**, um Benutzer für den Zugriff und die anschließende Anmeldung zu registrieren.

1. Wählen Sie auf der Azure AD B2C-Portalseite die Option **Benutzerflows** aus, und klicken Sie auf **Neuer Benutzerflow**.
2. Klicken Sie auf der Registerkarte **Empfohlen** auf **Registrierung und Anmeldung**.

    Konfigurieren Sie Ihren Benutzerflow mit den folgenden Einstellungen:

    ![Hinzufügen eines Registrierungs- oder Anmeldebenutzerflows](media/active-directory-b2c-tutorials-spa/add-susi-user-flow.png)

    | Einstellung      | Empfohlener Wert  | Beschreibung                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Name** | SiUpIn | Geben Sie unter **Name** einen Namen für den Benutzerflow ein. Der Name des Benutzerflows wird mit dem Präfix **B2C_1_** versehen. Im Beispielcode wird der vollständige Benutzerflowname **B2C_1_SiUpIn** verwendet. | 
    | **Identitätsanbieter** | E-Mail-Registrierung | Der Identitätsanbieter zur eindeutigen Identifizierung des Benutzers. |

3. Klicken Sie unter **Benutzerattribute und Ansprüche** auf **Mehr anzeigen**, und wählen Sie die folgenden Einstellungen aus:

    ![Hinzufügen eines Registrierungs- oder Anmeldebenutzerflows](media/active-directory-b2c-tutorials-spa/add-attributes-and-claims.png)

    | Column      | Empfohlener Wert  | BESCHREIBUNG                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Attribut sammeln** | „Anzeigename“ und „Postleitzahl“ | Wählen Sie Attribute aus, die im Rahmen der Benutzerregistrierung erfasst werden sollen. |
    | **Anspruch zurückgeben** | „Anzeigename“, „Postleitzahl“, „User is new“ (Benutzer ist neu), „User's Object ID“ (Objekt-ID des Benutzers) | Wählen Sie [Ansprüche](../active-directory/develop/developer-glossary.md#claim) aus, die im [Zugriffstoken](../active-directory/develop/developer-glossary.md#access-token) enthalten sein sollen. |

4. Klicken Sie auf **OK**.
5. Klicken Sie auf **Erstellen**, um den Benutzerflow zu erstellen. 

### <a name="create-a-profile-editing-user-flow"></a>Erstellen eines Benutzerflows für die Profilbearbeitung

Erstellen Sie einen **Benutzerflow für die Profilbearbeitung**, um Benutzern das eigenständige Zurücksetzen ihrer Benutzerprofilinformationen zu ermöglichen.

1. Wählen Sie auf der Azure AD B2C-Portalseite die Option **Benutzerflows** aus, und klicken Sie auf **Neuer Benutzerflow**.
2. Klicken Sie auf der Registerkarte **Empfohlen** auf **Profilbearbeitung**.

    Konfigurieren Sie Ihren Benutzerflow mit den folgenden Einstellungen:

    | Einstellung      | Empfohlener Wert  | Beschreibung                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Name** | SiPe | Geben Sie unter **Name** einen Namen für den Benutzerflow ein. Der Name des Benutzerflows wird mit dem Präfix **B2C_1_** versehen. Im Beispielcode wird der vollständige Benutzerflowname **B2C_1_SiPe** verwendet. | 
    | **Identitätsanbieter** | Anmeldung mit lokalem Konto | Der Identitätsanbieter zur eindeutigen Identifizierung des Benutzers. |

3.  Klicken Sie unter **Benutzerattribute** auf **Mehr anzeigen**, und wählen Sie die folgenden Einstellungen aus:

    | Column      | Empfohlener Wert  | BESCHREIBUNG                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Attribut sammeln** | „Anzeigename“ und „Postleitzahl“ | Wählen Sie Attribute aus, die Benutzer bei der Profilbearbeitung ändern können. |
    | **Anspruch zurückgeben** | „Anzeigename“, „Postleitzahl“, „User's Object ID“ (Objekt-ID des Benutzers) | Wählen Sie [Ansprüche](../active-directory/develop/developer-glossary.md#claim) aus, die nach erfolgreicher Profilbearbeitung im [Zugriffstoken](../active-directory/develop/developer-glossary.md#access-token) enthalten sein sollen. |

4. Klicken Sie auf **OK**.
5. Klicken Sie auf **Erstellen**, um den Benutzerflow zu erstellen. 

### <a name="create-a-password-reset-user-flow"></a>Erstellen eines Benutzerflows zur Kennwortrücksetzung

Wenn Sie in Ihrer Anwendung das Zurücksetzen des Kennworts ermöglichen möchten, müssen Sie einen **Benutzerflow für die Kennwortzurücksetzung** erstellen. Dieser Benutzerflow beschreibt die Kundenerfahrung während der Kennwortzurücksetzung und den Inhalt der Token, die die Anwendung bei erfolgreichem Abschluss erhält.

1. Wählen Sie auf der Azure AD B2C-Portalseite die Option **Benutzerflows** aus, und klicken Sie auf **Neuer Benutzerflow**.
2. Klicken Sie auf der Registerkarte **Empfohlen** auf **Kennwortzurücksetzung**.

    Konfigurieren Sie Ihren Benutzerflow mit den folgenden Einstellungen:

    | Einstellung      | Empfohlener Wert  | Beschreibung                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Name** | SSPR | Geben Sie unter **Name** einen Namen für den Benutzerflow ein. Der Name des Benutzerflows wird mit dem Präfix **B2C_1_** versehen. Im Beispielcode wird der vollständige Benutzerflowname **B2C_1_SSPR** verwendet. | 
    | **Identitätsanbieter** | Kennwort mittels E-Mail-Adresse zurücksetzen | Dies ist der Identitätsanbieter zur eindeutigen Identifizierung des Benutzers. |

3. Klicken Sie unter **Anwendungsansprüche** auf **Mehr anzeigen**, und wählen Sie die folgenden Einstellungen aus:

    | Column      | Empfohlener Wert  | BESCHREIBUNG                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Anspruch zurückgeben** | User's Object ID (Objekt-ID des Benutzers) | Wählen Sie [Ansprüche](../active-directory/develop/developer-glossary.md#claim) aus, die nach erfolgreicher Kennwortzurücksetzung im [Zugriffstoken](../active-directory/develop/developer-glossary.md#access-token) enthalten sein sollen. |

4. Klicken Sie auf **OK**.
5. Klicken Sie auf **Erstellen**, um den Benutzerflow zu erstellen. 

## <a name="update-single-page-app-code"></a>Aktualisieren des Codes der einseitigen App

Nachdem Sie eine App registriert und Benutzerflows erstellt haben, müssen Sie Ihre App für die Verwendung Ihres Azure AD B2C-Verzeichnisses konfigurieren. In diesem Tutorial konfigurieren Sie ein Beispiel für eine einseitige JavaScript-App, die Sie von GitHub herunterladen können. 

[Laden Sie eine ZIP-Datei herunter](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp/archive/master.zip), oder klonen Sie die Beispiel-Web-App aus GitHub.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp.git
```
Die Beispiel-App veranschaulicht, wie eine einseitige App Azure AD B2C für die Benutzerregistrierung und -anmeldung sowie zum Aufrufen einer geschützten Web-API verwenden kann. Sie müssen die App ändern, damit sie die App-Registrierung in Ihrem Verzeichnis verwendet, und die von Ihnen erstellten Benutzerflows konfigurieren. 

Gehen Sie zum Ändern der App-Einstellungen wie folgt vor:

1. Öffnen Sie die Datei `index.html` im Beispiel für die einseitige Node.js-App.
2. Konfigurieren Sie das Beispiel mit den Registrierungsinformationen für das Azure AD B2C-Verzeichnis. Ändern Sie die folgenden Codezeilen. (Ersetzen Sie dabei unbedingt die Werte durch die Namen Ihres Verzeichnisses und Ihre APIs.)

    ```javascript
    // The current application coordinates were pre-registered in a B2C directory.
    var applicationConfig = {
        clientID: '<Application ID for your SPA obtained from portal app registration>',
        authority: "https://fabrikamb2c.b2clogin.com/tfp/fabrikamb2c.onmicrosoft.com/B2C_1_<Sign-up or sign-in policy name>",
        b2cScopes: ["https://fabrikamb2c.onmicrosoft.com/demoapi/demo.read"],
        webApi: 'https://fabrikamb2chello.azurewebsites.net/hello',
    };
    ```

    Der in diesem Tutorial verwendete Benutzerflowname lautet **B2C_1_SiUpIn**. Verwenden Sie bei einem anderen Benutzerflownamen den Benutzerflownamen im Wert `authority`.

## <a name="run-the-sample"></a>Ausführen des Beispiels

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

Die Beispiel-App unterstützt Registrierung, Anmeldung, Profilbearbeitung und Kennwortzurücksetzung. In diesem Tutorial wird beschrieben, wie ein Benutzer sich mit einer E-Mail-Adresse für die Nutzung der App anmeldet. Die anderen Szenarien können Sie selbstständig erkunden.

### <a name="sign-up-using-an-email-address"></a>Registrieren mit einer E-Mail-Adresse

1. Klicken Sie auf **Anmelden**, um sich als Benutzer der SPA-App zu registrieren. Hierbei wird der zuvor definierte Benutzerflow **B2C_1_SiUpIn** verwendet.

2. Azure AD B2C zeigt eine Anmeldeseite mit einem Registrierungslink an. Da Sie noch nicht über ein Konto verfügen, klicken Sie auf den Link **Jetzt registrieren**. 

3. Der Registrierungsworkflow zeigt eine Seite an, über die die Identität des Benutzers mithilfe einer E-Mail-Adresse erfasst und überprüft wird. Darüber hinaus werden im Rahmen des Registrierungsworkflows auch das Kennwort des Benutzers sowie die angeforderten Attribute erfasst, die im Benutzerflow definiert wurden.

    Verwenden Sie eine gültige E-Mail-Adresse, und bestätigen Sie sie mithilfe des Prüfcodes. Legen Sie ein Kennwort fest. Geben Sie Werte für die angeforderten Attribute ein. 

    ![Registrierungsworkflow](media/active-directory-b2c-tutorials-desktop-app/sign-up-workflow.png)

4. Klicken Sie auf **Erstellen**, um im Azure AD B2C-Verzeichnis ein lokales Konto zu erstellen.

Nun kann sich der Benutzer mithilfe seiner E-Mail-Adresse anmelden und die SPA-App verwenden.

> [!NOTE]
> Nach der Anmeldung wird von der App der Fehler „Nicht genügend Berechtigungen“ angezeigt. Der Fehler wird angezeigt, da Sie versuchen, über das Demoverzeichnis auf eine Ressource zuzugreifen. Da Ihr Zugriffstoken nur für Ihr Azure AD-Verzeichnis gilt, ist der API-Aufruf nicht autorisiert. Fahren Sie mit dem nächsten Tutorial fort, um eine geschützte Web-API für Ihr Verzeichnis zu erstellen. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Sie können Ihr Azure AD B2C-Verzeichnis für weitere Azure AD B2C-Tutorials verwenden. Wenn Sie es nicht mehr benötigt, können Sie [Ihr Azure AD B2C-Verzeichnis löschen](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie ein Azure AD B2C-Verzeichnis und Benutzerflows erstellen und wie Sie die einseitige Beispiel-App für die Verwendung Ihres Azure AD B2C-Verzeichnisses aktualisieren. Im nächsten Tutorial erfahren Sie, wie Sie eine geschützte Web-API registrieren, konfigurieren und über eine Desktop-App aufrufen.

> [!div class="nextstepaction"]
> [Azure-Codebeispiele](https://azure.microsoft.com/resources/samples/?service=active-directory-b2c&sort=0)
