---
title: Ablauf von Azure Service Bus-Nachrichten | Microsoft-Dokumentation
description: "Ablauf und Gültigkeitsdauer von Azure Service Bus-Nachrichten"
services: service-bus-messaging
documentationcenter: 
author: clemensv
manager: timlt
editor: 
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2017
ms.author: sethm
ms.openlocfilehash: 504010a39a4012b9a9edb60bb9a5b33ac20499c1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="message-expiration-time-to-live"></a>Nachrichtenablauf (Gültigkeitsdauer)

Die Nutzlast innerhalb einer Nachricht, oder ein Befehl oder eine Abfrage, den/die eine Nachricht an einen Empfänger übermittelt, unterliegt fast immer einer Form von Ablauffrist auf Anwendungsebene. Nach Ablauf einer solchen Frist wird der Inhalt nicht mehr zugestellt oder der gewünschte Vorgang nicht mehr ausgeführt.

Für Entwicklungs- und Testumgebungen, in denen Warteschlangen und Themen häufig im Rahmen von Teilläufen von Anwendungen oder Anwendungsteilen verwendet werden, ist es auch wünschenswert, dass nicht zugestellte Testnachrichten automatisch gesammelt werden, damit der nächste Testlauf davon unbeeinflusst starten kann.

Der Ablauf jeder einzelnen Nachricht kann durch das Festlegen der Systemeigenschaft [TimeToLive](/dotnet/api/microsoft.azure.servicebus.message.timetolive#Microsoft_Azure_ServiceBus_Message_TimeToLive) gesteuert werden, die eine relative Dauer angibt. Der Ablauf wird zu einem absoluten Zeitpunkt, an dem die Nachricht in die Warteschlange der Entität gestellt wird. Derzeit akzeptiert die [ExpiresAtUtc](/dotnet/api/microsoft.azure.servicebus.message.expiresatutc)-Eigenschaft nur den Wert [**EnqueuedTimeUtc**](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.enqueuedtimeutc#Microsoft_ServiceBus_Messaging_BrokeredMessage_EnqueuedTimeUtc) + [**TimeToLive**](/dotnet/api/microsoft.azure.servicebus.message.timetolive#Microsoft_Azure_ServiceBus_Message_TimeToLive).

Nach Ablauf des **ExpiresAtUtc**-Zeitpunkts können Nachrichten nicht mehr abgerufen werden. Die Ablaufzeit wirkt sich nicht auf Nachrichten aus, die momentan für die Zustellung gesperrt sind. Diese Nachrichten werden weiterhin normal behandelt. Wenn die Sperre abläuft oder die Nachricht abgebrochen wird, tritt der Ablauf sofort in Kraft.

Solange die Nachricht gesperrt ist, kann die Anwendung möglicherweise im Besitz einer Nachricht sein, die nominell abgelaufen ist. Ob die Anwendung bereit ist, mit der Verarbeitung fortzufahren oder ob die Nachricht abgebrochen wird, bleibt dem Implementierer überlassen.

## <a name="entity-level-expiration"></a>Ablauf auf Entitätsebene

Alle Nachrichten, die in eine Warteschlange oder an ein Thema gesendet werden, unterliegen einer Standardablaufzeit, die auf Entitätsebene mit der [defaultMessageTimeToLive](/azure/templates/microsoft.servicebus/namespaces/queues)-Eigenschaft festgelegt wird. Diese Einstellung auch im Portal während der Erstellung festgelegt und später angepasst werden kann. Die Standardablaufzeit wird für alle an die Entität gesendeten Nachrichten verwendet, bei denen [TimeToLive](/dotnet/api/microsoft.azure.servicebus.message.timetolive#Microsoft_Azure_ServiceBus_Message_TimeToLive) nicht explizit festgelegt ist. Die Standardablaufzeit dient auch als Obergrenze für den **TimeToLive**-Wert. Nachrichten, die eine längere **TimeToLive**-Ablaufzeit als der Standardwert haben, werden automatisch an den **defaultMessageTimeToLive**-Wert angepasst, bevor sie in die Warteschlange gestellt werden.

Abgelaufene Nachrichten können optional in eine [Warteschlange für nicht zustellbare Nachrichten](service-bus-dead-letter-queues.md) verschoben werden, indem die [EnableDeadLetteringOnMessageExpiration](/dotnet/api/microsoft.servicebus.messaging.queuedescription.enabledeadletteringonmessageexpiration#Microsoft_ServiceBus_Messaging_QueueDescription_EnableDeadLetteringOnMessageExpiration)-Eigenschaft festgelegt oder das entsprechende Kontrollkästchen im Portal aktiviert wird. Wenn die Option deaktiviert bleibt, werden abgelaufene Nachrichten verworfen. Abgelaufene Nachrichten, die in die Warteschlange für unzustellbare Nachrichten verschoben werden, können durch die Auswertung der [DeadletterReason](service-bus-dead-letter-queues.md#moving-messages-to-the-dlq)-Eigenschaft, die der Broker in den Benutzereigenschaften speichert, von anderen unzustellbaren Nachrichten unterschieden werden. In diesem Fall ist der Wert [TTLExpiredException](service-bus-dead-letter-queues.md#moving-messages-to-the-dlq).

In dem oben genannten Fall, in dem die Nachricht vor dem Ablaufen geschützt ist, solange sie gesperrt ist und das Flag für die Entität festgelegt ist, wird die Nachricht in die Warteschlange für unzustellbare Nachrichten verschoben, sobald die Sperre aufgehoben wird oder abläuft. Sie wird jedoch nicht verschoben, wenn die Nachricht erfolgreich verarbeitet wurde. Dann wird davon ausgegangen, dass die Anwendung sie trotz des nominalen Ablaufs erfolgreich abgearbeitet hat.

Die Kombination aus [TimeToLive](/dotnet/api/microsoft.azure.servicebus.message.timetolive#Microsoft_Azure_ServiceBus_Message_TimeToLive) und automatischer (und transaktionaler) Funktion für unzustellbare Nachrichten bei Ablauf ist ein wertvolles Instrument, um Vertrauen bei der Frage zu schaffen, ob ein Auftrag, der einem Handler oder einer Gruppe von Handlern innerhalb einer Frist erteilt wurde, bei Erreichen der Frist zur Verarbeitung abgerufen wird.

Stellen Sie sich beispielsweise eine Website vor, die Aufträge zuverlässig in einem Back-End mit eingeschränkter Skalierung ausführen muss, und bei der es gelegentlich zu Datenverkehrsspitzen kommt oder die von Verfügbarkeitszeiträumen dieses Back-Ends isoliert werden soll. Im Regelfall verschiebt der serverseitige Handler für die übermittelten Benutzerdaten die Informationen in eine Warteschlange und erhält anschließend eine Antwort, die die erfolgreiche Verarbeitung der Transaktion in einer Antwortwarteschlange bestätigt. Wenn es eine Datenverkehrsspitze gibt und der Back-End-Handler seine Backlog Items nicht rechtzeitig verarbeiten kann, werden die abgelaufenen Aufträge an die Warteschlange für unzustellbare Nachrichten zurückgegeben. Der interaktive Benutzer kann benachrichtigt werden, dass der angeforderte Vorgang etwas länger als gewöhnlich dauert. Die Anforderung kann dann in eine andere Warteschlange für einen Verarbeitungspfad gestellt werden, woraufhin das endgültige Verarbeitungsergebnis per E-Mail an den Benutzer gesendet wird. 

## <a name="temporary-entities"></a>Temporäre Entitäten

Service Bus-Warteschlangen, -Themen und -Abonnements können als temporäre Entitäten erstellt werden, die automatisch entfernt werden, sobald sie für einen angegebenen Zeitraum nicht verwendet wurden.
 
Die automatische Bereinigung ist in Entwicklungs- und Testszenarien sinnvoll, in denen Entitäten dynamisch erstellt werden und nach der Nutzung nicht bereinigt werden, da der Test- oder Debuglauf unterbrochen wurde. Sie ist auch nützlich, wenn eine Anwendung dynamische Entitäten erstellt, wie z.B. eine Antwortwarteschlange, um Antworten wieder in einen Webserverprozess oder in einem anderen relativ kurzlebigen Objekt zu empfangen. In diesem Fall ist es schwierig, diese Entitäten zuverlässig zu bereinigen, wenn die Objektinstanz nicht mehr vorhanden ist.

Das Feature wird mit der [AutoDeleteOnIdle](/azure/templates/microsoft.servicebus/namespaces/queues)-Eigenschaft aktiviert, die auf die Dauer festgelegt ist, die eine Entität inaktiv (nicht verwendet) sein muss, bevor sie automatisch gelöscht wird. Die Mindestdauer ist fünf Minuten.
 
Die Eigenschaft muss über einen Azure Resource Manager-Vorgang oder über die [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager)-APIs des .NET Framework-Clients festgelegt werden. Sie kann nicht im Portal festgelegt werden.


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Service Bus-Messaging finden Sie in folgenden Themen:

* [Service Bus – Grundlagen](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus-Warteschlangen, -Themen und -Abonnements](service-bus-queues-topics-subscriptions.md)
* [Erste Schritte mit Service Bus-Warteschlangen](service-bus-dotnet-get-started-with-queues.md)
* [Verwenden von Service Bus-Themen und -Abonnements](service-bus-dotnet-how-to-use-topics-subscriptions.md)