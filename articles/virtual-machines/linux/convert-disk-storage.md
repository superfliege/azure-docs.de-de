---
title: Konvertieren zwischen dem Standardspeicher und Storage Premium für verwaltete Azure-Datenträger | Microsoft-Dokumentation
description: Vorgehensweise zum Konvertieren zwischen dem Standardspeicher und Storage Premium für verwaltete Azure-Datenträger mithilfe der Azure CLI
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/12/2018
ms.author: cynthn
ms.openlocfilehash: 783dd82041e21242ac4ac136a5d9c91fc9b94e84
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2018
ms.locfileid: "43666776"
---
# <a name="convert-azure-managed-disks-storage-from-standard-to-premium-and-vice-versa"></a>Konvertieren zwischen dem Standardspeicher und Storage Premium für verwaltete Azure-Datenträger

Managed Disks bietet drei Speicheroptionen: [Premium-SSDs](../windows/premium-storage.md), Standard-SSDs (Vorschauversion) und [Standard-Festplattenlaufwerke](../windows/standard-storage.md). Entsprechend Ihren Leistungsanforderungen können Sie problemlos zwischen den Optionen wechseln – und das bei minimaler Ausfallzeit. Dies wird für nicht verwaltete Datenträger nicht unterstützt. Sie können jedoch problemlos eine [Konvertierung in verwaltete Datenträger](convert-unmanaged-to-managed-disks.md) durchführen, um einfach zwischen den Datenträgertypen zu wechseln.

In diesem Artikel wird die Konvertierung zwischen verwalteten Standard- und Premium-Datenträgern mithilfe der Azure CLI erläutert. Wenn Sie die Befehlszeilenschnittstelle installieren oder aktualisieren müssen, finden Sie unter [Installieren von Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) Informationen dazu. 

## <a name="before-you-begin"></a>Voraussetzungen

* Da für die Konvertierung ein Neustart der VM erforderlich ist, sollten Sie die Migration Ihres Datenträgerspeichers während eines bereits vorhandenen Wartungsfensters durchführen. 
* Wenn Sie nicht verwaltete Datenträger verwenden, [konvertieren Sie sie zuerst in verwaltete Datenträger](convert-unmanaged-to-managed-disks.md), um mithilfe der Anweisungen in diesem Artikel zwischen den Speicheroptionen zu wechseln. 


## <a name="convert-all-the-managed-disks-of-a-vm-from-standard-to-premium-and-vice-versa"></a>Konvertieren zwischen dem Standardspeicher und Storage Premium für alle verwalteten Datenträger einer VM

Im folgenden Beispiel wird gezeigt, wie Sie für alle Datenträger einer VM eine Konvertierung eines Standard-Speichers zu Storage Premium durchführen. Zur Verwendung von verwalteten Premium-Datenträgern muss Ihre VM eine [VM-Größe](sizes.md) verwenden, die Premium-Speicher unterstützt. In diesem Beispiel erfolgt ein Wechsel zu einer Größe, die Premium-Speicher unterstützt.

 ```azurecli

#resource group that contains the virtual machine
rgName='yourResourceGroup'

#Name of the virtual machine
vmName='yourVM'

#Premium capable size 
#Required only if converting from standard to premium
size='Standard_DS2_v2'

#Choose between Standard_LRS and Premium_LRS based on your scenario
sku='Premium_LRS'

#Deallocate the VM before changing the size of the VM
az vm deallocate --name $vmName --resource-group $rgName

#Change the VM size to a size that supports premium storage 
#Skip this step if converting storage from premium to standard
az vm resize --resource-group $rgName --name $vmName --size $size

#Update the sku of all the data disks 
az vm show -n $vmName -g $rgName --query storageProfile.dataDisks[*].managedDisk -o tsv \
 | awk -v sku=$sku '{system("az disk update --sku "sku" --ids "$1)}'

#Update the sku of the OS disk
az vm show -n $vmName -g $rgName --query storageProfile.osDisk.managedDisk -o tsv \
| awk -v sku=$sku '{system("az disk update --sku "sku" --ids "$1)}'

az vm start --name $vmName --resource-group $rgName

```
## <a name="convert-a-managed-disk-from-standard-to-premium-and-vice-versa"></a>Konvertieren zwischen dem Standardspeicher und Storage Premium für einen verwalteten Datenträger

