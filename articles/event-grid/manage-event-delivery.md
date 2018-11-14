---
title: Richtlinien für unzustellbare Nachrichten und Wiederholungen für Azure Event Grid-Abonnements
description: Hier erfahren Sie, wie Sie Ereignisübermittlungsoptionen für Event Grid anpassen. Legen Sie ein Ziel für unzustellbare Nachrichten fest, und geben Sie an, wie lange die Übermittlung wiederholt wird.
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: tomfitz
ms.openlocfilehash: 0a89a315f9c97f3cc6a8683f13c22b5066dc5dab
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2018
ms.locfileid: "51277748"
---
# <a name="dead-letter-and-retry-policies"></a>Richtlinien für unzustellbare Nachrichten und Wiederholungen

Bei der Erstellung von Ereignisabonnements können Sie die Einstellungen für die Ereignisübermittlung anpassen. In diesem Artikel wird erläutert, wie Sie einen Speicherort für unzustellbare Nachrichten einrichten und die Wiederholungseinstellungen anpassen. Informationen zu diesen Features finden Sie unter [Event Grid – Übermittlung und Wiederholung von Nachrichten](delivery-and-retry.md).

## <a name="install-preview-feature"></a>Installieren des Vorschaufeatures

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="set-dead-letter-location"></a>Festlegen eines Speicherorts für unzustellbare Nachrichten

Zum Festlegen eines Speicherorts für unzustellbare Nachrichten benötigen Sie ein Speicherkonto für Ereignisse, die nicht an einen Endpunkt übermittelt werden können. Die Beispiele erhalten die Ressourcen-ID eines vorhandenen Speicherkontos. Sie erstellen ein Ereignisabonnement, das einen Container in diesem Speicherkonto als Endpunkt für unzustellbare Nachrichten verwendet.

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

```azurecli-interactive
# If you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

containername=testcontainer

topicid=$(az eventgrid topic show --name demoTopic -g gridResourceGroup --query id --output tsv)
storageid=$(az storage account show --name demoStorage --resource-group gridResourceGroup --query id --output tsv)

az eventgrid event-subscription create \
  --source-resource-id $topicid \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL> \
  --deadletter-endpoint $storageid/blobServices/default/containers/$containername
```

Um unzustellbare Nachrichten zu deaktivieren, führen Sie den Befehl zum Erstellen des Ereignisabonnements erneut aus, aber geben Sie keinen Wert für `deadletter-endpoint` an. Sie müssen das Ereignisabonnement nicht löschen.

### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
# If you have not already installed the module, do it now.
# This module is required for preview features.
Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery

$containername = "testcontainer"

$topicid = (Get-AzureRmEventGridTopic -ResourceGroupName gridResourceGroup -Name demoTopic).Id
$storageid = (Get-AzureRmStorageAccount -ResourceGroupName gridResourceGroup -Name demostorage).Id

New-AzureRmEventGridSubscription `
  -ResourceId $topicid `
  -EventSubscriptionName <event_subscription_name> `
  -Endpoint <endpoint_URL> `
  -DeadLetterEndpoint "$storageid/blobServices/default/containers/$containername"
```

Um unzustellbare Nachrichten zu deaktivieren, führen Sie den Befehl zum Erstellen des Ereignisabonnements erneut aus, aber geben Sie keinen Wert für `DeadLetterEndpoint` an. Sie müssen das Ereignisabonnement nicht löschen.

## <a name="set-retry-policy"></a>Festlegen der Wiederholungsrichtlinie

Bei der Erstellung eines Event Grid-Abonnements können Sie festlegen, wie lange Event Grid versuchen soll, das Ereignis zu übermitteln. Standardmäßig versucht es Event Grid für 24 Stunden (1.440 Minuten) oder 30-mal. Diese Werte können Sie für Ihr Event Grid-Abonnement festlegen. Der Wert für die Ereignisgültigkeitsdauer muss eine ganze Zahl zwischen 1 und 1440 sein. Der Wert für die maximalen Versuche muss ein Integer zwischen 1 und 30 sein.

Sie können den [Wiederholungszeitplan](delivery-and-retry.md#retry-schedule-and-duration) nicht konfigurieren.

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

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

Um die maximale Anzahl von Wiederholungsversuchen auf einen anderen Wert als 30 festzulegen, verwenden Sie Folgendes:

```azurecli-interactive
az eventgrid event-subscription create \
  -g gridResourceGroup \
  --topic-name <topic_name> \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL> \
  --max-delivery-attempts 18
```

Wenn Sie sowohl `event-ttl` als auch `max-deliver-attempts` festlegen, verwendet Event Grid den zuerst ablaufenden Wert, um zu ermitteln, wann die Ereignisübermittlung beendet werden soll.

### <a name="powershell"></a>PowerShell

So legen Sie die auf 1440 Minuten festgelegte Ereignislebensdauer auf einen anderen Wert fest:

```azurepowershell-interactive
# If you have not already installed the module, do it now.
# This module is required for preview features.
Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery

$topicid = (Get-AzureRmEventGridTopic -ResourceGroupName gridResourceGroup -Name demoTopic).Id

New-AzureRmEventGridSubscription `
  -ResourceId $topicid `
  -EventSubscriptionName <event_subscription_name> `
  -Endpoint <endpoint_URL> `
  -EventTtl 720
```

Um die maximale Anzahl von Wiederholungsversuchen auf einen anderen Wert als 30 festzulegen, verwenden Sie Folgendes:

```azurepowershell-interactive
$topicid = (Get-AzureRmEventGridTopic -ResourceGroupName gridResourceGroup -Name demoTopic).Id

New-AzureRmEventGridSubscription `
  -ResourceId $topicid `
  -EventSubscriptionName <event_subscription_name> `
  -Endpoint <endpoint_URL> `
  -MaxDeliveryAttempt 18
```

Wenn Sie sowohl `EventTtl` als auch `MaxDeliveryAttempt` festlegen, verwendet Event Grid den zuerst ablaufenden Wert, um zu ermitteln, wann die Ereignisübermittlung beendet werden soll.

## <a name="next-steps"></a>Nächste Schritte

* Eine Beispielanwendung, die eine Azure-Funktions-App zum Verarbeiten von Ereignissen im Zusammenhang mit unzustellbaren Nachrichten verwendet, finden Sie unter [Azure Event Grid Dead Letter Samples for .NET](https://azure.microsoft.com/resources/samples/event-grid-dotnet-handle-deadlettered-events/) (Azure Event Grid-Beispiele für unzustellbare Nachrichten für .NET).
* Informationen zu Ereignisübermittlungen und Wiederholungen finden Sie unter [Event Grid – Übermittlung und Wiederholung von Nachrichten](delivery-and-retry.md).
* Eine Einführung in Event Grid finden Sie unter [Informationen zu Event Grid](overview.md).
* Um sich schnell mit der Verwendung von Event Grid vertraut zu machen, lesen Sie [Erstellen und Weiterleiten benutzerdefinierter Ereignisse mit Azure Event Grid](custom-event-quickstart.md).
