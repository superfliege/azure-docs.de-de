---
title: Konvertieren zwischen dem Standardspeicher und Storage Premium für verwaltete Azure-Datenträger | Microsoft-Dokumentation
description: Vorgehensweise zum Konvertieren zwischen dem Standardspeicher und Storage Premium für verwaltete Azure-Datenträger mithilfe von Azure PowerShell
services: virtual-machines-windows
documentationcenter: ''
author: ramankum
manager: kavithag
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/04/2018
ms.author: ramankum
ms.openlocfilehash: 958f661585b38b156cf523fe00986e7594474917
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2018
ms.locfileid: "49093815"
---
# <a name="update-the-storage-type-of-a-managed-disk"></a>Aktualisieren des Speichertyps eines verwalteten Datenträgers

Azure Managed Disks bietet drei Speichertypoptionen: [SSD Premium](../windows/premium-storage.md), [SSD Standard](../windows/disks-standard-ssd.md) und [HDD Standard](../windows/standard-storage.md). Basierend auf Ihren Leistungsanforderungen können Sie bei minimaler Downtime zwischen Speichertypen eines verwalteten Datenträgers wechseln. Bei nicht verwalteten Datenträgern wird das Wechseln zwischen Speichertypen nicht unterstützt. Sie können jedoch problemlos [einen nicht verwalteten Datenträger in einen verwalteten Datenträger konvertieren](convert-unmanaged-to-managed-disks.md).

In diesem Artikel wird beschrieben, wie Sie mit Azure PowerShell den Speichertyp eines verwalteten Datenträgers von Standard in Premium konvertieren. Wenn Sie PowerShell installieren oder aktualisieren müssen, lesen Sie [Installieren und Konfigurieren von Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-6.8.1).

## <a name="prerequisites"></a>Voraussetzungen

* Weil für die Konvertierung ein Neustart des virtuellen Computers (Virtual Machine, VM) erforderlich ist, sollten Sie die Migration Ihres Datenträgerspeichers während eines bereits vorhandenen Wartungsfensters durchführen. 
* Wenn Sie einen nicht verwalteten Datenträger verwenden, müssen Sie [ihn zuerst in einen verwalteten Datenträger konvertieren](convert-unmanaged-to-managed-disks.md), um zwischen den Speichertypen wechseln zu können. 
* Für die Beispiele in diesem Artikel muss Version 6.0.0 oder höher des Azure PowerShell-Moduls verwendet werden. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu. Führen Sie [Connect-AzureRmAccount](https://docs.microsoft.com/powershell/module/azurerm.profile/connect-azurermaccount) aus, um eine Verbindung mit Azure herzustellen.


## <a name="convert-all-the-managed-disks-of-a-vm-from-standard-to-premium"></a>Konvertieren aller verwalteten Datenträger eines virtuellen Computers von Storage Standard in Storage Premium

Im folgenden Beispiel wird gezeigt, wie Sie für alle Datenträger einer VM eine Konvertierung eines Standard-Speichers zu Storage Premium durchführen. Zur Verwendung von verwalteten Premium-Datenträgern muss Ihr virtueller Computer eine [VM-Größe](sizes.md) verwenden, die Storage Premium unterstützt. In diesem Beispiel erfolgt auch ein Wechsel zu einer Größe, die Storage Premium unterstützt:

```azurepowershell-interactive
# Name of the resource group that contains the VM
$rgName = 'yourResourceGroup'

# Name of the your virtual machine
$vmName = 'yourVM'

# Choose between StandardLRS and PremiumLRS based on your scenario
$storageType = 'Premium_LRS'

# Premium capable size
# Required only if converting storage from standard to premium
$size = 'Standard_DS2_v2'

# Stop and deallocate the VM before changing the size
Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force

$vm = Get-AzureRmVM -Name $vmName -resourceGroupName $rgName

# Change the VM size to a size that supports premium storage
# Skip this step if converting storage from premium to standard
$vm.HardwareProfile.VmSize = $size
Update-AzureRmVM -VM $vm -ResourceGroupName $rgName

# Get all disks in the resource group of the VM
$vmDisks = Get-AzureRmDisk -ResourceGroupName $rgName 

# For disks that belong to the selected VM, convert to premium storage
foreach ($disk in $vmDisks)
{
    if ($disk.ManagedBy -eq $vm.Id)
    {
        $diskUpdateConfig = New-AzureRmDiskUpdateConfig –AccountType $storageType
        Update-AzureRmDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $rgName `
        -DiskName $disk.Name
    }
}

Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
```

## <a name="convert-a-managed-disk-from-standard-to-premium"></a>Konvertieren eines verwalteten Datenträgers von Storage Standard in Storage Premium

Für Ihre Entwicklungs-/Testworkload sollten Sie eine Kombination aus Standard- und Premium-Datenträgern verwenden, um Ihre Kosten zu senken. Führen Sie dazu nur für die Datenträger, die eine höhere Leistung erfordern, ein Upgrade auf Storage Premium durch. Im folgenden Beispiel wird gezeigt, wie Sie für einen einzelnen Datenträger einer VM eine Konvertierung eines Standard-Speichers zu Storage Premium und umgekehrt durchführen. Zur Verwendung von verwalteten Premium-Datenträgern muss Ihr virtueller Computer eine [VM-Größe](sizes.md) verwenden, die Storage Premium unterstützt. In diesem Beispiel wird auch gezeigt, wie Sie zu einer Größe wechseln, die Storage Premium unterstützt:

```azurepowershell-interactive

