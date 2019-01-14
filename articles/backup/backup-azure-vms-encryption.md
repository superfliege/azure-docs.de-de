---
title: Sichern und Wiederherstellen von verschlüsselten virtuellen Computern mit Azure Backup
description: In diesem Artikel werden die Sicherung und Wiederherstellung von virtuellen Computern mit Azure Disk Encryption beschrieben.
services: backup
author: sogup
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 7/10/2018
ms.author: sogup
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 40f8f1e9470201292c9dee27187d2155c879e13b
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/18/2018
ms.locfileid: "53583067"
---
# <a name="back-up-and-restore-encrypted-virtual-machines-with-azure-backup"></a>Sichern und Wiederherstellen verschlüsselter virtueller Computer mit Azure Backup
In diesem Artikel werden die Schritte zum Sichern und Wiederherstellen von virtuellen Computern mit Azure Backup beschrieben. Außerdem enthält er Details zu unterstützten Szenarien, Voraussetzungen und Problembehandlungsschritten für Fehler.

## <a name="supported-scenarios"></a>Unterstützte Szenarien

 Sicherung und Wiederherstellung von verschlüsselten virtuellen Computern werden nur für virtuelle Computer unterstützt, die das Azure Resource Manager-Bereitstellungsmodell verwenden. Für virtuelle Computer, die das klassische Bereitstellungsmodell verwenden, werden diese Vorgänge nicht unterstützt. Sicherung und Wiederherstellung verschlüsselter virtueller Computer werden für virtuelle Windows- und Linux-Computer unterstützt, die Azure Disk Encryption nutzen. Disk Encryption verwendet das Branchenstandardfeature „BitLocker“ von Windows und das Feature „dm-crypt“ von Linux für die Verschlüsselung von Datenträgern. Die folgende Tabelle zeigt den Verschlüsselungstyp und die Unterstützung für VMs.

   |  | BEK- + KEK-VMs | Virtuelle Computer nur mit BEK |
   | --- | --- | --- |
   | **Nicht verwaltete virtuelle Computer**  | JA | JA  |
   | **Verwaltete VMs**  | JA | JA  |

   > [!NOTE]
   > Azure Backup unterstützt mit eigenständigen Schlüsseln verschlüsselte VMs. Schlüssel, die Teil eines zum Verschlüsseln eines virtuellen Computers verwendeten Zertifikats sind, werden derzeit nicht unterstützt.
   >   

## <a name="prerequisites"></a>Voraussetzungen
* Der virtuelle Computer wurde mit [Azure Disk Encryption](../security/azure-security-disk-encryption.md) verschlüsselt.

* Anhand der Schritte unter [Vorbereiten der Umgebung für die Sicherung von mit Resource Manager bereitgestellten virtuellen Computern](backup-azure-arm-vms-prepare.md) wurde ein Recovery Services-Tresor erstellt und die Speicherreplikation eingerichtet.

