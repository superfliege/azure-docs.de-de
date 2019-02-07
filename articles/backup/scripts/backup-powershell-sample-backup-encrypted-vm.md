---
title: Azure PowerShell-Skriptbeispiel – Sichern eines virtuellen Azure-Computers | Microsoft-Dokumentation
description: Azure PowerShell-Skriptbeispiel – Sichern eines virtuellen Azure-Computers
services: backup
documentationcenter: ''
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: sample
ms.date: 01/31/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 915c5f6c8e8de1b5a7a7590ba41125cbff7b8f36
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55497637"
---
# <a name="back-up-an-encrypted-azure-virtual-machine-with-powershell"></a>Sichern eines verschlüsselten virtuellen Azure-Computers mit PowerShell

Dieses Skript erstellt einen Recovery Services-Tresor mit georedundantem Speicher (Geo-Redundant Storage, GRS) für einen verschlüsselten virtuellen Azure-Computer. Die Standardschutzrichtlinie wird auf den Tresor angewendet. Die Richtlinie generiert täglich eine Sicherung des virtuellen Computers und behält diese für 30 Tage bei. Das Skript löst auch den ersten Wiederherstellungspunkt für den virtuellen Computer aus und behält diesen Wiederherstellungspunkt für 365 Tage bei. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/backup/backup-encrypted-vm/backup-encrypted-vm.ps1 "Back up encrypted virtual machine")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, den virtuellen Computer und alle zugehörigen Ressourcen zu entfernen.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle zum Erstellen der Bereitstellung. Jedes Element in der Tabelle ist mit der befehlsspezifischen Dokumentation verknüpft.


| Get-Help | Notizen | 
|---|---| 
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. | 
| [New-AzureRmRecoveryServicesVault](/powershell/module/azurerm.recoveryservices/New-AzureRmRecoveryServicesVault) | Erstellt einen Recovery Services-Tresor zum Speichern von Sicherungen | 
| [Set-AzureRmRecoveryServicesBackupProperties](/powershell/module/azurerm.recoveryservices/Set-AzureRmRecoveryServicesBackupProperties) | Legt die Sicherungsspeichereigenschaften für den Recovery Services-Tresor fest | 
| [New-AzureRmRecoveryServicesBackupProtectionPolicy](/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)| Erstellt eine Schutzrichtlinie mithilfe der Planungsrichtlinie und der Aufbewahrungsrichtlinie im Recovery Services-Tresor. | 
| [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) | Legt Berechtigungen für Key Vault fest, um dem Dienstprinzipal Zugriff auf Verschlüsselungsschlüssel zu gewähren. | 
| [Enable-AzureRmRecoveryServicesBackupProtection](/powershell/module/azurerm.recoveryservices.backup/enable-azurermrecoveryservicesbackupprotection) | Aktiviert die Sicherung für ein Element mit einer bestimmten Azure Backup-Schutzrichtlinie | 
| [Set-AzureRmRecoveryServicesBackupProtectionPolicy](/powershell/module/azurerm.recoveryservices.backup/set-azurermrecoveryservicesbackupprotectionpolicy)| Ändert eine vorhandene Azure Backup-Schutzrichtlinie | 
| [Backup-AzureRmRecoveryServicesBackupItem](/powershell/module/azurerm.recoveryservices.backup/backup-azurermrecoveryservicesbackupitem) | Startet eine Sicherung für ein geschütztes Azure Backup-Element, das nicht an den Sicherungszeitplan gebunden ist |
| [Wait-AzureRmRecoveryServicesBackupJob](/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob) | Wartet auf die Fertigstellung eines Azure Backup-Auftrags | 
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Entfernt eine Ressourcengruppe und alle darin enthaltenen Ressourcen. | 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

