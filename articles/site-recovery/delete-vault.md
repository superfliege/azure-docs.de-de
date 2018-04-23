---
title: Löschen eines Site Recovery-Tresors
description: Hier wird anhand des Site Recovery-Szenarios erläutert, wie Sie einen Azure Site Recovery-Tresor löschen.
service: site-recovery
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.date: 03/05/2018
ms.author: rajani-janaki-ram
ms.openlocfilehash: 62792747b4efe2de4c22af6f0886503d7d63ed44
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2018
---
# <a name="delete-a-site-recovery-vault"></a>Löschen eines Site Recovery-Tresors
Abhängigkeiten können das Löschen eines Azure Site Recovery-Tresors verhindern. Die auszuführenden Aktionen variieren basierend auf dem Site Recovery-Szenario: VMware zu Azure, Hyper-V (mit und ohne System Center Virtual Machine Manager) zu Azure und Azure Backup. Informationen zum Löschen eines in Azure Backup verwendeten Tresors finden Sie unter [Löschen eines Azure Backup-Tresors](../backup/backup-azure-delete-vault.md).



## <a name="delete-a-site-recovery-vault"></a>Löschen eines Site Recovery-Tresors 
Befolgen Sie die empfohlenen Schritte für Ihr Szenario, um den Tresor zu löschen:

### <a name="vmware-vms-to-azure"></a>Replizieren von VMware-VMs in Azure

1. Löschen Sie alle geschützten virtuellen Computer anhand der Schritte unter [Deaktivieren des Schutzes für eine VMware-VM oder einen physischen Server](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure).

2. Löschen Sie alle Replikationsrichtlinien anhand der Schritte unter [Löschen einer Replikationsrichtlinie](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy).

3. Löschen Sie Verweise auf vCenter anhand der Schritte unter [Manage VMware vCenter servers](vmware-azure-manage-vcenter.md#delete-a-vcenter-server) (Verwalten von VMware vCenter-Servern).

4. Löschen Sie den Konfigurationsserver anhand der Schritte unter [Außerbetriebnahme eines Konfigurationsservers](vmware-azure-manage-configuration-server.md#delete-or-unregister-a-configuration-server).

5. Löschen Sie den Tresor.


### <a name="hyper-v-vms-with-virtual-machine-manager-to-azure"></a>Virtuelle Hyper-V-Computer (mit Virtual Machine Manager) zu Azure
1. Löschen Sie alle geschützten VMs, indem Sie die Schritte unter [Deaktivieren des Schutzes für einen virtuellen Hyper-V-Computer mit Replikation mithilfe von System Center VMM nach Azure](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-hyper-v-virtual-machine-replicating-to-azure-using-the-system-center-vmm-to-azure-scenario) durchführen.

2. Heben Sie die Zuordnung für alle Replikationsrichtlinien auf, und löschen Sie sie, indem Sie zu „Tresor“ > **Site Recovery-Infrastruktur** > **Für System Center VMM** -> **Replikationsrichtlinien** navigieren.

3.  Löschen Sie Verweise auf Virtual Machine Manager-Server anhand der Schritte unter [Aufheben der Registrierung eines verbundenen VMM-Servers](site-recovery-manage-registration-and-protection.md##unregister-a-vmm-server).

4.  Löschen Sie den Tresor.

### <a name="hyper-v-vms-without-virtual-machine-manager-to-azure"></a>Virtuelle Hyper-V-Computer (ohne Virtual Machine Manager) zu Azure
1. Löschen Sie alle geschützten VMs anhand der Schritte unter [Deaktivieren des Schutzes für einen virtuellen Hyper-V-Computer (Hyper-V nach Azure)](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-hyper-v-virtual-machine-hyper-v-to-azure).

2. Heben Sie die Zuordnung für alle Replikationsrichtlinien auf, und löschen Sie sie, indem Sie zu „Tresor“ > **Site Recovery-Infrastruktur** > **Für Hyper-V-Sites** -> **Replikationsrichtlinien** navigieren.

3. Löschen Sie Verweise auf Hyper-V-Server anhand der Schritte unter [Unregister a Hyper-V host](/site-recovery-manage-registration-and-protection.md##unregister-a-hyper-v-host-in-a-hyper-v-site) (Aufheben der Registrierung eines Hyper-V-Hosts).

4. Löschen Sie den Hyper-V-Standort.

5. Löschen Sie den Tresor.


## <a name="use-powershell-to-force-delete-the-vault"></a>Verwenden von PowerShell, um das Löschen des Tresors zu erzwingen 

> [!Important]
> Wenn Sie das Produkts testen und keine Bedenken bezüglich eines Datenverlusts haben, können Sie mit der Löschen erzwingen-Methode den Tresor und alle Abhängigkeiten schnell entfernen.
> Der PowerShell-Befehl löscht den gesamten Inhalt des Tresors und ist **nicht umkehrbar**.

Verwenden Sie die folgenden Befehle, um den Site Recovery-Tresor zu löschen, selbst wenn geschützte Elemente vorhanden sind:

    Connect-AzureRmAccount

    Select-AzureRmSubscription -SubscriptionName "XXXXX"

    $vault = Get-AzureRmSiteRecoveryVault -Name "vaultname"

    Remove-AzureRmSiteRecoveryVault -Vault $vault
