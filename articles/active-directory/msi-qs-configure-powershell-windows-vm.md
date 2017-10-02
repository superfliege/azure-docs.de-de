---
title: Konfigurieren von MSI auf einem virtuellen Azure-Computer mit PowerShell
description: "Schrittweise Anweisungen zum Konfigurieren einer verwalteten Dienstidentität (Managed Service Identity, MSI) auf einem virtuellen Azure-Computer mit PowerShell."
services: active-directory
documentationcenter: 
author: bryanla
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/14/2017
ms.author: bryanla
ms.translationtype: HT
ms.sourcegitcommit: 4f77c7a615aaf5f87c0b260321f45a4e7129f339
ms.openlocfilehash: 612311f1c4e081e87dde76ce4a1d8efd46428c06
ms.contentlocale: de-de
ms.lasthandoff: 09/22/2017

---

# <a name="configure-a-vm-managed-service-identity-msi-using-powershell"></a>Konfigurieren einer VM-MSI (Managed Service Identity, verwaltete Dienstidentität) mit PowerShell

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

Eine verwaltete Dienstidentität stellt für Azure-Dienste eine automatisch verwaltete Identität in Azure Active Directory bereit. Sie können diese Identität für die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung unterstützt. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein. 

In diesem Artikel erfahren Sie, wie Sie MSI für einen virtuellen Azure-Computer mit PowerShell aktivieren und entfernen.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [msi-qs-configure-prereqs](../../includes/msi-qs-configure-prereqs.md)]

Installieren Sie [Azure PowerShell, Version 4.3.1](https://www.powershellgallery.com/packages/AzureRM/4.3.1), sofern noch nicht geschehen.

## <a name="enable-msi-during-creation-of-an-azure-vm"></a>Aktivieren von MSI beim Erstellen eines virtuellen Azure-Computers

So erstellen Sie einen MSI-fähigen virtuellen Computer:

1. Verwenden Sie einen der folgenden Schnellstarts für virtuelle Azure-Computer, und setzen Sie nur die erforderlichen Abschnitte um („Anmelden bei Azure“, „Erstellen einer Ressourcengruppe“, „Erstellen einer Netzwerkgruppe“, „Erstellen des virtuellen Computers“). 

   > [!IMPORTANT] 
   > Wenn Sie zum Abschnitt „Erstellen des virtuellen Computers“ gelangen, nehmen Sie eine kleine Änderung an der Syntax des Cmdlets [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvm) vor. Achten Sie darauf, dass Sie einen `-IdentityType "SystemAssigned"`-Parameter hinzufügen, um einen MSI für den virtuellen Computer bereitzustellen, z. B.:
   >  
   > `$vmConfig = New-AzureRmVMConfig -VMName myVM -IdentityType "SystemAssigned" ...`

   - [Erstellen eines virtuellen Windows-Computers mithilfe von PowerShell](../virtual-machines/windows/quick-create-powershell.md)
   - [Erstellen eines virtuellen Linux-Computers mithilfe von PowerShell](../virtual-machines/linux/quick-create-powershell.md)



2. Fügen Sie die MSI-VM-Erweiterung hinzu, indem Sie den `-Type`-Parameter im Cmdlet [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) verwenden. Sie können abhängig vom Typ des virtuellen Computers „ManagedIdentityExtensionForWindows“ oder „ManagedIdentityExtensionForLinux“ übergeben und mithilfe des `-Name`-Parameters benennen. Der `-Settings`-Parameter gibt den Port an, der vom OAuth-Token-Endpunkt für den Tokenabruf verwendet wird:

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```

## <a name="enable-msi-on-an-existing-azure-vm"></a>Aktivieren von MSI auf einem vorhandenen virtuellen Azure-Computer

Wenn Sie MSI auf einem vorhandenen virtuellen Computer aktivieren möchten, gehen Sie wie folgt vor:

1. Melden Sie sich mit `Login-AzureRmAccount` bei Azure an. Verwenden Sie ein Konto, das dem Azure-Abonnement zugeordnet ist, das den virtuellen Computer enthält. Stellen Sie außerdem sicher, dass Ihr Konto zu einer Rolle gehört, die Ihnen Schreibberechtigungen auf dem virtuellen Computer erteilt, z. B. „Mitwirkender für virtuelle Computer“:

   ```powershell
   Login-AzureRmAccount
   ```

2. Rufen Sie zunächst die VM-Einstellungen mithilfe des Cmdlets `Get-AzureRmVM` ab. Verwenden Sie dann zum Aktivieren von MSI den `-IdentityType`-Switch im Cmdlet [Update-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm):

   ```powershell
   $vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
   Update-AzureRmVM -ResourceGroupName myResourceGroup -VM $vm -IdentityType "SystemAssigned"
   ```

3. Fügen Sie die MSI-VM-Erweiterung hinzu, indem Sie den `-Type`-Parameter im Cmdlet [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) verwenden. Sie können abhängig vom Typ des virtuellen Computers „ManagedIdentityExtensionForWindows“ oder „ManagedIdentityExtensionForLinux“ übergeben und mithilfe des `-Name`-Parameters benennen. Der `-Settings`-Parameter gibt den Port an, der vom OAuth-Token-Endpunkt für den Tokenabruf verwendet wird. Achten Sie darauf, den richtigen `-Location`-Parameter anzugeben, der dem Speicherort des vorhandenen virtuellen Computers entspricht:

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```

## <a name="remove-msi-from-an-azure-vm"></a>Entfernen von MSI von einem virtuellen Azure-Computer

Wenn MSI auf einem virtuellen Computer nicht mehr benötigt wird, können Sie das Cmdlet `RemoveAzureRmVMExtension` verwenden, um MSI von diesem virtuellen Computer zu entfernen:

1. Melden Sie sich mit `Login-AzureRmAccount` bei Azure an. Verwenden Sie ein Konto, das dem Azure-Abonnement zugeordnet ist, das den virtuellen Computer enthält. Stellen Sie außerdem sicher, dass Ihr Konto zu einer Rolle gehört, die Ihnen Schreibberechtigungen auf dem virtuellen Computer erteilt, z. B. „Mitwirkender für virtuelle Computer“:

   ```powershell
   Login-AzureRmAccount
   ```

2. Verwenden Sie den Schalter `-Name` mit dem Cmdlet [Remove-AzureRmVMExtension](/powershell/module/azurerm.compute/remove-azurermvmextension), und geben Sie den gleichen Namen an, den Sie beim Hinzufügen der Erweiterung verwendet haben:

   ```powershell
   Remove-AzureRmVMExtension -ResourceGroupName myResourceGroup -Name "ManagedIdentityExtensionForWindows" -VMName myVM
   ```

## <a name="related-content"></a>Verwandte Inhalte

- [Übersicht über verwaltete Dienstidentitäten](msi-overview.md)
- Die vollständigen Schnellstarts zum Erstellen virtueller Azure-Computer finden Sie unter:
  
  - [Erstellen eines virtuellen Windows-Computers mit PowerShell](../virtual-machines/windows/quick-create-powershell.md) 
  - [Erstellen eines virtuellen Linux-Computers mit PowerShell](../virtual-machines/linux/quick-create-powershell.md) 

Verwenden Sie den folgenden Kommentarabschnitt, um uns Feedback zu senden und uns bei der Verbesserung unserer Inhalte zu unterstützen.

















