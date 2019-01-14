---
title: 'Azure Backup: Wiederherstellen virtueller Computer über das Azure-Portal'
description: Wiederherstellen eines virtuellen Azure-Computers mithilfe eines Wiederherstellungspunkts über das Azure-Portal
services: backup
author: geethalakshmig
manager: vijayts
keywords: Sicherung wiederherstellen; Wiederherstellungsschritte; Wiederherstellungspunkt;
ms.service: backup
ms.topic: conceptual
ms.date: 09/04/2017
ms.author: geg
ms.openlocfilehash: ac0c727e41ad9b361ea9f558a97f16446c12ef0e
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/27/2018
ms.locfileid: "53793375"
---
# <a name="use-the-azure-portal-to-restore-virtual-machines"></a>Wiederherstellen virtueller Computer über das Azure-Portal
Schützen Sie Ihre Daten, indem Sie in festgelegten Abständen Momentaufnahmen Ihrer Daten erstellen. Diese Momentaufnahmen werden als Wiederherstellungspunkte bezeichnet. Sie werden in Recovery Services-Tresoren gespeichert. Wenn es erforderlich ist, einen virtuellen Computer zu reparieren oder wiederherzustellen, können Sie den virtuellen Computer auf der Grundlage eines gespeicherten Wiederherstellungspunkts wiederherstellen. Beim Herstellen eines Wiederherstellungspunkts haben Sie folgende Möglichkeiten:

* Erstellen eines neuen virtuellen Computers: über eine Zeitpunktwiederherstellung Ihrer gesicherten virtuellen Computers.
* Wiederherstellen von Datenträgern: Verwenden Sie die zugehörige Vorlage des Prozesses, um den wiederhergestellten virtuellen Computer anzupassen oder einzelne Dateien wiederherzustellen.

In diesem Artikel wird erläutert, wie ein virtueller Computer als neuer virtueller Computer wiederhergestellt wird oder alle gesicherten Datenträger wiederhergestellt werden. Informationen zur Wiederherstellung einzelner Dateien finden Sie unter [Wiederherstellen von Dateien aus einer Sicherung von virtuellen Azure-Computern](backup-azure-restore-files-from-vm.md).

![Drei Möglichkeiten zum Wiederherstellen auf der Grundlage einer VM-Sicherung](./media/backup-azure-arm-restore-vms/azure-vm-backup-restore.png)


Das Wiederherstellen eines virtuellen Computers oder aller Datenträger aus einer VM-Sicherung umfasst zwei Schritte:

* Auswählen eines Wiederherstellungspunkts für die Wiederherstellung
* Auswählen des Wiederherstellungstyps
    - Option 1: Erstellen eines neuen virtuellen Computers
    - Option 2: Wiederherstellen von Datenträgern

