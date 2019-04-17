---
title: Azure Site Recovery – Ausschließen von Datenträgern während der Replikation virtueller Azure-Computer über Azure PowerShell | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Datenträger für virtuelle Azure-Computer mit Azure Site Recovery über Azure PowerShell ausschließen.
services: site-recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 02/18/2019
ms.author: asgang
ms.openlocfilehash: 1c278d810df7e5ba8701529a59987c9bb16fa40c
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/05/2019
ms.locfileid: "59044117"
---
# <a name="exclude-disks-from-replication-of-azure-vms-to-azure-using-azure-powershell"></a>Ausschließen von Datenträgern von der Replikation virtueller Azure-Computer in Azure mithilfe von Azure PowerShell

In diesem Artikel wird beschrieben, wie Datenträger von der Replikation virtueller Azure-Computer ausgeschlossen werden. Durch diesen Ausschluss können die beanspruchte Replikationsbandbreite oder die zielseitigen Ressourcen optimiert werden, die solche Datenträger verwenden. Derzeit wird diese Funktion nur durch Azure PowerShell bereitgestellt.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Voraussetzungen

Vorbereitung:

- Stellen Sie sicher, dass Sie die [Architektur und die Komponenten des Szenarios](azure-to-azure-architecture.md) verstehen.
- Überprüfen Sie die [Supportanforderungen](azure-to-azure-support-matrix.md) für alle Komponenten.
- Sie verfügen über das Azure PowerShell-Modul `Az`. Wenn Sie PowerShell installieren oder aktualisieren müssen, befolgen Sie die Anweisungen unter [Handbuch zum Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/install-az-ps).
- Sie haben bereits einen Recovery Services-Tresor erstellt und den Schutz virtueller Computer mindestens einmal durchgeführt. Wenn nicht, verwenden Sie dazu die [hier](azure-to-azure-powershell.md) erwähnte Dokumentation.

## <a name="why-exclude-disks-from-replication"></a>Gründe für das Ausschließen von Datenträgern von der Replikation
Das Ausschließen von Datenträgern von der Replikation ist häufig aus folgenden Gründen erforderlich:

- Ihr virtueller Computer hat die [Azure Site Recovery-Grenzwerte zur Replikation von Datenänderungsraten](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-support-matrix) erreicht.

- Die auf dem ausgeschlossenen Datenträger verarbeiteten Daten sind nicht wichtig oder müssen nicht repliziert werden.

- Sie können Speicher- und Netzwerkressourcen sparen, indem Sie diese Daten nicht replizieren.


## <a name="how-to-exclude-disks-from-replication"></a>Ausschließen von Datenträgern von der Replikation

In dem Beispiel in diesem Artikel wird ein virtueller Computer mit einem Betriebssystem und drei Datenträgern in der Region „USA, Osten“ in die Region „USA, Westen 2“ repliziert. Der Name des virtuellen Computers im Beispiel lautet AzureDemoVM. Datenträger 1 wird ausgeschlossen und die Datenträger 2 und 3 beibehalten.

## <a name="get-details-of-the-virtual-machines-to-be-replicated"></a>Abrufen von Details der zu replizierenden virtuellen Computer

```azurepowershell
# Get details of the virtual machine
$VM = Get-AzVM -ResourceGroupName "A2AdemoRG" -Name "AzureDemoVM"

Write-Output $VM     
```

```
ResourceGroupName  : A2AdemoRG
Id                 : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/A2AdemoRG/providers/Microsoft.Compute/virtualMachines/AzureDemoVM
VmId               : 1b864902-c7ea-499a-ad0f-65da2930b81b
Name               : AzureDemoVM
Type               : Microsoft.Compute/virtualMachines
Location           : eastus
Tags               : {}
DiagnosticsProfile : {BootDiagnostics}
HardwareProfile    : {VmSize}
NetworkProfile     : {NetworkInterfaces}
OSProfile          : {ComputerName, AdminUsername, WindowsConfiguration, Secrets}
ProvisioningState  : Succeeded
StorageProfile     : {ImageReference, OsDisk, DataDisks}
```


Rufen Sie detaillierte Informationen zu den Datenträgern des virtuellen Computers ab. Die Datenträgerdetails werden später beim Starten der Replikation für den virtuellen Computer verwendet.

```azurepowershell
$OSDiskVhdURI = $VM.StorageProfile.OsDisk.Vhd
$DataDisk1VhdURI = $VM.StorageProfile.DataDisks[0].Vhd
```

## <a name="replicate-azure-virtual-machine"></a>Replizieren eines virtuellen Azure-Computers

Im folgenden Beispiel wird davon ausgegangen, dass Sie bereits über ein Cachespeicherkonto, eine Replikationsrichtlinie und Zuordnungen verfügen. Wenn nicht, verwenden Sie dazu die [hier](azure-to-azure-powershell.md) erwähnte Dokumentation. 


