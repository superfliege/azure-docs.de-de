---
title: "Azure-Schnellstart – Sichern einer VM mit PowerShell | Microsoft-Dokumentation"
description: Es wird beschrieben, wie Sie Ihre virtuellen Computer mit Azure PowerShell sichern.
services: virtual-machines-windows, azure-backup
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager, virtual-machine-backup
ms.assetid: 
ms.service: virtual-machines-windows, azure-backup
ms.devlang: azurecli
ms.topic: hero-article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 09/18/2017
ms.author: iainfou
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 7dceb7bb38b1dac778151e197db3b5be49dd568a
ms.openlocfilehash: 3ca0e9d905e23e25b57b46454399ad12e2890d52
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="back-up-a-virtual-machine-in-azure-with-powershell"></a>Sichern eines virtuellen Computers in Azure mit PowerShell
Das Azure PowerShell-Modul dient zum Erstellen und Verwalten von Azure-Ressourcen über die Befehlszeile oder mit Skripts. Sie können Ihre Daten schützen, indem Sie in regelmäßigen Abständen Sicherungen erstellen. Azure Backup erstellt Wiederherstellungspunkte, die in georedundanten Recovery-Tresoren gespeichert werden können. In diesem Artikel wird ausführlich beschrieben, wie Sie einen virtuellen Computer (VM) mit dem Azure PowerShell-Modul sichern. Sie können diese Schritte auch mit der [Azure CLI](quick-backup-vm-cli.md) oder mit dem [Azure-Portal](quick-backup-vm-portal.md) ausführen.

Dieser Schnellstart ermöglicht die Sicherung auf einer vorhandenen Azure-VM. Wenn Sie eine VM erstellen müssen, können Sie die [Erstellung mit Azure PowerShell durchführen](../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json).

Für diesen Schnellstart ist das Azure PowerShell-Modul Version 4.4 oder höher erforderlich. Führen Sie ` Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Install and configure Azure PowerShell](/powershell/azure/install-azurerm-ps) (Installieren des Azure PowerShell-Moduls) Informationen dazu.


## <a name="log-in-to-azure"></a>Anmelden an Azure
Melden Sie sich mit dem Befehl `Login-AzureRmAccount` bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm.

```powershell
Login-AzureRmAccount
```

Bei der ersten Verwendung von Azure Backup müssen Sie den Azure Recovery Service-Anbieter in Ihrem Abonnement mit [Register-AzureRmResourceProvider](/powershell/module/AzureRM.Resources/Register-AzureRmResourceProvider) registrieren.

```powershell
Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
```


## <a name="create-a-recovery-services-vault"></a>Erstellen eines Recovery Services-Tresors
Ein Recovery Services-Tresor ist ein logischer Container, in dem die Sicherungsdaten für jede geschützte Ressource, z.B. Azure-VMs, gespeichert werden. Wenn der Sicherungsauftrag für eine geschützte Ressource ausgeführt wird, wird im Recovery Services-Tresor ein Wiederherstellungspunkt erstellt. Sie können einen dieser Wiederherstellungspunkte dann verwenden, um Daten für einen bestimmten Zeitpunkt wiederherzustellen.

Erstellen Sie mithilfe des Cmdlets [New-AzureRmRecoveryServicesVault](/powershell/module/azurerm.recoveryservices/new-azurermrecoveryservicesvault) einen Recovery Services-Tresor. Geben Sie dieselbe Ressourcengruppe und denselben Standort wie für die zu schützende VM an. Wenn Sie das [Beispielskript](../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json) zum Erstellen Ihrer VM verwendet haben, hat die Ressourcengruppe den Namen *myResourceGroup*, die VM heißt *myVM* und die Ressourcen befinden sich am Standort *WestEurope*.

```powershell
New-AzureRmRecoveryServicesVault `
    -ResourceGroupName "myResourceGroup" `
    -Name "myRecoveryServicesVault" `
    -Location "WestEurope"
```

Für den Tresor ist standardmäßig die georedundante Speicherung festgelegt. Als weiterer Schutz für Ihre Daten wird auf dieser Speicherredundanzebene sichergestellt, dass Ihre Sicherungsdaten in einer sekundären Azure-Region repliziert werden, die Hunderte Kilometer von der primären Region entfernt ist.

Legen Sie den Tresorkontext mit [Set-AzureRmRecoveryServicesVaultContext](/powershell/module/AzureRM.RecoveryServices/Set-AzureRmRecoveryServicesVaultContext) fest, um diesen Tresor für die verbleibenden Schritte zu verwenden.

```powershell
Get-AzureRmRecoveryServicesVault `
    -Name "myRecoveryServicesVault" | Set-AzureRmRecoveryServicesVaultContext
