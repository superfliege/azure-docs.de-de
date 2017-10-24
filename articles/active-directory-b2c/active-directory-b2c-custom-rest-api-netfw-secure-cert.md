---
title: 'Azure Active Directory B2C: Sichern Ihrer RESTful-Dienste mit Clientzertifikaten'
description: "Beispiel der Sicherung Ihres benutzerdefinierten REST-API-Ansprücheaustauschs in Ihrem Azure AD B2C mit Clientzertifikaten"
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
ms.date: 09/25/2017
ms.author: yoelh
ms.openlocfilehash: a5cfe0e3f40b929e969e0a118939caa1ccb33cc2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="azure-active-directory-b2c-secure-your-restful-services-using-client-certificates"></a>Azure Active Directory B2C: Sichern Ihrer RESTful-Dienste mit Clientzertifikaten
Nach dem [Erstellen eines RESTful-Diensts](active-directory-b2c-custom-rest-api-netfw.md) zur Interaktion mit Azure AD B2C erfahren Sie, wie Sie den Zugriff auf Ihre Azure-Web-App (RESTful-API) mithilfe eines Clientzertifikats beschränken. Dieser Mechanismus wird als gegenseitige TLS-Authentifizierung oder **Clientzertifikatauthentifizierung** bezeichnet.  Nur Dienste wie Azure AD B2C mit dem richtigen Zertifikat können auf Ihren Dienst zugreifen.

> [!NOTE]
>
>Sie können Ihren RESTful-Dienst auch [unter Verwendung der HTTP-Standardauthentifizierung](active-directory-b2c-custom-rest-api-netfw-secure-basic.md) sichern. Jedoch sollten Sie wissen, dass HTTP-Standardauthentifizierung als unsicherer als die Authentifizierung über das Clientzertifikat gilt. Unsere Empfehlung: Sichern Sie den RESTful-Dienst wie in diesem Artikel beschrieben mit der Clientzertifikatauthentifizierung.

In diesem Artikel wird Folgendes ausführlich erläutert:
1. Einrichten Ihrer Web-App für die Verwendung der Clientzertifikatauthentifizierung
1. Hochladen des Zertifikats in Azure AD B2C-Richtlinienschlüssel
1. Konfigurieren Ihrer benutzerdefinierten Richtlinie zur Verwendung des Clientzertifikats

## <a name="prerequisites"></a>Voraussetzungen
* Führen Sie die vorherigen Schritte im Artikel [Azure Active Directory B2C: Integrate REST API claims exchanges in your Azure AD B2C user journey as validation on user input](active-directory-b2c-custom-rest-api-netfw.md) (Azure Active Directory B2C: Integrieren eines REST-API-Ansprücheaustauschs in Ihre Azure AD B2C-User Journey als Validierung bei Benutzereingabe) aus.
* Sie benötigen ein gültiges Zertifikat (PFX-Datei mit privatem Schlüssel).

## <a name="step-1-configure-web-app-for-client-certificate-authentication"></a>Schritt 1: Konfigurieren einer Web-App für die Clientzertifikatauthentifizierung
Damit der **Azure App Service** so eingerichtet werden kann, dass Clientzertifikate erforderlich sind, muss die Web-App-Standorteinstellung `clientCertEnabled` auf „true“ gesetzt werden. Diese Einstellung ist derzeit nicht auf der Verwaltungsoberfläche im Azure-Portal verfügbar, weshalb Sie dazu die REST-API verwenden müssen.

