---
title: "Konvertieren zwischen dem Standardspeicher und Storage Premium für verwaltete Azure-Datenträger | Microsoft-Dokumentation"
description: "Vorgehensweise zum Konvertieren zwischen dem Standardspeicher und Storage Premium für verwaltete Azure-Datenträger mithilfe von Azure PowerShell"
services: virtual-machines-windows
documentationcenter: 
author: ramankum
manager: kavithag
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: ramankum
ms.openlocfilehash: 213b00b4f94ef6181c4c1629c97729a8843bd230
ms.sourcegitcommit: 9c3150e91cc3075141dc2955a01f47040d76048a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2017
---
# <a name="convert-azure-managed-disks-storage-from-standard-to-premium-and-vice-versa"></a>Konvertieren zwischen dem Standardspeicher und Storage Premium für verwaltete Azure-Datenträger

Für verwaltete Datenträger stehen zwei Speicheroptionen zur Verfügung: [Premium](../../storage/storage-premium-storage.md) (SSD-basiert) und [Standard](../../storage/storage-standard-storage.md) (HDD-basiert). Entsprechend Ihren Leistungsanforderungen können Sie problemlos zwischen beiden Optionen wechseln – und das bei minimaler Downtime. Diese Funktion ist nicht für nicht verwaltete Datenträger verfügbar. Sie können jedoch problemlos eine [Konvertierung in verwaltete Datenträger ](convert-unmanaged-to-managed-disks.md) durchführen, um einfach zwischen beiden Optionen zu wechseln.

In diesem Artikel wird die Konvertierung zwischen dem Standardspeicher und Storage Premium für verwaltete Datenträger mithilfe von Azure PowerShell erläutert. Wenn Sie PowerShell installieren oder aktualisieren ausführen müssen, finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/install-azurerm-ps.md) Informationen dazu.

## <a name="before-you-begin"></a>Voraussetzungen

* Da für die Konvertierung ein Neustart der VM erforderlich ist, sollten Sie die Migration Ihres Datenträgerspeichers während eines bereits vorhandenen Wartungsfensters durchführen. 
* Wenn Sie nicht verwaltete Datenträger verwenden, [konvertieren Sie sie zuerst in verwaltete Datenträger](convert-unmanaged-to-managed-disks.md), um mithilfe der Anweisungen in diesem Artikel zwischen beiden Speicheroptionen zu wechseln. 


## <a name="convert-all-the-managed-disks-of-a-vm-from-standard-to-premium-and-vice-versa"></a>Konvertieren zwischen dem Standardspeicher und Storage Premium für alle verwalteten Datenträger einer VM

Im folgenden Beispiel wird gezeigt, wie Sie für alle Datenträger einer VM eine Konvertierung vom Standardspeicher in Storage Premium durchführen. Zur Verwendung von verwalteten Premium-Datenträgern muss Ihre VM eine [VM-Größe](sizes.md) verwenden, die Premium-Speicher unterstützt. In diesem Beispiel erfolgt ein Wechsel zu einer Größe, die Premium-Speicher unterstützt.

```azurepowershell-interactive
# Name of the resource group that contains the VM
$rgName = 'yourResourceGroup'

# Name of the your virtual machine
$vmName = 'yourVM'

# Choose between StandardLRS and PremiumLRS based on your scenario
$storageType = 'PremiumLRS'

# Premium capable size
# Required only if converting storage from standard to premium
$size = 'Standard_DS2_v2'
$vm = Get-AzureRmVM -Name $vmName -resourceGroupName $rgName

# Stop and deallocate the VM before changing the size
Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force

# Change the VM size to a size that supports premium storage
# Skip this step if converting storage from premium to standard
$vm.HardwareProfile.VmSize = $size
Update-AzureRmVM -VM $vm -ResourceGroupName $rgName

# Get all disks in the resource group of the VM
$vmDisks = Get-AzureRmDisk -ResourceGroupName $rgName 

# For disks that belong to the selected VM, convert to premium storage
foreach ($disk in $vmDisks)
{
    if ($disk.OwnerId -eq $vm.Id)
    {
        $diskUpdateConfig = New-AzureRmDiskUpdateConfig –AccountType $storageType
        Update-AzureRmDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $rgName `
        -DiskName $disk.Name
    }
}

Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
```
## <a name="convert-a-managed-disk-from-standard-to-premium-and-vice-versa"></a>Konvertieren zwischen dem Standardspeicher und Storage Premium für einen verwalteten Datenträger

Für Ihre Entwicklungs-/Testworkload sollten Sie eine Kombination aus Standard- und Premium-Datenträger verwenden, um Ihre Kosten zu senken. Führen Sie hierfür nur für die Datenträger, die eine höhere Leistung erfordern, ein Upgrade auf Storage Premium durch. Im folgenden Beispiel wird gezeigt, wie Sie für einen einzelnen Datenträger einer VM eine Konvertierung zwischen dem Standardspeicher und Storage Premium durchführen. Zur Verwendung von verwalteten Premium-Datenträgern muss Ihre VM eine [VM-Größe](sizes.md) verwenden, die Premium-Speicher unterstützt. In diesem Beispiel erfolgt ein Wechsel zu einer Größe, die Premium-Speicher unterstützt.

```azurepowershell-interactive

$diskName = 'yourDiskName'
# resource group that contains the managed disk
$rgName = 'yourResourceGroupName'
# Choose between StandardLRS and PremiumLRS based on your scenario
$storageType = 'PremiumLRS'
# Premium capable size 
$size = 'Standard_DS2_v2'

$disk = Get-AzureRmDisk -DiskName $diskName -ResourceGroupName $rgName

# Get the ARM resource to get name and resource group of the VM
$vmResource = Get-AzureRmResource -ResourceId $disk.OwnerId
$vm = Get-AzureRmVM $vmResource.ResourceGroupName -Name $vmResource.ResourceName 

# Stop and deallocate the VM before changing the storage type
Stop-AzureRmVM -ResourceGroupName $vm.ResourceGroupName -Name $vm.Name -Force

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

## <a name="next-steps"></a>Nächste Schritte

Erstellen Sie eine schreibgeschützte Kopie eines virtuellen Computers mit [Momentaufnahmen](snapshot-copy-managed-disk.md).