```


## <a name="enable-backup-for-an-azure-vm"></a>Aktivieren der Sicherung für eine Azure-VM
Sie erstellen und verwenden Richtlinien, um zu definieren, wann ein Sicherungsauftrag ausgeführt wird und wie lange die Wiederherstellungspunkte gespeichert werden. Bei der Standardschutzrichtlinie wird jeden Tag ein Sicherungsauftrag ausgeführt, und Wiederherstellungspunkte werden 30 Tage lang beibehalten. Sie können diese Standardrichtlinienwerte verwenden, um Ihre VM schnell zu schützen. Legen Sie zuerst mit [Get-AzureRmRecoveryServicesBackupProtectionPolicy](/powershell/module/AzureRM.RecoveryServices.Backup/Get-AzureRmRecoveryServicesBackupProtectionPolicy) die Standardrichtlinie fest:

```powershell
$policy = Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "DefaultPolicy"
```

Verwenden Sie [Enable-AzureRmRecoveryServicesBackupProtection](/powershell/module/AzureRM.RecoveryServices.Backup/Enable-AzureRmRecoveryServicesBackupProtection), um den Sicherungsschutz für eine VM zu aktivieren. Geben Sie die Richtlinie und dann die Ressourcengruppe und die zu schützende VM an:

```powershell
Enable-AzureRmRecoveryServicesBackupProtection `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVM" `
    -Policy $policy
```


## <a name="start-a-backup-job"></a>Starten eines Sicherungsauftrags
Verwenden Sie [Backup-AzureRmRecoveryServicesBackupItem](/powershell/module/azurerm.recoveryservices.backup/backup-azurermrecoveryservicesbackupitem), um sofort einen Sicherungsvorgang zu starten, anstatt zu warten, bis die Standardrichtlinie den Auftrag zum geplanten Zeitpunkt ausführt. Bei diesem ersten Sicherungsauftrag wird ein vollständiger Wiederherstellungspunkt erstellt. Bei jedem Sicherungsauftrag nach diesem ersten Sicherungsvorgang werden dann inkrementelle Wiederherstellungspunkte erstellt. Inkrementelle Wiederherstellungspunkte sind in Bezug auf die Speicherung und die Dauer effizient, da nur Änderungen übertragen werden, die seit der letzten Sicherung vorgenommen wurden.

Im folgenden Befehlssatz geben Sie einen Container im Recovery Services-Tresor an, der Ihre Sicherungsdaten enthält, indem Sie [Get-AzureRmRecoveryServicesBackupContainer](/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupcontainer) verwenden. Jede zu sichernde VM wird als gesondertes Element behandelt. Rufen Sie Informationen zu Ihrem VM-Element mit [Get-AzureRmRecoveryServicesBackupItem](/powershell/module/AzureRM.RecoveryServices.Backup/Get-AzureRmRecoveryServicesBackupItem) ab, um einen Sicherungsauftrag zu starten.

```powershell
$backupcontainer = Get-AzureRmRecoveryServicesBackupContainer `
    -ContainerType "AzureVM" `
    -FriendlyName "myVM"

$item = Get-AzureRmRecoveryServicesBackupItem `
    -Container $backupcontainer `
    -WorkloadType "AzureVM"

Backup-AzureRmRecoveryServicesBackupItem -Item $item
```

Da bei diesem ersten Sicherungsauftrag ein vollständiger Wiederherstellungspunkt erstellt wird, kann der Vorgang bis zu 20 Minuten dauern.


## <a name="monitor-the-backup-job"></a>Überwachen des Sicherungsauftrags
Verwenden Sie [Get-AzureRmRecoveryservicesBackupJob](/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupjob), um den Status von Sicherungsaufträgen zu überwachen:

```powershell
Get-AzureRmRecoveryservicesBackupJob
```

Die Ausgabe ähnelt dem folgenden Beispiel, bei dem für den Sicherungsauftrag der Status **InProgress** angezeigt wird:

```
WorkloadName   Operation         Status       StartTime              EndTime                JobID
------------   ---------         ------       ---------              -------                -----
myvm           Backup            InProgress   9/18/2017 9:38:02 PM                          9f9e8f14
myvm           ConfigureBackup   Completed    9/18/2017 9:33:18 PM   9/18/2017 9:33:51 PM   fe79c739
```

Wenn der *Status* des Sicherungsauftrags als *Completed* gemeldet wird, ist Ihre VM durch Recovery Services geschützt und verfügt über einen gespeicherten vollständigen Wiederherstellungspunkt.


## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung
Wenn die Komponenten nicht mehr benötigt werden, können Sie den Schutz auf der VM deaktivieren, die Wiederherstellungspunkte und den Recovery Services-Tresor entfernen und anschließend die Ressourcengruppe und die dazugehörigen VM-Ressourcen löschen. Wenn Sie eine vorhandene VM verwendet haben, können Sie das abschließende [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup)-Cmdlet überspringen, um die Ressourcengruppe und die VM beizubehalten.

Wenn Sie mit einem Backup-Tutorial fortfahren möchten, in dem die Wiederherstellung von Daten für Ihre VM beschrieben wird, können Sie die Schritte in diesem Abschnitt überspringen und mit [Nächste Schritte](#next-steps) fortfahren. 

```powershell
Disable-AzureRmRecoveryServicesBackupProtection -Item $item -RemoveRecoveryPoints
$vault = Get-AzureRmRecoveryServicesVault -Name "myRecoveryServicesVault"
Remove-AzureRmRecoveryServicesVault -Vault $vault
Remove-AzureRmResourceGroup -Name "myResourceGroup"
```


## <a name="next-steps"></a>Nächste Schritte
In diesem Schnellstart haben Sie einen Recovery Services-Tresor erstellt, den Schutz auf einer VM aktiviert und den ersten Wiederherstellungspunkt erstellt. Weitere Informationen zu Azure Backup und Recovery Services sind in den Tutorials enthalten.

> [!div class="nextstepaction"]
> [Sichern von mehreren Azure-VMs](./tutorial-backup-vm-at-scale.md)
