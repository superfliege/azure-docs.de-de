---
title: Ändern der Verfügbarkeitsgruppe eines virtuellen Computers | Microsoft Docs
description: Erfahren Sie, wie Sie mithilfe von Azure PowerShell und des Resource Manager-Bereitstellungsmodells die Verfügbarkeitsgruppe für Ihre virtuellen Computer ändern.
keywords: ''
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/30/2018
ms.author: cynthn
ms.openlocfilehash: 8d11f81d5fea47ccef8689a84c06768c4ca36012
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44162035"
---
# <a name="change-the-availability-set-for-a-windows-vm"></a>Ändern der Verfügbarkeitsgruppe für einen virtuellen Windows-Computer
Die folgenden Schritte beschreiben, wie Sie die Verfügbarkeitsgruppe eines virtuellen Computers über Azure PowerShell ändern. Ein virtueller Computer kann nur zum Zeitpunkt der Erstellung zu einer Verfügbarkeitsgruppe hinzugefügt werden. Um die Verfügbarkeitsgruppe zu ändern, müssen Sie den virtuellen Computer löschen und neu erstellen. 

## <a name="change-the-availability-set"></a>Ändern der Verfügbarkeitsgruppe 

Das folgende Skript zeigt ein Beispiel für das Erfassen der erforderlichen Informationen, das Löschen des ursprünglichen virtuellen Computers und das erneute Erstellen des Computers in einer neuen Verfügbarkeitsgruppe.

```powershell
# Set variables
    $resourceGroup = "myResourceGroup"
    $vmName = "myVM"
    $newAvailSetName = "myAvailabilitySet"

# Get the details of the VM to be moved to the Availablity Set
    $originalVM = Get-AzureRmVM `
       -ResourceGroupName $resourceGroup `
       -Name $vmName

# Remove the original VM
    Remove-AzureRmVM -ResourceGroupName $resourceGroup -Name $vmName

# Create new availability set if it does not exist
    $availSet = Get-AzureRmAvailabilitySet `
       -ResourceGroupName $resourceGroup `
       -Name $newAvailSetName `
       -ErrorAction Ignore
    if (-Not $availSet) {
    $availSet = New-AzureRmAvailabilitySet `
       -Location $originalVM.Location `
       -Name $newAvailSetName `
       -ResourceGroupName $resourceGroup `
       -PlatformFaultDomainCount 2 `
       -PlatformUpdateDomainCount 2 `
       -Sku Aligned
    }

# Create the basic configuration for the replacement VM
    $newVM = New-AzureRmVMConfig `
       -VMName $originalVM.Name `
       -VMSize $originalVM.HardwareProfile.VmSize `
       -AvailabilitySetId $availSet.Id
  
    Set-AzureRmVMOSDisk `
       -VM $newVM -CreateOption Attach `
       -ManagedDiskId $originalVM.StorageProfile.OsDisk.ManagedDisk.Id `
       -Name $originalVM.StorageProfile.OsDisk.Name `
       -Windows

# Add Data Disks
    foreach ($disk in $originalVM.StorageProfile.DataDisks) { 
    Add-AzureRmVMDataDisk -VM $newVM `
       -Name $disk.Name `
       -ManagedDiskId $disk.ManagedDisk.Id `
       -Caching $disk.Caching `
       -Lun $disk.Lun `
       -DiskSizeInGB $disk.DiskSizeGB `
       -CreateOption Attach
    }
    
# Add NIC(s)
    foreach ($nic in $originalVM.NetworkProfile.NetworkInterfaces) {
        Add-AzureRmVMNetworkInterface `
           -VM $newVM `
           -Id $nic.Id
    }

# Recreate the VM
    New-AzureRmVM `
       -ResourceGroupName $resourceGroup `
       -Location $originalVM.Location `
       -VM $newVM `
       -DisableBginfoExtension
```

## <a name="next-steps"></a>Nächste Schritte

Erweitern Sie den Speicher für Ihren virtuellen Computer, indem Sie einen zusätzlichen [Datenträger](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)hinzufügen.

