---
title: "Azure Active Directory B2C: Integrieren von REST-API-Anspruchsaustausch-Vorgängen in Ihre Azure AD B2C User Journey als Validierung der Benutzereingabe"
description: "Anhand eines Beispiels wird beschrieben, wie Sie REST-API-Anspruchsaustausch-Vorgänge in Ihre Azure AD B2C User Journey als Validierung der Benutzereingabe integrieren."
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
ms.date: 09/30/2017
ms.author: yoelh
ms.openlocfilehash: da7b8326a89fc6eb0bc3e7365c522d4a3adc809b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="azure-active-directory-b2c-integrate-rest-api-claims-exchanges-in-your-azure-ad-b2c-user-journey-as-validation-on-user-input"></a>Azure Active Directory B2C: Integrieren von REST-API-Anspruchsaustausch-Vorgängen in Ihre Azure AD B2C User Journey als Validierung der Benutzereingabe
Das Framework für die Identitätsfunktion (Identity Experience Framework, IEF), das Azure Active Directory B2C (Azure AD B2C) zugrunde liegt, ermöglicht Ihnen die Integration in eine User Journey per RESTful-API. In dieser exemplarischen Vorgehensweise wird veranschaulicht, wie B2C mit .NET Framework-RESTful-Diensten (Web-API) interagiert.

## <a name="introduction"></a>Einführung
Mit Azure AD B2C können Sie einer User Journey Ihre eigene Geschäftslogik hinzufügen, indem Sie Ihren eigenen RESTful-Dienst aufrufen. Das Framework für die Identitätsfunktion sendet Daten an den RESTful-Dienst in der Sammlung für **Eingabeansprüche** und erhält Daten über RESTful in der Sammlung für **Ausgabeansprüche** zurück. Mit der RESTful-Dienstintegration haben Sie folgende Möglichkeiten:

* Überprüfen Sie Benutzereingabedaten, um zu verhindern, dass falsch formatierte Daten in Azure AD beibehalten werden. Wenn der Wert des Benutzers nicht gültig ist, gibt Ihr RESTful-Dienst eine Fehlermeldung zurück, in der der Benutzer angewiesen wird, einen Eintrag anzugeben. Beispielsweise können Sie überprüfen, ob die vom Benutzer angegebene E-Mail-Adresse in Ihrer Kundendatenbank vorhanden ist.
* Setzen Sie Eingabeansprüche außer Kraft. Wenn der Benutzer beispielsweise den Vornamen in Klein- oder Großbuchstaben angibt, können Sie den Namen so formatieren, dass nur der erste Buchstabe ein Großbuchstabe ist.
* Erweitern Sie die Benutzerdaten, indem Sie sie stärker in die Branchenanwendung des Unternehmens integrieren. Es kann sein, dass Ihr RESTful-Dienst die E-Mail-Adresse des Benutzers erhält, die Kundendatenbank abfragt und die Treuenummer des Benutzers an B2C zurückgibt. Die Rückgabeansprüche können unter dem AAD-Benutzerkonto gespeichert, in den nächsten **Orchestrierungsschritten** ausgewertet oder in das Zugriffstoken eingebunden werden.
* Führen Sie benutzerdefinierte Geschäftslogik aus: Senden von Pushbenachrichtigungen, Aktualisieren von Datenbanken des Unternehmens, Ausführen des Benutzermigrationsprozesses, Berechtigungsverwaltung, Überwachung usw.

Die Integration in die RESTful-Dienste kann als Anspruchsaustausch oder als technisches Validierungsprofil entworfen werden:

* **Technisches Validierungsprofil**: Der Aufruf des RESTful-Diensts wird im ValidationTechnicalProfile-Element eines vorhandenen TechnicalProfile-Elements durchgeführt. Mit dem technischen Validierungsprofil werden die vom Benutzer bereitgestellten Daten überprüft, bevor die User Journey weitergeht. Beim technischen Validierungsprofil haben Sie folgende Möglichkeiten:
  * Senden von Eingabeansprüchen
  * Überprüfen der Eingabeansprüche und Auslösen von benutzerdefinierten Fehlermeldungen
  * Zurücksenden von Ausgabeansprüchen

