---
title: "Anhalten von Azure Service Bus-Messagingentitäten | Microsoft-Dokumentation"
description: "Anhalten und Reaktivieren von Azure Service Bus-Messagingentitäten"
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
ms.date: 10/03/2017
ms.author: sethm
ms.openlocfilehash: 09aee1ba9ec3ce72732cb1f60c9a840ffc4beb2f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="suspend-and-reactivate-messaging-entities-disable"></a>Anhalten und Reaktivieren von Messagingentitäten (deaktiviert)

Warteschlangen, Themen und Abonnements können vorübergehend angehalten werden. Das Anhalten versetzt die Entität in einen deaktivierten Zustand, in dem alle Nachrichten im Speicher beibehalten werden. Es können jedoch keine Nachrichten entfernt oder hinzugefügt werden, und die entsprechenden Protokollvorgänge führen zu Fehlern.

Das Anhalten einer Entität wird üblicherweise aus dringenden administrativen Gründen durchgeführt. Ein Szenario ist, dass ein fehlerhafter Empfänger bereitgestellt wurde, der Nachrichten aus der Warteschlange nimmt, diese fehlerhaft verarbeitet, falsch vervollständigt und schließlich entfernt. Wenn dieses Verhalten diagnostiziert wird, kann die Warteschlange für den Empfang deaktiviert werden, bis der korrigierte Code bereitgestellt ist, sodass weiterer Datenverlust durch den fehlerhaften Code verhindert werden kann.

Das Anhalten oder Reaktivieren kann entweder vom Benutzer oder vom System durchgeführt werden. Das System hält Entitäten nur aufgrund von schwerwiegenden administrativen Gründen an, z.B. das Erreichen des Ausgabenlimits des Abonnements. Vom System deaktivierte Entitäten können nicht vom Benutzer reaktiviert werden, sondern werden wiederhergestellt, wenn die Ursache des Anhaltens behandelt wurde.

Im Portal ermöglicht der Abschnitt **Eigenschaften** für die entsprechende Entität das Ändern des Zustands. Folgender Screenshot stellt das Umschalten einer Warteschlange dar:

![][1]

Das Portal ermöglicht nur das vollständige Deaktivieren von Warteschlangen. Sie können ebenfalls die Vorgänge „Senden“ und „Empfangen“ separat deaktivieren, indem Sie die Service Bus-API [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) im .NET Framework SDK oder eine Azure Resource Manager-Vorlage über die Azure-CLI oder Azure PowerShell verwenden.

## <a name="suspension-states"></a>Anhaltezustände

Folgende Zustände können für eine Warteschlange festgelegt werden:

-   **Active**: Die Warteschlange ist aktiv.
-   **Disabled**: Die Warteschlange wurde angehalten.
-   **SendDisabled**: Die Warteschlange ist teilweise angehalten, der Empfang ist zulässig.
-   **ReceiveDisabled**: Die Warteschlange ist teilweise angehalten, das Senden ist zulässig.

Für Abonnements und Themen können nur **Active** und **Disabled** festgelegt werden.

Die Enumeration [EntityStatus](/dotnet/api/microsoft.servicebus.messaging.entitystatus) definiert ebenfalls eine Reihe von Übergangszuständen, die nur vom System festgelegt werden können. Der PowerShell-Befehl zum Deaktivieren einer Warteschlange wird im folgenden Beispiel gezeigt. Der Befehl für die Reaktivierung ist identisch und legt `Status` auf **Active** fest.

```powershell
$q = Get-AzureRmServiceBusQueue -ResourceGroup mygrp -NamespaceName myns -QueueName myqueue

$q.Status = "Disabled"

Set-AzureRmServiceBusQueue -ResourceGroup mygrp -NamespaceName myns -QueueName myqueue -QueueObj $q
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Service Bus Messaging finden Sie in folgenden Themen:

* [Service Bus – Grundlagen](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus-Warteschlangen, -Themen und -Abonnements](service-bus-queues-topics-subscriptions.md)
* [Erste Schritte mit Service Bus-Warteschlangen](service-bus-dotnet-get-started-with-queues.md)
* [Verwenden von Service Bus-Themen und -Abonnements](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[1]: ./media/entity-suspend/queue-disable.png

