---
title: Richtlinien für unzustellbare Nachrichten und Wiederholungen für Azure Event Grid-Abonnements
description: Hier erfahren Sie, wie Sie Ereignisübermittlungsoptionen für Event Grid anpassen. Legen Sie ein Ziel für unzustellbare Nachrichten fest, und geben Sie an, wie lange die Übermittlung wiederholt wird.
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 08/03/2018
ms.author: tomfitz
ms.openlocfilehash: 5a37fadc179157ba590b31a79fcd98f223cb1869
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/03/2018
ms.locfileid: "39501948"
---
# <a name="dead-letter-and-retry-policies"></a>Richtlinien für unzustellbare Nachrichten und Wiederholungen

Bei der Erstellung von Ereignisabonnements können Sie die Einstellungen für die Ereignisübermittlung anpassen. Sie können festlegen, wie lange Event Grid versuchen soll, die Nachricht zu übermitteln. Außerdem können Sie ein Speicherkonto zum Speichern von Ereignissen festlegen, die nicht einen Endpunkt übermittelt werden können.

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="set-dead-letter-location"></a>Festlegen eines Speicherorts für unzustellbare Nachrichten

Wenn ein Ereignis nicht übermittelt werden kann, kann Event Grid das nicht übermittelte Ereignis an ein Speicherkonto senden. Dieser Prozess wird als Speicherung unzustellbarer Nachrichten bezeichnet. Die Speicherung unzustellbarer Nachrichten ist standardmäßig nicht aktiviert. Wenn Sie das Feature aktivieren möchten, müssen Sie bei der Erstellung des Ereignisabonnements ein Speicherkonto zum Speichern nicht übermittelter Ereignisse angeben. Ereignisse werden aus diesem Speicherkonto gepullt, um Übermittlungsprobleme zu beheben.

Event Grid sendet ein Ereignis an den Speicherort für unzustellbare Nachrichten, wenn alle konfigurierten Wiederholungsversuche unternommen wurden oder eine Fehlermeldung empfangen wurde, die angibt, dass die Übermittlung keine Aussicht auf Erfolg hat. Wenn Event Grid bei der Ereignisübermittlung also beispielsweise einen Formatfehler empfängt, wird ein Ereignis an den Speicherort für unzustellbare Nachrichten gesendet. Es gibt eine fünfminütige Verzögerung zwischen dem letzten Versuch, ein Ereignis zu übermitteln, und der Übermittlung an den Speicherort für unzustellbare Nachrichten. Diese Verzögerung dient dazu, die Anzahl der Blob Storage-Vorgänge zu reduzieren. Wenn der Speicherort für unzustellbare Nachrichten für vier Stunden nicht verfügbar ist, wird das Ereignis gelöscht.

Wenn Sie den Speicherort für unzustellbare Nachrichten festlegen möchten, benötigen Sie ein Speicherkonto mit einem Container. Der Endpunkt für diesen Container wird bei der Erstellung des Ereignisabonnements angegeben. (Format des Endpunkts: `/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<storage-name>/blobServices/default/containers/<container-name>`)

Das folgende Skript ruft die Ressourcen-ID eines vorhandenen Speicherkontos ab und erstellt ein Ereignisabonnement, das einen Container in diesem Speicherkonto als Endpunkt für unzustellbare Nachrichten verwendet.

```azurecli-interactive
# if you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

storagename=demostorage
containername=testcontainer

storageid=$(az storage account show --name $storagename --resource-group gridResourceGroup --query id --output tsv)

az eventgrid event-subscription create \
  -g gridResourceGroup \
  --topic-name <topic_name> \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL> \
  --deadletter-endpoint $storageid/blobServices/default/containers/$containername
```

Wenn Event Grid auf nicht übermittelte Ereignisse reagieren soll, [erstellen Sie ein Ereignisabonnement](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json) für den Blobspeicher für unzustellbare Nachrichten. Sobald bei Ihrem Blobspeicher für unzustellbare Nachrichten ein nicht übermitteltes Ereignis eingeht, informiert Event Grid Ihren Handler. Der Handler reagiert mit den gewünschten Aktionen zur Abstimmung nicht übermittelter Ereignisse. 

Um unzustellbare Nachrichten zu deaktivieren, führen Sie den Befehl zum Erstellen des Ereignisabonnements erneut aus, aber geben Sie keinen Wert für `deadletter-endpoint` an. Sie müssen das Ereignisabonnement nicht löschen.

## <a name="set-retry-policy"></a>Festlegen der Wiederholungsrichtlinie

Bei der Erstellung eines Event Grid-Abonnements können Sie festlegen, wie lange Event Grid versuchen soll, das Ereignis zu übermitteln. Standardmäßig ist der Zeitraum für Event Grid auf 24 Stunden (1440 Minuten) festgelegt, und es werden bis zu 30 Versuche unternommen. Diese Werte können Sie für Ihr Event Grid-Abonnement festlegen. Der Wert für die Ereignisgültigkeitsdauer muss eine ganze Zahl zwischen 1 und 1440 sein. Der Wert für die maximalen Übermittlungsversuche muss eine ganze Zahl zwischen 1 und 30 sein.

Sie können das [Wiederholungsintervall](delivery-and-retry.md#retry-intervals-and-duration) nicht konfigurieren.

So legen Sie die auf 1440 Minuten festgelegte Ereignislebensdauer auf einen anderen Wert fest:

```azurecli-interactive
# if you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid event-subscription create \
  -g gridResourceGroup \
  --topic-name <topic_name> \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL> \
  --event-ttl 720
```

So legen Sie die auf maximal 30 Versuche festgelegte Anzahl von Wiederholungsversuchen auf einen anderen Wert fest:

```azurecli-interactive
az eventgrid event-subscription create \
  -g gridResourceGroup \
  --topic-name <topic_name> \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL> \
  --max-delivery-attempts 18
```

Wenn Sie für die Wiederholungsversuche sowohl `event-ttl` als auch `max-deliver-attempts` festlegen, verwendet Event Grid die Option, die zuerst abläuft.

## <a name="next-steps"></a>Nächste Schritte

* Eine Beispielanwendung, die eine Azure-Funktions-App zum Verarbeiten von Ereignissen im Zusammenhang mit unzustellbaren Nachrichten verwendet, finden Sie unter [Azure Event Grid Dead Letter Samples for .NET](https://azure.microsoft.com/resources/samples/event-grid-dotnet-handle-deadlettered-events/) (Azure Event Grid-Beispiele für unzustellbare Nachrichten für .NET).
* Informationen zu Ereignisübermittlungen und Wiederholungen finden Sie unter [Event Grid – Übermittlung und Wiederholung von Nachrichten](delivery-and-retry.md).
* Eine Einführung in Event Grid finden Sie unter [Informationen zu Event Grid](overview.md).
* Um sich schnell mit der Verwendung von Event Grid vertraut zu machen, lesen Sie [Erstellen und Weiterleiten benutzerdefinierter Ereignisse mit Azure Event Grid](custom-event-quickstart.md).
