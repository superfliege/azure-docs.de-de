---
title: Verschieben einer Windows-VM-Ressource in Azure | Microsoft-Dokumentation
description: Verschieben Sie einen virtuellen Windows-Computer im Resource Manager-Bereitstellungsmodell in ein anderes Azure-Abonnement oder in eine andere Ressourcengruppe.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 4e383427-4aff-4bf3-a0f4-dbff5c6f0c81
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2018
ms.author: cynthn
ms.openlocfilehash: 1daf04e3f878d0748bfa0904259c7b7187481843
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/14/2018
ms.locfileid: "45580485"
---
# <a name="move-a-windows-vm-to-another-azure-subscription-or-resource-group"></a>Verschieben eines virtuellen Windows-Computers in ein anderes Azure-Abonnement oder in eine andere Ressourcengruppe
In diesem Artikel wird beschrieben, wie Sie einen virtuellen Windows-Computer (VM) zwischen Ressourcengruppen oder Abonnements verschieben. Das Verschieben zwischen Abonnements kann hilfreich sein, wenn Sie einen virtuellen Computer ursprünglich in einem persönlichen Abonnement erstellt haben und ihn nun in das Abonnement Ihres Unternehmens verschieben möchten, um weiterarbeiten zu können.

> [!IMPORTANT]
>Zum aktuellen Zeitpunkt können Azure Managed Disks nicht verschoben werden. 
>
>Im Rahmen der Verschiebung werden neue Ressourcen-IDs erstellt. Nachdem die VM verschoben wurde, müssen Sie Ihre Tools und Skripts aktualisieren, damit die neuen Ressourcen-IDs verwendet werden. 
> 
> 

[!INCLUDE [virtual-machines-common-move-vm](../../../includes/virtual-machines-common-move-vm.md)]

## <a name="use-powershell-to-move-a-vm"></a>Verschieben eines virtuellen Computers mithilfe von PowerShell

Wenn Sie einen virtuellen Computer in eine andere Ressourcengruppe verschieben möchten, müssen Sie auch alle abhängigen Ressourcen verschieben. Um eine Liste mit der Ressourcen-ID jeder dieser Ressourcen zu erhalten, verwenden Sie das Cmdlet [Get-AzureRMResource](/powershell/module/azurerm.resources/get-azurermresource).

```azurepowershell-interactive
 Get-AzureRMResource -ResourceGroupName <sourceResourceGroupName> | Format-table -Property ResourceId 
```

Sie können die Ausgabe des vorherigen Befehls als durch Trennzeichen getrennte Liste von Ressourcen-IDs an [Move-AzureRMResource](/powershell/module/azurerm.resources/move-azurermresource) verwenden, um jede Ressource an das Ziel zu verschieben. 

```azurepowershell-interactive
Move-AzureRmResource -DestinationResourceGroupName "<myDestinationResourceGroup>" `
    -ResourceId <myResourceId,myResourceId,myResourceId>
```
    
Schließen Sie den Parameter **-DestinationSubscriptionId** ein, um Ressourcen in ein anderes Abonnement zu verschieben. 

```azurepowershell-interactive
Move-AzureRmResource -DestinationSubscriptionId "<myDestinationSubscriptionID>" `
    -DestinationResourceGroupName "<myDestinationResourceGroup>" `
    -ResourceId <myResourceId,myResourceId,myResourceId>
```


Wenn Sie aufgefordert werden, zu bestätigen, dass die angegebene Ressource verschoben werden soll, geben Sie **J** ein.

## <a name="next-steps"></a>Nächste Schritte
Sie können viele verschiedene Arten von Ressourcen zwischen Ressourcengruppen und Abonnements verschieben. Weitere Informationen finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../../resource-group-move-resources.md).    

