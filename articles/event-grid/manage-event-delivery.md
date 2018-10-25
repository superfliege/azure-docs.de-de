---
title: Richtlinien für unzustellbare Nachrichten und Wiederholungen für Azure Event Grid-Abonnements
description: Hier erfahren Sie, wie Sie Ereignisübermittlungsoptionen für Event Grid anpassen. Legen Sie ein Ziel für unzustellbare Nachrichten fest, und geben Sie an, wie lange die Übermittlung wiederholt wird.
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: tomfitz
ms.openlocfilehash: fcf3ecaff6e8ba1421496a96d01428946cf8ab8e
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "49077781"
---
# <a name="dead-letter-and-retry-policies"></a>Richtlinien für unzustellbare Nachrichten und Wiederholungen

Bei der Erstellung von Ereignisabonnements können Sie die Einstellungen für die Ereignisübermittlung anpassen. In diesem Artikel wird erläutert, wie Sie einen Speicherort für unzustellbare Nachrichten einrichten und die Wiederholungseinstellungen anpassen. Informationen zu diesen Features finden Sie unter [Event Grid – Übermittlung und Wiederholung von Nachrichten](delivery-and-retry.md).

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="set-dead-letter-location"></a>Festlegen eines Speicherorts für unzustellbare Nachrichten

Zum Festlegen eines Speicherorts für unzustellbare Nachrichten benötigen Sie ein Speicherkonto für Ereignisse, die nicht an einen Endpunkt übermittelt werden können. Das folgende Skript ruft die Ressourcen-ID eines vorhandenen Speicherkontos ab und erstellt ein Ereignisabonnement, das einen Container in diesem Speicherkonto als Endpunkt für unzustellbare Nachrichten verwendet.

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

Um unzustellbare Nachrichten zu deaktivieren, führen Sie den Befehl zum Erstellen des Ereignisabonnements erneut aus, aber geben Sie keinen Wert für `deadletter-endpoint` an. Sie müssen das Ereignisabonnement nicht löschen.

## <a name="set-retry-policy"></a>Festlegen der Wiederholungsrichtlinie

Bei der Erstellung eines Event Grid-Abonnements können Sie festlegen, wie lange Event Grid versuchen soll, das Ereignis zu übermitteln. Standardmäßig ist der Zeitraum für Event Grid auf 24 Stunden (1440 Minuten) festgelegt, und es werden bis zu 30 Versuche unternommen. Diese Werte können Sie für Ihr Event Grid-Abonnement festlegen. Der Wert für die Ereignisgültigkeitsdauer muss eine ganze Zahl zwischen 1 und 1440 sein. Der Wert für die maximalen Übermittlungsversuche muss eine ganze Zahl zwischen 1 und 30 sein.

Sie können den [Wiederholungszeitplan](delivery-and-retry.md#retry-schedule-and-duration) nicht konfigurieren.

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
