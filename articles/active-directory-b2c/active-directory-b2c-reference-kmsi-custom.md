---
title: "Azure Active Directory B2C: „Angemeldet bleiben“ | Microsoft-Dokumentation"
description: "Ein Thema, in dem veranschaulicht wird, wie „Angemeldet bleiben“ eingerichtet werden kann"
services: active-directory-b2c
documentationcenter: 
author: vigunase
manager: ajalexander
ms.assetid: 926e9711-71c0-49e8-b658-146ffb7386c0
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2016
ms.author: vigunase
ms.openlocfilehash: d09e15c6f6765eac436f573f89c6703039cd8397
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="azure-active-directory-b2c-enable-keep-me-signed-in-kmsi"></a>Azure Active Directory B2C: Aktivieren von „Angemeldet bleiben“  
[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Mit Azure AD B2C können Ihre Webanwendungen und nativen Anwendungen jetzt die Funktion „Angemeldet bleiben“ aktivieren. Dieses Feature gewährt wiederkehrenden Benutzern Zugriff auf Anwendungen, ohne dass diese dazu aufgefordert werden, ihren Benutzernamen und das zugehörige Kennwort erneut einzugeben. Dieser Zugriff wird widerrufen, wenn sich der Benutzer abmeldet. 

Benutzern wird davon abgeraten, diese Option auf öffentlichen Computern zu aktivieren. 

![Abbildung](images/kmsi.PNG)


## <a name="prerequisites"></a>Voraussetzungen

Ein Azure AD B2C-Mandant ist zur Zulassung der Registrierung/Anmeldung für ein lokales Konto konfiguriert, wie unter [Erste Schritte](active-directory-b2c-get-started-custom.md) beschrieben.

## <a name="how-to-enable-kmsi"></a>Aktivieren von „Angemeldet bleiben“

Nehmen Sie folgende Änderungen in Ihrer Richtlinie für Erweiterungen des Vertrauensframeworks vor.

## <a name="adding-a-content-definition-element"></a>Hinzufügen eines Inhaltsdefinitionselements 

Der Knoten `BuildingBlocks` Ihrer Erweiterungsdatei muss das Element `ContentDefinitions` enthalten. 

1. Definieren Sie im Abschnitt `ContentDefinitions` eine neue `ContentDefinition` mit der ID `api.signuporsigninwithkmsi`.
2. Ihre neue `ContentDefinition` muss wie folgt ein `LoadUri`, ein `RecoveryUri` und ein `DataUri` beinhalten.
3. Datauri`urn:com:microsoft:aad:b2c:elements:unifiedssp:1.1.0` ist ein maschinenlesbarer Bezeichner, der auf den Anmeldeseiten ein Kontrollkästchen für „Angemeldet bleiben“ hervorbringt. Stellen Sie sicher, dass Sie diesen Wert nicht ändern. 

```XML
  <BuildingBlocks>
    <ContentDefinitions>
      <ContentDefinition Id="api.signuporsigninwithkmsi">
        <LoadUri>~/tenant/default/unified.cshtml</LoadUri>
        <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
        <DataUri>urn:com:microsoft:aad:b2c:elements:unifiedssp:1.1.0</DataUri>
        <Metadata>
          <Item Key="DisplayName">Signin and Signup</Item>
        </Metadata>
      </ContentDefinition>
    </ContentDefinitions>
  </BuildingBlocks>                       
```



## <a name="add-a--local-account-sign-in-claims-provider"></a>Hinzufügen eines Anspruchsanbieters für die Anmeldung mit lokalem Konto 

Sie können „Anmeldung mit lokalem Konto“ als Anspruchsanbieter für den Knoten `<ClaimsProvider>` in der Erweiterungsdatei Ihrer Richtlinie definieren:

1. Öffnen Sie die Erweiterungsdatei (TrustFrameworkExtensions.xml) aus Ihrem Arbeitsverzeichnis. 
2. Suchen Sie nach dem Abschnitt `<ClaimsProviders>`. Wenn dieser nicht vorhanden sein sollte, fügen Sie ihn im Stammknoten hinzu.
3. Im Lieferumfang des Starter Packs von [Erste Schritte](active-directory-b2c-get-started-custom.md) ist der Anspruchsanbieter „Anmeldung mit lokalem Konto“ inbegriffen. 
4. Ist dies nicht der Fall, fügen Sie wie folgt einen neuen `<ClaimsProvider>`-Knoten hinzu:

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
            <InputClaim ClaimTypeReferenceId="client_id" DefaultValue="ProxyIdentityExperienceFrameworkAppID" />
            <InputClaim ClaimTypeReferenceId="resource_id" PartnerClaimType="resource" DefaultValue="IdentityExperienceFrameworkAppID" />
           </InputClaims>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
 </ClaimsProviders>
```

### <a name="add-the-application-ids-to-your-custom-policy"></a>Hinzufügen der Anwendungs-IDs zur benutzerdefinierten Richtlinie

Fügen Sie die Anwendungs-IDs zur Datei mit den Erweiterungen hinzu (`TrustFrameworkExtensions.xml`):

1. Suchen Sie in der Erweiterungsdatei („TrustFrameworkExtensions.xml“) nach dem Element `<TechnicalProfile Id="login-NonInteractive">` und `<TechnicalProfile Id="login-NonInteractive-PasswordChange">`

2. Ersetzen Sie alle Instanzen von `IdentityExperienceFrameworkAppId` durch die Anwendungs-ID der Identity Experience Framework-Anwendung, wie unter [Erste Schritte](active-directory-b2c-get-started-custom.md) beschrieben. Beispiel:

   ```
   <Item Key="client_id">8322dedc-cbf4-43bc-8bb6-141d16f0f489</Item>
   ```

3. Ersetzen Sie alle Instanzen von `ProxyIdentityExperienceFrameworkAppId` durch die Anwendungs-ID der Proxy Identity Experience Framework-Anwendung, wie unter [Erste Schritte](active-directory-b2c-get-started-custom.md) beschrieben.

4. Speichern Sie Ihre Erweiterungsdatei.

## <a name="create-a-kmsi-in-enabled-user-journey"></a>Erstellen von „Angemeldet bleiben“ in aktivierter User Journey

Sie müssen jetzt den Anspruchsanbieter „Anmeldung mit lokalem Konto“ zu Ihrer User Journey hinzufügen. 

1. Öffnen Sie die Basisdatei Ihrer Richtlinie (z.B. „TrustFrameworkBase.xml“).
2. Suchen das Element `<UserJourneys>`, und kopieren Sie den gesamten `<UserJourney>`-Knoten, der `Id="SignUpOrSignIn"` enthält.
3. Öffnen Sie die Erweiterungsdatei (z.B. „TrustFrameworkExtensions.xml“), und suchen Sie nach dem Element `<UserJourneys>`. Wenn das Element nicht vorhanden ist, fügen Sie ein solches hinzu.
4. Fügen Sie den gesamten `<UserJourney>`-Knoten ein, die Sie als untergeordnetes Element des `<UserJourneys>`-Elements kopiert haben.
5. Benennen Sie die ID der neuen User Journey (z.B. als `SignUpOrSignInWithKmsi`).
6. Ändern Sie schließlich in `OrchestrationStep 1` die `ContentDefinitionReferenceId` in `api.signuporsigninwithkmsi`, die Sie in den vorherigen Schritten definiert haben. Dadurch wird das Kontrollkästchen in der User Journey aktiviert. 
7. Die Änderung der Erweiterungsdatei ist abgeschlossen. Speichern Sie diese Datei, und laden Sie sie hoch. Stellen Sie sicher, dass alle Überprüfungen erfolgreich waren.

```XML
<UserJourneys>
    <UserJourney Id="SignUpOrSignInWithKmsi">
      <OrchestrationSteps>
        <OrchestrationStep Order="1" Type="CombinedSignInAndSignUp" ContentDefinitionReferenceId="api.signuporsigninwithkmsi">
          <ClaimsProviderSelections>
            <ClaimsProviderSelection ValidationClaimsExchangeId="LocalAccountSigninEmailExchange" />
          </ClaimsProviderSelections>
          <ClaimsExchanges>
            <ClaimsExchange Id="LocalAccountSigninEmailExchange" TechnicalProfileReferenceId="SelfAsserted-LocalAccountSignin-Email" />
          </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="2" Type="ClaimsExchange">
          <Preconditions>
            <Precondition Type="ClaimsExist" ExecuteActionsIf="true">
              <Value>objectId</Value>
              <Action>SkipThisOrchestrationStep</Action>
            </Precondition>
          </Preconditions>
          <ClaimsExchanges>
            <ClaimsExchange Id="SignUpWithLogonEmailExchange" TechnicalProfileReferenceId="LocalAccountSignUpWithLogonEmail" />
          </ClaimsExchanges>
        </OrchestrationStep>
        <!-- This step reads any user attributes that we may not have received when in the token. -->
        <OrchestrationStep Order="3" Type="ClaimsExchange">
          <ClaimsExchanges>
            <ClaimsExchange Id="AADUserReadWithObjectId" TechnicalProfileReferenceId="AAD-UserReadUsingObjectId" />
          </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="4" Type="SendClaims" CpimIssuerTechnicalProfileReferenceId="JwtIssuer" />
      </OrchestrationSteps>
      <ClientDefinition ReferenceId="DefaultWeb" />
    </UserJourney>
  </UserJourneys>
```

## <a name="create-a-relying-party-rp-file"></a>Erstellen einer Datei der vertrauenden Seite (Reyling Party, RP)

Aktualisieren Sie als Nächstes die Datei der vertrauenden Seite (Relying Party, RP), die die User Journey initiiert, die Sie erstellt haben:

1. Erstellen Sie in Ihrem Arbeitsverzeichnis eine Kopie von „SignUpOrSignIn.xml“. Benennen Sie sie anschließend um (z.B. in „SignUpOrSignInWithKmsi.xml“).

2. Öffnen Sie die neue Datei, und ändern Sie das `PolicyId`-Attribut für `<TrustFrameworkPolicy>` in einen eindeutigen Wert. Dies ist der Name Ihrer Richtlinie (z.B. SignUpOrSignInWithKmsi).

3. Ändern Sie das Attribut `ReferenceId` in `<DefaultUserJourney>`, sodass es der `Id` der neuen User Journey entspricht, die Sie erstellt haben (z.B. „SignUpOrSignInWithKmsi“).

4. „Angemeldet bleiben“ wird in `UserJourneyBehaviors` konfiguriert. 

5. **`KeepAliveInDays`** steuert, wie lange der Benutzer angemeldet bleibt. Im folgenden Beispiel läuft die Sitzung „Angemeldet bleiben“ nach 14 Tagen automatisch ab, unabhängig davon, wie häufig der Benutzer die Authentifizierung im Hintergrund ausführt.

   Durch Festlegen des Werts `KeepAliveInDays` auf 0 wird die Funktion „Angemeldet bleiben“ deaktiviert. Standardmäßig ist dieser Wert auf 0 festgelegt.

6. Wenn **`SessionExpiryType`** auf *Parallel* festgelegt ist, wird die Sitzung „Angemeldet bleiben“ jedes Mal um 14 Tage erweitert, wenn der Benutzer die Authentifizierung im Hintergrund durchführt.  Wenn *Parallel* ausgewählt ist, wird empfohlen, die Anzahl von Tagen auf einem Minimum zu halten. 

       <RelyingParty>
       <DefaultUserJourney ReferenceId="SignUpOrSignInWithKmsi" />
       <UserJourneyBehaviors>
         <SingleSignOn Scope="Tenant" KeepAliveInDays="14" />
         <SessionExpiryType>Absolute</SessionExpiryType>
         <SessionExpiryInSeconds>1200</SessionExpiryInSeconds>
       </UserJourneyBehaviors>
       <TechnicalProfile Id="PolicyProfile">
         <DisplayName>PolicyProfile</DisplayName>
         <Protocol Name="OpenIdConnect" />
         <OutputClaims>
           <OutputClaim ClaimTypeReferenceId="displayName" />
           <OutputClaim ClaimTypeReferenceId="givenName" />
           <OutputClaim ClaimTypeReferenceId="surname" />
           <OutputClaim ClaimTypeReferenceId="email" />
           <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
         </OutputClaims>
         <SubjectNamingInfo ClaimType="sub" />
       </TechnicalProfile>
       </RelyingParty>

7. Speichern Sie die Änderungen, und laden Sie die Datei dann hoch.

8. Um die benutzerdefinierte Richtlinie zu testen, die Sie hochgeladen haben, wechseln Sie im Azure-Portal zum Richtlinienblatt, und klicken Sie anschließend auf **Jetzt ausführen**.


## <a name="link-to-sample-policy"></a>Link zu Beispielrichtlinie

Sie finden die Beispielrichtlinie [hier](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/keep%20me%20signed%20in).








