---
title: "Azure Active Directory B2C: Self-Service-Kennwortänderung | Microsoft-Dokumentation"
description: "In diesem Thema wird das Einrichten der Self-Service-Kennwortänderung für Ihre Kunden in Azure Active Directory B2C erläutert."
services: active-directory-b2c
documentationcenter: 
author: vigunase
manager: ajalexander
ms.assetid: 712a7128-5788-4914-8a52-24e200aa4de1
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2016
ms.author: vigunase
ms.openlocfilehash: b152c22c96da38f8724010504cc2711ab82af00a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="azure-active-directory-b2c-configure-password-change-in-custom-policies"></a>Azure Active Directory B2C: Konfigurieren der Kennwortänderung in benutzerdefinierten Richtlinien  
[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Mit dem Kennwortänderungsfeature können über lokale Konten angemeldete Kunden ihre Kennwörter ohne Authentifizierung per E-Mail-Bestätigung ändern (siehe [Azure Active Directory B2C: Einrichten der Self-Service-Kennwortrücksetzung für Ihre Kunden](active-directory-b2c-reference-sspr.md)). Sollte die Sitzung ablaufen, wenn der Kunde den Kennwortänderungsablauf erreicht, muss er sich erneut anmelden. 

## <a name="prerequisites"></a>Voraussetzungen

Ein Azure AD B2C-Mandant ist zur Durchführung der Registrierung/Anmeldung für ein lokales Konto konfiguriert, wie unter [Erste Schritte](active-directory-b2c-get-started-custom.md) beschrieben.

## <a name="how-to-configure-password-change-in-custom-policy"></a>Konfigurieren der Kennwortänderung in einer benutzerdefinierten Richtlinie

Nehmen Sie in Ihrer Richtlinie für Vertrauensframeworkerweiterungen die folgenden Änderungen vor, um die Kennwortänderung in einer benutzerdefinierten Richtlinie zu konfigurieren: 

## <a name="define-a-claimtype-oldpassword"></a>Festlegen von „ClaimType“ auf „oldPassword“

Die Gesamtstruktur Ihrer benutzerdefinierten Richtlinie muss ein `ClaimsSchema` enthalten und einen neuen `ClaimType` (oldPassword) definieren, wie hier zu sehen: 

```XML
  <BuildingBlocks>
    <ClaimsSchema>
      <ClaimType Id="oldPassword">
        <DisplayName>Old Password</DisplayName>
        <DataType>string</DataType>
        <UserHelpText>Enter password</UserHelpText>
        <UserInputType>Password</UserInputType>
      </ClaimType>
    </ClaimsSchema>
  </BuildingBlocks>
```

Der Zweck dieser Elemente lautet folgendermaßen:

- Das `ClaimsSchema` definiert, welcher Anspruch überprüft wird.  In diesem Fall wird das alte Kennwort überprüft. 

## <a name="add-a-password-change-claims-provider-with-its-supporting-elements"></a>Hinzufügen eines Anspruchsanbieters für Kennwortänderungen mit den dazugehörigen Elementen

Der Anspruchsanbieter für Kennwortänderungen führt Folgendes durch:

1. Er authentifiziert den Benutzer anhand des alten Kennworts.
2. Er speichert den Wert des neuen Kennworts im B2C-Datenspeicher, sofern das neue Kennwort und die Kennwortbestätigung identisch sind. Damit ist die Kennwortänderung abgeschlossen. 

![Abbildung](images/passwordchange.jpg)

Fügen Sie Ihrer Erweiterungsrichtlinie die folgenden Anspruchsanbieter hinzu: 

```XML
<ClaimsProviders>
    <ClaimsProvider>
      <DisplayName>Local Account SignIn</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="login-NonInteractive">
          <Metadata>
           <Item Key="client_id">ProxyIdentityExperienceFrameworkAppId</Item>
           <Item Key="IdTokenAudience">IdentityExperienceFrameworkAppId</Item>
          </Metadata>
          <InputClaims>
            <InputClaim ClaimTypeReferenceId="client_id" DefaultValue="ProxyIdentityExperienceFrameworkAppID" />
            <InputClaim ClaimTypeReferenceId="resource_id" PartnerClaimType="resource" DefaultValue="IdentityExperienceFrameworkAppID" />
          </InputClaims>
        </TechnicalProfile>
        <TechnicalProfile Id="login-NonInteractive-PasswordChange">
          <DisplayName>Local Account SignIn</DisplayName>
          <Protocol Name="OpenIdConnect" />
          <Metadata>
            <Item Key="UserMessageIfClaimsPrincipalDoesNotExist">We can't seem to find your account</Item>
            <Item Key="UserMessageIfInvalidPassword">Your password is incorrect</Item>
            <Item Key="UserMessageIfOldPasswordUsed">Looks like you used an old password</Item>
            <Item Key="ProviderName">https://sts.windows.net/</Item>
            <Item Key="METADATA">https://login.microsoftonline.com/{tenant}/.well-known/openid-configuration</Item>
            <Item Key="authorization_endpoint">https://login.microsoftonline.com/{tenant}/oauth2/token</Item>
            <Item Key="response_types">id_token</Item>
            <Item Key="response_mode">query</Item>
            <Item Key="scope">email openid</Item>
            <Item Key="UsePolicyInRedirectUri">false</Item>
            <Item Key="HttpBinding">POST</Item>
            <Item Key="client_id">ProxyIdentityExperienceFrameworkAppId</Item>
            <Item Key="IdTokenAudience">IdentityExperienceFrameworkAppId</Item>
          </Metadata>
          <InputClaims>
            <InputClaim ClaimTypeReferenceId="signInName" PartnerClaimType="username" Required="true" />
            <InputClaim ClaimTypeReferenceId="oldPassword" PartnerClaimType="password" Required="true" />
            <InputClaim ClaimTypeReferenceId="grant_type" DefaultValue="password" />
            <InputClaim ClaimTypeReferenceId="scope" DefaultValue="openid" />
            <InputClaim ClaimTypeReferenceId="nca" PartnerClaimType="nca" DefaultValue="1" />
            <InputClaim ClaimTypeReferenceId="client_id" DefaultValue="ProxyIdentityExperienceFrameworkAppID" />
            <InputClaim ClaimTypeReferenceId="resource_id" PartnerClaimType="resource" DefaultValue="IdentityExperienceFrameworkAppID" />
          </InputClaims>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="oid" />
            <OutputClaim ClaimTypeReferenceId="tenantId" PartnerClaimType="tid" />
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
            <OutputClaim ClaimTypeReferenceId="surName" PartnerClaimType="family_name" />
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
            <OutputClaim ClaimTypeReferenceId="userPrincipalName" PartnerClaimType="upn" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="localAccountAuthentication" />
          </OutputClaims>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    <ClaimsProvider>
      <DisplayName>Local Account Password Change</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="LocalAccountWritePasswordChangeUsingObjectId">
          <DisplayName>Change password (username)</DisplayName>
          <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
          <Metadata>
            <Item Key="ContentDefinitionReferenceId">api.selfasserted</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="issuer_secret" StorageReferenceId="B2C_1A_TokenSigningKeyContainer" />
          </CryptographicKeys>
          <InputClaims>
            <InputClaim ClaimTypeReferenceId="objectId" />
          </InputClaims>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="oldPassword" Required="true" />
            <OutputClaim ClaimTypeReferenceId="newPassword" Required="true" />
            <OutputClaim ClaimTypeReferenceId="reenterPassword" Required="true" />
          </OutputClaims>
          <ValidationTechnicalProfiles>
            <ValidationTechnicalProfile ReferenceId="login-NonInteractive-PasswordChange" />
            <ValidationTechnicalProfile ReferenceId="AAD-UserWritePasswordUsingObjectId" />
          </ValidationTechnicalProfiles>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
  </ClaimsProviders>
```



### <a name="add-the-application-ids-to-your-custom-policy"></a>Hinzufügen der Anwendungs-IDs zur benutzerdefinierten Richtlinie

Fügen Sie die Anwendungs-IDs zur Datei mit den Erweiterungen hinzu (`TrustFrameworkExtensions.xml`):

1. Suchen Sie in der Erweiterungsdatei („TrustFrameworkExtensions.xml“) nach den Elementen `<TechnicalProfile Id="login-NonInteractive">` und `<TechnicalProfile Id="login-NonInteractive-PasswordChange">`.

2. Ersetzen Sie alle Instanzen von `IdentityExperienceFrameworkAppId` durch die Anwendungs-ID der Identity Experience Framework-Anwendung, wie unter [Erste Schritte](active-directory-b2c-get-started-custom.md) beschrieben. Beispiel:

   ```
   <Item Key="client_id">8322dedc-cbf4-43bc-8bb6-141d16f0f489</Item>
   ```

3. Ersetzen Sie alle Instanzen von `ProxyIdentityExperienceFrameworkAppId` durch die Anwendungs-ID der Proxy Identity Experience Framework-Anwendung, wie unter [Erste Schritte](active-directory-b2c-get-started-custom.md) beschrieben.

4. Speichern Sie Ihre Erweiterungsdatei.



## <a name="create-a-password-change-user-journey"></a>Erstellen einer User Journey für die Kennwortänderung

```XML
 <UserJourneys>
    <UserJourney Id="PasswordChange">
      <OrchestrationSteps>
        <OrchestrationStep Order="1" Type="ClaimsProviderSelection" ContentDefinitionReferenceId="api.idpselections">
          <ClaimsProviderSelections>
            <ClaimsProviderSelection TargetClaimsExchangeId="LocalAccountSigninEmailExchange" />
          </ClaimsProviderSelections>
        </OrchestrationStep>
        <OrchestrationStep Order="2" Type="ClaimsExchange">
          <ClaimsExchanges>
            <ClaimsExchange Id="LocalAccountSigninEmailExchange" TechnicalProfileReferenceId="SelfAsserted-LocalAccountSignin-Email" />
          </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="3" Type="ClaimsExchange">
          <ClaimsExchanges>
            <ClaimsExchange Id="NewCredentials" TechnicalProfileReferenceId="LocalAccountWritePasswordChangeUsingObjectId" />
          </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="4" Type="SendClaims" CpimIssuerTechnicalProfileReferenceId="JwtIssuer" />
      </OrchestrationSteps>
      <ClientDefinition ReferenceId="DefaultWeb" />
    </UserJourney>
  </UserJourneys>
```

Die Änderung der Erweiterungsdatei ist abgeschlossen. Speichern Sie diese Datei, und laden Sie sie hoch. Stellen Sie sicher, dass alle Überprüfungen erfolgreich waren.



## <a name="create-a-relying-party-rp-file"></a>Erstellen einer Datei der vertrauenden Seite (Relying Party, RP)

Aktualisieren Sie als Nächstes die Datei der vertrauenden Seite (Relying Party, RP), die die User Journey initiiert, die Sie erstellt haben:

1. Erstellen Sie in Ihrem Arbeitsverzeichnis eine Kopie von „ProfileEdit.xml“. Benennen Sie sie anschließend um (beispielsweise in „PasswordChange.xml“).
2. Öffnen Sie die neue Datei, und ändern Sie das `PolicyId`-Attribut für `<TrustFrameworkPolicy>` in einen eindeutigen Wert. Dies ist der Name Ihrer Richtlinie (beispielsweise „PasswordChange“).
3. Ändern Sie das `ReferenceId`-Attribut in `<DefaultUserJourney>`, sodass es der `Id` der neuen User Journey entspricht, die Sie erstellt haben (beispielsweise „PasswordChange“).
4. Speichern Sie die Änderungen, und laden Sie die Datei dann hoch.
5. Um die benutzerdefinierte Richtlinie zu testen, die Sie hochgeladen haben, wechseln Sie im Azure-Portal zum Richtlinienblatt, und klicken Sie anschließend auf **Jetzt ausführen**.




## <a name="link-to-password-change-sample-policy"></a>Link zur Beispielrichtlinie für die Kennwortänderung

Die Beispielrichtlinie finden Sie [hier](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/password-change). 










