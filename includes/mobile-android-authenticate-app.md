
1. Öffnen Sie das Projekt in Android Studio.

2. In **Projektexplorer** öffnen Sie in Android Studio die ToDoActivity.java-Datei, und fügen Sie die folgende Import-Anweisungen hinzu:

        import java.util.concurrent.ExecutionException;
        import java.util.concurrent.atomic.AtomicBoolean;

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;

        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceAuthenticationProvider;
        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceUser;

3. Fügen Sie die folgende Methode, die **ToDoActivity** Klasse:

        // You can choose any unique number here to differentiate auth providers from each other. Note this is the same code at login() and onActivityResult().
        public static final int GOOGLE_LOGIN_REQUEST_CODE = 1;

        private void authenticate() {
            // Login using the Google provider.
            mClient.login("Google", "{url_scheme_of_your_app}", GOOGLE_LOGIN_REQUEST_CODE);
        }

        @Override
        protected void onActivityResult(int requestCode, int resultCode, Intent data) {
            // When request completes
            if (resultCode == RESULT_OK) {
                // Check the request code matches the one we send in the login request
                if (requestCode == GOOGLE_LOGIN_REQUEST_CODE) {
                    MobileServiceActivityResult result = mClient.onActivityResult(data);
                    if (result.isLoggedIn()) {
                        // login succeeded
                        createAndShowDialog(String.format("You are now logged in - %1$2s", mClient.getCurrentUser().getUserId()), "Success");
                        createTable();
                    } else {
                        // login failed, check the error message
                        String errorMessage = result.getErrorMessage();
                        createAndShowDialog(errorMessage, "Error");
                    }
                }
            }
        }

    Dieser Code erstellt eine Methode, um die Behandlung des Authentifizierungsprozesses Google. Ein Dialogfeld zeigt die ID des authentifizierten Benutzers. Sie können nur auf eine erfolgreiche Authentifizierung fortgesetzt werden.

    > [!NOTE]
    > Wenn Sie einen Identitätsanbieter als Google verwenden, ändern Sie den Wert, der zum Übergeben der **Anmeldung** Methode, um einen der folgenden Werte: _MicrosoftAccount_, _Facebook_, _Twitter_, oder _Windowsazureactivedirectory_.

4. In der **OnCreate** -Methode, fügen Sie die folgende Codezeile hinzu, nach dem Code, der instanziiert die `MobileServiceClient` Objekt.

        authenticate();

    Dieser Aufruf startet den Authentifizierungsprozess abgebrochen.

5. Im restlichen Code nach dem Verschieben `authenticate();` in der **OnCreate** Methode, um ein neues **CreateTable** Methode:

        private void createTable() {

            // Get the table instance to use.
            mToDoTable = mClient.getTable(ToDoItem.class);

            mTextNewToDo = (EditText) findViewById(R.id.textNewToDo);

            // Create an adapter to bind the items with the view.
            mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);
            ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
            listViewToDo.setAdapter(mAdapter);

            // Load the items from Azure.
            refreshItemsFromTable();
        }

6. Damit werden Umleitung funktioniert wie erwartet, hinzufügen den folgenden Ausschnitt des _RedirectUrlActivity_ auf _AndroidManifest.xml_:

        <activity android:name="com.microsoft.windowsazure.mobileservices.authentication.RedirectUrlActivity">
            <intent-filter>
                <action android:name="android.intent.action.VIEW" />
                <category android:name="android.intent.category.DEFAULT" />
                <category android:name="android.intent.category.BROWSABLE" />
                <data android:scheme="{url_scheme_of_your_app}"
                    android:host="easyauth.callback"/>
            </intent-filter>
        </activity>

7. Hinzufügen von RedirectUriScheme zu _build.gradle_ der Android-Anwendung.

        android {
            buildTypes {
                release {
                    // … …
                    manifestPlaceholders = ['redirectUriScheme': '{url_scheme_of_your_app}://easyauth.callback']
                }
                debug {
                    // … …
                    manifestPlaceholders = ['redirectUriScheme': '{url_scheme_of_your_app}://easyauth.callback']
                }
            }
        }

8. Fügen Sie die Abhängigkeiten in Ihrer build.gradle com.android.support:customtabs:23.0.1 hinzu:

      Abhängigkeiten {/ /... "com.android.support:customtabs:23.0.1" Kompilieren}

9. Aus der **ausführen** Menü klicken Sie auf **app ausführen** um die app, und melden Sie sich mit Ihrem ausgewählten Identitätsanbieter zu starten.

> [!WARNING]
> Das URL-Schema erwähnt wird Groß-/Kleinschreibung beachtet.  Sicherstellen, dass alle Vorkommen von `{url_scheme_of_you_app}` verwenden die gleiche Groß-/Kleinschreibung.

Wenn Sie sich erfolgreich angemeldet haben, sollte sollte die app ohne Fehler ausgeführt, Sie auch können Abfragen des Back-End-Diensts und Updates an Daten vorzunehmen.