* **Anspruchsaustausch**: Ähnelt der vorherigen Vorgehensweise, aber der Vorgang wird in einem Orchestrierungsschritt ausgeführt. Diese Definition ist auf folgende Vorgänge beschränkt:
   * Senden von Eingabeansprüchen
   * Zurücksenden von Ausgabeansprüchen

## <a name="restful-walkthrough"></a>Exemplarische Vorgehensweise für RESTful
In dieser exemplarischen Vorgehensweise entwickeln Sie eine .NET Framework-Web-API, mit der die Benutzereingabe überprüft und die Treuenummer des Benutzers angegeben wird. Beispielsweise kann Ihre Anwendung basierend auf der Treuenummer Zugriff auf „Platin-Vorteile“ gewähren.

Übersicht:
*   Entwickeln des RESTful-Diensts (.NET Framework-Web-API)
*   Verwenden des RESTful-Diensts in der User Journey
*   Senden von Eingabeansprüchen und Lesen in Ihrem Code
*   Überprüfen des Vornamens eines Benutzers
*   Zurücksenden der Treuenummer 
*   Hinzufügen der Treuenummer an das JSON Web Token (JWT)

## <a name="prerequisites"></a>Voraussetzungen
Führen Sie die Schritte im Artikel [Erste Schritte mit benutzerdefinierten Richtlinien](active-directory-b2c-get-started-custom.md) aus.

## <a name="step-1-create-an-aspnet-web-api"></a>Schritt 1: Erstellen einer ASP.NET-Web-API
1.  Erstellen Sie in Visual Studio ein Projekt durch Auswählen von **Datei > Neu > Projekt**.
2.  Wählen Sie im Dialogfeld **Neues Projekt** die Optionen **Visual C# > Web > ASP.NET-Webanwendung (.NET Framework)** aus.
3.  Nennen Sie die Anwendung beispielsweise „Contoso.AADB2C.API“, und wählen Sie dann **OK**.

    ![Erstellen eines neuen Visual Studio-Projekts](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-create-project.png)

4.  Wählen Sie **Web-API** oder **Azure-API-App** als Vorlage aus.
5.  Stellen Sie sicher, dass für die Authentifizierung **Keine Authentifizierung** festgelegt ist.

    ![Auswählen der Web-API-Vorlage](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-select-web-api.png)

6.  Klicken Sie auf **OK**, um das Projekt zu erstellen.

## <a name="step-2-prepare-the-rest-api-endpoint"></a>Schritt 2: Vorbereiten des REST-API-Endpunkts

### <a name="step-21-add-data-models"></a>Schritt 2.1: Hinzufügen von Datenmodellen
Die Modelle stehen für die Eingabeanspruchs- und Ausgabeanspruchsdaten in Ihrem RESTful-Dienst. Ihr Code liest die Eingabedaten, indem das Eingabeanspruchsmodell aus einer JSON-Zeichenfolge in ein C#-Objekt (Ihr Modell) deserialisiert wird. Mit der ASP.NET-Web-API wird das Ausgabeanspruchsmodell automatisch zurück in JSON deserialisiert, und anschließend werden die serialisierten Daten in den Hauptteil der HTTP-Antwortnachricht geschrieben. Wir erstellen zuerst ein Modell, das für Eingabeansprüche steht.

1.  Wenn der Projektmappen-Explorer nicht bereits sichtbar ist, können Sie auf das Menü **Ansicht** klicken und **Projektmappen-Explorer** wählen. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Ordner „Modelle“. Wählen Sie im Kontextmenü die Option **Hinzufügen** und dann **Klasse**.

    ![Modell hinzufügen](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-add-model.png)

2.  Geben Sie der Klasse den Namen `InputClaimsModel`, und fügen Sie der `InputClaimsModel`-Klasse die folgenden Eigenschaften hinzu:

    ```C#
    namespace Contoso.AADB2C.API.Models
    {
        public class InputClaimsModel
        {
            public string email { get; set; }
            public string firstName { get; set; }
            public string lastName { get; set; }
        }
    }
    ```

