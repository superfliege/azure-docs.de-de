---
title: "Azure Active Directory B2C: Fügen Sie hinzu + Google als Identitätsanbieter \"oauth2\" Verwenden benutzerdefinierter Richtlinien"
description: "Beispiel mit Google + als Identitätsanbieter mithilfe von OAuth2-Protokoll"
services: active-directory-b2c
documentationcenter: 
author: yoelhor
manager: joroja
editor: 
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 08/04/2017
ms.author: yoelh
ms.openlocfilehash: e0aaf710d230f7667fff32b50ddb64104509d740
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2017
---
# <a name="azure-active-directory-b2c-add-google-as-an-oauth2-identity-provider-using-custom-policies"></a>Azure Active Directory B2C: Fügen Sie hinzu + Google als Identitätsanbieter "oauth2" Verwenden benutzerdefinierter Richtlinien

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Erfahren Sie, wie die Anmeldung für Benutzer von Google +-Konto mithilfe des aktiviert [benutzerdefinierte Richtlinien](active-directory-b2c-overview-custom.md).

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie die Schritte im Artikel [Erste Schritte mit benutzerdefinierten Richtlinien](active-directory-b2c-get-started-custom.md) aus.

Diese Schritte umfassen:

1.  Erstellen einer Google-Konto-Anwendung.
2.  Hinzufügen von Google +-Konto Schlüssels der Anwendung zu Azure AD B2C
3.  Hinzufügen von Anspruchsanbieter für eine Richtlinie
4.  Registrieren den Google +-Konto Anspruchsanbieter für einen Benutzer weg
5.  Hochladen von der Richtlinie in eine Azure AD B2C-Mandanten und testen

## <a name="create-a-google-account-application"></a>Erstellen einer Google-Konto-Anwendung
Um Google+ als Identitätsanbieter in Azure Active Directory (Azure AD) B2C verwenden zu können, müssen Sie eine Google+-Anwendung erstellen und die entsprechenden Parameter bereitstellen. Sie können eine Google +-Anwendung hier registrieren: [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp)

