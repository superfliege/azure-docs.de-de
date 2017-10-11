## <a name="webapi-project"></a>WebAPI-Projekt
1. Öffnen Sie in Visual Studio die **AppBackend** Projekt, das Sie in erstellt die **Benutzer benachrichtigen** Lernprogramm.
2. Ersetzen Sie im Notifications.cs, die gesamte **Benachrichtigungen** Klasse durch den folgenden Code. Achten Sie darauf, dass Sie die Platzhalter durch Ihre Verbindungszeichenfolge (mit vollständigem Zugriff) für die benachrichtigungs-Hub und den Hubnamen zu ersetzen. Sie erhalten diese Werte aus der [klassischen Azure-Portal](http://manage.windowsazure.com). Dieses Modul stellt nun die verschiedenen sichere Benachrichtigungen, die gesendet werden sollen. Die Benachrichtigungen werden in eine vollständige Implementierung in einer Datenbank gespeichert werden; der Einfachheit halber speichern in diesem Fall wird im Arbeitsspeicher.
   
        public class Notification
        {
            public int Id { get; set; }
            public string Payload { get; set; }
            public bool Read { get; set; }
        }

        public class Notifications
        {
            public static Notifications Instance = new Notifications();

            private List<Notification> notifications = new List<Notification>();

            public NotificationHubClient Hub { get; set; }

            private Notifications() {
                Hub = NotificationHubClient.CreateClientFromConnectionString("{conn string with full access}",     "{hub name}");
            }

            public Notification CreateNotification(string payload)
            {
                var notification = new Notification() {
                Id = notifications.Count,
                Payload = payload,
                Read = false
                };

                notifications.Add(notification);

                return notification;
            }

            public Notification ReadNotification(int id)
            {
                return notifications.ElementAt(id);
            }
        }

1. In NotificationsController.cs, ersetzen Sie den Code innerhalb der **NotificationsController** Klassendefinition durch den folgenden Code. Diese Komponente implementiert eine Möglichkeit für das Gerät die Benachrichtigung geschützt abrufen und zeigt außerdem ein Verfahren (für die Zwecke dieses Lernprogramms), um eine sichere Push auf Geräte auszulösen. Beachten Sie, dass beim Senden der benachrichtigungs an den benachrichtigungshub wir nur eine unformatierte Benachrichtigung mit der ID von der Benachrichtigung (und keine tatsächliche Nachricht) senden:
   
       public NotificationsController()
       {
           Notifications.Instance.CreateNotification("This is a secure notification!");
       }
   
       // GET api/notifications/id
       public Notification Get(int id)
       {
           return Notifications.Instance.ReadNotification(id);
       }
   
       public async Task<HttpResponseMessage> Post()
       {
           var secureNotificationInTheBackend = Notifications.Instance.CreateNotification("Secure confirmation.");
           var usernameTag = "username:" + HttpContext.Current.User.Identity.Name;
   
           // windows
           var rawNotificationToBeSent = new Microsoft.Azure.NotificationHubs.WindowsNotification(secureNotificationInTheBackend.Id.ToString(),
                           new Dictionary<string, string> {
                               {"X-WNS-Type", "wns/raw"}
                           });
           await Notifications.Instance.Hub.SendNotificationAsync(rawNotificationToBeSent, usernameTag);
   
           // apns
           await Notifications.Instance.Hub.SendAppleNativeNotificationAsync("{\"aps\": {\"content-available\": 1}, \"secureId\": \"" + secureNotificationInTheBackend.Id.ToString() + "\"}", usernameTag);
   
           // gcm
           await Notifications.Instance.Hub.SendGcmNativeNotificationAsync("{\"data\": {\"secureId\": \"" + secureNotificationInTheBackend.Id.ToString() + "\"}}", usernameTag);

            return Request.CreateResponse(HttpStatusCode.OK);
        }


Beachten Sie, dass die `Post` -Methode sendet jetzt keine Popupbenachrichtigung. Er sendet eine unformatierte Benachrichtigung, die nur die benachrichtigungs-ID und keinen vertraulichem Inhalt enthält. Stellen Sie außerdem sicher, dass den Sendevorgang für die Plattformen Kommentar für die Sie nicht auf Ihren benachrichtigungshub konfigurierte Anmeldeinformationen verfügen, wie sie Fehler führt.

1. Jetzt werden wir diese app eine Azure-Website erneut bereitstellen, um über alle Geräte zugänglich zu machen. Mit der rechten Maustaste auf die **AppBackend** Projekt, und wählen Sie **veröffentlichen**.
2. Azure-Website als Ihre Veröffentlichungsziel auswählen. Melden Sie sich mit Ihrem Azure-Konto und wählen Sie eine vorhandene oder neue Website, und notieren Sie sich die **Ziel-URL** Eigenschaft in der **Verbindung** Registerkarte. Wir bezeichnen diese URL als Ihre *Back-End-Endpunkt* weiter unten in diesem Lernprogramm. Klicken Sie auf **veröffentlichen**.

