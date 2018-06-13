---
title: Was ist Azure Notification Hubs?
description: Erfahren Sie, wie Sie mit Azure Notification Hubs Pushbenachrichtigungsfunktionen hinzuzufügen.
author: dimazaid
manager: kpiteira
editor: spelluru
services: notification-hubs
documentationcenter: ''
ms.assetid: fcfb0ce8-0e19-4fa8-b777-6b9f9cdda178
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: multiple
ms.devlang: multiple
ms.topic: overview
ms.custom: mvc
ms.date: 04/14/2018
ms.author: dimazaid
ms.openlocfilehash: 44086bc20966d9c01ff27dda68f837101c71a778
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
ms.locfileid: "33777928"
---
# <a name="what-is-azure-notification-hubs"></a>Was ist Azure Notification Hubs?
Azure Notification Hubs bietet ein benutzerfreundliches, horizontal skalierbares Pushmodul, mit dem Sie von einem beliebigen Back-End (cloudbasiert oder lokal) Benachrichtigungen an sämtliche Plattformen (iOS, Android, Windows, Kindle, Baidu usw.) senden können. Notification Hubs eignet sich sowohl für Unternehmens- als auch Privatkundenszenarien. Es folgen einige Beispielszenarios:

- Senden von Benachrichtigungen zu brandaktuellen Nachrichten an Millionen von Empfängern mit niedriger Latenz
- Senden standortbasierter Gutscheine an interessierte Kundengruppen
- Senden von Ereignisbenachrichtigungen an Benutzer oder Gruppen für Medien-/Sport-/Finanz-/Spieleanwendungen
- Pushübertragung von Werbeinhalten an Anwendungen, um Kunden anzusprechen und zum Kauf anzuregen
- Benachrichtigen von Benutzer zu Unternehmensereignissen wie neue Nachrichten und Arbeitsaufgaben
- Senden von Codes für die mehrstufige Authentifizierung

## <a name="what-are-push-notifications"></a>Was sind Pushbenachrichtigungen?
Pushbenachrichtigungen sind eine Form der Kommunikation zwischen einer App und Benutzern, bei der Benutzer mobiler Apps zu bestimmten gewünschten Informationen benachrichtigt werden, meist in einem Popupfenster oder Dialogfeld. Benutzer haben im Allgemeinen die Wahl, ob Sie die Nachricht anzeigen oder löschen möchten. Wenn sie die Nachricht anzeigen, wird die mobile App geöffnet, die die Benachrichtigung kommuniziert hat.

Pushbenachrichtigungen sind bei Privatkunden-Apps zum Steigern der App-Nutzung und bei Unternehmens-Apps zum Verbreiten aktueller Geschäftsinformationen von Bedeutung. Dies ist die beste Lösung für die Kommunikation zwischen App und Benutzer, da sie für Mobilgeräte energieeffizient, für die Absender von Benachrichtigungen flexibel und verfügbar ist, wenn entsprechende Apps nicht aktiv sind.

