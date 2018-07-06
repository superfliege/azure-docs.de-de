---
title: Hinzufügen eines mehrinstanzenfähigen Azure AD-Identitätsanbieters mithilfe von benutzerdefinierten Richtlinien in Azure Active Directory B2C | Microsoft-Dokumentation
description: Hinzufügen eines mehrinstanzenfähigen Azure AD-Identitätsanbieters mithilfe von benutzerdefinierten Richtlinien – Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/14/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 2a8a23245a17c9a80c70860588a8312dbbb5e926
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37446069"
---
# <a name="azure-active-directory-b2c-allow-users-to-sign-in-to-a-multi-tenant-azure-ad-identity-provider-using-custom-policies"></a>Azure Active Directory B2C: Benutzern mithilfe von benutzerdefinierten Richtlinien die Anmeldung bei einem mehrinstanzenfähigen Azure AD-Identitätsanbieter ermöglichen

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

In diesem Artikel wird erläutert, wie Sie die Anmeldung für Benutzer des mehrinstanzenfähigen Endpunkts für Azure Active Directory (Azure AD) mithilfe von [benutzerdefinierten Richtlinien](active-directory-b2c-overview-custom.md) aktivieren. Dadurch können Benutzer von mehreren Azure AD-Mandanten sich ohne Konfiguration eines technischen Anbieters für jeden Mandanten bei Azure AD B2C anmelden. Allerdings können sich Gastmitglieder in diesen Mandanten **nicht** anmelden. Dazu müssen Sie [jeden Mandanten einzeln konfigurieren](active-directory-b2c-setup-aad-custom.md).

>[!NOTE]
> Wir verwenden „Contoso.com“ für den Azure AD-Mandanten der Organisation und „fabrikamb2c.onmicrosoft.com“ als den Azure AD B2C-Mandanten in den folgenden Anweisungen.

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie die Schritte im Artikel [Erste Schritte mit benutzerdefinierten Richtlinien](active-directory-b2c-get-started-custom.md) aus.

Diese Schritte umfassen:
     
1. Erstellen eines Azure Active Directory B2C-Mandanten (Azure AD B2C)
1. Erstellen einer Azure AD B2C-Anwendung    
1. Registrieren von zwei Richtlinienmodulanwendungen  
1. Einrichten von Schlüsseln 
1. Einrichten des Starter Pack

## <a name="step-1-create-a-multi-tenant-azure-ad-app"></a>Schritt 1: Erstellen einer mehrinstanzenfähigen Azure AD-App

Um die Anmeldung für Benutzer des Azure AD-Endpunkts für mehrere Mandanten aktivieren zu können, müssen Sie über eine mehrinstanzenfähige Anwendung verfügen, die in einem Ihrer Azure AD-Mandanten registriert ist. In diesem Artikel zeigen wir Ihnen, wie Sie eine mehrinstanzenfähige Azure AD-Anwendung in Ihrem Azure AD B2C-Mandanten erstellen. Dann können Sie die Anmeldung für Benutzer durch die Verwendung dieser mehrinstanzenfähigen Azure AD-Anwendung aktivieren.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie in der oberen Leiste Ihr Konto aus. Wählen Sie in der Liste **Verzeichnis** den Azure AD B2C-Mandanten für die Registrierung der Azure AD-Anwendung (fabrikamb2c.onmicrosoft.com) aus.
1. Wählen Sie im linken Navigationsbereich **Weitere Dienste** aus, und suchen Sie nach „App-Registrierungen“.
1. Wählen Sie **Registrierung einer neuen Anwendung** aus.
1. Geben Sie einen Namen für Ihre Anwendung ein (z.B. `Azure AD B2C App`).
1. Wählen Sie **Web-App/API** als Anwendungstyp aus.
1. Geben Sie für **Anmelde-URL** die folgende URL ein, in der `yourtenant` durch den Namen Ihres Azure AD B2C-Mandanten (`fabrikamb2c.onmicrosoft.com`) ersetzt wird:

    >[!NOTE]
    >Der Wert für „yourtenant“ muss in der **Anmelde-URL** in Kleinbuchstaben angegeben werden.

    ```
    https://login.microsoftonline.com/te/yourtenant.onmicrosoft.com/oauth2/authresp
    ```

