---
title: Hinzufügen von AD FS als SAML-Identitätsanbieter mithilfe benutzerdefinierter Richtlinien in Azure Active Directory B2C | Microsoft-Dokumentation
description: Einrichten von AD FS 2016 mit dem SAML-Protokoll und benutzerdefinierten Richtlinien in Azure Active Directory B2C
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/31/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 2c2e6861fda42a9e8c1aabcba303bfede47ac3c1
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2018
ms.locfileid: "43669225"
---
# <a name="add-adfs-as-a-saml-identity-provider-using-custom-policies-in-azure-active-directory-b2c"></a>Hinzufügen von AD FS als SAML-Identitätsanbieter mithilfe benutzerdefinierter Richtlinien in Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

In diesem Artikel erfahren Sie, wie Sie die Anmeldung für ein AD FS-Benutzerkonto mithilfe [benutzerdefinierter Richtlinien](active-directory-b2c-overview-custom.md) in Azure Active Directory (Azure AD) B2C aktivieren.

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie die Schritte im Artikel [Erste Schritte mit benutzerdefinierten Richtlinien](active-directory-b2c-get-started-custom.md) aus.

## <a name="add-the-adfs-account-application-key-to-azure-ad-b2c"></a>Hinzufügen des AD FS-Kontoanwendungsschlüssels zu Azure AD B2C

Ein Verbund mit einem AD FS-Konto erfordert einen geheimen Clientschlüssel für das Konto, um Azure AD B2C im Auftrag der Anwendung zu vertrauen. Sie müssen Ihr AD FS-Zertifikat in Ihrem Azure AD B2C-Mandanten speichern. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Vergewissern Sie sich, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält, indem Sie oben rechts im Azure-Portal zu diesem Verzeichnis wechseln. Wählen Sie **Verzeichnis wechseln** und dann das Verzeichnis mit dem erstellten Mandanten aus. In diesem Tutorial wird das Verzeichnis *contoso* verwendet, das den Mandanten *contoso0522Tenant.onmicrosoft.com* enthält.

    ![Wechseln von Verzeichnissen](./media/active-directory-b2c-custom-setup-adfs2016-idp/switch-directories.png)

3. Klicken Sie links oben im Azure-Portal auf **Alle Dienste**, suchen Sie nach **Azure AD B2C**, und klicken Sie darauf. Sie sollten nun Ihren Mandanten verwenden.
4. Wählen Sie auf der Seite „Übersicht“ die Option **Framework für die Identitätsfunktion** aus.
5. Wählen Sie **Richtlinienschlüssel** aus, um die in Ihrem Mandanten verfügbaren Schlüssel anzuzeigen, und klicken Sie dann auf **Hinzufügen**.
6. Wählen Sie **Hochladen** als Option aus.
7. Geben Sie `ADFSSamlCert` als Namen ein. Das Präfix `B2C_1A_` wird möglicherweise automatisch hinzugefügt.
8. Navigieren Sie zur PFX-Datei Ihres Zertifikats mit dem privaten Schlüssel, und wählen Sie sie aus. Dieses Zertifikat mit dem privaten Schlüssel sollte das gleiche sein, das für die vertrauende AD FS-Seite ausgestellt wurde und verwendet wird.
9. Klicken Sie auf **Erstellen**, und vergewissern Sie sich, dass Sie den Schlüssel `B2C_1A_ADFSSamlCert` erstellt haben.

## <a name="add-a-claims-provider-in-your-extension-policy"></a>Hinzufügen eines Anspruchsanbieters in Ihrer Erweiterungsrichtlinie

Wenn Sie möchten, dass Benutzer sich mithilfe eines AD FS-Kontos anmelden, müssen Sie das Konto als Anspruchsanbieter definieren. Dazu geben Sie einen Endpunkt an, mit dem Azure AD B2C kommuniziert. Der Endpunkt bietet eine Reihe von Ansprüchen, mit denen Azure AD B2C überprüft, ob ein bestimmter Benutzer authentifiziert wurde.

Definieren Sie AD FS als Anspruchsanbieter, indem Sie das **ClaimsProvider**-Element in Ihrer Erweiterungsrichtliniendatei hinzufügen.

