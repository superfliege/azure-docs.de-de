---
title: Erkennung doppelter Nachrichten von Azure Service Bus | Microsoft-Dokumentation
description: Erkennen doppelter Nachrichten von Service Bus
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
ms.date: 09/27/2017
ms.author: sethm
ms.openlocfilehash: 470246469297d5fa95eba2b147d5304e74c0003f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="duplicate-detection"></a>Duplikaterkennung

Tritt bei einer Anwendung unmittelbar nach dem Senden einer Nachricht ein schwerwiegender Fehler auf, aufgrund dessen die neu gestartete Anwendungsinstanz fälschlicherweise davon ausgeht, dass die vorherige Nachrichtenübermittlung nicht stattgefunden hat, ist die Nachricht nach einem weiteren Sendevorgang zweimal im System enthalten.

Es ist auch möglich, dass kurz zuvor ein Fehler auf Client- oder Netzwerkebene aufgetreten ist, sodass eine gesendete Nachricht in die Warteschlange gestellt wird, ohne dass die Bestätigung erfolgreich an den Client zurückgesendet werden kann. In diesem Fall weiß der Client nicht sicher, welches Ergebnis der Sendevorgang hatte.

Durch die Erkennung von Duplikaten werden diese Situationen aufgelöst, indem der Absender die gleiche Nachricht erneut senden kann, während die Warteschlange oder das Thema mögliche Duplikate verwirft.

Das Aktivieren der Erkennung von Duplikaten unterstützt das Nachverfolgen der anwendungsgesteuerten *MessageId* aller in einem angegebenen Zeitfenster an eine Warteschlange oder ein Thema gesendeten Nachrichten. Wenn eine neue Nachricht mit einer *MessageId* gesendet wird, die bereits während des Zeitfensters erfasst wurde, wird die Nachricht als akzeptiert gemeldet (der Sendevorgang war erfolgreich). Die neu gesendete Nachricht wird jedoch sofort ignoriert und verworfen. Es werden keine anderen Teile der Nachricht außer der *MessageId* ausgewertet.

Die Anwendungssteuerung der ID ist wichtig, da nur diese es der Anwendung erlaubt, die *MessageId* einem Geschäftsprozesskontext zuzuordnen, aus dem sie bei einem Fehler vorhersagbar rekonstruiert werden kann.

Für einen Geschäftsprozess, bei dem für die Behandlung von Anwendungskontext mehrere Nachrichten gesendet werden, setzt sich die *MessageId* möglicherweise aus dem Kontextbezeichner auf Anwendungsebene (z.B. einer Bestellnummer) und dem Betreff der Nachricht (z.B. **12345.2017/Bezahlung**) zusammen.

Die *MessageId* kann immer auch eine GUID sein, aber das Verankern des Bezeichners in den Geschäftsprozess sorgt für eine vorhersagbare Wiederholbarkeit, die für die effektive Erkennung von Duplikaten gewünscht wird.

## <a name="enable-duplicate-detection"></a>Aktivieren der Duplikaterkennung

Im Portal wird das Feature während der Erstellung der Entität mit dem Kontrollkästchen **Duplikaterkennung aktivieren** aktiviert, das standardmäßig deaktiviert ist. Die Einstellung für das Erstellen neuer Themen funktioniert genauso.

![][1]

Programmgesteuert legen Sie das Flag mit der [QueueDescription.requiresDuplicateDetection](/dotnet/api/microsoft.servicebus.messaging.queuedescription.requiresduplicatedetection#Microsoft_ServiceBus_Messaging_QueueDescription_RequiresDuplicateDetection)-Eigenschaft im vollständigen Framework der .NET-API fest. Mit der Azure Resource Manager-API wird der Wert mit der [queueProperties.requiresDuplicateDetection](/azure/templates/microsoft.servicebus/namespaces/queues#property-values)-Eigenschaft festgelegt.

Die Zeit für den Verlauf der Duplikaterkennung beträgt für Warteschlangen und Themen standardmäßig 30 Sekunden. Der Höchstwert beträgt 40 Minuten. Sie können diese Einstellung im Eigenschaftenfenster für Warteschlangen und Themen im Azure-Portal ändern.

![][2]

Programmgesteuert können Sie die Dauer der Duplikaterkennung, während der Nachrichten-IDs beibehalten werden, mithilfe der [QueueDescription.DuplicateDetectionHistoryTimeWindow](/dotnet/api/microsoft.servicebus.messaging.queuedescription.duplicatedetectionhistorytimewindow#Microsoft_ServiceBus_Messaging_QueueDescription_DuplicateDetectionHistoryTimeWindow)-Eigenschaft im vollständigen Framework der .NET-API konfigurieren. Mit der Azure Resource Manager-API wird der Wert mit der [queueProperties.duplicateDetectionHistoryTimeWindow](/azure/templates/microsoft.servicebus/namespaces/queues#property-values)-Eigenschaft festgelegt.

Beachten Sie, dass das Aktivieren der Duplikaterkennung und die Größe des Zeitfensters direkte Auswirkungen auf den Durchsatz von Warteschlangen (und Themen) haben, da alle aufgezeichneten Nachrichten-IDs mit den neu übermittelten Nachrichten-IDs verglichen werden müssen.

Ein kürzeres Zeitfenster bedeutet, dass weniger Nachrichten-IDs beibehalten und verglichen werden müssen, sodass der Durchsatz weniger beeinträchtigt wird. Für Entitäten mit einem hohen Durchsatz, die eine Duplikaterkennung erfordern, sollten Sie das Zeitfenster so klein wie möglich halten.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Service Bus-Messaging finden Sie in folgenden Themen:

* [Service Bus – Grundlagen](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus-Warteschlangen, -Themen und -Abonnements](service-bus-queues-topics-subscriptions.md)
* [Erste Schritte mit Service Bus-Warteschlangen](service-bus-dotnet-get-started-with-queues.md)
* [Verwenden von Service Bus-Themen und -Abonnements](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[1]: ./media/duplicate-detection/create-queue.png
[2]: ./media/duplicate-detection/queue-prop.png