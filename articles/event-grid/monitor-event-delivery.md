---
title: "Überwachen der Azure Event Grid-Nachrichtenübermittlung"
description: "Beschreibt, wie die Übermittlung von Azure Event Grid-Nachrichten überwacht wird."
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 01/10/2018
ms.author: tomfitz
ms.openlocfilehash: 1552174589e91c370c3b85a9af7b5cc1106cbb90
ms.sourcegitcommit: 71fa59e97b01b65f25bcae318d834358fea5224a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2018
---
# <a name="monitor-event-grid-message-delivery"></a>Überwachen der Event Grid-Nachrichtenübermittlung 

Dieser Artikel beschreibt, wie Sie im Portal den Status von Ereignisübermittlungen einsehen können.

Event Grid bietet permanente Übermittlung. Jede Nachricht wird für jedes Abonnement mindestens einmal übermittelt. Ereignisse werden sofort an den registrierten Webhook des jeweiligen Abonnements gesendet. Wenn ein Webhook den Eingang eines Ereignisses nicht innerhalb von 60 Sekunden nach dem ersten Übermittlungsversuch bestätigt, wiederholt Event Grid die Übermittlung des Ereignisses.

Informationen zu Ereignisübermittlungen und Wiederholungen finden Sie unter [Event Grid – Übermittlung und Wiederholung von Nachrichten](delivery-and-retry.md).

## <a name="delivery-metrics"></a>Übermittlungsmetriken

Das Portal zeigt Metriken für den Status der Übermittlung von Ereignisnachrichten an.

Für Themen gibt es diese Metriken:

* **Veröffentlichen erfolgreich**: Das Ereignis wurde erfolgreich an das Thema gesendet und mit einer Antwort des Typs 2xx verarbeitet.
* **Fehler beim Veröffentlichen**: Das Ereignis wurde an das Thema gesendet, aber mit einem Fehlercode abgelehnt.
* **Nicht übereinstimmende**: Das Ereignis wurde erfolgreich im Thema veröffentlicht, stimmt aber mit keinem Ereignisabonnement überein. Das Ereignis wurde gelöscht.

Für Abonnements gibt es diese Metriken:

* **Übermittlung erfolgreich**: Das Ereignis wurde erfolgreich an den Endpunkt des Abonnements übermittelt und erhielt eine Antwort des Typs 2xx.
* **Übermittlungsfehler**: Das Ereignis wurde erfolgreich an den Endpunkt des Abonnements übermittelt, erhielt aber eine Antwort des Typs 4xx oder 5xx.
* **Abgelaufene Ereignisse**: Das Ereignis wurde nicht übermittelt, und alle Wiederholungsversuche wurden gesendet. Das Ereignis wurde gelöscht.
* **Übereinstimmende Ereignisse**: Das Ereignis im Thema stimmt mit dem Ereignisabonnement überein.

## <a name="event-subscription-status"></a>Status des Ereignisabonnements

Um Metriken für ein Ereignisabonnement anzuzeigen, suchen Sie in den verfügbaren Diensten nach **Event Grid- Abonnements**, und wählen Sie diesen aus.

![Suchen nach Ereignisabonnements](./media/monitor-event-delivery/select-event-subscriptions.png)

Filtern Sie nach dem Typ des Ereignisses, des Abonnements und des Standorts. Wählen Sie **Metriken** für das Abonnement aus, um diese anzuzeigen.

![Filtern von Ereignisabonnements](./media/monitor-event-delivery/filter-events.png)

Zeigen Sie die Metriken für das Ereignisthema und Abonnement an.

![Anzeigen von Ereignismetriken](./media/monitor-event-delivery/subscription-metrics.png)

## <a name="custom-event-status"></a>Benutzerdefinierter Ereignisstatus

Wenn Sie ein benutzerdefiniertes Thema veröffentlicht haben, können Sie die Metriken dafür anzeigen. Wählen Sie die Ressourcengruppe, die das Thema enthält, und dann das Thema aus.

![Auswählen eines benutzerdefinierten Themas](./media/monitor-event-delivery/select-custom-topic.png)

Zeigen Sie die Metriken für das benutzerdefinierte Ereignisthema an.

![Anzeigen von Ereignismetriken](./media/monitor-event-delivery/custom-topic-metrics.png)

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu Ereignisübermittlungen und Wiederholungen finden Sie unter [Event Grid – Übermittlung und Wiederholung von Nachrichten](delivery-and-retry.md).
* Eine Einführung in Event Grid finden Sie unter [Informationen zu Event Grid](overview.md).
* Um sich schnell mit der Verwendung von Event Grid vertraut zu machen, lesen Sie [Erstellen und Weiterleiten benutzerdefinierter Ereignisse mit Azure Event Grid](custom-event-quickstart.md).