Replizieren Sie den virtuellen Azure-Computer mit **verwalteten Datenträgern**.

```azurepowershell

#Get the resource group that the virtual machine must be created in when failed over.
$RecoveryRG = Get-AzResourceGroup -Name "a2ademorecoveryrg" -Location "West US 2"

#Specify replication properties for each disk of the VM that is to be replicated (create disk replication configuration)

#OsDisk
$OSdiskId =  $vm.StorageProfile.OsDisk.ManagedDisk.Id
$RecoveryOSDiskAccountType = $vm.StorageProfile.OsDisk.ManagedDisk.StorageAccountType
$RecoveryReplicaDiskAccountType =  $vm.StorageProfile.OsDisk.ManagedDisk.StorageAccountType

$OSDiskReplicationConfig = New-AzRecoveryServicesAsrAzureToAzureDiskReplicationConfig -ManagedDisk -LogStorageAccountId $EastUSCacheStorageAccount.Id `
         -DiskId $OSdiskId -RecoveryResourceGroupId  $RecoveryRG.ResourceId -RecoveryReplicaDiskAccountType  $RecoveryReplicaDiskAccountType `
         -RecoveryTargetDiskAccountType $RecoveryOSDiskAccountType

# Data Disk 1 i.e StorageProfile.DataDisks[0] is excluded so we will provide it during the time of replication 

# Data disk 2
$datadiskId2  = $vm.StorageProfile.DataDisks[1].ManagedDisk.id
$RecoveryReplicaDiskAccountType =  $vm.StorageProfile.DataDisks[1]. StorageAccountType
$RecoveryTargetDiskAccountType = $vm.StorageProfile.DataDisks[1]. StorageAccountType

$DataDisk2ReplicationConfig  = New-AzRecoveryServicesAsrAzureToAzureDiskReplicationConfig -ManagedDisk -LogStorageAccountId $CacheStorageAccount.Id `
         -DiskId $datadiskId2 -RecoveryResourceGroupId  $RecoveryRG.ResourceId -RecoveryReplicaDiskAccountType  $RecoveryReplicaDiskAccountType `
         -RecoveryTargetDiskAccountType $RecoveryTargetDiskAccountType

# Data Disk 3

$datadiskId3  = $vm.StorageProfile.DataDisks[2].ManagedDisk.id
$RecoveryReplicaDiskAccountType =  $vm.StorageProfile.DataDisks[2]. StorageAccountType
$RecoveryTargetDiskAccountType = $vm.StorageProfile.DataDisks[2]. StorageAccountType

$DataDisk3ReplicationConfig  = New-AzRecoveryServicesAsrAzureToAzureDiskReplicationConfig -ManagedDisk -LogStorageAccountId $CacheStorageAccount.Id `
         -DiskId $datadiskId3 -RecoveryResourceGroupId  $RecoveryRG.ResourceId -RecoveryReplicaDiskAccountType  $RecoveryReplicaDiskAccountType `
         -RecoveryTargetDiskAccountType $RecoveryTargetDiskAccountType

#Create a list of disk replication configuration objects for the disks of the virtual machine that are to be replicated.
$diskconfigs = @()
$diskconfigs += $OSDiskReplicationConfig, $DataDisk2ReplicationConfig, $DataDisk3ReplicationConfig


#Start replication by creating replication protected item. Using a GUID for the name of the replication protected item to ensure uniqueness of name.
$TempASRJob = New-ASRReplicationProtectedItem -AzureToAzure -AzureVmId $VM.Id -Name (New-Guid).Guid -ProtectionContainerMapping $EusToWusPCMapping -AzureToAzureDiskReplicationConfiguration $diskconfigs -RecoveryResourceGroupId $RecoveryRG.ResourceId
```

Sobald der Replikationsvorgang erfolgreich gestartet wurde, werden Daten des virtuellen Computers in die Wiederherstellungsregion repliziert.

Sie können zum Azure-Portal navigieren und unter den replizierten Elemente sehen, welche virtuellen Computer repliziert werden.
Zu Beginn des Replikationsprozesses wird zunächst durch Seeding eine Kopie der zu replizierenden Datenträger des virtuellen Computers in die Wiederherstellungsregion übernommen. Diese Phase wird als erste Replikationsphase bezeichnet.

Sobald die erste Replikation abgeschlossen ist, wird in die Phase der differenziellen Synchronisierung gewechselt. Jetzt ist der virtuelle Computer geschützt. Klicken Sie auf den geschützten virtuellen Computer und dann auf „Datenträger“, um zu sehen, ob der Datenträger ausgeschlossen wurde oder nicht.

## <a name="next-steps"></a>Nächste Schritte

[Erfahren Sie mehr](site-recovery-test-failover-to-azure.md) über die Ausführung von Testfailovervorgängen.
