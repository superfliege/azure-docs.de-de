---
title: "Anfügen eines Datenträgers an einen virtuellen Windows-Computer in Azure mithilfe von PowerShell | Microsoft-Dokumentation"
description: "Hier erhalten Sie Informationen zum Anfügen von neuen oder vorhandenen Datenträgern an einen virtuellen Windows-Computer mithilfe von PowerShell mit dem Resource Manager-Bereitstellungsmodell."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/11/2017
ms.author: cynthn
ms.openlocfilehash: 6bc52262105fd9b162ad8ada9ae5cc3dbf623df2
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2017
---
# <a name="attach-a-data-disk-to-a-windows-vm-using-powershell"></a>Anfügen eines Datenträgers an einen virtuellen Windows-Computer mithilfe von PowerShell

In diesem Artikel wird beschrieben, wie Sie mithilfe von PowerShell neue und vorhandene Datenträger an einen virtuellen Windows-Computer anfügen können. 

Lesen Sie die folgenden Tipps, bevor Sie mit diesem Vorgang fortfahren:
* Die Größe des virtuellen Computers bestimmt, wie viele Datenträger Sie anfügen können. Ausführliche Informationen finden Sie unter [Größen für virtuelle Computer](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Für die Verwendung von Premium-Speicher benötigen Sie einen virtuellen Computer mit einer für Premium-Speicher geeigneten Größe, z.B. die DS- oder GS-Serie. Nähere Informationen finden Sie unter [Premium-Speicher: Hochleistungsspeicher für Workloads auf virtuellen Azure-Computern](premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Wenn Sie PowerShell lokal installieren und verwenden möchten, müssen Sie für dieses Tutorial mindestens Version 3.6 des Azure PowerShell-Moduls verwenden. Führen Sie ` Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Login-AzureRmAccount` ausführen, um eine Verbindung mit Azure herzustellen.


## <a name="add-an-empty-data-disk-to-a-virtual-machine"></a>Hinzufügen eines leeren Datenträgers zu einem virtuellen Computer

Dieses Beispiel zeigt, wie Sie einem vorhandenen virtuellen Computer einen leeren Datenträger hinzufügen.

### <a name="using-managed-disks"></a>Verwenden verwalteter Datenträger

```azurepowershell-interactive
$rgName = 'myResourceGroup'
$vmName = 'myVM'
$location = 'East US' 
$storageType = 'PremiumLRS'
$dataDiskName = $vmName + '_datadisk1'

$diskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location $location -CreateOption Empty -DiskSizeGB 128
$dataDisk1 = New-AzureRmDisk -DiskName $dataDiskName -Disk $diskConfig -ResourceGroupName $rgName

$vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName 
$vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1

Update-AzureRmVM -VM $vm -ResourceGroupName $rgName
```

### <a name="using-managed-disks-in-an-availability-zone"></a>Verwenden von verwalteten Datenträgern in einer Verfügbarkeitszone
Verwenden Sie zum Erstellen eines Datenträgers in einer Verfügbarkeitszone [New-AzureRmDiskConfig](/powershell/module/azurerm.compute/new-azurermdiskconfig) mit dem `-Zone`-Parameter. Im folgenden Beispiel wird ein Datenträger in Zone *1* erstellt.

[!INCLUDE [availability-zones-preview-statement.md](../../../includes/availability-zones-preview-statement.md)]

```powershell
$rgName = 'myResourceGroup'
$vmName = 'myVM'
$location = 'East US 2' 
$storageType = 'PremiumLRS'
$dataDiskName = $vmName + '_datadisk1'

$diskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location $location -CreateOption Empty -DiskSizeGB 128 -Zone 1
$dataDisk1 = New-AzureRmDisk -DiskName $dataDiskName -Disk $diskConfig -ResourceGroupName $rgName

$vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName 
$vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1

Update-AzureRmVM -VM $vm -ResourceGroupName $rgName
```


### <a name="initialize-the-disk"></a>Datenträger initialisieren

Nachdem Sie einen leeren Datenträger hinzugefügt haben, müssen Sie ihn initialisieren. Um den Datenträger zu initialisieren, können Sie sich bei einem virtuellen Computer anmelden und die Datenträgerverwaltung verwenden. Wenn Sie bei der Erstellung WinRM aktiviert und ein Zertifikat auf dem virtuellen Computer installiert haben, können Sie den Datenträger mit einem PowerShell-Remoteaufruf initialisieren. Alternativ können Sie auch eine benutzerdefinierte Skripterweiterung verwenden: 

```azurepowershell-interactive
    $location = "location-name"
    $scriptName = "script-name"
    $fileName = "script-file-name"
    Set-AzureRmVMCustomScriptExtension -ResourceGroupName $rgName -Location $locName -VMName $vmName -Name $scriptName -TypeHandlerVersion "1.4" -StorageAccountName "mystore1" -StorageAccountKey "primary-key" -FileName $fileName -ContainerName "scripts"
```
        
Der Skriptdateiinhalt für die Datenträgerinitialisierung kann etwa wie folgender Code aussehen:

```azurepowershell-interactive
    $disks = Get-Disk | Where partitionstyle -eq 'raw' | sort number

    $letters = 70..89 | ForEach-Object { [char]$_ }
    $count = 0
    $labels = "data1","data2"

    foreach ($disk in $disks) {
        $driveLetter = $letters[$count].ToString()
        $disk | 
        Initialize-Disk -PartitionStyle MBR -PassThru |
        New-Partition -UseMaximumSize -DriveLetter $driveLetter |
        Format-Volume -FileSystem NTFS -NewFileSystemLabel $labels[$count] -Confirm:$false -Force
    $count++
    }
```


## <a name="attach-an-existing-data-disk-to-a-vm"></a>Hinzufügen eines vorhandenen Datenträgers zu einem virtuellen Computer

Sie können einen vorhandenen verwalteten Datenträger als Datenträger für Daten an einen virtuellen Computer anfügen. 

```azurepowershell-interactive
$rgName = "myResourceGroup"
$vmName = "myVM"
$location = "East US" 
$dataDiskName = "myDisk"
$disk = Get-AzureRmDisk -ResourceGroupName $rgName -DiskName $dataDiskName 

$vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName 

$vm = Add-AzureRmVMDataDisk -CreateOption Attach -Lun 0 -VM $vm -ManagedDiskId $disk.Id

Update-AzureRmVM -VM $vm -ResourceGroupName $rgName
```

## <a name="next-steps"></a>Nächste Schritte

Erstellen einer [Momentaufnahme](snapshot-copy-managed-disk.md)
