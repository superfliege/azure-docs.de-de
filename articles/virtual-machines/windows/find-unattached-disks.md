---
title: "Suchen und Löschen von nicht angefügten verwalteten und nicht verwalteten Azure-Datenträgern | Microsoft-Dokumentation"
description: "Informationen zum Suchen und Löschen von nicht angefügten verwalteten und nicht verwalteten Azure-Datenträgern (VHDs/Seitenblobs) über Azure PowerShell"
services: virtual-machines-windows
documentationcenter: 
author: ramankumarlive
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: ramankum
ms.openlocfilehash: a846d3578d40b19762f185381c92bdf8e225b185
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/20/2018
---
# <a name="find-and-delete-unattached-azure-managed-and-unmanaged-disks"></a>Suchen und Löschen von nicht angefügten verwalteten und nicht verwalteten Azure-Datenträgern
Wenn Sie einen virtuellen Computer in Azure löschen, werden die daran angefügten Datenträger nicht standardmäßig gelöscht. Dadurch werden Datenverluste aufgrund versehentlich gelöschter virtueller Computer verhindert, aber Sie zahlen weiterhin unnötigerweise für die nicht angefügten Datenträger. Verwenden Sie die Informationen in diesem Artikel, um alle nicht angefügten Datenträger zu suchen und zu löschen und so Kosten zu sparen. 


## <a name="find-and-delete-unattached-managed-disks"></a>Suchen und Löschen nicht angefügter verwalteter Datenträger 

Das folgende Skript zeigt Ihnen, wie Sie nicht angefügte [verwaltete Datenträger](managed-disks-overview.md) mithilfe der *ManagedBy*-Eigenschaft suchen. Das Skript durchläuft alle verwalteten Datenträger in einem Abonnement und überprüft, ob die Eigenschaft *ManagedBy* NULL ist, um nicht angefügte verwaltete Datenträger zu finden. Die Eigenschaft *ManagedBy* speichert die Ressourcen-ID des virtuellen Computers, an den ein verwalteter Datenträger angefügt ist.

Es wird dringend empfohlen, das Skript zuerst mit auf „0“ festgelegter Variable *deleteUnattachedDisks* auszuführen, um alle nicht angefügten Datenträger anzuzeigen. Nachdem Sie die nicht angefügten Datenträger überprüft haben, führen Sie das Skript mit auf „1“ festgelegter Variable *deleteUnattachedDisks* erneut aus, um alle nicht angefügten Datenträger zu löschen.

```azurepowershell-interactive

# Set deleteUnattachedDisks=1 if you want to delete unattached Managed Disks
# Set deleteUnattachedDisks=0 if you want to see the Id of the unattached Managed Disks
$deleteUnattachedDisks=0

$managedDisks = Get-AzureRmDisk

foreach ($md in $managedDisks) {
    
    # ManagedBy property stores the Id of the VM to which Managed Disk is attached to
    # If ManagedBy property is $null then it means that the Managed Disk is not attached to a VM
    if($md.ManagedBy -eq $null){

        if($deleteUnattachedDisks -eq 1){
            
            Write-Host "Deleting unattached Managed Disk with Id: $($md.Id)"

            $md | Remove-AzureRmDisk -Force

            Write-Host "Deleted unattached Managed Disk with Id: $($md.Id) "

        }else{

            $md.Id

        }
           
    }
     
 } 
```
## <a name="find-and-delete-unattached-unmanaged-disks"></a>Suchen und Löschen nicht angefügter nicht verwalteter Datenträger 

Bei nicht verwalteten Datenträgern handelt es sich um VHD-Dateien, die als [Seitenblobs] (/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-page-blobs) in [Azure Storage-Konten](../../storage/common/storage-create-storage-account.md) gespeichert sind. Das folgende Skript zeigt Ihnen, wie Sie nicht angefügte nicht verwaltete Datenträger (Seitenblobs) mithilfe der *LeaseStatus*-Eigenschaft suchen. Es durchläuft alle nicht verwalteten Datenträger in allen Speicherkonten in einem Abonnement und überprüft, ob die *LeaseStatus*-Eigenschaft entsperrt ist, um nicht angefügte nicht verwaltete Datenträger zu finden. Die *LeaseStatus*-Eigenschaft ist auf „locked“ festgelegt, wenn ein nicht verwalteter Datenträger an einen virtuellen Computer angefügt ist.

Es wird dringend empfohlen, das Skript zuerst mit auf „0“ festgelegter Variable *deleteUnattachedVHDs* auszuführen, um alle nicht angefügten Datenträger anzuzeigen. Nachdem Sie die nicht angefügten Datenträger überprüft haben, führen Sie das Skript mit auf „1“ festgelegter Variable *deleteUnattachedVHDs* erneut aus, um alle nicht angefügten Datenträger zu löschen.


```azurepowershell-interactive
   
# Set deleteUnattachedVHDs=1 if you want to delete unattached VHDs
# Set deleteUnattachedVHDs=0 if you want to see the Uri of the unattached VHDs
$deleteUnattachedVHDs=1

$storageAccounts = Get-AzureRmStorageAccount

foreach($storageAccount in $storageAccounts){

    $storageKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $storageAccount.ResourceGroupName -Name $storageAccount.StorageAccountName)[0].Value

    $context = New-AzureStorageContext -StorageAccountName $storageAccount.StorageAccountName -StorageAccountKey $storageKey

    $containers = Get-AzureStorageContainer -Context $context

    foreach($container in $containers){

        $blobs = Get-AzureStorageBlob -Container $container.Name -Context $context

        #Fetch all the Page blobs with extension .vhd as only Page blobs can be attached as disk to Azure VMs
        $blobs | Where-Object {$_.BlobType -eq 'PageBlob' -and $_.Name.EndsWith('.vhd')} | ForEach-Object { 
        
            #If a Page blob is not attached as disk then LeaseStatus will be unlocked
            if($_.ICloudBlob.Properties.LeaseStatus -eq 'Unlocked'){
              
                  if($deleteUnattachedVHDs -eq 1){

                        Write-Host "Deleting unattached VHD with Uri: $($_.ICloudBlob.Uri.AbsoluteUri)"

                        $_ | Remove-AzureStorageBlob -Force

                        Write-Host "Deleted unattached VHD with Uri: $($_.ICloudBlob.Uri.AbsoluteUri)"
                  }
                  else{

                        $_.ICloudBlob.Uri.AbsoluteUri

                  }

            }
        
        }

    }

}

```

## <a name="next-steps"></a>Nächste Schritte

[Löschen eines Speicherkontos](../../storage/common/storage-create-storage-account.md)




