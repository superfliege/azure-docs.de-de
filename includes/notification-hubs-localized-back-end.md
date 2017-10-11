



Wenn Sie Vorlage Benachrichtigungen senden, die Sie nur einen Satz von Eigenschaften angeben müssen, in unserem Fall wir den Satz von Eigenschaften, die die lokalisierte Version des die aktuelle Neuigkeiten, für die Instanz enthält, sendet:

    {
        "News_English": "World News in English!",
        "News_French": "World News in French!",
        "News_Mandarin": "World News in Mandarin!"
    }


In diesem Abschnitt wird gezeigt, wie zum Senden von Benachrichtigungen, die mit einer Konsolenanwendung

Der Code enthalten, sendet für Windows Store und iOS-Geräte, seit das Back-End auf allen unterstützten Geräten senden kann.

### <a name="to-send-notifications-using-a-c-console-app"></a>Zum Senden von Benachrichtigungen, die mit einer C#-Konsolenanwendung
Ändern der `SendTemplateNotificationAsync` Methode in die Konsolen-app, die Sie zuvor erstellt, durch den folgenden Code haben. Beachten Sie, wie in diesem Fall besteht keine Notwendigkeit, die mehrere Benachrichtigungen für verschiedene Gebietsschemas und Plattformen.

        private static async void SendTemplateNotificationAsync()
        {
            // Define the notification hub.
            NotificationHubClient hub = 
                NotificationHubClient.CreateClientFromConnectionString(
                    "<connection string with full access>", "<hub name>");

            // Sending the notification as a template notification. All template registrations that contain 
            // "messageParam" or "News_<local selected>" and the proper tags will receive the notifications. 
            // This includes APNS, GCM, WNS, and MPNS template registrations.
            Dictionary<string, string> templateParams = new Dictionary<string, string>();

            // Create an array of breaking news categories.
            var categories = new string[] { "World", "Politics", "Business", "Technology", "Science", "Sports"};
            var locales = new string[] { "English", "French", "Mandarin" };

            foreach (var category in categories)
            {
                templateParams["messageParam"] = "Breaking " + category + " News!";

                // Sending localized News for each tag too...
                foreach( var locale in locales)
                {
                    string key = "News_" + locale;

                    // Your real localized news content would go here.
                    templateParams[key] = "Breaking " + category + " News in " + locale + "!";
                }

                await hub.SendTemplateNotificationAsync(templateParams, category);
            }
        }


Beachten Sie, dass dieser einfache Aufruf den lokalisierten Teil Nachrichten übermittelt, **alle** Ihrer Geräte, die von der Plattform, unabhängig davon, wie dem Benachrichtigungshub erstellt und übermittelt die richtige native Nutzlast für alle Geräte zu einem bestimmten Tag abonniert.

### <a name="sending-the-notification-with-mobile-services"></a>Senden der Benachrichtigung mit Mobile Services
In dem Planer Mobile Service können Sie das folgende Skript aus:

    var azure = require('azure');
    var notificationHubService = azure.createNotificationHubService('<hub name>', '<connection string with full access>');
    var notification = {
            "News_English": "World News in English!",
            "News_French": "World News in French!",
            "News_Mandarin", "World News in Mandarin!"
    }
    notificationHubService.send('World', notification, function(error) {
        if (!error) {
            console.warn("Notification successful");
        }
    });


