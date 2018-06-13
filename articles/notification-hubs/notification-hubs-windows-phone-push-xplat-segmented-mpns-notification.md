---
title: Senden von Pushbenachrichtigungen an bestimmte Windows-Telefone mit Azure Notification Hubs | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie mit Azure Notification Hubs Pushbenachrichtigungen an bestimmte (nicht alle) Windows Phone 8- oder Windows Phone 8.1-Geräte senden, die beim Back-End der Anwendung registriert sind.
services: notification-hubs
documentationcenter: windows
author: dimazaid
manager: kpiteira
editor: spelluru
ms.assetid: 42726bf5-cc82-438d-9eaa-238da3322d80
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: tutorial
ms.custom: mvc
ms.date: 04/14/2018
ms.author: dimazaid
ms.openlocfilehash: c61a6efaa4a56636400acfe5a212cddad47f4f0c
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
ms.locfileid: "33776993"
---
# <a name="tutorial-push-notifications-to-specific-windows-phone-devices-by-using-azure-notification-hubs"></a>Tutorial: Senden von Pushbenachrichtigungen an bestimmte Windows Phone-Geräte mit Azure Notification Hubs
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

In diesem Tutorial wird gezeigt, wie Sie mit Azure Notification Hubs Pushbenachrichtigungen an bestimmte Windows Phone 8- oder Windows Phone 8.1-Geräte senden können. Informationen zu Windows Phone 8.1 (nicht Silverlight) finden Sie in der [Windows Universal](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)-Version dieses Tutorials.

Sie aktivieren dieses Szenario durch das Einfügen von einem oder mehreren *Tags*, wenn eine Registrierung im Notification Hub erstellt wird. Wenn Benachrichtigungen zu einem Tag gesendet werden, erhalten alle Geräte, die für das Tag registriert wurden, diese Benachrichtigung. Weitere Informationen zu Tags finden Sie unter [Weiterleitung und Tagausdrücke](notification-hubs-tags-segment-push-message.md).

> [!NOTE]
> Im Windows Phone-SDK von Notification Hubs wird die Verwendung des Windows-Pushbenachrichtigungsdienstes (Windows Push Notification Service, WNS) mit Windows Phone 8.1 Silverlight-Apps nicht unterstützt. Um WNS (anstelle von MPNS) mit Windows Phone 8.1-Silverlight-Apps zu verwenden, führen Sie das Tutorial für [Notification Hubs – Windows Phone Silverlight] aus, in dem REST-APIs verwendet werden.

In diesem Tutorial lernen Sie Folgendes: 

> [!div class="checklist"]
> * Hinzufügen der Kategorieauswahl zur mobilen App
> * Registrieren für Benachrichtigungen mit Tags
> * Senden von Benachrichtigungen mit Tags
> * Testen der App

## <a name="prerequisites"></a>Voraussetzungen
Arbeiten Sie das Tutorial [Erste Schritte mit Azure Notification Hubs für Windows Phone-Apps](notification-hubs-windows-mobile-push-notifications-mpns.md) durch. In diesem Tutorial aktualisieren Sie die mobile Anwendung, sodass Sie sich für Kategorien aktueller Nachrichten registrieren können, die Sie interessieren, und nur Pushbenachrichtigungen für diese Kategorien empfangen. 

## <a name="add-category-selection-to-the-mobile-app"></a>Hinzufügen der Kategorieauswahl zur mobilen App
Der erste Schritt besteht daraus, Benutzeroberflächenelemente zur vorhandenen Hauptseite hinzuzufügen, welche dem Benutzer die Auswahl der Kategorien für die Registrierung ermöglichen. Die durch den Benutzer ausgewählten Kategorien werden auf dem Gerät gespeichert. Wenn die App gestartet wird, wird eine Geräteregistrierung in Ihrem Notification Hub mit den ausgewählten Kategorien als Tags erstellt.

