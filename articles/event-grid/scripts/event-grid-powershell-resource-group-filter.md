---
title: 'Azure PowerShell-Skriptbeispiel: Abonnieren einer Ressourcengruppe und Filtern nach Ressource | Microsoft-Dokumentation'
description: 'Azure PowerShell-Skriptbeispiel: Abonnieren einer Ressourcengruppe und Filtern nach Ressource'
services: event-grid
documentationcenter: na
author: tfitzmac
ms.service: event-grid
ms.devlang: powershell
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/02/2018
ms.author: tomfitz
ms.openlocfilehash: ed77e8f09af841a1414212d7df6b60655ac158cd
ms.sourcegitcommit: fc5555a0250e3ef4914b077e017d30185b4a27e6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/03/2018
ms.locfileid: "39482582"
---
# <a name="subscribe-to-events-for-a-resource-group-and-filter-for-a-resource-with-powershell"></a>Abonnieren von Ereignissen für eine Ressourcengruppe und Filtern nach einer Ressource mit PowerShell

Dieses Skript erstellt ein Event Grid-Abonnement für die Ereignisse für eine Ressourcengruppe. Es verwendet einen Filter, um nur Ereignisse für eine angegebene Ressource in der Ressourcengruppe abzurufen.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

```powershell
# Provide an endpoint for handling the events.
$myEndpoint = "<endpoint URL>"

# Select the Azure subscription that contains the resource group.
Set-AzureRmContext -Subscription "Contoso Subscription"

# Get the resource ID to filter events
$resourceId = (Get-AzureRmResource -ResourceName demoSecurityGroup -ResourceGroupName myResourceGroup).ResourceId

# Subscribe to the resource group. Provide the name of the resource group you want to subscribe to.
New-AzureRmEventGridSubscription `
  -Endpoint $myEndpoint `
  -EventSubscriptionName demoSubscriptionToResourceGroup `
  -ResourceGroupName myResourceGroup `
  -SubjectBeginsWith $resourceId
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet den folgenden Befehl, um das Ereignisabonnement zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [New-AzureRmEventGridSubscription](https://docs.microsoft.com/powershell/module/azurerm.eventgrid/new-azurermeventgridsubscription) | Erstellen Sie ein Event Grid-Abonnement. |

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung in verwaltete Anwendungen finden Sie in der [Übersicht über verwaltete Azure-Anwendungen](../overview.md).
* Weitere Informationen zu PowerShell finden Sie in der [Azure PowerShell-Dokumentation](https://docs.microsoft.com/powershell/azure/get-started-azureps).
