---
title: Verwalten von Azure-Lösungen mit PowerShell | Microsoft-Dokumentation
description: Verwenden Sie Azure PowerShell und Resource Manager zum Verwalten von Ressourcen.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: b33b7303-3330-4af8-8329-c80ac7e9bc7f
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: powershell
ms.devlang: na
ms.topic: conceptual
ms.date: 02/16/2018
ms.author: tomfitz
ms.openlocfilehash: 13e5836aea0e307cdce5bcdcd5cf3c50969dfbf8
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
---
# <a name="manage-resources-with-azure-powershell"></a>Verwalten von Ressourcen mit Azure PowerShell

[!INCLUDE [Resource Manager governance introduction](../../includes/resource-manager-governance-intro.md)]

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Wenn Sie PowerShell lokal installieren und nutzen möchten, lesen Sie die Informationen unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps). Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Connect-AzureRmAccount` ausführen, um eine Verbindung mit Azure herzustellen.

## <a name="understand-scope"></a>Der Bereich

[!INCLUDE [Resource Manager governance scope](../../includes/resource-manager-governance-scope.md)]

In diesem Artikel wenden Sie alle Verwaltungseinstellungen auf eine Ressourcengruppe an, Sie können diese Einstellungen also am Ende problemlos entfernen.

Zuerst erstellen wir eine Ressourcengruppe.

```azurepowershell-interactive
Set-AzureRmContext -Subscription <subscription-name>
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

Die Ressourcengruppe ist derzeit leer.

## <a name="role-based-access-control"></a>Rollenbasierte Zugriffssteuerung

[!INCLUDE [Resource Manager governance policy](../../includes/resource-manager-governance-rbac.md)]

### <a name="assign-a-role"></a>Zuweisen einer Rolle

In diesem Artikel stellen Sie einen virtuellen Computer und das zugehörige virtuelle Netzwerk bereit. Zum Verwalten virtueller Computerlösungen gibt es drei ressourcenspezifische Rollen, die Zugriff auf häufig benötigte Ressourcen bereitstellen:

