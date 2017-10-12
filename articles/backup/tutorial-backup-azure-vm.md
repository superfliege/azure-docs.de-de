---
title: Effektives Sichern virtueller Azure-Computer | Microsoft-Dokumentation
description: "Dieses Tutorial enthält Informationen zum Sichern mehrerer virtueller Azure-Computer in einem Recovery Services-Tresor."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: Sicherung virtueller Computer; Sichern virtueller Computer; Sicherung und Notfallwiederherstellung
ms.assetid: 
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/06/2017
ms.author: trinadhk;jimpark;markgal;
ms.custom: 
ms.openlocfilehash: db4e1392acaeb2431d29a851113b7bc5a6dc1e9d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="back-up-azure-virtual-machines-in-azure-at-scale"></a>Effektives Sichern virtueller Azure-Computer in Azure

Dieses Tutorial enthält Informationen zum Sichern virtueller Azure-Computer in einem Recovery Services-Tresor. Beim Sichern von virtuellen Computern macht die Vorbereitung die meiste Arbeit. Bevor Sie einen virtuellen Computer sichern oder schützen können, müssen Sie die [Voraussetzungen](backup-azure-arm-vms-prepare.md) zum Vorbereiten der Umgebung auf den Schutz Ihrer VMs schaffen. 

> [!IMPORTANT]
> In diesem Tutorial wird davon ausgegangen, dass Sie bereits eine Ressourcengruppe und einen virtuellen Azure-Computer erstellt haben.

## <a name="create-a-recovery-services-vault"></a>Erstellen eines Recovery Services-Tresors

Ein [Recovery Services-Tresor](backup-azure-recovery-services-vault-overview.md) ist ein Container, in dem die Wiederherstellungspunkte für die zu sichernden Elemente gespeichert werden. Ein Recovery Services-Tresor ist eine Azure-Ressource, die als Teil einer Azure-Ressourcengruppe bereitgestellt und verwaltet werden kann. In diesem Tutorial erstellen Sie einen Recovery Services-Tresor in der gleichen Ressourcengruppe wie der zu schützende virtuelle Computer.


Bei der ersten Verwendung von Azure Backup müssen Sie sich mit Ihrem Abonnement beim Azure Recovery Service-Anbieter registrieren. Wenn Sie sich bereits mit Ihrem Abonnement beim Anbieter registriert haben, fahren Sie mit dem nächsten Schritt fort.

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.RecoveryServices
```

Erstellen Sie mithilfe des Cmdlets **New-AzureRmRecoveryServicesVault** einen Recovery Services-Tresor. Geben Sie unbedingt den Namen und Speicherort der Ressourcengruppe an, die für die Konfiguration des virtuellen Computers, den Sie sichern möchten, verwendet wurde. 

```powershell
New-AzureRmRecoveryServicesVault -Name myRSvault -ResourceGroupName "myResourceGroup" -Location "EastUS"
```

Viele Azure Backup-Cmdlets benötigen das Recovery Services-Tresorobjekt als Eingabe. Aus diesem Grund sollte das zur Sicherung verwendete Recovery Services-Tresorobjekt in einer Variablen gespeichert werden. Verwenden Sie dann **Set-AzureRmRecoveryServicesBackupProperties**, um die Option **-BackupStorageRedundancy** auf [Georedundanter Speicher (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage) festzulegen. 

```powershell
$vault1 = Get-AzureRmRecoveryServicesVault –Name myRSVault
Set-AzureRmRecoveryServicesBackupProperties  -vault $vault1 -BackupStorageRedundancy GeoRedundant
```

## <a name="back-up-azure-virtual-machines"></a>Sichern von virtuellen Azure-Computern

Vor dem Ausführen der ersten Sicherung müssen Sie den Tresorkontext festlegen. Der Tresorkontext ist der Datentyp, der im Tresor geschützt wird. Wenn Sie einen Recovery Services-Tresor erstellen, enthält er automatisch standardmäßige Schutz- und Aufbewahrungsrichtlinien. Die standardmäßige Schutzrichtlinie löst täglich zu einem angegebenen Zeitpunkt einen Sicherungsauftrag aus. Die standardmäßige Aufbewahrungsrichtlinie bewahrt den täglichen Wiederherstellungspunkt für 30 Tage auf. Übernehmen Sie für dieses Tutorial die Standardrichtlinie. 

Verwenden Sie **[Set-AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)**, um den Tresorkontext festzulegen. Sobald der Tresorkontext festgelegt ist, gilt er für alle nachfolgenden-Cmdlets. 

```powershell
Get-AzureRmRecoveryServicesVault -Name myRSVault | Set-AzureRmRecoveryServicesVaultContext
```

Verwenden Sie **[Backup-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/backup-azurermrecoveryservicesbackupitem)**, um einen Sicherungsauftrag auszulösen. Der Sicherungsauftrag erstellt einen Wiederherstellungspunkt. Wenn dies die erste Sicherung ist, ist der Wiederherstellungspunkt eine vollständige Sicherung. Bei nachfolgenden Sicherungen werden inkrementelle Kopien erstellt.

```powershell
$namedContainer = Get-AzureRmRecoveryServicesBackupContainer -ContainerType AzureVM -Status Registered -FriendlyName "V2VM"
$item = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer -WorkloadType AzureVM
$job = Backup-AzureRmRecoveryServicesBackupItem -Item $item
```

## <a name="delete-the-recovery-services-vault"></a>Löschen des Recovery Services-Tresors

Um einen Recovery Services-Tresor zu löschen, müssen Sie zunächst sämtliche Wiederherstellungspunkte im Tresor löschen und die Registrierung des Tresors aufheben. Die folgenden Befehle werden für diese Schritte verwendet. 


```powershell
$Cont = Get-AzureRmRecoveryServicesBackupContainer -ContainerType AzureVM -Status Registered
$PI = Get-AzureRmRecoveryServicesBackupItem -Container $Cont[0] -WorkloadType AzureVm
Disable-AzureRmRecoveryServicesBackupProtection -RemoveRecoveryPoints $PI[0]
Unregister-AzureRmRecoveryServicesBackupContainer -Container $namedContainer
Remove-AzureRmRecoveryServicesVault -Vault $vault1
```

## <a name="troubleshooting-errors"></a>Problembehandlung
Wenn beim Sichern des virtuellen Computers Probleme auftreten, finden Sie Hilfe im Artikel [Beheben von Problemen beim Sichern virtueller Azure-Computer](backup-azure-vms-troubleshoot.md).

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie Ihre virtuellen Computer geschützt haben, können Sie sich mit den folgenden Artikeln über Verwaltungsaufgaben und über die Wiederherstellung virtueller Computer aus einem Wiederherstellungspunkt informieren.

* Informationen zum Ändern der Sicherungsrichtlinie finden Sie unter [Verwenden von AzureRM.RecoveryServices.Backup-Cmdlets zum Sichern virtueller Computer](backup-azure-vms-automation.md#create-a-protection-policy).
* [Verwalten und Überwachen Ihrer virtuellen Computer](backup-azure-manage-vms.md)
* [Wiederherstellen virtueller Computer](backup-azure-arm-restore-vms.md)
