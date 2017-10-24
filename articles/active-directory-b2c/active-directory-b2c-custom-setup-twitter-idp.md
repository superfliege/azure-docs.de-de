---
title: "Azure Active Directory B2C: Hinzufügen von Twitter als ein OAuth1-Identitätsanbieter mithilfe benutzerdefinierter Richtlinien"
description: "Beispiel mit Twitter als Identitätsanbieter unter Verwendung des OAuth1-Protokolls"
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
ms.date: 10/02/2017
ms.author: yoelh
ms.openlocfilehash: 8d7bd2ed1cba62677a7eff19ba7018cd25a4345c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="azure-active-directory-b2c-add-twitter-as-an-oauth1-identity-provider-using-custom-policies"></a>Azure Active Directory B2C: Hinzufügen von Twitter als ein OAuth1-Identitätsanbieter mithilfe benutzerdefinierter Richtlinien
[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

In diesem Artikel erfahren Sie, wie Sie die Anmeldung für Benutzer von einem Twitter-Konto mittels [benutzerdefinierter Richtlinien](active-directory-b2c-overview-custom.md) aktivieren.

## <a name="prerequisites"></a>Voraussetzungen
Führen Sie die Schritte im Artikel [Erste Schritte mit benutzerdefinierten Richtlinien](active-directory-b2c-get-started-custom.md) aus.

Diese Schritte umfassen:

1.  Erstellen einer Twitter-Kontoanwendung.
2.  Hinzufügen des Twitter-Kontoanwendungsschlüssels zu Azure AD B2C
3.  Hinzufügen des Anspruchsanbieters zu einer Richtlinie.
4.  Registrieren des Twitter-Kontoanspruchsanbieters bei einer User Journey.
5.  Hochladen der Richtlinie in einen Azure AD B2C-Mandanten und deren Test.

## <a name="step-1-create-a-twitter-account-application"></a>Schritt 1: Erstellen einer Twitter-Kontoanwendung
Um Twitter als Identitätsanbieter in Azure Active Directory (Azure AD) B2C verwenden zu können, müssen Sie eine Twitter-Anwendung erstellen und die entsprechenden Parameter bereitstellen. Sie können eine Twitter-Anwendung hier registrieren: [https://twitter.com/signup](https://twitter.com/signup)

1.  Navigieren Sie zur [Twitter Developers](https://apps.twitter.com/) -Website, melden Sie sich mit den Anmeldeinformationen für Ihr Twitter-Konto an, und klicken Sie auf **Create new app**.

    ![Twitter-Konto: Neue App erstellen](media/active-directory-b2c-custom-setup-twitter-idp/adb2c-ief-setup-twitter-idp-new-app1.png)

2.  Geben Sie **Name** und **Beschreibung** für Ihre neue App ein. Fügen Sie `https://login.microsoftonline.com` für den Wert der **Website** ein. Fügen Sie dann für die **Rückruf-URL** das `https://login.microsoftonline.com/te/{tenant}.onmicrosoft.com/oauth2/authresp` ein. Ersetzen Sie {tenant} durch den Namen Ihres Mandanten (z.B. „contosob2c.onmicrosoft.com“). Stellen Sie sicher, dass Sie das HTTPS-Schema verwenden. Lesen und akzeptieren Sie die Bedingungen am Ende der Seite. Klicken Sie dann auf **Create your Twitter application**.

    ![Twitter-Konto: Hinzufügen einer neuen App](media/active-directory-b2c-custom-setup-twitter-idp/adb2c-ief-setup-twitter-idp-new-app2.png)

3.  Klicken Sie auf die Registerkarte **Einstellungen**, aktivieren Sie die Option **Allow this application to be used to sign in with Twitter** (Anmeldung bei Twitter mithilfe dieser Anwendung zulassen), und klicken Sie dann auf **Update Settings** (Einstellungen aktualisieren).
4.  Wählen Sie die Registerkarte **Keys and Access Tokens** . Notieren Sie die Werte für **Consumer Key (API Key)** (Verbraucherschlüssel (API-Schlüssel)) und **Consumer secret (API Secret)** (Verbrauchergeheimnis (API-Geheimnis)).

    ![Twitter-Konto – Festlegen von Anwendungseigenschaften](media/active-directory-b2c-custom-setup-twitter-idp/adb2c-ief-setup-twitter-idp-new-app3.png)

> [!NOTE]
>
>Das Consumer Secret ist eine wichtige Anmeldeinformation. Teilen Sie diesen Schlüssel mit niemandem, und geben Sie ihn nicht über Ihre App frei.

## <a name="step-2-add-the-twitter-account-application-key-to-azure-ad-b2c"></a>Schritt 2: Hinzufügen des Twitter-Kontoanwendungsschlüssels zu Azure AD B2C
Der Verbund mit Twitter-Konten erfordert einen geheimen Clientschlüssel für das Twitter-Konto, um Azure AD B2C im Auftrag der Anwendung zu vertrauen. Sie müssen das Consumer-Secret der Twitter-Anwendung in Ihrem Azure AD B2C-Mandanten speichern:  

1.  Wechseln Sie zu Ihrem Azure AD B2C-Mandanten, und wählen Sie **B2C-Einstellungen** > **Framework für die Identitätsfunktion**.
2.  Wählen Sie **Policy Keys** (Richtlinienschlüssel), um die in Ihrem Mandanten verfügbaren Schlüssel anzuzeigen.
3.  Klicken Sie auf **+Hinzufügen**.
4.  Verwenden Sie für **Optionen** die Option **Manuell**.
5.  Verwenden Sie für **Name** den Wert `TwitterSecret`.  
    Das Präfix `B2C_1A_` wird möglicherweise automatisch hinzugefügt.
6.  Geben Sie in das Feld **Geheimnis** Ihr Microsoft-Anwendungsgeheimnis aus „https://apps.dev.microsoft.com“ ein.
7.  Verwenden Sie für **Schlüsselverwendung** die Option **Verschlüsselung**.
8.  Klicken Sie auf **Erstellen**
9.  Vergewissern Sie sich, dass Sie den Schlüssel `B2C_1A_TwitterSecret` erstellt haben.

## <a name="step-3-add-a-claims-provider-in-your-extension-policy"></a>Schritt 3: Hinzufügen eines Anspruchsanbieters in Ihrer Erweiterungsrichtlinie

Wenn Sie möchten, dass Benutzer sich mithilfe von Twitter-Konten anmelden, müssen Sie Twitter als einen Anspruchsanbieter definieren. Das heißt in anderen Worten, dass Sie Endpunkte festlegen müssen, mit denen Azure AD B2C kommuniziert. Die Endpunkte bieten verschiedene Ansprüche, mit denen Azure AD B2C überprüft, ob ein bestimmter Benutzer authentifiziert wurde.

Definieren Sie Twitter als einen Anspruchsanbieter, indem Sie den `<ClaimsProvider>`-Knoten in Ihrer Erweiterungsrichtliniendatei hinzufügen:

1.  Öffnen Sie die Erweiterungsrichtliniendatei (TrustFrameworkExtensions.xml) von Ihrem Arbeitsverzeichnis aus. 
2.  Suchen Sie nach dem `<ClaimsProviders>`-Abschnitt.
3.  Fügen Sie den folgenden XML-Codeausschnitt unter dem `<ClaimsProviders>`-Knoten hinzu:  

    ```xml
    <ClaimsProvider>
        <Domain>twitter.com</Domain>
        <DisplayName>Twitter</DisplayName>
        <TechnicalProfiles>
        <TechnicalProfile Id="Twitter-OAUTH1">
            <DisplayName>Twitter</DisplayName>
            <Protocol Name="OAuth1" />
            <Metadata>
            <Item Key="ProviderName">Twitter</Item>
            <Item Key="authorization_endpoint">https://api.twitter.com/oauth/authenticate</Item>
            <Item Key="access_token_endpoint">https://api.twitter.com/oauth/access_token</Item>
            <Item Key="request_token_endpoint">https://api.twitter.com/oauth/request_token</Item>
            <Item Key="ClaimsEndpoint">https://api.twitter.com/1.1/account/verify_credentials.json?include_email=true</Item>
            <Item Key="ClaimsResponseFormat">json</Item>
            <Item Key="client_id">Your Twitter application consumer key</Item>
            </Metadata>
            <CryptographicKeys>
            <Key Id="client_secret" StorageReferenceId="B2C_1A_TwitterSecret" />
            </CryptographicKeys>
            <InputClaims />
            <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="user_id" />
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="screen_name" />
            <OutputClaim ClaimTypeReferenceId="email" />
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="twitter.com" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
            </OutputClaims>
            <OutputClaimsTransformations>
            <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName" />
            <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName" />
            <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId" />
            <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId" />
            </OutputClaimsTransformations>
            <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin" />
        </TechnicalProfile>
        </TechnicalProfiles>
    </ClaimsProvider>
    ```

4.  Ersetzen Sie den `client_id`-Wert mit dem Consumerschlüssel Ihrer Twitter-Kontoanwendung.

5.  Speichern Sie die Datei.

## <a name="step-4-register-the-twitter-account-claims-provider-to-sign-up-or-sign-in-user-journey"></a>Schritt 4: Registrieren des Twitter-Kontoanspruchsanbieters zum Registrieren oder Anmelden bei einer User Journey
An diesem Punkt wurde der Identitätsanbieter eingerichtet. Er ist jedoch nicht in den Bildschirmen zur Registrierung/Anmeldung verfügbar. Nun müssen Sie den Identitätsanbieter des Twitter-Kontos Ihrer-`SignUpOrSignIn`-User Journey hinzufügen.

### <a name="step-41-make-a-copy-of-the-user-journey"></a>Schritt 4.1: Erstellen einer Kopie der User Journey
Um sie verfügbar zu machen, erstellen Sie ein Duplikat einer vorhandenen User Journey-Vorlage. Fügen Sie anschließend den Twitter-Identitätsanbieter hinzu:

> [!NOTE]
>
>Wenn Sie das `<UserJourneys>`-Element aus der Basisdatei Ihrer Richtlinie in die Erweiterungsdatei (TrustFrameworkExtensions.xml) kopiert haben, können Sie zum nächsten Abschnitt übergehen.

1.  Öffnen Sie die Basisdatei Ihrer Richtlinie (z.B. „TrustFrameworkBase.xml“).
2.  Suchen Sie das `<UserJourneys>`-Element, und kopieren Sie den gesamten Inhalt des `<UserJourneys>`-Knotens.
3.  Öffnen Sie die Erweiterungsdatei (z.B. „TrustFrameworkExtensions.xml“), und suchen Sie nach dem Element `<UserJourneys>`. Wenn das Element nicht vorhanden ist, fügen Sie ein solches hinzu.
4.  Fügen Sie den gesamten Inhalt des `<UserJournesy>`-Knotens ein, den Sie als untergeordnetes Element des `<UserJourneys>`-Elements kopiert haben.

### <a name="step-42-display-the-button"></a>Schritt 4.2: Anzeigen der Schaltfläche
Das `<ClaimsProviderSelections>`-Element definiert die Liste der Anspruchsanbieter-Auswahloptionen und deren Reihenfolge.  Das `<ClaimsProviderSelection>`-Element entspricht einer Schaltfläche für einen Identitätsanbieter auf einer Registrierungs-/Anmeldeseite. Wenn Sie ein `<ClaimsProviderSelection>`-Element zum Twitter-Konto hinzufügen, wird eine neue Schaltfläche angezeigt, wenn ein Benutzer zu der Seite gelangt. So fügen Sie dieses Element hinzu:

1.  Suchen Sie den `<UserJourney>`-Knoten, der `Id="SignUpOrSignIn"` in der User Journey enthält, die Sie kopiert haben.
2.  Suchen Sie den `<OrchestrationStep>`-Knoten, der `Order="1"` enthält.
3.  Fügen Sie folgenden XML-Codeausschnitt unter dem `<ClaimsProviderSelections>`-Knoten hinzu:

    ```xml
    <ClaimsProviderSelection TargetClaimsExchangeId="TwitterExchange" />
    ```

### <a name="step-43-link-the-button-to-an-action"></a>Schritt 4.3: Verknüpfen der Schaltfläche mit einer Aktion
Nachdem Sie eine Schaltfläche implementiert haben, müssen Sie sie mit einer Aktion verknüpfen. In diesem Fall soll bei der Aktion Azure AD B2C mit dem Twitter-Konto kommunizieren, um ein Token zu empfangen. Verknüpfen Sie die Schaltfläche mit einer Aktion, indem Sie das technische Profil für Ihren Twitter-Konto-Anspruchsanbieter verknüpfen:

1.  Suchen Sie `<OrchestrationStep>`, das `Order="2"` im `<UserJourney>`-Knoten enthält.
2.  Fügen Sie folgenden XML-Codeausschnitt unter dem `<ClaimsExchanges>`-Knoten hinzu:

    ```xml
    <ClaimsExchange Id="TwitterExchange" TechnicalProfileReferenceId="Twitter-OAUTH1" />
    ```

    > [!NOTE]
    >
    > * Stellen Sie sicher, dass `Id` den gleichen Wert wie `TargetClaimsExchangeId` im vorherigen Abschnitt hat.
    > * Stellen Sie sicher, dass `TechnicalProfileReferenceId`-ID für das technische Profil festgelegt ist, das Sie zuvor erstellt haben (Twitter-OAUTH1).

## <a name="step-5-upload-the-policy-to-your-tenant"></a>Schritt 5: Hochladen der Richtlinie in Ihren Mandanten
1.  Wechseln Sie im [Azure-Portal](https://portal.azure.com) in den [Kontext Ihres Azure AD B2C-Mandanten](active-directory-b2c-navigate-to-b2c-context.md), und klicken Sie auf **Azure AD B2C**.
2.  Wählen Sie **Framework für die Identitätsfunktion** aus.
3.  Klicken Sie auf **All Policies** (Alle Richtlinien).
4.  Wählen Sie **Richtlinie hochladen** aus.
5.  Aktivieren Sie das Kontrollkästchen **Richtlinie überschreiben, sofern vorhanden**.
6.  **Laden** Sie „TrustFrameworkExtensions.xml“ hoch, und stellen Sie sicher, dass kein Fehler bei der Validierung auftritt.

## <a name="step-6-test-the-custom-policy-by-using-run-now"></a>Schritt 6: Testen der benutzerdefinierten Richtlinie mit „Jetzt ausführen“

1.  Öffnen Sie die **Azure AD B2C-Einstellungen**, und navigieren Sie zu **Framework für die Identitätsfunktion**.

    > [!NOTE]
    >
    >**Jetzt ausführen** macht erforderlich, dass mindestens eine Anwendung im Mandanten vorab registriert wird. Informationen zum Registrieren von Anwendungen finden Sie in den Artikeln [Erste Schritte](active-directory-b2c-get-started.md) bzw. [Anwendungsregistrierung](active-directory-b2c-app-registration.md) zu Azure AD B2C.

2.  Öffnen Sie **B2C_1A_signup_signin** – die benutzerdefinierte Richtlinie der vertrauenden Seite (Relying Party, RP), die Sie hochgeladen haben. Wählen Sie **Jetzt ausführen** aus.
3.  Sie sollten sich mit dem Twitter-Konto anmelden können.

## <a name="step-7-optional-register-the-twitter-account-claims-provider-to-profile-edit-user-journey"></a>Schritt 7: [Optional] Registrieren des Twitter-Kontoanspruchsanbieters in Profile-Edit-User Journey
Sie sollten Twitter-Kontoidentitätsanbieter auch zur `ProfileEdit`-User Journey Ihres Benutzers hinzufügen. Um diesen verfügbar zu machen, wiederholen Sie den vierten Schritt. Wählen Sie dieses Mal den `<UserJourney>`-Knoten aus, der `Id="ProfileEdit"` enthält. Speichern Sie Ihre Richtlinie, laden Sie diese hoch, und testen Sie sie.


## <a name="download-the-complete-policy-files"></a>Herunterladen der vollständigen Richtliniendateien
Optional: Sie sollten nach Abschluss der exemplarischen Vorgehensweise „Erste Schritte mit benutzerdefinierten Richtlinien“ Ihr Szenario mithilfe Ihrer eigenen Dateien mit benutzerdefinierten Richtlinien erstellen, anstatt diese Beispieldateien zu verwenden.  [Beispielrichtliniendateien zur Referenz](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-twitter-app)