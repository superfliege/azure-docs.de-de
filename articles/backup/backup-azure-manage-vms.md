---
title: Verwalten und Überwachen von Azure-VM-Sicherungen mit dem Azure Backup-Dienst
description: Erfahren Sie, wie Sie Azure-VM-Sicherungen mit dem Azure Backup-Dienst verwalten und überwachen.
services: backup
author: sogup
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 02/17/2019
ms.author: sogup
ms.openlocfilehash: f5c0373e2ef094a7fc5be64f4aeb8c0bb132e683
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2019
ms.locfileid: "56430062"
---
# <a name="manage-azure-vm-backups"></a>Verwalten von Azure-VM-Sicherungen

Dieser Artikel beschreibt die Verwaltung von Azure-VMs, die mit dem [Azure Backup-Dienst](backup-overview.md) gesichert wurden, und fasst die im Dashboard des Portals verfügbaren Informationen zu Sicherungswarnmeldungen zusammen.


Im Azure-Portal ermöglicht das Dashboard für den Recovery Services-Tresor den Zugriff auf Informationen zum Tresor, z.B.:

* Die letzte Sicherung, die gleichzeitig der letzte Wiederherstellungspunkt ist.
* Die Sicherungsrichtlinie
* Die Gesamtgröße aller Sicherungsmomentaufnahmen
* Die Anzahl der VMs, die für die Sicherung aktiviert sind.

Sie können Backups über das Dashboard und durch ein Drilldown auf einzelne VMs verwalten. Bei vielen Verwaltungsaufgaben mit einer VM-Sicherung muss zunächst der Tresor über das Dashboard geöffnet werden. 

![Vollständige Ansicht mit Schieberegler](./media/backup-azure-manage-vms/bottom-slider.png)

## <a name="view-vms-in-the-dashboard"></a>Anzeigen von VMs im Dashboard

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Klicken Sie im Hub-Menü auf **Durchsuchen**, und geben Sie in der Liste mit den Ressourcen **Recovery Services** ein. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Klicken Sie auf **Recovery Services-Tresor**. 
    ![Erstellen eines Recovery Services-Tresors – Schritt 1](./media/backup-azure-manage-vms/browse-to-rs-vaults.png)

3. Der Einfachheit halber klicken Sie mit der rechten Maustaste auf den Tresor in der List, und wählen Sie **An Dashboard anheften** aus.
3. Öffnen Sie das Dashboard des Tresors. 
    ![Tresor-Dashboard und Blatt „Einstellungen“ öffnen](./media/backup-azure-manage-vms/full-view-rs-vault.png)

4. Klicken Sie auf der Kachel **Sicherungselemente** auf **Virtuelle Azure-Computer**.

    ![Kachel „Sicherungselemente“ öffnen](./media/backup-azure-manage-vms/contoso-vault-1606.png)

5. Auf dem Blatt **Sicherungselemente** ist jeweils der letzte Sicherungsauftrag für jedes Element aufgeführt. In diesem Beispiel ist ein virtueller Computer „demovm-markgal“ vorhanden, der mit diesem Tresor geschützt wird.  

    ![Kachel „Sicherungselemente“](./media/backup-azure-manage-vms/backup-items-blade-select-item.png)

 
6. Im Dashboard für Tresorobjekte können Sie Sicherungsrichtlinien erstellen oder ändern, Wiederherstellungspunkte anzeigen, eine On-Demand-Sicherung durchführen, den Schutz von VMs beenden und wieder aufnehmen, Wiederherstellungspunkte löschen und eine Wiederherstellung durchführen.

    ![Sicherungselemente-Dashboard mit Blatt „Einstellungen“](./media/backup-azure-manage-vms/item-dashboard-settings.png)



