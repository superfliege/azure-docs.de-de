
Das vorherige Beispiel zeigte einen Standard-Anmeldung, die erfordert, dass der Client wenden Sie sich an den Identitätsanbieter und dem Back-End-Azure-Dienst jedes Mal, wenn die app gestartet wird. Diese Methode ist ineffizient, und Sie können Nutzung bezogene Probleme haben, wenn versucht wird, dass viele Kunden die app gleichzeitig zu starten. Ein besserer Ansatz ist das Zwischenspeichern von des Autorisierungstoken von Azure-Dienst zurückgegebenen versuchen, zuerst zu verwenden, bevor er einen anbieterbasierte anmelden.

> [!NOTE]
> Sie können das von dem Back-End-Azure-Dienst, unabhängig davon, ob Sie die Client- oder vom Dienst verwaltet Authentifizierung verwenden ausgegebene Token Zwischenspeichern. Dieses Lernprogramm verwendet die Authentication-Dienst verwaltet.
>
>

1. Öffnen Sie die Datei ToDoActivity.java, und fügen Sie die folgende Import-Anweisungen hinzu:

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;
2. Fügen Sie die folgenden Elemente auf der `ToDoActivity` Klasse.

        public static final String SHAREDPREFFILE = "temp";    
        public static final String USERIDPREF = "uid";    
        public static final String TOKENPREF = "tkn";    
3. In der Datei ToDoActivity.java fügen Sie die folgende Definition für die `cacheUserToken` Methode.

        private void cacheUserToken(MobileServiceUser user)
        {
            SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
            Editor editor = prefs.edit();
            editor.putString(USERIDPREF, user.getUserId());
            editor.putString(TOKENPREF, user.getAuthenticationToken());
            editor.commit();
        }    

    Diese Methode speichert den Benutzer-ID und das Token in einer Einstellung-Datei, die als privat gekennzeichnet wird. Dies sollte Zugriff auf den Cache zu schützen, damit andere apps auf dem Gerät keinen Zugriff auf das Token haben. Die Einstellung ist für die app Sandbox. Wenn ein Benutzer den Zugriff auf das Gerät erreicht hat, ist es jedoch möglich, dass sie Zugriff auf den Tokencache mithilfe anderer Methoden können.

   > [!NOTE]
   > Darüber hinaus können Sie das Token mit Verschlüsselung, wenn token Zugriff auf Ihre Daten ist streng vertrauliche berücksichtigen, die eine Person möglicherweise Zugriff auf das Gerät schützen. Eine vollständig sichere Lösung ist nicht Gegenstand dieses Lernprogramm jedoch und hängt von Ihren sicherheitsanforderungen.
   >
   >
4. In der Datei ToDoActivity.java fügen Sie die folgende Definition für die `loadUserTokenCache` Methode.

        private boolean loadUserTokenCache(MobileServiceClient client)
        {
            SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
            String userId = prefs.getString(USERIDPREF, null);
            if (userId == null)
                return false;
            String token = prefs.getString(TOKENPREF, null);
            if (token == null)
                return false;

            MobileServiceUser user = new MobileServiceUser(userId);
            user.setAuthenticationToken(token);
            client.setCurrentUser(user);

            return true;
        }
5. In der *ToDoActivity.java* Datei, ersetzen Sie die `authenticate` Methode durch die folgende Methode, die einen Tokencache verwendet. Ändern Sie den Anmeldeanbieter, wenn Sie ein anderes Konto als Google verwenden möchten.

        private void authenticate() {
            // We first try to load a token cache if one exists.
            if (loadUserTokenCache(mClient))
            {
                createTable();
            }
            // If we failed to load a token cache, login and create a token cache
            else
            {
                // Login using the Google provider.    
                ListenableFuture<MobileServiceUser> mLogin = mClient.login(MobileServiceAuthenticationProvider.Google);

                Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                    @Override
                    public void onFailure(Throwable exc) {
                        createAndShowDialog("You must log in. Login Required", "Error");
                    }           
                    @Override
                    public void onSuccess(MobileServiceUser user) {
                        createAndShowDialog(String.format(
                                "You are now logged in - %1$2s",
                                user.getUserId()), "Success");
                        cacheUserToken(mClient.getCurrentUser());
                        createTable();    
                    }
                });
            }
        }
6. Erstellen Sie die app und Test-Authentifizierung mit einem gültigen Konto an. Führen sie mindestens zweimal aus. Während der ersten Ausführung sollten Sie erhalten eine Aufforderung zum Anmelden und erstellen den Tokencache ab. Danach versucht zu jedem Durchlauf der Tokencache für die Authentifizierung zu laden. Sie sollten nicht mehr aufgefordert, sich anzumelden.
