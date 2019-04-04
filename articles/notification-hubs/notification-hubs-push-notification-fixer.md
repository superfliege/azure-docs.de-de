---
title: Azure Notification Hubs – Diagnostizieren verworfener Benachrichtigungen
description: Erfahren Sie, wie Sie allgemeine Probleme mit verworfenen Benachrichtigungen in Azure Notification Hubs diagnostizieren.
services: notification-hubs
documentationcenter: Mobile
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: b5c89a2a-63b8-46d2-bbed-924f5a4cce61
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: NA
ms.devlang: multiple
ms.topic: article
ms.date: 01/04/2019
ms.author: jowargo
ms.openlocfilehash: c0fd7dec31a2c4054c59db3bae52cdb15ba01eed
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57884420"
---
# <a name="azure-notification-hubs---diagnose-dropped-notifications"></a>Azure Notification Hubs – Diagnostizieren verworfener Benachrichtigungen

Eine der häufigsten Fragen von Azure Notification Hubs-Kunden dreht sich darum, dass Benachrichtigungen, die von einer Anwendung gesendet werden, auf Clientgeräten nicht angezeigt werden. Kunden möchten wissen, wo und warum Benachrichtigungen verworfen werden und wie sich das Problem lösen lässt. Dieser Artikel erläutert, warum Benachrichtigungen möglicherweise verworfen oder von Geräten nicht empfangen werden. Sie erfahren, wie Sie das Problem analysieren und die Ursache ermitteln.

Zunächst müssen Sie wissen, wie der Notification Hubs-Dienst Benachrichtigungen an ein Gerät übermittelt.

![Notification Hubs-Architektur][0]

In einem typischen Benachrichtigungsflow wird eine Nachricht vom *Anwendungs-Back-End* an Notification Hubs gesendet. Notification Hubs führt eine bestimmte Form der Verarbeitung in allen Registrierungen aus. Bei der Verarbeitung werden die konfigurierten Tags und Tagausdrücke berücksichtigt, um „Ziele“ zu bestimmen. Bei den Zielen handelt es sich um alle Registrierungen, die die Pushbenachrichtigung erhalten sollen. Diese Registrierungen können sich über einige oder sämtliche unserer unterstützten Plattformen erstrecken: iOS, Google, Windows, Windows Phone, Kindle und Baidu unter Android für China.

Wenn die Ziele bestimmt sind, übermittelt der Notification Hubs-Dienst Benachrichtigungen per Push an den *Pushbenachrichtigungsdienst* für die Geräteplattform. Apple Push Notification Service (APNs) für Apple und Firebase Cloud Messaging (FCM) für Google sind zwei Beispiele für einen solchen Dienst. Notification Hubs übermittelt die Benachrichtigungen aufgeteilt auf mehrere Registrierungsbatches. Notification Hubs authentifiziert sich beim jeweiligen Pushbenachrichtigungsdienst mit den Anmeldeinformationen, die Sie im Azure-Portal unter **Notification Hub konfigurieren** festgelegt haben. Danach leitet der Pushbenachrichtigungsdienst die Benachrichtigungen an die entsprechenden *Clientgeräte* weiter.

Der letzte Abschnitt einer Benachrichtigungsübermittlung erfolgt zwischen dem Pushbenachrichtigungsdienst der Plattform und dem Gerät. Jede der vier Hauptkomponenten im Pushbenachrichtigungsprozess (Client, Anwendungs-Back-End, Notification Hubs und der Pushbenachrichtigungsdienst der Plattform) kann dazu führen, dass Benachrichtigungen verworfen werden. Weitere Informationen zur Architektur von Notification Hubs finden Sie in der Übersicht über [Übersicht über Notification Hubs].

Fehler bei der Übermittlung von Benachrichtigungen können während der anfänglichen Test-/Stagingphase auftreten. In dieser Phase verworfenen Benachrichtigungen können auf ein Konfigurationsproblem hindeuten. Wenn Fehler bei der Übermittlung von Benachrichtigungen in der Produktionsphase auftreten, werden einige oder möglicherweise alle Benachrichtigungen verworfen. In diesem Fall ist die Ursache in einem tiefer liegenden Anwendungs- oder Nachrichtenmusterfehler zu suchen.

Der nächste Abschnitt beschreibt Szenarien, in denen Benachrichtigungen verworfen werden können. Zunächst werden häufiger anzutreffende Szenarien beschrieben, danach seltenere.

