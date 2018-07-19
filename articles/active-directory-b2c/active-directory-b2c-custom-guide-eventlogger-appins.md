---
title: Nachverfolgen des Benutzerverhaltens mithilfe von Ereignissen in Application Insights von Azure Active Directory B2C | Microsoft-Dokumentation
description: Ausführliche Anleitung zum Aktivieren von Ereignisprotokollen in Application Insights von Azure AD B2C-User Journeys mit benutzerdefinierten Richtlinien (Vorschau)
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.date: 04/16/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 300a8a15552fe1e8ec9d6b434a14a31b3d827f19
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37445584"
---
# <a name="track-user-behavior-in-azure-ad-b2c-journeys-by-using-application-insights"></a>Nachverfolgen des Benutzerverhaltens in Azure AD B2C-Journeys mithilfe von Application Insights

Azure Active Directory B2C (Azure AD B2C) arbeitet gut mit Azure Application Insights zusammen. Die beiden Komponenten bieten detaillierte und angepasste Ereignisprotokolle für Ihre benutzerdefiniert erstellten User Journeys. Dieser Artikel zeigt die ersten Schritte für folgende Vorgänge:

* Gewinnen von Einblicken in Benutzerverhalten.
* Beheben von Problemen mit eigenen Richtlinien in der Entwicklung oder in der Produktion.
* Messen der Leistung.
* Erstellen von Benachrichtigungen von Application Insights.

> [!NOTE]
> Dieses Feature befindet sich in der Vorschauphase.

## <a name="how-it-works"></a>So funktioniert's

Das Framework für die Identitätsfunktion in Azure AD B2C enthält jetzt den Anbieter `Handler="Web.TPEngine.Providers.UserJourneyContextProvider, Web.TPEngine, Version=1.0.0.0`.  Er sendet Ereignisdaten mithilfe des für Azure AD B2C bereitgestellten Instrumentierungsschlüssels direkt an Application Insights.

Ein technisches Profil verwendet diesen Anbieter zum Definieren eines Ereignisses aus B2C.  Das Profil gibt den Namen des Ereignisses, die aufzuzeichnenden Ansprüche und den Instrumentierungsschlüssel an.  Um ein Ereignis zu senden, wird das technische Profil dann als `orchestration step` oder `validation technical profile` einer benutzerdefinierten User Journey hinzugefügt.

Application Insights kann die Ereignisse mithilfe einer Korrelations-ID zum Aufzeichnen einer Benutzersitzung vereinheitlichen. Application Insights macht das Ereignis und die Sitzung innerhalb von Sekunden verfügbar und stellt viele Visualisierungs-, Export- und Analysetools bereit.

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie die Schritte unter [Erste Schritte mit benutzerdefinierten Richtlinien](active-directory-b2c-get-started-custom.md) aus. In diesem Artikel wird davon ausgegangen, dass Sie das Startpaket für die benutzerdefinierte Richtlinie verwenden. Aber das Startpaket ist nicht erforderlich.

## <a name="step-1-create-an-application-insights-resource-and-get-the-instrumentation-key"></a>Schritt 1: Erstellen einer Application Insights-Ressource und Abrufen des Instrumentierungsschlüssels

