---
title: "Azure Active Directory B2C: Hinzufügen von LinkedIn als OAuth2-Identitätsanbieter mithilfe benutzerdefinierter Richtlinien"
description: Artikel zur Einrichtung einer LinkedIn-Anwendung mithilfe des OAuth2-Protokolls und benutzerdefinierter Richtlinien
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
ms.openlocfilehash: f72fac677aa7d461f174b2b06db69df235273375
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2017
---
# <a name="azure-active-directory-b2c-add-linkedin-as-an-identity-provider-by-using-custom-policies"></a>Azure Active Directory B2C: Hinzufügen von LinkedIn als Identitätsanbieter mithilfe benutzerdefinierter Richtlinien
[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

In diesem Artikel erfahren Sie, wie Sie die Anmeldung für Benutzer eines LinkedIn-Kontos mittels [benutzerdefinierter Richtlinien](active-directory-b2c-overview-custom.md) aktivieren.

## <a name="prerequisites"></a>Voraussetzungen
Führen Sie die Schritte im Artikel [Erste Schritte mit benutzerdefinierten Richtlinien](active-directory-b2c-get-started-custom.md) aus.

## <a name="step-1-create-a-linkedin-account-application"></a>1: Erstellen einer LinkedIn-Kontoanwendung
Um LinkedIn als Identitätsanbieter in Azure Active Directory B2C (Azure AD B2C) verwenden zu können, müssen Sie eine LinkedIn-Anwendung erstellen und mit den entsprechenden Parametern bereitstellen. Sie können eine LinkedIn-Anwendung auf der [LinkedIn-Registrierungsseite](https://LinkedIn.com/signup) registrieren.

1. Wechseln Sie zur Website [LinkedIn-Anwendungsverwaltung](https://www.linkedin.com/secure/developer?newapp=), melden Sie sich mit den Anmeldeinformationen für Ihr LinkedIn-Konto an, und wählen Sie **Create Application** (Anwendung erstellen) aus.

    ![LinkedIn-Konto: Anwendung erstellen](media/active-directory-b2c-custom-setup-li-idp/adb2c-ief-setup-li-idp-new-app1.png)

2. Gehen Sie auf der Seite **Create a New Application** (Neue Anwendung erstellen) folgendermaßen vor:

    a. Geben Sie Ihren **Firmennamen** und einen aussagekräftigen **Namen** für das Unternehmen sowie eine **Beschreibung** für Ihre neue Anwendung ein.

    b. Laden Sie Ihr **Anwendungslogo** hoch.

    c. Wählen Sie unter **Application Use** einen Anwendungsverwendungszweck aus.

    d. Fügen Sie im Feld **Website URL** die Zeichenfolge **https://login.microsoftonline.com** ein.

    e. Machen Sie Angaben für **Business Email** (Geschäftliche E-Mail-Adresse) und **Business Phone** (Geschäftliche Telefonnummer).

    f. Lesen und akzeptieren Sie die Nutzungsbedingungen am Ende der Seite, und wählen Sie dann **Submit** (Übermitteln) aus.

    ![LinkedIn-Konto: Konfigurieren von Anwendungseigenschaften](media/active-directory-b2c-custom-setup-li-idp/adb2c-ief-setup-li-idp-new-app2.png)

3. Wählen Sie **Authentication** (Authentifizierung) aus, und notieren Sie die Werte für **Client ID** und **Client Secret** (Clientgeheimnis).

4. Fügen Sie im Feld **Authorized Redirect URLs** (Autorisierte Weiterleitungs-URLs) die Zeichenfolge **https://login.microsoftonline.com/te/{Mandant}.onmicrosoft.com/oauth2/authresp** ein. Ersetzen Sie {*Mandant*} durch den Namen Ihres Mandanten (z.B. „contosob2c.onmicrosoft.com“). Stellen Sie sicher, dass Sie das HTTPS-Schema verwenden. 

    ![LinkedIn-Konto: Festlegen von autorisierten Umleitungs-URLs](media/active-directory-b2c-custom-setup-li-idp/adb2c-ief-setup-li-idp-new-app3.png)

    >[!NOTE]
    >Der geheime Clientschlüssel ist eine wichtige Anmeldeinformation. Teilen Sie diesen Schlüssel mit niemandem, und geben Sie ihn nicht über Ihre App frei.

5. Wählen Sie **Hinzufügen**.

6. Wählen Sie **Settings** (Einstellungen) aus, ändern Sie **Application status** (Anwendungsstatus) in **Live**, und wählen Sie dann **Update** aus.

    ![LinkedIn-Konto: Festlegen des Anwendungsstatus](media/active-directory-b2c-custom-setup-li-idp/adb2c-ief-setup-li-idp-new-app4.png)

## <a name="step-2-add-your-linkedin-application-key-to-azure-ad-b2c"></a>2: Hinzufügen Ihres LinkedIn-Anwendungsschlüssels zu Azure AD B2C
Ein Verbund mit LinkedIn-Konten erfordert einen geheimen Clientschlüssel für das LinkedIn-Konto, um Azure AD B2C im Auftrag der Anwendung zu vertrauen. Gehen Sie folgendermaßen vor, um das LinkedIn-Anwendungsgeheimnis in Ihrem Azure AD B2C-Mandanten zu speichern:  

1. Wählen Sie in Ihrem Azure AD B2C-Mandanten **B2C-Einstellungen** > **Framework für die Identitätsfunktion** aus.

2. Um die in Ihrem Mandanten verfügbaren Schlüssel anzuzeigen, wählen Sie **Richtlinienschlüssel** aus.

3. Wählen Sie **Hinzufügen**.

4. Wählen Sie im Feld **Optionen** die Option **Upload** aus.

5. Geben Sie im Feld **Name** die Zeichenfolge **B2cRestClientCertificate** ein.  
    Das Präfix *B2C_1A_* wird möglicherweise automatisch hinzugefügt.

6. Geben Sie im Feld **Geheimnis** Ihr LinkedIn-Anwendungsgeheimnis aus dem [Anwendungsregistrierungsportal](https://apps.dev.microsoft.com) ein.

7. Wählen Sie für **Schlüsselverwendung** die Option **Verschlüsselung** aus.

8. Klicken Sie auf **Erstellen**. 

9. Vergewissern Sie sich, dass Sie den Schlüssel `B2C_1A_LinkedInSecret` erstellt haben.

## <a name="step-3-add-a-claims-provider-in-your-extension-policy"></a>Schritt 3: Hinzufügen eines Anspruchsanbieters in Ihrer Erweiterungsrichtlinie
Wenn Sie möchten, dass Benutzer sich mithilfe ihres LinkedIn-Kontos anmelden, müssen Sie LinkedIn als Anspruchsanbieter definieren. Das heißt mit anderen Worten, dass Sie die Endpunkte festlegen müssen, mit denen Azure AD B2C kommuniziert. Die Endpunkte bieten verschiedene Ansprüche, mit denen Azure AD B2C überprüft, ob ein bestimmter Benutzer authentifiziert wurde.

Definieren Sie LinkedIn als Anspruchsanbieter, indem Sie einen `<ClaimsProvider>`-Knoten in Ihrer Erweiterungsrichtliniendatei hinzufügen:

1. Öffnen Sie die Erweiterungsrichtliniendatei *TrustFrameworkExtensions.xml* in Ihrem Arbeitsverzeichnis. 

2. Suchen Sie nach dem `<ClaimsProviders>`-Element.

3. Fügen Sie im `<ClaimsProviders>`-Element den folgenden XML-Codeausschnitt hinzu: 

    ```xml
    <ClaimsProvider>
      <Domain>linkedin.com</Domain>
      <DisplayName>LinkedIn</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="LinkedIn-OAUTH">
          <DisplayName>LinkedIn</DisplayName>
          <Protocol Name="OAuth2" />
          <Metadata>
            <Item Key="ProviderName">linkedin</Item>
            <Item Key="authorization_endpoint">https://www.linkedin.com/oauth/v2/authorization</Item>
            <Item Key="AccessTokenEndpoint">https://www.linkedin.com/oauth/v2/accessToken</Item>
            <Item Key="ClaimsEndpoint">https://api.linkedin.com/v1/people/~:(id,first-name,last-name,email-address,headline)</Item>
            <Item Key="ClaimsEndpointAccessTokenName">oauth2_access_token</Item>
            <Item Key="ClaimsEndpointFormatName">format</Item>
            <Item Key="ClaimsEndpointFormat">json</Item>
            <Item Key="scope">r_emailaddress r_basicprofile</Item>
            <Item Key="HttpBinding">POST</Item>
            <Item Key="UsePolicyInRedirectUri">0</Item>
            <Item Key="client_id">Your LinkedIn application client ID</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="client_secret" StorageReferenceId="B2C_1A_LinkedInSecret" />
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="id" />
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="firstName" />
            <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="lastName" />
            <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="emailAddress" />
            <!--<OutputClaim ClaimTypeReferenceId="jobTitle" PartnerClaimType="headline" />-->
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="linkedin.com" />
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

4. Ersetzen Sie den Wert *client_id* durch Ihre LinkedIn-Anwendungsclient-ID.

5. Speichern Sie die Datei.

## <a name="step-4-register-the-linkedin-account-claims-provider"></a>Schritt 4: Registrieren des LinkedIn Kontoanspruchsanbieters
Sie haben den Identitätsanbieter eingerichtet. Er ist jedoch nicht in den Fenstern zur Registrierung oder Anmeldung verfügbar. Nun müssen Sie den LinkedIn-Kontoidentitätsanbieter der `SignUpOrSignIn`-User Journey Ihres Benutzers hinzufügen.

### <a name="step-41-make-a-copy-of-the-user-journey"></a>Schritt 4.1: Erstellen einer Kopie der User Journey
Um die User Journey verfügbar zu machen, erstellen Sie ein Duplikat einer vorhandenen User Journey-Vorlage und fügen dann den LinkedIn-Identitätsanbieter hinzu:

>[!NOTE]
>Wenn Sie das `<UserJourneys>`-Element aus der Basisdatei Ihrer Richtlinie in die Erweiterungsdatei *TrustFrameworkExtensions.xml* kopiert haben, können Sie diesen Abschnitt überspringen.

1. Öffnen Sie die Basisdatei Ihrer Richtlinie (z.B. „TrustFrameworkBase.xml“).

2. Suchen Sie nach dem `<UserJourneys>`-Element, markieren Sie den gesamten Inhalt des `<UserJourney>`-Knotens, und wählen Sie dann **Ausschneiden** aus, um den markierten Text in die Zwischenablage zu verschieben.

3. Öffnen Sie die Erweiterungsdatei (z.B. „TrustFrameworkExtensions.xml“), und suchen Sie das `<UserJourneys>`-Element. Wenn das Element nicht vorhanden ist, fügen Sie es hinzu.

4. Fügen Sie den gesamten Inhalt des `<UserJourney>`-Knotens, den Sie in Schritt 2 in die Zwischenablage verschoben haben, in das `<UserJourneys>`-Element.

### <a name="step-42-display-the-button"></a>Schritt 4.2: Anzeigen der Schaltfläche
Das `<ClaimsProviderSelections>`-Element definiert die Liste der Anspruchsanbieter-Auswahloptionen und deren Reihenfolge. Der `<ClaimsProviderSelection>`-Knoten entspricht einer Schaltfläche für einen Identitätsanbieter auf einer Registrierungs- oder Anmeldeseite. Wenn Sie einen `<ClaimsProviderSelection>`-Knoten für ein LinkedIn-Konto hinzufügen, wird eine neue Schaltfläche angezeigt, wenn ein Benutzer zu der Seite gelangt. Um dieses Element hinzuzufügen, gehen Sie folgendermaßen vor:

1. Suchen Sie den `<UserJourney>`-Knoten, der in der kopierten User Journey `Id="SignUpOrSignIn"` enthält.

2. Suchen Sie den `<OrchestrationStep>`-Knoten, der `Order="1"` enthält.

3. Fügen Sie im `<ClaimsProviderSelections>`-Element den folgenden XML-Codeausschnitt hinzu:

    ```xml
    <ClaimsProviderSelection TargetClaimsExchangeId="LinkedInExchange" />
    ```

### <a name="step-43-link-the-button-to-an-action"></a>Schritt 4.3: Verknüpfen der Schaltfläche mit einer Aktion
Nachdem Sie eine Schaltfläche implementiert haben, müssen Sie sie mit einer Aktion verknüpfen. In diesem Fall ist die Aktion die Kommunikation von Azure AD B2C mit dem LinkedIn-Konto, um ein Token zu empfangen. Verknüpfen Sie die Schaltfläche mit einer Aktion, indem Sie das technische Profil für Ihren LinkedIn-Kontoanspruchsanbieter verknüpfen:

1. Suchen Sie den `<OrchestrationStep>`-Knoten, der `Order="2"` im `<UserJourney>`-Knoten enthält.

2. Fügen Sie im `<ClaimsExchanges>`-Element den folgenden XML-Codeausschnitt hinzu:

    ```xml
    <ClaimsExchange Id="LinkedInExchange" TechnicalProfileReferenceId="LinkedIn-OAuth" />
    ```

    >[!NOTE]
    >* Stellen Sie sicher, dass `Id` denselben Wert wie `TargetClaimsExchangeId` im vorherigen Abschnitt aufweist.
    >* Stellen Sie sicher, dass für die `TechnicalProfileReferenceId`-ID das technische Profil festgelegt ist, das Sie zuvor erstellt haben (LinkedIn-OAuth).

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
    Sie sollten sich jetzt mit dem LinkedIn-Konto anmelden können.

## <a name="step-7-optional-register-the-linkedin-account-claims-provider-to-the-profile-edit-user-journey"></a>Schritt 7 (optional): Registrieren des LinkedIn-Kontoanspruchsanbieters bei der Profile-Edit-User Journey
Möglicherweise möchten Sie außerdem den LinkedIn-Kontoidentitätsanbieter der `ProfileEdit`-User Journey Ihres Benutzers hinzufügen. Um die User Journey verfügbar zu machen, wiederholen Sie Schritt 4. Wählen Sie dieses Mal den `<UserJourney>`-Knoten aus, der `Id="ProfileEdit"` enthält. Speichern Sie Ihre Richtlinie, laden Sie diese hoch, und testen Sie sie.

## <a name="optional-download-the-complete-policy-files"></a>(Optional:) Herunterladen der vollständigen Richtliniendateien
Nachdem Sie die exemplarische Vorgehensweise unter [Erste Schritte mit benutzerdefinierten Richtlinien](active-directory-b2c-get-started-custom.md) abgeschlossen haben, empfiehlt es sich, ein Szenario mit Ihren eigenen benutzerdefinierten Richtliniendateien zu erstellen. Zu Referenzzwecken haben wir [Beispiele für Richtliniendateien](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-li-app) bereitgestellt.