1. Speichern Sie die Anwendungs-ID.
1. Wählen Sie die neu erstellte Anwendung aus.
1. Wählen Sie auf dem Blatt **Einstellungen** die Option **Eigenschaften** aus.
1. Legen Sie **Mehrinstanzenfähig** auf **Ja** fest.
1. Wählen Sie auf dem Blatt **Einstellungen** die Option **Schlüssel** aus.
1. Erstellen Sie einen neuen Schlüssel, und speichern Sie ihn. Sie werden ihn in den Schritten im nächsten Abschnitt verwenden.

## <a name="step-2-add-the-azure-ad-key-to-azure-ad-b2c"></a>Schritt 2: Hinzufügen des Azure AD-Schlüssels zu Azure AD B2C

Sie müssen den Anwendungsschlüssel in den Azure AD B2C-Einstellungen registrieren. Gehen Sie dazu folgendermaßen vor:

1. Wechseln Sie zum Menü „Einstellungen“ für Azure AD B2C.
1. Klicken Sie auf **Framework für die Identitätsfunktion** > **Richtlinienschlüssel**.
1. Wählen Sie **+ Hinzufügen** aus.
1. Wählen Sie diese Optionen aus, oder geben Sie sie ein:
   * Wählen Sie **Manuell** aus.
   * Wählen Sie für **Name** einen Namen aus, der mit dem Namen Ihres Azure AD-Mandanten übereinstimmt (z.B. `AADAppSecret`).  Dem Namen Ihres Schlüssels wird automatisch das Präfix `B2C_1A_` hinzugefügt.
   * Fügen Sie Ihren Anwendungsschlüssel in das Textfeld **Secret** (Geheimer Schlüssel) ein.
   * Wählen Sie **Signatur** aus.
1. Klicken Sie auf **Erstellen**.
1. Vergewissern Sie sich, dass Sie den Schlüssel `B2C_1A_AADAppSecret` erstellt haben.

## <a name="step-3-add-a-claims-provider-in-your-base-policy"></a>Schritt 3: Hinzufügen eines Anspruchsanbieters in Ihrer Basisrichtlinie

Wenn Sie möchten, dass Benutzer sich mithilfe von Azure AD anmelden, müssen Sie Azure AD als Anspruchsanbieter definieren. Das heißt, Sie müssen einen Endpunkt festlegen, mit dem Azure AD B2C kommunizieren soll. Der Endpunkt bietet eine Reihe von Ansprüchen, mit denen Azure AD B2C überprüft, ob ein bestimmter Benutzer authentifiziert wurde. 

Sie können Azure AD als Anspruchsanbieter definieren, indem Sie Azure AD in der Erweiterungsdatei Ihrer Richtlinie im `<ClaimsProvider>`-Knoten hinzufügen:

1. Öffnen Sie die Erweiterungsdatei (TrustFrameworkExtensions.xml) aus Ihrem Arbeitsverzeichnis.
1. Suchen Sie nach dem Abschnitt `<ClaimsProviders>`. Wenn dieser nicht vorhanden sein sollte, fügen Sie ihn im Stammknoten hinzu.
1. Fügen Sie einen neuen `<ClaimsProvider>`-Knoten wie folgt ein:

```XML
<ClaimsProvider>
  <Domain>commonaad</Domain>
  <DisplayName>Common AAD</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="Common-AAD">
      <DisplayName>Multi-Tenant AAD</DisplayName>
      <Protocol Name="OpenIdConnect" />
      <Metadata>
        <!-- Update the Client ID below to the Application ID -->
        <Item Key="client_id">00000000-0000-0000-0000-000000000000</Item>
        <Item Key="UsePolicyInRedirectUri">0</Item>
        <Item Key="METADATA">https://login.microsoftonline.com/common/.well-known/openid-configuration</Item>
        <Item Key="response_types">code</Item>
        <Item Key="scope">openid</Item>
        <Item Key="response_mode">form_post</Item>
        <Item Key="HttpBinding">POST</Item>
        <Item Key="DiscoverMetadataByTokenIssuer">true</Item>
        
        <!-- The key below allows you to specify each of the Azure AD tenants that can be used to sign in. Update the GUIDs below for each tenant. -->
        <Item Key="ValidTokenIssuerPrefixes">https://sts.windows.net/00000000-0000-0000-0000-000000000000,https://sts.windows.net/11111111-1111-1111-1111-111111111111</Item>

        <!-- The commented key below specifies that users from any tenant can sign-in. Uncomment if you would like anyone with an Azure AD account to be able to sign in. -->
        <!-- <Item Key="ValidTokenIssuerPrefixes">https://sts.windows.net/</Item> -->

      </Metadata>
      <CryptographicKeys>
      <!-- Make sure to update the reference ID of the client secret below you just created (B2C_1A_AADAppSecret) -->
        <Key Id="client_secret" StorageReferenceId="B2C_1A_AADAppSecret" />
      </CryptographicKeys>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
        <OutputClaim ClaimTypeReferenceId="identityProvider" PartnerClaimType="iss" />
        <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="sub" />
        <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
        <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
        <OutputClaim ClaimTypeReferenceId="surName" PartnerClaimType="family_name" />
        <OutputClaim ClaimTypeReferenceId="email" />
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

1. Aktualisieren Sie im `<ClaimsProvider>`-Knoten den Wert für `<Domain>` in einen eindeutigen Wert, der eine Unterscheidung von anderen Identitätsanbietern ermöglicht.
1. Aktualisieren Sie im `<TechnicalProfile>`-Knoten den Wert für `<DisplayName>`. Dieser Wert wird auf dem Anmeldebildschirm auf der Anmeldeschaltfläche angezeigt.
1. Aktualisieren Sie den Wert für `<Description>`.
1. Legen Sie `<Item Key="client_id">` auf die Anwendungs-ID aus der Registrierung der mehrinstanzenfähigen Azure AD-App fest.

### <a name="step-31-restrict-access-to-a-specific-list-of-azure-ad-tenants"></a>Schritt 3.1 Beschränken des Zugriffs auf eine bestimmte Liste von Azure AD-Mandanten

> [!NOTE]
> Mit `https://sts.windows.net` als Wert für **ValidTokenIssuerPrefixes** können sich ALLE Azure AD-Benutzer bei Ihrer App anmelden.

Sie müssen die Liste der gültigen Tokenaussteller aktualisieren und den Zugriff auf eine bestimmte Liste von Azure AD-Mandanten beschränken, über die sich Benutzer anmelden können. Zum Abrufen der Werte müssen Sie sich die Metadaten für jeden einzelnen Azure AD-Mandanten ansehen, über den sich die Benutzer anmelden können sollen. Das Format der Daten sieht wie folgt aus: `https://login.windows.net/yourAzureADtenant/.well-known/openid-configuration`, wobei `yourAzureADtenant` der Name Ihres Azure AD-Mandanten ist („contoso.com“ oder ein anderer Azure AD-Mandant).
1. Öffnen Sie Ihren Browser, und navigieren Sie zur Metadaten-URL.
1. Suchen Sie im Browser nach dem Objekt „Aussteller“, und kopieren Sie dessen Wert. Es sollte in etwa wie folgt aussehen: `https://sts.windows.net/{tenantId}/`.
1. Fügen Sie den Wert für den Schlüssel `ValidTokenIssuerPrefixes` ein. Sie können mehrere Werte hinzufügen, indem Sie diese durch ein Komma trennen. Ein Beispiel dafür ist im obigen XML-Beispiel auskommentiert.

## <a name="step-4-register-the-azure-ad-account-claims-provider"></a>Schritt 4: Registrieren des Azure AD-Kontoanspruchsanbieters

