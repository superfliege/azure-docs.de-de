---
title: Aktivieren der Sicherung virtueller Azure-Computer beim Erstellen
description: Hier erfahren Sie, wie Sie im Rahmen des Erstellungsprozesses die Sicherung virtueller Azure-Computer aktivieren.
services: backup, virtual-machines
author: markgalioto
manager: carmonm
tags: azure-resource-manager, virtual-machine-backup
ms.service: backup, virtual-machines
ms.topic: conceptual
ms.date: 01/08/2018
ms.author: trinadhk
ms.openlocfilehash: 928481f07875286a21f68dae6556f04eb2b6ae5c
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34606119"
---
# <a name="enable-backup-during-azure-virtual-machine-creation"></a>Aktivieren der Sicherung während der Erstellung virtueller Azure-Computer 

Der Azure Backup-Dienst stellt eine Schnittstelle zum Erstellen und Konfigurieren von Sicherungen in der Cloud bereit. Schützen Sie Ihre Daten, indem Sie in regelmäßigen Abständen Sicherungen (so genannte Wiederherstellungspunkte) erstellen. Azure Backup erstellt Wiederherstellungspunkte, die in georedundanten Recovery-Tresoren gespeichert werden können. In diesem Artikel erfahren Sie, wie Sie die Sicherung während der Erstellung eines virtuellen Computers (Virtual Machine, VM) über das Azure-Portal aktivieren.  

## <a name="log-in-to-azure"></a>Anmelden an Azure 