## <a name="notification-hubs-misconfiguration"></a>Fehlkonfiguration von Notification Hubs

Der Notification Hubs-Dienst muss sich im Kontext der Anwendung des Entwicklers authentifizieren, um Benachrichtigungen erfolgreich an den jeweiligen Pushbenachrichtigungsdienst senden zu können. Zu diesem Zweck erstellt der Entwickler ein Entwicklerkonto auf der jeweiligen Plattform (Google, Apple, Windows usw.). Danach registriert der Entwickler die Anwendung bei der Plattform, wo sie die notwendigen Anmeldeinformationen erhält.

Sie müssen die Anmeldeinformationen der Plattform zum Azure-Portal hinzufügen. Wenn auf einem Gerät keine Benachrichtigungen ankommen, sollten Sie zuerst sicherstellen, dass in Notification Hubs die richtigen Anmeldeinformationen konfiguriert sind. Die Anmeldeinformationen müssen der Anwendung entsprechen, in einem plattformspezifischen Entwicklerkonto erstellt wurde.

Schrittanleitungen zum Durchführen dieses Prozesses finden Sie unter [Erste Schritte mit Azure Notification Hubs].

Dies sind einige häufige Konfigurationsfehler, nach denen Sie suchen können:

**Allgemein:**

    * Stellen Sie sicher, dass der Name des Notification Hubs (ohne Tippfehler) an folgenden Stellen identisch ist:
        * Bei der Registrierung vom Client aus
        * Beim Senden von Benachrichtigungen aus dem Back-End
        * Beim Konfigurieren der Anmeldeinformationen für den Pushbenachrichtigungsdienst
    * Stellen Sie sicher, dass Sie auf dem Client und im Anwendungs-Back-End die richtigen Konfigurationszeichenfolgen für die Signatur des gemeinsamen Zugriffs verwenden. Allgemein gilt: Sie müssen auf dem Client **DefaultListenSharedAccessSignature** und **DefaultFullSharedAccessSignature** im Anwendungs-Back-End verwenden (erteilt die Berechtigung, Benachrichtigungen an Notification Hubs senden zu können).

**APNs-Konfiguration:**

    You must maintain two different hubs: one hub for production, and another hub for testing. This means that you must upload the certificate that you use in a sandbox environment to a separate hub than the certificate and hub that you are going to use in production. Don't try to upload different types of certificates to the same hub. This might cause notification failures.

    If you inadvertently upload different types of certificates to the same hub, we recommend that you delete the hub and start fresh with a new hub. If for some reason you can't delete the hub, at a minimum, you must delete all the existing registrations from the hub.

**FCM-Konfiguration:**

    1. Stellen Sie sicher, dass der *Serverschlüssel*, den Sie von Firebase erhalten haben, mit dem Serverschlüssel übereinstimmt, den Sie im Azure-Portal registriert haben.

    ![Firebase-Serverschlüssel][3]

    2. Stellen Sie sicher, dass Sie die **Projekt-ID** auf dem Client konfiguriert haben. Sie können den Wert der **Projekt-ID** aus dem Firebase-Dashboard abrufen.

    ![Firebase-Projekt-ID][1]

## <a name="application-issues"></a>Anwendungsprobleme

**Tags und Tagausdrücke:**

    If you use tags or tag expressions to segment your audience, it's possible that when you send the notification, no target is found based on the tags or tag expressions that you specify in your send call.

    Review your registrations to ensure that there are matching tags when you send a notification. Then, verify the notification receipt only from the clients that have those registrations.

    As an example, if all your registrations with Notification Hubs were made by using the tag "Politics" and you send a notification with the tag "Sports," the notification isn't sent to any device. A complex case might involve tag expressions in which you registered by using "Tag A" OR "Tag B," but while sending notifications, you target "Tag A && Tag B." In the self-diagnosis tips section later in the article, we show you how to review your registrations and their tags.

**Vorlagenprobleme:**

    If you use templates, ensure that you follow the guidelines described in [Templates].

