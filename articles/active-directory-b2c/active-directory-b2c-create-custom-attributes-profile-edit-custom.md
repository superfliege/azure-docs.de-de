---
title: Hinzufügen eigener Attribute zu benutzerdefinierten Richtlinien in Azure Active Directory B2C | Microsoft-Dokumentation
description: Eine exemplarische Vorgehensweise zur Verwendung von Erweiterungseigenschaften und benutzerdefinierten Attributen sowie zu ihrer Einbindung in die Benutzeroberfläche.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/04/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: ecde4d8cd8ee454290b16b640ba05d310cf348fe
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37449175"
---
# <a name="azure-active-directory-b2c-creating-and-using-custom-attributes-in-a-custom-profile-edit-policy"></a>Azure Active Directory B2C: Erstellen und Verwenden von benutzerdefinierten Attributen in einer benutzerdefinierten Richtlinie für die Profilbearbeitung

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

In diesem Artikel erstellen Sie ein benutzerdefiniertes Attribut in Ihrem Azure AD B2C-Verzeichnis und verwenden dieses neue Attribut als benutzerdefinierten Anspruch in der User Journey für die Profilbearbeitung.

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie die Schritte im Artikel [Erste Schritte mit benutzerdefinierten Richtlinien](active-directory-b2c-get-started-custom.md) aus.

## <a name="use-custom-attributes-to-collect-information-about-your-customers-in-azure-active-directory-b2c-using-custom-policies"></a>Verwenden von benutzerdefinierten Attributen zum Erfassen von Informationen zu Ihren Kunden in Azure Active Directory B2C mit benutzerdefinierten Richtlinien
Das Azure Active Directory (Azure AD) B2C-Verzeichnis bietet einen integrierten Satz von Attributen: Vorname, Nachname, Ort, Postleitzahl, userPrincipalName usw.  Es kann häufiger vorkommen, dass Sie eigene Attribute erstellen müssen.  Beispiel: 
* Für eine Kundenanwendung muss ein Attribut beibehalten werden, z.B. „LoyaltyNumber“.
* Ein Identitätsanbieter verfügt über eine eindeutige Benutzer-ID, die gespeichert werden muss, z.B. „uniqueUserGUID“.
* Für eine benutzerdefinierte User Journey muss der Status eines Benutzers beibehalten werden, z.B. „migrationStatus“.

Mit Azure AD B2C haben Sie die Möglichkeit, den für die einzelnen Benutzerkonten gespeicherten Satz von Attributen zu erweitern. Außerdem können Sie diese Attribute mit der [Azure AD Graph-API](active-directory-b2c-devquickstarts-graph-dotnet.md)lesen und schreiben.

Mit Erweiterungseigenschaften wird das Schema der Benutzerobjekte im Verzeichnis erweitert.  Die Begriffe „Erweiterungseigenschaft“, „benutzerdefiniertes Attribut“ und „benutzerdefinierter Anspruch“ beziehen sich im Kontext dieses Artikels auf die gleiche Sache, und der Name variiert je nach Kontext (Anwendung, Objekt, Richtlinie).

Erweiterungseigenschaften können nur für ein Anwendungsobjekt registriert werden, obwohl sie Daten für einen Benutzer enthalten können. Die Eigenschaft ist an die Anwendung angefügt. Dem Anwendungsobjekt muss Schreibzugriff zum Registrieren einer Erweiterungseigenschaft gewährt werden. 100 Erweiterungseigenschaften (für ALLE Typen und ALLE Anwendungen) können in jedes einzelne Objekt geschrieben werden. Erweiterungseigenschaften werden dem Zielverzeichnistyp hinzugefügt und sind im Azure AD B2C-Verzeichnismandanten dann sofort zugänglich.
Wenn die Anwendung gelöscht wird, werden auch diese Erweiterungseigenschaften zusammen mit darin enthaltenen Daten für alle Benutzer entfernt. Falls eine Erweiterungseigenschaft von der Anwendung gelöscht wird, wird sie aus den Zielverzeichnisobjekten entfernt, und die Werte werden gelöscht.

Erweiterungseigenschaften sind nur im Kontext einer registrierten Anwendung im Mandanten vorhanden. Die Objekt-ID dieser Anwendung muss in das TechnicalProfile einbezogen werden, von dem sie verwendet wird.

