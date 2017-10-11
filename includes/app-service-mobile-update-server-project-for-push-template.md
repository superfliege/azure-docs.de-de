In diesem Abschnitt Aktualisieren Sie Code in Ihrem vorhandenen Mobile Apps-Back-End-Projekt zum Senden einer Pushbenachrichtigung jedes Mal, wenn ein neues Element hinzugefügt wird. Dies beruht die [Vorlage](../articles/notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) Funktion von Azure Notification Hubs, plattformübergreifende Pushbenachrichtigungen zu aktivieren. Verschiedene Clients sind für Pushbenachrichtigungen mithilfe von Vorlagen registriert, und ein einzelner universal Push für alle Clientplattformen abrufen kann.

Wählen Sie eine der folgenden Verfahren, die vom Back-End-Projekttyp entspricht&mdash;entweder [.NET back-End](#dotnet) oder [Node.js-Back-End](#nodejs).

### <a name="dotnet"></a>.NET Back-End-Projekt
1. Klicken Sie in Visual Studio mit der rechten Maustaste des Server-Projekts, und klicken Sie auf **NuGet-Pakete verwalten**. Suchen Sie nach `Microsoft.Azure.NotificationHubs`, und klicken Sie dann auf **installieren**. Hiermit werden die Bibliothek Benachrichtigungshubs zum Senden von Benachrichtigungen von Back-End installiert.
2. Öffnen Sie im Server-Projekt **Controller** > **TodoItemController.cs**, und fügen Sie die folgenden using-Anweisungen:

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;
3. In der **PostTodoItem** -Methode, fügen Sie den folgenden Code nach dem Aufruf von **InsertAsync**:  

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
        .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

        // Sending the message so that all template registrations that contain "messageParam"
        // will receive the notifications. This includes APNS, GCM, WNS, and MPNS template registrations.
        Dictionary<string,string> templateParams = new Dictionary<string,string>();
        templateParams["messageParam"] = item.Text + " was added to the list.";

        try
        {
            // Send the push notification and log the results.
            var result = await hub.SendTemplateNotificationAsync(templateParams);

            // Write the success result to the logs.
            config.Services.GetTraceWriter().Info(result.State.ToString());
        }
        catch (System.Exception ex)
        {
            // Write the failure result to the logs.
            config.Services.GetTraceWriter()
                .Error(ex.Message, null, "Push.SendAsync Error");
        }

    Hiermit wird eine vorlagenbenachrichtigung, die das Element enthält. Text, wenn ein neues Element eingefügt wird.
4. Veröffentlichen Sie das Server-Projekt.

### <a name="nodejs"></a>Node.js-Back-End-Projekt
1. Wenn Sie, nicht bereits getan [das Schnellstart-Back-End-Projekt herunterladen](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart), oder verwenden Sie andernfalls die [online-Editor im Azure-Portal](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).
2. Ersetzen Sie den vorhandenen Code in todoitem.js durch Folgendes:

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');

        var table = azureMobileApps.table();

        table.insert(function (context) {
        // For more information about the Notification Hubs JavaScript SDK,
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');

        // Define the template payload.
        var payload = '{"messageParam": "' + context.item.text + '" }';  

        // Execute the insert.  The insert returns the results as a Promise,
        // Do the push as a post-execute action within the promise flow.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
                if (context.push) {
                    // Send a template notification.
                    context.push.send(null, payload, function (error) {
                        if (error) {
                            logger.error('Error while sending push notification: ', error);
                        } else {
                            logger.info('Push notification sent successfully!');
                        }
                    });
                }
                // Don't forget to return the results from the context.execute()
                return results;
            })
            .catch(function (error) {
                logger.error('Error while running context.execute: ', error);
            });
        });

        module.exports = table;  

    Hiermit wird eine vorlagenbenachrichtigung, die die item.text enthält, wenn ein neues Element eingefügt wird.
3. Wenn Sie die Datei auf dem lokalen Computer bearbeiten, erneut veröffentlichen Sie, das Server-Projekt.
