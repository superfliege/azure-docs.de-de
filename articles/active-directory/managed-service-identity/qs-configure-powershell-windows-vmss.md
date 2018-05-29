---
title: Konfigurieren von MSI in einer Azure VM-Skalierungsgruppe mit PowerShell
description: Schrittweise Anweisungen für ein Konfigurieren von system- und benutzerzugewiesenen Identitäten in einer Azure VM-Skalierungsgruppe mit PowerShell.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/27/2017
ms.author: daveba
ms.openlocfilehash: 97c5e2dde3faeaad13317597bef4f70455d22102
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
ms.locfileid: "33930128"
---
# <a name="configure-a-vmss-managed-service-identity-msi-using-powershell"></a>Konfigurieren einer MSI (Managed Service Identity, verwaltete Dienstidentität) für eine VM-Skalierungsgruppe mit PowerShell

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Eine verwaltete Dienstidentität stellt für Azure-Dienste eine automatisch verwaltete Identität in Azure Active Directory bereit. Sie können diese Identität für die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung unterstützt. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein. 

In diesem Artikel erfahren Sie, wie Sie mit PowerShell die folgenden Vorgänge für verwaltete Dienstidentitäten (Managed Service Identity, MSI) für eine Azure VM-Skalierungsgruppe (Virtual Machine Scale Set, VMSS) ausführen können:
- Aktivieren und Deaktivieren der systemzugewiesenen Identität in einer Azure VM-Skalierungsgruppe
- Hinzufügen und Entfernen einer benutzerzugewiesenen Identität in einer Azure VM-Skalierungsgruppe

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie nicht mit „Verwaltete Dienstidentität“ vertraut sind, helfen Ihnen die Informationen in dieser [Übersicht](overview.md) weiter. **Machen Sie sich den [Unterschied zwischen einer systemzugewiesenen und einer benutzerzugewiesenen Identität](overview.md#how-does-it-work)** bewusst.
- Wenn Sie noch kein Azure-Konto haben, sollten Sie sich [für ein kostenloses Konto registrieren](https://azure.microsoft.com/free/), bevor Sie fortfahren.
- Installieren Sie [die aktuelle Version von Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM), sofern noch nicht geschehen. 

## <a name="system-assigned-managed-identity"></a>Systemzugewiesene verwaltete Identität

In diesem Abschnitt erfahren Sie, wie Sie mithilfe von Azure PowerShell eine systemzugewiesene Identität aktivieren und entfernen.

### <a name="enable-system-assigned-identity-during-the-creation-of-an-azure-vmss"></a>Aktivieren einer systemzugewiesenen Identität beim Erstellen einer Azure VM-Skalierungsgruppe

So erstellen Sie eine VM-Skalierungsgruppe mit aktivierter systemzugewiesener Identität:

1. Informationen, wie eine VM-Skalierungsgruppe mit systemzugewiesener Identität erstellt wird, finden Sie unter *Example 1* im Referenzartikel für das [New-AzureRmVmssConfigAzureRmVmssConfig](/powershell/module/azurerm.compute/new-azurermvmssconfig)-Cmdlet.  Fügen Sie den Parameter `-IdentityType SystemAssigned` zum `New-AzureRmVmssConfig`-Cmdlet hinzu:

    ```powershell
    $VMSS = New-AzureRmVmssConfig -Location $Loc -SkuCapacity 2 -SkuName "Standard_A0" -UpgradePolicyMode "Automatic" -NetworkInterfaceConfiguration $NetCfg -IdentityType SystemAssigned`
    ```

2. (Optional) Fügen Sie die MSI-Erweiterung für eine VM-Skalierungsgruppe hinzu, indem Sie den `-Name`- und den `-Type`-Parameter im [Add-AzureRmVmssExtension](/powershell/module/azurerm.compute/add-azurermvmssextension)-Cmdlet verwenden. Sie können abhängig vom Typ des virtuellen Computers „ManagedIdentityExtensionForWindows“ oder „ManagedIdentityExtensionForLinux“ übergeben und mithilfe des `-Name`-Parameters benennen. Der `-Settings`-Parameter gibt den Port an, der vom OAuth-Token-Endpunkt für den Tokenabruf verwendet wird:

    > [!NOTE]
    > Dieser Schritt ist optional, da Sie für den Tokenabruf auch den Azure IMDS-Identitätsendpunkt (Instance Metadata Service) verwenden können.

   ```powershell
   $setting = @{ "port" = 50342 }
   $vmss = Get-AzureRmVmss
   Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Setting $settings 
   ```

## <a name="enable-system-assigned-identity-on-an-existing-azure-vmss"></a>Aktivieren der systemzugewiesenen Identität in einer vorhandenen Azure VM-Skalierungsgruppe

Wenn Sie eine systemzugewiesene Identität in einer vorhandenen Azure VM-Skalierungsgruppe aktivieren müssen, gehen Sie wie folgt vor:

1. Melden Sie sich mit `Login-AzureRmAccount` bei Azure an. Verwenden Sie ein Konto, das dem Azure-Abonnement zugeordnet ist, das den virtuellen Computer enthält. Stellen Sie außerdem sicher, dass Ihr Konto zu einer Rolle gehört, die Ihnen Schreibberechtigungen auf dem virtuellen Computer erteilt, z. B. „Mitwirkender für virtuelle Computer“:

   ```powershell
   Login-AzureRmAccount
   ```

2. Rufen Sie zunächst die Einstellungen der VM-Skalierungsgruppe mit dem [`Get-AzureRmVmss`](/powershell/module/azurerm.compute/get-azurermvmss)-Cmdlet ab. Verwenden Sie dann, um die systemzugewiesene Identität zu aktivieren, den `-IdentityType`-Schalter im [Update-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm)-Cmdlet:

   ```powershell
   $vm = Get-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVM
   Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name -myVM -IdentityType "SystemAssigned"
   ```

3. Fügen Sie die MSI-Erweiterung für eine VM-Skalierungsgruppe hinzu, indem Sie den `-Name`- und den `-Type`-Parameter im [Add-AzureRmVmssExtension](/powershell/module/azurerm.compute/add-azurermvmssextension)-Cmdlet verwenden. Sie können abhängig vom Typ des virtuellen Computers „ManagedIdentityExtensionForWindows“ oder „ManagedIdentityExtensionForLinux“ übergeben und mithilfe des `-Name`-Parameters benennen. Der `-Settings`-Parameter gibt den Port an, der vom OAuth-Token-Endpunkt für den Tokenabruf verwendet wird:

   ```powershell
   $setting = @{ "port" = 50342 }
   $vmss = Get-AzureRmVmss
   Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Setting $settings 
   ```

### <a name="disable-the-system-assigned-identity-from-an-azure-vmss"></a>Deaktivieren der systemzugewiesenen Identität aus einer Azure VM-Skalierungsgruppe

> [!NOTE]
> Ein Deaktivieren von MSI (Managed Service Identity, Verwaltete Dienstidentität) aus einer VM-Skalierungsgruppe wird derzeit nicht unterstützt. In der Zwischenzeit können Sie zwischen systemzugewiesenen und benutzerzugewiesenen Identitäten wechseln.

Wenn Sie über eine VM-Skalierungsgruppe verfügen, die nicht mehr die systemzugewiesene Identität, jedoch weiterhin benutzerzugewiesene Identitäten benötigt, verwenden Sie das folgende Cmdlet:

1. Melden Sie sich mit `Login-AzureRmAccount` bei Azure an. Verwenden Sie ein Konto, das dem Azure-Abonnement zugeordnet ist, das den virtuellen Computer enthält. Stellen Sie außerdem sicher, dass Ihr Konto zu einer Rolle gehört, die Ihnen Schreibberechtigungen auf dem virtuellen Computer erteilt, z. B. „Mitwirkender für virtuelle Computer“:

2. Führen Sie das folgende Cmdlet aus:

    ```powershell
    Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType "UserAssigned"
    ```

## <a name="user-assigned-identity"></a>Benutzerzugewiesene Identität

In diesem Abschnitt erfahren Sie, wie Sie mithilfe von Azure PowerShell eine benutzerzugewiesene Identität zu einer VM-Skalierungsgruppe hinzufügen und aus dieser entfernen.

### <a name="assign-a-user-assigned-identity-during-creation-of-an-azure-vmss"></a>Zuweisen einer benutzerzugewiesenen Identität beim Erstellen einer Azure VM-Skalierungsgruppe

Erstellen einer neuen VM-Skalierungsgruppe mit einer benutzerzugewiesenen Identität über PowerShell wird derzeit nicht unterstützt. Informationen, wie Sie einer vorhandenen VM-Skalierungsgruppe eine benutzerzugewiesene Identität hinzufügen, finden Sie im nächsten Abschnitt. Überprüfen Sie zu einem späteren Zeitpunkt auf dieser Seite, ob neue Informationen vorliegen.

### <a name="assign-a-user-identity-to-an-existing-azure-vmss"></a>Zuweisen einer Benutzeridentität zu einer vorhandenen Azure VM-Skalierungsgruppe

So weisen Sie eine benutzerzugewiesene Identität zu einer vorhandenen Azure VM-Skalierungsgruppe zu

1. Melden Sie sich mit `Connect-AzureRmAccount` bei Azure an. Verwenden Sie ein Konto, das dem Azure-Abonnement zugeordnet ist, das den virtuellen Computer enthält. Stellen Sie außerdem sicher, dass Ihr Konto zu einer Rolle gehört, die Ihnen Schreibberechtigungen auf dem virtuellen Computer erteilt, z. B. „Mitwirkender für virtuelle Computer“:

   ```powershell
   Connect-AzureRmAccount
   ```

2. Rufen Sie zunächst die VM-Einstellungen mithilfe des Cmdlets `Get-AzureRmVM` ab. Weisen Sie der Azure VM-Skalierungsgruppe dann eine benutzerzugewiesene Identität zu, indem Sie die Schalter `-IdentityType` und `-IdentityID` im [Update-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm)-Cmdlet verwenden. Ersetzen Sie `<VM NAME>`, `<SUBSCRIPTION ID>`, `<RESROURCE GROUP>`, `<USER ASSIGNED ID1>` und `USER ASSIGNED ID2` durch Ihre eigenen Werte.

   > [!IMPORTANT]
   > Für die Erstellung von Identitäten, die vom Benutzer zugewiesen werden, werden nur alphanumerische Zeichen und Bindestriche („0-9“, „a-Z“ bzw. „A-Z“ oder „-“) unterstützt. Darüber hinaus sollten Namen max. 24 Zeichen enthalten, damit die Zuordnung zur VM/VMSS richtig funktioniert. Überprüfen Sie zu einem späteren Zeitpunkt auf dieser Seite, ob neue Informationen vorliegen. Weitere Informationen finden Sie unter [FAQs und bekannte Probleme mit der verwalteten Dienstidentität (Managed Service Identity, MSI) für Azure Active Directory](known-issues.md).


   ```powershell
   $vmss = Get-AzureRmVmss -ResourceGroupName <RESOURCE GROUP> -Name <VMSS NAME>
   Update-AzureRmVmss -ResourceGroupName <RESOURCE GROUP> -VM $vmss -IdentityType UserAssigned -IdentityID "<USER ASSIGNED ID1>","<USER ASSIGNED ID2>"
   ```

### <a name="remove-a-user-assigned-identity-from-an-azure-vmss"></a>Entfernen einer benutzerzugewiesenen Identität aus einer Azure VM-Skalierungsgruppe

> [!NOTE]
> Ein Entfernen aller benutzerzugewiesenen Identitäten aus einer VM-Skalierungsgruppe wird derzeit nicht unterstützt, es sei denn, Sie verwenden eine systemzugewiesene Identität. Überprüfen Sie zu einem späteren Zeitpunkt auf dieser Seite, ob neue Informationen vorliegen.

Wenn Ihre VM-Skalierungsgruppe mehrere benutzerzugewiesene Identitäten hat, können Sie mit den folgenden Befehlen alle bis auf die letzte Identität entfernen. Ersetzen Sie die Parameterwerte `<RESOURCE GROUP>` und `<VMSS NAME>` durch Ihre eigenen Werte. `<MSI NAME>` ist die Namenseigenschaft der benutzerzugewiesenen Identität, die für die VM-Skalierungsgruppe erhalten bleiben soll. Diese Informationen finden Sie im Identitätsabschnitt der VM-Skalierungsgruppe mithilfe von `az vmss show`:

```powershell
$vmss = Get-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss
$vmss.Identity.IdentityIds = "<MSI NAME>"
Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -VirtualMachineScaleSet $vmss
```

Wenn Ihre VM-Skalierungsgruppe sowohl system- als auch benutzerzugewiesene Identitäten hat, können Sie alle benutzerzugewiesenen Identitäten entfernen, indem Sie so wechseln, dass ausschließlichen die systemzugewiesene Identität verwendet wird. Verwenden Sie den folgenden Befehl:

```powershell
$vmss = Get-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss
$vmss.Identity.IdentityIds = $null
Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -VirtualMachine $vmss -IdentityType "SystemAssigned"
```

## <a name="related-content"></a>Verwandte Inhalte

- [Übersicht über verwaltete Dienstidentitäten](overview.md)
- Die vollständigen Schnellstarts zum Erstellen virtueller Azure-Computer finden Sie unter:
  
  - [Erstellen eines virtuellen Windows-Computers mit PowerShell](../../virtual-machines/windows/quick-create-powershell.md) 
  - [Erstellen eines virtuellen Linux-Computers mit PowerShell](../../virtual-machines/linux/quick-create-powershell.md) 

















