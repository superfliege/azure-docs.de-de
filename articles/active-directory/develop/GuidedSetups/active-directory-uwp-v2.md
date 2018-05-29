---
title: Erste Schritte mit der UWP in Azure AD v2 | Microsoft-Dokumentation
description: Informationen, wie die Universelle Windows-Plattform-Anwendungen (XAML) eine API aufrufen können, die Zugriffstoken vom Azure Active Directory v2-Endpunkt anfordert
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/20/2018
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 390559922b3b8fb293d1c8b38f36dfd0a1df9ebd
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
ms.locfileid: "33763372"
---
# <a name="call-the-microsoft-graph-api-from-a-universal-windows-platform-uwp-application"></a>Aufrufen der Microsoft Graph-API über eine UWP-Anwendung (UWP = Universelle Windows-Plattform)

Diese Anleitung veranschaulicht, wie eine native Universelle Windows-Plattform-Anwendung (XAML) ein Zugriffstoken abrufen und anschließend die Microsoft Graph-API und andere APIs aufrufen kann, die Zugriffstoken von einem Azure Active Directory v2-Endpunkt anfordern.

Am Ende dieser Anleitung kann Ihre Anwendung eine geschützte API sowohl mit persönlichen Konten (z.B. outlook.com, live.com u.a.) als auch Geschäfts,- Schul- und Unikonten von Unternehmen oder Organisationen aufrufen, die mit Azure Active Directory arbeiten.  

