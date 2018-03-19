---
title: Includedatei
description: Includedatei
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/16/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 21216d19fb8a37d3e9e02e410d39b2a999d3935e
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/23/2018
---
Verwenden Sie zum Hinzufügen von zwei Tags zu einer Ressourcengruppe den Befehl [Set-AzureRmResourceGroup](/powershell/module/azurerm.resources/set-azurermresourcegroup):

```azurepowershell-interactive
Set-AzureRmResourceGroup -Name myResourceGroup -Tag @{ Dept="IT"; Environment="Test" }
```

Angenommen, Sie möchten ein drittes Tag hinzufügen. Wenn Sie Tags auf eine Ressource oder Ressourcengruppe anwenden, werden die bereits vorhandenen Tags für diese Ressource oder Ressourcengruppe überschrieben. Um ein neues Tag hinzufügen, ohne die vorhandenen Tags zu verlieren, müssen Sie die vorhandenen Tags abrufen, ein neues Tag hinzufügen und dann die Tagsammlung erneut anwenden:

```azurepowershell-interactive
# Get existing tags and add a new tag
$tags = (Get-AzureRmResourceGroup -Name myResourceGroup).Tags
$tags += @{Project="Documentation"}

# Reapply the updated set of tags 
Set-AzureRmResourceGroup -Tag $tags -Name myResourceGroup
```

Ressourcen erben keine Tags aus der Ressourcengruppe. Derzeit enthält Ihre Ressourcengruppe drei Tags, die Ressourcen besitzen jedoch keine Tags. Verwenden Sie das folgende Skript, um alle Tags einer Ressourcengruppe auf die darin enthaltenen Ressourcen anzuwenden und vorhandene Tags für Ressourcen beizubehalten, die keine Duplikate sind:

```azurepowershell-interactive
# Get the resource group
$group = Get-AzureRmResourceGroup myResourceGroup

if ($group.Tags -ne $null) {
    # Get the resources in the resource group
    $resources = $group | Find-AzureRmResource

    # Loop through each resource
    foreach ($r in $resources)
    {
        # Get the tags for this resource
        $resourcetags = (Get-AzureRmResource -ResourceId $r.ResourceId).Tags

        # Loop through each tag from the resource group
        foreach ($key in $group.Tags.Keys)
        {
            # Check if the resource already has a tag for the key from the resource group. If so, remove it from the resource
            if (($resourcetags) -AND ($resourcetags.ContainsKey($key))) { $resourcetags.Remove($key) }
        }

        # Add the tags from the resource group to the resource tags
        $resourcetags += $group.Tags

        # Reapply the updated tags to the resource 
        Set-AzureRmResource -Tag $resourcetags -ResourceId $r.ResourceId -Force
    }
}
```

Alternativ können Sie Tags aus der Ressourcengruppe auf die Ressourcen anwenden, ohne die vorhandenen Tags beizubehalten:

```azurepowershell-interactive
# Get the resource group
$g = Get-AzureRmResourceGroup -Name myResourceGroup

# Find all the resources in the resource group, and for each resource apply the tags from the resource group
Find-AzureRmResource -ResourceGroupNameEquals $g.ResourceGroupName | ForEach-Object {Set-AzureRmResource -ResourceId $_.ResourceId -Tag $g.Tags -Force }
```

Kombinieren Sie mehrere Werte in einem einzelnen Tag mithilfe einer JSON-Zeichenfolge.

```azurepowershell-interactive
Set-AzureRmResourceGroup -Name myResourceGroup -Tag @{ CostCenter="{`"Dept`":`"IT`",`"Environment`":`"Test`"}" }
```

Durch Übergeben einer leeren Hashtabelle können Sie alle Tags löschen.

```azurepowershell-interactive
Set-AzureRmResourceGroup -Name myResourceGroup -Tag @{ }
```