Wenn Sie Application Insights mit Azure AD B2C verwenden, besteht die einzige Anforderung darin, eine Ressource zu erstellen und einen Instrumentierungsschlüssel abzurufen. Sie erstellen eine Ressource im [Azure-Portal](https://portal.azure.com).

1. Im Azure-Portal wählen Sie in Ihrem Abonnementmandanten **+ Ressource erstellen**. Bei diesem Mandanten handelt es sich nicht um Ihren Azure AD B2C-Mandanten.  
2. Suchen Sie nach **Application Insights**, und wählen Sie die Option aus.  
3. Erstellen Sie eine Ressource, die die **ASP.NET-Webanwendung** als **Anwendungstyp** verwendet, unter einem Abonnement Ihrer Wahl.
4. Nachdem Sie die Application Insights-Ressource erstellt haben, öffnen Sie sie, und notieren Sie sich den Instrumentierungsschlüssel.

![Application Insights – Übersicht und Instrumentierungsschlüssel](./media/active-directory-b2c-custom-guide-eventlogger-appins/app-ins-key.png)

## <a name="step-2-add-new-claimtype-definitions-to-your-trust-framework-extension-file"></a>Schritt 2: Hinzufügen von neuen ClaimType-Definitionen zu Ihrer Vertrauensframework-Erweiterungsdatei

Öffnen Sie die Erweiterungsdatei aus dem Starter-Pack, und fügen Sie dem Knoten `<BuildingBlocks>` die folgenden Elemente hinzu. Der Dateiname lautet in der Regel `yourtenant.onmicrosoft.com-B2C_1A_TrustFrameworkExtensions.xml`.

```xml
<ClaimsSchema>
  <ClaimType Id="EventType">
    <DisplayName>EventType</DisplayName>
    <DataType>string</DataType>
    <AdminHelpText />
    <UserHelpText />
  </ClaimType>
  <ClaimType Id="PolicyId">
    <DisplayName>PolicyId</DisplayName>
    <DataType>string</DataType>
    <AdminHelpText />
    <UserHelpText />
  </ClaimType>
  <ClaimType Id="Culture">
    <DisplayName>Culture</DisplayName>
    <DataType>string</DataType>
    <AdminHelpText />
    <UserHelpText />
  </ClaimType>
  <ClaimType Id="CorrelationId">
    <DisplayName>CorrelationId</DisplayName>
    <DataType>string</DataType>
    <AdminHelpText />
    <UserHelpText />
  </ClaimType>
  <!--Additional claims used for passing claims to Application Insights Provider -->
  <ClaimType Id="federatedUser">
    <DisplayName>federatedUser</DisplayName>
    <DataType>boolean</DataType>
    <UserHelpText />
  </ClaimType>
  <ClaimType Id="parsedDomain">
    <DisplayName>Parsed Domain</DisplayName>
    <DataType>string</DataType>
    <UserHelpText>The domain portion of the email address.</UserHelpText>
  </ClaimType>
  <ClaimType Id="userInLocalDirectory">
    <DisplayName>userInLocalDirectory</DisplayName>
    <DataType>boolean</DataType>
    <UserHelpText />
  </ClaimType>
</ClaimsSchema>
```

## <a name="step-3-add-new-technical-profiles-that-use-the-application-insights-provider"></a>Schritt 3: Hinzufügen von neuen technischen Profilen, die den Application Insights-Anbieter verwenden

Technische Profile können im Azure AD B2C-Framework für die Identitätsfunktion als Funktionen angesehen werden. In diesem Beispiel werden fünf technische Profile zum Öffnen einer Sitzung und Senden von Ereignissen definiert:

| Technisches Profil | Aufgabe |
| ----------------- | -----|
| AzureInsights-Common | Erstellt einen gemeinsamer Satz von Parametern, die in alle technischen Profile von AzureInsights einbezogen werden sollen | 
| JourneyContextForInsights | Öffnet die Sitzung in Application Insights und sendet eine Korrelations-ID |
| AzureInsights-SignInRequest | Erstellt ein `SignIn`-Ereignis mit einem Satz von Ansprüchen, wenn eine Anmeldeanforderung empfangen wird | 
| AzureInsights-UserSignup | Erstellt ein UserSignup-Ereignis, wenn der Benutzer die Registrierungsoption in einer Registrierungs-/Anmeldungsjourney auslöst | 
| AzureInsights-SignInComplete | Zeichnet den erfolgreichen Abschluss einer Authentifizierung auf, wenn ein Token an die Anwendung der vertrauenden Seite gesendet wurde | 

Fügen Sie der Erweiterungsdatei aus dem Starter-Pack die Profile hinzu, indem Sie dem Knoten `<ClaimsProviders>` die folgenden Elemente hinzufügen.  Der Dateiname lautet in der Regel `yourtenant.onmicrosoft.com-B2C_1A_TrustFrameworkExtensions.xml`.

> [!IMPORTANT]
> Ändern Sie den Instrumentierungsschlüssel im technischen Profil `ApplicationInsights-Common` in die GUID, die von Ihrer Application Insights-Ressource bereitgestellt wurde.

```xml
<ClaimsProvider>
  <DisplayName>Application Insights</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="JourneyContextForInsights">
      <DisplayName>Application Insights</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.UserJourneyContextProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="CorrelationId" />
      </OutputClaims>
    </TechnicalProfile>
    <TechnicalProfile Id="AzureInsights-SignInRequest">
      <InputClaims>
        <!-- An input claim with a PartnerClaimType="eventName" is required. This is used by the AzureApplicationInsightsProvider to create an event with the specified value. -->
        <InputClaim ClaimTypeReferenceId="EventType" PartnerClaimType="eventName" DefaultValue="SignInRequest" />
      </InputClaims>
      <IncludeTechnicalProfile ReferenceId="AzureInsights-Common" />
    </TechnicalProfile>
    <TechnicalProfile Id="AzureInsights-SignInComplete">
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="EventType" PartnerClaimType="eventName" DefaultValue="SignInComplete" />
        <InputClaim ClaimTypeReferenceId="federatedUser" PartnerClaimType="{property:FederatedUser}" DefaultValue="false" />
        <InputClaim ClaimTypeReferenceId="parsedDomain" PartnerClaimType="{property:FederationPartner}" DefaultValue="Not Applicable" />
      </InputClaims>
      <IncludeTechnicalProfile ReferenceId="AzureInsights-Common" />
    </TechnicalProfile>
    <TechnicalProfile Id="AzureInsights-UserSignup">
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="EventType" PartnerClaimType="eventName" DefaultValue="UserSignup" />
      </InputClaims>
      <IncludeTechnicalProfile ReferenceId="AzureInsights-Common" />
    </TechnicalProfile>
    <TechnicalProfile Id="AzureInsights-Common">
      <DisplayName>Alternate Email</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.Insights.AzureApplicationInsightsProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
      <Metadata>
        <!-- The ApplicationInsights instrumentation key which will be used for logging the events -->
        <Item Key="InstrumentationKey">xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx</Item>
        <!-- A Boolean that indicates whether developer mode is enabled. This controls how events are buffered. In a development environment with minimal event volume, enabling developer mode results in events being sent immediately to ApplicationInsights. -->
        <Item Key="DeveloperMode">false</Item>
        <!-- A Boolean that indicates whether telemetry should be enabled or not. -->
        <Item Key="DisableTelemetry ">false</Item>
      </Metadata>
      <InputClaims>
        <!-- Properties of an event are added through the syntax {property:NAME}, where NAME is property being added to the event. DefaultValue can be either a static value or a value that's resolved by one of the supported DefaultClaimResolvers. -->
        <InputClaim ClaimTypeReferenceId="PolicyId" PartnerClaimType="{property:Policy}" DefaultValue="{Policy:PolicyId}" />
        <InputClaim ClaimTypeReferenceId="CorrelationId" PartnerClaimType="{property:JourneyId}" />
        <InputClaim ClaimTypeReferenceId="Culture" PartnerClaimType="{property:Culture}" DefaultValue="{Culture:RFC5646}" />
      </InputClaims>
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="step-4-add-the-technical-profiles-for-application-insights-as-orchestration-steps-in-an-existing-user-journey"></a>Schritt 4: Hinzufügen der technischen Profile für Application Insights als Orchestrierungsschritte in einer vorhandenen User Journey

Rufen Sie `JournyeContextForInsights` als Orchestrierungsschritt 1 auf:

```xml
<!-- Initialize a session with Application Insights -->
<OrchestrationStep Order="1" Type="ClaimsExchange">
  <ClaimsExchanges>
    <ClaimsExchange Id="JourneyContextForInsights" TechnicalProfileReferenceId="JourneyContextForInsights" />
  </ClaimsExchanges>
</OrchestrationStep>
```

Rufen Sie `Azure-Insights-SignInRequest` als Orchestrierungsschritt 2 auf, um nachzuverfolgen, ob eine Anmeldungs-/Registrierungsanforderung empfangen wurde:

```xml
<!-- Track that we have received a sign in request -->
<OrchestrationStep Order="2" Type="ClaimsExchange">
  <ClaimsExchanges>
    <ClaimsExchange Id="TrackSignInRequest" TechnicalProfileReferenceId="AzureInsights-SignInRequest" />
  </ClaimsExchanges>
</OrchestrationStep>
```

Fügen Sie direkt *vor* dem Orchestrierungsschritt `SendClaims` einen neuen Schritt ein, der `Azure-Insights-UserSignup` aufruft. Es wird ausgelöst, wenn der Benutzer die Registrierungsschaltfläche in einer Registrierungs-/Anmeldungsjourney auswählt.

```xml
<!-- Handles the user clicking the sign up link in the local account sign in page -->
<OrchestrationStep Order="9" Type="ClaimsExchange">
  <Preconditions>
    <Precondition Type="ClaimsExist" ExecuteActionsIf="false">
      <Value>newUser</Value>
      <Action>SkipThisOrchestrationStep</Action>
    </Precondition>
    <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
      <Value>newUser</Value>
      <Value>false</Value>
      <Action>SkipThisOrchestrationStep</Action>
    </Precondition>
  </Preconditions>
  <ClaimsExchanges>
    <ClaimsExchange Id="TrackUserSignUp" TechnicalProfileReferenceId="AzureInsights-UserSignup" />
  </ClaimsExchanges>
```

Rufen Sie direkt nach dem Orchestrierungsschritt `SendClaims` den Befehl `Azure-Insights-SignInComplete` auf. Dieser Schritt weist auf eine erfolgreich abgeschlossene Journey hin.

```xml
<!-- Track that we have successfully sent a token -->
<OrchestrationStep Order="11" Type="ClaimsExchange">
  <ClaimsExchanges>
    <ClaimsExchange Id="TrackSignInComplete" TechnicalProfileReferenceId="AzureInsights-SignInComplete" />
  </ClaimsExchanges>
</OrchestrationStep>
```

> [!IMPORTANT]
> Nummerieren Sie nach dem Hinzufügen der neuen Orchestrierungsschritte die Schritte nacheinander von 1 bis N neu, ohne einen Integer zu überspringen.


## <a name="step-5-upload-your-modified-extensions-file-run-the-policy-and-view-events-in-application-insights"></a>Schritt 5: Hochladen Ihrer geänderten Erweiterungsdatei, Ausführen der Richtlinie und Anzeigen der Ereignisse in Application Insights

Speichern Sie die neue Vertrauensframework-Erweiterungsdatei, und laden Sie sie hoch. Rufen Sie dann die Richtlinie für die vertrauende Seite Ihrer Anwendung ab, oder verwenden Sie `Run Now` in der Azure AD B2C-Schnittstelle. In wenigen Sekunden sind die Ereignisse in Application Insights verfügbar.

1. Öffnen Sie die **Application Insights**-Ressource in Ihrem Azure Active Directory-Mandanten.
2. Wählen Sie **Verwendung** > **Ereignisse** aus.
3. Legen Sie **Während** auf **letzte Stunde** und **Für** auf **3 Minuten** fest.  Sie müssen möglicherweise **Aktualisieren** auswählen, um Ihre Ergebnisse anzuzeigen.

![Application Insights-Nutzungsereignisbasis](./media/active-directory-b2c-custom-guide-eventlogger-appins/app-ins-graphic.png)

##  <a name="next-steps"></a>Nächste Schritte

Fügen Sie Ihrer User Journey entsprechend Ihren Anforderungen Anspruchstypen und Ereignisse hinzu. Im Folgenden finden Sie eine Liste möglicher Ansprüche unter Verwendung zusätzlicher Anspruchskonfliktlöser.

### <a name="culture-specific-claims"></a>Kulturspezifische Ansprüche

```xml
Referenced using: {Culture:One of the property names below}
```

| Anspruch | Definition | Beispiel |
| ----- | -----------| --------|
| LanguageName | Der aus zwei Buchstaben bestehende ISO-Code für die Sprache | en |
| RegionName | Der aus zwei Buchstaben bestehende ISO-Code für die Region | US |
| RFC5646 | Der Sprachcode RFC5646 | en-US |
| LCID   | Die LCID des Sprachcodes | 1033 |

### <a name="policy-specific-claims"></a>Richtlinienspezifische Ansprüche

```xml
Referenced using {Policy:One of the property names below}
```

| Anspruch | Definition | Beispiel |
| ----- | -----------| --------|
| TrustFrameworkTenantId | Die TrustFramework-Mandanten-ID | N/V |
| RelyingPartyTenantId | Die Mandanten-ID der vertrauenden Seite | N/V |
| PolicyId | Die ID der Richtlinie | N/V |
| TenantObjectId | Die Mandantenobjekt-ID der Richtlinie | N/V |

### <a name="openid-connect-specific-claims"></a>OpenID Connect-spezifische Ansprüche

```xml
Referenced using {OIDC:One of the property names below}
```

| Anspruch | OpenIdConnect-Parameter | Beispiel |
| ----- | ----------------------- | --------|
| Prompt | prompt | N/V |
| LoginHint |  login_hint | N/V |
| DomainHint | domain_hint | N/V |
|  MaxAge | max_age | N/V |
| ClientId | client_id | N/V |
| Username | login_hint | N/V |
|  Ressource | resource| N/V |
| AuthenticationContextReferences | acr_values | N/V |

### <a name="non-protocol-parameters-included-with-oidc--oauth2-requests"></a>In OIDC- und OAuth2-Anforderungen enthaltene protokollfremde Parameter

```xml
Referenced using { OAUTH-KV:Querystring parameter name }
```

Jeder Parametername, der als Bestandteil einer OIDC- oder OAuth2-Anforderung eingeschlossen wird, kann einem Anspruch in der User Journey zugeordnet werden. Sie können ihn dann im Ereignis aufzeichnen. Beispielsweise kann die Anforderung von der Anwendung einen Abfragezeichenfolgen-Parameter mit einem der Namen `app_session`, `loyalty_number` oder `any_string` enthalten.

Hier ist eine Beispielanforderung aus der Anwendung:

```
https://login.microsoftonline.com/sampletenant.onmicrosoft.com/oauth2/v2.0/authorize?p=B2C_1A_signup_signin&client_id=e1d2612f-c2bc-4599-8e7b-d874eaca1ae1&nonce=defaultNonce&redirect_uri=https%3A%2F%2Fjwt.ms&scope=openid&response_type=id_token&prompt=login&app_session=0a2b45c&loyalty_number=1234567

```
Sie können dann die Ansprüche hinzufügen, indem Sie dem Application Insights-Ereignis ein `Input Claim`-Element hinzufügen. Die Eigenschaften eines Ereignisses werden über die Syntax {property:NAME} hinzugefügt, wobei NAME die Eigenschaft ist, die zum Ereignis hinzugefügt wird. Beispiel: 

```
<InputClaim ClaimTypeReferenceId="app_session" PartnerClaimType="{property:app_session}" DefaultValue="{OAUTH-KV:app_session}" />
<InputClaim ClaimTypeReferenceId="loyalty_number" PartnerClaimType="{property:loyalty_number}" DefaultValue="{OAUTH-KV:loyalty_number}" />
```

### <a name="other-system-claims"></a>Sonstige Systemansprüche

Einige Systemansprüche müssen dem Anspruchsbehälter hinzugefügt werden, bevor sie als Ereignisse aufgezeichnet werden können. Das technische Profil `SimpleUJContext` muss als eine Orchestrierungsschritt oder technisches Validierungsprofil aufgerufen werden, bevor diese Ansprüche verfügbar sind.

```xml
<ClaimsProvider>
  <DisplayName>User Journey Context Provider</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="SimpleUJContext">
      <DisplayName>User Journey Context Provide</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.UserJourneyContextProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="IP-Address" />
        <OutputClaim ClaimTypeReferenceId="CorrelationId" />
        <OutputClaim ClaimTypeReferenceId="DateTimeInUtc" />
        <OutputClaim ClaimTypeReferenceId="Build" />
      </OutputClaims>
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```


