---
title: "Azure Active Directory B2C: Hinzufügen von Twitter als OAuth1-Identitätsanbieter mithilfe benutzerdefinierter Richtlinien"
description: "Verwenden von Twitter als Identitätsanbieter unter Verwendung des OAuth1-Protokolls"
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
ms.date: 10/23/2017
ms.author: yoelh
ms.openlocfilehash: f3a7936a468df7b0a2713f1f30c5b91e74d1d917
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2017
---
# <a name="azure-active-directory-b2c-add-twitter-as-an-oauth1-identity-provider-by-using-custom-policies"></a>Azure Active Directory B2C: Hinzufügen von Twitter als OAuth1-Identitätsanbieter mithilfe benutzerdefinierter Richtlinien
[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

In diesem Artikel erfahren Sie, wie Sie die Anmeldung für Benutzer eines Twitter-Kontos mittels [benutzerdefinierter Richtlinien](active-directory-b2c-overview-custom.md) aktivieren.

## <a name="prerequisites"></a>Voraussetzungen
Führen Sie die Schritte im Artikel [Erste Schritte mit benutzerdefinierten Richtlinien](active-directory-b2c-get-started-custom.md) aus.

## <a name="step-1-create-a-twitter-account-application"></a>Schritt 1: Erstellen einer Twitter-Kontoanwendung
Um Twitter als Identitätsanbieter in Azure Active Directory B2C (Azure AD B2C) verwenden zu können, müssen Sie eine Twitter-Anwendung erstellen und die entsprechenden Parameter bereitstellen. Sie können eine Twitter-Anwendung auf der [Twitter-Registrierungsseite](https://twitter.com/signup) registrieren.

1. Wechseln Sie zur [Twitter Developers](https://apps.twitter.com/)-Website, melden Sie sich mit den Anmeldeinformationen für Ihr Twitter-Konto an, und wählen Sie dann **Create new app** (Neue App erstellen) aus.

    ![Twitter-Konto: Neue App erstellen](media/active-directory-b2c-custom-setup-twitter-idp/adb2c-ief-setup-twitter-idp-new-app1.png)

2. Gehen Sie im Fenster **Create an Application** (Anwendung erstellen) folgendermaßen vor:
 
    a. Geben Sie **Name** und **Beschreibung** für Ihre neue App ein. 

    b. Fügen Sie im Feld **Website** die Zeichenfolge **https://login.microsoftonline.com** ein. 

    c. Fügen Sie im Feld **Callback URL** (Rückruf-URL) die Zeichenfolge **https://login.microsoftonline.com/te/{Mandant}.onmicrosoft.com/oauth2/authresp** ein. Ersetzen Sie {*Mandant*} durch den Namen Ihres Mandanten (z.B. „contosob2c.onmicrosoft.com“). Stellen Sie sicher, dass Sie das HTTPS-Schema verwenden. 

    d. Lesen und bestätigen Sie die Nutzungsbedingungen am Ende der Seite, und wählen Sie **Create your Twitter application** (Twitter-Anwendung erstellen) aus.

    ![Twitter-Konto: Hinzufügen einer neuen App](media/active-directory-b2c-custom-setup-twitter-idp/adb2c-ief-setup-twitter-idp-new-app2.png)

3. Wählen Sie im Fenster **B2C demo** die Option **Settings** (Einstellungen) aus, aktivieren Sie das Kontrollkästchen **Allow this application to be used to sign in with Twitter** (Diese Anwendung darf für die Anmeldung bei Twitter verwendet werden), und wählen Sie dann **Update Settings** (Einstellungen aktualisieren) aus.

4. Wählen Sie **Keys and Access Tokens** (Schlüssel und Zugriffstoken) aus, und notieren Sie die Werte **Consumer Key (API Key)** (Consumerschlüssel [API-Schlüssel]) und **Consumer Secret (API Secret)** (Consumergeheimnis [API-Geheimnis]).

    ![Twitter-Konto – Festlegen von Anwendungseigenschaften](media/active-directory-b2c-custom-setup-twitter-idp/adb2c-ief-setup-twitter-idp-new-app3.png)

    >[!NOTE]
    >Consumer secret ist eine wichtige Anmeldeinformation. Teilen Sie diesen Schlüssel mit niemandem, und geben Sie ihn nicht über Ihre App frei.

## <a name="step-2-add-your-twitter-account-application-key-to-azure-ad-b2c"></a>Schritt 2: Hinzufügen Ihres Twitter-Kontoanwendungsschlüssels zu Azure AD B2C
Der Verbund mit Twitter-Konten erfordert einen geheimen Consumerschlüssel für das Twitter-Konto, um Azure AD B2C im Auftrag der Anwendung zu vertrauen. Gehen Sie folgendermaßen vor, um das Consumergeheimnis der Twitter-Anwendung in Ihrem Azure AD B2C-Mandanten zu speichern: 

1. Wählen Sie in Ihrem Azure AD B2C-Mandanten **B2C-Einstellungen** > **Framework für die Identitätsfunktion** aus.

2. Um die in Ihrem Mandanten verfügbaren Schlüssel anzuzeigen, wählen Sie **Richtlinienschlüssel** aus.

3. Wählen Sie **Hinzufügen**.

4. Wählen Sie im Feld **Optionen** die Option **Manuell** aus.

5. Wählen Sie im Feld **Name** die Option **TwitterSecret** aus.  
    Das Präfix *B2C_1A_* wird möglicherweise automatisch hinzugefügt.

6. Geben Sie im Feld **Geheimnis** Ihr Microsoft-Anwendungsgeheimnis aus dem [Anwendungsregistrierungsportal](https://apps.dev.microsoft.com) ein.

7. Verwenden Sie für **Schlüsselverwendung** die Option **Verschlüsselung**.

8. Klicken Sie auf **Erstellen**.

9. Vergewissern Sie sich, dass Sie den Schlüssel `B2C_1A_TwitterSecret` erstellt haben.

## <a name="step-3-add-a-claims-provider-in-your-extension-policy"></a>Schritt 3: Hinzufügen eines Anspruchsanbieters in Ihrer Erweiterungsrichtlinie

Wenn Sie möchten, dass Benutzer sich mithilfe von Twitter-Konten anmelden, müssen Sie Twitter als einen Anspruchsanbieter definieren. Das heißt mit anderen Worten, dass Sie die Endpunkte festlegen müssen, mit denen Azure AD B2C kommuniziert. Die Endpunkte bieten verschiedene Ansprüche, mit denen Azure AD B2C überprüft, ob ein bestimmter Benutzer authentifiziert wurde.

Definieren Sie Twitter als Anspruchsanbieter, indem Sie in Ihrer Erweiterungsrichtliniendatei einen `<ClaimsProvider>`-Knoten hinzufügen:

1. Öffnen Sie die Erweiterungsrichtliniendatei *TrustFrameworkExtensions.xml* in Ihrem Arbeitsverzeichnis. 

2. Suchen Sie nach dem Abschnitt `<ClaimsProviders>`.

3. Fügen Sie im `<ClaimsProviders>`-Knoten den folgenden XML-Codeausschnitt hinzu:  

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

4. Ersetzen Sie den Wert *client_id* durch den Anwendungsconsumerschlüssel Ihres Twitter-Kontos.

5. Speichern Sie die Datei.

## <a name="step-4-register-the-twitter-account-claims-provider-to-your-sign-up-or-sign-in-user-journey"></a>Schritt 4: Registrieren des Twitter-Kontoanspruchsanbieters bei Ihrer User Journey für Registrierung oder Anmeldung
Sie haben den Identitätsanbieter eingerichtet. Er ist jedoch nicht in den Fenstern zur Registrierung oder Anmeldung verfügbar. Nun müssen Sie den Identitätsanbieter des Twitter-Kontos Ihrer `SignUpOrSignIn`-User Journey hinzufügen.

### <a name="step-41-make-a-copy-of-the-user-journey"></a>Schritt 4.1: Erstellen einer Kopie der User Journey
Um die User Journey verfügbar zu machen, erstellen Sie ein Duplikat einer vorhandenen User Journey-Vorlage und fügen dann den Twitter-Identitätsanbieter hinzu:

>[!NOTE]
>Wenn Sie das `<UserJourneys>`-Element aus der Basisdatei Ihrer Richtlinie in die Erweiterungsdatei *TrustFrameworkExtensions.xml* kopiert haben, können Sie mit dem nächsten Abschnitt fortfahren.

1. Öffnen Sie die Basisdatei Ihrer Richtlinie (z.B. „TrustFrameworkBase.xml“).

2. Suchen Sie nach dem `<UserJourneys>`-Element, markieren Sie den gesamten Inhalt des `<UserJourney>`-Knotens, und wählen Sie dann **Ausschneiden** aus, um den markierten Text in die Zwischenablage zu verschieben.

3. Öffnen Sie die Erweiterungsdatei (z.B. „TrustFrameworkExtensions.xml“), und suchen Sie dann das `<UserJourneys>`-Element. Wenn das Element nicht vorhanden ist, fügen Sie es hinzu.

4. Fügen Sie den gesamten Inhalt des `<UserJourney>`-Knotens, den Sie in Schritt 2 in die Zwischenablage verschoben haben, in das `<UserJourneys>`-Element.

### <a name="step-42-display-the-button"></a>Schritt 4.2: Anzeigen der Schaltfläche
Das `<ClaimsProviderSelections>`-Element definiert die Liste der Anspruchsanbieter-Auswahloptionen und deren Reihenfolge. Der `<ClaimsProviderSelection>`-Knoten entspricht einer Schaltfläche für einen Identitätsanbieter auf einer Registrierungs- oder Anmeldeseite. Wenn Sie einen `<ClaimsProviderSelection>`-Knoten für ein Twitter-Konto hinzufügen, wird eine neue Schaltfläche angezeigt, wenn ein Benutzer zu der Seite gelangt. Um dieses Element hinzuzufügen, gehen Sie folgendermaßen vor:

1. Suchen Sie den `<UserJourney>`-Knoten, der in der kopierten User Journey `Id="SignUpOrSignIn"` enthält.

2. Suchen Sie den `<OrchestrationStep>`-Knoten, der `Order="1"` enthält.

3. Fügen Sie im `<ClaimsProviderSelections>`-Element den folgenden XML-Codeausschnitt hinzu:

    ```xml
    <ClaimsProviderSelection TargetClaimsExchangeId="TwitterExchange" />
    ```

### <a name="step-43-link-the-button-to-an-action"></a>Schritt 4.3: Verknüpfen der Schaltfläche mit einer Aktion
Nachdem Sie eine Schaltfläche implementiert haben, müssen Sie sie mit einer Aktion verknüpfen. In diesem Fall soll bei der Aktion Azure AD B2C mit dem Twitter-Konto kommunizieren, um ein Token zu empfangen. Verknüpfen Sie die Schaltfläche mit einer Aktion, indem Sie das technische Profil für Ihren Twitter-Konto-Anspruchsanbieter verknüpfen:

1. Suchen Sie den `<OrchestrationStep>`-Knoten, der `Order="2"` im `<UserJourney>`-Knoten enthält.
2. Fügen Sie im `<ClaimsExchanges>`-Element den folgenden XML-Codeausschnitt hinzu:

    ```xml
    <ClaimsExchange Id="TwitterExchange" TechnicalProfileReferenceId="Twitter-OAUTH1" />
    ```

    >[!NOTE]
    >* Stellen Sie sicher, dass `Id` denselben Wert wie `TargetClaimsExchangeId` im vorherigen Abschnitt aufweist.
    >* Stellen Sie sicher, dass für die `TechnicalProfileReferenceId`-ID das technische Profil festgelegt ist, das Sie zuvor erstellt haben (Twitter-OAUTH1).

## <a name="step-5-upload-the-policy-to-your-tenant"></a>Schritt 5: Hochladen der Richtlinie in Ihren Mandanten
1. Wechseln Sie im [Azure-Portal](https://portal.azure.com) zum [Kontext Ihres Azure AD B2C-Mandanten](active-directory-b2c-navigate-to-b2c-context.md), und wählen Sie anschließend **Azure AD B2C** aus.

2. Wählen Sie **Framework für die Identitätsfunktion** aus.

3. Wählen Sie die Option **Alle Richtlinien** aus.

4. Wählen Sie **Richtlinie hochladen** aus.

5. Aktivieren Sie das Kontrollkästchen **Richtlinie überschreiben, sofern vorhanden**.

6. Laden Sie die Dateien *TrustFrameworkBase.xml* und *TrustFrameworkExtensions.xml* hoch, und stellen Sie sicher, dass sie die Überprüfung bestehen.

## <a name="step-6-test-the-custom-policy-by-using-run-now"></a>Schritt 6: Testen der benutzerdefinierten Richtlinie mit „Jetzt ausführen“

1. Wählen Sie **Azure AD B2C-Einstellungen** und dann **Framework für die Identitätsfunktion** aus.

    >[!NOTE]
    >Für „Jetzt ausführen“ muss mindestens eine Anwendung vorab im Mandanten registriert werden. Informationen zum Registrieren von Anwendungen finden Sie in den Artikeln [Erste Schritte](active-directory-b2c-get-started.md) bzw. [Anwendungsregistrierung](active-directory-b2c-app-registration.md) zu Azure AD B2C.

2. Öffnen Sie **B2C_1A_signup_signin**. Dies ist die benutzerdefinierte Richtlinie der vertrauenden Seite (Relying Party, RP), die Sie hochgeladen haben. Wählen Sie anschließend **Jetzt ausführen** aus.  
    Sie sollten sich jetzt mit dem Twitter-Konto anmelden können.

## <a name="step-7-optional-register-the-twitter-account-claims-provider-to-the-profile-edit-user-journey"></a>Schritt 7 (optional): Registrieren des Twitter-Kontoanspruchsanbieters in der Profile-Edit-User Journey
Möglicherweise möchten Sie außerdem den Twitter-Kontoidentitätsanbieter der `ProfileEdit`-User Journey Ihres Benutzers hinzufügen. Um die User Journey verfügbar zu machen, wiederholen Sie Schritt 4. Wählen Sie dieses Mal den `<UserJourney>`-Knoten aus, der `Id="ProfileEdit"` enthält. Speichern Sie Ihre Richtlinie, laden Sie diese hoch, und testen Sie sie.


## <a name="optional-download-the-complete-policy-files"></a>(Optional:) Herunterladen der vollständigen Richtliniendateien
Nachdem Sie die exemplarische Vorgehensweise unter [Erste Schritte mit benutzerdefinierten Richtlinien](active-directory-b2c-get-started-custom.md) abgeschlossen haben, empfiehlt es sich, ein Szenario mit Ihren eigenen benutzerdefinierten Richtliniendateien zu erstellen. Zu Referenzzwecken haben wir [Beispiele für Richtliniendateien](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-twitter-app) bereitgestellt.