### <a name="step-41-make-a-copy-of-the-user-journey"></a>Schritt 4.1: Erstellen einer Kopie der User Journey

Nun müssen Sie den Azure AD-Identitätsanbieter zu einer Ihrer User Journeys hinzufügen. An dieser Stelle wurde der Identitätsanbieter eingerichtet, ist jedoch auf keinem der Registrierungs-/Anmeldebildschirme vorhanden.

Zu diesem Zweck erstellen wir ein Duplikat einer vorhandenen User Journey-Vorlage und ändern diese, damit sie ebenfalls einen Azure AD-Identitätsanbieter aufweist:

1. Öffnen Sie die Basisdatei Ihrer Richtlinie (z.B. „TrustFrameworkBase.xml“).
1. Suchen das Element `<UserJourneys>`, und kopieren Sie den gesamten `<UserJourney>`-Knoten, der `Id="SignUpOrSignIn"` enthält.
1. Öffnen Sie die Erweiterungsdatei (z.B. „TrustFrameworkExtensions.xml“), und suchen Sie nach dem Element `<UserJourneys>`. Wenn das Element nicht vorhanden ist, fügen Sie ein solches hinzu.
1. Fügen Sie den gesamten `<UserJourney>`-Knoten ein, die Sie als untergeordnetes Element des `<UserJourneys>`-Elements kopiert haben.
1. Benennen Sie die ID der neuen User Journey (z.B. als `SignUpOrSignUsingAzureAD`). 

### <a name="step-42-display-the-button"></a>Schritt 4.2: Anzeigen der Schaltfläche

Das Element `<ClaimsProviderSelection>` entspricht einer Schaltfläche für einen Identitätsanbieter auf einem Registrierungs-/Anmeldebildschirm. Wenn Sie ein `<ClaimsProviderSelection>`-Element für Azure AD hinzufügen, wird eine neue Schaltfläche angezeigt, wenn ein Benutzer zu der Seite gelangt. So fügen Sie dieses Element hinzu:

1. Suchen Sie den `<OrchestrationStep>`-Knoten, der `Order="1"` in der von Ihnen erstellten User Journey enthält.
1. Fügen Sie Folgendes hinzu:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="AzureADExchange" />
    ```

1. Legen Sie `TargetClaimsExchangeId` als entsprechenden Wert fest. Es wird empfohlen, der allgemein üblichen Konvention zu folgen: *\[NameDesAnspruchsanbieters\]Exchange*.

### <a name="step-43-link-the-button-to-an-action"></a>Schritt 4.3: Verknüpfen der Schaltfläche mit einer Aktion

Nachdem Sie eine Schaltfläche implementiert haben, müssen Sie sie mit einer Aktion verknüpfen. In diesem Fall soll bei der Aktion Azure AD B2C mit Azure AD kommunizieren, um ein Token zu empfangen. Dies gelingt Ihnen, indem Sie das technische Profil für Ihren Azure AD-Anspruchsanbieter verknüpfen:

1. Suchen Sie `<OrchestrationStep>`, das `Order="2"` im `<UserJourney>`-Knoten enthält.
1. Fügen Sie Folgendes hinzu:

    ```XML
    <ClaimsExchange Id="AzureADExchange" TechnicalProfileReferenceId="Common-AAD" />
    ```

1. Aktualisieren Sie `Id` auf den gleichen Wert wie `TargetClaimsExchangeId` im vorherigen Abschnitt.
1. Ändern Sie `TechnicalProfileReferenceId` in die ID des technischen Profils, das Sie zuvor erstellt haben („Common-AAD“).

## <a name="step-5-create-a-new-rp-policy"></a>Schritt 5: Erstellen einer neuen RP-Richtlinie

Nun müssen Sie die Datei der vertrauenden Seite (Relying Party, RP) aktualisieren, die die gerade erstellte User Journey initiiert:
 
1. Erstellen Sie eine Kopie von der Datei „SignUpOrSignIn.xml“ in Ihrem Arbeitsverzeichnis, und benennen Sie sie um (z.B. „SignUpOrSignInWithAAD.xml“).  
1. Öffnen Sie die neue Datei, und ändern Sie das `PolicyId`-Attribut für `<TrustFrameworkPolicy>` in einen eindeutigen Wert (z.B. SignUpOrSignInWithAAD). Dies ist der Name Ihrer Richtlinie (z.B. B2C\_1A\_SignUpOrSignInWithAAD). 
1. Ändern Sie das `ReferenceId`-Attribut in `<DefaultUserJourney>`, sodass es der ID der neuen von Ihnen erstellten User Journey entspricht („SignUpOrSignUsingContoso“). 
1. Speichern Sie die Änderungen, und laden Sie die Datei hoch. 

## <a name="step-6-upload-the-policy-to-your-tenant"></a>Schritt 6: Hochladen der Richtlinie zu Ihrem Mandanten

1. Wechseln Sie im [Azure-Portal](https://portal.azure.com) zum [Kontext Ihres Azure AD B2C-Mandanten](active-directory-b2c-navigate-to-b2c-context.md), und wählen Sie anschließend **Azure AD B2C** aus.
1. Wählen Sie **Framework für die Identitätsfunktion** aus.
1. Wählen Sie die Option **Alle Richtlinien** aus.
1. Wählen Sie **Richtlinie hochladen** aus.
1. Aktivieren Sie das Kontrollkästchen **Richtlinie überschreiben, sofern vorhanden**.
1. Laden Sie die Datei `TrustFrameworkExtensions.xml` und die RP-Datei (z. B. `SignUpOrSignInWithAAD.xml`) hoch, und stellen Sie sicher, dass die Dateien die Überprüfung bestehen.

## <a name="step-7-test-the-custom-policy-by-using-run-now"></a>Schritt 7: Testen der benutzerdefinierten Richtlinie mit „Jetzt ausführen“

1. Wählen Sie **Azure AD B2C-Einstellungen** und dann **Framework für die Identitätsfunktion** aus.
    > [!NOTE]
    > Für „Jetzt ausführen“ muss mindestens eine Anwendung vorab im Mandanten registriert werden. Informationen zum Registrieren von Anwendungen finden Sie in den Artikeln [Erste Schritte](active-directory-b2c-get-started.md) bzw. [Anwendungsregistrierung](active-directory-b2c-app-registration.md) zu Azure AD B2C.

1. Öffnen Sie die benutzerdefinierte Richtlinie der vertrauenden Seite (Relying Party, RP), die Sie hochgeladen haben (*B2C\_1A\_SignUpOrSignInWithAAD*). Wählen Sie dann **Jetzt ausführen** aus.
1. Sie sollten sich jetzt mit dem Azure AD-Konto anmelden können.

## <a name="optional-step-8-register-the-azure-ad-account-claims-provider-to-the-profile-edit-user-journey"></a>(Optional) Schritt 8: Registrieren des Azure AD-Kontoanspruchsanbieters bei der Profile-Edit-User Journey

Möglicherweise möchten Sie Ihrer `ProfileEdit`-User Journey auch den Azure AD-Kontoidentitätsanbieter hinzufügen. Wiederholen Sie die Schritte 4 bis 6, um die User Journey verfügbar zu machen. Wählen Sie dieses Mal den `<UserJourney>`-Knoten aus, der `Id="ProfileEdit"` enthält. Speichern Sie Ihre Richtlinie, laden Sie diese hoch, und testen Sie sie.

## <a name="troubleshooting"></a>Problembehandlung

Lesen Sie den Artikel zur [Problembehandlung](active-directory-b2c-troubleshoot-custom.md), um mehr zum Diagnostizieren von Problemen zu erfahren.

## <a name="next-steps"></a>Nächste Schritte

Senden Sie uns Ihr Feedback an [AADB2CPreview@microsoft.com](mailto:AADB2CPreview@microsoft.com).