* Backup wurden die [Berechtigungen zum Zugriff auf den Schlüsseltresor](#provide-permissions-to-backup) mit Schlüsseln und Geheimnissen für verschlüsselte virtuelle Computer gewährt.

## <a name="backup-encrypted-vm"></a>Mit Backup verschlüsselter virtueller Computer
Führen Sie die folgenden Schritte zum Festlegen des Sicherungsziels, Definieren der Richtlinie, Konfigurieren von Elementen und Auslösen der Sicherung aus.

### <a name="configure-backup"></a>Konfigurieren der Sicherung
1. Falls Sie bereits über einen geöffneten Recovery Services-Tresor verfügen, können Sie mit dem nächsten Schritt fortfahren. Wenn Sie keinen Recovery Services-Tresor geöffnet haben und sich im Azure-Portal befinden, wählen Sie **Alle Dienste** aus.

   a. Geben Sie in der Liste mit den Ressourcen **Recovery Services**ein.

   b. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Wählen Sie **Recovery Services-Tresore**, wenn der Eintrag angezeigt wird.

      ![Recovery Services-Tresor](./media/backup-azure-vms-encryption/browse-to-rs-vaults.png) <br/>

    c. Die Liste mit den Recovery Services-Tresoren wird angezeigt. Wählen Sie in der Liste einen Tresor aus.

     Das ausgewählte Tresor-Dashboard wird geöffnet.
1. Klicken Sie in der Liste mit den Elementen, die im Tresor angezeigt werden, auf **Sicherung**, um die Sicherung von verschlüsselten virtuellen Computern zu starten.

      ![Blatt „Sicherung“](./media/backup-azure-vms-encryption/select-backup.png)
1. Klicken Sie auf der Kachel **Sicherung** auf **Sicherungsziel**.

      ![Blatt „Szenario“](./media/backup-azure-vms-encryption/select-backup-goal-one.png)
1. Wählen Sie für **Wo wird Ihre Workload ausgeführt?** die Option **Azure** aus. Wählen Sie für **Was möchten Sie sichern?** die Option **Virtueller Computer** aus. Wählen Sie dann **OK**aus.

   ![Blatt „Szenario“ öffnen](./media/backup-azure-vms-encryption/select-backup-goal-two.png)
1. Wählen Sie unter **Sicherungsrichtlinie auswählen** die Sicherungsrichtlinie aus, die Sie auf den Tresor anwenden möchten. Wählen Sie dann **OK**aus.

      ![Sicherungsrichtlinie auswählen](./media/backup-azure-vms-encryption/setting-rs-backup-policy-new.png)

    Die Details zur Standardrichtlinie werden aufgeführt. Wählen Sie in der Dropdownliste die Option **Neu erstellen**, wenn Sie eine Richtlinie erstellen möchten. Nachdem Sie auf **OK** geklickt haben, wird die Sicherungsrichtlinie dem Tresor zugeordnet.

1. Wählen Sie die verschlüsselten virtuellen Computer aus, die der angegebenen Richtlinie zugeordnet werden sollen, und klicken Sie auf **OK**.

      ![Auswählen von verschlüsselten VMs](./media/backup-azure-vms-encryption/selected-encrypted-vms.png)
1. Auf dieser Seite wird eine Meldung mit einem Hinweis zu Schlüsseltresoren angezeigt, die den ausgewählten verschlüsselten virtuellen Computern zugeordnet sind. Für Backup ist Lesezugriff auf die Schlüssel und Geheimnisse im Schlüsseltresor erforderlich. Diese Berechtigungen werden verwendet, um die Schlüssel und Geheimnisse zusammen mit den zugeordneten virtuellen Computern zu sichern.<br>
Falls Sie ein **Mitgliedsbenutzer** sind, erhält der Prozess zum Aktivieren der Sicherung ohne Benutzereingriff Zugriff auf den Schlüsseltresor, um verschlüsselte virtuelle Computer zu sichern.

   ![Meldung zu verschlüsselten VMs](./media/backup-azure-vms-encryption/member-user-encrypted-vm-warning-message.png)

   Für einen **Gastbenutzer** müssen Sie dem Sicherungsdienst die Berechtigungen zum Zugriff auf den Schlüsseltresor gewähren, damit Sicherungen funktionieren. Sie können diese Berechtigungen mit den [im folgenden Abschnitt beschriebenen Schritten](#provide-permissions-to-backup) gewähren.

   ![Meldung zu verschlüsselten VMs](./media/backup-azure-vms-encryption/guest-user-encrypted-vm-warning-message.png)

    Nachdem Sie nun alle Einstellungen für den Tresor definiert haben, klicken Sie unten auf der Seite auf **Sicherung aktivieren**. Mit **Sicherung aktivieren** wird die Richtlinie im Tresor und auf den virtuellen Computern bereitgestellt.

1. Die nächste Phase der Vorbereitung ist die Installation des VM-Agents, bzw. sicherzustellen, dass der VM-Agent installiert ist. Führen Sie dazu die Schritte unter [Vorbereiten der Umgebung für die Sicherung von mit Resource Manager bereitgestellten virtuellen Computern](backup-azure-arm-vms-prepare.md) aus.

### <a name="trigger-a-backup-job"></a>Auslösen eines Sicherungsauftrags
Führen Sie die Schritte unter [Sichern von virtuellen Azure-Computern in einem Recovery Services-Tresor](backup-azure-arm-vms.md) aus, um einen Sicherungsauftrag auszulösen.

### <a name="continue-backups-of-already-backed-up-vms-with-encryption-enabled"></a>Fortsetzen von Sicherungen von bereits gesicherten virtuellen Computern mit aktivierter Verschlüsselung  
Wenn virtuelle Computer bereits in einem Recovery Services-Tresor gesichert und später für die Verschlüsselung aktiviert werden, müssen Sie Azure Backup Zugriffsberechtigungen für den Schlüsseltresor erteilen, damit die Sicherungen fortgesetzt werden können. Sie können diese Berechtigungen mit den [Schritten im folgenden Abschnitt](#provide-permissions-to-azure-backup) gewähren. Alternativ können Sie die PowerShell-Schritte im Abschnitt zur Aktivierung der Sicherung in der [PowerShell-Dokumentation](backup-azure-vms-automation.md) ausführen.

## <a name="provide-permissions-to-azure-backup"></a>Gewähren von Berechtigungen für Backup
Gehen Sie anhand der folgenden Schritte vor, um Backup die erforderlichen Berechtigungen für den Zugriff auf den Schlüsseltresor und für die Ausführung der Sicherung von verschlüsselten virtuellen Computern zu gewähren:
1. Wählen Sie **Alle Dienste** aus, und suchen Sie nach **Schlüsseltresore**.

    ![Schlüsseltresore](./media/backup-azure-vms-encryption/search-key-vault.png)

1. Wählen Sie in der Liste der Schlüsseltresore den Schlüsseltresor aus, der dem verschlüsselten virtuellen Computer zugeordnet ist, der gesichert werden muss.

     ![Auswahl der Schlüsseltresore](./media/backup-azure-vms-encryption/select-key-vault.png)

1. Wählen Sie **Zugriffsrichtlinien** aus, und klicken Sie auf **Neue hinzufügen**.

    ![Neue hinzufügen](./media/backup-azure-vms-encryption/select-key-vault-access-policy.png)

1. Klicken Sie auf **Prinzipal auswählen**, und geben Sie im Suchfeld **Sicherungsverwaltungsdienst** ein.

    ![Suche nach dem Sicherungsdienst](./media/backup-azure-vms-encryption/search-backup-service.png)

1. Wählen Sie **Sicherungsverwaltungsdienst** aus, und klicken Sie dann auf **Auswählen**.

    ![Auswählen des Sicherungsdiensts](./media/backup-azure-vms-encryption/select-backup-service.png)

1. Wählen Sie unter **Anhand einer Vorlage konfigurieren (optional)** die Option **Azure Backup** aus. Unter **Schlüsselberechtigungen** und **Berechtigungen für Geheimnis** sind bereits die erforderlichen Berechtigungen angegeben. Wenn Ihr virtueller Computer mithilfe von **Nur BEK** verschlüsselt wird, sind Berechtigungen nur für Geheimnisse erforderlich, sodass Sie die Auswahl für **Schlüsselberechtigungen** entfernen müssen.

    ![Auswählen von Azure Backup](./media/backup-azure-vms-encryption/select-backup-template.png)

1. Klicken Sie auf **OK**. Beachten Sie, dass **Sicherungsverwaltungsdienst** unter **Zugriffsrichtlinien** hinzugefügt wird.

    ![Zugriffsrichtlinien](./media/backup-azure-vms-encryption/backup-service-access-policy.png)

1. Klicken Sie auf **Speichern**, damit Backup die erforderlichen Berechtigungen erhält.

    ![Backup-Zugriffsrichtlinie](./media/backup-azure-vms-encryption/save-access-policy.png)

Nachdem Berechtigungen gewährt wurden, können Sie damit fortfahren, die Sicherung für verschlüsselte virtuelle Computer zu aktivieren.

## <a name="restore-an-encrypted-vm"></a>Wiederherstellen eines verschlüsselten virtuellen Computers
Stellen Sie zum Wiederherstellen verschlüsselter virtueller Computer zunächst Datenträger gemäß den Schritten im Abschnitt „Wiederherstellen von gesicherten Datenträgern“ unter [Auswählen einer Konfiguration für die VM-Wiederherstellung](backup-azure-arm-restore-vms.md#choose-a-vm-restore-configuration) wieder her. Sie können anschließend eine der folgenden Optionen wählen:

* Führen Sie die unter [Erstellen eines virtuellen Computers aus wiederhergestellten Datenträgern](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) beschriebenen PowerShell-Schritte aus, um auf der Grundlage der wiederhergestellten Datenträger einen vollständigen virtuellen Computer zu erstellen.
* Oder [verwenden Sie Vorlagen zum Anpassen eines wiederhergestellten virtuellen Computers](backup-azure-arm-restore-vms.md#use-templates-to-customize-a-restored-vm), um virtuelle Computer auf der Grundlage von wiederhergestellten Datenträgern zu erstellen. Vorlagen können nur für Wiederherstellungspunkte verwendet werden, die nach dem 26. April 2017 erstellt wurden.

## <a name="troubleshooting-errors"></a>Problembehandlung
| Vorgang | Fehlerdetails | Lösung |
| --- | --- | --- |
|Backup | Backup verfügt nicht über ausreichende Berechtigungen für den Schlüsseltresor zur Sicherung verschlüsselter virtueller Computer. | Backup müssen diese Berechtigungen anhand der [Schritte im vorherigen Abschnitt](#provide-permissions-to-azure-backup) gewährt werden. Alternativ können Sie die PowerShell-Schritte im Abschnitt „Schutz aktivieren“ des Artikels [Verwenden von PowerShell zum Sichern und Wiederherstellen von virtuellen Computern](backup-azure-vms-automation.md#enable-protection) ausführen. |  
| Restore |Sie können diesen verschlüsselten virtuellen Computer nicht wiederherstellen, da der diesem virtuellen Computer zugeordnete Schlüsseltresor nicht vorhanden ist. |Erstellen Sie einen Schlüsseltresor anhand der Vorgehensweise unter [Erste Schritte mit dem Azure-Schlüsseltresor](../key-vault/key-vault-get-started.md). Verwenden Sie die Informationen unter [Wiederherstellen von Key Vault-Schlüssel und -Geheimschlüssel für verschlüsselte virtuelle Computer mithilfe von Azure Backup](backup-azure-restore-key-secret.md), um einen Schlüssel und ein Geheimnis wiederherzustellen, falls diese Angaben nicht schon vorhanden sind. |
| Restore |Sie können diesen verschlüsselten virtuellen Computer nicht wiederherstellen, da der Schlüssel und das Geheimnis, der bzw. das diesem virtuellen Computer zugeordnet ist, nicht vorhanden sind. |Verwenden Sie die Informationen unter [Wiederherstellen von Key Vault-Schlüssel und -Geheimschlüssel für verschlüsselte virtuelle Computer mithilfe von Azure Backup](backup-azure-restore-key-secret.md), um einen Schlüssel und ein Geheimnis wiederherzustellen, falls diese Angaben nicht schon vorhanden sind. |
| Restore |Backup ist nicht zum Zugreifen auf Ressourcen in Ihrem Abonnement autorisiert. |Stellen Sie wie zuvor erwähnt Datenträger zunächst gemäß den Schritten im Abschnitt „Wiederherstellen von gesicherten Datenträgern“ unter [Auswählen einer Konfiguration für die VM-Wiederherstellung](backup-azure-arm-restore-vms.md#choose-a-vm-restore-configuration) wieder her. Verwenden Sie anschließend PowerShell zum [Erstellen eines virtuellen Computers aus wiederhergestellten Datenträgern](backup-azure-vms-automation.md#create-a-vm-from-restored-disks). |
