---
title: "Azure PowerShell-Skriptbeispiel – Sichern eines virtuellen Azure-Computers | Microsoft-Dokumentation"
description: "Azure PowerShell-Skriptbeispiel – Sichern eines virtuellen Azure-Computers"
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
tags: 
ms.assetid: 
ms.service: backup
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 09/07/2017
ms.author: markgal
ms.custom: mvc
ms.openlocfilehash: 4376add4a2e51806bd5db228ad2fe2afcf2e4f57
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
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


| Befehl | Hinweise | 
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