1.  Klicken Sie auf die [Google Developers Console](https://console.developers.google.com/), und melden Sie sich mit Ihren Anmeldeinformationen für Google+ an.
2.  Klicken Sie auf **Projekt erstellen**, geben Sie unter **Projektname** einen Projektnamen ein, und klicken Sie auf **Erstellen**.

3.  Klicken Sie auf die **Menü Projekte**.

    ![Google +-Konto – Select-Projekt](media/active-directory-b2c-custom-setup-goog-idp/goog-add-new-app1.png)

4.  Klicken Sie auf die  **+**  Schaltfläche.

    ![Google +-Konto – neues Projekt erstellen](media/active-directory-b2c-custom-setup-goog-idp//goog-add-new-app2.png)

5.  Geben Sie einen **Projektname**, und klicken Sie dann auf **erstellen**.

    ![Google +-Konto – neues Projekt](media/active-directory-b2c-custom-setup-goog-idp//goog-app-name.png)

6.  Warten Sie, bis das Projekt bereit ist, und klicken Sie auf die **Menü Projekte**.

    ![Google +-Konto – warten, bis ein neues Projekt zur Verwendung bereit ist.](media/active-directory-b2c-custom-setup-goog-idp//goog-select-app1.png)

7.  Klicken Sie auf den Projektnamen.

    ![Google +-Konto – wählen Sie das neue Projekt](media/active-directory-b2c-custom-setup-goog-idp//goog-select-app2.png)

8.  Klicken Sie links im Navigationsbereich auf **API Manager** und dann auf **Zugangsdaten**.
9.  Klicken Sie oben auf die Registerkarte **OAuth-Zustimmungsbildschirm**.

    ![Google +-Konto – legen Sie OAuth-Zustimmung-Bildschirm](media/active-directory-b2c-custom-setup-goog-idp/goog-add-cred.png)

10.  Wählen Sie unter **E-Mail-Adresse** eine gültige Adresse aus, oder geben Sie sie ein, geben Sie einen Wert für **Produktname** ein, und klicken Sie auf **Speichern**.

    ![Google + - Anwendungsanmeldeinformationen](media/active-directory-b2c-custom-setup-goog-idp/goog-consent-screen.png)

11.  Klicken Sie auf **Anmeldeinformationen hinzufügen**, und wählen Sie dann **OAuth-Client-ID** aus.

    ![Google + - Anwendungsanmeldeinformationen für die neue erstellen](media/active-directory-b2c-custom-setup-goog-idp/goog-add-oauth2-client-id.png)

12.  Wählen Sie unter **Anwendungstyp** die Option **Webanwendung** aus.

    ![Google + - Anwendungsauswahl-Typ](media/active-directory-b2c-custom-setup-goog-idp/goog-web-app.png)

13.  Geben Sie einen **Namen** für die Anwendung an, und geben Sie `https://login.microsoftonline.com` im Feld **Autorisierte JavaScript-Quellen** und `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` im Feld **Autorisierte Weiterleitungs-URIs** ein. Ersetzen Sie **{tenant}** durch den Namen Ihres Mandanten (z.B. „contosob2c.onmicrosoft.com“). Beim Wert für **{tenant}** wird die Groß-/Kleinschreibung beachtet. Klicken Sie auf **Erstellen**.

    ![Google + - autorisierte JavaScript-Ursprünge bereitstellen und die umleitungs-URIs](media/active-directory-b2c-custom-setup-goog-idp/goog-create-client-id.png)

14.  Kopieren Sie die Werte der **Clientkennung** und **clientgeheimnis**. Sie benötigen beide, Google als Identitätsanbieter in Ihrem Mandanten konfigurieren. Das **Clientgeheimnis** ist eine wichtige Anmeldeinformation.

    ![Google + - kopieren Sie die Werte des Client-Id und des geheimen Clientschlüssels](media/active-directory-b2c-custom-setup-goog-idp/goog-client-secret.png)

## <a name="add-the-google-account-application-key-to-azure-ad-b2c"></a>Azure AD B2C Google +-Anwendung kontoschlüssel hinzufügen
Verbund mit Google +-Konten erfordert einen geheimen Clientschlüssel für Google +-Konto Trust Azure AD B2C für die Anwendung. In diesem Fall müssen Sie eine geheime Google +-Anwendung in Azure AD B2C-Mandanten zu speichern:  

1.  Wechseln Sie zu Ihrem Azure AD B2C-Mandanten, und wählen Sie **B2C Einstellungen** > **Identität Erfahrung Framework**
2.  Wählen Sie **Policy Keys** (Richtlinienschlüssel), um die in Ihrem Mandanten verfügbaren Schlüssel anzuzeigen.
3.  Klicken Sie auf **+ hinzufügen**.
4.  Verwenden Sie für **Optionen** die Option **Manuell**.
5.  Verwenden Sie für **Name** den Wert `GoogleSecret`.  
    Das Präfix `B2C_1A_` wird möglicherweise automatisch hinzugefügt.
6.  In der **geheimen** Geben Sie Ihr Microsoft-anwendungsgeheimnis aus https://apps.dev.microsoft.com
7.  Verwenden Sie für **Schlüsselverwendung**, die Option **Signatur**.
8.  Klicken Sie auf **Erstellen**
9.  Vergewissern Sie sich, dass Sie den Schlüssel `B2C_1A_GoogleSecret` erstellt haben.

## <a name="add-a-claims-provider-in-your-extension-policy"></a>Fügen Sie einem Anspruchsanbieter in Ihrer Richtlinie Erweiterung hinzu.

Wenn Sie Benutzer mithilfe von Google +-Konto anmelden möchten, müssen Sie Google +-Konto als Anspruchsanbieter definieren. Das heißt, müssen Sie einen Endpunkt angeben, dem mit Azure AD B2C kommuniziert. Der Endpunkt stellt einen Satz von Ansprüchen, die von Azure AD B2C verwendet werden, um sicherzustellen, dass ein bestimmter Benutzer authentifiziert hat.

Definieren von Google +-Konto als Anspruchsanbieter, durch Hinzufügen von `<ClaimsProvider>` Knoten in der Erweiterung Richtliniendatei:

1.  Öffnen Sie die Richtliniendatei für die Erweiterung (TrustFrameworkExtensions.xml) aus Ihrem Arbeitsverzeichnis. Wenn Sie einen XML-Editor benötigen, können Sie [Visual Studio Code](https://code.visualstudio.com/download), einen einfachen plattformübergreifenden Editor, ausprobieren.
2.  Suchen der `<ClaimsProviders>` Abschnitt
3.  Fügen Sie die folgenden XML-Ausschnitt unter der `ClaimsProviders` Element, und Ersetzen `client_id` Wert mit Google +-Anwendung Client-ID Ihres Kontos vor dem Speichern der Datei.  

```xml
<ClaimsProvider>
    <Domain>google.com</Domain>
    <DisplayName>Google</DisplayName>
    <TechnicalProfiles>
    <TechnicalProfile Id="Google-OAUTH">
        <DisplayName>Google</DisplayName>
        <Protocol Name="OAuth2" />
        <Metadata>
        <Item Key="ProviderName">google</Item>
        <Item Key="authorization_endpoint">https://accounts.google.com/o/oauth2/auth</Item>
        <Item Key="AccessTokenEndpoint">https://accounts.google.com/o/oauth2/token</Item>
        <Item Key="ClaimsEndpoint">https://www.googleapis.com/oauth2/v1/userinfo</Item>
        <Item Key="scope">email</Item>
        <Item Key="HttpBinding">POST</Item>
        <Item Key="UsePolicyInRedirectUri">0</Item>
        <Item Key="client_id">Your Google+ application ID</Item>
        </Metadata>
        <CryptographicKeys>
        <Key Id="client_secret" StorageReferenceId="B2C_1A_GoogleSecret" />
        </CryptographicKeys>
        <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="id" />
        <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email" />
        <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
        <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="family_name" />
        <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
        <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="google.com" />
        <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
        </OutputClaims>
        <OutputClaimsTransformations>
        <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName" />
        <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName" />
        <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId" />
        <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId" />
        </OutputClaimsTransformations>
        <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin" />
        <ErrorHandlers>
        <ErrorHandler>
            <ErrorResponseFormat>json</ErrorResponseFormat>
            <ResponseMatch>$[?(@@.error == 'invalid_grant')]</ResponseMatch>
            <Action>Reauthenticate</Action>
            <!--In case of authorization code used error, we don't want the user to select his account again.-->
            <!--AdditionalRequestParameters Key="prompt">select_account</AdditionalRequestParameters-->
        </ErrorHandler>
        </ErrorHandlers>
    </TechnicalProfile>
    </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="register-the-google-account-claims-provider-to-sign-up-or-sign-in-user-journey"></a>Registrieren von Google +-Konto Anspruchsanbieter zum Registrieren oder Anmelden bei Benutzer weg

Der Identitätsanbieter wurde eingerichtet.  Es ist jedoch nicht in den Bildschirmen der Registrierung-Einrichtung /-Anmeldung verfügbar. Fügen Sie den Google +-Konto-Identitätsanbieter, die für Benutzer `SignUpOrSignIn` Benutzer weg. Um verfügbar zu machen, erstellen Sie ein Duplikat einer vorhandenen Vorlage Benutzer weg.  Wir fügen Sie dann den Google +-Konto-Identitätsanbieter hinzu:

>[!NOTE]
>
>Wenn Sie kopiert die `<UserJourneys>` Element aus der Basisdatei Ihrer Richtlinie auf die Erweiterungsdatei (TrustFrameworkExtensions.xml), können Sie zu diesem Abschnitt überspringen.

1.  Öffnen Sie die Basisdatei Ihrer Richtlinie (z.B. „TrustFrameworkBase.xml“).
2.  Suchen der `<UserJourneys>` Element, und kopieren Sie den gesamten Inhalt des `<UserJourneys>` Knoten.
3.  Öffnen Sie die Erweiterungsdatei (z.B. „TrustFrameworkExtensions.xml“), und suchen Sie nach dem Element `<UserJourneys>`. Wenn das Element nicht vorhanden ist, fügen Sie ein solches hinzu.
4.  Fügen Sie den gesamten Inhalt des `<UserJournesy>` Knoten, die Sie als ein untergeordnetes Element des kopiert die `<UserJourneys>` Element.

### <a name="display-the-button"></a>Zeigen Sie die Schaltfläche ""
Die `<ClaimsProviderSelections>` -Element definiert die Liste der Ansprüche Auswahl Anbieteroptionen und deren Reihenfolge.  `<ClaimsProviderSelection>`Element ist analog zu einer Identity Provider-Schaltfläche auf eine Registrierung-Einrichtung/Anmeldeseite. Wenn Sie beim Hinzufügen einer `<ClaimsProviderSelection>` Element für Google +-Konto, eine Schaltfläche "Neu" angezeigt wird, wenn ein Benutzer auf der Seite kommt. So fügen Sie dieses Element hinzu:

1.  Suchen der `<UserJourney>` Knoten, der enthält `Id="SignUpOrSignIn"` in der Benutzer weg, den Sie kopiert haben.
2.  Suchen Sie die `<OrchestrationStep>` Knoten, der enthält`Order="1"`
3.  Fügen Sie folgenden XML-Ausschnitt unter `<ClaimsProviderSelections>` Knoten:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="GoogleExchange" />
```

### <a name="link-the-button-to-an-action"></a>Verknüpfen der Schaltfläche mit einer Aktion
Nachdem Sie eine Schaltfläche implementiert haben, müssen Sie sie mit einer Aktion verknüpfen. Die Aktion ist in diesem Fall für Azure AD B2C für die Kommunikation mit Google +-Konto zum Empfang eines Tokens.

1.  Suchen Sie `<OrchestrationStep>`, das `Order="2"` im `<UserJourney>`-Knoten enthält.
2.  Fügen Sie folgenden XML-Ausschnitt unter `<ClaimsExchanges>` Knoten:

```xml
<ClaimsExchange Id="GoogleExchange" TechnicalProfileReferenceId="Google-OAUTH" />
```

>[!NOTE]
>
> * Sicherstellen der `Id` hat den gleichen Wert wie der `TargetClaimsExchangeId` im vorherigen Abschnitt
> * Stellen Sie sicher `TechnicalProfileReferenceId` -ID festgelegt technische Profil erstellten früheren (Google OAUTH).

## <a name="upload-the-policy-to-your-tenant"></a>Die Richtlinie für Ihren Mandanten hochladen
1.  Wechseln Sie im [Azure-Portal](https://portal.azure.com) in den [Kontext Ihres Azure AD B2C-Mandanten](active-directory-b2c-navigate-to-b2c-context.md), und öffnen Sie das Blatt **Azure AD B2C**.
2.  Wählen Sie **Framework für die Identitätsfunktion** aus.
3.  Öffnen der **alle Richtlinien** Blatt.
4.  Wählen Sie **hochladen Richtlinie**.
5.  Überprüfen Sie **überschreiben Sie die Richtlinie aus, falls vorhanden** Feld.
6.  **Hochladen** TrustFrameworkExtensions.xml und stellen Sie sicher, dass sie nicht die Überprüfung fehlschlägt

## <a name="test-the-custom-policy-by-using-run-now"></a>Testen der benutzerdefinierten Richtlinie mit „Jetzt ausführen“
1.  Öffnen Sie die **Azure AD B2C-Einstellungen**, und navigieren Sie zu **Framework für die Identitätsfunktion**.

    >[!NOTE]
    >
    >    **Jetzt ausführen** macht erforderlich, dass mindestens eine Anwendung im Mandanten vorab registriert wird. 
    >    Informationen zum Registrieren von Anwendungen finden Sie in den Artikeln [Erste Schritte](active-directory-b2c-get-started.md) bzw. [Anwendungsregistrierung](active-directory-b2c-app-registration.md) zu Azure AD B2C.


2.  Open **B2C_1A_signup_signin**, die vertrauenden Seite Partei (RP) benutzerdefinierte Richtlinie, die Sie hochgeladen haben. Wählen Sie **Jetzt ausführen** aus.
3.  Sie sollten sich mit Google +-Konto anmelden können.

## <a name="optional-register-the-google-account-claims-provider-to-profile-edit-user-journey"></a>[Optional] Registrieren Sie den Google +-Konto Anspruchsanbieter für die Benutzer Weg Profil bearbeiten
Sie möchten den Google +-Konto-Identitätsanbieter auch Ihre Benutzer hinzufügen `ProfileEdit` Benutzer weg. Um verfügbar zu machen, wiederholen wir die letzten beiden Schritte aus:

### <a name="display-the-button"></a>Zeigen Sie die Schaltfläche ""
1.  Öffnen Sie die Erweiterungsdatei Ihrer Richtlinie (z. B. TrustFrameworkExtensions.xml).
2.  Suchen der `<UserJourney>` Knoten, der enthält `Id="ProfileEdit"` in der Benutzer weg, den Sie kopiert haben.
3.  Suchen Sie die `<OrchestrationStep>` Knoten, der enthält`Order="1"`
4.  Fügen Sie folgenden XML-Ausschnitt unter `<ClaimsProviderSelections>` Knoten:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="GoogleExchange" />
```

### <a name="link-the-button-to-an-action"></a>Verknüpfen der Schaltfläche mit einer Aktion
1.  Suchen Sie `<OrchestrationStep>`, das `Order="2"` im `<UserJourney>`-Knoten enthält.
2.  Fügen Sie folgenden XML-Ausschnitt unter `<ClaimsExchanges>` Knoten:

```xml
<ClaimsExchange Id="GoogleExchange" TechnicalProfileReferenceId="Google-OAUTH" />
```

### <a name="test-the-custom-profile-edit-policy-by-using-run-now"></a>Testen Sie die benutzerdefinierte Richtlinie ein Profil bearbeiten, indem jetzt ausführen

1.  Öffnen Sie die **Azure AD B2C-Einstellungen**, und navigieren Sie zu **Framework für die Identitätsfunktion**.
2.  Open **B2C_1A_ProfileEdit**, die vertrauenden Seite Partei (RP) benutzerdefinierte Richtlinie, die Sie hochgeladen haben. Wählen Sie **Jetzt ausführen** aus.
3.  Sie sollten sich mit Google +-Konto anmelden können.

## <a name="download-the-complete-policy-files"></a>Die vollständige Richtliniendateien herunterladen
Optional: Es wird empfohlen, dass Sie Ihr Szenario verwenden eine eigene benutzerdefinierte Richtlinie, die Dateien nach Abschluss der erste Schritte mit benutzerdefinierten Richtlinien durchlaufen anstatt diese Beispieldateien erstellen.  [Beispiel-Richtliniendateien Referenz](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-goog-app)