Für Ihre Entwicklungs-/Testworkload sollten Sie eine Kombination aus Standard- und Premium-Datenträger verwenden, um Ihre Kosten zu senken. Führen Sie hierfür nur für die Datenträger, die eine höhere Leistung erfordern, ein Upgrade auf Storage Premium durch. Im folgenden Beispiel wird gezeigt, wie Sie für einen einzelnen Datenträger einer VM eine Konvertierung eines Standard-Speichers zu Storage Premium und umgekehrt durchführen. Zur Verwendung von verwalteten Premium-Datenträgern muss Ihre VM eine [VM-Größe](sizes.md) verwenden, die Premium-Speicher unterstützt. In diesem Beispiel erfolgt ein Wechsel zu einer Größe, die Premium-Speicher unterstützt.

 ```azurecli

#resource group that contains the managed disk
rgName='yourResourceGroup'

#Name of your managed disk
diskName='yourManagedDiskName'

#Premium capable size 
#Required only if converting from standard to premium
size='Standard_DS2_v2'

#Choose between Standard_LRS and Premium_LRS based on your scenario
sku='Premium_LRS'

#Get the parent VM Id 
vmId=$(az disk show --name $diskName --resource-group $rgName --query managedBy --output tsv)

#Deallocate the VM before changing the size of the VM
az vm deallocate --ids $vmId 

#Change the VM size to a size that supports premium storage 
#Skip this step if converting storage from premium to standard
az vm resize --ids $vmId --size $size

# Update the sku
az disk update --sku $sku --name $diskName --resource-group $rgName 

az vm start --ids $vmId 
```

## <a name="convert-a-managed-disk-from-standard-hdd-to-standard-ssd-and-vice-versa"></a>Konvertieren eines verwalteten Datenträgers eines Standard-Festplattenlaufwerks zu einer Standard-SSD und umgekehrt

Im folgenden Beispiel wird gezeigt, wie Sie für einen einzelnen Datenträger einer VM eine Konvertierung eines Standard-Festplattenlaufwerks zu einem Standard-SSD und umgekehrt durchführen.

 ```azurecli

#resource group that contains the managed disk
rgName='yourResourceGroup'

#Name of your managed disk
diskName='yourManagedDiskName'

#Choose between Standard_LRS and StandardSSD_LRS based on your scenario
sku='StandardSSD_LRS'

#Get the parent VM Id 
vmId=$(az disk show --name $diskName --resource-group $rgName --query managedBy --output tsv)

#Deallocate the VM before changing the disk type
az vm deallocate --ids $vmId 

# Update the sku
az disk update --sku $sku --name $diskName --resource-group $rgName 

az vm start --ids $vmId 
```

## <a name="convert-using-the-azure-portal"></a>Konvertieren über das Azure-Portal

Sie haben auch die Möglichkeit, nicht verwaltete Datenträger über das Azure-Portal in verwaltete Datenträger zu konvertieren.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie die VM in der Liste der VMs im Portal aus.
3. Wählen Sie auf dem Blatt für die VM aus dem Menü die Option **Datenträger** aus.
4. Wählen Sie oben auf dem Blatt **Datenträger** die Option **Zu Managed Disks migrieren** aus.
5. Wenn sich Ihre VM in einer Verfügbarkeitsgruppe befindet, wird auf dem Blatt **Zu Managed Disks migrieren** eine Warnung angezeigt, die besagt, dass Sie die Verfügbarkeitsgruppe zuerst konvertieren müssen. Die Warnung sollte einen Link enthalten, über den Sie die Verfügbarkeitsgruppe konvertieren können. Sobald die Verfügbarkeitsgruppe konvertiert wurde, oder wenn sich Ihre VM nicht in einer Verfügbarkeitsgruppe befindet, klicken Sie auf **Migrieren**, um den Prozess der Migration Ihrer Datenträger zu Managed Disks zu starten. 

Nach Abschluss der Migration wird die VM angehalten und neu gestartet.

## <a name="next-steps"></a>Nächste Schritte

Erstellen Sie eine schreibgeschützte Kopie eines virtuellen Computers mit [Momentaufnahmen](snapshot-copy-managed-disk.md).

