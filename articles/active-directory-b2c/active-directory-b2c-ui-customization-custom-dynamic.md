---
title: "Azure Active Directory B2C: Dynamisches Anpassen der Azure AD B2C-Benutzeroberfläche (UI) mit benutzerdefinierten Richtlinien"
description: "Unterstützung mehrerer Brandingdarstellungen mit HTML/CSS-Inhalt, der sich während der Laufzeit dynamisch ändert"
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
ms.date: 09/20/2017
ms.author: yoelh
ms.openlocfilehash: cbce9b72c25c90dde526ff229f77a85ce9478efa
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="azure-active-directory-b2c-configure-the-ui-with-dynamic-content-using-custom-policies"></a>Azure Active Directory B2C: Konfigurieren der Benutzeroberfläche mit dynamischen Inhalten mithilfe von benutzerdefinierten Richtlinien
Mit benutzerdefinierten Azure AD B2C-Richtlinien können Sie über einen Parameter in einer Abfragezeichenfolge senden. Dieser Parameter wird an Ihren HTML-Endpunkt übergeben und kann den Seiteninhalt dynamisch ändern. Sie können z.B. das Hintergrundbild für die B2C-Registrierung oder -Anmeldung auf der Basis eines Parameters ändern, den Sie von der Web-/mobilen Anwendung übergeben. 

## <a name="prerequisites"></a>Voraussetzungen
Dieser Artikel konzentriert sich auf das Anpassen der Azure AD B2C-Benutzeroberfläche mit **dynamischen Inhalten** unter Verwendung benutzerdefinierter Richtlinien. Lesen Sie zum Einstieg in die Benutzeroberflächenanpassung mithilfe benutzerdefinierter Richtlinien den Artikel [Azure Active Directory B2C: Konfigurieren der Benutzeroberflächenanpassung in einer benutzerdefinierten Richtlinie](active-directory-b2c-ui-customization-custom.md). 

>[!NOTE]
>
>  Im Artikel [Azure Active Directory B2C: Konfigurieren der Benutzeroberflächenanpassung in einer benutzerdefinierten Richtlinie](active-directory-b2c-ui-customization-custom.md) werden die Grundlagen der Azure AD B2C-Benutzeroberflächenanpassung mit benutzerdefinierten Richtlinien erläutert:
> * Das Anpassungsfeature für die Seitenbenutzeroberfläche (UI).
> * Ein Tool, mit dem Sie das Anpassungsfeature für die Seiten-UI mit unserem Beispielinhalt testen können.
> * Die wichtigsten UI-Elemente in jedem Seitentyp.
> * Bewährte Methoden beim Üben des Umgangs mit diesem Feature.

## <a name="adding-a-link-to-html5css-templates-to-your-user-journey"></a>Hinzufügen eines Links zu HTML5/CSS-Vorlagen für die User Journey

In einer benutzerdefinierten Richtlinie definiert eine Inhaltsdefinition den HTML5-Seiten-URI, der für einen bestimmten Schritt der Benutzeroberfläche verwendet wird. Beispiel: die Registrierungs- oder Anmeldungsseiten. Die Basisrichtlinie definiert das standardmäßige Aussehen und Verhalten durch Verweis auf einen URI der HTML5-Dateien (er verweist auf seine CSS-Dateien). In der Erweiterungsrichtlinie können Sie das Aussehen und Verhalten durch Überschreiben des LoadUri für diese HTML5-Datei ändern. Inhaltsdefinitionen als solche enthalten URLs zu externem Inhalt, der durch Erstellen von HTML5/CSS-Dateien nach Bedarf definiert wird. 

Der Abschnitt `ContentDefinitions` enthält eine Reihe von `ContentDefinition`-XML-Elementen. Das ID-Attribut des `ContentDefinition`-Elements gibt den Typ der Seiten an, der mit der Inhaltsdefinition verknüpft ist. Folglich auch den Kontext, in dem eine benutzerdefinierte HTML5/CSS-Vorlage verwendet werden soll. In der folgenden Tabelle werden die Gruppe mit den IDs der Inhaltsdefinitionen, die von der IEF-Engine erkannt werden, und der Typ der entsprechenden Seiten beschrieben.

