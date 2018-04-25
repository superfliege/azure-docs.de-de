---
title: Azure CLI-Skriptbeispiel – Abonnieren eines Blobspeicherkontos | Microsoft-Dokumentation
description: Azure CLI-Skriptbeispiel – Abonnieren eines Blobspeicherkontos
services: event-grid
documentationcenter: na
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/05/2018
ms.author: tomfitz
ms.openlocfilehash: 434d94b9ba2c06c85c84c17be68d8493e8344d2b
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="subscribe-to-events-for-a-blob-storage-account-with-azure-cli"></a>Abonnieren von Ereignissen für ein Blobspeicherkonto mit der Azure CLI

Dieses Skript erstellt ein Event Grid-Abonnement für die Ereignisse für ein Blobspeicherkonto. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/event-grid/subscribe-to-blob-storage/subscribe-to-blob-storage.sh "Subscribe to Blob storage")]

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet den folgenden Befehl zum Erstellen des Ereignisabonnements. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az eventgrid event-subscription create](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription#az-eventgrid-event-subscription-create) | Erstellen Sie ein Event Grid-Abonnement. |


## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Abfragen von Abonnements finden Sie unter [Abfragen von Event Grid-Abonnements](../query-event-subscriptions.md).
* Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).