## <a name="select-a-restore-point-for-restore"></a>Auswählen eines Wiederherstellungspunkts für die Wiederherstellung
1. Melden Sie sich beim [Azure-Portal](http://portal.azure.com/) an.

2. Wählen Sie im Azure-Menü die Option **Alle Dienste** aus. Geben Sie in der Liste der Dienste **Recovery Services** ein, oder wechseln Sie zu **STORAGE**, wo die **Recovery Service-Tresore** aufgelistet sind, und wählen Sie einen aus.

    ![Recovery Services-Tresor](./media/backup-azure-arm-restore-vms/open-recovery-services-vault1.png)

3. Die Liste der Tresore im Abonnement wird angezeigt.

    ![Liste der Recovery Services-Tresore](./media/backup-azure-arm-restore-vms/list-of-rs-vaults1.png)

4. Wählen Sie in der Liste der Recovery Service-Tresore den Tresor aus, der dem virtuellen Computer zugeordnet ist, der wiederhergestellt werden soll. Wenn Sie den Tresor auswählen, wird das zugehörige Dashboard geöffnet.

    ![Ausgewählter Recovery Services-Tresor](./media/backup-azure-arm-restore-vms/select-vault-open-vault-blade1.png)

5. Klicken Sie im Tresordashboard auf der Kachel **Sicherungselemente** auf **Virtueller Azure-Computer**.

    ![Tresordashboard](./media/backup-azure-arm-restore-vms/vault-dashboard1.png)

6. Das Blatt **Sicherungselemente** mit der Liste der virtuellen Azure-Computer wird geöffnet.

    ![Liste der virtuellen Computer im Tresor](./media/backup-azure-arm-restore-vms/list-of-vms-in-vault1.png)

7. Wählen Sie in der Liste einen virtuellen Computer aus, um auf das Dashboard zu öffnen. Das VM-Dashboard wird geöffnet und zeigt den Überwachungsbereich an, der die **Wiederherstellungspunkte** enthält. Alle Vorgänge auf VM-Ebene wie **Jetzt sichern**, **Dateiwiederherstellung**, **Sicherung beenden** können von diesem Blatt aus durchgeführt werden.
Die Wiederherstellung kann auf vielfältige Weise von diesem Blatt aus durchgeführt werden. Beachten Sie, dass auf diesem Blatt nur die Wiederherstellungspunkte der letzten 30 Tage auflistet werden.

    - Klicken Sie mit der rechten Maustaste auf den virtuellen Computer und dann auf **Virtuellen Computer wiederherstellen**, um eine Wiederherstellung über einen Wiederherstellungspunkt der letzten 30 Tage durchzuführen.
    - Klicken Sie auf den Link unter **Wiederherstellungspunkte**, um eine Wiederherstellung über einen Wiederherstellungspunkt durchzuführen, der älter als 30 Tage ist.
    - Klicken Sie im Menü auf **Virtuellen Computer wiederherstellen**, um virtuelle Computer mit benutzerdefinierten Daten aufzulisten und zu filtern. Verwenden Sie den Filter, um den Zeitbereich für die angezeigten Wiederherstellungspunkte zu ändern. Sie können auch nach unterschiedlichen Datenkonsistenztypen filtern.
8. Überprüfen Sie die Einstellungen für den Wiederherstellungspunkt:
    - Die Datenkonsistenz zeigt den [Konsistenztyp](backup-azure-vms-introduction.md#consistency-types) für den Wiederherstellungspunkt an.
    - Der **Wiederherstellungstyp** zeigt an, wo der Punkt gespeichert wird (in einem Speicherkonto, im Tresor oder in beiden). Erfahren Sie mehr über [sofortige Wiederherstellungspunkte](https://azure.microsoft.com/blog/large-disk-support/).

  ![Wiederherstellungspunkte](./media/backup-azure-arm-restore-vms/vm-blade1.png)
9. Wählen Sie einen Wiederherstellungspunkt aus.

10. Klicken Sie auf **Konfiguration wiederherstellen**. Das Blatt **Wiederherstellungskonfiguration** wird geöffnet.

## <a name="choose-a-vm-restore-configuration"></a>Auswählen einer Konfiguration für die VM-Wiederherstellung
Wählen Sie nach der Auswahl des Wiederherstellungspunkts eine Konfiguration für die VM-Wiederherstellung aus. Zum Konfigurieren des wiederhergestellten virtuellen Computers können Sie das Azure-Portal oder PowerShell verwenden.

### <a name="restore-options"></a>Wiederherstellungsoptionen

**Option** | **Details**
--- | ---
**Neu erstellen > VM erstellen** | Entspricht der schnellen Erstellung eines virtuellen Computers. Ein einfacher virtueller Computer wird über einen Wiederherstellungspunkt betriebsbereit gemacht.<br/><br/> Die Einstellungen des virtuellen Computers können als Teil des Wiederherstellungsprozesses geändert werden.
**Neu erstellen > Restore disk (Datenträger wiederherstellen)** | Es wird ein virtueller Computer erstellt, den Sie als Teil des Wiederherstellungsprozesses anpassen können.<br/><br/> Mit dieser Option werden virtuelle Festplatten in das von Ihnen angegebene Speicherkonto kopiert.<br/><br/> Das Konto sollte sich am gleichen Speicherort wie der Tresor befinden.<br/><br/> Wenn Sie kein geeignetes Speicherkonto besitzen, müssen Sie eines erstellen.<br/><br/> Der Replikationstyp des Speicherkontos wird in Klammern angezeigt. Der zonenredundante Speicher (Zone Redundant Storage, ZRS) wird nicht unterstützt.<br/><br/> Über das Speicherkonto können Sie die wiederhergestellten Datenträger an einen vorhandenen virtuellen Computer anfügen. Alternativ können Sie einen neuen virtuellen Computer mithilfe von PowerShell aus den wiederhergestellten Datenträgern erstellen.
**Vorhandene ersetzen** | Mit dieser Option müssen Sie keinen virtuellen Computer erstellen.<br/><br/> Der aktuelle virtuelle Computer muss jedoch vorhanden sein. Wenn dieser gelöscht wurde, kann diese Option nicht verwendet werden.<br/><br/> Azure Backup erstellt eine Momentaufnahme des vorhandenen virtuellen Computers und speichert diese am angegebenen Stagingspeicherort. Die vorhandenen Datenträger, die mit dem virtuellen Computer verbunden sind, werden daraufhin durch den ausgewählten Wiederherstellungspunkt ersetzt. Die zuvor erstellte Momentaufnahme wird in den Tresor kopiert und gemäß Ihrer Aufbewahrungsrichtlinie als Wiederherstellungspunkt gespeichert.<br/><br/> Die Option „Vorhandene ersetzen“ wird für nicht verschlüsselte verwaltete VMs unterstützt. Für nicht verwaltete Datenträger, [generalisierte VMs](https://docs.microsoft.com/azure/virtual-machines/windows/capture-image-resource) oder für virtuelle Computer, die mit [benutzerdefinierten Images](https://azure.microsoft.com/resources/videos/create-a-custom-virtual-machine-image-in-azure-resource-manager-with-powershell/) erstellt wurden, wird sie nicht unterstützt.<br/><br/> Wenn der Wiederherstellungspunkt mehr oder weniger Datenträger als der aktuelle virtuelle Computer aufweist, dann übernimmt er die Anzahl des virtuellen Computers.

> [!NOTE]
> Wenn Sie einen virtuellen Computer mit erweiterten Netzwerkeinstellungen wiederherstellen möchten, stellen Sie diesen mit PowerShell wieder her, zum Beispiel wenn die Einstellungen von einem internen oder externen Lastenausgleich verwaltet werden oder über mehrere NICs oder mehrere IP-Adressen verfügen. [Weitere Informationen](#restore-vms-with-special-network-configurations)
> Wenn ein virtueller Windows-Computer [HUB-Lizenzierung](../virtual-machines/windows/hybrid-use-benefit-licensing.md) verwendet, verwenden Sie die Option **Neu erstellen > Restore disk (Datenträger wiederherstellen)** und anschließend PowerShell, oder stellen Sie die Vorlage wieder her, um den virtuellen Computer wiederherzustellen, wobei **Lizenztyp** auf **Windows_Server** festgelegt ist. Diese Einstellung kann auch nach dem Erstellen des virtuellen Computers angewendet werden.


Geben Sie die Einstellungen zur Wiederherstellung wie folgt an:

1. Wählen Sie unter **Wiederherstellen** einen [Wiederherstellungspunkt](#select-a-restore-point-for-restore) und dann **Wiederherstellungskonfiguration** aus.
2. Wählen Sie unter **Wiederherstellungskonfiguration** aus, wie Sie den virtuellen Computer entsprechend den Einstellungen wiederherstellen möchten, die in der Tabelle oben zusammengefasst sind.

    ![Assistent für die Wiederherstellungskonfiguration](./media/backup-azure-arm-restore-vms/restore-configuration-create-new1.png)


## <a name="create-new-create-a-vm"></a>Neu erstellen > Virtuellen Computer erstellen

1. Wählen Sie unter **Konfiguration wiederherstellen** > **Neu erstellen** > **Wiederherstellungstyp** die Option **Virtuellen Computer erstellen** aus.
2. Geben Sie unter **Name des virtuellen Computers** einen virtuellen Computer an, der nicht im Abonnement vorhanden ist.
3. Wählen Sie unter **Ressourcengruppe** eine vorhandene Ressourcengruppe für den neuen virtuellen Computer aus, oder erstellen Sie eine neue Ressourcengruppe mit einem global eindeutigen Namen. Wenn Sie einen Namen zuweisen, der bereits vorhanden ist, weist Azure der Gruppe denselben Namen wie der VM zu.
4. Wählen Sie unter **Virtuelles Netzwerk** das virtuelle Netzwerk aus, in das der virtuelle Computer platziert wird. Alle dem Abonnement zugeordneten VNETs werden angezeigt. Wählen Sie das Subnetz aus. Das erste Subnetz wird standardmäßig ausgewählt.
5. Geben Sie unter **Speicherort** den Speichertyp an, der für den virtuellen Computer verwendet wird.

    ![Assistent für die Wiederherstellungskonfiguration](./media/backup-azure-arm-restore-vms/recovery-configuration-wizard1.png)

6. Klicken Sie unter **Konfiguration wiederherstellen** auf **OK**. Klicken Sie unter **Wiederherstellen** auf **Wiederherstellen**, um den Wiederherstellungsvorgang auszulösen.



## <a name="create-new-restore-disks"></a>Neu erstellen > Restore disk (Datenträger wiederherstellen)

1. Wählen Sie unter **Konfiguration wiederherstellen** > **Neu erstellen** > **Wiederherstellungstyp** die Option **Datenträger wiederherstellen** aus.
2. Wählen Sie unter **Ressourcengruppe** eine vorhandene Ressourcengruppe für den wiederhergestellten Datenträger aus, oder erstellen Sie eine neue Ressourcengruppe mit einem global eindeutigen Namen.
3. Geben Sie für das **Speicherkonto** das Konto an, in das die virtuellen Festplatten kopiert werden sollen. Stellen Sie sicher, dass sich das Konto in derselben Region wie der Tresor befindet.

    ![Konfiguration der Wiederherstellung abgeschlossen](./media/backup-azure-arm-restore-vms/trigger-restore-operation1.png)

4. Klicken Sie unter **Konfiguration wiederherstellen** auf **OK**. Klicken Sie unter **Wiederherstellen** auf **Wiederherstellen**, um den Wiederherstellungsvorgang auszulösen.
5. Nachdem der Datenträger wiederhergestellt wurde, können Sie eine der folgenden Aktionen durchführen, um den Wiederherstellungsvorgang für den virtuellen Computer abzuschließen.

    - Verwenden Sie die Vorlage, die als Teil des Wiederherstellungsvorgangs generiert wurde, um Einstellungen anzupassen und die Bereitstellung des virtuellen Computers auszulösen. Bearbeiten Sie die Standardeinstellungen für die Vorlage, und übermitteln Sie die Vorlage für die Bereitstellung des virtuellen Computers.
    - Sie können den wiederhergestellten Datenträger an einen vorhandenen virtuellen Computer [anfügen](https://docs.microsoft.com/azure/virtual-machines/windows/attach-disk-ps).
    - Sie können [einen neuen virtuellen Computer](backup-azure-vms-automation.md#restore-an-azure-vm) aus den wiederhergestellten Datenträgern mithilfe von PowerShell erstellen.


## <a name="replace-existing-disks"></a>Ersetzen vorhandener Datenträger

Verwenden Sie diese Option, um vorhandene Datenträger im aktuellen virtuellen Computer durch den ausgewählten Wiederherstellungspunkt zu ersetzen.

1. Klicken Sie unter **Konfiguration wiederherstellen** auf **Vorhandene ersetzen**.
2. Wählen Sie für den **Wiederherstellungstyp** die Option **Replace disk/s** (Datenträger ersetzen) aus (diese ist der Wiederherstellungspunkt, der den bzw. die vorhandenen VM-Datenträger ersetzt).
3. Geben Sie für den **Stagingspeicherort** an, an dem Momentaufnahmen der aktuellen verwalteten Datenträger gespeichert werden sollen.

   ![Assistent für die Wiederherstellungskonfiguration – Vorhandene ersetzen](./media/backup-azure-arm-restore-vms/restore-configuration-replace-existing.png)


## <a name="track-the-restore-operation"></a>Nachverfolgen des Wiederherstellungsvorgangs
Nachdem Sie den Wiederherstellungsvorgang ausgelöst haben, erstellt der Sicherungsdienst einen Auftrag zum Nachverfolgen des Wiederherstellungsvorgangs. Der Sicherungsdienst erstellt außerdem die Benachrichtigung und zeigt sie vorübergehend im Bereich **Benachrichtigung** des Portals an. Wenn die Benachrichtigung nicht angezeigt wird, klicken Sie auf das Symbol **Benachrichtigungen**, um Ihre Benachrichtigungen anzuzeigen.

![Wiederherstellung ausgelöst](./media/backup-azure-arm-restore-vms/restore-notification1.png)

Klicken Sie auf den Link der Benachrichtigungen, um zur Liste **BackupJobs** zu wechseln. Alle Details zu den Vorgängen für einen bestimmten Auftrag finden Sie in den **BackupJobs**-Listen. Sie können vom Dashboard des Tresors aus zu **BackupJobs** wechseln, indem Sie auf die Kachel „Aufträge sichern“ klicken und **Azure Virtual Machine** auswählen, um die mit dem Tresor verbundenen Aufträge anzuzeigen.

Das Blatt **Sicherungsaufträge** wird geöffnet und zeigt die Liste der Aufträge an.

![Liste der virtuellen Computer in einem Tresor](./media/backup-azure-arm-restore-vms/restore-job-in-progress1.png)

Auf dem Blatt **Wiederherstellungsdetails** ist die **Statusanzeige** nun verfügbar. Das Blatt **Wiederherstellungsdetails** kann durch Klicken auf einen beliebigen Wiederherstellungsauftrag geöffnet werden, der den Status **In Bearbeitung** aufweist. Die **Statusanzeige** ist bei allen Arten von Wiederherstellungsvorgängen verfügbar, z. B. **Neu erstellen**, **Datenträger wiederherstellen** und **Vorhandene ersetzen**. Die Details, die in der Statusanzeige unter Restore (Wiederherstellen) enthalten sind, sind Informationen zur **geschätzten Dauer der Wiederherstellung**, zum **Fortschritt der Wiederherstellung** und **zur Anzahl der übertragenen Bytes**.

Details zur Statusanzeige für die Wiederherstellung sind nachfolgend angegeben:

- Die **geschätzte Dauer der Wiederherstellung** gibt zunächst an, wieviel Zeit benötigt wird, um den Wiederherstellungsvorgang abzuschließen. Während der Vorgang weiter ausgeführt wird, verringert sich die benötigte Zeit und erreicht schließlich 0, sobald der Wiederherstellungsvorgang abgeschlossen ist.
- Der **Fortschritt der Wiederherstellung** gibt an, zu wie viel Prozent der Wiederherstellungsvorgang bereits abgeschlossen ist.
- Die **Anzahl der übertragenen Bytes** ist in der untergeordneten Aufgabe verfügbar, wenn die Wiederherstellung über die Option zum Herstellen eines neuen virtuellen Computers durchgeführt wird. Sie erhalten Informationen darüber, wie viele Bytes im Vergleich zu der Gesamtzahl der insgesamt zu übertragenden Bytes bereits übertragen wurden.


## <a name="use-templates-to-customize-a-restored-vm"></a>Verwenden von Vorlagen zum Anpassen eines wiederhergestellten virtuellen Computers
Verwenden Sie [nach Abschluss der Datenträgerwiederherstellung](#Track-the-restore-operation) die Vorlage, die im Rahmen des Wiederherstellungsvorgangs generiert wurde, um einen neuen virtuellen Computer mit einer anderen Konfiguration als der Sicherungskonfiguration zu erstellen. Sie können mit der Vorlage auch Namen von Ressourcen anpassen, die beim Erstellen eines neuen virtuellen Computers über einen Wiederherstellungspunkt erstellt wurden.

![Drilldown des Wiederherstellungsauftrags](./media/backup-azure-arm-restore-vms/restore-job-drill-down1.png)

Gehen Sie wie folgt vor, um die im Rahmen der Datenträgerwiederherstellung generierte Vorlage zu erhalten:

1. Wechseln Sie zu den **Details des Wiederherstellungsauftrags** für den Auftrag.

2. Klicken Sie auf dem Bildschirm **Restore Job Details** (Details zum Wiederherstellungsauftrag) auf **Vorlage bereitstellen**, um die Vorlagenbereitstellung zu initiieren.

3. Verwenden Sie auf dem Blatt **Vorlage bereitstellen** für die benutzerdefinierte Bereitstellung die Vorlagenbereitstellung, um [die Vorlage zu bearbeiten und bereitzustellen](../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), oder fügen Sie durch [Erstellen einer Vorlage](../azure-resource-manager/resource-group-authoring-templates.md) vor der Bereitstellung weitere Anpassungen hinzu.

  ![Laden der Vorlagenbereitstellung](./media/backup-azure-arm-restore-vms/edit-template1.png)

4. Akzeptieren Sie nach der Eingabe der erforderlichen Werte die **Geschäftsbedingungen**, und klicken Sie auf **Kaufen**.

  ![Übermitteln der Vorlagenbereitstellung](./media/backup-azure-arm-restore-vms/submitting-template1.png)

## <a name="post-restore-steps"></a>Schritte nach der Wiederherstellung
* Bei Verwendung einer Cloud-Init-basierten Linux-Verteilung wie etwa Ubuntu wird das Kennwort aus Sicherheitsgründen nach der Wiederherstellung blockiert. Verwenden Sie zum [Zurücksetzen des Kennworts](../virtual-machines/linux/reset-password.md) die VMAccess-Erweiterung auf dem wiederhergestellten virtuellen Computer. Es wird empfohlen, SSH-Schlüssel für diese Verteilungen zu verwenden, um das Zurücksetzen des Kennworts nach der Wiederherstellung zu vermeiden.
* Erweiterungen, die während der Konfiguration der Sicherung vorhanden waren, werden zwar installiert, aber nicht aktiviert. Falls ein Problem auftritt, installieren Sie die Erweiterungen neu.
* Wenn der gesicherte virtuelle Computer nach der Wiederherstellung über eine statische IP-Adresse verfügt, erhält der wiederhergestellte virtuelle Computer eine dynamische IP-Adresse, um Konflikte beim Erstellen eines wiederhergestellten virtuellen Computers zu vermeiden. Erfahren Sie mehr über das [Hinzufügen einer statischen IP-Adresse zu einem wiederhergestellten virtuellen Computer](../virtual-network/virtual-networks-reserved-private-ip.md#how-to-add-a-static-internal-ip-to-an-existing-vm).
* Für wiederhergestellte virtuelle Computer ist kein Verfügbarkeitswert festgelegt. Es wird empfohlen, die Option zur Datenträgerwiederherstellung zu verwenden, um [eine Verfügbarkeitsgruppe hinzuzufügen](../virtual-machines/windows/tutorial-availability-sets.md), wenn Sie einen virtuellen Computer mithilfe von PowerShell oder Vorlagen und unter Verwendung von wiederhergestellten Datenträgern erstellen.


## <a name="backup-for-restored-vms"></a>Sicherung für wiederhergestellte virtuelle Computer
Wenn Sie einen virtuellen Computer in derselben Ressourcengruppe mit dem Namen des ursprünglich gesicherten virtuellen Computers wiederhergestellt haben, wird der virtuelle Computer nach der Wiederherstellung weiterhin gesichert. Wenn Sie den virtuellen Computer in einer anderen Ressourcengruppe wiederhergestellt oder einen anderen Namen für den wiederhergestellten virtuellen Computer angegeben haben, wird der virtuelle Computer wie ein neuer virtueller Computer behandelt. In diesem Fall müssen Sie die Sicherung für den wiederhergestellten virtuellen Computer festlegen.

## <a name="restore-a-vm-during-an-azure-datacenter-disaster"></a>Wiederherstellen eines virtuellen Computers während einer Azure-Datencenter-Notfallwiederherstellung
Mit Azure Backup können gesicherte virtuelle Computer im gekoppelten Datencenter wiederhergestellt werden, falls in dem primären Datencenter, in dem virtuelle Computer ausgeführt werden, ein Notfall eingetreten ist, und Sie den Sicherungstresor als georedundant konfiguriert haben. Wählen Sie bei diesem Szenario ein Speicherkonto, das in einem gekoppelten Datencenter vorhanden ist. Der Rest des Wiederherstellungsvorgangs bleibt gleich. Backup verwendet den Computedienst des gekoppelten georedundanten Datencenters, um den wiederhergestellten virtuellen Computer zu erstellen. Weitere Informationen finden Sie unter [Azure resiliency technical guidance: recovery from a region-wide service disruption](../resiliency/resiliency-technical-guidance-recovery-loss-azure-region.md) (Technischer Leitfaden zur Resilienz in Azure: Wiederherstellung nach einer regionsweiten Dienstunterbrechnung).

## <a name="restore-domain-controller-vms"></a>Wiederherstellen von virtuellen Computern mit Domänencontroller
Die Sicherung von virtuellen Computern mit Domänencontroller (DC) wird in Backup unterstützt. Beim Wiederherstellungsvorgang ist jedoch Vorsicht geboten. Der ordnungsgemäße Wiederherstellungsvorgang hängt von der Struktur der Domäne ab. Im einfachsten Fall verfügen Sie über einen einzelnen Domänencontroller in einer einzelnen Domäne. Doch in Produktionsumgebungen ist es üblicher, über eine einzelne Domäne mit mehreren Domänencontrollern zu verfügen, von denen einige lokal sein können. Möglich ist auch, dass Sie eine Gesamtstruktur mit mehreren Domänen besitzen.

Aus Active Directory-Sicht entspricht der virtuelle Azure-Computer beliebigen anderen virtuellen Computern auf einem modernen unterstützten Hypervisor. Der Hauptunterschied bei lokalen Hypervisoren ist, dass keine VM-Konsole in Azure verfügbar ist. Eine Konsole ist für bestimmte Szenarien erforderlich, z.B. zur Wiederherstellung mithilfe einer Sicherung vom Typ „Bare-Metal-Recovery“ (BMR). Die VM-Wiederherstellung aus dem Sicherungstresor ist jedoch ein vollständiger Ersatz für BMR. Der Verzeichnisdienste-Wiederherstellungsmodus (Directory Services Restore Mode, DSRM) ist ebenfalls verfügbar, sodass alle Active Directory-Wiederherstellungsszenarien realisierbar sind. Weitere Informationen finden Sie unter [Backup and restore considerations for virtualized domain controllers](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(v=ws.10).aspx#backup_and_restore_considerations_for_virtualized_domain_controllers) (Sicherungs- und Wiederherstellungsaspekte für virtualisierte Domänencontroller) und [Planning for Active Directory forest recovery](https://technet.microsoft.com/library/planning-active-directory-forest-recovery(v=ws.10).aspx) (Planen der Wiederherstellung der Active Directory-Gesamtstruktur).

### <a name="single-dc-in-a-single-domain"></a>Einzelner Domänencontroller in einer einzelnen Domäne
Der virtuelle Computer kann (wie alle anderen virtuellen Computer) über das Azure-Portal oder mithilfe von PowerShell wiederhergestellt werden.

### <a name="multiple-dcs-in-a-single-domain"></a>Mehrere Domänencontroller in einer einzelnen Domäne
Wenn andere Domänencontroller in der gleichen Domäne über das Netzwerk erreicht werden können, kann der Domänencontroller wie ein beliebiger virtueller Computer wiederhergestellt werden. Wenn es sich um den letzten verbleibenden Domänencontroller in der Domäne handelt oder eine Wiederherstellung in einem isolierten Netzwerk erfolgt, muss ein Vorgang zur Wiederherstellung der Gesamtstruktur ausgeführt werden.

### <a name="multiple-domains-in-one-forest"></a>Mehrere Domänen in einer Gesamtstruktur
Wenn andere Domänencontroller in der gleichen Domäne über das Netzwerk erreicht werden können, kann der Domänencontroller wie ein beliebiger virtueller Computer wiederhergestellt werden. In allen anderen Fällen wird eine Wiederherstellung der Gesamtstruktur empfohlen.

## <a name="restore-vms-with-special-network-configurations"></a>Wiederherstellen von virtuellen Computern mit speziellen Netzwerkkonfigurationen
Das Sichern und Wiederherstellen virtueller Computer ist mit den folgenden speziellen Netzwerkkonfigurationen möglich. Für diese Konfigurationen sind während des Wiederherstellungsvorgangs jedoch einige besondere Überlegungen anzustellen:

* Virtuelle Computer unter Lastenausgleich (intern und extern)
* VMs mit mehreren reservierten IP-Adressen
* VMs mit mehreren Netzwerkkarten (NICs)

> [!IMPORTANT]
> Beim Erstellen der speziellen Netzwerkkonfiguration für virtuelle Computer müssen Sie PowerShell verwenden, um virtuelle Computer aus den wiederhergestellten Datenträgern zu erstellen.
>
>

Zum vollständigen Neuerstellen der virtuellen Computer nach der Wiederherstellung auf dem Datenträger führen Sie die folgenden Schritte aus:

1. Stellen Sie die Datenträger mit [PowerShell](backup-azure-vms-automation.md#restore-an-azure-vm) aus einem Recovery Services-Tresor wieder her.

1. Erstellen Sie die für den Lastenausgleich/mehrere NICs/mehrere reservierte IP-Adressen erforderliche VM-Konfiguration mithilfe der PowerShell-Cmdlets. Verwenden Sie sie zum Erstellen des virtuellen Computers mit der gewünschten Konfiguration:

   a. Erstellen Sie den virtuellen Computer im Clouddienst mit einem [internem Lastenausgleich](https://azure.microsoft.com/documentation/articles/load-balancer-internal-getstarted/).

   b. Erstellen Sie einen virtuellen Computer zum Herstellen einer Verbindung mit einem [Load Balancer mit Internetzugriff](https://azure.microsoft.com/documentation/articles/load-balancer-internet-getstarted/).

   c. Erstellen Sie einen virtuellen Computer mit [mehreren Netzwerkkarten](../virtual-machines/windows/multiple-nics.md).

   d. Erstellen Sie einen virtuellen Computer mit [mehreren reservierten IP-Adressen](../virtual-network/virtual-network-multiple-ip-addresses-powershell.md).

## <a name="next-steps"></a>Nächste Schritte
Jetzt können Sie Ihre virtuellen Computer wiederherstellen. Informationen zu häufig bei VMs auftretenden Fehlern finden Sie im Artikel zur Problembehandlung. Lesen Sie auch den Artikel zur Verwaltung von Aufgaben mit Ihren virtuellen Computern.

* [Problembehandlung](backup-azure-vms-troubleshoot.md#restore)
* [Verwalten virtueller Computer](backup-azure-manage-vms.md)
