---
title: "Azure Service Bus-Nachrichtenverzögerung | Microsoft-Dokumentation"
description: "Verzögertes Übermitteln von Service Bus-Nachrichten"
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
ms.openlocfilehash: f84b870de4b79399d5edc90284c9c56222156b5d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="message-deferral"></a>Nachrichtenverzögerung

Wenn eine Warteschlange oder ein Abonnementclient eine Nachricht erhält, die verarbeitet werden soll, aber die Verarbeitung aufgrund von besonderen Umständen innerhalb der Anwendung zu diesem Zeitpunkt nicht möglich ist, besteht die Möglichkeit, das Abrufen der Nachricht auf einen späteren Zeitpunkt zu „verzögern“. Die Nachricht verbleibt in der Warteschlange oder im Abonnement, wird jedoch zurückgestellt.

Das Verzögerungsfeature wurde speziell für Szenarios für die Workflowverarbeitung erstellt. Workflowframeworks erfordern möglicherweise bestimmte Vorgänge, um in einer bestimmten Reihenfolge verarbeitet zu werden. Das Verarbeiten mancher empfangenen Nachrichten muss möglicherweise verschoben werden, bis die vorgeschriebenen vorherigen Arbeiten, die auf anderen Nachrichten basieren, abgeschlossen wurden.

Ein einfaches und veranschaulichendes Beispiel hierfür ist die Verarbeitungssequenz für eine Bestellung, bei der eine Zahlungsaufforderung von einem externen Zahlungsdienstleister in einem System angezeigt wird, bevor die entsprechende Bestellung vom Laden an das Abwicklungssystem weitergegeben wurde. In diesem Fall kann das Abwicklungssystem das Verarbeiten der Zahlungsaufforderung verzögern, bis eine Bestellung vorhanden ist, der diese zugeordnet werden kann. In Rendezvousszenarios, bei denen Nachrichten von verschiedenen Quellen einen Workflow vorantreiben, kann die Echtzeitausführung einer Bestellung korrekt sein, aber die Nachrichten, die die Ergebnisse darstellen, können in der falschen Reihenfolge eingehen.

Die Verzögerung unterstützt letztendlich das Neuordnen der Nachrichten von der Eingangsreihenfolge zu einer Reihenfolge, in der diese verarbeitet werden können. Dabei werden die Nachrichten sicher im Nachrichtenspeicher belassen, deren Verarbeitung verschoben werden muss.

## <a name="message-deferral-apis"></a>APIs für die Nachrichtenverzögerung

Die API im .NET Framework-Client ist [BrokeredMessage.Defer](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.defer?view=azureservicebus-4.1.1#Microsoft_ServiceBus_Messaging_BrokeredMessage_Defer) oder [BrokeredMessage.DeferAsync](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.deferasync?view=azureservicebus-4.1.1#Microsoft_ServiceBus_Messaging_BrokeredMessage_DeferAsync), [MessageReceiver.DeferAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.deferasync) im .NET Standard-Client und **mesageReceiver.defer** oder **messageReceiver.deferSync** im Java-Client. 

Verzögerte Nachrichten bleiben im Gegensatz zu unzustellbaren Nachrichten, die sich in einer Unterwarteschlange befinden, mit allen anderen aktiven Nachrichten in der Hauptwarteschlange, können jedoch nicht mehr mithilfe der regulären Funktionen „Receive“ bzw. „ReceiveAsync“ empfangen werden. Verzögerte Nachrichten können über das [Durchsuchen von Nachrichten](message-browsing.md) ermittelt werden, wenn eine Anwendung diese nicht mehr findet.

Zum Abrufen einer verzögerten Nachricht muss deren „Besitzer“ sich die [SequenceNumber](/dotnet/api/microsoft.azure.servicebus.message.systempropertiescollection.sequencenumber#Microsoft_Azure_ServiceBus_Message_SystemPropertiesCollection_SequenceNumber) beim Verzögern merken. Jeder Empfänger, der die **SequenceNumber** einer verzögerten Nachricht kennt, kann die Nachricht später explizit durch „Receive(sequenceNumber)“ empfangen.

Wenn eine Nachricht nicht verarbeitet werden kann, weil eine bestimmte Ressource für die Verarbeitung dieser Nachricht vorübergehend nicht verfügbar ist, aber die Verarbeitung der Nachricht nicht sofort angehalten werden soll, stellt das Merken der **SequenceNumber** in einer [geplanten Nachricht](message-sequencing.md), die in ein paar Minuten gesendet werden soll und das erneute Abrufen der verzögerten Nachricht, wenn die geplante Nachricht eingeht, eine elegante Methode dar, um diese Nachricht für ein paar Minuten zurückzustellen. Hinweis: Wenn alle Vorgänge eines Nachrichtenhandlers von einer Datenbank abhängen und diese Datenbank vorübergehend nicht verfügbar ist, sollten Sie nicht die Verzögerung nutzen, sondern das Empfangen von Nachrichten vollständig anhalten, bis die Datenbank wieder verfügbar ist.

Das Verzögern von Nachrichten wirkt sich nicht auf den Nachrichtenablauf aus. Das bedeutet, dass verzögerte Nachrichten weiterhin zur ursprünglich geplanten Zeit ablaufen und in eine Warteschlange für unzustellbare Nachrichten verschoben werden, falls dies konfiguriert ist.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Service Bus Messaging finden Sie in folgenden Themen:

* [Service Bus – Grundlagen](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus-Warteschlangen, -Themen und -Abonnements](service-bus-queues-topics-subscriptions.md)
* [Erste Schritte mit Service Bus-Warteschlangen](service-bus-dotnet-get-started-with-queues.md)
* [Verwenden von Service Bus-Themen und -Abonnements](service-bus-dotnet-how-to-use-topics-subscriptions.md)