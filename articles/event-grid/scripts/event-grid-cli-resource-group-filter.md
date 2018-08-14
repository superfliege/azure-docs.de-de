---
title: 'Azure CLI-Skriptbeispiel: Abonnieren einer Ressourcengruppe und Filtern nach Ressource | Microsoft-Dokumentation'
description: 'Azure CLI-Skriptbeispiel: Abonnieren einer Ressourcengruppe und Filtern nach Ressource'
services: event-grid
documentationcenter: na
author: tfitzmac
ms.service: event-grid
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/02/2018
ms.author: tomfitz
ms.openlocfilehash: 4c3ac2eed6d7304be136fb20e7d9b8322658159a
ms.sourcegitcommit: fc5555a0250e3ef4914b077e017d30185b4a27e6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/03/2018
ms.locfileid: "39482565"
---
# <a name="subscribe-to-events-for-a-resource-group-and-filter-for-a-resource-with-azure-cli"></a>Abonnieren von Ereignissen für eine Ressourcengruppe und Filtern nach einer Ressource mit der Azure CLI

Dieses Skript erstellt ein Event Grid-Abonnement für die Ereignisse für eine Ressourcengruppe. Es verwendet einen Filter, um nur Ereignisse für eine angegebene Ressource in der Ressourcengruppe abzurufen.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

```azurecli
#!/bin/bash

# Provide an endpoint for handling the events.
myEndpoint="<endpoint URL>"

# Select the Azure subscription that contains the resource group.
az account set --subscription "Contoso Subscription"

# Get the resource ID to filter events
resourceId=$(az resource show --name demoSecurityGroup --resource-group myResourceGroup --resource-type Microsoft.Network/networkSecurityGroups --query id --output tsv)

# Subscribe to the resource group. Provide the name of the resource group you want to subscribe to.
az eventgrid event-subscription create \
  --name demoSubscriptionToResourceGroup \
  --resource-group myResourceGroup \
  --endpoint $myEndpoint \
  --subject-begins-with $resourceId
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet den folgenden Befehl, um das Ereignisabonnement zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az eventgrid event-subscription create](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription#az-eventgrid-event-subscription-create) | Erstellen Sie ein Event Grid-Abonnement. |


## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Abfragen von Abonnements finden Sie unter [Abfragen von Event Grid-Abonnements](../query-event-subscriptions.md).
* Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).
