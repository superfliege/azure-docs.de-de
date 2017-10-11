---
title: 'Verwenden von Azure AD v2 Windows Desktop erste wurde gestartet: | Microsoft Docs'
description: "Wie Windows Desktop .NET (XAML)-Anwendungen eine API, die erfordern Zugriffstoken von Azure Active Directory-v2-Endpunkt aufrufen können"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 826ba0a00b26993d4f37f0a8ce587d7bb77e7eb4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
## <a name="use-the-microsoft-authentication-library-msal-to-get-a-token-for-the-microsoft-graph-api"></a>Verwenden Sie die Microsoft Authentication Library (MSAL), für die Microsoft Graph-API ein Token abzurufen

Dieser Abschnitt zeigt, wie MSAL, ein Token mit der Graph-API von Microsoft abzurufen.

1.  In `MainWindow.xaml.cs`, den Verweis für MSAL-Bibliothek für die Klasse hinzufügen:

```csharp
using Microsoft.Identity.Client;
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
Ersetzen Sie <code>MainWindow</code> -Klasse Code mit:
</li>
</ol>

```csharp
public partial class MainWindow : Window
{
    //Set the API Endpoint to Graph 'me' endpoint
    string _graphAPIEndpoint = "https://graph.microsoft.com/v1.0/me";

    //Set the scope for API call to user.read
    string[] _scopes = new string[] { "user.read" };


    public MainWindow()
    {
        InitializeComponent();
    }

    /// <summary>
    /// Call AcquireTokenAsync - to acquire a token requiring user to sign-in
    /// </summary>
    private async void CallGraphButton_Click(object sender, RoutedEventArgs e)
    {
        AuthenticationResult authResult = null;

        try
        {
            authResult = await App.PublicClientApp.AcquireTokenSilentAsync(_scopes, App.PublicClientApp.Users.FirstOrDefault());
        }
        catch (MsalUiRequiredException ex)
        {
            // A MsalUiRequiredException happened on AcquireTokenSilentAsync. This indicates you need to call AcquireTokenAsync to acquire a token
            System.Diagnostics.Debug.WriteLine($"MsalUiRequiredException: {ex.Message}");

            try
            {
                authResult = await App.PublicClientApp.AcquireTokenAsync(_scopes);
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
            ResultText.Text = await GetHttpContentWithToken(_graphAPIEndpoint, authResult.AccessToken);
            DisplayBasicTokenInfo(authResult);
            this.SignOutButton.Visibility = Visibility.Visible;
        }
    }
}
```

<!--start-collapse-->
### <a name="more-information"></a>Weitere Informationen
#### <a name="getting-a-user-token-interactive"></a>Ein Benutzertoken abrufen interaktive
Aufrufen der `AcquireTokenAsync` -Methode führen dazu, in einem Fenster, die der Benutzer aufgefordert, melden Sie sich. Anwendungen erfordern in der Regel einen Benutzer interaktiv beim ersten Anmelden sie eine geschützte Ressource zuzugreifen müssen oder bei einem automatischen Vorgang, ein token ein Fehler auftritt (z. B. das Kennwort des Benutzers abgelaufen) abzurufen.

#### <a name="getting-a-user-token-silently"></a>Einen Benutzer abrufen im Hintergrund token
`AcquireTokenSilentAsync`behandelt das token problemloses und Erneuerung ohne Eingreifen des Benutzers. Nach dem `AcquireTokenAsync` zum ersten Mal ausgeführt wird `AcquireTokenSilentAsync` ist die übliche Methode zum Abrufen von Tokens, die Zugriff auf geschützte Ressourcen für nachfolgende Aufrufe - verwendet wird, wie im Hintergrund Aufrufe anzufordern oder Token zu erneuern ausgeführten.
Schließlich `AcquireTokenSilentAsync` fehl – z. B. der Benutzer abgemeldet hat, oder ihr Kennwort auf einem anderen Gerät wurde geändert. Wenn MSAL festgestellt wird, dass das Problem gelöst werden kann, durch das Anfordern einer interaktiven Aktion löst einen `MsalUiRequiredException`. Ihre Anwendung kann diese Ausnahme auf zwei Arten verarbeiten:

1.  Stellen Sie einen Aufruf für `AcquireTokenAsync` sofort, was dazu führt, in der Benutzer zur Anmeldung aufgefordert. Dieses Muster wird in der Regel in onlineanwendungen verwendet, es kein offline Inhalt in der Anwendung für den Benutzer verfügbar ist. Im Beispiel durch die Einführung des Setups generiert verwendet dieses Muster: sehen Sie es in der ersten Aktionszeit Sie das Beispiel führen:, da kein Benutzer die Anwendung verwendete `PublicClientApp.Users.FirstOrDefault()` enthält einen null-Wert und eine `MsalUiRequiredException` Ausnahme wird ausgelöst. Der Code im Beispiel verarbeitet die Ausnahme klicken Sie dann durch den Aufruf `AcquireTokenAsync` führt der Benutzer zur Anmeldung aufgefordert.

2.  Anwendungen können auch einen visuellen Hinweis für dem Benutzer, die eine interaktive Anmeldung erforderlich ist, damit der Benutzer den tatsächlichen Zeitpunkt auswählen kann, sich anzumelden, oder die Anwendung kann wiederholen vornehmen `AcquireTokenSilentAsync` zu einem späteren Zeitpunkt. Dies wird normalerweise verwendet, wenn der Benutzer kann andere Funktionen der Anwendung verwenden, ohne unterbrochen wird – z. B. Offlineinhalt in der Anwendung verfügbar ist. In diesem Fall kann der Benutzer entscheiden, beim Zugriff auf die geschützte Ressource oder die veraltete Informationen zu aktualisieren. Melden Sie sich soll, oder Ihre Anwendung kann bestimmen, um einen Wiederholungsversuch `AcquireTokenSilentAsync` beim Netzwerk wiederhergestellt wird, nach einem vorübergehend nicht verfügbar.
<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-using-the-token-you-just-obtained"></a>Rufen Sie die Microsoft Graph-API mit dem Token, das Sie soeben erhalten haben.

1. Fügen Sie die neue Methode unten auf der `MainWindow.xaml.cs`. Die Methode wird verwendet, damit eine `GET` Anforderung an die Graph-API ein Autorisierungsheader mit:

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
        //Add the token in Authorization header
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
<!--start-collapse-->
### <a name="more-information-on-making-a-rest-call-against-a-protected-api"></a>Weitere Informationen über eine REST-Aufruf für eine geschützte API

In dieser beispielanwendung der `GetHttpContentWithToken` Methode wird verwendet, um eine HTTP stellen `GET` für eine geschützte Ressource, die ein Token benötigt anfordern, und klicken Sie dann den Inhalt an den Aufrufer zurückgibt. Diese Methode fügt das abgerufene Token in der *HTTP-Autorisierungsheader*. Für dieses Beispiel ist die Ressource der Graph-API von Microsoft *me* Endpoint – die Profilinformationen des Benutzers anzeigt.
<!--end-collapse-->

## <a name="add-a-method-to-sign-out-the-user"></a>Fügen Sie eine Methode zum Abmelden des Benutzers

1. Fügen Sie die folgende Methode, die Ihre `MainWindow.xaml.cs` zum Abmelden des Benutzers:

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
<!--start-collapse-->
### <a name="more-info-on-sign-out"></a>Weitere Informationen zum Abmelden

`SignOutButton_Click`Entfernt den Benutzer aus MSAL Benutzercache – Dies wird effektiv MSAL soll. um den aktuellen Benutzer vergessen, damit nur eine späteren Anforderung zum Abrufen eines Tokens erfolgreich ist, wenn sie interaktiv bereitgestellt wird.
Obwohl die Anwendung in diesem Beispiel einen einzelnen Benutzer unterstützt, unterstützt MSAL Szenarien, in denen mehrere Konten angemeldet werden können, zur gleichen Zeit – ein Beispiel für eine e-Mail-Anwendung, in denen mehrere Konten verfügt ein Benutzer ist.
<!--end-collapse-->

## <a name="display-basic-token-information"></a>Grundlegende Informationen anzeigen

1. Fügen Sie die folgende Methode hinzu, um Ihre `MainWindow.xaml.cs` um grundlegende Informationen über das Token anzuzeigen:

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
<!--start-collapse-->
### <a name="more-information"></a>Weitere Informationen

Token abgerufen wird, über *OpenID Connect* enthalten auch eine kleine Teilmenge der Informationen für den Benutzer. `DisplayBasicTokenInfo`Zeigt grundlegende Informationen, die im Token enthaltenen: z. B. Anzeigename des Benutzers und ID, sowie das Ablaufdatum des Tokens und die Zeichenfolge, die Zugriff darstellt token selbst. Diese Informationen werden für die Sie anzeigen angezeigt. Sie erreichen die *Microsoft Graph-API Aufrufen* mehrere Male, und sehen, dass das gleiche Token für nachfolgende Anforderungen wiederverwendet wurde. Sie können auch das Ablaufdatum feststellen, erweitert werden, wenn MSAL Bank erneuern Sie das Token ist.
<!--end-collapse-->