**Ungültige Registrierungen:**

    If the notification hub was configured correctly, and if any tags or tag expressions were used correctly, valid targets are found. Notifications should be sent to these targets. The Notification Hubs service then fires off several processing batches in parallel. Each batch sends messages to a set of registrations.

    > [!NOTE]
    > Because processing is performed in parallel, the order in which the notifications are delivered is not guaranteed.

    Notification Hubs is optimized for an "at-most once" message delivery model. We attempt deduplication, so that no notifications are delivered more than once to a device. To ensure this, we check registrations and ensure that only one message is sent per device identifier before the message is sent to the push notification service.

    As each batch is sent to the push notification service, which in turn is accepting and validating the registrations, it's possible that the push notification service will detect an error with one or more of the registrations in a batch. In this case, the push notification service returns an error to Notification Hubs, and the process stops. The push notification service drops that batch completely. This is especially true with APNS, which uses a TCP stream protocol.

    We are optimized for at-most once delivery. But in this case, the faulting registration is removed from the database. Then, we retry notification delivery for the rest of the devices in that batch.

    To get more error information about the failed delivery attempt against a registration, you can use the Notification Hubs REST APIs [Per Message Telemetry: Get Notification Message Telemetry](https://msdn.microsoft.com/library/azure/mt608135.aspx) and [PNS feedback](https://msdn.microsoft.com/library/azure/mt705560.aspx). For sample code, see the [Send REST example](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/SendRestExample).

## <a name="push-notification-service-issues"></a>Probleme mit dem Pushbenachrichtigungsdienst

Nachdem die Benachrichtigung vom Pushbenachrichtigungsdienst der Plattform empfangen wurde, ist der Pushbenachrichtigungsdienst dafür zuständig, die Benachrichtigung an das Gerät zu übermitteln. Der Notification Hubs-Dienst ist hieran nicht mehr beteiligt und hat keine Kontrolle darüber, wann oder ob die Benachrichtigung an das Gerät gesendet wird.

Da Plattformbenachrichtigungsdienste sehr robust sind, erfolgt die Übertragung von Benachrichtigungen vom Pushbenachrichtigungsdienst an die Geräte üblicherweise innerhalb weniger Sekunden. Wenn der Pushbenachrichtigungsdienst gedrosselt ist, wendet Notification Hubs eine exponentielle Backoffstrategie an. Wenn der Pushbenachrichtigungsdienst 30 Minuten lang nicht erreichbar ist, sorgt eine Richtlinie dafür, dass diese Nachrichten als abgelaufen markiert und dauerhaft verworfen werden.

Wenn ein Pushbenachrichtigungsdienst versucht, eine Benachrichtigung zu übermitteln, das Gerät aber offline ist, wird die Benachrichtigung vom Pushbenachrichtigungsdienst für einen begrenzten Zeitraum gespeichert. Die Benachrichtigung wird an das Gerät gesendet, wenn es wieder verfügbar ist.

Für jede App wird nur eine zuletzt verwendete Benachrichtigung gespeichert. Werden mehrere Benachrichtigungen gesendet, während ein Gerät offline ist, bewirkt jede neue Benachrichtigung, dass die vorherige Benachrichtigung verworfen wird. Dieses Konzept, bei dem nur die neueste Benachrichtigung beibehalten wird, wird in APNs als *coalescing notifications* (Zusammenfügen von Benachrichtigungen) und in FCM (in dem ein reduzierbarer Schlüssel verwendet wird) als *collapsing* (reduzierend) bezeichnet. Wenn das Gerät über einen längeren Zeitraum offline bleibt, werden alle für das Gerät gespeicherten Benachrichtigungen verworfen. Weitere Informationen finden Sie unter [APNs overview] (Übersicht über APNs) und [About FCM messages] (Informationen zu FCM-Nachrichten).

Bei Azure Notification Hubs können Sie einen zusammenfügenden Schlüssel mit der generischen SendNotification-API in einem HTTP-Header übergeben. Für das .NET SDK verwenden Sie z.B. `SendNotificationAsync`. Die SendNotification-API akzeptiert auch HTTP-Header, die unverändert an den jeweiligen Pushbenachrichtigungsdienst übergeben werden.

## <a name="self-diagnosis-tips"></a>Tipps für die Selbstdiagnose

Hier finden Sie Möglichkeiten, die Ursache von verworfenen Benachrichtigungen in Notification Hubs zu diagnostizieren:

### <a name="verify-credentials"></a>Überprüfen von Anmeldeinformationen

**Entwicklerportal für den Pushbenachrichtigungsdienst:**

Überprüfen Sie die Anmeldeinformationen im Entwicklerportal des jeweiligen Pushbenachrichtigungsdiensts (APNs, FCM, Windows-Benachrichtigungsdienst usw.). Weitere Informationen finden Sie unter [Erste Schritte mit Azure Notification Hubs].

**Azure-Portal:**

Um die Anmeldeinformationen zu überprüfen und mit denjenigen abzugleichen, die Sie aus dem Entwicklerportal für den Pushbenachrichtigungsdienst erhalten haben, wechseln Sie im Azure-Portal zur Registerkarte **Zugriffsrichtlinien**.

![Azure-Portal – Zugriffsrichtlinien][4]

### <a name="verify-registrations"></a>Überprüfen von Registrierungen

**Visual Studio:**

Wenn Sie mit Visual Studio entwickeln, können Sie über den Server-Explorer eine Verbindung mit Azure herstellen, um verschiedene Azure-Dienste anzuzeigen und zu verwalten, einschließlich Notification Hubs. Dies ist in erster Linie für Ihre Test-/Entwicklungsumgebung nützlich.

![Server-Explorer von Visual Studio][9]

Sie können alle Registrierungen in Ihrem Hub anzeigen und verwalten. Die Registrierungen sind nach Plattform, nativer oder Vorlagenregistrierung, jeglichen Tags, Bezeichner des Pushbenachrichtigungsdiensts, Registrierungs-ID und Ablaufdatum kategorisiert. Auf dieser Seite können Sie Registrierungen auch bearbeiten. Dies ist besonders nützlich zur Bearbeitung von Tags.

![Visual Studio – Geräteregistrierungen][8]

> [!NOTE]
> Verwenden Sie Visual Studio, um während der Test-/Entwicklungsphase einige wenige Registrierungen zu bearbeiten. Wenn Sie Ihre Registrierungen per Massenvorgang bearbeiten müssen, bietet es sich an, die Funktion zum Exportieren/Importieren von Registrierungen zu verwenden, die unter [Massenhaftes Exportieren und Ändern von Registrierungen](https://msdn.microsoft.com/library/dn790624.aspx) beschrieben wird.

**Service Bus-Explorer:**

Viele Kunden verwenden [Service Bus-Explorer], um ihren Notification Hub anzuzeigen und zu verwalten. Service Bus-Explorer ist ein Open Source-Projekt. Beispiele finden Sie im [Service Bus-Explorer-Code].

### <a name="verify-message-notifications"></a>Überprüfen von Benachrichtigungen

 **Azure-Portal:**

Um eine Testbenachrichtigung an Ihre Clients zu senden, ohne ein Dienst-Back-End einrichten und ausführen zu müssen, wählen Sie unter **SUPPORT UND PROBLEMBEHANDLUNG** die Option **Testsendevorgang** aus.

![Funktion für Testsendevorgang in Azure][7]

**Visual Studio:**

Sie können Testbenachrichtigungen auch aus Visual Studio senden.

![Funktion für Testsendevorgang in Visual Studio][10]

Weitere Informationen zur Verwendung von Notification Hubs mit Visual Studio-Server-Explorer finden Sie in diesen Artikeln:

* [Anzeigen von Geräteregistrierungen für Notification Hubs]
* [Ausführliche Betrachtung: Visual Studio 2013 Update 2 RC und Azure SDK 2.3]
* [Announcing release of Visual Studio 2013 Update 3 and Azure SDK 2.4] (Ankündigung der Releases Visual Studio 2013 Update 3 und Azure SDK 2.4)

### <a name="debug-failed-notifications-and-review-notification-outcome"></a>Debuggen von Benachrichtigungsfehlern und Überprüfen von Benachrichtigungsergebnissen

**`EnableTestSend`-Eigenschaft:**

Wenn Sie eine Benachrichtigung über Notification Hubs senden, wird die Benachrichtigung zunächst in Notification Hubs in die Warteschlange für die Verarbeitung eingereiht. Notification Hubs bestimmt die richtigen Ziele und sendet die Benachrichtigung dann an den Pushbenachrichtigungsdienst. Wenn Sie die REST-API oder eines der Client-SDKs verwenden, bedeutet die erfolgreiche Rückgabe des Sendeaufrufs nur, dass die Nachricht erfolgreich in die Warteschlange von Notification Hubs eingereiht wurde. Sie erfahren nicht, was passiert, wenn Notification Hubs die Nachricht schließlich an den Pushbenachrichtigungsdienst sendet.

Wenn Ihre Benachrichtigung nicht auf dem Clientgerät ankommt, kann ein Fehler aufgetreten sein, als Notification Hubs versucht hat, die Nachricht an den Pushbenachrichtigungsdienst zu übermitteln. Möglicherweise überschreitet die Größe der Nutzlast das im Pushbenachrichtigungsdienst zulässige Maximum, oder die in Notification Hubs konfigurierten Anmeldeinformationen sind ungültig.

Um Einblick in Fehler des Pushbenachrichtigungsdiensts zu erhalten, können Sie die Eigenschaft [EnableTestSend] verwenden. Diese Eigenschaft wird automatisch aktiviert, wenn Sie Testnachrichten aus dem Portal oder aus dem Visual Studio-Client senden. Sie können diese Eigenschaft verwenden, um detaillierte Debuginformationen anzuzeigen. Sie können die Eigenschaft auch über APIs verwenden. Zurzeit können Sie sie im .NET SDK verwenden. In Zukunft soll sie zu allen Client-SDKs hinzugefügt werden.

Um die `EnableTestSend`-Eigenschaft mit dem REST-Aufruf zu verwenden, fügen Sie den Abfragezeichenfolgen-Parameter *test* an das Ende des Sendeaufrufs an. Beispiel: 

```text
https://mynamespace.servicebus.windows.net/mynotificationhub/messages?api-version=2013-10&test
```

**Beispiel (.NET SDK):**

Im Folgenden finden Sie ein Beispiel für die Verwendung des .NET SDK zum Senden einer nativen Popupbenachrichtigung:

```csharp
NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName);
var result = await hub.SendWindowsNativeNotificationAsync(toast);
Console.WriteLine(result.State);
```

Am Ende der Ausführung hat `result.State` einfach den Wert `Enqueued`. Die Ergebnisse bieten keinerlei Informationen darüber, was mit Ihrer Pushbenachrichtigung geschehen ist.

Anschließend können Sie die boolesche `EnableTestSend`-Eigenschaft verwenden. Verwenden Sie die `EnableTestSend`-Eigenschaft, während Sie `NotificationHubClient` initialisieren, um einen detaillierten Status zu Fehlern im Pushbenachrichtigungsdienst zu erhalten, die beim Senden einer Benachrichtigung auftreten. Der Sendeaufruf benötigt zusätzliche Zeit für die Rückgabe, da er erst zurückgegeben wird, nachdem Notification Hubs die Benachrichtigung an den Pushbenachrichtigungsdienst übermittelt hat, um das Ergebnis zu ermitteln.

```csharp
    bool enableTestSend = true;
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName, enableTestSend);

    var outcome = await hub.SendWindowsNativeNotificationAsync(toast);
    Console.WriteLine(outcome.State);

    foreach (RegistrationResult result in outcome.Results)
    {
        Console.WriteLine(result.ApplicationPlatform + "\n" + result.RegistrationId + "\n" + result.Outcome);
    }
```

**Beispielausgabe:**

```text
DetailedStateAvailable
windows
7619785862101227384-7840974832647865618-3
The Token obtained from the Token Provider is wrong
```

Diese Nachricht weist darauf hin, dass entweder in Notification Hubs ungültige Anmeldeinformationen konfiguriert wurden oder dass ein Problem mit den Registrierungen im Hub vorliegt. Es empfiehlt sich, diese Registrierung zu löschen und den Client die Registrierung vor dem Senden der Nachricht erneut erstellen zu lassen.

> [!NOTE]
> Die Verwendung der `EnableTestSend`-Eigenschaft ist stark eingeschränkt. Verwenden Sie diese Option nur in einer Entwicklungs-/Testumgebung und nur mit einer begrenzten Anzahl von Registrierungen. Debugbenachrichtigungen werden nur an 10 Geräte gesendet. Außerdem ist die Verarbeitung von Debugsendevorgängen auf 10 pro Minute beschränkt.

### <a name="review-telemetry"></a>Überprüfen der Telemetrie

**Verwenden des Azure-Portals:**

Das Portal bietet Ihnen einen schnellen Überblick über alle Aktivitäten in Ihrem Notification Hub.

1. Auf der Registerkarte **Übersicht** finden Sie eine aggregierte Ansicht der Registrierungen, Benachrichtigungen und Fehler für jede Plattform.

    ![Übersicht über Notification Hubs – Dashboard][5]

2. Auf der Registerkarte **Überwachen** können Sie viele weitere plattformspezifische Metriken hinzufügen, um detailliertere Einblicke zu erhalten. Sie können speziell nach Fehlern im Zusammenhang mit dem Pushbenachrichtigungsdienst suchen, die zurückgegeben werden, wenn der Notification Hubs-Dienst versucht, die Benachrichtigung an den Pushbenachrichtigungsdienst zu senden.

    ![Azure-Portal – Aktivitätsprotokoll][6]

3. Beginnen Sie mit der Überprüfung von **Eingehende Nachrichten**, **Registrierungsvorgänge** und **Erfolgreiche Benachrichtigungen**. Wechseln Sie dann zu der Registerkarte für die jeweilige Plattform, um spezifische Fehler beim Pushbenachrichtigungsdienst zu überprüfen.

4. Wenn die Authentifizierungseinstellungen für Ihren Notification Hub falsch sind, wird die Nachricht **PNS-Authentifizierungsfehler** angezeigt. Dies ist ein Hinweis darauf, dass Sie die Anmeldeinformationen für den Pushbenachrichtigungsdienst überprüfen sollten.

* **Programmgesteuerter Zugriff**

Weitere Informationen zum programmgesteuerten Zugriff finden Sie unter [Programmgesteuerter Telemetriezugriff].

> [!NOTE]
> Verschiedene telemetriebezogene Features wie z.B. der Export und Import von Registrierungen und der Telemetriezugriff über APIs sind nur auf der Dienstebene „Standard“ verfügbar. Beim Versuch, diese Features auf der Dienstebene „Free“ oder „Basic“ zu verwenden, erhalten Sie eine Ausnahmemeldung, wenn Sie das SDK verwenden, und einen HTTP 403-Fehler (Verboten), wenn Sie die Features direkt über die REST-APIs verwenden.
>
> Um telemetriebezogene Features zu verwenden, stellen Sie zunächst im Azure-Portal sicher, dass Sie die Dienstebene „Standard“ verwenden.  

<!-- IMAGES -->
[0]: ./media/notification-hubs-diagnosing/Architecture.png
[1]: ./media/notification-hubs-diagnosing/FCMConfigure.png
[3]: ./media/notification-hubs-diagnosing/FCMServerKey.png
[4]: ../../includes/media/notification-hubs-portal-create-new-hub/notification-hubs-connection-strings-portal.png
[5]: ./media/notification-hubs-diagnosing/PortalDashboard.png
[6]: ./media/notification-hubs-diagnosing/PortalAnalytics.png
[7]: ./media/notification-hubs-ios-get-started/notification-hubs-test-send.png
[8]: ./media/notification-hubs-diagnosing/VSRegistrations.png
[9]: ./media/notification-hubs-diagnosing/VSServerExplorer.png
[10]: ./media/notification-hubs-diagnosing/VSTestNotification.png

<!-- LINKS -->
[Übersicht über Notification Hubs]: notification-hubs-push-notification-overview.md
[Erste Schritte mit Azure Notification Hubs]: notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[Templates]: https://msdn.microsoft.com/library/dn530748.aspx
[APNs overview]: https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html (Übersicht über APNs)
[About FCM messages]: https://firebase.google.com/docs/cloud-messaging/concept-options (Informationen zu FCM-Nachrichten)
[Export and modify registrations in bulk]: https://msdn.microsoft.com/library/dn790624.aspx
[Service Bus-Explorer]: https://msdn.microsoft.com/library/dn530751.aspx#sb_explorer
[Service Bus-Explorer-Code]: https://code.msdn.microsoft.com/windowsazure/Service-Bus-Explorer-f2abca5a
[Anzeigen von Geräteregistrierungen für Notification Hubs]: https://msdn.microsoft.com/library/windows/apps/xaml/dn792122.aspx
[Ausführliche Betrachtung: Visual Studio 2013 Update 2 RC und Azure SDK 2.3]: https://azure.microsoft.com/blog/2014/04/09/deep-dive-visual-studio-2013-update-2-rc-and-azure-sdk-2-3/#NotificationHubs
[Announcing release of Visual Studio 2013 Update 3 and Azure SDK 2.4]: https://azure.microsoft.com/blog/2014/08/04/announcing-release-of-visual-studio-2013-update-3-and-azure-sdk-2-4/ (Ankündigung der Releases Visual Studio 2013 Update 3 und Azure SDK 2.4)
[EnableTestSend]: https://docs.microsoft.com/dotnet/api/microsoft.azure.notificationhubs.notificationhubclient.enabletestsend?view=azure-dotnet
[Programmgesteuerter Telemetriezugriff]: https://msdn.microsoft.com/library/azure/dn458823.aspx