> Für diesen Leitfaden ist Visual Studio 2017 mit einer installierten Entwicklung für die Universelle Windows-Plattform erforderlich. Im [Artikel](https://docs.microsoft.com/windows/uwp/get-started/get-set-up "Einrichten von Visual Studio für UWP") finden Sie Anweisungen zum Herunterladen und Konfigurieren von Visual Studio für die Entwicklung der Apps der Universellen Windows-Plattform.

### <a name="how-this-guide-works"></a>Funktionsweise dieser Anleitung

![Funktionsweise dieser Anleitung](media/active-directory-mobileanddesktopapp-windowsuniversalplatform-introduction/uwp-intro.png)

Die über diesen Leitfaden erstellte Beispiel-App ermöglicht einer UWP-Einzelseitenanwendung das Abfragen der Microsoft Graph-API oder einer Web-API, die Token von einem Azure Active Directory v2-Endpunkt akzeptiert. In diesem Szenario wird ein Token über den Autorisierungsheader HTTP-Anforderungen hinzugefügt. Tokenabrufe und -verlängerungen werden von der Microsoft Authentication Library (MSAL) verarbeitet.

### <a name="nuget-packages"></a>NuGet-Pakete

In dieser Anleitung werden die folgenden NuGet-Pakete verwendet:

|Bibliothek|BESCHREIBUNG|
|---|---|
|[Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)|Microsoft Authentication Library (MSAL)|


## <a name="set-up-your-project"></a>Einrichten des Projekts

Dieser Abschnitt enthält detaillierte Anweisungen zum Integrieren einer Windows Desktop .NET (XAML)-Anwendung in *Mit Microsoft anmelden*, damit Web-APIs abgefragt werden können, die ein Token anfordern (z.B. die Microsoft Graph-API).

In der von diesem Leitfaden erstellten Anwendung werden eine Schaltfläche, über welche die Graph-API abgefragt werden kann, eine Anmeldeschaltfläche sowie Textfelder mit den Ergebnissen der Aufrufe angezeigt.

> Möchten Sie stattdessen das Visual Studio-Projekt dieses Beispiels herunterladen? [Laden Sie ein Projekt herunter](https://github.com/Azure-Samples/active-directory-dotnet-native-uwp-v2/archive/master.zip), und fahren Sie mit dem Schritt [Anwendungsregistrierung](#register-your-application "Schritt „Anwendungsregistrierung“") fort, um das Codebeispiel vor der Ausführung zu konfigurieren.


### <a name="create-your-application"></a>Erstellen der Anwendung
1. In Visual Studio: **Datei** > **Neu** > **Projekt**<br/>
2. Wählen Sie unter *Vorlagen* die Option **Visual C#** aus
3. Wählen Sie **Leere App (universelles Windows)** aus
4. Benennen Sie sie, und klicken Sie auf „OK“.
5. Wenn Sie dazu aufgefordert werden, wählen Sie eine Version für die *Zielversion* und die *mindestens erforderliche Version* aus, und klicken Sie auf „OK“':<br/><br/>![Mindestens erforderliche Version und Zielversion](media/active-directory-uwp-v2.md/vs-minimum-target.png)

## <a name="add-the-microsoft-authentication-library-msal-to-your-project"></a>Hinzufügen der MSAL (Microsoft Authentication Library) zu Ihrem Projekt
1. In Visual Studio: **Tools** > **NuGet-Paket-Manager** > **Paket-Manager-Konsole**
2. Kopieren Sie folgenden Befehl, und fügen Sie ihn in das Fenster „Paket-Manager-Konsole“ ein:

    ```powershell
    Install-Package Microsoft.Identity.Client -Pre
    ```

> [!NOTE]
> Das obige Paket installiert die [Microsoft Authentication Library (MSAL)](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet). Die MSAL kümmert sich um das Erfassen, Zwischenspeichern und Aktualisieren von Benutzertoken für den Zugriff auf APIs, die von Azure Active Directory v2 geschützt werden.

## <a name="initialize-msal"></a>MSAL initialisieren
Dieser Schritt hilft Ihnen beim Erstellen einer Klasse zum Verarbeiten der Interaktion mit der MSAL-Bibliothek, z.B. zum Verarbeiten von Token.

1. Öffnen Sie die Datei **App.xaml.cs**, und fügen Sie der Klasse den Verweis für die MSAL-Bibliothek hinzu:

    ```csharp
    using Microsoft.Identity.Client;
    ```

2. Fügen Sie die folgenden Zeilen zur Klasse der App hinzu (in Block <code>sealed partial class App : Application</code>):

    ```csharp
    // Below is the clientId of your app registration. 
    // You have to replace the below with the Application Id for your app registration
    private static string ClientId = "your_client_id_here";
    
    public static PublicClientApplication PublicClientApp = new PublicClientApplication(ClientId);
    ```

## <a name="create-your-applications-ui"></a>Erstellen der Benutzeroberfläche Ihrer Anwendung

Die Datei **MainPage.xaml** sollte als Teil Ihrer Projektvorlage automatisch erstellt werden. Öffnen Sie diese Datei, und befolgen Sie anschließend die nachstehenden Anweisungen:

1.  Ersetzen Sie den Knoten **<Grid>** Ihrer Anwendung durch:

    ```xml
    <Grid>
        <StackPanel Background="Azure">
            <StackPanel Orientation="Horizontal" HorizontalAlignment="Right">
                <Button x:Name="CallGraphButton" Content="Call Microsoft Graph API" HorizontalAlignment="Right" Padding="5" Click="CallGraphButton_Click" Margin="5" FontFamily="Segoe Ui"/>
                <Button x:Name="SignOutButton" Content="Sign-Out" HorizontalAlignment="Right" Padding="5" Click="SignOutButton_Click" Margin="5" Visibility="Collapsed" FontFamily="Segoe Ui"/>
            </StackPanel>
            <TextBlock Text="API Call Results" Margin="2,0,0,-5" FontFamily="Segoe Ui" />
            <TextBox x:Name="ResultText" TextWrapping="Wrap" MinHeight="120" Margin="5" FontFamily="Segoe Ui"/>
            <TextBlock Text="Token Info" Margin="2,0,0,-5" FontFamily="Segoe Ui" />
            <TextBox x:Name="TokenInfoText" TextWrapping="Wrap" MinHeight="70" Margin="5" FontFamily="Segoe Ui"/>
        </StackPanel>
    </Grid>
    ```
    
## <a name="use-the-microsoft-authentication-library-msal-to-get-a-token-for-the-microsoft-graph-api"></a>Verwenden der Microsoft Authentication Library (MSAL) zum Abrufen eines Tokens für die Microsoft Graph-API

Dieser Abschnitt zeigt, wie Sie über die MSAL ein Token für die Microsoft Graph-API abrufen können.

1.  Fügen Sie in der Datei **MainPage.xaml.cs** den Verweis auf die MSAL-Bibliothek zur Klasse hinzu:

    ```csharp
    using Microsoft.Identity.Client;
    ```
2. Ersetzen Sie den Code Ihrer <code>MainPage</code>-Klasse durch:

    ```csharp
    public sealed partial class MainPage : Page
    {
        // Set the API Endpoint to Graph 'me' endpoint
        string graphAPIEndpoint = "https://graph.microsoft.com/v1.0/me";
    
        // Set the scope for API call to user.read
        string[] scopes = new string[] { "user.read" };
    
        public MainPage()
        {
            this.InitializeComponent();
        }
    
        /// <summary>
        /// Call AcquireTokenAsync - to acquire a token requiring user to sign-in
        /// </summary>
        private async void CallGraphButton_Click(object sender, RoutedEventArgs e)
        {
            AuthenticationResult authResult = null;
            ResultText.Text = string.Empty;
            TokenInfoText.Text = string.Empty;
    
            try
            {
                authResult = await App.PublicClientApp.AcquireTokenSilentAsync(scopes, App.PublicClientApp.Users.FirstOrDefault());
            }
            catch (MsalUiRequiredException ex)
            {
                // A MsalUiRequiredException happened on AcquireTokenSilentAsync. This indicates you need to call AcquireTokenAsync to acquire a token
                System.Diagnostics.Debug.WriteLine($"MsalUiRequiredException: {ex.Message}");
    
                try
                {
                    authResult = await App.PublicClientApp.AcquireTokenAsync(scopes);
                }
                catch (MsalException msalex)
                {
                    ResultText.Text = $"Error Acquiring Token:{System.Environment.NewLine}{msalex}";
                }
            }
            catch (Exception ex)
            {
                ResultText.Text = $"Error Acquiring Token Silently:{System.Environment.NewLine}{ex}";
                return;
            }
    
            if (authResult != null)
            {
                ResultText.Text = await GetHttpContentWithToken(graphAPIEndpoint, authResult.AccessToken);
                DisplayBasicTokenInfo(authResult);
                this.SignOutButton.Visibility = Visibility.Visible;
            }
        }
    }
    ```

### <a name="more-information"></a>Weitere Informationen
#### <a name="get-a-user-token-interactively"></a>Interaktives Abrufen eines Benutzertokens
Das Aufrufen der `AcquireTokenAsync`-Methode führt zu einem Fenster, in dem Benutzer zum Anmelden aufgefordert werden. Bei Anwendungen müssen sich Benutzer interaktiv anmelden, wenn sie zum ersten Mal auf eine geschützte Ressource zugreifen müssen. Die Anmeldung kann auch erforderlich sein, wenn bei dem automatischen Vorgang zum Beziehen eines Tokens ein Fehler auftritt (beispielsweise bei einem abgelaufenen Kennwort des Benutzers).

#### <a name="get-a-user-token-silently"></a>Automatisches Abrufen eines Benutzertokens
Die `AcquireTokenSilentAsync`-Methode dient zum Verwalten des Abrufens und Erneuerns von Token ohne Benutzereingriff. Nachdem `AcquireTokenAsync` zum ersten Mal ausgeführt wurde, ist `AcquireTokenSilentAsync` die übliche Methode zum Abrufen von Token, die für den Zugriff auf geschützte Ressourcen bei nachfolgenden Aufrufen verwendet werden. Der Grund hierfür ist, dass Aufrufe zum Anfordern oder Verlängern von Token automatisch erfolgen.

Für die `AcquireTokenSilentAsync`-Methode tritt schließlich ein Fehler auf. Gründe für den Fehler können sein, dass der Benutzer sich entweder abgemeldet oder sein Kennwort auf einem anderen Gerät geändert hat. Wenn die MSAL feststellt, dass das Problem durch Anfordern einer interaktiven Aktion gelöst werden kann, löst sie eine `MsalUiRequiredException`-Ausnahme aus. Ihre Anwendung kann diese Ausnahme auf zwei Arten handhaben:

* Sie kann sofort `AcquireTokenAsync` aufrufen. Dieser Aufruf führt dazu, dass der Benutzer zum Anmelden aufgefordert wird. Dieses Muster wird normalerweise in Onlineanwendungen verwendet, in denen keine Offlineinhalte für den Benutzer verfügbar sind. Das Beispiel, das in diesem Setup mit Anleitung generiert wird, basiert auf diesem Muster. Sie können dies bei der ersten Ausführung des Beispiels in Aktion sehen. 
    * Da bisher noch kein Benutzer die Anwendung genutzt hat, enthält `PublicClientApp.Users.FirstOrDefault()` einen NULL-Wert, und es wird die Ausnahme `MsalUiRequiredException` ausgelöst. 
    * Der Code im Beispiel behandelt die Ausnahme dann durch das Aufrufen von `AcquireTokenAsync`. Dies führt dazu, dass der Benutzer zum Anmelden aufgefordert wird.

* Stattdessen kann für Benutzer auch ein visueller Hinweis gegeben werden, dass eine interaktive Anmeldung erforderlich ist, damit diese den richtigen Zeitpunkt für die Anmeldung auswählen können. Oder die Anwendung kann später noch einmal versuchen, `AcquireTokenSilentAsync` auszuführen. Dieses Muster wird häufig verwendet, wenn Benutzer andere Anwendungsfunktionen ohne Störungen nutzen können, z.B. wenn Offlineinhalte in der Anwendung verfügbar sind. In diesem Fall können Benutzer entscheiden, wann sie sich anmelden, um entweder auf die geschützte Ressource zuzugreifen oder die veralteten Informationen zu aktualisieren. Alternativ dazu kann die Anwendung auch versuchen, `AcquireTokenSilentAsync` erneut auszuführen, wenn das Netzwerk nach einem kurzzeitigen Ausfall wiederhergestellt wurde.

## <a name="call-the-microsoft-graph-api-using-the-token-you-just-obtained"></a>Aufrufen der Microsoft Graph-API mit dem zuvor bezogenen Token

1. Fügen Sie folgende neue Methode zur Datei **MainPage.xaml.cs** hinzu. Die Methode dient zum Richten einer `GET`-Anforderung an die Graph-API mithilfe eines „Authorize“-Headers:

    ```csharp
    /// <summary>
    /// Perform an HTTP GET request to a URL using an HTTP Authorization header
    /// </summary>
    /// <param name="url">The URL</param>
    /// <param name="token">The token</param>
    /// <returns>String containing the results of the GET operation</returns>
    public async Task<string> GetHttpContentWithToken(string url, string token)
    {
        var httpClient = new System.Net.Http.HttpClient();
        System.Net.Http.HttpResponseMessage response;
        try
        {
            var request = new System.Net.Http.HttpRequestMessage(System.Net.Http.HttpMethod.Get, url);
            // Add the token in Authorization header
            request.Headers.Authorization = new System.Net.Http.Headers.AuthenticationHeaderValue("Bearer", token);
            response = await httpClient.SendAsync(request);
            var content = await response.Content.ReadAsStringAsync();
            return content;
        }
        catch (Exception ex)
        {
            return ex.ToString();
        }
    }
    ```

### <a name="more-information-on-making-a-rest-call-against-a-protected-api"></a>Weitere Informationen zum Richten eines REST-Aufrufs an eine geschützte API

In dieser Beispielanwendung dient die `GetHttpContentWithToken`-Methode zum Richten einer HTTP `GET`-Anforderung an eine geschützte Ressource, die ein Token erfordert, und anschließenden Zurückgeben des Inhalts an den Aufrufer. Diese Methode fügt das abgerufene Token in den *HTTP-Autorisierungsheader* ein. In diesem Beispiel ist die Ressource der Endpunkt *me* der Microsoft Graph-API, der die Profilinformationen des Benutzers anzeigt.
<!--end-collapse-->

## <a name="add-a-method-to-sign-out-the-user"></a>Hinzufügen einer Methode zum Abmelden des Benutzers

1. Fügen Sie folgende Methode zur Datei **MainPage.xaml.cs** hinzu, um den Benutzer abzumelden:

    ```csharp
    /// <summary>
    /// Sign out the current user
    /// </summary>
    private void SignOutButton_Click(object sender, RoutedEventArgs e)
    {
        if (App.PublicClientApp.Users.Any())
        {
            try
            {
                App.PublicClientApp.Remove(App.PublicClientApp.Users.FirstOrDefault());
                this.ResultText.Text = "User has signed-out";
                this.CallGraphButton.Visibility = Visibility.Visible;
                this.SignOutButton.Visibility = Visibility.Collapsed;
            }
            catch (MsalException ex)
            {
                ResultText.Text = $"Error signing-out user: {ex.Message}";
            }
        }
    }
    ```

### <a name="more-info-on-sign-out"></a>Weitere Informationen zum Abmelden

Die Methode `SignOutButton_Click` entfernt den Benutzer aus dem MSAL-Benutzercache. Dadurch wird die MSAL effektiv angewiesen, den aktuellen Benutzer zu vergessen, sodass eine künftige Anforderung zum Abrufen eines Tokens nur erfolgreich ist, wenn diese interaktiv gestaltet wird.
Obwohl die Anwendung in diesem Beispiel einen einzelnen Benutzer unterstützt, werden von der MSAL Szenarien unterstützt, in denen mehrere Konten gleichzeitig angemeldet werden können. Ein Beispiel ist eine E-Mail-Anwendung, in der ein Benutzer mehrere Konten hat.

## <a name="display-basic-token-information"></a>Anzeigen grundlegender Informationen zum Token

1. Fügen Sie folgende Methode zur Datei **MainPage.xaml.cs** hinzu, um grundlegende Informationen zum Token anzuzeigen:

    ```csharp
    /// <summary>
    /// Display basic information contained in the token
    /// </summary>
    private void DisplayBasicTokenInfo(AuthenticationResult authResult)
    {
        TokenInfoText.Text = "";
        if (authResult != null)
        {
            TokenInfoText.Text += $"Name: {authResult.User.Name}" + Environment.NewLine;
            TokenInfoText.Text += $"Username: {authResult.User.DisplayableId}" + Environment.NewLine;
            TokenInfoText.Text += $"Token Expires: {authResult.ExpiresOn.ToLocalTime()}" + Environment.NewLine;
            TokenInfoText.Text += $"Access Token: {authResult.AccessToken}" + Environment.NewLine;
        }
    }
    ```

### <a name="more-information"></a>Weitere Informationen

Über *OpenID Connect* abgerufene ID-Token enthalten auch eine kleine Teilmenge von Informationen zum Benutzer. `DisplayBasicTokenInfo` zeigt grundlegende Informationen im Token: beispielsweise den Anzeigenamen und die ID des Benutzers sowie das Ablaufdatum des Tokens und die Zeichenfolge, die das Zugriffstoken selbst darstellt. Diese Informationen werden Ihnen angezeigt. Sie können mehrmals auf die Schaltfläche **Microsoft Graph-API** klicken, um zu erkennen, dass dasselbe Token für nachfolgende Anforderungen wiederverwendet wurde. Sie können auch feststellen, dass das Ablaufdatum verlängert wurde, wenn die MSAL entscheidet, dass es Zeit ist, das Token zu verlängern.

## <a name="register-your-application"></a>Anwendung registrieren

Nun müssen Sie Ihre Anwendung im *Microsoft-Anwendungsregistrierungsportal* registrieren:
1. Registrieren Sie Ihre Anwendung im [Microsoft-Anwendungsregistrierungsportal](https://apps.dev.microsoft.com/portal/register-app).
2. Geben Sie einen Namen für Ihre Anwendung ein. 
3. Stellen Sie sicher, dass die Option für geführtes Setup deaktiviert ist.
4. Klicken Sie auf **Plattformen hinzufügen**, wählen Sie anschließend **Native Anwendung** aus, und klicken Sie auf „Speichern“
5. Kopieren Sie die GUID in „Anwendungs-ID“, wechseln Sie zurück zu Visual Studio, öffnen Sie **App.xaml.cs**, und ersetzen Sie `your_client_id_here` durch die Anwendungs-ID, die Sie gerade registriert haben:

    ```csharp
    private static string ClientId = "your_application_id_here";
    ```

## <a name="enable-integrated-authentication-on-federated-domains-optional"></a>Aktivieren einer integrierten Authentifizierung für Verbunddomänen (optional)

Das Anwendungsmanifest muss zusätzliche Funktionen aktivieren, um eine integrierte Windows-Authentifizierung bei der Verwendung mit einer Azure Active Directory-Verbunddomäne zu aktivieren:

1. Doppelklicken Sie auf **Package.appxmanifest**
2. Wählen Sie die Registerkarte **Funktionen** aus, und stellen Sie sicher, dass folgende Einstellungen aktiviert sind:

    - Unternehmensauthentifizierung
    - Private Netzwerke (Client und Server)
    - Freigegebene Benutzerzertifikate 

3. Öffnen Sie anschließend **App.xaml.cs**, und fügen Sie die folgende Zeile im App-Konstruktor hinzu:

    ```csharp
    App.PublicClientApp.UseCorporateNetwork = true;
    ```

> [!IMPORTANT]
> Die integrierte Windows-Authentifizierung ist standardmäßig nicht für dieses Beispiel konfiguriert, da für Anwendungen, welche die Funktion *Unternehmensauthentifizierung* oder *Freigegebene Benutzerzertifikate* anfordern, im Windows Store eine höhere Überprüfungsebene erforderlich ist und nicht alle Entwickler die Überprüfung auf der höheren Ebene durchführen möchten. Aktivieren Sie diese Einstellung nur, wenn Sie die integrierte Windows-Authentifizierung mit einer Azure Active Directory-Verbunddomäne benötigen.


## <a name="test-your-code"></a>Testen Ihres Codes

Drücken Sie zum Testen Ihrer Anwendung die Taste `F5`, um das Projekt in Visual Studio auszuführen. Das Hauptfenster sollte angezeigt werden:

![Benutzeroberfläche der Anwendung](media/active-directory-uwp-v2.md/testapp-ui.png)

Wenn Sie zum Testen bereit sind, klicken Sie auf *Microsoft Graph-API aufrufen*. Verwenden Sie zum Anmelden ein Microsoft Azure Active Directory-Konto (Organisationskonto) oder ein Microsoft-Konto (live.com, outlook.com). Wenn Sie sich erstmals anmelden, wird ein Fenster eingeblendet, das den Benutzer auffordert, sich anzumelden:

![Anmeldeseite](media/active-directory-uwp-v2.md/sign-in-page.png)

### <a name="consent"></a>Zustimmung
Bei der ersten Anmeldung bei Ihrer Anwendung wird ein Genehmigungsbildschirm ähnlich dem folgenden angezeigt, auf dem Sie explizit Ihre Zustimmung geben müssen:

![Genehmigungsbildschirm](media/active-directory-uwp-v2.md/consentscreen.png)
### <a name="expected-results"></a>Erwartete Ergebnisse
Es sollten Benutzerprofilinformationen angezeigt werden, die von einem Microsoft Graph-API-Aufruf auf dem Bildschirm „API-Aufrufergebnisse“ zurückgegeben werden:

![Ergebnisbildschirm](media/active-directory-uwp-v2.md/uwp-results-screen.PNG)

Es sollten im Feld mit den Informationen zum Token auch grundlegende Informationen zum Token angezeigt werden, das über `AcquireTokenAsync` oder `AcquireTokenSilentAsync` abgerufen wurde:

|Eigenschaft  |Format  |BESCHREIBUNG |
|---------|---------|---------|
|**Name** |Vollständiger Name des Benutzers |Vor- und Nachname des Benutzers|
|**Benutzername** |<span>user@domain.com</span> |Der zur Identifizierung des Benutzers verwendete Benutzername|
|**Ablaufdatum des Tokens** |Datetime |Uhrzeit, zu der das Token abläuft. MSAL verlängert die Ablauffrist, indem das Token bei Bedarf verlängert wird.|
|**Zugriffstoken** |Zeichenfolge |Die Tokenzeichenfolge, die bei HTTP-Anforderungen gesendet wird, die einen *Autorisierungsheader* erfordern|

#### <a name="see-what-is-in-the-access-token-optional"></a>Überprüfen Sie, was im Zugriffstoken ist (optional)
Sie können den Wert in „Zugriffstoken“ optional kopieren und in https://jwt.ms einfügen, um ihn zu decodieren und die Liste der Ansprüche anzuzeigen.

### <a name="more-information-about-scopes-and-delegated-permissions"></a>Weitere Informationen zu Bereichen und delegierten Berechtigungen

Die Microsoft Graph-API benötigt den Bereich *user.read*, um das Benutzerprofil zu lesen. Dieser Bereich wird standardmäßig jeder Anwendung automatisch hinzugefügt, die im Anwendungsregistrierungsportal registriert wird. Andere Microsoft Graph-APIs sowie benutzerdefinierte APIs für Ihren Back-End-Server erfordern unter Umständen zusätzliche Bereiche. Die Microsoft Graph-API benötigt den Bereich *Calendars.Read*, um die Kalender des Benutzers aufzuführen.

Um auf die Kalender des Benutzers im Kontext einer Anwendung zugreifen zu können, müssen Sie den Informationen für die Anwendungsregistrierung die delegierte Berechtigung *Calendars.Read* hinzufügen. Fügen Sie dann dem Aufruf von `acquireTokenSilent` den Bereich *Calendars.Read* hinzu. 

> [!NOTE]
> Wenn Sie die Anzahl der Bereiche erhöhen, wird der Benutzer ggf. zu weiteren Genehmigungen aufgefordert.

## <a name="known-issues"></a>Bekannte Probleme

### <a name="issue-1"></a>Problem 1:
Wenn Sie sich in einer Azure Active Directory-Verbunddomäne bei Ihrer Anwendung anmelden, tritt möglicherweise einer der folgenden Fehler auf:
 - In der Anforderung wurde kein gültiges Clientzertifikat gefunden.
 - Im Zertifikatspeicher des Benutzers wurden keine gültigen Zertifikate gefunden.
 - Versuchen Sie erneut, und wählen Sie eine andere Authentifizierungsmethode aus.

**Ursache:** Unternehmens- und Zertifikatsfunktionen sind nicht aktiviert

**Lösung:** Führen Sie die Schritte unter [Integrierte Authentifizierung für Verbunddomänen](#enable-integrated-authentication-on-federated-domains-optional) aus

### <a name="issue-2"></a>Problem 2:
Nach der Aktivierung von [Integrierte Authentifizierung für Verbunddomänen](#enable-integrated-authentication-on-federated-domains-optional) und dem Versuch, sich über Windows Hello auf einem Windows 10-Computer bei einer Umgebung mit konfigurierter mehrstufiger Authentifizierung anzumelden, wird die Liste mit den Zertifikaten angezeigt. Wenn Sie Ihre PIN verwenden möchten, wird das Fenster „PIN“ jedoch nie angezeigt.

**Ursache:** Bekannte Einschränkung des Webauthentifizierungsbrokers in UWP-Anwendungen, die unter Windows 10 Desktop ausgeführt werden (funktioniert unter Windows 10 Mobile)

**Problemumgehung:** Benutzer müssen sich mit anderen Optionen anmelden. Sie müssen stattdessen *Mit Benutzernamen und Kennwort anmelden* und „Kennwort angeben“ auswählen, und anschließend die Telefonauthentifizierung durchlaufen.