$diskName = 'yourDiskName'
# resource group that contains the managed disk
$rgName = 'yourResourceGroupName'
# Choose between StandardLRS and PremiumLRS based on your scenario
$storageType = 'Premium_LRS'
# Premium capable size 
$size = 'Standard_DS2_v2'

$disk = Get-AzureRmDisk -DiskName $diskName -ResourceGroupName $rgName

# Get parent VM resource
$vmResource = Get-AzureRmResource -ResourceId $disk.ManagedBy

# Stop and deallocate the VM before changing the storage type
Stop-AzureRmVM -ResourceGroupName $vmResource.ResourceGroupName -Name $vmResource.Name -Force

$vm = Get-AzureRmVM -ResourceGroupName $vmResource.ResourceGroupName -Name $vmResource.Name 

# Change the VM size to a size that supports premium storage
# Skip this step if converting storage from premium to standard
$vm.HardwareProfile.VmSize = $size
Update-AzureRmVM -VM $vm -ResourceGroupName $rgName

# Update the storage type
$diskUpdateConfig = New-AzureRmDiskUpdateConfig -AccountType $storageType -DiskSizeGB $disk.DiskSizeGB
Update-AzureRmDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $rgName `
-DiskName $disk.Name

Start-AzureRmVM -ResourceGroupName $vm.ResourceGroupName -Name $vm.Name
```

## <a name="convert-a-managed-disk-from-standard-hdd-to-standard-ssd"></a>Konvertieren eines verwalteten Datenträgers von HDD Standard in SSD Standard

Im folgenden Beispiel wird gezeigt, wie Sie einen einzelnen Datenträger eines virtuellen Computers von HDD Standard in SSD Standard (und umgekehrt) konvertieren:

```azurepowershell-interactive

$diskName = 'yourDiskName'
# resource group that contains the managed disk
$rgName = 'yourResourceGroupName'
# Choose between Standard_LRS and StandardSSD_LRS based on your scenario
$storageType = 'StandardSSD_LRS'

$disk = Get-AzureRmDisk -DiskName $diskName -ResourceGroupName $rgName

# Get parent VM resource
$vmResource = Get-AzureRmResource -ResourceId $disk.ManagedBy

# Stop and deallocate the VM before changing the storage type
Stop-AzureRmVM -ResourceGroupName $vmResource.ResourceGroupName -Name $vmResource.Name -Force

$vm = Get-AzureRmVM -ResourceGroupName $vmResource.ResourceGroupName -Name $vmResource.Name 

# Update the storage type
$diskUpdateConfig = New-AzureRmDiskUpdateConfig -AccountType $storageType -DiskSizeGB $disk.DiskSizeGB
Update-AzureRmDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $rgName `
-DiskName $disk.Name

Start-AzureRmVM -ResourceGroupName $vm.ResourceGroupName -Name $vm.Name
```

## <a name="next-steps"></a>Nächste Schritte

Erstellen Sie eine schreibgeschützte Kopie eines virtuellen Computers mithilfe einer [Momentaufnahme](snapshot-copy-managed-disk.md).