Weitere Informationen zu Pushbenachrichtigungen für einige gängige Plattformen finden Sie in den folgenden Themen: 
* [iOS](https://developer.apple.com/notifications/)
* [Android](https://developer.android.com/guide/topics/ui/notifiers/notifications.html)
* [Windows](http://msdn.microsoft.com/library/windows/apps/hh779725.aspx)

## <a name="how-push-notifications-work"></a>Funktionsweise von Pushbenachrichtigungen
Pushbenachrichtigungen werden über plattformspezifische Infrastrukturen übermittelt, die *Platform Notification Systems* (PNS, Plattformbenachrichtigungssystem) genannt werden. Sie bieten schlanke Pushfunktionen für die Übermittlung von Nachrichten an ein Gerät mittels eines bereitgestellten Handles und haben keine gemeinsame Schnittstelle. Zum Senden einer Benachrichtigung an alle Benutzer der iOS-, Android- und Windows-Versionen einer App muss der Entwickler mit APNS (Apple Push Notification Service), FCM (Firebase Cloud Messaging) und WNS (Windows Notification Service) arbeiten.

Aus allgemeiner Sicht funktionieren Pushbenachrichtigungen wie folgt:

1. Die Client-App bestimmt, dass Benachrichtigungen empfangen werden sollen. Sie kontaktiert deshalb das entsprechende PNS zum Abrufen ihres eindeutigen und temporären Push-Handles. Der Handletyp hängt vom System ab (WNS weist z.B URIs auf, während APNS mit Token arbeitet).
2. Die Client-App speichert dieses Handle zur späteren Verwendung im Back-End oder Anbieter.
3. Um eine Pushbenachrichtigung zu senden, nimmt das Back-End der App über den Handle Kontakt mit dem PNS auf, um eine bestimmte Client-App als Ziel zu wählen.
4. Das PNS leitet die Benachrichtigung an das vom Handle angegebene Gerät weiter.

![Pushbenachrichtigungsworkflow](./media/notification-hubs-overview/registration-diagram.png)

## <a name="the-challenges-of-push-notifications"></a>Die Herausforderungen von Pushbenachrichtigungen
PNS-Systeme sind leistungsstark. Der App-Entwickler hat jedoch immer noch sehr viel Arbeit, selbst wenn er nur allgemeine Pushbenachrichtigungsszenarien wie das Übertragungen von Pushbenachrichtigungen an in Segmente eingeteilte Benutzer implementieren möchte.

Pushbenachrichtigungen erfordern eine komplexe Infrastruktur, die nicht in Bezug zur Hauptgeschäftslogik der Anwendung steht. Es folgen einige der Herausforderungen an die Infrastruktur:

- **Plattformabhängigkeit**
    - Das Back-End muss komplexe und schwer zu verwaltende plattformabhängige Logik zum Senden von Benachrichtigungen an Geräte auf verschiedenen Plattformen aufweisen, da PNS-Systeme nicht vereinheitlicht sind.
- **Skalieren**
    - Gemäß PNS-Richtlinien müssen Gerätetoken bei jedem Start einer App aktualisiert werden. Das Back-End muss sehr viel Datenverkehr und Datenbankzugriffe bewältigen, bloß damit die Token aktuell bleiben. Wenn die Anzahl der Geräte auf Hunderte und Tausende von Millionen wächst, sind die Kosten der Erstellung und Wartung dieser Infrastruktur beträchtlich.
    - Die meisten PNS-Systeme unterstützten keine Übertragung an mehrere Geräte. Eine einfache Übertragung an eine Million Geräte mündet in einer Million Aufrufe an die PNS-Systeme. Das Skalieren dieses Datenverkehrsvolumens mit minimaler Latenz ist keine einfache Aufgabe.
- **Routing** 
    - Obwohl PNS-Systeme eine Möglichkeit zum Senden von Nachrichten an Geräte bieten, richten sich die meisten App-Benachrichtigungen zielgerichtet an Benutzer oder Interessengruppen. Das Back-End muss eine Registrierung verwalten, um Geräte Interessengruppen, Benutzern, Eigenschaften usw. zuzuordnen. Dieser Aufwand kommt noch zur Markteinführungszeit und zu den Wartungskosten einer App hinzu.

## <a name="why-use-azure-notification-hubs"></a>Gründe für die Verwendung von Azure Notification Hubs
Notification Hubs nimmt Ihnen die gesamte Komplexität des eigenständigen Bereitstellens von Pushbenachrichtigungen von Ihrem App-Back-End ab. Die horizontal skalierte Pushbenachrichtigungsinfrastruktur für mehrere Plattformen reduziert den Code für Pushvorgänge und vereinfacht Ihr Back-End. Bei Notification Hubs sind Geräte lediglich zuständig für das Registrieren ihrer PNS-Handles bei einem Hub, während das Back-End Nachrichten an Benutzer oder Interessengruppen sendet (siehe die folgende Abbildung):

![Notification Hubs-Diagramm](./media/notification-hubs-overview/notification-hub-diagram.png)

Notification Hubs ist Ihre einsatzbereite Push-Engine mit den folgenden Vorteilen:

- **Plattformübergreifend**
    - Unterstützung für alle wichtigen Pushplattformen, einschließlich iOS, Android, Windows, Kindle und Baidu.
    - Eine gemeinsame Schnittstelle für Pushbenachrichtigungen an alle Plattformen in plattformspezifischen oder plattformunabhängigen Formaten ohne plattformspezifische Aufgaben.
    - Zentrale Verwaltung von Gerätehandles.
- **Back-End-übergreifend**
    - Cloud oder lokal
    - .NET, Node.js, Java, usw.
- **Umfangreiche Bereitstellungsmuster**
    - Übertragung an eine oder mehrere Plattformen: Über einen einzigen API-Aufruf können Sie Millionen von Geräten auf verschiedenen Plattformen erreichen.
    - Push an Gerät: Sie können Benachrichtigungen an einzelne Geräte richten.
    - Push an Benutzer: Die Features „Tags“ und „Vorlagen“ helfen Ihnen, alle Geräte eines Benutzers auf verschiedenen Plattformen zu erreichen.
    - Push an Segment mit dynamischen Tags: Das Feature „Tags“ hilft Ihnen, Geräte zu segmentieren und sie per Push gemäß Ihren Anforderungen zu erreichen, ganz gleich, ob Sie an ein Segment oder an einen Ausdruck von Segmenten senden (Beispiel: aktiv UND lebt in Stuttgart NICHT neuer Benutzer). Anstatt auf Veröffentlichung/Abonnements beschränkt zu sein, können Sie Gerätetags jederzeit und überall aktualisieren.
    - Lokalisierte Pushbenachrichtigungen: Das Feature „Vorlagen“ verhilft Ihnen zu einer Lokalisierung, ohne sich auf den Back-End-Code auszuwirken.
    - Automatische Pushbenachrichtigungen: Sie können das Push-Pull-Muster aktivieren, indem Sie automatische Benachrichtigungen an Geräte senden, um auszulösen, dass sie bestimmte Pullvorgänge oder Aktionen ausführen.
    - Geplante Pushbenachrichtigungen: Sie können jederzeit das Senden von Benachrichtigungen planen.
    - Direkte Pushbenachrichtigungen: Sie können das Registrieren von Geräten beim Notification Hubs-Dienst überspringen und direkte Pushbenachrichtigungen im Batch an eine Liste von Gerätehandles auslösen.
    - Personalisierte Pushbenachrichtigungen: Pushvariablen für Geräte helfen Ihnen beim Senden gerätespezifischer, personalisierter Pushbenachrichtigungen mit angepassten Schlüssel-Wert-Paaren.
- **Umfangreiche Telemetrie**
    - Allgemeine Telemetriedaten zu Pushbenachrichtigungen, Geräten, Fehlern und Betrieb stehen im Azure-Portal und programmgesteuert zur Verfügung.
    - Nachrichtenbasierte Telemetrie dient zum Nachverfolgen jedes Pushvorgangs von Ihrem anfänglichen Anforderungsaufruf an den Notification Hubs-Dienst, der die Pushbenachrichtigungen als Batch erfolgreich verarbeitet.
    - Das Feedback zum Plattformbenachrichtigungssystem dient zum Kommunizieren des gesamten Feedbacks zum Plattformbenachrichtigungssystem, um das Debuggen zu erleichtern.
- **Skalierbarkeit** 
    - Senden Sie Nachrichten schnell an Millionen von Geräten, ohne die Architektur umzubauen oder ein Sharding von Geräten durchzuführen.
- **Sicherheit**
    - Gemeinsamer geheimer Zugriffsschlüssel (Shared Access Secret, SAS) oder Verbundauthentifizierung.

## <a name="integration-with-app-service-mobile-apps"></a>Integration in Mobile App Service-Apps
Um eine nahtlose und einheitliche Verwendung in Azure-Diensten zu ermöglichen, ist in [Mobile App Service-Apps](../app-service-mobile/app-service-mobile-value-prop.md) die Unterstützung von Pushbenachrichtigungen, die über Notification Hubs abgewickelt werden, bereits integriert. [Mobile App Service-Apps](../app-service-mobile/app-service-mobile-value-prop.md) bietet eine hochgradig skalierbare, global verfügbare Entwicklungsplattform für mobile Anwendungen für Unternehmensentwickler und Systemintegratoren, die umfassende Funktionen für mobile Entwickler bereitstellt.

Mobile Apps-Entwickler können Notification Hubs mit dem folgenden Workflow verwenden:

1. Abrufen des PNS-Gerätehandle
2. Registrieren des Geräts bei Notification Hubs über die praktische Registrierungs-API der Mobile Apps-Client-SDKs

    > [!NOTE]
    > Beachten Sie, dass Mobile Apps aus Sicherheitsgründen alle Tags für Registrierungen entfernt. Arbeiten Sie direkt von Ihrem Back-End aus mit Notification Hubs, um Tags Geräten zuzuordnen.
1. Senden von Benachrichtigungen von Ihrem App-Back-End mit Notification Hubs

Einige Vorteile, die sich aus dieser Integration für Entwickler ergeben:

- **Mobile Apps-Client-SDKs**: Diese plattformübergreifenden SDKs bieten einfache APIs für die Registrierung und Kommunikation mit dem Notification Hub, der automatisch mit der mobilen App verknüpft wird. Entwickler müssen benötigen keine Notification Hubs-Anmeldeinformationen und müssen keinen zusätzlichen Dienst verwenden.
    - *Push an Benutzer*: Die SDKs markieren automatisch das angegebene Gerät mit einer bei Mobile Apps authentifizierten Benutzer-ID, um das Push-an-Benutzer-Szenario zu ermöglichen.
    - *Push an Gerät*: Die SDKs verwenden die Installations-ID von Mobile Apps automatisch als GUID für die Registrierung bei Notification Hubs, sodass Entwickler nicht mehrere Dienst-GUIDs verwalten müssen.
- **Installationsmodell**: Mobile Apps verwendet das neueste Notification Hubs-Pushmodell zur Darstellung aller einem Gerät zugeordneten Pusheigenschaften in einer JSON-Installation, das auf Push Notification Services ausgerichtet ist und sich einfach verwenden lässt.
- **Flexibilität**: Entwickler können auch bei eingerichteter Integration immer mit Notification Hubs direkt arbeiten.
- **In das [Azure-Portal](https://portal.azure.com)** integrierte Umgebung: Push als Funktion ist in Mobile Apps visuell dargestellt, sodass Entwickler problemlos mit dem zugeordneten Notification Hub über Mobile Apps arbeiten können.

## <a name="next-steps"></a>Nächste Schritte
Informationen zu den ersten Schritten beim Erstellen und Verwenden eines Benachrichtigungshubs finden Sie unter [Erste Schritte mit Azure Notification Hubs für Android-Apps und Firebase Cloud Messaging](notification-hubs-android-push-notification-google-fcm-get-started.md). [0]: ./media/notification-hubs-overview/registration-diagram.png [1]: ./media/notification-hubs-overview/notification-hub-diagram.png [Wie Kunden Notification Hubs verwenden]: http://azure.microsoft.com/services/notification-hubs [Tutorials und Anleitungen zu Notification Hubs]: http://azure.microsoft.com/documentation/services/notification-hubs [iOS]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started [Android]: http://azure.microsoft.com/documentation/articles/notification-hubs-android-get-started [Windows Universal]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started [Windows Phone]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-phone-get-started [Kindle]: http://azure.microsoft.com/documentation/articles/notification-hubs-kindle-get-started [Xamarin.iOS]: http://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-ios-get-started [Xamarin.Android]: http://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-android-get-started [Microsoft.WindowsAzure.Messaging.NotificationHub]: http://msdn.microsoft.com/library/microsoft.windowsazure.messaging.notificationhub.aspx [Microsoft.ServiceBus.Notifications]: http://msdn.microsoft.com/library/microsoft.servicebus.notifications.aspx [App Service Mobile Apps]: https://azure.microsoft.com/documentation/articles/app-service-mobile-value-prop/ [Vorlagen]: notification-hubs-templates-cross-platform-push-messages.md [Azure-Portal]: https://portal.azure.com [Tags]: (http://msdn.microsoft.com/library/azure/dn530749.aspx)
