---
title: "Erste Schritte mit Azure Notification Hubs für Windows Phone-Apps | Microsoft-Dokumentation"
description: "In diesem Lernprogramm erfahren Sie, wie Sie mit Azure Notification Hubs Pushbenachrichtigungen an eine Windows Phone 8- oder Windows Phone 8.1 Silverlight-Anwendung senden können."
services: notification-hubs
documentationcenter: windows
keywords: Pushbenachrichtigung, Pushbenachrichtigung, Windows Phone Push
author: jwhitedev
manager: kpiteira
editor: 
ms.assetid: d872d8dc-4658-4d65-9e71-fa8e34fae96e
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 12/22/2017
ms.author: jawh
ms.openlocfilehash: 7d44d0a0f8683ad6ad55136ad17879e98e26498b
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/02/2018
---
# <a name="get-started-with-azure-notification-hubs-for-windows-phone-apps"></a>Erste Schritte mit Azure Notification Hubs für Windows Phone-Apps
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Übersicht
> [!NOTE]
> Sie benötigen ein aktives Azure-Konto, um dieses Lernprogramm abzuschließen. Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-phone-get-started%2F).
> 
> 

In diesem Lernprogramm wird gezeigt, wie Sie mit Azure Notification Hubs Pushbenachrichtigungen an eine Windows Phone 8- oder Windows Phone 8.1 Silverlight-Anwendung senden können. Informationen zu Windows Phone 8.1 (nicht Silverlight) finden Sie bei der [Windows Universal](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) -Version.
In diesem Lernprogramm erstellen Sie eine leere Windows Phone 8-App, die Pushbenachrichtigungen mithilfe des Microsoft-Pushbenachrichtigungsdienstes (Microsoft Push Notification Service, MPNS) erhält. Sobald Sie dieses Lernprogramm abgeschlossen haben, können Sie über Ihren Notification Hub Pushbenachrichtigungen an alle Geräte senden, die Ihre App ausführen.

> [!NOTE]
> Im Windows Phone-SDK von Notification Hubs wird die Verwendung des Windows-Pushbenachrichtigungsdienstes (Windows Push Notification Service, WNS) mit Windows Phone 8.1 Silverlight-Apps nicht unterstützt. Um WNS (anstelle von MPNS) mit Windows Phone 8.1-Silverlight-Apps zu verwenden, führen Sie das Tutorial für [Notification Hubs – Windows Phone Silverlight]aus, in dem REST-APIs verwendet werden.
> 
> 

Das Lernprogramm zeigt ein einfaches Übertragungsszenario mithilfe von Notification Hubs.

## <a name="prerequisites"></a>Voraussetzungen
Für dieses Lernprogramm ist Folgendes erforderlich:

* [Visual Studio 2012 Express für Windows Phone]oder eine höhere Version

Das Abschließen dieses Lernprogramms ist eine Voraussetzung für alle anderen Notification Hubs-Lernprogramme für Windows Phone 8-Apps.

## <a name="create-your-notification-hub"></a>Erstellen Ihres Notification Hub
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li><p>Klicken Sie unter <b>Notification Services</b> auf <b>Windows Phone (MPNS)</b>, und aktivieren Sie dann das Kontrollkästchen <b>Nicht authentifizierte Pushbenachrichtigungen zulassen</b>.</p>
</li>
</ol>

&emsp;&emsp;![Azure-Portal: Nicht authentifizierte Pushbenachrichtigungen zulassen](./media/notification-hubs-windows-phone-get-started/azure-portal-unauth.png)

Der Hub ist jetzt erstellt und so konfiguriert, dass nicht authentifizierte Benachrichtigungen für Windows Phone gesendet werden.

