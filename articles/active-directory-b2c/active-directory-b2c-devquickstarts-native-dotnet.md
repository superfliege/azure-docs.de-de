---
title: Azure Active Directory B2C | Microsoft Docs
description: "Wie eine Windows-Desktopanwendung erstellen, die Anmeldeseite, registrieren Sie sich, enthält und die Verwaltung von Profilen mithilfe von Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 9da14362-8216-4485-960e-af17cd5ba3bd
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.openlocfilehash: 8e2b5c704230ee2ba1395dc76a1551aaa8e7af7f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-ad-b2c-build-a-windows-desktop-app"></a>Azure AD B2C: Erstellen einer Windows-desktop-app
Mithilfe von Azure Active Directory (Azure AD) B2C können Sie leistungsstarke Self-service-Identity-Management-Funktionen zu Ihrer desktop-app in wenigen kurzen Schritten hinzufügen. In diesem Artikel wird gezeigt, wie Sie eine .NET Windows Presentation Foundation (WPF) "Aufgabenliste"-app zu erstellen, die Benutzer, die Registrierung, Anmeldung und profilverwaltung enthält. Die app wird Unterstützung für die Anmeldung und melden Sie sich mit einem Benutzernamen oder die e-Mail-enthalten. Unterstützung für die Anmeldung und melden Sie sich dazu gehören auch mit sozialen wie Facebook und Google-Konten.

## <a name="get-an-azure-ad-b2c-directory"></a>Abrufen einer Azure AD B2C-Verzeichnis
Bevor Sie Azure AD B2C verwenden können, müssen Sie ein Verzeichnis erstellt oder Mandanten.  Ein Verzeichnis ist ein Container für alle Ihre Benutzer, apps und Gruppen. Wenn Sie noch, besitzen keine [erstellen Sie ein B2C-Verzeichnis](active-directory-b2c-get-started.md) in dieser Anleitung fortfahren.

## <a name="create-an-application"></a>Eine Anwendung erstellen
Als Nächstes müssen Sie eine app in Ihrem B2C-Verzeichnis zu erstellen. Dies bietet Azure AD-Informationen, die zur sicheren Kommunikation mit Ihrer app benötigt. Befolgen Sie zum Erstellen einer app [diese Anweisungen](active-directory-b2c-app-registration.md).  Achten Sie darauf,:

* Einschließen einer **native Client** in der Anwendung.
* Kopieren der **Umleitungs-URI** `urn:ietf:wg:oauth:2.0:oob`. Es ist die Standard-URL für dieses Codebeispiel.
* Kopieren der **Anwendungs-ID** , Ihre app zugewiesen ist. Sie benötigen ihn später erneut.

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Erstellen Sie Ihre Richtlinien
In Azure AD B2C, wird jede benutzererfahrung von definiert eine [Richtlinie](active-directory-b2c-reference-policies.md). In diesem Codebeispiel enthält drei Identität Erfahrungen: Registrieren Sie sich anmelden und Profil bearbeiten. Sie müssen zum Erstellen einer Richtlinie für jeden Typ, wie in beschrieben die [Richtlinienartikel](active-directory-b2c-reference-policies.md#create-a-sign-up-policy). Wenn Sie die drei Richtlinien erstellen, achten Sie darauf, dass Sie zu:

* Wählen Sie entweder **Anmeldung von Benutzer-ID** oder **E-Mail Anmeldung** im Blatt für die Identity-Anbieter.
* Wählen Sie **Anzeigenamen** und andere Registrierung Attribute in der Registrierung Richtlinie.
* Wählen Sie **Anzeigenamen** und **Objekt-ID** Ansprüche als Anwendung Ansprüche für jede Richtlinie. Sie können andere Ansprüche auch auswählen.
* Kopieren der **Namen** jeder Richtlinie, nach der Erstellung. Sie sollten das Präfix aufweisen `b2c_1_`.  Sie benötigen diesen Namen später.

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nachdem Sie die drei Richtlinien erfolgreich erstellt haben, können Sie beim Erstellen Ihrer app bereit.

## <a name="download-the-code"></a>Laden Sie den code
Der Code für dieses Lernprogramm [bleibt auf GitHub](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet). Zum Erstellen des Beispiels, wie Sie fortfahren, können Sie [ein Gerüst eines Projekt als ZIP-Datei herunterladen](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/skeleton.zip). Sie können auch das Skelett Klonen:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git
```

Abgeschlossene Anwendung ist auch [als ZIP-Datei verfügbar](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip) oder auf die `complete` Verzweigung des gleichen Repositorys.

Öffnen Sie nach dem Herunterladen des Beispielcodes der Visual Studio sln-Datei, um zu beginnen. Die `TaskClient` Projekt ist der WPF-Desktopanwendung, die Interaktion des Benutzers mit. Für die Zwecke dieses Lernprogramms können sie eine Back-End-Vorgang Web-API aufruft, gehostet in Azure, in der Aufgabenliste für jeden Benutzer gespeichert.  Sie müssen nicht die Web-API erstellen, wir haben bereits für Sie ausführen.

Um zu erfahren, wie eine Web-API sicher Anforderungen authentifiziert mit Azure AD B2C, sehen Sie sich die [Web-API Einstieg Artikel](active-directory-b2c-devquickstarts-api-dotnet.md).

## <a name="execute-policies"></a>Ausführen von Richtlinien
Die app kommuniziert mit Azure AD B2C Authentifizierungsnachrichten senden, die die Richtlinie angeben, die sie als Teil der HTTP-Anforderung ausgeführt werden soll. Für desktop .NET-Anwendungen können Sie die Preview-Version Microsoft Authentication Library (MSAL) verwenden, um OAuth 2.0-Authentifizierung-Nachrichten senden, führen Sie die Richtlinien und Abrufen von Token, die Web-APIs aufrufen.

### <a name="install-msal"></a>Installieren von MSAL
MSAL zum Hinzufügen der `TaskClient` Projekt mithilfe der Visual Studio-Paket-Manager-Konsole.

```
PM> Install-Package Microsoft.Identity.Client -IncludePrerelease
```

### <a name="enter-your-b2c-details"></a>Geben Sie die Details der B2C
Öffnen Sie die Datei `Globals.cs` und Ersetzen Sie jeden der Eigenschaftswerte mit Ihren eigenen. Diese Klasse wird verwendet, in der gesamten `TaskClient` zum Verweisen auf häufig verwendete Werte.

```C#
public static class Globals
{
    ...

    // TODO: Replace these five default with your own configuration values
    public static string tenant = "fabrikamb2c.onmicrosoft.com";
    public static string clientId = "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6";
    public static string signInPolicy = "b2c_1_sign_in";
    public static string signUpPolicy = "b2c_1_sign_up";
    public static string editProfilePolicy = "b2c_1_edit_profile";

    ...
}
```

[!INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

### <a name="create-the-publicclientapplication"></a>Erstellen Sie die PublicClientApplication
Die primäre Klasse der MSAL ist `PublicClientApplication`. Diese Klasse stellt die Anwendung im Azure AD B2C-System. Beim Erstellen der app initialisiert einer Instanz von `PublicClientApplication` in `MainWindow.xaml.cs`. Dies kann während des gesamten Fensters verwendet werden.

```C#
protected async override void OnInitialized(EventArgs e)
{
    base.OnInitialized(e);

    pca = new PublicClientApplication(Globals.clientId)
    {
        // MSAL implements an in-memory cache by default.  Since we want tokens to persist when the user closes the app,
        // we've extended the MSAL TokenCache and created a simple FileCache in this app.
        UserTokenCache = new FileCache(),
    };

    ...
```

### <a name="initiate-a-sign-up-flow"></a>Initiieren Sie einen Anmeldung Fluss
Wenn ein Benutzer auf Anzeichen von opts, möchten Sie einen Anmeldung Datenfluss zu initiieren, der der Anmelde Richtlinie verwendet, die Sie erstellt haben. Mithilfe von MSAL rufen Sie einfach `pca.AcquireTokenAsync(...)`. Die Parameter, die Sie, um übergeben `AcquireTokenAsync(...)` zu bestimmen, welcher Token erhalten Sie, die Richtlinie verwendet wird, in die Authentifizierungsanforderung und vieles mehr.

```C#
private async void SignUp(object sender, RoutedEventArgs e)
{
    AuthenticationResult result = null;
    try
    {
        // Use the app's clientId here as the scope parameter, indicating that
        // you want a token to the your app's backend web API (represented by
        // the cloud hosted task API).  Use the UiOptions.ForceLogin flag to
        // indicate to MSAL that it should show a sign-up UI no matter what.
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                Globals.signUpPolicy);

        // Upon success, indicate in the app that the user is signed in.
        SignInButton.Visibility = Visibility.Collapsed;
        SignUpButton.Visibility = Visibility.Collapsed;
        EditProfileButton.Visibility = Visibility.Visible;
        SignOutButton.Visibility = Visibility.Visible;

        // When the request completes successfully, you can get user
        // information from the AuthenticationResult
        UsernameLabel.Content = result.User.Name;

        // After the sign up successfully completes, display the user's To-Do List
        GetTodoList();
    }

    // Handle any exeptions that occurred during execution of the policy.
    catch (MsalException ex)
    {
        if (ex.ErrorCode != "authentication_canceled")
        {
            // An unexpected error occurred.
            string message = ex.Message;
            if (ex.InnerException != null)
            {
                message += "Inner Exception : " + ex.InnerException.Message;
            }

            MessageBox.Show(message);
        }

        return;
    }
}
```

### <a name="initiate-a-sign-in-flow"></a>Initiieren einer anmeldedatenflusses
Sie können eine anmeldedatenflusses auf die gleiche Weise initiieren, dass Sie einen Registrierung Fluss initiieren. Wenn ein Benutzer anmeldet, stellen Sie die gleichen Aufruf MSAL, diesmal mit Ihrer Anmeldung Richtlinie:

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
    AuthenticationResult result = null;
    try
    {
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                    string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                    Globals.signInPolicy);
        ...
```

### <a name="initiate-an-edit-profile-flow"></a>Initiieren Sie einen Profil bearbeiten-Fluss
Erneut, können Sie eine Richtlinie bearbeiten-Profil auf dieselbe Weise ausführen:

```C#
private async void EditProfile(object sender, RoutedEventArgs e)
{
    AuthenticationResult result = null;
    try
    {
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                    string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                    Globals.editProfilePolicy);
```

In all diesen Fällen gibt MSAL entweder ein Token in `AuthenticationResult` oder löst eine Ausnahme aus. Jedes Mal, rufen Sie ein Token aus MSAL, können Sie die `AuthenticationResult.User` Objekt, das die Benutzerdaten in der app, wie z. B. die Benutzeroberfläche zu aktualisieren. Das Token für die Verwendung in anderen Teilen der Anwendung werden auch von ADAL zwischengespeichert.

### <a name="check-for-tokens-on-app-start"></a>Für app-Start-Token überprüfen
Sie können auch MSAL verwenden, zum Nachverfolgen des Status des Benutzers anmelden.  In dieser app möchten wir den Benutzer um angemeldet zu bleiben, sogar nachdem sie die app zu schließen und erneut zu öffnen.  Wieder in die `OnInitialized` überschreiben, verwenden Sie die MSAL `AcquireTokenSilent` -Methode überprüft für zwischengespeicherte Token:

```C#
AuthenticationResult result = null;
try
{
    // If the user has has a token cached with any policy, we'll display them as signed-in.
    TokenCacheItem tci = pca.UserTokenCache.ReadItems(Globals.clientId).Where(i => i.Scope.Contains(Globals.clientId) && !string.IsNullOrEmpty(i.Policy)).FirstOrDefault();
    string existingPolicy = tci == null ? null : tci.Policy;
    result = await pca.AcquireTokenSilentAsync(new string[] { Globals.clientId }, string.Empty, Globals.authority, existingPolicy, false);

    SignInButton.Visibility = Visibility.Collapsed;
    SignUpButton.Visibility = Visibility.Collapsed;
    EditProfileButton.Visibility = Visibility.Visible;
    SignOutButton.Visibility = Visibility.Visible;
    UsernameLabel.Content = result.User.Name;
    GetTodoList();
}
catch (MsalException ex)
{
    if (ex.ErrorCode == "failed_to_acquire_token_silently")
    {
        // There are no tokens in the cache.  Proceed without calling the To Do list service.
    }
    else
    {
        // An unexpected error occurred.
        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }
    return;
}
```

## <a name="call-the-task-api"></a>Rufen Sie die API-Aufgabe
Sie haben jetzt MSAL zum Ausführen von Richtlinien und Abrufen von Token verwendet.  Wenn Sie eine diese Token verwenden, den Task-API-aufrufen möchten, können Sie erneut MSALs verwenden `AcquireTokenSilent` Methode zu suchende zwischengespeicherte Token:

```C#
private async void GetTodoList()
{
    AuthenticationResult result = null;
    try
    {
        // Here we want to check for a cached token, independent of whatever policy was used to acquire it.
        TokenCacheItem tci = pca.UserTokenCache.ReadItems(Globals.clientId).Where(i => i.Scope.Contains(Globals.clientId) && !string.IsNullOrEmpty(i.Policy)).FirstOrDefault();
        string existingPolicy = tci == null ? null : tci.Policy;

        // Use AcquireTokenSilent to indicate that MSAL should throw an exception if a token cannot be acquired
        result = await pca.AcquireTokenSilentAsync(new string[] { Globals.clientId }, string.Empty, Globals.authority, existingPolicy, false);

    }
    // If a token could not be acquired silently, we'll catch the exception and show the user a message.
    catch (MsalException ex)
    {
        // There is no access token in the cache, so prompt the user to sign-in.
        if (ex.ErrorCode == "failed_to_acquire_token_silently")
        {
            MessageBox.Show("Please sign up or sign in first");
            SignInButton.Visibility = Visibility.Visible;
            SignUpButton.Visibility = Visibility.Visible;
            EditProfileButton.Visibility = Visibility.Collapsed;
            SignOutButton.Visibility = Visibility.Collapsed;
            UsernameLabel.Content = string.Empty;
        }
        else
        {
            // An unexpected error occurred.
            string message = ex.Message;
            if (ex.InnerException != null)
            {
                message += "Inner Exception : " + ex.InnerException.Message;
            }
            MessageBox.Show(message);
        }

        return;
    }
    ...
```

Wenn der Aufruf von `AcquireTokenSilentAsync(...)` erfolgreich ausgeführt wird und ein Token im Cache gefunden wird, können Sie das Token zum Hinzufügen der `Authorization` -Header der HTTP-Anforderung. Die Task-Web-API wird dieser Header verwenden, um die Anforderung zum Lesen der Aufgabenliste für den Benutzer zu authentifizieren:

```C#
    ...
    // Once the token has been returned by MSAL, add it to the http authorization header, before making the call to access the To Do list service.
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);

    // Call the To Do list service.
    HttpResponseMessage response = await httpClient.GetAsync(Globals.taskServiceUrl + "/api/tasks");
    ...
```

## <a name="sign-the-user-out"></a>Den Benutzer abmelden
Schließlich können Sie MSAL um der Sitzung eines Benutzers mit der app zu beenden, wenn der Benutzer auswählt **Abmelden**.  Wenn Sie MSAL verwenden zu können, geschieht dies durch die durch das Löschen aller der Token aus dem Tokencache:

```C#
private void SignOut(object sender, RoutedEventArgs e)
{
    // Clear any remnants of the user's session.
    pca.UserTokenCache.Clear(Globals.clientId);

    // This is a helper method that clears browser cookies in the browser control that MSAL uses, it is not part of MSAL.
    ClearCookies();

    // Update the UI to show the user as signed out.
    TaskList.ItemsSource = string.Empty;
    SignInButton.Visibility = Visibility.Visible;
    SignUpButton.Visibility = Visibility.Visible;
    EditProfileButton.Visibility = Visibility.Collapsed;
    SignOutButton.Visibility = Visibility.Collapsed;
    return;
}
```

## <a name="run-the-sample-app"></a>Führen Sie die Beispielapp
Schließlich erstellen Sie, und führen Sie das Beispiel.  Registrieren Sie sich für die app mithilfe eines e-Mail-Adresse oder den Benutzer-namens an. Melden Sie sich ab und wieder mit demselben Benutzerkonto anmelden. Profil des Benutzers zu bearbeiten. Melden Sie sich ab, und melden Sie sich durch die Verwendung eines anderen Benutzers.

## <a name="add-social-idps"></a>Soziale IDPs hinzufügen
Die app aktuell unterstützt, nur Benutzer, die Registrierung und melden Sie sich, mit denen **lokale Konten**. Hierbei handelt es sich um Konten in Ihrem B2C-Verzeichnis gespeichert, die einen Benutzernamen und ein Kennwort verwenden. Mithilfe von Azure AD B2C können Sie die Unterstützung für andere Identitätsanbieter (IDPs) hinzufügen, ohne eine Änderung des Codes.

Um Ihre app soziale IDPs hinzugefügt haben, befolgen Sie zunächst die detaillierten Anweisungen in den folgenden Artikeln. Für jede IDP, die Sie unterstützen möchten, müssen Sie eine Anwendung in diesem System registrieren und erhalten eine Client-ID

* [Einrichten von Facebook als ein IDP](active-directory-b2c-setup-fb-app.md)
* [Richten Sie Google als ein IDP](active-directory-b2c-setup-goog-app.md)
* [Einrichten von Amazon als ein IDP](active-directory-b2c-setup-amzn-app.md)
* [Richten Sie LinkedIn als ein IDP](active-directory-b2c-setup-li-app.md)

Nachdem Sie die Identitätsanbieter aus Ihrem B2C-Verzeichnis hinzufügen, müssen Sie jede Ihrer drei Richtlinien, um die neue IDPs einzuschließen bearbeiten, wie in beschrieben die [Richtlinienartikel](active-directory-b2c-reference-policies.md). Nachdem Sie Ihre Richtlinien gespeichert haben, führen Sie die app erneut aus. Daraufhin sollte die neue IDPs als Anmeldung hinzugefügt und Anmeldeoptionen aller Ihrer Identität auftritt.

Sie können mit Ihren Richtlinien experimentieren und beobachten die Auswirkungen auf die Beispiel-app. Fügen Sie hinzu oder entfernen Sie IDPs, bearbeiten Sie die Anwendung Ansprüche oder ändern Sie Registrierung Attribute. Experimentieren Sie, bis Sie sehen, wie Richtlinien, authentifizierungsanforderungen und MSAL zu verbinden.

Referenz zu den vollständigen Beispiel [dient als ZIP-Datei](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip). Sie können auch von GitHub Klonen:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git```
