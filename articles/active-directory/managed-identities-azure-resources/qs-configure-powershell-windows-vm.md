---
title: Konfigurieren von verwalteten Identitäten für Azure-Ressourcen auf einem virtuellen Azure-Computer mithilfe von PowerShell
description: Schrittweise Anweisungen zum Konfigurieren von verwalteten Identitäten für Azure-Ressourcen auf einem virtuellen Azure-Computer mithilfe von PowerShell.
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
ms.openlocfilehash: e8d85144b89d81e67d5ac225f0b6467230608ce0
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2018
ms.locfileid: "47106370"
---
# <a name="configure-managed-identities-for-azure-resources-on-an-azure-vm-using-powershell"></a>Konfigurieren von verwalteten Identitäten für Azure-Ressourcen auf einem virtuellen Azure-Computer mithilfe von PowerShell

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Verwaltete Identitäten für Azure-Ressourcen stellen für Azure-Dienste eine automatisch verwaltete Identität in Azure Active Directory bereit. Sie können diese Identität für die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung unterstützt. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein. 

In diesem Artikel erfahren Sie, wie Sie mithilfe von PowerShell die folgenden Vorgänge für verwaltete Identitäten für Azure-Ressourcen auf einem virtuellen Azure-Computer ausführen können:

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie nicht mit verwalteten Identitäten für Azure-Ressourcen vertraut sind, helfen Ihnen die Informationen in der [Übersicht](overview.md) weiter. **Machen Sie sich den [Unterschied zwischen einer vom System und einer vom Benutzer zugewiesenen verwalteten Identität](overview.md#how-does-it-work)** bewusst.
- Wenn Sie noch kein Azure-Konto haben, sollten Sie sich [für ein kostenloses Konto registrieren](https://azure.microsoft.com/free/), bevor Sie fortfahren.
- Um die Verwaltungsvorgänge in diesem Artikel auszuführen, benötigt Ihr Konto die folgenden Zuweisungen der rollenbasierten Azure-Zugriffssteuerung:

    > [!NOTE]
    > Es sind keine weiteren Azure AD-Verzeichnisrollenzuweisungen erforderlich.

    - [Mitwirkender für virtuelle Computer](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor), um eine VM zu erstellen und die vom System und/oder Benutzer zugewiesene verwaltete Identität für eine Azure-VM zu aktivieren bzw. zu entfernen
    - [Mitwirkender für verwaltete Identität](/azure/role-based-access-control/built-in-roles#managed-identity-contributor), um eine vom Benutzer zugewiesene verwaltete Identität zu erstellen
    - [Operator für verwaltete Identität](/azure/role-based-access-control/built-in-roles#managed-identity-operator), um eine vom Benutzer zugewiesene verwaltete Identität einer VM zuzuweisen bzw. davon zu entfernen
- Installieren Sie [die aktuelle Version von Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM), sofern noch nicht geschehen.

## <a name="system-assigned-managed-identity"></a>Systemseitig zugewiesene verwaltete Identität

In diesem Abschnitt erfahren Sie, wie Sie die vom System zugewiesene verwaltete Identität über Azure PowerShell aktivieren und deaktivieren.

### <a name="enable-system-assigned-managed-identity-during-creation-of-an-azure-vm"></a>Aktivieren einer vom System zugewiesenen verwalteten Identität beim Erstellen eines virtuellen Azure-Computers

So erstellen Sie einen virtuellen Azure-Computer mit aktivierter systemzugewiesener verwalteter Identität:

1. Verwenden Sie einen der folgenden Schnellstarts für virtuelle Azure-Computer, und setzen Sie nur die erforderlichen Abschnitte um („Anmelden bei Azure“, „Erstellen einer Ressourcengruppe“, „Erstellen einer Netzwerkgruppe“, „Erstellen des virtuellen Computers“).
    
    Wenn Sie zum Abschnitt „Erstellen des virtuellen Computers“ gelangen, nehmen Sie eine kleine Änderung an der Syntax des Cmdlets [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvm) vor. Fügen Sie einen `-AssignIdentity:$SystemAssigned`-Parameter hinzu, um den virtuellen Computer mit aktivierter systemzugewiesener Identität bereitzustellen. Beispiel:
      
    ```powershell
    $vmConfig = New-AzureRmVMConfig -VMName myVM -AssignIdentity:$SystemAssigned ...
    ```

   - [Erstellen eines virtuellen Windows-Computers mithilfe von PowerShell](../../virtual-machines/windows/quick-create-powershell.md)
   - [Erstellen eines virtuellen Linux-Computers mithilfe von PowerShell](../../virtual-machines/linux/quick-create-powershell.md)

2. (Optional) Fügen Sie die VM-Erweiterung für verwaltete Identitäten für Azure-Ressourcen hinzu (Veraltung geplant für Januar 2019), indem Sie den Parameter `-Type` im Cmdlet [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) verwenden. Sie können abhängig vom Typ des virtuellen Computers „ManagedIdentityExtensionForWindows“ oder „ManagedIdentityExtensionForLinux“ übergeben und mithilfe des `-Name`-Parameters benennen. Der `-Settings`-Parameter gibt den Port an, der vom OAuth-Token-Endpunkt für den Tokenabruf verwendet wird:

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```
    > [!NOTE]
    > Dieser Schritt ist optional, da Sie für den Tokenabruf auch den Azure IMDS-Identitätsendpunkt (Instance Metadata Service) verwenden können. Die Einstufung der verwalteten Identitäten für die VM-Erweiterung der Azure-Ressourcen als veraltet ist für Januar 2019 geplant. 

### <a name="enable-system-assigned-managed-identity-on-an-existing-azure-vm"></a>Aktivieren einer vom System zugewiesenen verwalteten Identität auf einem vorhandenen virtuellen Azure-Computer

Wenn Sie eine systemzugewiesene verwaltete Identität auf einem vorhandenen virtuellen Computer aktivieren möchten, gehen Sie wie folgt vor:

1. Melden Sie sich mit `Login-AzureRmAccount` bei Azure an. Verwenden Sie ein Konto, das dem Azure-Abonnement zugeordnet ist, das den virtuellen Computer enthält.

   ```powershell
   Login-AzureRmAccount
   ```

2. Rufen Sie zunächst die VM-Einstellungen mithilfe des Cmdlets `Get-AzureRmVM` ab. Verwenden Sie dann zum Aktivieren einer systemzugewiesenen verwalteten Identität den `-AssignIdentity`-Switch im Cmdlet [Update-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm):

   ```powershell
   $vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
   Update-AzureRmVM -ResourceGroupName myResourceGroup -VM $vm -AssignIdentity:$SystemAssigned
   ```

3. (Optional) Fügen Sie die VM-Erweiterung für verwaltete Identitäten für Azure-Ressourcen hinzu (Veraltung geplant für Januar 2019), indem Sie den Parameter `-Type` im Cmdlet [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) verwenden. Sie können abhängig vom Typ des virtuellen Computers „ManagedIdentityExtensionForWindows“ oder „ManagedIdentityExtensionForLinux“ übergeben und mithilfe des `-Name`-Parameters benennen. Der `-Settings`-Parameter gibt den Port an, der vom OAuth-Token-Endpunkt für den Tokenabruf verwendet wird. Achten Sie darauf, den richtigen `-Location`-Parameter anzugeben, der dem Speicherort des vorhandenen virtuellen Computers entspricht:

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```
    > [!NOTE]
    > Dieser Schritt ist optional, da Sie für den Tokenabruf auch den Azure IMDS-Identitätsendpunkt (Instance Metadata Service) verwenden können.

## <a name="disable-system-assigned-managed-identity-from-an-azure-vm"></a>Deaktivieren einer vom System zugewiesenen verwalteten Identität auf einem virtuellen Azure-Computer

Wenn Sie über einen virtuellen Computer verfügen, der nicht mehr die systemzugewiesene verwaltete Identität, jedoch weiterhin benutzerzugewiesene verwaltete Identitäten benötigt, verwenden Sie das folgende Cmdlet:

1. Melden Sie sich mit `Login-AzureRmAccount` bei Azure an. Verwenden Sie ein Konto, das dem Azure-Abonnement zugeordnet ist, das den virtuellen Computer enthält.

   ```powershell
   Login-AzureRmAccount
   ```

2. Rufen Sie die VM-Eigenschaften mithilfe des Cmdlets `Get-AzureRmVM` ab, und legen Sie für den `-IdentityType`-Parameter `UserAssigned` fest:

   ```powershell   
   $vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM    
   Update-AzureRmVm -ResourceGroupName myResourceGroup -VM $vm -IdentityType "UserAssigned"
   ```

Bei einem virtuellen Computer, der die systemzugewiesene verwaltete Identität nicht mehr benötigt und über keine benutzerzugewiesenen verwalteten Identitäten verfügt, verwenden Sie die folgenden Befehle:

```powershell
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
Update-AzureRmVm -ResourceGroupName myResourceGroup -VM $vm -IdentityType None
```

Um die VM-Erweiterung für verwaltete Identitäten für Azure-Ressourcen zu entfernen, verwenden Sie den Switch „-Name“ mit dem Cmdlet [Remove-AzureRmVMExtension](/powershell/module/azurerm.compute/remove-azurermvmextension), und geben Sie den gleichen Namen an, den Sie beim Hinzufügen der Erweiterung verwendet haben:

   ```powershell
   Remove-AzureRmVMExtension -ResourceGroupName myResourceGroup -Name "ManagedIdentityExtensionForWindows" -VMName myVM
   ```

## <a name="user-assigned-managed-identity"></a>Benutzerseitig zugewiesene verwaltete Identität

In diesem Abschnitt erfahren Sie, wie Sie mithilfe von Azure PowerShell eine benutzerzugewiesene verwaltete Identität zu einem virtuellen Computer hinzufügen und von diesem entfernen.

### <a name="assign-a-user-assigned-managed-identity-to-a-vm-during-creation"></a>Zuweisen einer vom Benutzer zugewiesenen verwalteten Identität zu einem virtuellen Computer bei der Erstellung

So weisen Sie einem virtuellen Azure-Computer bei der Erstellung eine benutzerzugewiesene verwaltete Identität zu:

1. Verwenden Sie einen der folgenden Schnellstarts für virtuelle Azure-Computer, und setzen Sie nur die erforderlichen Abschnitte um („Anmelden bei Azure“, „Erstellen einer Ressourcengruppe“, „Erstellen einer Netzwerkgruppe“, „Erstellen des virtuellen Computers“). 
  
    Wenn Sie zum Abschnitt zum Erstellen der VM gelangen, nehmen Sie eine kleine Änderung an der Syntax des Cmdlets [`New-AzureRmVMConfig`](/powershell/module/azurerm.compute/new-azurermvm) vor. Fügen Sie die Parameter `-IdentityType UserAssigned` und `-IdentityID ` für die Bereitstellung des virtuellen Computers mit einer benutzerzugewiesenen Identität hinzu.  Ersetzen Sie `<VM NAME>`, `<SUBSCRIPTION ID>`, `<RESROURCE GROUP>` und `<USER ASSIGNED IDENTITY NAME>` durch Ihre eigenen Werte.  Beispiel: 
    
    ```powershell 
    $vmConfig = New-AzureRmVMConfig -VMName <VM NAME> -IdentityType UserAssigned -IdentityID "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESROURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>..."
    ```
    
    - [Erstellen eines virtuellen Windows-Computers mithilfe von PowerShell](../../virtual-machines/windows/quick-create-powershell.md)
    - [Erstellen eines virtuellen Linux-Computers mithilfe von PowerShell](../../virtual-machines/linux/quick-create-powershell.md)

2. (Optional) Fügen Sie die VM-Erweiterung für verwaltete Identitäten für Azure-Ressourcen hinzu, indem Sie den `-Type`-Parameter im Cmdlet [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) verwenden. Sie können abhängig vom Typ des virtuellen Computers „ManagedIdentityExtensionForWindows“ oder „ManagedIdentityExtensionForLinux“ übergeben und mithilfe des `-Name`-Parameters benennen. Der `-Settings`-Parameter gibt den Port an, der vom OAuth-Token-Endpunkt für den Tokenabruf verwendet wird. Achten Sie darauf, den richtigen `-Location`-Parameter anzugeben, der dem Speicherort des vorhandenen virtuellen Computers entspricht:
      > [!NOTE]
    > Dieser Schritt ist optional, da Sie für den Tokenabruf auch den Azure IMDS-Identitätsendpunkt (Instance Metadata Service) verwenden können. Die Einstufung der verwalteten Identitäten für die VM-Erweiterung der Azure-Ressourcen als veraltet ist für Januar 2019 geplant.

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```

### <a name="assign-a-user-assigned-managed-identity-to-an-existing-azure-vm"></a>Zuweisen einer vom Benutzer zugewiesenen verwalteten Identität zu einem vorhandenen virtuellen Azure-Computer

So weisen Sie eine benutzerzugewiesene verwaltete Identität einem vorhandenen virtuellen Azure-Computer zu:

1. Melden Sie sich mit `Connect-AzureRmAccount` bei Azure an. Verwenden Sie ein Konto, das dem Azure-Abonnement zugeordnet ist, das den virtuellen Computer enthält.

   ```powershell
   Connect-AzureRmAccount
   ```

2. Erstellen Sie mit dem Cmdlet [New-AzureRmUserAssignedIdentity](/powershell/module/azurerm.managedserviceidentity/new-azurermuserassignedidentity) eine benutzerzugewiesene verwaltete Identität.  Beachten Sie die `Id` in der Ausgabe, da Sie diese im nächsten Schritt benötigen.

   > [!IMPORTANT]
   > Für die Erstellung von benutzerzugewiesenen verwalteten Identitäten werden nur alphanumerische Zeichen und Bindestriche („0-9“, „a-Z“ bzw. „A-Z“ oder „-“) unterstützt. Darüber hinaus sollten Namen max. 24 Zeichen enthalten, damit die Zuordnung zur VM/VMSS richtig funktioniert. Überprüfen Sie zu einem späteren Zeitpunkt auf dieser Seite, ob neue Informationen vorliegen. Weitere Informationen finden Sie unter [FAQs und bekannte Probleme mit der verwalteten Dienstidentität (Managed Service Identity, MSI) für Azure Active Directory](known-issues.md).

   ```powershell
   New-AzureRmUserAssignedIdentity -ResourceGroupName <RESOURCEGROUP> -Name <USER ASSIGNED IDENTITY NAME>
   ```
3. Rufen Sie zunächst die VM-Eigenschaften mithilfe des Cmdlets `Get-AzureRmVM` ab. Weisen Sie dem virtuellen Azure-Computer anschließend eine benutzerzugewiesene verwaltete Identität zu, und verwenden Sie dazu die Switches `-IdentityType` und `-IdentityID` im Cmdlet [Update-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm).  Der Wert für den Parameter `-IdentityId` ist die `Id`, die Sie im vorherigen Schritt notiert haben.  Ersetzen Sie `<VM NAME>`, `<SUBSCRIPTION ID>`, `<RESROURCE GROUP>` und `<USER ASSIGNED IDENTITY NAME>` durch Ihre eigenen Werte.

   ```powershell
   $vm = Get-AzureRmVM -ResourceGroupName <RESOURCE GROUP> -Name <VM NAME>
   Update-AzureRmVM -ResourceGroupName <RESOURCE GROUP> -VM $vm -IdentityType UserAssigned -IdentityID "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESROURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>"
   ```

4. Fügen Sie die VM-Erweiterung für verwaltete Identitäten für Azure-Ressourcen hinzu (Veraltung geplant für Januar 2019), indem Sie den Parameter `-Type` im Cmdlet [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) verwenden. Sie können abhängig vom Typ des virtuellen Computers „ManagedIdentityExtensionForWindows“ oder „ManagedIdentityExtensionForLinux“ übergeben und mithilfe des `-Name`-Parameters benennen. Der `-Settings`-Parameter gibt den Port an, der vom OAuth-Token-Endpunkt für den Tokenabruf verwendet wird. Geben Sie den richtigen `-Location`-Parameter an, der dem Speicherort der vorhandenen VM entspricht.

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```

### <a name="remove-a-user-assigned-managed-identity-from-an-azure-vm"></a>Entfernen einer vom Benutzer zugewiesenen verwalteten Identität von einer Azure-VM

Wenn Ihr virtueller Computer mehrere benutzerzugewiesene Identitäten umfasst, können Sie mit den folgenden Befehlen alle bis auf die letzte Identität entfernen. Ersetzen Sie die Parameterwerte `<RESOURCE GROUP>` und `<VM NAME>` durch Ihre eigenen Werte. `<USER ASSIGNED IDENTITY NAME>` ist die Namenseigenschaft der benutzerzugewiesenen verwalteten Identität, die weiterhin auf dem virtuellen Computer festgelegt sein sollte. Diese Informationen finden Sie im Identitätsabschnitt der VM mithilfe von `az vm show`:

```powershell
$vm = Get-AzureRmVm -ResourceGroupName myResourceGroup -Name myVm
Update-AzureRmVm -ResourceGroupName myResourceGroup -VirtualMachine $vm -IdentityType UserAssigned -IdentityID <USER ASSIGNED IDENTITY NAME>
```
Wenn Ihr virtueller Computer keine systemzugewiesene verwaltete Identität hat, und Sie alle benutzerzugewiesenen verwalteten Identitäten entfernen möchten, verwenden Sie den folgenden Befehl:

```powershell
$vm = Get-AzureRmVm -ResourceGroupName myResourceGroup -Name myVm
Update-AzureRmVm -ResourceGroupName myResourceGroup -VM $vm -IdentityType None
```
Wenn der virtuelle Computer sowohl system- als auch benutzerzugewiesene verwaltete Identitäten aufweist, können Sie alle benutzerzugewiesenen verwalteten Identitäten entfernen, indem Sie in den Modus wechseln, in dem nur systemzugewiesene verwaltete Identitäten verwendet werden.

```powershell 
$vm = Get-AzureRmVm -ResourceGroupName myResourceGroup -Name myVm
Update-AzureRmVm -ResourceGroupName myResourceGroup -VirtualMachine $vm -IdentityType "SystemAssigned"
```

## <a name="next-steps"></a>Nächste Schritte

- [Verwaltete Identitäten für Azure-Ressourcen – Übersicht](overview.md)
- Die vollständigen Schnellstarts zum Erstellen virtueller Azure-Computer finden Sie unter:
  
  - [Erstellen eines virtuellen Windows-Computers mit PowerShell](../../virtual-machines/windows/quick-create-powershell.md) 
  - [Erstellen eines virtuellen Linux-Computers mit PowerShell](../../virtual-machines/linux/quick-create-powershell.md) 
