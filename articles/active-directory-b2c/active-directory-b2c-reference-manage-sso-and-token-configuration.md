---
title: Verwalten von SSO und der Tokenanpassung mit benutzerdefinierten Richtlinien in Azure Active Directory B2C | Microsoft-Dokumentation
description: Erfahren Sie mehr über die Verwaltung von SSO und der Tokenanpassung mit benutzerdefinierten Richtlinien.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 05/02/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 43e392979c50d340a10575898edb25b119e1410b
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34712231"
---
# <a name="azure-active-directory-b2c-manage-sso-and-token-customization-with-custom-policies"></a>Azure Active Directory B2C: Verwalten von SSO und der Tokenanpassung mit benutzerdefinierten Richtlinien
Bei Verwendung von benutzerdefinierten Richtlinien haben Sie die gleiche Kontrolle über Ihre Konfigurationen für die Bereiche für Token, Sitzung und einmaliges Anmelden (Single Sign-On, SSO) wie mit integrierten Richtlinien.  Informationen zu den einzelnen Einstellungen finden Sie in [dieser Dokumentation](#active-directory-b2c-token-session-sso).

## <a name="token-lifetimes-and-claims-configuration"></a>Tokengültigkeitsdauern und Anspruchskonfiguration
Zum Ändern der Einstellungen für Ihre Tokengültigkeitsdauern müssen Sie in der Datei der vertrauenden Seite für die gewünschte Richtlinie ein `<ClaimsProviders>`-Element hinzufügen.  Das `<ClaimsProviders>`-Element ist ein untergeordnetes Element von `<TrustFrameworkPolicy>`.  Sie müssen dort die Informationen einfügen, die sich auf die Gültigkeitsdauer Ihrer Token auswirken.  Die XML sieht wie im folgenden Beispiel aus:

```XML
<ClaimsProviders>
   <ClaimsProvider>
      <DisplayName>Token Issuer</DisplayName>
      <TechnicalProfiles>
         <TechnicalProfile Id="JwtIssuer">
            <Metadata>
               <Item Key="token_lifetime_secs">3600</Item>
               <Item Key="id_token_lifetime_secs">3600</Item>
               <Item Key="refresh_token_lifetime_secs">1209600</Item>
               <Item Key="rolling_refresh_token_lifetime_secs">7776000</Item>
               <Item Key="IssuanceClaimPattern">AuthorityAndTenantGuid</Item>
               <Item Key="AuthenticationContextReferenceClaimPattern">None</Item>
            </Metadata>
         </TechnicalProfile>
      </TechnicalProfiles>
   </ClaimsProvider>
</ClaimsProviders>
```

**Access token lifetimes (Gültigkeitsdauer Zugriffstoken)** – Sie können die Gültigkeitsdauer des Zugriffstokens ändern, indem Sie den Wert in `<Item>` mit Key="token_lifetime_secs" anpassen (Sekunden).  Der integrierte Standardwert beträgt 3.600 Sekunden (60 Minuten).

**ID token lifetime (Gültigkeitsdauer ID-Token)** – Sie können die Gültigkeitsdauer des ID-Tokens ändern, indem Sie den Wert in `<Item>` mit Key="id_token_lifetime_secs" anpassen (Sekunden).  Der integrierte Standardwert beträgt 3.600 Sekunden (60 Minuten).

**Refresh token lifetime (Gültigkeitsdauer Aktualisierungstoken)** – Sie können die Gültigkeitsdauer des Aktualisierungstokens ändern, indem Sie den Wert in `<Item>` mit Key="refresh_token_lifetime_secs" anpassen (Sekunden).  Der integrierte Standardwert beträgt 1.209.600 Sekunden (14 Tage).

**Refresh token sliding window lifetime (Gültigkeitsdauer für gleitendes Fenster des Aktualisierungstokens)** – Wenn Sie eine Gültigkeitsdauer für das gleitende Fenster des Aktualisierungstokens festlegen möchten, ändern Sie den Wert in `<Item>` mit Key="rolling_refresh_token_lifetime_secs" (Sekunden).  Der integrierte Standardwert beträgt 7.776.000 Sekunden (90 Tage).  Falls Sie keine Gültigkeitsdauer für das gleitende Fenster erzwingen möchten, ersetzen Sie diese Zeile wie folgt:
```XML
<Item Key="allow_infinite_rolling_refresh_token">True</Item>
```

**Issuer (iss) claim (Ausstelleranspruch)** – Wenn Sie den Ausstelleranspruch ändern möchten, ändern Sie den Wert in `<Item>` mit Key="IssuanceClaimPattern".  Die gültigen Werte sind `AuthorityAndTenantGuid` und `AuthorityWithTfp`.

**Setting claim representing policy ID (Festlegen des Anspruchs zur Darstellung der Richtlinien-ID)** – Die Optionen zum Festlegen dieses Werts sind „TFP“ (Vertrauensframework-Richtlinie) und „ACR“ (Authentifizierungskontext-Referenz).  
Wir empfehlen Ihnen, diese Option auf „TFP“ festzulegen. Stellen Sie hierfür sicher, dass `<Item>` mit Key="AuthenticationContextReferenceClaimPattern" vorhanden ist und der Wert `None` lautet.
Fügen Sie in Ihrem `<OutputClaims>`-Element dieses Element hinzu:
```XML
<OutputClaim ClaimTypeReferenceId="trustFrameworkPolicy" Required="true" DefaultValue="{policy}" />
```
Entfernen Sie für ACR `<Item>` mit Key="AuthenticationContextReferenceClaimPattern".

**Subject (sub) claim (Anspruch „Antragsteller“)** – Diese Option hat standardmäßig den Wert „ObjectID“. Gehen Sie wie folgt vor, um ihn in `Not Supported` zu ändern:

Ersetzen Sie diese Zeile: 
```XML
<OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub" />
```
Durch diese Zeile:
```XML
<OutputClaim ClaimTypeReferenceId="sub" />
```

## <a name="session-behavior-and-sso"></a>Sitzungsverhalten und SSO

Zum Ändern Ihres Sitzungsverhaltens und der SSO-Konfigurationen ist es erforderlich, dass Sie im `<RelyingParty>`-Element ein `<UserJourneyBehaviors>`-Element hinzufügen.  Auf das `<UserJourneyBehaviors>`-Element muss unmittelbar `<DefaultUserJourney>` folgen.  Das `<UserJourneyBehavors>`-Element sollte Folgendes enthalten:

```XML
<UserJourneyBehaviors>
   <SingleSignOn Scope="Application" />
   <SessionExpiryType>Absolute</SessionExpiryType>
   <SessionExpiryInSeconds>86400</SessionExpiryInSeconds>
</UserJourneyBehaviors>
```
**Single sign-on (SSO) configuration (Einmaliges Anmelden (SSO) – Konfiguration)** – Zum Ändern der Konfiguration des einmaligen Anmeldens müssen Sie den Wert von `<SingleSignOn>` ändern.  Die gültigen Werte sind `Tenant`, `Application`, `Policy` und `Disabled`. 

**Web app session lifetime (minutes) (Gültigkeitsdauer der Web-App-Sitzung (Minuten))** – Zum Ändern der Gültigkeitsdauer der Web-App-Sitzung müssen Sie den Wert des `<SessionExpiryInSeconds>`-Elements ändern.  Der Standardwert von integrierten Richtlinien beträgt 86.400 Sekunden (1.440 Minuten).

**Web app session timeout (Timeout für Web-App-Sitzung)** – Zum Ändern der Zeitüberschreitung für die Web-App-Sitzung müssen Sie den Wert von `<SessionExpiryType>` ändern.  Die gültigen Werte sind `Absolute` und `Rolling`.