3.  Erstellen Sie das neue Modell `OutputClaimsModel`, und fügen Sie der `OutputClaimsModel`-Klasse die folgenden Eigenschaften hinzu:

    ```C#
    namespace Contoso.AADB2C.API.Models
    {
        public class OutputClaimsModel
        {
            public string loyaltyNumber { get; set; }
        }
    }
    ```

4.  Erstellen Sie ein weiteres Modell: `B2CResponseContent`. Dieses Modell wird zum Auslösen von Fehlermeldungen für die Eingabeüberprüfung verwendet. Fügen Sie der `B2CResponseContent`-Klasse die folgenden Eigenschaften hinzu, geben Sie die fehlenden Verweise an, und speichern Sie die Datei.

    ```C#
    namespace Contoso.AADB2C.API.Models
    {
        public class B2CResponseContent
        {
            public string version { get; set; }
            public int status { get; set; }
            public string userMessage { get; set; }

            public B2CResponseContent(string message, HttpStatusCode status)
            {
                this.userMessage = message;
                this.status = (int)status;
                this.version = Assembly.GetExecutingAssembly().GetName().Version.ToString();
            }    
        }
    }
    ```

### <a name="step-22-add-a-controller"></a>Schritt 2.2: Hinzufügen eines Controllers
In der Web-API ist ein _Controller_ ein Objekt zum Verarbeiten von HTTP-Anforderungen. Wir fügen einen Controller hinzu, mit dem Ausgabeansprüche zurückgegeben werden oder – falls der Vorname nicht gültig ist – eine Fehlermeldung aufgrund eines HTTP-Konflikts ausgelöst wird.

1.  Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner „Controller“. Wählen Sie **Hinzufügen** und dann **Controller**.

    ![Hinzufügen eines neuen Controllers](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-add-controller-1.png)

2.  Wählen Sie im Dialogfeld **Gerüst hinzufügen** die Option **Web API Controller - Empty** (Web-API-Controller – Leer). Klicken Sie auf **Hinzufügen**.

    !![Auswählen von „Web API 2 Controller - Empty“ (Web-API 2-Controller – Leer)](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-add-controller-2.png)

3.  Geben Sie dem Controller im Dialogfeld **Controller hinzufügen**den Namen `IdentityController`, und klicken Sie dann auf **Hinzufügen**.

    ![Eingeben des Controllernamens](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-add-controller-3.png)

    Im Rahmen des Gerüstbauvorgangs wird im Ordner „Controller“ die Datei „IdentityController.cs“ erstellt.

4.  Doppelklicken Sie auf die Datei, um sie zu öffnen, falls sie noch nicht geöffnet ist. Ersetzen Sie den Code in dieser Datei durch die folgenden Codezeilen:

    ```C#
    using Contoso.AADB2C.API.Models;
    using Newtonsoft.Json;
    using System;
    using System.NET;
    using System.Web.Http;

    namespace Contoso.AADB2C.API.Controllers
    {
        public class IdentityController: ApiController
        {
            [HttpPost]
            public IHttpActionResult SignUp()
            {
                // If not data came in, then return
                if (this.Request.Content == null) throw new Exception();

                // Read the input claims from the request body
                string input = Request.Content.ReadAsStringAsync().Result;

                // Check input content value
                if (string.IsNullOrEmpty(input))
                {
                    return Content(HttpStatusCode.Conflict, new B2CResponseContent("Request content is empty", HttpStatusCode.Conflict));
                }

                // Convert the input string into InputClaimsModel object
                InputClaimsModel inputClaims = JsonConvert.DeserializeObject(input, typeof(InputClaimsModel)) as InputClaimsModel;

                if (inputClaims == null)
                {
                    return Content(HttpStatusCode.Conflict, new B2CResponseContent("Can not deserialize input claims", HttpStatusCode.Conflict));
                }

                // Run input validation
                if (inputClaims.firstName.ToLower() == "test")
                {
                    return Content(HttpStatusCode.Conflict, new B2CResponseContent("Test name is not valid, please provide a valid name", HttpStatusCode.Conflict));
                }

                // Create output claims object and set the loyalty number with random value
                OutputClaimsModel outputClaims = new OutputClaimsModel();
                outputClaims.loyaltyNumber = new Random().Next(100, 1000).ToString();

                // Return the output claim(s)
                return Ok(outputClaims);
            }
        }
    }
    ```

