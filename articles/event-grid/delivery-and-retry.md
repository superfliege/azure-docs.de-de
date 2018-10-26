---
title: Azure Event Grid – Übermittlung und Wiederholung
description: Beschreibt, wie Azure Event Grid Ereignisse übermittelt und wie nicht übermittelte Nachrichten verarbeitet werden.
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: tomfitz
ms.openlocfilehash: 4d53c33daefaadb4c58ce500a5d564af7988b606
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "49077087"
---
# <a name="event-grid-message-delivery-and-retry"></a>Event Grid – Übermittlung und Wiederholung von Nachrichten

In diesem Artikel wird beschrieben, wie Azure Event Grid Ereignisse verarbeitet, wenn die Übermittlung nicht bestätigt wird.

Event Grid bietet permanente Übermittlung. Jede Nachricht wird für jedes Abonnement mindestens einmal übermittelt. Ereignisse werden sofort an den registrierten Endpunkt des jeweiligen Abonnements gesendet. Wenn ein Endpunkt den Eingang eines Ereignisses nicht bestätigt, wiederholt Event Grid die Übermittlung des Ereignisses.

Derzeit sendet Event Grid jedes Ereignis einzeln an Abonnenten. Der Abonnent empfängt ein Array mit einem einzelnen Ereignis.

## <a name="retry-schedule-and-duration"></a>Wiederholungszeitplan und Dauer

Event Grid verwendet exponentiell ansteigende Wartezeiten für Wiederholungsversuche für die Ereignisübermittlung. Wenn ein Endpunkt nicht antwortet oder einen Fehlercode zurückgibt, wiederholt Event Grid die Übermittlung nach folgendem Zeitplan:

1. 10 Sekunden
2. 30 Sekunden
3. 1 Minute
4. 5 Minuten
5. 10 Minuten
6. 30 Minuten
7. 1 Stunde

Event Grid fügt allen Wiederholungsschritten eine geringfügige zufällige Anordnung hinzu. Nach einer Stunde wird die Ereignisübermittlung erneut versucht. Der Versuch wird jede Stunde wiederholt.

Event Grid markiert alle Ereignisse standardmäßig als abgelaufen, die nicht innerhalb von 24 Stunden übermittelt werden. Bei der Erstellung von Ereignisabonnements können Sie die [Wiederholungsrichtlinie anpassen](manage-event-delivery.md). Sie geben die maximale Anzahl von Zustellversuchen (Standardwert 30) und die Gültigkeitsdauer des Ereignisses (Standardwert 1.440 Minuten) ein.

## <a name="dead-letter-events"></a>„Unzustellbare Nachrichten“-Ereignisse

Wenn ein Ereignis nicht übermittelt werden kann, kann Event Grid das nicht übermittelte Ereignis an ein Speicherkonto senden. Dieser Prozess wird als Speicherung unzustellbarer Nachrichten bezeichnet. Die Speicherung unzustellbarer Nachrichten ist standardmäßig nicht aktiviert. Wenn Sie das Feature aktivieren möchten, müssen Sie bei der Erstellung des Ereignisabonnements ein Speicherkonto zum Speichern nicht übermittelter Ereignisse angeben. Ereignisse werden aus diesem Speicherkonto gepullt, um Übermittlungsprobleme zu beheben.

Event Grid sendet ein Ereignis an den Speicherort für unzustellbare Nachrichten, wenn alle Wiederholungsversuche ausgeführt wurden. Wenn Event Grid den Antwortcode 400 (Ungültige Anforderung) oder 413 (Anforderungsentität zu groß) erhält, sendet der Dienst das Ereignis sofort an den Endpunkt für unzustellbare Nachrichten. Diese Antwortcodes geben an, dass die Übermittlung des Ereignisses nie erfolgreich ausgeführt wird.

Es gibt eine fünfminütige Verzögerung zwischen dem letzten Versuch, ein Ereignis zu übermitteln, und der Übermittlung an den Speicherort für unzustellbare Nachrichten. Diese Verzögerung dient dazu, die Anzahl der Blob Storage-Vorgänge zu reduzieren. Wenn der Speicherort für unzustellbare Nachrichten vier Stunden lang nicht verfügbar ist, wird das Ereignis gelöscht.

Wenn Sie den Speicherort für unzustellbare Nachrichten festlegen möchten, benötigen Sie ein Speicherkonto mit einem Container. Der Endpunkt für diesen Container wird bei der Erstellung des Ereignisabonnements angegeben. (Format des Endpunkts: `/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<storage-name>/blobServices/default/containers/<container-name>`)

Es wird empfohlen, sich benachrichtigen zu lassen, wenn ein Ereignis an den Speicherort für unzustellbare Nachrichten gesendet wurde. Wenn Event Grid auf nicht übermittelte Ereignisse reagieren soll, [erstellen Sie ein Ereignisabonnement](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json) für den Blobspeicher für unzustellbare Nachrichten. Sobald bei Ihrem Blobspeicher für unzustellbare Nachrichten ein nicht übermitteltes Ereignis eingeht, informiert Event Grid Ihren Handler. Der Handler reagiert mit den gewünschten Aktionen zur Abstimmung nicht übermittelter Ereignisse.

Ein Beispiel für das Einrichten eines Speicherorts für unzustellbare Nachrichten finden Sie unter [Unzustellbare Nachrichten und Wiederholungsrichtlinien](manage-event-delivery.md).

## <a name="message-delivery-status"></a>Nachrichtenübermittlungsstatus

Event Grid verwendet HTTP-Antwortcodes zum Bestätigen des Eingangs von Ereignissen. 

### <a name="success-codes"></a>Erfolgscodes

Die folgenden HTTP-Antwortcodes geben an, dass ein Ereignis erfolgreich an den Webhook übermittelt wurde. Die Übermittlung gilt dann in Event Grid als abgeschlossen.

- 200 – OK
- 202 – Akzeptiert

### <a name="failure-codes"></a>Fehlercodes

Die folgenden HTTP-Antwortcodes geben an, dass bei der Übermittlung eines Ereignisses ein Fehler aufgetreten ist.

- 400 – Ungültige Anforderung
- 401 – Nicht autorisiert
- 404 – Nicht gefunden
- 408 – Anforderungstimeout
- 413 – Anforderungsentität zu groß
- 414 – URI zu lang
- 429 – Zu viele Anforderungen
- 500 Interner Serverfehler
- 503 Dienst nicht verfügbar
- 504 Gateway-Timeout

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Anzeigen des Status von Ereignisübermittlungen finden Sie unter [Überwachen der Event Grid-Nachrichtenübermittlung](monitor-event-delivery.md).
* Um Optionen für die Ereignisübermittlung anzupassen, lesen Sie [Unzustellbare Nachrichten und Wiederholungsrichtlinien](manage-event-delivery.md).