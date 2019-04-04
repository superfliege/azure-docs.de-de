---
title: Aktivieren der Sicherung virtueller Azure-Computer beim Erstellen
description: Hier erfahren Sie, wie Sie die Sicherung virtueller Azure-Computer im Rahmen des Erstellungsprozesses aktivieren.
services: backup, virtual-machines
author: rayne-wiselman
manager: carmonm
tags: azure-resource-manager, virtual-machine-backup
ms.service: backup
ms.topic: conceptual
ms.date: 01/08/2018
ms.author: trinadhk
ms.openlocfilehash: fd2beaa39f03d4f2342c94bf1cd8b8aea7440e62
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2019
ms.locfileid: "57780442"
---
# <a name="enable-backup-when-you-create-an-azure-virtual-machine"></a>Aktivieren der Sicherung beim Erstellen eines virtuellen Azure-Computers

Verwenden Sie den Azure Backup-Dienst zum Sichern von Azure-VMs (virtuelle Computer). VMs werden gemäß eines Zeitplans gesichert, der in einer Sicherungsrichtlinie festgelegt wird. Die Wiederherstellungspunkte werden aus Sicherungen erstellt. Die Wiederherstellungspunkte werden in Recovery Services-Tresoren gespeichert.

In diesem Artikel erfahren Sie, wie Sie die Sicherung während der Erstellung einer VM (virtueller Computer) über das Azure-Portal aktivieren.  

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Falls Sie noch nicht bei Ihrem Konto angemeldet sind, melden Sie beim [Azure-Portal](https://portal.azure.com) an.
 
## <a name="create-a-vm-with-backup-configured"></a>Erstellen einer VM mit konfigurierter Sicherung 

1. Klicken Sie in der oberen linken Ecke des Azure-Portals auf **Neu**.

1. Klicken Sie auf **Compute**, und wählen Sie ein VM-Image aus.

1. Geben Sie die Informationen die VM ein. Der Benutzername und das Kennwort, die Sie angeben, werden für die Anmeldung bei der VM verwendet. Klicken Sie auf **OK**, wenn Sie fertig sind. 

1. Wählen Sie eine Größe für den virtuellen Computer.  

1. Klicken Sie unter **Einstellungen** > **Sicherung** auf **Aktiviert**, um die Einstellungen der Sicherungskonfiguration anzuzeigen.

   - Klicken Sie auf der Seite **Einstellungen** auf **OK**, um die Standardwerte zu akzeptieren. Anschließend öffnen Sie die Seite **Zusammenfassung**, um die VM zu erstellen. Überspringen Sie die Schritte 6-8.
   - Führen Sie die folgenden Schritte aus, um die Werte der Sicherungskonfiguration zu ändern.  

1. Erstellen Sie einen Recovery Services-Tresor, oder wählen Sie einen aus, in dem die Sicherungen der VM gespeichert werden sollen. Wenn Sie einen Recovery Services-Tresor erstellen, können Sie eine Ressourcengruppe für den Tresor auswählen.  

    ![Einstellungen für die Sicherungskonfiguration auf der Seite zum Erstellen der VM](./media/backup-during-vm-creation/create-vm-backup-config.png) 

    > [!NOTE] 
    > Die Ressourcengruppe für den Recovery Services-Tresor kann sich von der Ressourcengruppe für die VM unterscheiden.  

1. Für Sie wird standardmäßig eine Sicherungsrichtlinie ausgewählt, um den virtuellen Computer zu schützen. Eine Sicherungsrichtlinie gibt an, wie häufig Sicherungsmomentaufnahmen erstellt und wie lang die Sicherungskopien aufbewahrt werden sollen. 

   - Sie können die Standardrichtlinie übernehmen oder eine andere Sicherungsrichtlinie erstellen oder auswählen. 
   - Wenn Sie die Sicherungsrichtlinie bearbeiten möchten, klicken Sie auf **Sicherungsrichtlinie**, und ändern Sie die Werte.  

1. Wenn Sie die Werte für die Sicherungskonfiguration festgelegt haben, klicken Sie auf der Seite **Einstellungen** auf **OK**.  

1. Klicken Sie nach der Überprüfung auf der Seite **Zusammenfassung** auf **Erstellen**, um eine VM zu erstellen, die die konfigurierten Sicherungseinstellungen verwendet. 

## <a name="start-a-backup-after-creating-the-vm"></a>Starten einer Sicherung nach dem Erstellen der VM 

Obwohl Sie eine Sicherungsrichtlinie für Ihre VM konfiguriert haben, wird empfohlen, dass Sie eine erste Sicherung erstellen. 

Klicken Sie nach Abschluss der VM-Erstellungsvorlage im linken Menü auf **Vorgänge**, und klicken Sie dann auf **Sicherung**, um die Sicherungsdetails für die VM anzuzeigen. Auf dieser Seite können Sie folgende Aktionen durchführen:

- Auslösen einer bedarfsgesteuerten Sicherung
- Wiederherstellen einer vollständigen VM oder all ihrer Datenträger
- Wiederherstellen von Dateien aus einer VM-Sicherung
- Ändern der Sicherungsrichtlinie, die der VM zugeordnet ist  

## <a name="use-a-resource-manager-template-to-deploy-a-protected-vm"></a>Bereitstellen einer geschützten VM mithilfe einer Resource Manager-Vorlage

In den vorherigen Schritten wurde erläutert, wie Sie eine VM über das Azure-Portal erstellen und in einem Recovery Services-Tresor schützen. Informationen zur schnellen Bereitstellung und Schützen von VMs in einem Recovery Services-Tresor finden Sie unter [Deploy a Windows VM and enable backup (Bereitstellen einer Windows-VM und Aktivieren der Sicherung)](https://azure.microsoft.com/resources/templates/101-recovery-services-create-vm-and-configure-backup/).

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen 

### <a name="which-vm-images-support-backup-configuration-during-vm-creation"></a>Welche VM-Images unterstützen die Sicherungskonfiguration während der Erstellung der VM?

Die folgenden von Microsoft veröffentlichten Core-Images unterstützen die Aktivierung der Sicherung bei der VM-Erstellung. Für andere VMs können Sie die Sicherung nach der VM-Erstellung aktivieren. Weitere Informationen finden Sie unter [Sichern eines virtuellen Computers in Azure](quick-backup-vm-portal.md).

- **Windows:** Windows Server 2016 Datacenter, Windows Server 2016 Datacenter Core, Windows Server 2012 Datacenter, Windows Server 2012 R2 Datacenter, Windows Server 2008 R2 SP1 
- **Ubuntu:** Ubuntu Server 17.10, Ubuntu Server 17.04, Ubuntu Server 16.04 (LTS), Ubuntu Server 14.04 (LTS) 
- **Red Hat:** RHEL 6.7, 6.8, 6.9, 7.2, 7.3, 7.4 
- **SUSE:** SUSE Linux Enterprise Server 11 SP4, 12 SP2, 12 SP3 
- **Debian:** Debian 8, Debian 9 
- **CentOS:** CentOS 6.9, CentOS 7.3 
- **Oracle Linux:** Oracle Linux 6.7, 6.8, 6.9, 7.2, 7.3 
 
### <a name="is-the-backup-cost-included-in-the-vm-cost"></a>Sind die Sicherungskosten in den VM-Kosten enthalten? 

 Nein. Die Sicherungskosten werden separat zu den Kosten einer VM berechnet. Weitere Informationen zu Sicherungspreisen finden Sie unter [Azure Backup – Preise](https://azure.microsoft.com/pricing/details/backup/).
 
### <a name="which-permissions-are-required-to-enable-backup-on-a-vm"></a>Welche Berechtigungen werden zum Aktivieren der Sicherung für einen virtuellen Computer benötigt? 

Wenn Sie ein „VM-Mitwirkender“ sind, können Sie die Sicherung der VM aktivieren. Wenn Sie eine benutzerdefinierte Rolle verwenden, benötigen Sie die folgenden Berechtigungen zum Aktivieren der Sicherung der VM: 

- Microsoft.RecoveryServices/Vaults/write 
- Microsoft.RecoveryServices/Vaults/read 
- Microsoft.RecoveryServices/locations/* 
- Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/*/read 
- Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read 
- Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write 
- Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write 
- Microsoft.RecoveryServices/Vaults/backupPolicies/read 
- Microsoft.RecoveryServices/Vaults/backupPolicies/write 
 
Stellen Sie sicher, dass Sie in der Ressourcengruppe für den Recovery Services-Tresor über Schreibberechtigungen verfügen, wenn der Recovery Services-Tresor und die VM unterschiedliche Ressourcengruppen aufweisen.  

## <a name="next-steps"></a>Nächste Schritte 

Da Ihre VM nun gesichert ist, erfahren Sie in den folgenden Artikeln mehr zum Verwalten und Wiederherstellen von VMs:

- [Verwalten und Überwachen Ihrer virtuellen Computer](backup-azure-manage-vms.md) 
- [Wiederherstellen virtueller Computer](backup-azure-arm-restore-vms.md) 
