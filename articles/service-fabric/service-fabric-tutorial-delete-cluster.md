---
title: Löschen eines Service Fabric-Clusters in Azure | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie einen in Azure gehosteten Service Fabric-Cluster und alle zugehörigen Ressourcen löschen. Sie können die Ressourcengruppe mit dem Cluster oder selektiv einzelne Ressourcen löschen.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/26/2018
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 5174bb696b456a66cfc0666ac5c475437e441d82
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2019
ms.locfileid: "54848851"
---
# <a name="tutorial-remove-a-service-fabric-cluster-running-in-azure"></a>Tutorial: Entfernen eines in Azure ausgeführten Service Fabric-Clusters

Dieses Tutorial ist der vierte Teil einer Tutorialreihe und veranschaulicht, wie Sie einen in Azure ausgeführten Service Fabric-Cluster löschen. Um einen Service Fabric-Cluster vollständig zu löschen, müssen Sie die Ressourcen löschen, die vom Cluster verwendet werden. Dazu stehen Ihnen zwei Möglichkeiten zur Verfügung: Sie können die Ressourcengruppe löschen, in der sich der Cluster befindet (dabei werden die Clusterressource und alle weiteren Ressourcen in der Ressourcengruppe gelöscht). Alternativ können Sie speziell die Clusterressource und die dazugehörigen Ressourcen löschen (jedoch keine anderen Ressourcen in der Ressourcengruppe).

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Löschen einer Ressourcengruppe und aller zugehörigen Ressourcen
> * Selektives Löschen von Ressourcen aus einer Ressourcengruppe

In dieser Tutorialserie lernen Sie Folgendes:
> [!div class="checklist"]
> * Erstellen eines sicheren [Windows-Clusters](service-fabric-tutorial-create-vnet-and-windows-cluster.md) oder [Linux-Clusters](service-fabric-tutorial-create-vnet-and-linux-cluster.md) in Azure mithilfe einer Vorlage
> * [Horizontales Herunter- oder Hochskalieren eines Clusters](service-fabric-tutorial-scale-cluster.md)
> * [Aktualisieren der Runtime eines Clusters](service-fabric-tutorial-upgrade-cluster.md)
> * Löschen eines Clusters

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit diesem Tutorial beginnen können, müssen Sie Folgendes tun:

* Wenn Sie kein Azure-Abonnement besitzen, erstellen Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Installieren Sie das [Azure PowerShell-Modul Version 4.1 oder höher](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps) oder [Azure CLI](/cli/azure/install-azure-cli).
* Erstellen Sie einen sicheren [Windows-Cluster](service-fabric-tutorial-create-vnet-and-windows-cluster.md) oder [Linux-Cluster](service-fabric-tutorial-create-vnet-and-linux-cluster.md) in Azure.

## <a name="delete-the-resource-group-containing-the-service-fabric-cluster"></a>Löschen der Ressourcengruppe, die den Service Fabric-Cluster enthält
Die einfachste Möglichkeit zum Löschen des Clusters und aller darin genutzten Ressourcen besteht darin, die Ressourcengruppe zu löschen.

Melden Sie sich bei Azure an, und wählen Sie die Abonnement-ID aus, mit der Sie den Cluster entfernen möchten.  Sie finden Ihre Abonnement-ID, indem Sie sich beim [Azure-Portal](http://portal.azure.com) anmelden. Löschen Sie die Ressourcengruppe und alle Clusterressourcen mithilfe des Cmdlets [Remove-AzureRMResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) oder des Befehls [az group delete](/cli/azure/group?view=azure-cli-latest).

```powershell
Connect-AzureRmAccount
Set-AzureRmContext -SubscriptionId <guid>
$groupname = "sfclustertutorialgroup"
Remove-AzureRmResourceGroup -Name $groupname -Force
```

```azurecli
az login
az account set --subscription <guid>
ResourceGroupName="sfclustertutorialgroup"
az group delete --name $ResourceGroupName
```

## <a name="selectively-delete-the-cluster-resource-and-the-associated-resources"></a>Selektives Löschen der Clusterressource und der zugehörigen Ressourcen
Sind in der Ressourcengruppe nur Ressourcen enthalten, die mit dem Service Fabric-Cluster verknüpft sind, den Sie löschen möchten, ist es einfacher, die gesamte Ressourcengruppe zu löschen. Wenn Sie die Ressourcen in Ihrer Ressourcengruppe selektiv löschen und nicht zum Cluster gehörende Ressourcen beibehalten möchten, führen Sie die folgenden Schritte aus.

Listen Sie die Ressourcen in der Ressourcengruppe auf:

```powershell
Connect-AzureRmAccount
Set-AzureRmContext -SubscriptionId <guid>
$groupname = "sfclustertutorialgroup"
Get-AzureRmResource -ResourceGroupName $groupname | ft
```

```azurecli
az login
az account set --subscription <guid>
ResourceGroupName="sfclustertutorialgroup"
az resource list --resource-group $ResourceGroupName
```

Für jede Ressource, die Sie löschen möchten, führen Sie folgendes Skript aus:

```powershell
Remove-AzureRmResource -ResourceName "<name of the Resource>" -ResourceType "<Resource Type>" -ResourceGroupName $groupname -Force
```

```azurecli
az resource delete --name "<name of the Resource>" --resource-type "<Resource Type>" --resource-group $ResourceGroupName
```

Führen Sie folgendes Skript aus, um die Clusterressource zu löschen:

```powershell
Remove-AzureRmResource -ResourceName "<name of the Resource>" -ResourceType "Microsoft.ServiceFabric/clusters" -ResourceGroupName $groupname -Force
```

```azurecli
az resource delete --name "<name of the Resource>" --resource-type "Microsoft.ServiceFabric/clusters" --resource-group $ResourceGroupName
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Löschen einer Ressourcengruppe und aller zugehörigen Ressourcen
> * Selektives Löschen von Ressourcen aus einer Ressourcengruppe

Nachdem Sie dieses Tutorial abgeschlossen haben, fahren Sie mit Folgendem fort:
* Erfahren Sie, wie Sie einen Service Fabric-Cluster mithilfe von [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) untersuchen und verwalten.
* Erfahren Sie, wie Sie [Patches für das Windows-Betriebssystem](service-fabric-patch-orchestration-application.md) oder [Patches für das Linux-Betriebssystem](service-fabric-patch-orchestration-application-linux.md) der Clusterknoten aufspielen.
* Erfahren Sie, wie Sie Ereignisse für [Windows-Cluster](service-fabric-diagnostics-event-aggregation-wad.md) oder [Linux-Cluster](service-fabric-diagnostics-event-aggregation-lad.md) aggregieren und erfassen und [Log Analytics einrichten](service-fabric-diagnostics-oms-setup.md), um Clusterereignisse zu überwachen.