>[!NOTE]
>Normalerweise enthält das Azure AD B2C-Verzeichnis eine Web-App mit dem Namen `b2c-extensions-app`.  Diese Anwendung wird hauptsächlich von den integrierten B2C-Richtlinien für die benutzerdefinierten Ansprüche verwendet, die mit dem Azure-Portal erstellt werden.  Die Nutzung dieser Anwendung zum Registrieren von Erweiterungen für benutzerdefinierte B2C-Richtlinien wird nur für fortgeschrittene Benutzer empfohlen.  Eine Anleitung hierzu finden Sie in diesem Artikel im Abschnitt „Nächste Schritte“.


## <a name="creating-a-new-application-to-store-the-extension-properties"></a>Erstellen einer neuen Anwendung zum Speichern der Erweiterungseigenschaften

1. Öffnen Sie eine Browsersitzung, navigieren Sie zum [Azure-Portal](https://portal.azure.com), und melden Sie sich mit den Administratoranmeldeinformationen des B2C-Verzeichnisses an, das Sie konfigurieren möchten.
2. Klicken Sie auf **Azure Active Directory** im linken Navigationsmenü. Unter Umständen müssen Sie die Option „Weitere Dienste“ wählen, um das Verzeichnis zu finden.
3. Wählen Sie **App-Registrierungen** aus, und klicken Sie auf **Neue Awendungsregistrierung**.
4. Geben Sie die folgenden empfohlenen Einträge an:
    * Geben Sie einen Namen für die Webanwendung an: **WebApp-GraphAPI-DirectoryExtensions**
    * Anwendungstyp: Web-App/API
    * Anmelde-URL: https://{tenantName}.onmicrosoft.com/WebApp-GraphAPI-DirectoryExtensions
5. Klicken Sie auf **Erstellen**.
6. Wählen Sie die neu erstellte Webanwendung aus.
7. Wählen Sie **Einstellungen** > **Erforderliche Berechtigungen** aus.
8. Wählen Sie die API **Windows Azure Active Directory** aus.
9. Setzen Sie ein Häkchen in den Anwendungsberechtigungen für **Verzeichnisdaten lesen und schreiben**, und wählen Sie dann **Speichern** aus.
10. Wählen Sie **Berechtigungen erteilen** aus, und bestätigen Sie den Vorgang mit **Ja**.
11. Kopieren Sie die folgenden Bezeichner in die Zwischenablage, und speichern Sie sie:
    * **Anwendungs-ID** Beispiel: `103ee0e6-f92d-4183-b576-8c3739027780`
    * **Objekt-ID** Beispiel: `80d8296a-da0a-49ee-b6ab-fd232aa45201`



## <a name="modifying-your-custom-policy-to-add-the-applicationobjectid"></a>Ändern der benutzerdefinierten Richtlinie zum Hinzufügen der ApplicationObjectId

Wenn Sie die Schritte unter [Erste Schritte mit benutzerdefinierten Richtlinien](active-directory-b2c-get-started-custom.md) ausgeführt haben, haben Sie die [Dateien](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/archive/master.zip) mit dem Namen *TrustFrameworkBase.xml*, *TrustFrameworkExtensions.xml*, *SignUpOrSignin.xml*, *ProfileEdit.xml* und *PasswordReset.xml* heruntergeladen und geändert. In den folgenden Schritten nehmen Sie weitere Änderungen an diesen Dateien vor.

1. Öffnen Sie die Datei *TrustFrameworkBase.xml*, und fügen Sie den Abschnitt `Metadata` wie im folgenden Beispiel gezeigt ein. Fügen Sie die zuvor für den Wert `ApplicationObjectId` notierte Objekt-ID und die für den Wert `ClientId` notierte Anwendungs-ID ein: 

    ```xml
    <ClaimsProviders>
        <ClaimsProvider>
          <DisplayName>Azure Active Directory</DisplayName>
            <TechnicalProfile Id="AAD-Common">
          <DisplayName>Azure Active Directory</DisplayName>
          <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureActiveDirectoryProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
              
          <!-- Provide objectId and appId before using extension properties. -->
          <Metadata>
            <Item Key="ApplicationObjectId">insert objectId here</Item>
            <Item Key="ClientId">insert appId here</Item>
          </Metadata>
          <!-- End of changes -->
              
          <CryptographicKeys>
            <Key Id="issuer_secret" StorageReferenceId="TokenSigningKeyContainer" />
          </CryptographicKeys>
          <IncludeInSso>false</IncludeInSso>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
        </TechnicalProfile>
        </ClaimsProvider>
    </ClaimsProviders>
    ```

>[!NOTE]
>Wenn das TechnicalProfile zum ersten Mal einen Schreibvorgang in die neu erstellte Erweiterungseigenschaft durchführt, tritt ggf. ein einmaliger Fehler auf. Die Erweiterungseigenschaft wird bei erstmaliger Verwendung erstellt.  

## <a name="using-the-new-extension-property--custom-attribute-in-a-user-journey"></a>Verwenden der neuen Erweiterungseigenschaft bzw. des benutzerdefinierten Attributs in einer User Journey

1. Öffnen Sie die Datei *ProfileEdit.xml*.
2. Fügen Sie den benutzerdefinierten Anspruch `loyaltyId` hinzu.  Indem der benutzerdefinierte Anspruch in das Element `<RelyingParty>` eingefügt wird, wird er in das Token für die Anwendung eingefügt.
    
    ```xml
    <RelyingParty>
      <DefaultUserJourney ReferenceId="ProfileEdit" />
      <TechnicalProfile Id="PolicyProfile">
        <DisplayName>PolicyProfile</DisplayName>
        <Protocol Name="OpenIdConnect" />
        <OutputClaims>
          <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
          <OutputClaim ClaimTypeReferenceId="city" />

          <!-- Provide the custom claim identifier -->
          <OutputClaim ClaimTypeReferenceId="extension_loyaltyId" />
          <!-- End of changes -->
        </OutputClaims>
        <SubjectNamingInfo ClaimType="sub" />
      </TechnicalProfile>
    </RelyingParty>
    ```

3. Öffnen Sie die Datei *TrustFrameworkExtensions.xml*, und fügen Sie das `<ClaimsSchema>`-Element und seine untergeordneten Elemente in das `BuildingBlocks`-Element ein:

    ```xml
    <BuildingBlocks>
      <ClaimsSchema> 
        <ClaimType Id="extension_loyaltyId"> 
          <DisplayName>Loyalty Identification Tag</DisplayName> 
          <DataType>string</DataType> 
          <UserHelpText>Your loyalty number from your membership card</UserHelpText> 
          <UserInputType>TextBox</UserInputType> 
        </ClaimType> 
      </ClaimsSchema>
    </BuildingBlocks>
    ```

4. Fügen Sie die gleiche `ClaimType`-Definition in *TrustFrameworkBase.xml* ein. Das Hinzufügen einer `ClaimType`-Definition zur Basisdatei und zur Erweiterungsdatei ist normalerweise nicht erforderlich. Da in den nächsten Schritten aber `extension_loyaltyId` zu „TechnicalProfiles“ in der Basisdatei hinzugefügt wird, lehnt die Richtlinienvalidierung den Upload der Basisdatei ohne dieses Element ab. Es kann nützlich sein, die Ausführung der User Journey mit dem Namen „ProfileEdit“ in der Datei *TrustFrameworkBase.xml* nachzuverfolgen.  Suchen Sie im Editor nach der User Journey mit dem gleichen Namen, und verfolgen Sie, dass im Orchestrierungsschritt 5 TechnicalProfileReferenceID="SelfAsserted-ProfileUpdate" aufgerufen wird.  Untersuchen Sie dieses TechnicalProfile, um sich mit dem Ablauf vertraut zu machen.

5. Öffnen Sie die Datei *TrustFrameworkBase.xml*, und fügen Sie `loyaltyId` als Eingabe- und Ausgabeanspruch in das TechnicalProfile „SelfAsserted-ProfileUpdate“ hinzu:

    ```xml
    <TechnicalProfile Id="SelfAsserted-ProfileUpdate">
      <DisplayName>User ID signup</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
      <Metadata>
        <Item Key="ContentDefinitionReferenceId">api.selfasserted.profileupdate</Item>
      </Metadata>
      <IncludeInSso>false</IncludeInSso>
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="alternativeSecurityId" />
        <InputClaim ClaimTypeReferenceId="userPrincipalName" />
        <InputClaim ClaimTypeReferenceId="givenName" />
            <InputClaim ClaimTypeReferenceId="surname" />

        <!-- Add the loyalty identifier -->
        <InputClaim ClaimTypeReferenceId="extension_loyaltyId"/>
        <!-- End of changes -->
      </InputClaims>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="executed-SelfAsserted-Input" DefaultValue="true" />
        <OutputClaim ClaimTypeReferenceId="givenName" />
        <OutputClaim ClaimTypeReferenceId="surname" />
        
        <!-- Add the loyalty identifier -->
        <OutputClaim ClaimTypeReferenceId="extension_loyaltyId"/>
        <!-- End of changes -->

      </OutputClaims>
      <ValidationTechnicalProfiles>
        <ValidationTechnicalProfile ReferenceId="AAD-UserWriteProfileUsingObjectId" />
      </ValidationTechnicalProfiles>
    </TechnicalProfile>
    ```

6. Fügen Sie in der Datei *TrustFrameworkBase.xml* den Anspruch `loyaltyId` dem TechnicalProfile „AAD-UserWriteProfileUsingObjectId“ hinzu, um den Wert des Anspruchs in der Erweiterungseigenschaft für den aktuellen Benutzer im Verzeichnis beizubehalten:

    ```xml
    <TechnicalProfile Id="AAD-UserWriteProfileUsingObjectId">
      <Metadata>
        <Item Key="Operation">Write</Item>
        <Item Key="RaiseErrorIfClaimsPrincipalAlreadyExists">false</Item>
        <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">true</Item>
      </Metadata>
      <IncludeInSso>false</IncludeInSso>
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="objectId" Required="true" />
      </InputClaims>
      <PersistedClaims>
        <PersistedClaim ClaimTypeReferenceId="objectId" />
        <PersistedClaim ClaimTypeReferenceId="givenName" />
        <PersistedClaim ClaimTypeReferenceId="surname" />

        <!-- Add the loyalty identifier -->
        <PersistedClaim ClaimTypeReferenceId="extension_loyaltyId" />
        <!-- End of changes -->

      </PersistedClaims>
      <IncludeTechnicalProfile ReferenceId="AAD-Common" />
    </TechnicalProfile>
    ```

7. Fügen Sie in der Datei *TrustFrameworkBase.xml* den Anspruch `loyaltyId` dem TechnicalProfile „AAD-UserReadUsingObjectId“ hinzu, damit der Wert des Erweiterungsattributs bei jeder Anmeldung eines Benutzers gelesen wird. Bisher wurden die TechnicalProfiles nur im Ablauf der lokalen Konten geändert.  Wenn das neue Attribut im Ablauf eines Social Media- oder Verbundkontos gewünscht ist, muss eine andere Gruppe von TechnicalProfiles geändert werden. Weitere Informationen finden Sie unter „Nächste Schritte“.

    ```xml
    <TechnicalProfile Id="AAD-UserReadUsingObjectId">
      <Metadata>
        <Item Key="Operation">Read</Item>
        <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">true</Item>
      </Metadata>
      <IncludeInSso>false</IncludeInSso>
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="objectId" Required="true" />
      </InputClaims>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="signInNames.emailAddress" />
        <OutputClaim ClaimTypeReferenceId="displayName" />
        <OutputClaim ClaimTypeReferenceId="otherMails" />
        <OutputClaim ClaimTypeReferenceId="givenName" />
        <OutputClaim ClaimTypeReferenceId="surname" />

        <!-- Add the loyalty identifier -->
        <OutputClaim ClaimTypeReferenceId="extension_loyaltyId" />
        <!-- End of changes -->

      </OutputClaims>
      <IncludeTechnicalProfile ReferenceId="AAD-Common" />
    </TechnicalProfile>
    ```

## <a name="test-the-custom-policy"></a>Testen der benutzerdefinierten Richtlinie

1. Öffnen Sie das Blatt **Azure AD B2C**, und navigieren Sie zu **Identity Experience Framework > Benutzerdefinierte Richtlinien**.
1. Wählen Sie die benutzerdefinierte Richtlinie aus, die hochgeladen werden soll, und klicken Sie auf die Schaltfläche **Jetzt ausführen**.
1. Sie sollten sich mit einer E-Mail-Adresse registrieren können.

Das id-Token, das an Ihre Anwendung zurückgesendet wird, enthält die neue Erweiterungseigenschaft als benutzerdefinierten Anspruch mit Voranstellung von „extension_loyaltyId“. Weitere Informationen sind im Beispiel enthalten.

```json
{
  "exp": 1493585187,
  "nbf": 1493581587,
  "ver": "1.0",
  "iss": "https://login.microsoftonline.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
  "sub": "a58e7c6c-7535-4074-93da-b0023fbaf3ac",
  "aud": "4e87c1dd-e5f5-4ac8-8368-bc6a98751b8b",
  "acr": "b2c_1a_trustframeworkprofileedit",
  "nonce": "defaultNonce",
  "iat": 1493581587,
  "auth_time": 1493581587,
  "extension_loyaltyId": "abc",
  "city": "Redmond"
}
```

## <a name="next-steps"></a>Nächste Schritte

### <a name="add-the-new-claim-to-the-flows-for-social-account-logins-by-changing-the-technicalprofiles-listed-below-these-two-technicalprofiles-are-used-by-socialfederated-account-logins-to-write-and-read-the-user-data-using-the-alternativesecurityid-as-the-locator-of-the-user-object"></a>Fügen Sie den neuen Anspruch dem Ablauf für die Anmeldung an Social Media-Konten hinzu, indem Sie die unten aufgeführten TechnicalProfiles ändern. Diese beiden TechnicalProfiles werden von Anmeldungen für Social Media-/Verbundkonten genutzt, um die Benutzerdaten zu schreiben und zu lesen, indem alternativeSecurityId als Locator des Benutzerobjekts verwendet wird.
```xml
  <TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">

  <TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId">
```

Verwenden der gleichen Erweiterungsattribute zwischen integrierten und benutzerdefinierten Richtlinien.
Beim Hinzufügen von Erweiterungsattributen (benutzerdefinierten Attributen) über die Portaloberfläche werden diese Attribute per **b2c-extensions-app registriert, die unter jedem B2C-Mandanten vorhanden ist.  Gehen Sie wie folgt vor, um diese Erweiterungsattribute in Ihrer benutzerdefinierten Richtlinie zu verwenden:
1. Navigieren Sie in Ihrem B2C-Mandanten in „portal.azure.com“ zu **Azure Active Directory**, und wählen Sie die Option **App-Registrierungen**.
2. Suchen Sie nach **b2c-extensions-app**, und wählen Sie sie aus.
3. Notieren Sie sich unter „Essentials“ die **Anwendungs-ID** und die **Objekt-ID**.
4. Fügen Sie diese IDs wie folgt in die Metadaten Ihres technischen AAD-Common-Profils ein:

```xml
    <ClaimsProviders>
        <ClaimsProvider>
              <DisplayName>Azure Active Directory</DisplayName>
            <TechnicalProfile Id="AAD-Common">
              <DisplayName>Azure Active Directory</DisplayName>
              <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureActiveDirectoryProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
              <!-- Provide objectId and appId before using extension properties. -->
              <Metadata>
                <Item Key="ApplicationObjectId">insert objectId here</Item> <!-- This is the "Object ID" from the "b2c-extensions-app"-->
                <Item Key="ClientId">insert appId here</Item> <!--This is the "Application ID" from the "b2c-extensions-app"-->
              </Metadata>
```

Erstellen Sie diese Attribute mithilfe der Portalbenutzeroberfläche, *bevor* Sie sie in Ihren benutzerdefinierten Richtlinien verwenden, um die Einheitlichkeit mit der Portalumgebung zu wahren.  Wenn Sie im Portal das Attribut „ActivationStatus“ erstellen, müssen Sie wie folgt darauf verweisen:

```
extension_ActivationStatus in the custom policy
extension_<app-guid>_ActivationStatus via the Graph API.
```


## <a name="reference"></a>Verweis

* Ein **technisches Profil (TP)** ist ein Elementtyp, den Sie sich als *Funktion* mit dem folgenden Zweck vorstellen können: Definieren eines Endpunktnamens, der zugehörigen Metadaten, des Protokolls und der Details zum Austausch von Ansprüchen, die vom Identity Experience Framework durchgeführt werden sollen.  Wenn diese *Funktion* in einem Orchestrierungsschritt oder über ein anderes TechnicalProfile aufgerufen wird, werden die InputClaims und OutputClaims vom Aufrufer als Parameter bereitgestellt.


* Informationen zur umfassenden Behandlung von Erweiterungseigenschaften finden Sie im Artikel [Verzeichnisschemaerweiterungen | Graph-API-Konzepte](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions)

>[!NOTE]
>Erweiterungsattribute in Graph-API werden mithilfe der Konvention `extension_ApplicationObjectID_attributename` benannt. In benutzerdefinierten Richtlinien wird auf Erweiterungsattribute als „extension_attributename“ verwiesen, sodass die ApplicationObjectId im XML-Code weggelassen wird.
