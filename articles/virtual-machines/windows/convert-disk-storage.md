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
ms.date: 08/07/2017
ms.author: ramankum
ms.openlocfilehash: ac3f1368fb6d3f31b75b581d56e07fe11c3722b3
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
---
# <a name="convert-azure-managed-disks-storage-from-standard-to-premium-and-vice-versa"></a>Konvertieren zwischen dem Standardspeicher und Storage Premium für verwaltete Azure-Datenträger

Für verwaltete Datenträger stehen zwei Speicheroptionen zur Verfügung: [Premium](premium-storage.md) (SSD-basiert) und [Standard](standard-storage.md) (HDD-basiert). Entsprechend Ihren Leistungsanforderungen können Sie problemlos zwischen beiden Optionen wechseln – und das bei minimaler Downtime. Diese Funktion ist nicht für nicht verwaltete Datenträger verfügbar. Sie können jedoch problemlos eine [Konvertierung in verwaltete Datenträger ](convert-unmanaged-to-managed-disks.md) durchführen, um einfach zwischen beiden Optionen zu wechseln.

In diesem Artikel wird die Konvertierung zwischen dem Standardspeicher und Storage Premium für verwaltete Datenträger mithilfe von Azure PowerShell erläutert. Wenn Sie PowerShell installieren oder aktualisieren ausführen müssen, finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/install-azurerm-ps.md) Informationen dazu.

## <a name="before-you-begin"></a>Voraussetzungen

* Da für die Konvertierung ein Neustart der VM erforderlich ist, sollten Sie die Migration Ihres Datenträgerspeichers während eines bereits vorhandenen Wartungsfensters durchführen. 
* Wenn Sie nicht verwaltete Datenträger verwenden, [konvertieren Sie sie zuerst in verwaltete Datenträger](convert-unmanaged-to-managed-disks.md), um mithilfe der Anweisungen in diesem Artikel zwischen beiden Speicheroptionen zu wechseln. 
* Für diesen Artikel ist Version 6.0.0 oder höher des Azure PowerShell-Moduls erforderlich. Führen Sie ` Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu. Sie müssen außerdem `Connect-AzureRmAccount` ausführen, um eine Verbindung mit Azure herzustellen.


## <a name="convert-all-the-managed-disks-of-a-vm-from-standard-to-premium-and-vice-versa"></a>Konvertieren zwischen dem Standardspeicher und Storage Premium für alle verwalteten Datenträger einer VM

Im folgenden Beispiel wird gezeigt, wie Sie für alle Datenträger einer VM eine Konvertierung vom Standardspeicher in Storage Premium durchführen. Zur Verwendung von verwalteten Premium-Datenträgern muss Ihre VM eine [VM-Größe](sizes.md) verwenden, die Premium-Speicher unterstützt. In diesem Beispiel erfolgt ein Wechsel zu einer Größe, die Premium-Speicher unterstützt.

```azurepowershell-interactive
# Name of the resource group that contains the VM
$rgName = 'yourResourceGroup'

# Name of the your virtual machine
$vmName = 'yourVM'

# Choose between Standard_LRS and Premium_LRS based on your scenario
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
## <a name="convert-a-managed-disk-from-standard-to-premium-and-vice-versa"></a>Konvertieren zwischen dem Standardspeicher und Storage Premium für einen verwalteten Datenträger

Für Ihre Entwicklungs-/Testworkload sollten Sie eine Kombination aus Standard- und Premium-Datenträger verwenden, um Ihre Kosten zu senken. Führen Sie hierfür nur für die Datenträger, die eine höhere Leistung erfordern, ein Upgrade auf Storage Premium durch. Im folgenden Beispiel wird gezeigt, wie Sie für einen einzelnen Datenträger einer VM eine Konvertierung zwischen dem Standardspeicher und Storage Premium durchführen. Zur Verwendung von verwalteten Premium-Datenträgern muss Ihre VM eine [VM-Größe](sizes.md) verwenden, die Premium-Speicher unterstützt. In diesem Beispiel erfolgt ein Wechsel zu einer Größe, die Premium-Speicher unterstützt.

```azurepowershell-interactive

$diskName = 'yourDiskName'
# resource group that contains the managed disk
$rgName = 'yourResourceGroupName'
# Choose between Standard_LRS and Premium_LRS based on your scenario
$storageType = 'Premium_LRS'
# Premium capable size 
$size = 'Standard_DS2_v2'

$disk = Get-AzureRmDisk -DiskName $diskName -ResourceGroupName $rgName

# Get the ARM resource to get name and resource group of the VM
$vmResource = Get-AzureRmResource -ResourceId $disk.diskId

# Stop and deallocate the VM before changing the storage type
Stop-AzureRmVM -ResourceGroupName $vm.ResourceGroupName -Name $vm.Name -Force

$vm = Get-AzureRmVM $vmResource.ResourceGroupName -Name $vmResource.ResourceName 

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

