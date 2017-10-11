
1. Fügen Sie in der Projektdatei "MainPage.Xaml.cs" die folgenden **mit** Anweisungen:
   
        using System.Linq;        
        using Windows.Security.Credentials;
2. Ersetzen Sie die **AuthenticateAsync** -Methode durch folgenden Code:
   
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
                    // Login with the identity provider.
                    user = await App.MobileService
                        .LoginAsync(provider);
   
                    // Create and store the user credentials.
                    credential = new PasswordCredential(provider.ToString(),
                        user.UserId, user.MobileServiceAuthenticationToken);
                    vault.Add(credential);
   
                    success = true;
                    message = string.Format("You are now logged in - {0}", user.UserId);
                }
                catch (MobileServiceInvalidOperationException)
                {
                    message = "You must log in. Login Required";
                }
            }
   
            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
   
            return success;
        }
   
    In dieser Version von **AuthenticateAsync**, die app in gespeicherte Anmeldeinformationen zu verwenden versucht die **PasswordVault** auf den Dienst zuzugreifen. Reguläre anmelden wird auch ausgeführt, wenn keine gespeicherten Anmeldeinformationen.
   
   > [!NOTE]
   > Ein zwischengespeichertes Token ist abgelaufen und Ablauf des Tokens kann auch nach der Authentifizierung auftreten, wenn die app verwendet wird. Informationen zu bestimmen, ob ein Token abgelaufen ist, finden Sie unter [für abgelaufene Authentifizierungstoken überprüfen](http://aka.ms/jww5vp). Eine Lösung für die Behandlung von im Zusammenhang mit ablaufender Token Authentifizierungsfehler, finden Sie im Beitrag [Caching und Behandeln von abgelaufenen Token in Azure Mobile Services verwaltet SDK](http://blogs.msdn.com/b/carlosfigueira/archive/2014/03/13/caching-and-handling-expired-tokens-in-azure-mobile-services-managed-sdk.aspx). 
   > 
   > 
3. Starten Sie die app zweimal neu.
   
    Beachten Sie, dass beim ersten Start melden Sie sich mit den Anbieter erneut erforderlich ist. Allerdings auf den zweiten Neustart werden die zwischengespeicherten Anmeldeinformationen verwendet und Anmeldung umgangen wird. 