1. Öffnen Sie die Datei „MainPage.xaml“, und ersetzen Sie die **Grid**-Elemente mit den Namen `TitlePanel` und `ContentPanel` durch den folgenden Code:
   
        <StackPanel x:Name="TitlePanel" Grid.Row="0" Margin="12,17,0,28">
            <TextBlock Text="Breaking News" Style="{StaticResource PhoneTextNormalStyle}" Margin="12,0"/>
            <TextBlock Text="Categories" Margin="9,-7,0,0" Style="{StaticResource PhoneTextTitle1Style}"/>
        </StackPanel>
   
        <Grid Name="ContentPanel" Grid.Row="1" Margin="12,0,12,0">
            <Grid.RowDefinitions>
                <RowDefinition Height="auto"/>
                <RowDefinition Height="auto" />
                <RowDefinition Height="auto" />
                <RowDefinition Height="auto" />
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition />
                <ColumnDefinition />
            </Grid.ColumnDefinitions>
            <CheckBox Name="WorldCheckBox" Grid.Row="0" Grid.Column="0">World</CheckBox>
            <CheckBox Name="PoliticsCheckBox" Grid.Row="1" Grid.Column="0">Politics</CheckBox>
            <CheckBox Name="BusinessCheckBox" Grid.Row="2" Grid.Column="0">Business</CheckBox>
            <CheckBox Name="TechnologyCheckBox" Grid.Row="0" Grid.Column="1">Technology</CheckBox>
            <CheckBox Name="ScienceCheckBox" Grid.Row="1" Grid.Column="1">Science</CheckBox>
            <CheckBox Name="SportsCheckBox" Grid.Row="2" Grid.Column="1">Sports</CheckBox>
            <Button Name="SubscribeButton" Content="Subscribe" HorizontalAlignment="Center" Grid.Row="3" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click" />
        </Grid>
2. Fügen Sie dem Projekt eine Klasse namens **Notifications** hinzu. Fügen Sie den Modifizierer **public** der Klassendefinition hinzu. Fügen Sie der neuen Codedatei dann die folgenden **using**-Anweisungen hinzu:
   
    ```csharp
    using Microsoft.Phone.Notification;
    using Microsoft.WindowsAzure.Messaging;
    using System.IO.IsolatedStorage;
    using System.Windows;
    ```
1. Kopieren Sie den folgenden Code in die neue **Notifications** -Klasse:
   
    ```csharp
    private NotificationHub hub;

    // Registration task to complete registration in the ChannelUriUpdated event handler
    private TaskCompletionSource<Registration> registrationTask;

    public Notifications(string hubName, string listenConnectionString)
    {
        hub = new NotificationHub(hubName, listenConnectionString);
    }

    public IEnumerable<string> RetrieveCategories()
    {
        var categories = (string)IsolatedStorageSettings.ApplicationSettings["categories"];
        return categories != null ? categories.Split(',') : new string[0];
    }

    public async Task<Registration> StoreCategoriesAndSubscribe(IEnumerable<string> categories)
    {
        var categoriesAsString = string.Join(",", categories);
        var settings = IsolatedStorageSettings.ApplicationSettings;
        if (!settings.Contains("categories"))
        {
            settings.Add("categories", categoriesAsString);
        }
        else
        {
            settings["categories"] = categoriesAsString;
        }
        settings.Save();

        return await SubscribeToCategories();
    }

    public async Task<Registration> SubscribeToCategories()
    {
        registrationTask = new TaskCompletionSource<Registration>();

        var channel = HttpNotificationChannel.Find("MyPushChannel");

        if (channel == null)
        {
            channel = new HttpNotificationChannel("MyPushChannel");
            channel.Open();
            channel.BindToShellToast();
            channel.ChannelUriUpdated += channel_ChannelUriUpdated;

            // This is optional, used to receive notifications while the app is running.
            channel.ShellToastNotificationReceived += channel_ShellToastNotificationReceived;
        }

        // If channel.ChannelUri is not null, complete the registrationTask here.  
        // If it is null, the registrationTask will be completed in the ChannelUriUpdated event handler.
        if (channel.ChannelUri != null)
        {
            await RegisterTemplate(channel.ChannelUri);
        }

        return await registrationTask.Task;
    }

    async void channel_ChannelUriUpdated(object sender, NotificationChannelUriEventArgs e)
    {
        await RegisterTemplate(e.ChannelUri);
    }

    async Task<Registration> RegisterTemplate(Uri channelUri)
    {
        // Using a template registration to support notifications across platforms.
        // Any template notifications that contain messageParam and a corresponding tag expression
        // will be delivered for this registration.

        const string templateBodyMPNS = "<wp:Notification xmlns:wp=\"WPNotification\">" +
                                            "<wp:Toast>" +
                                                "<wp:Text1>$(messageParam)</wp:Text1>" +
                                            "</wp:Toast>" +
                                        "</wp:Notification>";

        // The stored categories tags are passed with the template registration.

        registrationTask.SetResult(await hub.RegisterTemplateAsync(channelUri.ToString(), 
            templateBodyMPNS, "simpleMPNSTemplateExample", this.RetrieveCategories()));

        return await registrationTask.Task;
    }

    // This is optional. It is used to receive notifications while the app is running.
    void channel_ShellToastNotificationReceived(object sender, NotificationEventArgs e)
    {
        StringBuilder message = new StringBuilder();
        string relativeUri = string.Empty;

        message.AppendFormat("Received Toast {0}:\n", DateTime.Now.ToShortTimeString());

        // Parse out the information that was part of the message.
        foreach (string key in e.Collection.Keys)
        {
            message.AppendFormat("{0}: {1}\n", key, e.Collection[key]);

            if (string.Compare(
                key,
                "wp:Param",
                System.Globalization.CultureInfo.InvariantCulture,
                System.Globalization.CompareOptions.IgnoreCase) == 0)
            {
                relativeUri = e.Collection[key];
            }
        }

        // Display a dialog of all the fields in the toast.
        System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() => 
        { 
            MessageBox.Show(message.ToString()); 
        });
    }
    ```
    
    Diese Klasse verwendet den isolierten Speicher, um Nachrichtenkategorien zu speichern, die das Gerät empfangen soll. Sie enthält außerdem Methoden zum Registrieren dieser Kategorien mithilfe einer [Vorlagenbenachrichtigungsregistrierung](notification-hubs-templates-cross-platform-push-messages.md) .
1. Fügen Sie in der Datei „App.xaml.cs“ folgende Eigenschaft zur Klasse **App** hinzu. Ersetzen Sie die Platzhalter `<connection string with listen access>` und  *durch den Namen Ihres Notification Hubs und die Verbindungszeichenfolge für `<hub name>`DefaultListenSharedAccessSignature*, die Sie zuvor abgerufen haben.
   
    ```csharp
    public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");
    ```

   > [!NOTE]
   > Da Anmeldenamen, die mit einer Client-App verteilt werden, nicht sehr sicher sind, sollten Sie nur den Schlüssel für den Abhörzugriff mit Ihrer Client-App verteilen. Der Abhörzugriff ermöglicht der App, sich für Benachrichtigungen zu registrieren, aber es können keine vorhandenen Registrierungen geändert und keine Benachrichtigungen versendet werden. Der Vollzugriffsschlüssel wird in einem gesicherten Back-End-Dienst für das Versenden von Benachrichtigungen und das Ändern vorhandener Benachrichtigungen verwendet.
   > 
   > 
2. Fügen Sie in "MainPage.xaml.cs" folgende Zeile hinzu:
   
    ```csharp
    using Windows.UI.Popups;
    ```
1. Fügen Sie in der Projektdatei "MainPage.xaml.cs" die folgende Methode hinzu:
   
    ```csharp
    private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
    {
        var categories = new HashSet<string>();
        if (WorldCheckBox.IsChecked == true) categories.Add("World");
        if (PoliticsCheckBox.IsChecked == true) categories.Add("Politics");
        if (BusinessCheckBox.IsChecked == true) categories.Add("Business");
        if (TechnologyCheckBox.IsChecked == true) categories.Add("Technology");
        if (ScienceCheckBox.IsChecked == true) categories.Add("Science");
        if (SportsCheckBox.IsChecked == true) categories.Add("Sports");

        var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(categories);

        MessageBox.Show("Subscribed to: " + string.Join(",", categories) + " on registration id : " +
            result.RegistrationId);
    }
    ```
   
    Diese Methode erstellt eine Liste von Kategorien und verwendet die **Notifications** -Klasse zum Speichern der Liste im lokalen Speicher sowie zum Registrieren der entsprechenden Tags bei Ihrem Notification Hub. Wenn Kategorien geändert werden, wird die Registrierung mit neuen Kategorien neu erstellt.

Die App kann jetzt verschiedene Kategorien in einem lokalen Speicher auf dem Gerät speichern und beim Notification Hub registrieren, wenn der Benutzer die Auswahl der Kategorien ändert.

## <a name="register-for-notifications"></a>Registrieren für Benachrichtigungen
Durch diese Schritte findet beim Starten eine Registrierung beim Notification Hub statt, wobei die im lokalen Speicher gespeicherten Kategorien verwendet werden.

> [!NOTE]
> Da sich der durch den Microsoft Push Notification Service (MPNS) zugeteilte Channel-URI jederzeit ändern kann, sollten Sie sich regelmäßig für Benachrichtigungen registrieren, um Benachrichtigungsausfälle zu vermeiden. Dieses Beispiel registriert sich bei jedem Start der App für Benachrichtigungen. Für häufig ausgeführte Anwendungen (öfters als einmal täglich) können Sie die Registrierung wahrscheinlich überspringen, wenn weniger als ein Tag seit der letzten Registrierung vergangen ist, um Bandbreite einzusparen.

1. Öffnen Sie die Datei „App.xaml.cs“, fügen Sie der **Application_Launching**-Methode den Modifizierer **async** hinzu, und ersetzen Sie den vorhandenen Notification Hubs-Registrierungscode, den Sie in [Erste Schritte mit Notification Hubs] erstellt haben, durch den folgenden Code:
   
    ```csharp
    private async void Application_Launching(object sender, LaunchingEventArgs e)
    {
        var result = await notifications.SubscribeToCategories();
    
        if (result != null)
            System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() =>
            {
                MessageBox.Show("Registration Id :" + result.RegistrationId, "Registered", MessageBoxButton.OK);
            });
    }
    ```
   
    Dieser Code stellt sicher, dass die App bei jedem Start die Kategorien aus dem lokalen Speicher abruft und eine Registrierung für diese Kategorien anfordert.
2. Fügen Sie in der Projektdatei "MainPage.xaml.cs" den folgenden Code hinzu, der die Methode **OnNavigatedTo** implementiert:
   
    ```csharp
    protected override void OnNavigatedTo(NavigationEventArgs e)
    {
        var categories = ((App)Application.Current).notifications.RetrieveCategories();
    
        if (categories.Contains("World")) WorldCheckBox.IsChecked = true;
        if (categories.Contains("Politics")) PoliticsCheckBox.IsChecked = true;
        if (categories.Contains("Business")) BusinessCheckBox.IsChecked = true;
        if (categories.Contains("Technology")) TechnologyCheckBox.IsChecked = true;
        if (categories.Contains("Science")) ScienceCheckBox.IsChecked = true;
        if (categories.Contains("Sports")) SportsCheckBox.IsChecked = true;
    }
    ```
   
    Durch diesen Code wird die Hauptseite basierend auf dem Status der zuvor gespeicherten Kategorien aktualisiert.

Die App kann ist jetzt vollständig und kann verschiedene Kategorien in einem lokalen Speicher auf dem Gerät speichern und beim Notification Hub registrieren, wenn der Benutzer die Auswahl der Kategorien ändert. Definieren Sie als Nächstes ein Back-End, das Kategoriebenachrichtigungen an diese App senden kann.

## <a name="send-tagged-notifications"></a>Senden von Benachrichtigungen mit Tags
[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="test-the-app"></a>Testen der App
1. Drücken Sie in Visual Studio die Taste F5, um die App zu starten.
   
    ![Mobile App mit Kategorien][1]
   
    Die Benutzeroberfläche der App bietet verschiedene Umschaltmöglichkeiten, mit denen Sie die Kategorien auswählen können, die Sie abonnieren möchten.
2. Aktivieren Sie eine oder mehrere Kategorien, und klicken Sie dann auf **Subscribe**.
   
    Die App konvertiert die ausgewählten Kategorien in Tags, und fordert eine neue Geräteregistrierung für die ausgewählten Tags vom Notification Hub an. Die registrierten Kategorien werden zurückgegeben und in einem Dialogfeld angezeigt.
   
    ![Abonnierte Nachricht][2]
3. Nach dem Empfang einer Bestätigung, dass das Abonnement Ihrer Kategorien abgeschlossen wurde, führen Sie die Konsolen-App aus, um Benachrichtigungen für die einzelnen Kategorien zu senden. Vergewissern Sie sich, dass Sie nur für die Kategorien eine Benachrichtigung erhalten, die Sie abonniert haben.
   
    ![Benachrichtigungsmeldung][3]

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie gelernt, wie Sie Pushbenachrichtigungen an bestimmte Geräte senden, deren Registrierungen Tags zugeordnet sind. Um zu erfahren, wie Sie Pushbenachrichtigungen an bestimmte Benutzer senden, die möglicherweise mehrere Geräte verwenden, fahren Sie mit dem folgenden Tutorial fort: 

> [!div class="nextstepaction"]
>[Senden von Pushbenachrichtigungen an bestimmte Benutzer](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md)


<!-- Anchors. -->
[Add category selection to the app]: #adding-categories
[Register for notifications]: #register
[Send notifications from your back-end]: #send
[Run the app and generate notifications]: #test-app
[Next Steps]: #next-steps

<!-- Images. -->
[1]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-breakingnews.png
[2]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-registration.png
[3]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-toast.png



<!-- URLs.-->
[Erste Schritte mit Notification Hubs]: /manage/services/notification-hubs/get-started-notification-hubs-wp8/
[Use Notification Hubs to broadcast localized breaking news]: ../breakingnews-localized-wp8.md
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users/
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Phone]: ??

