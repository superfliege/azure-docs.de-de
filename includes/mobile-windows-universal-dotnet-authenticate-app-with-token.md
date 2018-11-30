---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 11/25/2018
ms.author: crdun
ms.openlocfilehash: b5368123d6d96e6a58a6926539d6ef22ec68c28b
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/27/2018
ms.locfileid: "52440155"
---
1. Fügen Sie in der Projektdatei "MainPage.xaml.cs" die folgenden **using** -Anweisungen hinzu:
   
        using System.Linq;        
        using Windows.Security.Credentials;
2. Ersetzen Sie die Methode **AuthenticateAsync** durch den folgenden Code:
   
        private async System.Threading.Tasks.Task<bool> AuthenticateAsync()
        {
            string message;
            bool success = false;
   
            // This sample uses the Facebook provider.
            var provider = MobileServiceAuthenticationProvider.Facebook;
   
            // Use the PasswordVault to securely store and access credentials.
            PasswordVault vault = new PasswordVault();
            PasswordCredential credential = null;
   
            try
            {
                // Try to get an existing credential from the vault.
                credential = vault.FindAllByResource(provider.ToString()).FirstOrDefault();
            }
            catch (Exception)
            {
                // When there is no matching resource an error occurs, which we ignore.
            }
   
            if (credential != null)
            {
                // Create a user from the stored credentials.
                user = new MobileServiceUser(credential.UserName);
                credential.RetrievePassword();
                user.MobileServiceAuthenticationToken = credential.Password;
   
                // Set the user from the stored credentials.
                App.MobileService.CurrentUser = user;
   
                // Consider adding a check to determine if the token is 
                // expired, as shown in this post: http://aka.ms/jww5vp.
   
                success = true;
                message = string.Format("Cached credentials for user - {0}", user.UserId);
            }
            else
            {
                try
                {
                    // Sign in with the identity provider.
                    user = await App.MobileService
                        .LoginAsync(provider, "{url_scheme_of_your_app}");
   
                    // Create and store the user credentials.
                    credential = new PasswordCredential(provider.ToString(),
                        user.UserId, user.MobileServiceAuthenticationToken);
                    vault.Add(credential);
   
                    success = true;
                    message = string.Format("You are now signed in - {0}", user.UserId);
                }
                catch (MobileServiceInvalidOperationException)
                {
                    message = "You must sign in. Sign-In Required";
                }
            }
   
            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
   
            return success;
        }
   
    In dieser Version von **AuthenticateAsync** versucht die App, auf den Dienst über Anmeldeinformationen zuzugreifen, die in **PasswordVault** gespeichert sind. Dies erfolgt auch dann, wenn keine gespeicherten Anmeldeinformationen vorhanden sind.
   
   > [!NOTE]
   > Zwischengespeicherte Token können auch nach der Authentifizierung ablaufen, wenn die App verwendet wird. Weitere Informationen dazu, wie Sie ermitteln, ob ein Token abgelaufen ist, finden Sie unter [Check for expired Azure Mobile Services authentication tokens](https://aka.ms/jww5vp)(in englischer Sprache). Eine Lösung zur Behandlung von Autorisierungsfehlern in Zusammenhang mit abgelaufenen Token finden Sie im Beitrag [Caching and handling expired tokens in Azure Mobile Services managed SDK](https://blogs.msdn.com/b/carlosfigueira/archive/2014/03/13/caching-and-handling-expired-tokens-in-azure-mobile-services-managed-sdk.aspx)(Zwischenspeichern und Behandeln von abgelaufenen Token mit dem Azure Mobile Services-SDK für verwalteten Code, in englischer Sprache). 
   > 
   > 
3. Starten Sie die App zweimal neu.
   
    Beachten Sie, dass beim ersten Start die Anmeldung beim Anbieter wieder erforderlich ist. Beim zweiten Neustart jedoch werden die zwischengespeicherten Anmeldeinformationen verwendet, und die Anmeldung wird umgangen. 