> [!NOTE]
> Dieses Lernprogramm verwendet MPNS im nicht authentifizierten Modus. Im nicht authentifizierten MPNS-Modus liegen Einschränkungen für Benachrichtigungen vor, die Sie an jeden Kanal senden können. Notification Hubs unterstützt den [authentifizierten MPNS-Modus](http://msdn.microsoft.com/library/windowsphone/develop/ff941099.aspx) , wodurch Sie Ihr Zertifikat hochladen können.
> 
> 

## <a name="connecting-your-app-to-the-notification-hub"></a>Verbinden Ihrer App mit dem Notification Hub
1. Erstellen Sie in Visual Studio eine neue Windows Phone 8-Anwendung.
   
       ![Visual Studio - New Project - Windows Phone App][13]
   
    In Visual Studio 2013 Update 2 oder höher erstellen Sie stattdessen eine Windows Phone Silverlight-Anwendung.
   
    ![Visual Studio – Neues Projekt – Leere App – Windows Phone Silverlight][11]
2. Klicken Sie in Visual Studio mit der rechten Maustaste auf die Projektmappe, und klicken Sie dann auf **NuGet-Pakete verwalten**.
   
    Daraufhin wird das Dialogfeld **NuGet-Pakete verwalten** angezeigt.
3. Suchen Sie nach `WindowsAzure.Messaging.Managed` , klicken Sie auf **Installieren**, und akzeptieren Sie die Nutzungsbedingungen.
   
    ![Visual Studio – NuGet-Paket-Manager][20]
   
    Damit wird ein Verweis auf die Azure Messaging-Bibliothek für Windows mit dem <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">WindowsAzure.Messaging.Managed NuGet-Paket</a> heruntergeladen, installiert und hinzugefügt.
4. Öffnen Sie die Datei "App.xaml.cs", und fügen Sie die folgenden `using` -Anweisungen hinzu:
   
        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;
5. Fügen Sie den folgenden Code oben in der Methode **Application_Launching** in „App.xaml.cs“ ein:
   
        var channel = HttpNotificationChannel.Find("MyPushChannel");
        if (channel == null)
        {
            channel = new HttpNotificationChannel("MyPushChannel");
            channel.Open();
            channel.BindToShellToast();
        }
   
        channel.ChannelUriUpdated += new EventHandler<NotificationChannelUriEventArgs>(async (o, args) =>
        {
            var hub = new NotificationHub("<hub name>", "<connection string>");
            var result = await hub.RegisterNativeAsync(args.ChannelUri.ToString());
   
            System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() =>
            {
                MessageBox.Show("Registration :" + result.RegistrationId, "Registered", MessageBoxButton.OK);
            });
        });
   
   > [!NOTE]
   > Der Wert **MyPushChannel** ist ein Index, der verwendet wird, um nach einem vorhandenen Kanal in der Sammlung [HttpNotificationChannel](https://msdn.microsoft.com/library/windows/apps/microsoft.phone.notification.httpnotificationchannel.aspx) zu suchen. Wenn kein Kanal vorhanden ist, erstellen Sie einen neuen Eintrag mit diesem Namen.
   > 
   > 
   
    Stellen Sie sicher, dass Sie den Namen des Hubs und die Verbindungszeichenfolge **DefaultListenSharedAccessSignature** einfügen, die Sie im vorigen Abschnitt erhalten haben.
    Dieser Code ruft den Kanal-URI für die App von MPNS ab und registriert dann diesen Kanal-URI bei Ihrem Benachrichtigungshub. Er sorgt außerdem dafür, dass der Kanal-URI in Ihrem Benachrichtigungshub registriert ist, sobald die Anwendung gestartet wird.
   
   > [!NOTE]
   > Dieses Lernprogramm sendet eine Popupbenachrichtigung an das Gerät. Wenn Sie eine Kachelbenachrichtigung senden, müssen Sie stattdessen die **BindToShellTile** -Methode für den Kanal aufrufen. Um Popup- und Kachelbenachrichtigungen zu unterstützen, rufen Sie beide Methoden, **BindToShellTile** und **BindToShellToast**, auf.
   > 
   > 
6. Erweitern Sie im Projektmappen-Explorer **Eigenschaften**, öffnen Sie die Datei `WMAppManifest.xml`, klicken Sie auf die Registerkarte **Funktionen**, und aktivieren Sie die Funktion **ID_CAP_PUSH_NOTIFICATION**.
   
       ![Visual Studio - Windows Phone App Capabilities][14]
   
       This ensures that your app can receive push notifications. Without it, any attempt to send a push notification to the app will fail.
7. Drücken Sie `F5` , um die App auszuführen.
   
    In der App wird eine Registrierungsmeldung angezeigt.
8. Schließen Sie die App.  
   
   > [!NOTE]
   > Zum Empfangen einer Popup-Pushbenachrichtigung muss die Anwendung nicht im Vordergrund ausgeführt werden.
   >

## <a name="next-steps"></a>Nächste Schritte
In diesem einfachen Beispiel haben Sie Pushbenachrichtigungen an all Ihre Windows Phone 8-Geräte übertragen. 

Informationen zum Senden von Benachrichtigungen an bestimmte Benutzer finden Sie im Tutorial [Verwenden von Notification Hubs für Pushbenachrichtigungen an Benutzer] . 

Wenn Sie Ihre Benutzer in Interessengruppen einteilen möchten, finden Sie unter [Verwenden von Benachrichtigungshubs zum Übermitteln von Nachrichten]weitere Informationen. 

Weitere Informationen zur Verwendung von Benachrichtigungshubs finden Sie in der [Benachrichtigungshubs-Anleitung].

<!-- Images. -->
[6]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-console-app.png
[7]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-from-portal.png
[8]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-from-portal2.png
[9]: ./media/notification-hubs-windows-phone-get-started/notification-hub-select-from-portal.png
[10]: ./media/notification-hubs-windows-phone-get-started/notification-hub-select-from-portal2.png
[11]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-wp-silverlight-app.png
[12]: ./media/notification-hubs-windows-phone-get-started/notification-hub-connection-strings.png

[13]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-wp-app.png
[14]: ./media/notification-hubs-windows-phone-get-started/mobile-app-enable-push-wp8.png
[15]: ./media/notification-hubs-windows-phone-get-started/notification-hub-pushauth.png
[20]: ./media/notification-hubs-windows-phone-get-started/notification-hub-windows-universal-app-install-package.png
[213]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-console-app.png





<!-- URLs. -->
[Visual Studio 2012 Express für Windows Phone]: https://go.microsoft.com/fwLink/p/?LinkID=268374
[Benachrichtigungshubs-Anleitung]: http://msdn.microsoft.com/library/jj927170.aspx
[MPNS authenticated mode]: http://msdn.microsoft.com/library/windowsphone/develop/ff941099(v=vs.105).aspx
[Verwenden von Notification Hubs für Pushbenachrichtigungen an Benutzer]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Verwenden von Benachrichtigungshubs zum Übermitteln von Nachrichten]: notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md
[toast catalog]: http://msdn.microsoft.com/library/windowsphone/develop/jj662938(v=vs.105).aspx
[tile catalog]: http://msdn.microsoft.com/library/windowsphone/develop/hh202948(v=vs.105).aspx
[Notification Hubs – Windows Phone Silverlight]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToSLPhoneApp

