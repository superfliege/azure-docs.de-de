---
title: Erste Schritte mit benutzerdefinierten Richtlinien in Azure Active Directory B2C | Microsoft-Dokumentation
description: Enthält eine Beschreibung der ersten Schritte für benutzerdefinierte Azure Active Directory B2C-Richtlinien.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/17/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: b4ff8b607f9fded02a519b5f2a3abdfeedf93d88
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47181776"
---
# <a name="get-started-with-custom-policies-in-azure-active-directory-b2c"></a>Erste Schritte für benutzerdefinierte Richtlinien in Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

[Benutzerdefinierte Richtlinien](active-directory-b2c-overview-custom.md) sind Konfigurationsdateien, die das Verhalten Ihres Azure Active Directory B2C-Mandanten definieren. In diesem Artikel erfahren Sie, wie Sie eine benutzerdefinierte Richtlinie erstellen, die die Registrierung oder Anmeldung mit einem lokalen Konto über eine E-Mail-Adresse und ein Kennwort unterstützt. Zudem bereiten Sie Ihre Umgebung auf das Hinzufügen von Identitätsanbietern (z.B. Facebook oder Azure Active Directory) vor.

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie noch nicht über einen [Azure AD B2C-Mandanten](tutorial-create-tenant.md) verfügen, erstellen Sie einen, der mit Ihrem Azure-Abonnement verknüpft ist.

## <a name="add-signing-and-encryption-keys"></a>Hinzufügen von Signatur- und Verschlüsselungsschlüsseln

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als globaler Administrator Ihres Azure AD B2C-Mandanten an.
2. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält, indem Sie im oberen Menü auf den **Verzeichnis- und Abonnementfilter** klicken und das entsprechende Verzeichnis auswählen. 

    ![Wechseln zu Ihrem Azure AD B2C-Mandanten](./media/active-directory-b2c-setup-fb-app/switch-directories.png)

3. Klicken Sie links oben im Azure-Portal auf **Alle Dienste**, suchen Sie nach **Azure AD B2C**, und klicken Sie darauf.
4. Wählen Sie auf der Seite „Übersicht“ die Option **Identity Experience Framework – PREVIEW** (Framework für die Identitätsfunktion – VORSCHAU) aus.

### <a name="create-the-signing-key"></a>Erstellen des Signaturschlüssels

1. Klicken Sie erst auf **Richtlinienschlüssel** und anschließend auf **Hinzufügen**.
2. Klicken Sie unter **Optionen** auf `Generate`.
3. Geben Sie unter **Name** `TokenSigningKeyContainer` ein. Das Präfix `B2C_1A_` wird möglicherweise automatisch hinzugefügt.
4. Wählen Sie **RSA** als **Schlüsseltyp** aus.
5. Wählen Sie unter **Schlüsselverwendung** **Signatur** aus.
6. Klicken Sie auf **Create**.

### <a name="create-the-encryption-key"></a>Erstellen des Verschlüsselungsschlüssels

1. Klicken Sie erst auf **Richtlinienschlüssel** und anschließend auf **Hinzufügen**.
2. Klicken Sie unter **Optionen** auf `Generate`.
3. Geben Sie unter **Name** `TokenEncryptionKeyContainer` ein. Das Präfix `B2C_1A`_ wird möglicherweise automatisch hinzugefügt.
4. Wählen Sie **RSA** als **Schlüsseltyp** aus.
5. Wählen Sie für **Schlüsselverwendung** die Option **Verschlüsselung** aus.
6. Klicken Sie auf **Create**.

### <a name="create-the-facebook-key"></a>Erstellen des Facebook-Schlüssels

Wenn Sie bereits über ein [Facebook-Anwendungsgeheimnis](active-directory-b2c-setup-fb-app.md) verfügen, können Sie dieses Ihrem Mandanten als Richtlinienschlüssel hinzufügen. Andernfalls müssen Sie den Schlüssel mit einem Platzhalterwert erstellen, damit Ihre Richtlinien die Überprüfung bestehen.

1. Klicken Sie erst auf **Richtlinienschlüssel** und anschließend auf **Hinzufügen**.
2. Klicken Sie unter **Optionen** auf `Manual`.
3. Geben Sie unter **Name** `FacebookSecret` ein. Das Präfix `B2C_1A_` wird möglicherweise automatisch hinzugefügt.
4. Geben Sie unter **Geheimnis** Ihr Facebook-Geheimnis aus „developers.facebook.com“ oder `0` als Platzhalter ein. Es handelt sich dabei um das Geheimnis und nicht um die Anwendungs-ID.
5. Wählen Sie unter **Schlüsselverwendung** **Signatur** aus.
6. Klicken Sie auf **Create**.

## <a name="register-applications"></a>Registrieren von Anwendungen

