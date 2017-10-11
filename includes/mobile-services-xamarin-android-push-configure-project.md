
1. In der Projektmappe angezeigt (oder **Projektmappen-Explorer** in Visual Studio), mit der rechten Maustaste die **Komponenten** Ordner, klicken Sie auf **weitere Komponenten abrufen...** , suchen Sie nach der **Google Cloud Messaging-Client** Komponente und dem Projekt hinzugefügt.
2. Öffnen Sie die Projektdatei ToDoActivity.cs, und fügen Sie die folgenden using-Anweisungen der Klasse:
   
        using Gcm.Client;
3. In der **ToDoActivity** Klasse, fügen Sie den folgenden Code hinzu: 
   
        // Create a new instance field for this activity.
        static ToDoActivity instance = new ToDoActivity();
   
        // Return the current activity instance.
        public static ToDoActivity CurrentActivity
        {
            get
            {
                return instance;
            }
        }
        // Return the Mobile Services client.
        public MobileServiceClient CurrentClient
        {
            get
            {
                return client;
            }
        }
   
    Dadurch können Sie auf die Push-Handler-Dienstprozess mobile Clientinstanz zuzugreifen.
4. Fügen Sie den folgenden Code hinzu. die **OnCreate** Methode nach der **MobileServiceClient** wird erstellt:
   
       // Set the current instance of TodoActivity.
       instance = this;
   
       // Make sure the GCM client is set up correctly.
       GcmClient.CheckDevice(this);
       GcmClient.CheckManifest(this);
   
       // Register the app for push notifications.
       GcmClient.Register(this, ToDoBroadcastReceiver.senderIDs);

Ihre **ToDoActivity** ist jetzt für das Hinzufügen von Pushbenachrichtigungen vorbereitet.