## <a name="step-3-publish-to-azure"></a>Schritt 3: Veröffentlichen in Azure
1.  Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **Contoso.AADB2C.API**, und wählen Sie **Veröffentlichen**.

    ![Veröffentlichen in Microsoft Azure App Service](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-publish-to-azure-1.png)

2.  Vergewissern Sie sich, dass **Microsoft Azure App Service** ausgewählt ist, und wählen Sie **Veröffentlichen** aus.

    ![Erstellen eines neuen Microsoft Azure App Service](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-publish-to-azure-2.png)

    Daraufhin wird das Dialogfeld **App Service erstellen** geöffnet, in dem Sie alle erforderlichen Azure-Ressourcen erstellen können, um die ASP.NET-Web-App in Azure auszuführen.

> [!NOTE]
>Weitere Informationen zum Veröffentlichen finden Sie unter [Erstellen von ASP.NET-Web-Apps in Azure](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-web-get-started-dotnet#publish-to-azure).

3.  Geben Sie unter **Web-App-Name** einen eindeutigen Web-App-Namen ein (gültige Zeichen sind `a-z`, `0-9` und `-`). Die URL der Web-App lautet `http://<app_name>.azurewebsites.NET`, wobei `<app_name>` der Name der Web-App ist. Sie können den automatisch generierten Namen übernehmen. Dieser ist eindeutig.

    Wählen Sie **Erstellen** aus, um mit der Erstellung der Azure-Ressourcen zu beginnen.

    ![Bereitstellen der App Service-Eigenschaften](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-publish-to-azure-3.png)

4.  Nach Abschluss des Assistenten wird die ASP.NET Web-App in Azure veröffentlicht und anschließend im Standardbrowser gestartet. Kopieren Sie die URL.

## <a name="step-4-add-the-new-claim-loyaltynumber-to-the-schema-of-your-trustframeworkextensionsxml-file"></a>Schritt 4: Hinzufügen des neuen `loyaltyNumber`-Anspruchs zum Schema der Datei „TrustFrameworkExtensions.xml“
Der `loyaltyNumber`-Anspruch ist in unserem Schema noch nicht definiert. Fügen Sie daher eine Definition in das Element `<BuildingBlocks>` ein. Dieses Element befindet sich am Anfang der Datei „TrustFrameworkExtensions.xml“.

```xml
<BuildingBlocks>
    <ClaimsSchema>
        <ClaimType Id="loyaltyNumber">
            <DisplayName>loyaltyNumber</DisplayName>
            <DataType>string</DataType>
            <UserHelpText>Customer loyalty number</UserHelpText>
        </ClaimType>
    </ClaimsSchema>
</BuildingBlocks>
```

## <a name="step-5-adding-claims-provider"></a>Schritt 5: Hinzufügen des Anspruchsanbieters 
Jeder Anspruchsanbieter muss über mindestens ein technisches Profil verfügen, mit dem die Endpunkte und die Protokolle bestimmt werden, die für die Kommunikation mit diesem Anspruchsanbieter erforderlich sind. Ein Anspruchsanbieter kann aus verschiedenen Gründen über mehrere technische Profile verfügen. Beispielsweise können mehrere technische Profile definiert werden, weil der berücksichtigte Anspruchsanbieter mehrere Protokolle bzw. verschiedene Endpunkte mit unterschiedlichen Funktionen unterstützt oder unterschiedliche Ansprüche mit verschiedenen Vertrauensgraden freigibt. Es kann akzeptabel sein, in einer User Journey sensible Ansprüche freizugeben, während dies für eine andere nicht der Fall ist. 

Der folgende XML-Codeausschnitt enthält den Knoten `ClaimsProvider` mit zwei `TechnicalProfile`-Elementen:
* Mit `<TechnicalProfile Id="REST-API-SignUp">` wird Ihr RESTful-Dienst definiert. Das `Proprietary`-Element wird als Protokoll für einen RESTful-basierten Anbieter beschrieben. Das `InputClaims`-Element definiert die Ansprüche, die von B2C an den REST-Dienst gesendet werden. In diesem Beispiel werden der Inhalt des Anspruchs `givenName` als `firstName` und der Inhalt des Anspruchs `surename` als `lastName` an den REST-Dienst gesendet, während `email` unverändert gesendet wird. Mit dem `OutputClaims`-Element werden die Ansprüche definiert, die vom RESTful-Dienst abgerufen werden, um zurück an B2C gesendet zu werden.

* Mit `<TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">` wird das technische Validierungsprofil dem vorhandenen technischen Profil (in der Basisrichtlinie definiert) hinzugefügt. Während des Anmeldeablaufs ruft das technische Validierungsprofil das obige technische Profil auf. Wenn der RESTful-Dienst den HTTP-Fehler 409 (Konflikt) zurückgibt, wird dem Benutzer die Fehlermeldung angezeigt. 

Suchen Sie nach dem Knoten `<ClaimsProviders>`, und fügen Sie unter dem Knoten `<ClaimsProviders>` den folgenden XML-Codeausschnitt hinzu:

```xml
<ClaimsProvider>
    <DisplayName>REST APIs</DisplayName>
    <TechnicalProfiles>
    
    <!-- Custom Restful service -->
    <TechnicalProfile Id="REST-API-SignUp">
        <DisplayName>Validate user's input data and return loyaltyNumber claim</DisplayName>
        <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
        <Metadata>
        <Item Key="ServiceUrl">https://yourdomain.azurewebsites.NET/api/identity/signup</Item>
        <Item Key="AuthenticationType">None</Item>
        <Item Key="SendClaimsIn">Body</Item>
        </Metadata>
        <InputClaims>
        <InputClaim ClaimTypeReferenceId="email" />
        <InputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="firstName" />
        <InputClaim ClaimTypeReferenceId="surname" PartnerClaimType="lastName" />
        </InputClaims>
        <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="loyaltyNumber" PartnerClaimType="loyaltyNumber" />
        </OutputClaims>
        <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
    </TechnicalProfile>

<!-- Change LocalAccountSignUpWithLogonEmail technical profile to support your validation technical profile -->
    <TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">
        <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="loyaltyNumber" PartnerClaimType="loyaltyNumber" />
        </OutputClaims>
        <ValidationTechnicalProfiles>
        <ValidationTechnicalProfile ReferenceId="REST-API-SignUp" />
        </ValidationTechnicalProfiles>
    </TechnicalProfile>

    </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="step-6-add-the-loyaltynumber-claim-to-your-relying-party-policy-file-so-the-claim-is-sent-to-your-application"></a>Schritt 6: Hinzufügen des `loyaltyNumber`-Anspruchs zur Datei mit den Richtlinien der vertrauenden Seite, damit der Anspruch an Ihre Anwendung gesendet wird
Bearbeiten Sie die Datei „SignUpOrSignIn.xml“ der vertrauenden Seite, und ändern Sie das `<TechnicalProfile Id="PolicyProfile">`-Element, um Folgendes hinzuzufügen: `<OutputClaim ClaimTypeReferenceId="loyaltyNumber" />`.

Nachdem Sie den neuen Anspruch hinzugefügt haben, sieht das `RelyingParty`-Element wie folgt aus:

```xml
<RelyingParty>
    <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
    <TechnicalProfile Id="PolicyProfile">
        <DisplayName>PolicyProfile</DisplayName>
        <Protocol Name="OpenIdConnect" />
        <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="displayName" />
        <OutputClaim ClaimTypeReferenceId="givenName" />
        <OutputClaim ClaimTypeReferenceId="surname" />
        <OutputClaim ClaimTypeReferenceId="email" />
        <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
        <OutputClaim ClaimTypeReferenceId="identityProvider" />
        <OutputClaim ClaimTypeReferenceId="loyaltyNumber" DefaultValue="" />
        </OutputClaims>
        <SubjectNamingInfo ClaimType="sub" />
    </TechnicalProfile>
    </RelyingParty>
</TrustFrameworkPolicy>
```

## <a name="step-7-upload-the-policy-to-your-tenant"></a>Schritt 7: Hochladen der Richtlinie in Ihren Mandanten
1.  Wechseln Sie im [Azure-Portal](https://portal.azure.com) in den [Kontext Ihres Azure AD B2C-Mandanten](active-directory-b2c-navigate-to-b2c-context.md), und öffnen Sie **Azure AD B2C**.
2.  Wählen Sie **Framework für die Identitätsfunktion** aus.
3.  Öffnen Sie **Alle Richtlinien**. 
4.  Wählen Sie **Richtlinie hochladen** aus.
5.  Aktivieren Sie das Kontrollkästchen **Richtlinie überschreiben, sofern vorhanden**.
6.  **Laden** Sie „TrustFrameworkExtensions.xml“ hoch, und stellen Sie sicher, dass kein Fehler bei der Validierung auftritt.
7.  Wiederholen Sie den letzten Schritt, und laden Sie die Datei „SignUpOrSignIn.xml“ hoch.

## <a name="step-8-test-the-custom-policy-by-using-run-now"></a>Schritt 8: Testen der benutzerdefinierten Richtlinie mit „Jetzt ausführen“
1.  Öffnen Sie die **Azure AD B2C-Einstellungen**, und navigieren Sie zu **Framework für die Identitätsfunktion**.

> [!NOTE]
>
>**Jetzt ausführen** macht erforderlich, dass mindestens eine Anwendung im Mandanten vorab registriert wird. Informationen zum Registrieren von Anwendungen finden Sie in den Artikeln [Erste Schritte](active-directory-b2c-get-started.md) bzw. [Anwendungsregistrierung](active-directory-b2c-app-registration.md) zu Azure AD B2C.


2.  Öffnen Sie **B2C_1A_signup_signin** – die benutzerdefinierte Richtlinie der vertrauenden Seite (Relying Party, RP), die Sie hochgeladen haben. Wählen Sie **Jetzt ausführen** aus.

    ![](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-run.png)

3.  Versuchen Sie, „Test“ in das Feld **Vorname** einzugeben. B2C zeigt die Fehlermeldung am oberen Rand der Seite an.

    ![Testen Ihrer Richtlinie](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-test.png)

4.  Versuchen Sie, einen Namen (nicht „Test“) in das Feld **Vorname** einzugeben. B2C meldet den Benutzer an und sendet dann die Treuenummer (loyaltyNumber) an Ihre Anwendung. Die Treuenummer ist in diesem JWT-Code enthalten.

```
{
  "typ": "JWT",
  "alg": "RS256",
  "kid": "X5eXk4xyojNFum1kl2Ytv8dlNP4-c57dO6QGTVBwaNk"
}.{
  "exp": 1507125903,
  "nbf": 1507122303,
  "ver": "1.0",
  "iss": "https://login.microsoftonline.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
  "aud": "e1d2612f-c2bc-4599-8e7b-d874eaca1ee1",
  "acr": "b2c_1a_signup_signin",
  "nonce": "defaultNonce",
  "iat": 1507122303,
  "auth_time": 1507122303,
  "loyaltyNumber": "290",
  "given_name": "Emily",
  "emails": ["B2cdemo@outlook.com"]
}
```

## <a name="optional-download-the-complete-policy-files-and-code"></a>[Optional] Herunterladen der vollständigen Richtliniendateien und des Codes
* Sie sollten nach Abschluss der exemplarischen Vorgehensweise „Erste Schritte mit benutzerdefinierten Richtlinien“ Ihr Szenario mithilfe Ihrer eigenen Dateien mit benutzerdefinierten Richtlinien erstellen, anstatt diese Beispieldateien zu verwenden.  [Beispielrichtliniendateien zur Referenz](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-rest-api-netfw)
* Sie können den vollständigen Code hier herunterladen: [Visual Studio-Beispiellösung zur Referenz](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-rest-api-netfw/).
    
## <a name="next-steps"></a>Nächste Schritte
1.  [Secure your RESTful API with basic authentication (username and password)](active-directory-b2c-custom-rest-api-netfw-secure-basic.md) (Schützen Ihrer RESTful-API per Standardauthentifizierung (Benutzername und Kennwort))
2.  [Secure your RESTful API with client certificate](active-directory-b2c-custom-rest-api-netfw-secure-cert.md) (Schützen Ihrer RESTful-API mit einem Clientzertifikat)