Für Azure Active Directory B2C müssen Sie zwei Anwendungen registrieren, die zur Registrierung und Anmeldung von Benutzern verwendet werden: IdentityExperienceFramework (eine Web-App) und ProxyIdentityExperienceFramework (eine native App) mit delegierter Berechtigung für die IdentityExperienceFramework-App. Lokale Konten sind nur in Ihrem Mandanten vorhanden. Ihre Benutzer melden sich mit einer eindeutigen Kombination aus E-Mail-Adresse und Kennwort an, um auf Ihre bei Mandanten registrierten Anwendungen zuzugreifen.

### <a name="register-the-identityexperienceframework-application"></a>Registrieren der IdentityExperienceFramework-Anwendung

1. Klicken Sie links oben im Azure-Portal auf **Alle Dienste**, suchen Sie nach **Azure Active Directory**, klicken Sie auf diese Option, und klicken Sie anschließend auf **App-Registrierungen**.
2. Wählen Sie **Registrierung einer neuen Anwendung** aus.
3. Geben Sie unter **Name** `IdentityExperienceFramework` ein.
4. Wählen Sie unter **Anwendungstyp** die Option **Web-App/API** aus.
5. Geben Sie `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com` als **Anmelde-URL** ein, wobei `your-tenant-name` für den Domänennamen Ihres Azure AD B2C-Mandanten steht.
6. Klicken Sie auf **Create**. 
7. Wenn der Bestellvorgang abgeschlossen ist, kopieren Sie die Anwendungs-ID und speichern Sie sie zur späteren Verwendung.

### <a name="register-the-proxyidentityexperienceframework-application"></a>Registrieren der ProxyIdentityExperienceFramework-Anwendung

1. Klicken Sie erst auf **App-Registrierungen** und anschließend auf **New application registration** (Neue Anwendungsregistrierung).
2. Geben Sie unter **Name** `ProxyIdentityExperienceFramework` ein.
3. Wählen Sie **Nativ** als **Anwendungstyp** aus.
4. Geben Sie `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com` als **Umleitungs-URI** ein, wobei `yourtenant` für Ihren Azure AD B2C-Mandanten steht.
5. Klicken Sie auf **Create**. Wenn der Bestellvorgang abgeschlossen ist, kopieren Sie die Anwendungs-ID und speichern Sie sie zur späteren Verwendung.
6. Klicken Sie auf der Seite „Einstellungen“ zunächst auf **Erforderliche Berechtigungen** und dann auf **Hinzufügen**.
7. Wählen Sie **API auswählen** aus.
8. Suchen Sie **IdentityExperienceFramework**, und klicken Sie anschließend auf **Auswählen**.
9. Aktivieren Sie das Kontrollkästchen neben **Access IdentityExperienceFramework** (Auf IdentityExperienceFramework zugreifen), und klicken Sie erst auf **Auswählen** und dann auf **Done** (Fertig).
10. Wählen Sie **Berechtigungen erteilen** aus, und bestätigen Sie dies anschließend durch Auswahl von **Ja**.

## <a name="download-starter-pack-and-modify-policies"></a>Herunterladen des Starter Pack und Ändern von Richtlinien

Benutzerdefinierte Richtlinien sind ein Satz von XML-Dateien, die in Ihren Azure AD B2C-Mandanten hochgeladen werden müssen. Es sind Startpakete mit Dateien verfügbar, die Ihnen den schnellen Einstieg ermöglichen. Die einzelnen Starter Packs in der folgenden Liste enthalten die kleinste Anzahl von technischen Profilen sowie die zum Erreichen der beschriebenen Szenarios benötigten User Journeys:

- LocalAccounts: ermöglicht die ausschließliche Nutzung von lokalen Konten.
- SocialAccounts: ermöglicht die ausschließliche Nutzung von Konten sozialer Netzwerke (oder Verbundkonten).
- SocialAndLocalAccounts: ermöglicht sowohl die Verwendung von lokalen Konten als auch von Konten für soziale Netzwerke.
- SocialAndLocalAccountsWithMFA: ermöglicht lokale Optionen und Optionen für soziale Netzwerke sowie zur Multi-Factor Authentication.

Die einzelnen Starter Packs enthalten Folgendes:

- Die Basisdatei. Es sind einige Änderungen an der Basisdatei erforderlich.
* Die Erweiterungsdatei.  In dieser Datei werden die meisten Konfigurationsänderungen vorgenommen.
* Die Dateien der vertrauenden Seite. Aufgabenspezifische Dateien, die von Ihrer Anwendung aufgerufen werden.

