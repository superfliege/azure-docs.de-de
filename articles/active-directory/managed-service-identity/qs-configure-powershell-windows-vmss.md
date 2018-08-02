---
title: 'Gewusst wie: Konfigurieren einer verwalteten Dienstidentität auf einer Azure-VMSS (Scale Set, Skalierungsgruppe) mit PowerShell'
description: Schrittweise Anweisungen für ein Konfigurieren von system- und benutzerzugewiesenen Identitäten in einer Azure VM-Skalierungsgruppe mit PowerShell.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/27/2017
ms.author: daveba
ms.openlocfilehash: 5d4539c05d05053ac2ea6cd1c5fadbd161b41173
ms.sourcegitcommit: c2c64fc9c24a1f7bd7c6c91be4ba9d64b1543231
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2018
ms.locfileid: "39257741"
---
# <a name="configure-a-vmss-managed-service-identity-using-powershell"></a>Konfigurieren einer verwalteten Dienstidentität für eine VM-Skalierungsgruppe mit PowerShell

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Eine verwaltete Dienstidentität stellt für Azure-Dienste eine automatisch verwaltete Identität in Azure Active Directory bereit. Sie können diese Identität für die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung unterstützt. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein. 

In diesem Artikel erfahren Sie, wie Sie mit PowerShell die Vorgänge für verwaltete Dienstidentitäten für eine Azure-VM-Skalierungsgruppe ausführen:
- Aktivieren und Deaktivieren der systemzugewiesenen Identität in einer VM-Skalierungsgruppe
- Hinzufügen und Entfernen einer benutzerzugewiesenen Identität in einer VM-Skalierungsgruppe

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie nicht mit „Verwaltete Dienstidentität“ vertraut sind, helfen Ihnen die Informationen in dieser [Übersicht](overview.md) weiter. **Machen Sie sich den [Unterschied zwischen einer vom System und einer vom Benutzer zugewiesenen Identität](overview.md#how-does-it-work)** bewusst.
- Wenn Sie noch kein Azure-Konto haben, sollten Sie sich [für ein kostenloses Konto registrieren](https://azure.microsoft.com/free/), bevor Sie fortfahren.
- Um die Verwaltungsvorgänge in diesem Artikel durchzuführen, benötigt Ihr Konto die folgenden Rollenzuordnungen:
    - [Mitwirkender für virtuelle Computer](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor), um eine VM-Skalierungsgruppe zu erstellen und die vom System zugewiesene verwaltete Identität und/oder Benutzeridentität auf einer VM-Skalierungsgruppe zu aktivieren bzw. daraus zu entfernen.
    - [Mitwirkender für verwaltete Identität](/azure/role-based-access-control/built-in-roles#managed-identity-contributor), um eine vom Benutzer zugewiesene Identität zu erstellen.
    - [Operator für verwaltete Identität](/azure/role-based-access-control/built-in-roles#managed-identity-operator), um eine vom Benutzer zugewiesene Identität einer VM-Skalierungsgruppe zuzuweisen bzw. daraus zu entfernen.
- Installieren Sie [die aktuelle Version von Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM), sofern noch nicht geschehen. 

## <a name="system-assigned-managed-identity"></a>Systemzugewiesene verwaltete Identität

In diesem Abschnitt erfahren Sie, wie Sie mithilfe von Azure PowerShell eine systemzugewiesene Identität aktivieren und entfernen.

### <a name="enable-system-assigned-identity-during-the-creation-of-an-azure-virtual-machine-scale-set"></a>Aktivieren der systemzugewiesenen Identität bei der Erstellung einer Azure-VM-Skalierungsgruppe

So erstellen Sie eine VM-Skalierungsgruppe mit aktivierter systemzugewiesener Identität:

1. Informationen, wie eine VM-Skalierungsgruppe mit systemzugewiesener Identität erstellt wird, finden Sie unter *Example 1* im Referenzartikel für das [New-AzureRmVmssConfigAzureRmVmssConfig](/powershell/module/azurerm.compute/new-azurermvmssconfig)-Cmdlet.  Fügen Sie den Parameter `-IdentityType SystemAssigned` zum `New-AzureRmVmssConfig`-Cmdlet hinzu:

    ```powershell
    $VMSS = New-AzureRmVmssConfig -Location $Loc -SkuCapacity 2 -SkuName "Standard_A0" -UpgradePolicyMode "Automatic" -NetworkInterfaceConfiguration $NetCfg -IdentityType SystemAssigned`
    ```

2. (Optional) Fügen Sie die Erweiterung der verwalteten Dienstidentität für eine VM-Skalierungsgruppe hinzu, indem Sie den `-Name`- und `-Type`-Parameter im [Add-AzureRmVmssExtension](/powershell/module/azurerm.compute/add-azurermvmssextension)-Cmdlet verwenden. Sie können abhängig vom Typ der VM-Skalierungsgruppe „ManagedIdentityExtensionForWindows“ oder „ManagedIdentityExtensionForLinux“ übergeben und mithilfe des `-Name`-Parameters benennen. Der `-Settings`-Parameter gibt den Port an, der vom OAuth-Token-Endpunkt für den Tokenabruf verwendet wird:

    > [!NOTE]
    > Dieser Schritt ist optional, da Sie für den Tokenabruf auch den Azure IMDS-Identitätsendpunkt (Instance Metadata Service) verwenden können.

   ```powershell
   $setting = @{ "port" = 50342 }
   $vmss = Get-AzureRmVmss
   Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Setting $settings 
   ```

## <a name="enable-system-assigned-identity-on-an-existing-azure-virtual-machine-scale-set"></a>Aktivieren der systemzugewiesenen Identität in einer vorhandenen Azure VM-Skalierungsgruppe

Wenn Sie die systemzugewiesene Identität in einer vorhandenen Azure VM-Skalierungsgruppe aktivieren müssen:

1. Melden Sie sich mit `Login-AzureRmAccount` bei Azure an. Verwenden Sie ein Konto, das dem Azure-Abonnement zugeordnet ist, das die VM-Skalierungsgruppe enthält. Stellen Sie außerdem sicher, dass Ihr Konto zu einer Rolle gehört, die Ihnen Schreibberechtigungen auf der VM-Skalierungsgruppe erteilt, z.B. „Mitwirkender für virtuelle Computer“:

   ```powershell
   Login-AzureRmAccount
   ```

2. Rufen Sie zuerst mit dem [`Get-AzureRmVmss`](/powershell/module/azurerm.compute/get-azurermvmss)-Cmdlet die Eigenschaften der VM-Skalierungsgruppe ab. Verwenden Sie dann, um die systemzugewiesene Identität zu aktivieren, den `-IdentityType`-Schalter im [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss)-Cmdlet:

   ```powershell
   Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name -myVmss -IdentityType "SystemAssigned"
   ```

3. Fügen Sie die Erweiterung der verwalteten Dienstidentität für eine VM-Skalierungsgruppe hinzu, indem Sie den `-Name`- und `-Type`-Parameter im [Add-AzureRmVmssExtension](/powershell/module/azurerm.compute/add-azurermvmssextension)-Cmdlet verwenden. Sie können abhängig vom Typ der VM-Skalierungsgruppe „ManagedIdentityExtensionForWindows“ oder „ManagedIdentityExtensionForLinux“ übergeben und mithilfe des `-Name`-Parameters benennen. Der `-Settings`-Parameter gibt den Port an, der vom OAuth-Token-Endpunkt für den Tokenabruf verwendet wird:

   ```powershell
   $setting = @{ "port" = 50342 }
   $vmss = Get-AzureRmVmss
   Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Setting $settings 
   ```

### <a name="disable-the-system-assigned-identity-from-an-azure-virtual-machine-scale-set"></a>Deaktivieren der systemzugewiesenen Identität in einer Azure-VM-Skalierungsgruppe

Wenn Sie über eine VM-Skalierungsgruppe verfügen, die nicht mehr die systemzugewiesene Identität, jedoch weiterhin benutzerzugewiesene Identitäten benötigt, verwenden Sie das folgende Cmdlet:

1. Melden Sie sich mit `Login-AzureRmAccount` bei Azure an. Verwenden Sie ein Konto, das dem Azure-Abonnement zugeordnet ist, das den virtuellen Computer enthält. Stellen Sie außerdem sicher, dass Ihr Konto zu einer Rolle gehört, die Ihnen Schreibberechtigungen auf der VM-Skalierungsgruppe erteilt, z.B. „Mitwirkender für virtuelle Computer“:

2. Führen Sie das folgende Cmdlet aus:

   ```powershell
   Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType "UserAssigned"
   ```

Bei einer VM-Skalierungsgruppe, die nicht mehr die vom System zugewiesene Identität benötigt und über keine vom Benutzer zugewiesenen Identitäten verfügt, verwenden Sie die folgenden Befehle:

```powershell
Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType None
```

## <a name="user-assigned-identity"></a>Benutzerzugewiesene Identität

In diesem Abschnitt erfahren Sie, wie Sie mithilfe von Azure PowerShell eine benutzerzugewiesene Identität einer VM-Skalierungsgruppe hinzufügen und aus dieser entfernen.

### <a name="assign-a-user-assigned-identity-during-creation-of-an-azure-virtual-machine-scale-set"></a>Zuweisen einer benutzerzugewiesenen Identität bei der Erstellung einer Azure-VM-Skalierungsgruppe

Das Erstellen einer neuen VM-Skalierungsgruppe mit einer benutzerzugewiesenen Identität wird für PowerShell derzeit nicht unterstützt. Informationen, wie Sie einer vorhandenen VM-Skalierungsgruppe eine benutzerzugewiesene Identität hinzufügen, finden Sie im nächsten Abschnitt. Überprüfen Sie zu einem späteren Zeitpunkt auf dieser Seite, ob neue Informationen vorliegen.

### <a name="assign-a-user-identity-to-an-existing-azure-virtual-machine-scale-set"></a>Zuweisen einer Benutzeridentität zu einer vorhandenen Azure-VM-Skalierungsgruppe

So weisen Sie eine benutzerzugewiesene Identität einer vorhandenen Azure-VM-Skalierungsgruppe zu:

1. Melden Sie sich mit `Connect-AzureRmAccount` bei Azure an. Verwenden Sie ein Konto, das dem Azure-Abonnement zugeordnet ist, das die VM-Skalierungsgruppe enthält. Stellen Sie außerdem sicher, dass Ihr Konto zu einer Rolle gehört, die Ihnen Schreibberechtigungen auf der VM-Skalierungsgruppe erteilt, z.B. „Mitwirkender für virtuelle Computer“:

   ```powershell
   Connect-AzureRmAccount
   ```

2. Rufen Sie zuerst mit dem `Get-AzureRmVM`-Cmdlet die Eigenschaften der VM-Skalierungsgruppe ab. Weisen Sie der VM-Skalierungsgruppe dann eine benutzerzugewiesene Identität zu, indem Sie die Schalter `-IdentityType` und `-IdentityID` im [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss)-Cmdlet verwenden. Ersetzen Sie `<VM NAME>`, `<SUBSCRIPTION ID>`, `<RESROURCE GROUP>`, `<USER ASSIGNED ID1>` und `USER ASSIGNED ID2` durch Ihre eigenen Werte.

   [!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

   ```powershell
   Update-AzureRmVmss -ResourceGroupName <RESOURCE GROUP> -Name <VMSS NAME> -IdentityType UserAssigned -IdentityID "<USER ASSIGNED ID1>","<USER ASSIGNED ID2>"
   ```

### <a name="remove-a-user-assigned-managed-identity-from-an-azure-virtual-machine-scale-set"></a>Entfernen einer vom Benutzer zugewiesenen verwalteten Identität aus einer Azure-VM-Skalierungsgruppe

Wenn Ihre VM-Skalierungsgruppe mehrere benutzerzugewiesene Identitäten hat, können Sie mit den folgenden Befehlen alle bis auf die letzte Identität entfernen. Ersetzen Sie die Parameterwerte `<RESOURCE GROUP>` und `<VMSS NAME>` durch Ihre eigenen Werte. `<MSI NAME>` ist die Namenseigenschaft der benutzerzugewiesenen Identität, die für die VM-Skalierungsgruppe erhalten bleiben soll. Diese Informationen finden Sie im Identitätsabschnitt der VM-Skalierungsgruppe mithilfe von `az vmss show`:

```powershell
Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType UserAssigned -IdentityID "<MSI NAME>"
```
Wenn Ihre VM-Skalierungsgruppe keine vom System zugewiesene Identität hat und Sie alle vom Benutzer zugewiesenen Identitäten entfernen möchten, verwenden Sie den folgenden Befehl:

```powershell
Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType None
```
Wenn die VM-Skalierungsgruppe sowohl vom System als auch vom Benutzer zugewiesene Identitäten aufweist, können Sie alle vom Benutzer zugewiesenen Identitäten entfernen, indem Sie in den Modus wechseln, in dem nur die vom System zugewiesene Identität verwendet wird.

```powershell 
Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType "SystemAssigned"
```

## <a name="related-content"></a>Verwandte Inhalte

- [Übersicht über verwaltete Dienstidentitäten](overview.md)
- Die vollständigen Schnellstarts zum Erstellen virtueller Azure-Computer finden Sie unter:
  
  - [Erstellen eines virtuellen Windows-Computers mit PowerShell](../../virtual-machines/windows/quick-create-powershell.md) 
  - [Erstellen eines virtuellen Linux-Computers mit PowerShell](../../virtual-machines/linux/quick-create-powershell.md) 

