* [Mitwirkender von virtuellen Computern](../role-based-access-control/built-in-roles.md#virtual-machine-contributor)
* [Mitwirkender von virtuellem Netzwerk](../role-based-access-control/built-in-roles.md#network-contributor)
* [Mitwirkender von Speicherkonto](../role-based-access-control/built-in-roles.md#storage-account-contributor)

Anstatt einzelnen Benutzern Rollen zuzuweisen, ist es häufig einfacher, [eine Azure Active Directory-Gruppe zu erstellen](../active-directory/active-directory-groups-create-azure-portal.md), um die Benutzer zusammenzufassen, die ähnliche Aktionen ausführen müssen. Danach weisen Sie diese Gruppe der entsprechenden Rolle zu. Zur Vereinfachung erstellen Sie in diesem Artikel eine Azure Active Directory-Gruppe ohne Mitglieder. Sie können diese Gruppe dennoch zu einer Rolle für einen bestimmten Bereich zuweisen. 

Das folgende Beispiel erstellt eine Gruppe und weist diese zur Rolle „Mitwirkender von virtuellen Computern“ für die Ressourcengruppe zu. Um den Befehl `New-AzureAdGroup` auszuführen, müssen Sie entweder die [Azure Cloud Shell](/azure/cloud-shell/overview) verwenden oder [das Azure AD PowerShell-Modul herunterladen](https://www.powershellgallery.com/packages/AzureAD/).

```azurepowershell-interactive
$adgroup = New-AzureADGroup -DisplayName VMDemoContributors `
  -MailNickName vmDemoGroup `
  -MailEnabled $false `
  -SecurityEnabled $true
New-AzureRmRoleAssignment -ObjectId $adgroup.ObjectId `
  -ResourceGroupName myResourceGroup `
  -RoleDefinitionName "Virtual Machine Contributor"
```

In der Regel müssen Sie den Prozess für die Rollen **Mitwirkender von virtuellem Netzwerk** und **Mitwirkender von Speicherkonto** wiederholen, um sicherzustellen, dass die Benutzer den richtigen Rollen für die Verwaltung der bereitgestellten Ressourcen zugewiesen werden. In diesem Artikel können Sie diese Schritte überspringen.

## <a name="azure-policies"></a>Azure-Richtlinien

[!INCLUDE [Resource Manager governance policy](../../includes/resource-manager-governance-policy.md)]

### <a name="apply-policies"></a>Anwenden von Richtlinien

Ihr Abonnement enthält bereits verschiedene Richtliniendefinitionen. Verwenden Sie folgenden Befehl, um die verfügbaren Richtliniendefinitionen anzuzeigen:

```azurepowershell-interactive
(Get-AzureRmPolicyDefinition).Properties | Format-Table displayName, policyType
```

Die vorhandenen Richtliniendefinitionen werden angezeigt. Der Richtlinientyp lautet entweder **BuiltIn** oder **Custom**. Durchsuchen Sie die Definitionen, um eine Definition zu finden, in der eine Bedingung beschrieben wird, die Sie zuweisen möchten. In diesem Artikel weisen Sie Richtlinien zu, mit denen Folgendes erreicht wird:

* Begrenzen der Standorte für alle Ressourcen
* Begrenzen der SKUs für virtuelle Computer
* Überwachen von virtuellen Computern, die keine verwalteten Datenträger verwenden

```azurepowershell-interactive
$locations ="eastus", "eastus2"
$skus = "Standard_DS1_v2", "Standard_E2s_v2"

$rg = Get-AzureRmResourceGroup -Name myResourceGroup

$locationDefinition = Get-AzureRmPolicyDefinition | where-object {$_.properties.displayname -eq "Allowed locations"}
$skuDefinition = Get-AzureRmPolicyDefinition | where-object {$_.properties.displayname -eq "Allowed virtual machine SKUs"}
$auditDefinition = Get-AzureRmPolicyDefinition | where-object {$_.properties.displayname -eq "Audit VMs that do not use managed disks"}

New-AzureRMPolicyAssignment -Name "Set permitted locations" `
  -Scope $rg.ResourceId `
  -PolicyDefinition $locationDefinition `
  -listOfAllowedLocations $locations
New-AzureRMPolicyAssignment -Name "Set permitted VM SKUs" `
  -Scope $rg.ResourceId `
  -PolicyDefinition $skuDefinition `
  -listOfAllowedSKUs $skus
New-AzureRMPolicyAssignment -Name "Audit unmanaged disks" `
  -Scope $rg.ResourceId `
  -PolicyDefinition $auditDefinition
```

## <a name="deploy-the-virtual-machine"></a>Bereitstellen des virtuellen Computers

Sie haben Rollen und Richtlinien zugewiesen und können daher Ihre Lösung bereitstellen. Die Standardgröße ist „Standard_DS1_v2“ – eine Ihrer zulässigen SKUs. Wenn Sie diesen Schritt ausführen, werden Sie aufgefordert, Anmeldeinformationen einzugeben. Die Werte, die Sie eingeben, werden als Benutzername und Kennwort für den virtuellen Computer konfiguriert.

```azurepowershell-interactive
New-AzureRmVm -ResourceGroupName "myResourceGroup" `
     -Name "myVM" `
     -Location "East US" `
     -VirtualNetworkName "myVnet" `
     -SubnetName "mySubnet" `
     -SecurityGroupName "myNetworkSecurityGroup" `
     -PublicIpAddressName "myPublicIpAddress" `
     -OpenPorts 80,3389
```

Nach Abschluss der Bereitstellung können Sie weitere Verwaltungseinstellungen auf die Lösung anwenden.

## <a name="lock-resources"></a>Sperren von Ressourcen

[!INCLUDE [Resource Manager governance locks](../../includes/resource-manager-governance-locks.md)]

### <a name="lock-a-resource"></a>Sperren einer Ressource

Verwenden Sie folgenden Befehl, um den virtuellen Computer und die Netzwerksicherheitsgruppe zu sperren:

```azurepowershell-interactive
New-AzureRmResourceLock -LockLevel CanNotDelete `
  -LockName LockVM `
  -ResourceName myVM `
  -ResourceType Microsoft.Compute/virtualMachines `
  -ResourceGroupName myResourceGroup
New-AzureRmResourceLock -LockLevel CanNotDelete `
  -LockName LockNSG `
  -ResourceName myNetworkSecurityGroup `
  -ResourceType Microsoft.Network/networkSecurityGroups `
  -ResourceGroupName myResourceGroup
```

Der virtuelle Computer kann nur gelöscht werden, wenn Sie die Sperre explizit entfernen. Dieser Schritt wird unter [Bereinigen von Ressourcen](#clean-up-resources) gezeigt.

## <a name="tag-resources"></a>Markieren von Ressourcen

[!INCLUDE [Resource Manager governance tags](../../includes/resource-manager-governance-tags.md)]

### <a name="tag-resources"></a>Markieren von Ressourcen

[!INCLUDE [Resource Manager governance tags Powershell](../../includes/resource-manager-governance-tags-powershell.md)]

Um Tags auf einen virtuellen Computer anzuwenden, verwenden Sie folgenden Befehl:

```azurepowershell-interactive
$r = Get-AzureRmResource -ResourceName myVM `
  -ResourceGroupName myResourceGroup `
  -ResourceType Microsoft.Compute/virtualMachines
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test"; Project="Documentation" } -ResourceId $r.ResourceId -Force
```

### <a name="find-resources-by-tag"></a>Suchen von Ressourcen nach Tag

Um Ressourcen anhand von Tagname und -wert zu suchen, verwenden Sie folgenden Befehl:

```azurepowershell-interactive
(Find-AzureRmResource -TagName Environment -TagValue Test).Name
```

Sie können die zurückgegebenen Werte für Verwaltungstasks verwenden, z.B. zum Beenden aller virtuellen Computer mit einem Tagwert.

```azurepowershell-interactive
Find-AzureRmResource -TagName Environment -TagValue Test | Where-Object {$_.ResourceType -eq "Microsoft.Compute/virtualMachines"} | Stop-AzureRmVM
```

### <a name="view-costs-by-tag-values"></a>Anzeigen von Kosten nach Tagwert

Nachdem Sie Tags auf Ressourcen angewendet haben, können Sie die Kosten für Ressourcen mit diesen Tags anzeigen. Es dauert eine Weile, bis die Kostenanalyse die jüngste Nutzung anzeigt, daher werden möglicherweise noch keine Kosten angezeigt. Wenn die Kosten verfügbar sind, können Sie diese für Ressourcen in verschiedenen Ressourcengruppen Ihres Abonnements anzeigen. Benutzer müssen über [Zugriff auf Abrechnungsinformationen auf Abonnementebene](../billing/billing-manage-access.md) verfügen, um die Kosten sehen zu können.

Um Kosten nach Tag im Portal anzuzeigen, wählen Sie Ihr Abonnement und danach **Kostenanalyse** aus.

![Kostenanalyse](./media/powershell-azure-resource-manager/select-cost-analysis.png)

Filtern Sie die Anzeige nach Tagwert, und wählen Sie **Anwenden** aus.

![Anzeigen von Kosten nach Tag](./media/powershell-azure-resource-manager/view-costs-by-tag.png)

Sie können auch die [Azure-Abrechnungs-APIs](../billing/billing-usage-rate-card-overview.md) verwenden, um Kosten programmgesteuert anzuzeigen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Die gesperrte Netzwerksicherheitsgruppe kann erst gelöscht werden, wenn die Sperre entfernt wird. Um die Sperre zu entfernen, verwenden Sie folgenden Befehl:

```azurepowershell-interactive
Remove-AzureRmResourceLock -LockName LockVM `
  -ResourceName myVM `
  -ResourceType Microsoft.Compute/virtualMachines `
  -ResourceGroupName myResourceGroup
Remove-AzureRmResourceLock -LockName LockNSG `
  -ResourceName myNetworkSecurityGroup `
  -ResourceType Microsoft.Network/networkSecurityGroups `
  -ResourceGroupName myResourceGroup
```

Wenn Ressourcengruppe, virtueller Computer und alle zugehörigen Ressourcen nicht mehr benötigt werden, können Sie sie mit dem Befehl [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) entfernen.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zum Überwachen Ihrer virtuellen Computer finden Sie unter [Überwachen und Aktualisieren eines virtuellen Windows-Computers mit Azure PowerShell](../virtual-machines/windows/tutorial-monitoring.md).
* Informationen zum Implementieren empfohlener Sicherheitsmethoden über Azure Security Center finden Sie unter [Überwachen der Sicherheit virtueller Computer mit Azure Security Center](../virtual-machines/windows/tutorial-azure-security.md).
* Sie können vorhandene Ressourcen in eine neue Ressourcengruppe verschieben. Beispiele finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](resource-group-move-resources.md).
* Anleitungen dazu, wie Unternehmen Abonnements mit Resource Manager effektiv verwalten können, finden Sie unter [Azure-Unternehmensgerüst - Präskriptive Abonnementgovernance](resource-manager-subscription-governance.md).