| ID für Inhaltsdefinition | Standardmäßige HTML5-Vorlage| Beschreibung | 
|-----------------------|--------|-------------|
| *api.error* | [exception.cshtml](https://login.microsoftonline.com/static/tenant/default/exception.cshtml) | **Fehlerseite**: Diese Seite wird angezeigt, wenn eine Ausnahme oder ein Fehler auftreten. |
| *api.idpselections* | [idpSelector.cshtml](https://login.microsoftonline.com/static/tenant/default/idpSelector.cshtml) | **Seite zur Auswahl des Identitätsanbieters**: Diese Seite enthält eine Liste mit den Identitätsanbietern, unter denen der Benutzer bei der Anmeldung wählen kann. Bei diesen Optionen handelt es sich um typische Unternehmensidentitätsanbieter, Identitätsanbieter in Form von sozialen Netzwerken wie Facebook und Google+ oder lokale Konten. |
| *api.idpselections.signup* | [idpSelector.cshtml](https://login.microsoftonline.com/static/tenant/default/idpSelector.cshtml) | **Seite zur Auswahl des Identitätsanbieters für die Registrierung**: Diese Seite enthält eine Liste mit den Identitätsanbietern, aus denen der Benutzer bei der Registrierung auswählen kann. Bei diesen Optionen handelt es sich um Unternehmensidentitätsanbieter, Identitätsanbieter in Form von sozialen Netzwerken wie Facebook und Google+ oder lokale Konten. |
| *api.localaccountpasswordreset* | [selfasserted.html](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) | **Seite „Kennwort vergessen“**: Diese Seite enthält ein Formular, das vom Benutzer ausgefüllt werden muss, um eine Kennwortzurücksetzung zu initiieren.  |
| *api.localaccountsignin* | [selfasserted.html](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) | **Seite für Anmeldung mit lokalem Konto**: Diese Seite enthält ein Anmeldeformular für die Anmeldung mit einem lokalen Konto, das auf einer E-Mail-Adresse oder einem Benutzernamen basiert. Das Formular kann ein Texteingabefeld und ein Kennworteingabefeld enthalten. |
| *api.localaccountsignup* | [selfasserted.html](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) | **Seite für Registrierung mit lokalem Konto**. Diese Seite enthält ein Registrierungsformular für die Registrierung für ein lokales Konto, das auf einer E-Mail-Adresse oder einem Benutzernamen basiert. Das Formular kann verschiedene Eingabesteuerelemente enthalten, z.B. ein Texteingabefeld, ein Kennworteingabefeld, ein Optionsfeld, Dropdownfelder mit einer Auswahlmöglichkeit und Kontrollkästchen mit mehreren Optionen. |
| *api.phonefactor* | [multifactor-1.0.0.cshtml](https://login.microsoftonline.com/static/tenant/default/multifactor-1.0.0.cshtml) | **Seite „Multi-Factor Authentication“**: Auf dieser Seite können Benutzer während der Registrierung oder Anmeldung ihre Telefonnummern verifizieren (per SMS oder Sprachnachricht). |
| *api.selfasserted* | [selfasserted.html](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) | **Seite zur Registrierung über Konto für soziales Netzwerk**: Diese Seite enthält ein Registrierungsformular, das Benutzer ausfüllen müssen, wenn die Registrierung mit einem vorhandenen Konto bei einem sozialen Netzwerk als Identitätsanbieter erfolgt. Diese Seite ähnelt der vorherigen Seite für die Registrierung bei einem Konto für ein soziales Netzwerk, mit Ausnahme der Felder für die Kennworteingabe. |
| *api.selfasserted.profileupdate* | [updateprofile.html](https://login.microsoftonline.com/static/tenant/default/updateProfile.cshtml) | **Seite für Profilaktualisierung**: Diese Seite enthält ein Formular, das von Benutzern zum Aktualisieren des Profils verwendet werden kann. Diese Seite ähnelt der Seite für die Registrierung bei einem Konto für ein soziales Netzwerk, mit Ausnahme der Felder für die Kennworteingabe. |
| *api.signuporsignin* | [unified.html](https://login.microsoftonline.com/static/tenant/default/unified.cshtml) | **Einheitliche Seite für Registrierung oder Anmeldung**: Auf dieser Seite wird sowohl die Registrierung als auch Anmeldung von Benutzern durchgeführt. Die Benutzer können Unternehmensidentitätsanbieter, Identitätsanbieter in Form von sozialen Netzwerken wie Facebook und Google+ oder lokale Konten verwenden.  |

## <a name="serving-dynamic-content"></a>Bedienen von dynamischem Inhalt
Im vorherigen Artikel [Azure Active Directory B2C: Konfigurieren der Benutzeroberflächenanpassung in einer benutzerdefinierten Richtlinie](active-directory-b2c-ui-customization-custom.md) haben Sie die HTML5-Dateien in Azure Blob Storage hochgeladen. Diese HTML5-Dateien sind statisch und stellen für jede Anforderung den gleichen HTML-Inhalt dar. In diesem Artikel verwenden wir die ASP.Net-Web-App, die Abfragezeichenfolgen-Parameter akzeptieren und entsprechend reagieren kann. In dieser exemplarischen Vorgehensweise führen Sie folgende Aktionen aus:
* Erstellen einer ASP.NET Core-Webanwendung, die Ihre HTML5-Vorlage hostet 
* Hinzufügen der benutzerdefinierten HTML5-Vorlage _unified.cshtml_ 
* Veröffentlichen Ihrer Web-App in Azure App Service 
* Festlegen von CORS für Ihre Web-App
* Überschreiben der `LoadUri`-Elemente, sodass sie auf Ihre HTML5-Datei verweisen

## <a name="step-1-create-an-aspnet-web-app"></a>Schritt 1: Erstellen einer ASP.NET-Web-App

1.  Erstellen Sie in Visual Studio ein Projekt durch Auswählen von **Datei > Neu > Projekt**.
2.  Wählen Sie im Dialogfeld **Neues Projekt** die Optionen **Visual C# > Web > ASP.NET Core-Webanwendung (.NET Framework)** aus.
3.  Nennen Sie die Anwendung z.B. „Contoso.AADB2C.UI“, und wählen Sie dann **OK**.

    ![Erstellen eines neuen Visual Studio-Projekts](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-create-project1.png)

4.  Wählen Sie die Vorlage **Webanwendung**.
5.  Stellen Sie sicher, dass für die Authentifizierung **Keine Authentifizierung** festgelegt ist.

    ![Auswählen der Web-API-Vorlage](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-create-project2.png)

6.  Klicken Sie auf **OK**, um das Projekt zu erstellen.

## <a name="step-2-create-mvc-view"></a>Schritt 2: Erstellen der MVC-Ansicht
### <a name="step-21-download-b2c-built-in-html5-template"></a>Schritt 2.1: Herunterladen des in der HTML5-Vorlage erstellten B2C
Ihre benutzerdefinierte HTML5-Vorlage basiert auf dem in der HTML5-Vorlage erstellten B2C. Sie können die Datei [unified.html](https://login.microsoftonline.com/static/tenant/default/unified.cshtml) herunterladen, oder laden Sie vom [Startpaket](https://github.com/AzureADQuickStarts/B2C-AzureBlobStorage-Client/tree/master/sample_templates/wingtip) herunter. Sie verwenden diese HTML5-Datei für eine einheitliche Registrierungs- oder Anmeldeseite.

### <a name="step-22-add-mvc-view"></a>Schritt 2.2: Hinzufügen der MVC-Ansicht
1. Klicken Sie mit der rechten Maustaste auf den Ordner „Views/Home“, und klicken Sie dann auf **Hinzufügen > Neues Element**.
 ![Hinzufügen des neuen MVC-Elements](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-add-view1.png)
2. Wählen Sie im Dialogfeld **Neues Element hinzufügen – Contoso.AADB2C.UI** **Web > ASP > NET**.
3. Tippen Sie auf **MVC-Ansichtsseite**.
4. Ändern Sie im Feld **Name** den Namen in _unified.cshtml_.
5. Klicken Sie auf **Hinzufügen**
 ![MVC-Ansicht hinzufügen](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-add-view2.png).
6.  Wenn diese Datei noch nicht geöffnet ist, doppelklicken Sie auf die Datei, um sie zu öffnen. Löschen Sie den Inhalt der Datei _unified.cshtml_.
7. Für Demonstrationszwecke entfernen wir den Verweis auf die Layoutseite. Fügen Sie folgenden Codeausschnitt der Datei _unified.cshtml_ hinzu.

    ```C#
    @{
        Layout = null;
    }
    ```

8. Öffnen Sie die Datei _unified.cshtml_, die Sie aus dem in der HTML5-Vorlage erstellten AAD-B2C heruntergeladen haben.
9. Suchen Sie die `@`-Zeichen, und ersetzen Sie sie durch `@@`.
10. Kopieren Sie den Inhalt der Datei, und fügen Sie ihn unterhalb der Layout-Definition ein. Der Code sollte wie folgt aussehen: ![Datei „unified.cshtml“ nach dem Hinzufügen von HTML5](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-edit-view1.png)
### <a name="step-23-change-the-background-image"></a>Schritt 2.3: Ändern des Hintergrundbilds
10. Ersetzen Sie im `<img>`-Element mit der ID `background_background_image` `src` durch _https://kbdevstorage1.blob.core.windows.net/asset-blobs/19889_en_1_ oder ein anderes Hintergrundbild Ihrer Wahl.
![Ändern des Seitenhintergrunds](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-add-static-background.png)
### <a name="step-24-add-you-view-to-mvc-controller"></a>Schritt 2.4: Hinzufügen Ihrer Ansicht zum MVC-Controller
Öffnen Sie **Controllers\HomeController. cs**, und fügen Sie die folgende Methode hinzu. 
```C
public IActionResult unified()
{
    return View();
}
```
Dieser Code gibt an, dass die Methode eine Ansichtsvorlagendatei verwenden soll, um eine Antwort an den Browser auszugeben. Da wir den Namen der Ansichtsvorlagendatei nicht explizit angeben, verwendet MVC standardmäßig die Datei _unified.cshtml_ im Ordner „/Views/Home“.

Nach dem Hinzufügen der _unified_-Methode sollte der Code wie folgt aussehen: ![Ändern des Controllers zum Ausgeben der Ansicht](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-controller-view.png)

Debuggen Sie Ihre Web-App, und stellen Sie sicher, dass der Zugriff auf die _einheitliche_ Seite möglich ist. Beispiel: `http://localhost:<Port number>/Home/unified`

### <a name="step-25-publish-to-azure"></a>Schritt 2.5: Veröffentlichen in Azure
1.  Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **Contoso.AADB2C.UI**, und wählen Sie **Veröffentlichen** aus.

    ![Veröffentlichen in Microsoft Azure App Service](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-publish1.png)

2.  Vergewissern Sie sich, dass **Microsoft Azure App Service** ausgewählt ist, und wählen Sie **Veröffentlichen** aus.

    ![Erstellen eines neuen Microsoft Azure App Service](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-publish2.png)

    Im Dialogfeld **App Service erstellen** können Sie alle Azure-Ressourcen erstellen, die benötigt werden, um die ASP.NET-Web-App in Azure auszuführen.

> [!NOTE]
>
>Weitere Informationen zum Veröffentlichen finden Sie unter [Erstellen von ASP.NET-Web-Apps in Azure](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-web-get-started-dotnet#publish-to-azure).

3.  Geben Sie unter **Web-App-Name** einen eindeutigen Web-App-Namen ein (gültige Zeichen sind `a-z`, `0-9` und `-`). Die URL der Web-App lautet `http://<app_name>.azurewebsites.NET`, wobei `<app_name>` der Name der Web-App ist. Sie können den automatisch generierten Namen übernehmen. Dieser ist eindeutig.

    Wählen Sie **Erstellen** aus, um mit der Erstellung der Azure-Ressourcen zu beginnen.

    ![Bereitstellen der App Service-Eigenschaften](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-publish3.png)

4.  Nach Abschluss des Assistenten wird die ASP.NET-Web-App in Azure veröffentlicht und anschließend im Standardbrowser gestartet. Kopieren Sie die URL der _einheitlichen_ Seite. Beispiel: _https://<app_name>.azurewebsites.net/home/unified_.

## <a name="step-3-configure-cors-in-azure-app-service"></a>Schritt 3: Konfigurieren von CORS in Azure App Service
1. Navigieren Sie in Ihrem Browser zum [Azure-Portal](https://portal.azure.com/).

2. Klicken Sie auf **App Services**und dann auf den Namen der API-App.

    ![API-App im Portal auswählen](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-CORS1.png)

3. Scrollen Sie im Abschnitt **Einstellungen** nach unten zum Abschnitt **API**, und klicken Sie dann auf **CORS**.

    ![Auswählen der CORS-Einstellungen](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-CORS2.png)

4. Geben Sie im Textfeld die URLs ein, von denen JavaScript-Aufrufe ausgehen sollen.
Alternativ können Sie ein Sternchen (*) eingeben, um anzugeben, dass alle Ursprungsdomänen akzeptiert werden.

5. Klicken Sie auf **Speichern**.

    ![Klicken Sie auf Speichern.](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-CORS3.png)

Nachdem Sie auf „Speichern“ geklickt haben, akzeptiert die API-App JavaScript-Aufrufe von den angegebenen URLs. 

## <a name="step-4-html5-template-validation"></a>Schritt 4: Überprüfung der HTML5-Vorlage
Die HTML5-Vorlage ist einsatzbereit.  Sie ist jedoch in keiner `ContentDefinition` verfügbar. Vor dem Hinzufügen der `ContentDefinition` zu Ihrer benutzerdefinierten Richtlinie müssen Sie:
* Sicherstellen, dass Ihr Inhalt HTML5-konform und zugänglich ist
* Sicherstellen, dass Ihr Inhaltsserver für CORS aktiviert ist

>[!NOTE]
>
>Sie können die Website „http://test-cors.org/“ verwenden, um zu bestätigen, dass die von Ihnen gehostete Website für Ihren Inhalt über eine CORS-Aktivierung verfügt, und um CORS-Anforderungen zu testen. 

* Bediente Inhalte sind über **HTTPS** sicher.
* Verwenden Sie **absolute** URLs, z.B. „https://ihredomäne/content“, für alle Links sowie den gesamten CSS-Inhalt und Bilder.

## <a name="step-5-configure-your-content-definition"></a>Schritt 5: Konfigurieren Ihrer Inhaltsdefinition
So konfigurieren Sie die `ContentDefinition`
1.  Öffnen Sie die Basisdatei Ihrer Richtlinie (z.B. „TrustFrameworkBase.xml“).
2.  Suchen Sie das `<ContentDefinitions>`-Element, und kopieren Sie den gesamten Inhalt des `<ContentDefinitions>`-Knotens.
3.  Öffnen Sie die Erweiterungsdatei (z.B. „TrustFrameworkExtensions.xml“), und suchen Sie nach dem Element `<BuildingBlocks>`. Wenn das Element nicht vorhanden ist, fügen Sie ein solches hinzu.
4.  Fügen Sie den gesamten Inhalt des `<ContentDefinitions>`-Knotens ein, den Sie als untergeordnetes Element des `<BuildingBlocks>`-Elements kopiert haben. 
5.  Suchen Sie den `<ContentDefinition>`-Knoten, der in der XML-Datei, die Sie kopiert haben, `Id="api.signuporsignin"` enthält.
6.  Ändern Sie den Wert von `LoadUri` von _~/tenant/default/unified_ in _https://app_name.azurewebsites.net/home/unified_. Ihre benutzerdefinierte Richtlinie sollte wie folgt aussehen: ![Ihre Inhaltsdefinition](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-content-definition.png)

## <a name="step-6-upload-the-policy-to-your-tenant"></a>Schritt 6: Hochladen der Richtlinie zu Ihrem Mandanten
1.  Wechseln Sie im [Azure-Portal](https://portal.azure.com) in den [Kontext Ihres Azure AD B2C-Mandanten](active-directory-b2c-navigate-to-b2c-context.md), und öffnen Sie das **Azure AD B2C**.
2.  Wählen Sie **Framework für die Identitätsfunktion** aus.
3.  Öffnen Sie **Alle Richtlinien**.
4.  Wählen Sie **Richtlinie hochladen** aus.
5.  Aktivieren Sie das Kontrollkästchen **Richtlinie überschreiben, sofern vorhanden**.
6.  **Laden** Sie „TrustFrameworkExtensions.xml“ hoch, und stellen Sie sicher, dass kein Fehler bei der Validierung auftritt.

## <a name="step-7-test-the-custom-policy-by-using-run-now"></a>Schritt 7: Testen der benutzerdefinierten Richtlinie mit „Jetzt ausführen“
1.  Öffnen Sie die **Azure AD B2C-Einstellungen**, und navigieren Sie zu **Framework für die Identitätsfunktion**.

    >[!NOTE]
    >
    >    **Jetzt ausführen** macht erforderlich, dass mindestens eine Anwendung im Mandanten vorab registriert wird. 
    >    Informationen zum Registrieren von Anwendungen finden Sie in den Artikeln [Erste Schritte](active-directory-b2c-get-started.md) bzw. [Anwendungsregistrierung](active-directory-b2c-app-registration.md) zu Azure AD B2C.
    >

2.  Öffnen Sie **B2C_1A_signup_signin** – die benutzerdefinierte Richtlinie der vertrauenden Seite (Relying Party, RP), die Sie hochgeladen haben. Wählen Sie **Jetzt ausführen** aus.
3.  Sie sollten Ihre benutzerdefinierten HTML5-Inhalte mit dem Hintergrund sehen, den Sie zuvor erstellt haben. ![Ihre Registrierungs- oder Anmeldungsrichtlinie](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-demo1.png)

## <a name="step-8-adding-dynamic-content"></a>Schritt 8: Hinzufügen von dynamischem Inhalt
In diesem Abschnitt ändern wir den Hintergrund basierend auf dem Abfragezeichenfolgen-Parameter mit dem Namen _campaignId_. Die Anwendung Ihrer vertrauenden Seite (Web-/mobile Apps) sendet den Parameter an AAD B2C. Die Richtlinie liest den Parameter und sendet seinen Wert an die HTML5-Vorlage. 


### <a name="step-81-adding-content-definition-parameter"></a>Schritt 8.1: Hinzufügen des Inhaltsdefinitionsparameter

So fügen Sie das `ContentDefinitionParameters`-Element hinzu:
1.  Öffnen Sie die SignUpOrSignin-Datei Ihrer Richtlinie (z.B. „SignUpOrSignin.xml“).
2.  Suchen Sie den `<UserJourneyBehaviors>`-Knoten. 
4.  Fügen Sie folgenden XML-Codeausschnitt im `<UserJourneyBehaviors>`-Knoten hinzu. 

    ```XML
    <ContentDefinitionParameters>
        <Parameter Name="campaignId">{OAUTH-KV:campaignId}</Parameter>
    </ContentDefinitionParameters>
    ```

### <a name="step-82-change-your-code-to-accept-query-string-parameter-and-replace-the-background-image-accordingly"></a>Schritt 8.2: Ändern Ihres Codes, sodass er Abfragezeichenfolgen-Parameter akzeptiert und das Hintergrundbild entsprechend ersetzt
In diesem Abschnitt ändern wir die HomeController-Methode _unified_, sodass der campaignId-Parameter akzeptiert wird. Anschließend überprüft die Methode seinen Wert und legt die `ViewData["background"]`-Variable entsprechend fest.

1. Öffnen Sie **Controllers\HomeController.cs** , und ändern Sie die `unified`-Methode durch folgenden Codeausschnitt:

    ```C#
    public IActionResult unified(string campaignId)
    {
        // If campaign ID is Hawaii, show Hawaii background
        if (campaignId != null && campaignId.ToLower() == "hawaii")
        {
            ViewData["background"] = "https://kbdevstorage1.blob.core.windows.net/asset-blobs/19889_en_1";
        }
        // If campaign ID is Tokyo, show Tokyo background
        else if (campaignId != null && campaignId.ToLower() == "tokyo")
        {
            ViewData["background"] = "https://kbdevstorage1.blob.core.windows.net/asset-blobs/19666_en_1";
        }
        // Default background
        else
        {
            ViewData["background"] = "https://kbdevstorage1.blob.core.windows.net/asset-blobs/18983_en_1";
        }

        return View();
    }

    ```

2. Suchen Sie das `<img>`-Element mit ID `background_background_image`, und ersetzen Sie `src` durch `@ViewData["background"]`.

    ![Ändern des Seitenhintergrunds](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-add-dynamic-background.png)

### <a name="83-upload-the-changes-and-publish-your-policy"></a>8.3 Hochladen der Änderungen und Veröffentlichen Ihrer Richtlinie
1. Veröffentlichen Sie Ihr Visual Studio-Projekt in Azure App Service.
2. Laden Sie die Richtliniendatei „SignUporSignIn.xml“ in AAD B2C hoch.
3.  Öffnen Sie **B2C_1A_signup_signin** – die benutzerdefinierte Richtlinie der vertrauenden Seite (Relying Party, RP), die Sie hochgeladen haben. Wählen Sie **Jetzt ausführen** aus. Daraufhin sollte das gleiche Hintergrundbild wie vorher angezeigt werden.
4. Kopieren Sie die URL aus der Adressleiste des Browsers.
5. Fügen Sie den Abfragezeichenfolgen-Parameter _campaignId_ dem URI hinzu. Fügen Sie z.B. `&campaignId=hawaii` hinzu, wie im folgenden Bild gezeigt. ![Ändern des Seitenhintergrunds](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-campaignId-param.png)
6. Drücken Sie die **EINGABETASTE**, und auf Ihrer Seite wird der Hawaii-Hintergrund angezeigt. ![Ändern des Seitenhintergrunds](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-demo2.png)
7. Ändern Sie jetzt den Wert in *Tokyo* , und drücken Sie die **EINGABETASTE**. Auf der Seite wird der Tokyo-Hintergrund angezeigt. ![Ändern des Seitenhintergrunds](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-demo3.png)

## <a name="step-9-change-the-rest-of-the-user-journey"></a>Schritt 9: Ändern des restlichen Teils der User Journey
Wenn Sie zur Anmeldeseite navigieren und auf **Jetzt registrieren** klicken, sehen Sie den Standardhintergrund, nicht den von Ihnen definierten. Dieses Verhalten ist darauf zurückzuführen, dass Sie nur die Registrierungs- oder Anmeldeseite geändert haben; Sie müssen jedoch auch den Rest der Selbst-bestätigt-Inhaltsdefinitionen ändern. Führen Sie hierzu folgende Schritte aus:
* In Schritt 2:
    * Laden Sie die **selfasserted**-Datei herunter.
    * Kopieren Sie den Dateiinhalt.
    * Erstellen Sie eine neue **selfasserted**-Ansicht.
    * Fügen Sie die **selfasserted**-Ansicht dem **Home**-Controller hinzu.
* In Schritt 4: 
    * Suchen Sie in Ihrer Erweiterungsrichtlinie den `<ContentDefinition>`-Knoten, der `Id="api.selfasserted"`, `Id="api.localaccountsignup"` und `Id="api.localaccountpasswordreset"` enthält.
    *  Legen Sie das `LoadUri`-Attribut auf Ihren Selfasserted-URI fest.
* In Schritt 8.2: Ändern Sie den Code, sodass er Abfragezeichenfolgen-Parameter akzeptiert. 
* Laden Sie die Richtlinie „TrustFrameworkExtensions.xml“ hoch, und stellen Sie sicher, dass kein Fehler bei der Validierung auftritt.
* Führen Sie den Test der Richtlinie aus, und klicken Sie auf **Jetzt registrieren**, um das Ergebnis anzuzeigen.

## <a name="optional-download-the-complete-policy-files-and-code"></a>[Optional] Herunterladen der vollständigen Richtliniendateien und des Codes
* Sie sollten nach Abschluss der exemplarischen Vorgehensweise „Erste Schritte mit benutzerdefinierten Richtlinien“ Ihr Szenario mithilfe Ihrer eigenen Dateien mit benutzerdefinierten Richtlinien erstellen, anstatt diese Beispieldateien zu verwenden.  [Beispielrichtliniendateien zur Referenz](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-ui-customization)
* Sie können den vollständigen Code hier herunterladen: [Visual Studio-Beispiellösung zur Referenz](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-ui-customization).