>[!NOTE]
>Wenn Ihr XML-Editor Validierungen unterstützt, überprüfen Sie die Dateien anhand des XML-Schemas TrustFrameworkPolicy_0.3.0.0.xsd im Stammverzeichnis des Starter Packs . Durch die XML-Schemavalidierung werden vor dem Upload Fehler festgestellt.

1. [Laden Sie die ZIP-Datei herunter](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/archive/master.zip), oder gehen Sie wie folgt vor:

    ```console
    git clone https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack
    ```

2. Bearbeiten Sie im Ordner SocialAndLocalAccounts alle Dateien, indem Sie `yourtenant.onmicrosoft.com` durch den Namen Ihres Mandanten ersetzen. Beispiel: `contosoTenant.onmicrosoft.com`. Wenn Sie einen XML-Editor benötigen, können Sie [Visual Studio Code](https://code.visualstudio.com/download), einen einfachen plattformübergreifenden Editor, ausprobieren.

### <a name="add-application-ids-to-the-custom-policy"></a>Hinzufügen der Anwendungs-IDs zur benutzerdefinierten Richtlinie

Fügen Sie die Anwendung-ID zur Erweiterungsdatei *TrustFrameworkExtensions.xml* hinzu.

1. Öffnen Sie die Datei *TrustFrameworkExtensions.xml*, und suchen Sie das Element `<TechnicalProfile Id="login-NonInteractive">`.
2. Ersetzen Sie beide Instanzen von `IdentityExperienceFrameworkAppId` durch die Anwendungs-ID der Identity Experience Framework-Anwendung, die Sie zuvor erstellt haben. Ersetzen Sie beide Instanzen von `ProxyIdentityExperienceFrameworkAppId` durch die Anwendungs-ID der Anwendung des Frameworks für die Identitätsfunktion, die Sie zuvor erstellt haben. Im folgenden Beispiel sehen Sie, wie das technische Profil **login-NonInteractive** aussieht, nachdem die Änderungen vorgenommen wurden:

    ![Anwendungs-IDs](./media/active-directory-b2c-get-started-custom/login-NonInteractive.png)

3. Speichern Sie Ihre Erweiterungsdatei.

## <a name="upload-the-policies"></a>Hochladen der Richtlinien

1. Klicken Sie auf der Seite „Benutzerdefinierte Richtlinien“ des Frameworks für die Identitätsfunktion auf **Richtlinie hochladen**.
1. Laden Sie die folgenden Elemente in der angegebenen Reihenfolge hoch: *TrustFrameworkBase.xml*, *TrustFrameworkExtensions.xml*, *SignUpOrSignin.xml*, *ProfileEdit.xml* und *PasswordReset.xml*. Beim Hochladen einer Datei wird dem Namen der Richtliniendatei `B2C_1A_` vorangestellt.

## <a name="test-the-custom-policy"></a>Testen der benutzerdefinierten Richtlinie

1. Klicken Sie auf der Seite „Benutzerdefinierte Richtlinien“ auf **B2C_1A_signup_signin**. 
2. Wählen Sie **Jetzt ausführen** aus.

3. Sie sollten sich mit einer E-Mail-Adresse registrieren können.

4. Melden Sie sich zur Bestätigung der richtigen Konfiguration mit demselben Konto an.

## <a name="add-facebook-as-an-identity-provider"></a>Hinzufügen von Facebook als Identitätsanbieter

1. Konfigurieren Sie eine [Facebook-Anwendung](active-directory-b2c-setup-fb-app.md).
2. Ersetzen Sie in der Datei *TrustFrameworkExtensions.xml* den Wert `client_id` durch die ID der Facebook-Anwendung:

   ```xml
   <TechnicalProfile Id="Facebook-OAUTH">
     <Metadata>
     <!--Replace the value of client_id in this technical profile with the Facebook app ID"-->
       <Item Key="client_id">00000000000000</Item>
   ```
3. Laden Sie die Datei *TrustFrameworkExtensions.xml* in Ihren Mandanten hoch.
4. Führen Sie einen Test durch, indem Sie **Jetzt ausführen** verwenden, oder rufen Sie die Richtlinie direkt über Ihre registrierte Anwendung auf.

## <a name="next-steps"></a>Nächste Schritte

- Fügen Sie Azure Active Directory als Identitätsanbieter hinzu. Die in diesem Leitfaden für erste Schritte verwendete Basisdatei enthält bereits einige Inhalte, die Sie zum Hinzufügen anderer Identitätsanbieter benötigen. Weitere Informationen zur Einrichtung von Anmeldungen finden Sie unter [Set up sign-up and sign-in with an Azure Active Directory account using Active Directory B2C custom policies (Einrichten von Anmeldungen bei und Registrierungen von Azure Active Directory-Konten mithilfe von benutzerdefinierten Richtlinien für Azure Active Directory B2C)](active-directory-b2c-setup-aad-custom.md).