> [!NOTE]
>
>Stellen Sie sicher, dass Sie mindestens den standardmäßigen Azure App Service-Plan verwenden. Weitere Informationen hierzu finden Sie unter [Azure App Service-Pläne – Detaillierte Übersicht](https://docs.microsoft.com/en-us/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).


Verwenden Sie den [Azure-Ressourcen-Explorer (Vorschau)](https://resources.azure.com), um die Eigenschaft clientCertEnabled auf „true“ zu setzen. Der folgende Screenshot zeigt, wie der Wert clientCertEnabled über den Azure-Ressourcen-Explorer festgelegt wird. ![Einstellung von clientCertEnabled über den Azure-Ressourcen-Explorer](media/aadb2c-ief-rest-api-netfw-secure-cert/rest-api-netfw-secure-client-cert-resource-explorer.png)

> [!NOTE]
>
>Weitere Informationen zum Festlegen des clientCertEnabled-Attributs finden Sie unter: [Konfigurieren der gegenseitigen TLS-Authentifizierung für eine Web-App](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-web-configure-tls-mutual-auth).


> [!TIP]
>
>Alternativ können Sie das Tool [ARMClient](https://github.com/projectkudu/ARMClient) zum Vereinfachen des REST-API-Aufrufs verwenden.

## <a name="step-2-upload-your-certificate-to-azure-ad-b2c-policy-keys"></a>Schritt 2: Hochladen Ihres Zertifikats in Azure AD B2C-Richtlinienschlüssel
Nachdem Sie `clientCertEnabled` auf `true` festgelegt haben, setzt die Kommunikation mit Ihrer REST-API nun ein Clientzertifikat voraus. Zu diesem Zweck müssen Sie das Clientzertifikat hochladen und in Ihrem Azure AD B2C-Mandanten speichern.   
1.  Wechseln Sie zu Ihrem Azure AD B2C-Mandanten, und wählen Sie **B2C-Einstellungen** > **Framework für die Identitätsfunktion**.
2.  Wählen Sie **Policy Keys** (Richtlinienschlüssel), um die in Ihrem Mandanten verfügbaren Schlüssel anzuzeigen.
3.  Klicken Sie auf **+Hinzufügen**.
4.  Verwenden Sie für **Optionen** die Option **Hochladen**.
5.  Verwenden Sie für **Name** den Wert `B2cRestClientCertificate`.  
    Das Präfix `B2C_1A_` wird automatisch hinzugefügt.
6.  Wählen Sie in **Dateiupload** Ihre Zertifikat-PFX-Datei mit dem privaten Schlüssel. Geben Sie außerdem das **Kennwort** des Zertifikats an.

    ![Hochladen des Richtlinienschlüssels](media/aadb2c-ief-rest-api-netfw-secure-cert/rest-api-netfw-secure-client-cert-upload.png)

7.  Klicken Sie auf **Erstellen**
8.  Um zu bestätigen, dass Sie den Schlüssel `B2C_1A_B2cRestClientCertificate` erstellt haben, wählen Sie **Richtlinienschlüssel**, um die in Ihrem Mandanten verfügbaren Schlüssel anzuzeigen.

## <a name="step-3-change-the-technicalprofile-to-support-client-certificate-authentication-in-your-extension-policy"></a>Schritt 3: Ändern Sie `TechnicalProfile`, um die Clientzertifikatauthentifizierung in Ihrer Erweiterungsrichtlinie zu unterstützen
1.  Öffnen Sie die Erweiterungsrichtliniendatei (TrustFrameworkExtensions.xml) von Ihrem Arbeitsverzeichnis aus.
2.  Suchen Sie den `<TechnicalProfile>`-Knoten, der in `Id="REST-API-SignUp"` enthalten ist.
3.  Suchen Sie das `<Metadata>`-Element.
4.  Ändern Sie `AuthenticationType` in `ClientCertificate`.

    ```xml
    <Item Key="AuthenticationType">ClientCertificate</Item>
    ```

5.  Fügen Sie den folgenden XML-Codeausschnitt unmittelbar nach dem Schließen des `<Metadata>`-Elements hinzu:  

    ```xml
    <CryptographicKeys>
        <Key Id="ClientCertificate" StorageReferenceId="B2C_1A_B2cRestClientCertificate" />
    </CryptographicKeys>
    ```

Nach dem Hinzufügen der XML-Codeausschnitte sollte Ihr `TechnicalProfile` wie folgt aussehen:

![Festlegen der ClientCertificate-Authentifizierungs-XML-Elemente](media/aadb2c-ief-rest-api-netfw-secure-cert/rest-api-netfw-secure-client-cert-tech-profile.png)

## <a name="step-4-upload-the-policy-to-your-tenant"></a>Schritt 4: Hochladen der Richtlinie in Ihren Mandanten

1.  Wechseln Sie im [Azure-Portal](https://portal.azure.com) in den [Kontext Ihres Azure AD B2C-Mandanten](active-directory-b2c-navigate-to-b2c-context.md), und klicken Sie auf **Azure AD B2C**.
2.  Wählen Sie **Framework für die Identitätsfunktion** aus.
3.  Klicken Sie auf **Alle Richtlinien**.
4.  Wählen Sie **Richtlinie hochladen** aus.
5.  Aktivieren Sie das Kontrollkästchen **Richtlinie überschreiben, sofern vorhanden**.
6.  **Laden** Sie „TrustFrameworkExtensions.xml“ hoch, und stellen Sie sicher, dass kein Fehler bei der Validierung auftritt.

## <a name="step-5-test-the-custom-policy-by-using-run-now"></a>Schritt 5: Testen der benutzerdefinierten Richtlinie mit „Jetzt ausführen“
1.  Öffnen Sie die **Azure AD B2C-Einstellungen**, und navigieren Sie zu **Framework für die Identitätsfunktion**.

    >[!NOTE]
    >
    >    **Jetzt ausführen** macht erforderlich, dass mindestens eine Anwendung im Mandanten vorab registriert wird. 
    >    Informationen zum Registrieren von Anwendungen finden Sie in den Artikeln [Erste Schritte](active-directory-b2c-get-started.md) bzw. [Anwendungsregistrierung](active-directory-b2c-app-registration.md) zu Azure AD B2C.

2.  Öffnen Sie **B2C_1A_signup_signin** – die benutzerdefinierte Richtlinie der vertrauenden Seite (Relying Party, RP), die Sie hochgeladen haben. Wählen Sie **Jetzt ausführen** aus.

3.  Versuchen Sie, „Test“ in das Feld **Vorname** einzugeben. B2C zeigt die Fehlermeldung am oberen Rand der Seite an.

    ![Testen Ihrer Identitäts-API](media/aadb2c-ief-rest-api-netfw-secure-basic/rest-api-netfw-test.png)

4.  Versuchen Sie, einen Namen (nicht „Test“) in das Feld **Vorname** einzugeben. B2C meldet den Benutzer an und sendet dann loyaltyNumber an Ihre Anwendung. Beachten Sie die Nummer in diesem JWT in diesem Beispiel.

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

   > [!NOTE]
   >Wenn Sie die obige Fehlermeldung erhalten, bedeutet dies, dass Azure AD B2C während der Vorlage des Clientzertifikats erfolgreich Ihren RESTful-Dienst aufgerufen hat. Der nächste Schritt besteht im Überprüfen des Zertifikats.

## <a name="step-6-adding-certificate-validation"></a>Schritt 6: Hinzufügen der Überprüfung des Zertifikats
Das Clientzertifikat, das Azure AD B2C an Ihren RESTful-Dienst sendet, wird von der Azure-Web-Apps-Plattform keiner Überprüfung unterzogen (mit Ausnahme der Überprüfung des Vorhandenseins). Für das Überprüfen des Zertifikats ist die Web-App zuständig. Hier ist ASP.NET-Beispielcode, mit dem die Zertifikateigenschaften zum Zweck der Authentifizierung überprüft werden.

> [!NOTE]
>Weitere Informationen zum Konfigurieren von Azure App Service für die Clientzertifikatauthentifizierung finden Sie unter: [Konfigurieren der gegenseitigen TLS-Authentifizierung für eine Web-App](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-web-configure-tls-mutual-auth).

### <a name="61-add-application-settings-to-projects-webconfig-file"></a>6.1 Hinzufügen von Anwendungseinstellungen zur Datei „web.config“ des Projekts
Öffnen Sie das zuvor erstellte Visual Studio-Projekt, und fügen Sie der Datei „web.config“ unter dem Element `appSettings` die folgenden Anwendungseinstellungen hinzu.

```XML
<add key="ClientCertificate:Subject" value="CN=Subject name" />
<add key="ClientCertificate:Issuer" value="CN=Issuer name" />
<add key="ClientCertificate:Thumbprint" value="Certificate thumbprint" />
```

Ersetzen Sie die Werte des Zertifikats:
* Subject name
* Issuer name
* Certificate thumbprint

### <a name="62-add-isvalidclientcertificate-function"></a>6.2 Hinzufügen der Funktion IsValidClientCertificate
Öffnen Sie „Controllers\IdentityController.cs“, und fügen Sie der Controllerklasse `Identity` die folgende Funktion hinzu. 

```C#
private bool IsValidClientCertificate()
{
    string ClientCertificateSubject = ConfigurationManager.AppSettings["ClientCertificate:Subject"];
    string ClientCertificateIssuer = ConfigurationManager.AppSettings["ClientCertificate:Issuer"];
    string ClientCertificateThumbprint = ConfigurationManager.AppSettings["ClientCertificate:Thumbprint"];

    X509Certificate2 clientCertInRequest = RequestContext.ClientCertificate;
    if (clientCertInRequest == null)
    {
        Trace.WriteLine("Certificate is NULL");
        return false;
    }

    // Basic verification
    if (clientCertInRequest.Verify() == false)
    {
        Trace.TraceError("Basic verification failed");
        return false;
    }

    // 1. Check time validity of certificate
    if (DateTime.Compare(DateTime.Now, clientCertInRequest.NotBefore) < 0 ||
        DateTime.Compare(DateTime.Now, clientCertInRequest.NotAfter) > 0)
    {
        Trace.TraceError($"NotBefore '{clientCertInRequest.NotBefore}' or NotAfter '{clientCertInRequest.NotAfter}' not valid");
        return false;
    }

    // 2. Check subject name of certificate
    bool foundSubject = false;
    string[] certSubjectData = clientCertInRequest.Subject.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
    foreach (string s in certSubjectData)
    {
        if (String.Compare(s.Trim(), ClientCertificateSubject) == 0)
        {
            foundSubject = true;
            break;
        }
    }

    if (!foundSubject)
    {
        Trace.TraceError($"Subject name '{clientCertInRequest.Subject}' is not valid");
        return false;
    }
    
    // 3. Check issuer name of certificate
    bool foundIssuerCN = false;
    string[] certIssuerData = clientCertInRequest.Issuer.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
    foreach (string s in certIssuerData)
    {
        if (String.Compare(s.Trim(), ClientCertificateIssuer) == 0)
        {
            foundIssuerCN = true;
            break;
        }
    }

    if (!foundIssuerCN)
    {
        Trace.TraceError($"Issuer '{clientCertInRequest.Issuer}' is not valid");
        return false;
    }

    // 4. Check thumprint of certificate
    if (String.Compare(clientCertInRequest.Thumbprint.Trim().ToUpper(), ClientCertificateThumbprint) != 0)
    {
        Trace.TraceError($"Thumbprint '{clientCertInRequest.Thumbprint.Trim().ToUpper()}' is not valid");
        return false;
    }

    // 5. If you also want to test if the certificate chains to a Trusted Root Authority you can uncomment the code below
    //
    //X509Chain certChain = new X509Chain();
    //certChain.Build(certificate);
    //bool isValidCertChain = true;
    //foreach (X509ChainElement chElement in certChain.ChainElements)
    //{
    //    if (!chElement.Certificate.Verify())
    //    {
    //        isValidCertChain = false;
    //        break;
    //    }
    //}
    //if (!isValidCertChain) return false;
    return true;
}
```

In obigen **Beispiel** wird das Zertifikat nur dann als gültig akzeptiert, wenn alle Bedingungen erfüllt sind:
1. Das Zertifikat ist nicht abgelaufen und für die aktuelle Uhrzeit auf dem Server aktiv.
2. Der `Subject`-Name des Zertifikats mit dem allgemeinen Namen entspricht dem Anwendungseinstellungswert `ClientCertificate:Subject`.
3. Der `Issuer`-Name des Zertifikats mit dem allgemeinen Namen entspricht dem Anwendungseinstellungswert `ClientCertificate:Issuer`.
4. Der `thumbprint` des Zertifikats entspricht dem Anwendungseinstellungswert `ClientCertificate:Thumbprint`.

> [!IMPORTANT]
>
>Abhängig von der Vertraulichkeit Ihres Diensts müssen Sie möglicherweise weitere Überprüfungen hinzufügen. Beispiel: Testen, ob das Zertifikat mit einer vertrauenswürdigen Stammzertifizierungsstelle verkettet ist, Überprüfung des Namens der Organisation des Zertifikatausstellers usw.

### <a name="63-add-isvalidclientcertificate-function"></a>6.3 Hinzufügen der Funktion IsValidClientCertificate
Öffnen Sie „Controllers\IdentityController.cs“ am Anfang der Funktion `SignUp()`, um die folgenden Codezeilen hinzuzufügen. 

```C#
if (IsValidClientCertificate() == false)
{
    return Content(HttpStatusCode.Conflict, new B2CResponseContent("Your client certificate is not valid", HttpStatusCode.Conflict));
}
```

Nach dem Hinzufügen der Codeausschnitte sollte Ihr `Identity`-Controller wie folgt aussehen:

![Hinzufügen des Zertifikatüberprüfungscodes](media/aadb2c-ief-rest-api-netfw-secure-cert/rest-api-netfw-secure-client-code.png)

## <a name="step-7-publish-to-azure-and-test"></a>Schritt 7: Veröffentlichen in Azure und Testen
1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **Contoso.AADB2C.API**, und wählen Sie **Veröffentlichen** aus.
2. Wiederholen Sie Schritt 6, und testen Sie (erneut) die benutzerdefinierte Richtlinie mit der Überprüfung des Zertifikats. Versuchen Sie, die Richtlinie auszuführen, und stellen Sie sicher, dass alles funktioniert, nachdem Sie die Validierung hinzugefügt haben.
3. Ändern Sie jetzt in der Datei „web.config“ den Wert von `ClientCertificate:Subject` in einen **ungültigen** Wert. Führen Sie die Richtlinie erneut aus, und dann sollte eine Fehlermeldung angezeigt werden.
4. Ändern Sie den Wert wieder in einen **gültigen** Wert, und stellen Sie sicher, dass die Richtlinie Ihre REST-API aufrufen kann.
5. Problembehandlung durch [Erfassen von Protokollen mit Application Insights](active-directory-b2c-troubleshoot-custom.md)

## <a name="optional-download-the-complete-policy-files-and-code"></a>[Optional] Herunterladen der vollständigen Richtliniendateien und des Codes
* Sie sollten nach Abschluss der exemplarischen Vorgehensweise „Erste Schritte mit benutzerdefinierten Richtlinien“ Ihr Szenario mithilfe Ihrer eigenen Dateien mit benutzerdefinierten Richtlinien erstellen, anstatt diese Beispieldateien zu verwenden.  [Beispielrichtliniendateien zur Referenz](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-rest-api-netfw-secure-cert)
* Sie können den vollständigen Code hier herunterladen: [Visual Studio-Beispiellösung zur Referenz](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-rest-api-netfw/Contoso.AADB2C.API).