## <a name="manage-backup-policies"></a>Verwalten von Sicherungsrichtlinien
1. Klicken Sie im [Dashboard für Tresorobjekte](backup-azure-manage-vms.md#open-a-vault-item-dashboard) auf **Alle Einstellungen**.

    ![Blatt „Sicherungsrichtlinie“](./media/backup-azure-manage-vms/all-settings-button.png)
2. Klicken Sie in den **Einstellungen** auf**Sicherungsrichtlinie**.
3. Im Menü **Sicherungsrichtlinie auswählen** :

   * Wählen Sie zum Ändern von Richtlinien eine andere Richtlinie aus, und klicken Sie auf **Speichern**. Die neue Richtlinie wird sofort auf den Tresor angewendet.
   * Wählen Sie zum Erstellen einer Richtlinie die Option **Neu erstellen**aus. [Weitere Informationen](backup-azure-arm-vms-prepare.md#configure-a-backup-policy)

     ![Sicherung virtueller Computer](./media/backup-azure-manage-vms/backup-policy-create-new.png)


## <a name="run-an-on-demand-backup"></a>Ausführen einer On-Demand-Sicherung
Sie können eine On-Demand-Sicherung einer VM erstellen, sobald dieser für den Schutz konfiguriert ist.
- Wenn die erste Sicherung ansteht, erstellt die On-Demand-Sicherung eine vollständige Kopie des virtuellen Computers im Recovery Services-Tresor.
- Nachdem die erste Sicherung durchgeführt wurde, werden bei einer On-Demand-Sicherung nur Änderungen der vorherigen Momentaufnahme an den Recovery Services-Tresor gesendet. Die nachfolgenden Sicherungen sind also immer inkrementell.
- Die Aufbewahrungsdauer für eine On-Demand-Sicherung ist der Aufbewahrungswert, der in der Richtlinie für den Sicherungspunkt „Täglich“ angegeben wird. Wenn kein täglicher Sicherungspunkt ausgewählt ist, wird der wöchentliche Sicherungspunkt verwendet.


So lösen Sie eine On-Demand-Sicherung aus:

1. Klicken Sie auf [Dashboard für die Tresorelemente](backup-azure-manage-vms.md#open-a-vault-item-dashboard) auf **Backup now**(Jetzt sichern).

    ![Schaltfläche „Jetzt sichern“](./media/backup-azure-manage-vms/backup-now-button.png)

 2. Klicken Sie auf **Ja** , um den Sicherungsauftrag zu starten.

    ![Schaltfläche „Jetzt sichern“](./media/backup-azure-manage-vms/backup-now-check.png)

 
 Der Sicherungsauftrag erstellt einen Wiederherstellungspunkt. Die Beibehaltungsdauer des Wiederherstellungspunkts entspricht der Beibehaltungsdauer, die in der Richtlinie für den virtuellen Computer angegeben ist. Klicken Sie zum Nachverfolgen des Auftragsstatus auf dem Tresordashboard auf die Kachel **Sicherungsaufträge** .  

## <a name="stop-protecting-a-vm"></a>Beenden des Schutzes für eine VM

Es gibt zwei Möglichkeiten, den Schutz der VM zu beenden:

- Beenden aller zukünftigen Sicherungsaufträge und Löschen aller Wiederherstellungspunkte In diesem Fall kann die VM nicht wiederhergestellt werden.
- Beenden aller zukünftigen Sicherungsaufträge und Beibehalten der Wiederherstellungspunkte. Mit der Beibehaltung der Wiederherstellungspunkte im Speicher sind Kosten verbunden. Der Vorteil der Beibehaltung von Wiederherstellungspunkten ist, dass Sie die VM bei Bedarf wiederherstellen können. [Weitere Informationen](https://azure.microsoft.com/pricing/details/backup/) zu Preisdetails.

So beenden Sie den Schutz für einen virtuellen Computer

1. Klicken Sie auf dem [Dashboard für die Tresorelemente](backup-azure-manage-vms.md#open-a-vault-item-dashboard)auf **Sicherung beenden**.

    ![Stop backup button](./media/backup-azure-manage-vms/stop-backup-button.png
2. Wählen Sie aus, ob die Sicherungsdaten beibehalten oder gelöscht werden sollen, und bestätigen Sie dies. Bestätigen Sie nach Bedarf, und geben Sie optional einen Kommentar an. Falls Sie unsicher sind, wie der Name lautet, können Sie mit der Maus auf das Ausrufezeichen zeigen, um den Namen anzuzeigen.

    ![Schutz beenden](./media/backup-azure-manage-vms/retain-or-delete-option.png)

 Sie werden mit einer Benachrichtigung darüber informiert, dass die Sicherungsaufträge beendet wurden.


## <a name="resume-protection-of-a-vm"></a>Fortsetzen des Schutzes einer VM

Wenn Sie Sicherungsdaten beibehalten haben, als die VM beendet wurde, können Sie den Schutz fortsetzen. Wenn Sie die Sicherungsdaten gelöscht haben, ist eine Fortsetzung nicht möglich.

Te

1. Klicken Sie auf dem [Dashboard für die Tresorelemente](backup-azure-manage-vms.md#open-a-vault-item-dashboard) auf **Sicherung fortsetzen**.

2. Führen Sie die Schritte unter [Verwalten von Sicherungsrichtlinien](backup-azure-manage-vms.md#manage-backup-policies) aus, um die Richtlinie für den virtuellen Computer zuzuweisen. Sie können eine andere Richtlinie als die Richtlinie auswählen, gemäß der der virtuelle Computer zuvor geschützt wurde.
3. Nachdem die Sicherungsrichtlinie auf den virtuellen Computer angewendet wurde, wird die folgende Meldung angezeigt.

    ![Erfolgreich geschützte VM](./media/backup-azure-manage-vms/success-message.png)

## <a name="delete-backup-data"></a>Löschen von Sicherungsdaten

Sie können die Sicherungsdaten, die einer VM zugeordnet sind, während des Auftrags **Sicherung beenden** oder jederzeit nach Abschluss des Sicherungsauftrags löschen.

- Es kann gegebenenfalls sogar von Vorteil sein, mit dem Löschen der Wiederherstellungspunkte mehrere Tage oder Wochen zu warten.
- Im Gegensatz zur Wiederherstellung von Wiederherstellungspunkten können Sie beim Löschen von Sicherungsdaten keine bestimmten Wiederherstellungspunkte auswählen, die gelöscht werden sollen. Wenn Sie sich für die Löschung Ihrer Sicherungsdaten entscheiden, werden alle dem Element zugeordneten Wiederherstellungspunkte gelöscht.

Bei diesem Verfahren wird davon ausgegangen, dass der Sicherungsauftrag für die VM beendet oder deaktiviert wurde.


1. Klicken Sie auf dem [Dashboard für die Tresorelemente](backup-azure-manage-vms.md#open-a-vault-item-dashboard)auf **Sicherung löschen**.

    ![VM-Typ](./media/backup-azure-manage-vms/delete-backup-buttom.png)

2. Geben Sie den Namen des Elements ein, um zu bestätigen, dass Sie die Wiederherstellungspunkte löschen möchten.

    ![Überprüfung beenden](./media/backup-azure-manage-vms/item-verification-box.png)

4. Klicken Sie auf ![Stop backup button](./media/backup-azure-manage-vms/delete-button.png) (Backup-Schaltfläche beenden), um die Sicherungsdaten für das aktuelle Element zu löschen.

    Sie werden mit einer Benachrichtigung darüber informiert, dass die Sicherungsdaten gelöscht wurden.

## <a name="next-steps"></a>Nächste Schritte
- [Erfahren Sie mehr](backup-azure-vms-first-look-arm.md) über das Sichern von Azure-VMs aus VM-Einstellungen.
- [Erfahren Sie mehr](backup-azure-arm-restore-vms.md) über das Wiederherstellen von VMs. 
- [Erfahren Sie mehr](backup-azure-monitor-vms.md) das Überwachen von Azure-VM-Sicherungen.
 
