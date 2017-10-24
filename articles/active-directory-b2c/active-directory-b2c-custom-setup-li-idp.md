---
title: "Azure Active Directory B2C: Hinzufügen von LinkedIn als OAuth2-Identitätsanbieter mithilfe benutzerdefinierter Richtlinien"
description: Artikel zur Einrichtung der LinkedIn-App mithilfe des OAuth2-Protokolls und benutzerdefinierter Richtlinien
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
ms.date: 10/03/2017
ms.author: yoelh
ms.openlocfilehash: ac4c0212ffc13b24b6035756f1210d62b7b840c8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="azure-active-directory-b2c-add-linkedin-as-an-identity-provider-using-custom-policies"></a>Azure Active Directory B2C: Hinzufügen von LinkedIn als Identitätsanbieter mithilfe benutzerdefinierter Richtlinien
[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

In diesem Artikel erfahren Sie, wie Sie die Anmeldung für Benutzer eines LinkedIn-Kontos mittels [benutzerdefinierter Richtlinien](active-directory-b2c-overview-custom.md) aktivieren.

## <a name="prerequisites"></a>Voraussetzungen
Führen Sie die Schritte im Artikel [Erste Schritte mit benutzerdefinierten Richtlinien](active-directory-b2c-get-started-custom.md) aus.

Diese Schritte umfassen:
1.  Erstellen einer LinkedIn-Kontoanwendung
2.  Hinzufügen des LinkedIn-Kontoanwendungsschlüssels zu Azure AD B2C
3.  Hinzufügen des Anspruchsanbieters zu einer Richtlinie.
4.  Registrieren des LinkedIn-Kontoanspruchsanbieters bei einer User Journey
5.  Hochladen der Richtlinie in einen Azure AD B2C-Mandanten sowie Testen der Richtlinie

## <a name="step-1-create-a-linkedin-account-application"></a>1: Erstellen einer LinkedIn-Kontoanwendung
Um LinkedIn als Identitätsanbieter in Azure Active Directory (Azure AD) B2C verwenden zu können, müssen Sie eine LinkedIn-Anwendung erstellen und mit den entsprechenden Parametern bereitstellen. Sie können eine LinkedIn-Anwendung hier registrieren: [https://LinkedIn.com/signup](https://LinkedIn.com/signup)

1.  Navigieren Sie zur Website [LinkedIn-Anwendungsverwaltung](https://www.linkedin.com/secure/developer?newapp=), melden Sie sich mit den Anmeldeinformationen für Ihr LinkedIn-Konto an, und klicken Sie auf **Anwendung erstellen**.

    ![LinkedIn-Konto: Anwendung erstellen](media/active-directory-b2c-custom-setup-li-idp/adb2c-ief-setup-li-idp-new-app1.png)

2.  1.  Geben Sie Ihren **Firmennamen** sowie einen aussagekräftigen **Namen** und eine **Beschreibung** für Ihre neue Anwendung ein.
    2.  Laden Sie das **Anwendungslogo** hoch.
    3.  Klicken Sie auf **Application Use** (Anwendungsverwendung).
    4.  Kopieren Sie `https://login.microsoftonline.com` in **Website-URL**.
    5.  Geben Sie Ihre **geschäftliche E-Mail-Adresse** und **geschäftliche Telefonnummer** an.
    6.  Lesen und akzeptieren Sie die Nutzungsbedingungen am Ende der Seite. Klicken Sie dann auf **Senden**.

    ![LinkedIn-Konto: Konfigurieren von Anwendungseigenschaften](media/active-directory-b2c-custom-setup-li-idp/adb2c-ief-setup-li-idp-new-app2.png)

3.  Klicken Sie im Menü auf **Authentifizierung**. Notieren Sie sich die Werte der **Client-ID** und des **geheimen Clientschlüssels**.

    Geben Sie für den Wert **autorisierte Umleitungs-URLs** `https://login.microsoftonline.com/te/{tenant}.onmicrosoft.com/oauth2/authresp` an. Ersetzen Sie {tenant} durch den Namen Ihres Mandanten (z.B. „contosob2c.onmicrosoft.com“). Stellen Sie sicher, dass Sie das HTTPS-Schema verwenden, und klicken Sie auf **Hinzufügen**.

    ![LinkedIn-Konto: Festlegen von autorisierten Umleitungs-URLs](media/active-directory-b2c-custom-setup-li-idp/adb2c-ief-setup-li-idp-new-app3.png)

    > [!NOTE]
    >
    >Der geheime Clientschlüssel ist eine wichtige Anmeldeinformation. Teilen Sie diesen Schlüssel mit niemandem, und geben Sie ihn nicht über Ihre App frei.

4.  Klicken Sie auf die Registerkarte **Einstellungen**, ändern Sie den **Anwendungsstatus** in **Live** um, und klicken Sie dann auf **Aktualisieren**.

    ![LinkedIn-Konto: Festlegen des Anwendungsstatus](media/active-directory-b2c-custom-setup-li-idp/adb2c-ief-setup-li-idp-new-app4.png)

## <a name="step-2-add-your-linkedin-application-key-to-azure-ad-b2c"></a>2: Hinzufügen Ihres LinkedIn-Anwendungsschlüssels zu Azure AD B2C
Ein Verbund mit LinkedIn-Konten erfordert einen geheimen Clientschlüssel für das LinkedIn-Konto, um Azure AD B2C im Auftrag der Anwendung zu vertrauen. Sie müssen das LinkedIn-Anwendungsgeheimnis in Ihrem Azure AD B2C-Mandanten speichern:  

1.  Wechseln Sie zu Ihrem Azure AD B2C-Mandanten, und wählen Sie **B2C-Einstellungen** > **Framework für die Identitätsfunktion**.
2.  Wählen Sie **Policy Keys** (Richtlinienschlüssel), um die in Ihrem Mandanten verfügbaren Schlüssel anzuzeigen.
3.  Klicken Sie auf **+Hinzufügen**.
4.  Verwenden Sie für **Optionen** die Option **Manuell**.
5.  Verwenden Sie für **Name** den Wert `LinkedInSecret`.  
    Das Präfix `B2C_1A_` wird möglicherweise automatisch hinzugefügt.
6.  Geben Sie in das Feld **Geheimnis** Ihr LinkedIn-Anwendungsgeheimnis aus „https://apps.dev.microsoft.com“ ein.
7.  Verwenden Sie für **Schlüsselverwendung** die Option **Verschlüsselung**.
8.  Klicken Sie auf **Erstellen** 
9.  Vergewissern Sie sich, dass Sie den Schlüssel `B2C_1A_LinkedInSecret` erstellt haben.

## <a name="step-3-add-a-claims-provider-in-your-extension-policy"></a>Schritt 3: Hinzufügen eines Anspruchsanbieters in Ihrer Erweiterungsrichtlinie
Wenn Sie möchten, dass Benutzer sich mithilfe Ihres LinkedIn-Kontos anmelden, müssen Sie LinkedIn als Anspruchsanbieter definieren. Das heißt in anderen Worten, dass Sie Endpunkte festlegen müssen, mit denen Azure AD B2C kommuniziert. Die Endpunkte bieten verschiedene Ansprüche, mit denen Azure AD B2C überprüft, ob ein bestimmter Benutzer authentifiziert wurde.

Definieren Sie LinkedIn als Anspruchsanbieter, indem Sie den `<ClaimsProvider>`-Knoten in Ihrer Erweiterungsrichtliniendatei hinzufügen:

1.  Öffnen Sie die Erweiterungsrichtliniendatei (TrustFrameworkExtensions.xml) von Ihrem Arbeitsverzeichnis aus. 
2.  Suchen Sie nach dem `<ClaimsProviders>`-Abschnitt.
3.  Fügen Sie den folgenden XML-Codeausschnitt unter dem `<ClaimsProviders>`-Knoten hinzu:  

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

4.  Ersetzen Sie den `client_id`-Wert durch Ihre LinkedIn-Kontoanwendungsclient-ID.
5.  Speichern Sie die Datei.

## <a name="step-4-register-the-linkedin-account-claims-provider-to-sign-up-or-sign-in-user-journey"></a>4: Registrieren des LinkedIn-Kontoanspruchsanbieters zum Registrieren oder Anmelden bei einer User Journey
An diesem Punkt wurde der Identitätsanbieter eingerichtet. Er ist jedoch nicht in den Bildschirmen zur Registrierung/Anmeldung verfügbar. Nun müssen Sie den LinkedIn-Kontoidentitätsanbieter der `SignUpOrSignIn`-User Journey Ihres Benutzers hinzufügen.

### <a name="step-41-make-a-copy-of-the-user-journey"></a>Schritt 4.1: Erstellen einer Kopie der User Journey
Um sie verfügbar zu machen, erstellen Sie ein Duplikat einer vorhandenen User Journey-Vorlage. Fügen Sie anschließend den LinkedIn-Identitätsanbieter hinzu:

> [!NOTE]
>
>Wenn Sie das `<UserJourneys>`-Element aus der Basisdatei Ihrer Richtlinie in die Erweiterungsdatei (TrustFrameworkExtensions.xml) kopiert haben, können Sie diesen Abschnitt überspringen.

1.  Öffnen Sie die Basisdatei Ihrer Richtlinie (z.B. „TrustFrameworkBase.xml“).
2.  Suchen Sie das `<UserJourneys>`-Element, und kopieren Sie den gesamten Inhalt des `<UserJourneys>`-Knotens.
3.  Öffnen Sie die Erweiterungsdatei (z.B. „TrustFrameworkExtensions.xml“), und suchen Sie nach dem Element `<UserJourneys>`. Wenn das Element nicht vorhanden ist, fügen Sie ein solches hinzu.
4.  Fügen Sie den gesamten Inhalt des `<UserJournesy>`-Knotens ein, den Sie als untergeordnetes Element des `<UserJourneys>`-Elements kopiert haben.

### <a name="step-42-display-the-button"></a>Schritt 4.2: Anzeigen der Schaltfläche
Das `<ClaimsProviderSelections>`-Element definiert die Liste der Anspruchsanbieter-Auswahloptionen und deren Reihenfolge.  Das `<ClaimsProviderSelection>`-Element entspricht einer Schaltfläche für einen Identitätsanbieter auf einer Registrierungs-/Anmeldeseite. Wenn Sie ein `<ClaimsProviderSelection>`-Element für das LinkedIn-Konto hinzufügen, wird eine neue Schaltfläche angezeigt, wenn ein Benutzer zu der Seite gelangt. So fügen Sie dieses Element hinzu:

1.  Suchen Sie den `<UserJourney>`-Knoten, der `Id="SignUpOrSignIn"` in der User Journey enthält, die Sie kopiert haben.
2.  Suchen Sie den `<OrchestrationStep>`-Knoten, der `Order="1"` enthält.
3.  Fügen Sie folgenden XML-Codeausschnitt unter dem `<ClaimsProviderSelections>`-Knoten hinzu:
```xml
<ClaimsProviderSelection TargetClaimsExchangeId="LinkedInExchange" />
```

### <a name="step-43-link-the-button-to-an-action"></a>Schritt 4.3: Verknüpfen der Schaltfläche mit einer Aktion
Nachdem Sie eine Schaltfläche implementiert haben, müssen Sie sie mit einer Aktion verknüpfen. In diesem Fall ist die Aktion die Kommunikation von Azure AD B2C mit dem LinkedIn-Konto, um ein Token zu empfangen. Verknüpfen Sie die Schaltfläche mit einer Aktion, indem Sie das technische Profil für Ihren LinkedIn-Kontoanspruchsanbieter verknüpfen:

1.  Suchen Sie `<OrchestrationStep>`, das `Order="2"` im `<UserJourney>`-Knoten enthält.
2.  Fügen Sie folgenden XML-Codeausschnitt unter dem `<ClaimsExchanges>`-Knoten hinzu:

    ```xml
    <ClaimsExchange Id="LinkedInExchange" TechnicalProfileReferenceId="LinkedIn-OAuth" />
    ```

    > [!NOTE]
    >
    > * Stellen Sie sicher, dass `Id` den gleichen Wert wie `TargetClaimsExchangeId` im vorherigen Abschnitt hat.
    > * Stellen Sie sicher, dass für `TechnicalProfileReferenceId`-ID das technische Profil festgelegt ist, das Sie zuvor erstellt haben (LinkedIn-OAuth).

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
3.  Sie sollten sich mit dem LinkedIn-Konto anmelden können.

## <a name="step-7-optional-register-the-linkedin-account-claims-provider-to-profile-edit-user-journey"></a>7 (Optional): Registrieren des LinkedIn-Kontoanspruchsanbieters bei der Profile-Edit-User Journey
Sie sollten den LinkedIn-Kontoidentitätsanbieter auch Ihrer Benutzer-`ProfileEdit`-User Journey hinzufügen. Um diesen verfügbar zu machen, wiederholen Sie den vierten Schritt. Wählen Sie dieses Mal den `<UserJourney>`-Knoten aus, der `Id="ProfileEdit"` enthält. Speichern Sie Ihre Richtlinie, laden Sie diese hoch, und testen Sie sie.

## <a name="download-the-complete-policy-files"></a>Herunterladen der vollständigen Richtliniendateien
Optional: Sie sollten nach Abschluss der exemplarischen Vorgehensweise „Erste Schritte mit benutzerdefinierten Richtlinien“ Ihr Szenario mithilfe Ihrer eigenen Dateien mit benutzerdefinierten Richtlinien erstellen, anstatt diese Beispieldateien zu verwenden.  [Beispielrichtliniendateien zur Referenz](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-li-app)