Falls Sie noch nicht bei Ihrem Konto angemeldet sind, melden Sie beim [Azure-Portal](http://portal.azure.com) an.
 
## <a name="create-virtual-machine-with-backup-configured"></a>Erstellen eines virtuellen Computers mit konfigurierter Sicherung 

1. Klicken Sie im Azure-Portal oben links auf **Neu**. 

2. Klicken Sie auf **Compute**, und wählen Sie ein VM-Image aus.   

3. Geben Sie die Informationen zum virtuellen Computer ein. Der hier angegebene Benutzername und das Kennwort werden zum Anmelden am virtuellen Computer verwendet. Klicken Sie zum Abschluss auf **OK**. 

4. Wählen Sie eine Größe für den virtuellen Computer.  

5. Klicken Sie unter **Einstellungen > Sicherung** auf **Aktiviert**, um die Einstellungen der Sicherungskonfiguration anzuzeigen. Sie können die Standardwerte auf der Einstellungsseite übernehmen und auf **OK** klicken, um zur Zusammenfassungsseite zu gelangen und den virtuellen Computer zu erstellen. Falls Sie die Werte ändern möchten, folgen Sie den nächsten Schritten.  

6. Erstellen Sie einen Recovery Services-Tresor zum Speichern der Sicherungen des virtuellen Computers, oder wählen Sie einen Recovery Services-Tresor aus. Wenn Sie einen Recovery Services-Tresor erstellen, können Sie eine Ressourcengruppe für den Tresor auswählen.  

    ![Sicherungskonfiguration auf der VM-Erstellungsseite](./media/backup-during-vm-creation/create-vm-backup-config.png) 

    > [!NOTE] 
    > Die Ressourcengruppe für den Recovery Services-Tresor kann sich von der Ressourcengruppe für den virtuellen Computer unterscheiden.  
    > 
    > 

7. Für Sie wird standardmäßig eine Sicherungsrichtlinie ausgewählt, um den virtuellen Computer schnell zu schützen. Eine Sicherungsrichtlinie gibt an, wie häufig Sicherungsmomentaufnahmen erstellt und wie lang die Sicherungskopien aufbewahrt werden sollen. Sie können die Standardrichtlinie übernehmen oder eine andere Sicherungsrichtlinie erstellen oder auswählen. Wenn Sie die Sicherungsrichtlinie bearbeiten möchten, klicken Sie auf **Sicherungsrichtlinie**, und ändern Sie die Werte der Richtlinie.  

8. Wenn Sie mit den Werten der Sicherungskonfiguration zufrieden sind, klicken Sie auf der Einstellungsseite auf **OK**.  

9. Klicken Sie auf der Zusammenfassungsseite nach Abschluss der Überprüfung auf **Erstellen**, um einen virtuellen Computer mit den konfigurierten Sicherungseinstellungen zu erstellen. 

## <a name="initiate-a-backup-after-creating-the-vm"></a>Initiieren einer Sicherung nach dem Erstellen des virtuellen Computers 

Auch wenn die Sicherungsrichtlinie erstellt wurde, empfiehlt es sich, eine erste Sicherung zu erstellen. Klicken Sie nach Abschluss der VM-Erstellungsvorlage unter der Einstellung **Vorgänge** im linken Menü auf **Sicherung**, um die Sicherungsdetails für den virtuellen Computer anzuzeigen. Hier können Sie eine bedarfsgesteuerte Sicherung auslösen, einen vollständigen virtuellen Computer oder alle Datenträger wiederherstellen, Dateien aus einer VM-Sicherung wiederherstellen oder die dem virtuellen Computer zugeordnete Sicherungsrichtlinie ändern.  

## <a name="using-a-resource-manager-template-to-deploy-a-protected-vm"></a>Bereitstellen einer geschützten VM mithilfe einer Resource Manager-Vorlage

Die vorherigen Schritte erklären, wie Sie über das Azure-Portal einen virtuellen Computer erstellen und ihn in einem Recovery Services-Tresor schützen. Wenn Sie einen oder mehrere virtuelle Computer schnell bereitstellen und in einem Recovery Services-Tresor schützen möchten, finden Sie Informationen in der Vorlage zum [Bereitstellen einer Windows-VM und Aktivieren der Sicherung](https://azure.microsoft.com/resources/templates/101-recovery-services-create-vm-and-configure-backup/).

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen 

### <a name="which-vm-images-enable-backup-at-the-time-of-vm-creation"></a>Welche VM-Images ermöglichen die Sicherung bei der VM-Erstellung? 

Die von Microsoft veröffentlichten Core-Images in der folgenden Liste unterstützen die Aktivierung der Sicherung bei der VM-Erstellung. Für andere virtuelle Computer können Sie die Sicherung nach der VM-Erstellung aktivieren. Weitere Informationen finden Sie unter [Sichern eines virtuellen Computers in Azure](quick-backup-vm-portal.md). 

- **Windows:** Windows Server 2016 Datacenter, Windows Server 2016 Datacenter (Core), Windows Server 2012 Datacenter, Windows Server 2012 R2 Datacenter, Windows Server 2008 R2 SP1 
- **Ubuntu:** Ubuntu Server 1710, Ubuntu Server 1704, Ubuntu Server 1604 (LTS), Ubuntu Server 1404 (LTS) 
- **Red Hat:** RHEL 6.7, 6.8, 6.9, 7.2, 7.3, 7.4 
- **SUSE:** SUSE Linux Enterprise Server 11 SP4, 12 SP2, 12 SP3 
- **Debian:** Debian 8, Debian 9 
- **CentOS:** CentOS 6.9, CentOS 7.3 
- **Oracle Linux:** Oracle Linux 6.7, 6.8, 6.9, 7.2, 7.3 
 
### <a name="is-backup-cost-included-in-the-vm-cost"></a>Sind die Sicherungskosten in den VM-Kosten enthalten? 

Nein. Die Sicherungskosten sind von den Kosten für virtuelle Computer getrennt. Weitere Informationen zu Sicherungspreisen finden Sie unter [Backup – Preise](https://azure.microsoft.com/pricing/details/backup/).
 
### <a name="which-permissions-are-required-to-enable-backup-on-a-vm"></a>Welche Berechtigungen werden zum Aktivieren der Sicherung für einen virtuellen Computer benötigt? 

Als Mitwirkender für virtuelle Computer können Sie die Sicherung für den virtuellen Computer aktivieren. Bei Verwendung einer benutzerdefinierten Rolle benötigen Sie folgende Berechtigungen, um die Sicherung für den virtuellen Computer aktivieren zu können. 

- Microsoft.RecoveryServices/Vaults/write 
- Microsoft.RecoveryServices/Vaults/read 
- Microsoft.RecoveryServices/locations/* 
- Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/*/read 
- Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read 
- Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write 
- Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write 
- Microsoft.RecoveryServices/Vaults/backupPolicies/read 
- Microsoft.RecoveryServices/Vaults/backupPolicies/write 
 
Wenn der Recovery Services-Tresor und der virtuelle Computer unterschiedliche Ressourcengruppen haben, achten Sie darauf, dass Sie in der Ressourcengruppe des Recovery Services-Tresors über Schreibberechtigungen verfügen.  

## <a name="next-steps"></a>Nächste Schritte 

Nachdem Sie Ihren virtuellen Computer geschützt haben, können Sie sich mit den folgenden Artikeln über Verwaltungsaufgaben für virtuelle Computer und über die Wiederherstellung virtueller Computer informieren. 

- [Verwalten und Überwachen Ihrer virtuellen Computer](backup-azure-manage-vms.md) 
- [Wiederherstellen virtueller Computer](backup-azure-arm-restore-vms.md) 
