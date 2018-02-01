---
title: "Suchen und Löschen von nicht angefügten verwalteten und nicht verwalteten Azure-Datenträgern | Microsoft-Dokumentation"
description: "Informationen zum Suchen und Löschen von nicht angefügten verwalteten und nicht verwalteten Azure-Datenträgern (VHDs/Seitenblobs) über die Azure-Befehlszeilenschnittstelle"
services: virtual-machines-linux
documentationcenter: 
author: ramankumarlive
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: ramankum
ms.openlocfilehash: 9ada768cd4128b9dd6949b5a96c557496c6bb11c
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/20/2018
---
# <a name="find-and-delete-unattached-azure-managed-and-unmanaged-disks"></a>Suchen und Löschen von nicht angefügten verwalteten und nicht verwalteten Azure-Datenträgern
Wenn Sie einen virtuellen Computer in Azure löschen, werden die daran angefügten Datenträger nicht standardmäßig gelöscht. Dadurch werden Datenverluste aufgrund versehentlich gelöschter virtueller Computer verhindert, aber Sie zahlen weiterhin unnötigerweise für die nicht angefügten Datenträger. Verwenden Sie die Informationen in diesem Artikel, um alle nicht angefügten Datenträger zu suchen und zu löschen und so Kosten zu sparen. 


## <a name="find-and-delete-unattached-managed-disks"></a>Suchen und Löschen nicht angefügter verwalteter Datenträger 

Das folgende Skript zeigt Ihnen, wie Sie nicht angefügte verwaltete Datenträger mithilfe der ManagedBy-Eigenschaft suchen.  Das Skript durchläuft alle verwalteten Datenträger in einem Abonnement und überprüft, ob die Eigenschaft *ManagedBy* NULL ist, um nicht angefügte verwaltete Datenträger zu finden. Die Eigenschaft *ManagedBy* speichert die Ressourcen-ID des virtuellen Computers, an den ein verwalteter Datenträger angefügt ist. 

Es wird dringend empfohlen, das Skript zuerst mit auf „0“ festgelegter Variable *deleteUnattachedDisks* auszuführen, um alle nicht angefügten Datenträger anzuzeigen. Nachdem Sie die nicht angefügten Datenträger überprüft haben, führen Sie das Skript mit auf „1“ festgelegter Variable *deleteUnattachedDisks* erneut aus, um alle nicht angefügten Datenträger zu löschen.

 ```azurecli

# Set deleteUnattachedDisks=1 if you want to delete unattached Managed Disks
# Set deleteUnattachedDisks=0 if you want to see the Id of the unattached Managed Disks
deleteUnattachedDisks=0

unattachedDiskIds=$(az disk list --query '[?managedBy==`null`].[id]' -o tsv)
for id in ${unattachedDiskIds[@]}
do
    if (( $deleteUnattachedDisks == 1 ))
    then

        echo "Deleting unattached Managed Disk with Id: "$id
        az disk delete --ids $id --yes
        echo "Deleted unattached Managed Disk with Id: "$id

    else
        echo $id
    fi
done

```
## <a name="find-and-delete-unattached-unmanaged-disks"></a>Suchen und Löschen nicht angefügter nicht verwalteter Datenträger 

Nicht verwaltete Datenträger sind VHD-Dateien, die als [Seitenblobs](/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-page-blobs) in [Azure Storage-Konten](../../storage/common/storage-create-storage-account.md) gespeichert sind. Das folgende Skript zeigt Ihnen, wie Sie nicht angefügte nicht verwaltete Datenträger (Seitenblobs) mithilfe der LeaseStatus-Eigenschaft suchen. Es durchläuft alle nicht verwalteten Datenträger in allen Speicherkonten in einem Abonnement und überprüft, ob die *LeaseStatus*-Eigenschaft entsperrt ist, um nicht angefügte nicht verwaltete Datenträger zu finden. Die *LeaseStatus*-Eigenschaft ist auf „locked“ festgelegt, wenn ein nicht verwalteter Datenträger an einen virtuellen Computer angefügt ist. 

Es wird dringend empfohlen, das Skript zuerst mit auf „0“ festgelegter Variable *deleteUnattachedVHDs* auszuführen, um alle nicht angefügten Datenträger anzuzeigen. Nachdem Sie die nicht angefügten Datenträger überprüft haben, führen Sie das Skript mit auf „1“ festgelegter Variable *deleteUnattachedVHDs* erneut aus, um alle nicht angefügten Datenträger zu löschen.


 ```azurecli
   
# Set deleteUnattachedVHDs=1 if you want to delete unattached VHDs
# Set deleteUnattachedVHDs=0 if you want to see the details of the unattached VHDs
deleteUnattachedVHDs=1

storageAccountIds=$(az storage account list --query [].[id] -o tsv)

for id in ${storageAccountIds[@]}
do
    connectionString=$(az storage account show-connection-string --ids $id --query connectionString -o tsv)
    containers=$(az storage container list --connection-string $connectionString --query [].[name] -o tsv)

    for container in ${containers[@]}
    do 
        
        blobs=$(az storage blob list -c $container --connection-string $connectionString --query "[?properties.blobType=='PageBlob' && ends_with(name,'.vhd')].[name]" -o tsv)
        
        for blob in ${blobs[@]}
        do
            leaseStatus=$(az storage blob show -n $blob -c $container --connection-string $connectionString --query "properties.lease.status" -o tsv)
            
            if [ "$leaseStatus" == "unlocked" ]
            then 

                if (( $deleteUnattachedVHDs == 1 ))
                then 

                    echo "Deleting VHD: "$blob" in container: "$container" in storage account: "$id

                    az storage blob delete --delete-snapshots include  -n $blob -c $container --connection-string $connectionString

                    echo "Deleted VHD: "$blob" in container: "$container" in storage account: "$id
                else
                    echo "StorageAccountId: "$id" container: "$container" VHD: "$blob
                fi

            fi
        done
    done
done 

```

## <a name="next-steps"></a>Nächste Schritte

[Löschen eines Speicherkontos](../../storage/common/storage-create-storage-account.md)