1. Öffnen Sie die Richtliniendatei *TrustFrameworkExtensions.xml* in Ihrem Arbeitsverzeichnis. Wenn Sie einen XML-Editor benötigen, können Sie [Visual Studio Code](https://code.visualstudio.com/download), einen einfachen plattformübergreifenden Editor, ausprobieren.
2. Fügen Sie den folgenden XML-Code unter dem **ClaimsProviders**-Element hinzu, und ersetzen Sie **your-ADFS-domain** durch Ihren AD FS-Domänennamen sowie den Wert des **identityProvider**-Ausgabeanspruch durch Ihr DNS (beliebiger Wert, der Ihre Domäne angibt), und speichern Sie die Datei. 

    ```xml
    <ClaimsProvider>
      <Domain>contoso.com</Domain>
      <DisplayName>Contoso ADFS</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="Contoso-SAML2">
          <DisplayName>Contoso ADFS</DisplayName>
          <Description>Login with your Contoso account</Description>
          <Protocol Name="SAML2"/>
          <Metadata>
            <Item Key="RequestsSigned">false</Item>
            <Item Key="WantsEncryptedAssertions">false</Item>
            <Item Key="PartnerEntity">https://your-ADFS-domain/federationmetadata/2007-06/federationmetadata.xml</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="SamlAssertionSigning" StorageReferenceId="B2C_1A_ADFSSamlCert"/>
            <Key Id="SamlMessageSigning" StorageReferenceId="B2C_1A_ADFSSamlCert"/>
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="userPrincipalName" />
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name"/>
            <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="family_name"/>
            <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email"/>
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name"/>
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="contoso.com" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication"/>
          </OutputClaims>
          <OutputClaimsTransformations>
            <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName"/>
            <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName"/>
            <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId"/>
            <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId"/>
          </OutputClaimsTransformations>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop"/>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

## <a name="register-the-claims-provider-for-sign-up-and-sign-in"></a>Registrieren des Anspruchsanbieters für die Registrierung und Anmeldung

Um den AD FS-Kontoidentitätsanbieter auf den Seiten für Registrierung und Anmeldung verfügbar zu machen, müssen Sie ihn Ihrer User Journey **SignUpOrSignIn** hinzufügen. 

Erstellen Sie eine Kopie einer vorhandenen User Journey-Vorlage, und ergänzen Sie sie um den AD FS-Identitätsanbieter:

>[!NOTE]
>Wenn Sie zuvor das **UserJourneys**-Element aus der Basisdatei Ihrer Richtlinie in die Erweiterungsdatei (*TrustFrameworkExtensions.xml*) kopiert haben, können Sie diesen Abschnitt überspringen.

1. Öffnen Sie die Basisdatei Ihrer Richtlinie. Öffnen Sie z.B. *TrustFrameworkBase.xml*.
2. Kopieren Sie den gesamten Inhalt des **UserJourneys**-Elements.
3. Öffnen Sie die Erweiterungsdatei (*TrustFrameworkExtensions.xml*), und fügen Sie den gesamten Inhalt des **UserJourneys**-Elements ein, den Sie aus der Erweiterungsdatei kopiert haben.

### <a name="display-the-button"></a>Anzeigen der Schaltfläche

Das **ClaimsProviderSelections**-Element definiert die Liste der Auswahloptionen für Anspruchsanbieter und deren Reihenfolge.  Das **ClaimsProviderSelection**-Element entspricht einer Schaltfläche für einen Identitätsanbieter auf einer Registrierungs- oder Anmeldeseite. Wenn Sie ein **ClaimsProviderSelection**-Element für ein AD FS-Konto hinzufügen, wird eine neue Schaltfläche angezeigt, wenn ein Benutzer die Seite anzeigt. So fügen Sie dieses Element hinzu:

1. Suchen Sie im **UserJourneys**-Element mit dem Bezeichner `SignUpOrSignIn` in den kopierten User Journeys das **OrchestrationStep**-Element mit `Order="1"`.
2. Fügen Sie das folgende **ClaimsProviderSelection**-Element unter dem **ClaimsProviderSelections**-Element hinzu:

    ```xml
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>Verknüpfen der Schaltfläche mit einer Aktion

Nachdem Sie eine Schaltfläche implementiert haben, müssen Sie sie mit einer Aktion verknüpfen. In diesem Fall soll bei der Aktion Azure AD B2C mit dem AD FS-Konto kommunizieren, um ein Token zu empfangen. Verknüpfen Sie die Schaltfläche mit einer Aktion, indem Sie das technische Profil für Ihren AD FS-Kontoanspruchsanbieter verknüpfen:

1. Suchen Sie den **OrchestrationStep** mit `Order="2"` unter dem **UserJourney**-Element.
2. Fügen Sie das folgende **ClaimsExchange**-Element unter dem **ClaimsExchanges**-Element hinzu:

    ```xml
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="Contoso-SAML2" />
    ```

> [!NOTE]
> * Stellen Sie sicher, dass `Id` den gleichen Wert wie `TargetClaimsExchangeId` im vorherigen Abschnitt hat.
> * Stellen Sie sicher, dass für `TechnicalProfileReferenceId` das technische Profil festgelegt ist, das Sie zuvor erstellt haben (Contoso-SAML2).


## <a name="optional-register-the-claims-provider-for-profile-edit"></a>[Optional:] Registrieren des Anspruchsanbieters für die Profilbearbeitung

Sie können den AD FS-Kontoidentitätsanbieter auch Ihrer User Journey für die Profilbearbeitung hinzufügen.

### <a name="display-the-button"></a>Anzeigen der Schaltfläche

1. Öffnen Sie die Erweiterungsdatei Ihrer Richtlinie. Beispiel: *TrustFrameworkExtensions.xml*.
2. Suchen Sie im **UserJourney**-Element mit dem Bezeichner `ProfileEdit` in den User Journeys, die Sie kopiert haben, das **OrchestrationStep**-Element mit `Order="1"`.
3. Fügen Sie das folgende **ClaimsProviderSelection**-Element unter dem **ClaimsProviderSelections**-Element hinzu:

    ```xml
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>Verknüpfen der Schaltfläche mit einer Aktion

1. Suchen Sie den **OrchestrationStep** mit `Order="2"` unter dem **UserJourney**-Element.
2. Fügen Sie das folgende **ClaimsExchange**-Element unter dem **ClaimsExchanges**-Element hinzu:

    ```xml
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="Contoso-SAML2" />
    ```

## <a name="upload-the-policy-to-your-tenant"></a>Hochladen der Richtlinie in Ihren Mandanten

1. Wählen Sie im Azure-Portal die Option **Alle Richtlinien** aus.
2. Wählen Sie **Richtlinie hochladen** aus.
3. Aktivieren Sie **Richtlinie überschreiben, sofern vorhanden**.
4. Navigieren Sie zur Richtliniendatei *TrustFrameworkExtensions.xml* (sofern vorhanden), und wählen Sie dann **Hochladen** aus. Vergewissern Sie sich, dass die Überprüfung erfolgreich ist.


## <a name="configure-an-adfs-relying-party-trust"></a>Konfigurieren einer AD FS-Vertrauensstellung der vertrauenden Seite

Um AD FS als Identitätsanbieter in Azure AD B2C verwenden zu können, müssen Sie eine AD FS-Vertrauensstellung der vertrauenden Seite mit den SAML-Metadaten von Azure AD B2C erstellen. Das folgende Beispiel zeigt eine URL der SAML-Metadaten eines technischen Azure AD B2C-Profils:

```
https://login.microsoftonline.com/te/your-tenant/your-policy/samlp/metadata?idptp=your-technical-profile
```

Ersetzen Sie die folgenden Werte:

- **your-tenant** durch Ihren Mandantennamen, z.B. „ihr-mandant.onmicrosoft.com“
- **your-policy** durch den Namen Ihrer Richtlinie. Verwenden Sie die Richtlinie, in der Sie das technische Profil des SAML-Anbieters konfigurieren, oder eine Richtlinie, die von dieser Richtlinie erbt.
- **your-technical-profile** durch den Namen des technische Profil des SAML-Identitätsanbieters
 
Öffnen Sie einen Browser, und browsen Sie zu dieser URL. Stellen Sie sicher, dass Sie die richtige URL eingeben und dass Sie Zugriff auf die XML-Metadatendatei haben.

Um eine neue Vertrauensstellung der vertrauenden Seite mithilfe des AD FS-Verwaltungs-Snap-Ins hinzuzufügen und die Einstellungen manuell zu konfigurieren, führen Sie das folgende Verfahren auf einem Verbundserver aus. Eine Mitgliedschaft in **Administratoren** oder Entsprechendem auf dem lokalen Computer ist die Mindestvoraussetzung, um dieses Verfahren abzuschließen. Beachten Sie die näheren Informationen zur Verwendung der geeigneten Konten und Gruppenmitgliedschaften unter [Local and Domain Default Groups](http://go.microsoft.com/fwlink/?LinkId=83477) (Lokale und Domänenstandardgruppen).

1. Wählen Sie im Server-Manager **Tools** und dann **AD FS-Verwaltung** aus.
2. Wählen Sie **Hinzufügen der Vertrauensstellung der vertrauenden Seite**.
3. Wählen Sie auf der Seite **Willkommen** die Option **Ansprüche unterstützend** aus, und klicken Sie dann auf **Start**.
4. Wählen Sie auf der Seite **Datenquelle auswählen** die Option **Import data about the relying party publish online or on a local network** (Daten über die vertrauende Seite für die Veröffentlichung online oder in einem lokalen Netzwerk importieren) aus, geben Sie Ihre Azure AD B2C-Metadaten-URL an, und klicken Sie dann auf **Weiter**.
5. Geben Sie auf der Seite **Anzeigename angeben** einen Namen in **Anzeigename** und unter **Hinweise** eine Beschreibung für diese Vertrauensstellung der vertrauenden Seite ein, und klicken Sie dann auf **Weiter**.
6. Wählen Sie auf der Seite **Zugriffssteuerungsrichtlinie auswählen** eine Richtlinie aus, und klicken Sie dann auf **Weiter**.
7. Überprüfen Sie die Einstellungen auf der Seite **Bereit zum Hinzufügen der Vertrauensstellung**, und klicken Sie dann auf **Weiter**, um die Daten Ihrer Vertrauensstellung der vertrauenden Seite zu speichern.
8. Klicken Sie auf der Seite **Fertig stellen** auf **Schließen**. Diese Aktion zeigt automatisch das Dialogfeld **Anspruchsregeln bearbeiten** an.
9. Wählen Sie **Regel hinzufügen** aus.  
10. Wählen Sie in **Anspruchsregelvorlage** die Option **Senden von LDAP-Attributen als Ansprüche** aus.
11. Geben Sie einen **Anspruchsregelnamen** an. Wählen Sie als **Attributspeicher** **Active Directory** aus. Fügen Sie die folgenden Ansprüche hinzu, und klicken Sie auf **Fertig stellen** und **OK**.

    ![Festlegen von Regeleigenschaften](./media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-3.png)

12.  Basierend auf Ihrem Zertifikattyp müssen Sie möglicherweise den Hashalgorithmus festlegen. Wählen Sie im Eigenschaftenfenster der Vertrauensstellung der vertrauenden Seite (B2C-Demo) die Registerkarte **Erweitert** aus, und ändern Sie **Sicherer Hashalgorithmus** in `SHA-1` oder `SHA-256`. Klicken Sie auf **OK**.  

### <a name="update-the-relying-party-metadata"></a>Aktualisieren der Metadaten der vertrauenden Seite

Zum Ändern des technischen SAML-Profils müssen Sie AD FS mit der aktualisierten Metadatenversion aktualisieren. Sie müssen die Metadaten nicht aktualisieren, wenn Sie die Anwendung der vertrauenden Seite erstellen. Wenn Sie aber eine Änderung vornehmen, müssen Sie die Metadaten in AD FS aktualisieren.

1. Wählen Sie im Server-Manager **Tools** und dann **AD FS-Verwaltung** aus.
2. Wählen Sie die Vertrauensstellung der vertrauenden Seite, die Sie erstellt haben, und **Update from Federation Metadata** (Von Verbundmetadaten aktualisieren) aus, und klicken Sie dann auf **Aktualisieren**. 

### <a name="test-the-policy-by-using-run-now"></a>Testen der Richtlinie mit „Jetzt ausführen“

1.  Öffnen Sie die **Azure AD B2C-Einstellungen**, und navigieren Sie zu **Framework für die Identitätsfunktion**.
2.  Öffnen Sie **B2C_1A_ProfileEdit** – die benutzerdefinierte Richtlinie der vertrauenden Seite, die Sie hochgeladen haben. Wählen Sie **Jetzt ausführen** aus. Sie sollten sich mit dem AD FS-Konto anmelden können.

## <a name="download-the-complete-policy-files"></a>Herunterladen der vollständigen Richtliniendateien

Optional: Sie können nach Abschluss der Schritte in [Erste Schritte mit benutzerdefinierten Richtlinien](active-directory-b2c-get-started-custom.md) Ihr Szenario mithilfe Ihrer eigenen Richtliniendateien erstellen. Beispieldateien finden Sie unter [Policy sample files for reference only](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-adfs2016-app) (Richtlinienbeispieldateien zur Referenz).
