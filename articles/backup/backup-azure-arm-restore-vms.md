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
ms.openlocfilehash: 7de9d1674860a6369c1dc09462a06def672fbdf2
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50420522"
---
# <a name="use-the-azure-portal-to-restore-virtual-machines"></a>Wiederherstellen virtueller Computer über das Azure-Portal
Schützen Sie Ihre Daten, indem Sie in festgelegten Abständen Momentaufnahmen Ihrer Daten erstellen. Diese Momentaufnahmen werden als Wiederherstellungspunkte bezeichnet. Sie werden in Recovery Services-Tresoren gespeichert. Wenn es erforderlich ist, einen virtuellen Computer zu reparieren oder wiederherzustellen, können Sie den virtuellen Computer auf der Grundlage eines gespeicherten Wiederherstellungspunkts wiederherstellen. Beim Herstellen eines Wiederherstellungspunkts haben Sie folgende Möglichkeiten:

* Sie können einen neuen virtuellen Computer erstellen, der den gesicherten virtuellen Computer zu einem bestimmten Zeitpunkt darstellt.
* Sie können Datenträger wiederherstellen und die zugehörige Vorlage des Prozesses verwenden, um den wiederhergestellten virtuellen Computer anzupassen oder einzelne Dateien wiederherzustellen.

In diesem Artikel wird erläutert, wie ein virtueller Computer als neuer virtueller Computer wiederhergestellt wird oder alle gesicherten Datenträger wiederhergestellt werden. Informationen zur Wiederherstellung einzelner Dateien finden Sie unter [Wiederherstellen von Dateien aus einer Sicherung von virtuellen Azure-Computern](backup-azure-restore-files-from-vm.md).

![Drei Möglichkeiten zum Wiederherstellen auf der Grundlage einer VM-Sicherung](./media/backup-azure-arm-restore-vms/azure-vm-backup-restore.png)

> [!NOTE]
> Azure verfügt über zwei Bereitstellungsmodelle zum Erstellen und Verwenden von Ressourcen: [Azure Resource Manager-Modell und klassisches Modell](../azure-resource-manager/resource-manager-deployment-model.md). Dieser Artikel enthält die Informationen und Verfahren, die für die Wiederherstellung bereitgestellter virtueller Computer mithilfe des Resource Manager-Modells verwendet werden.
>
>

Das Wiederherstellen eines virtuellen Computers oder aller Datenträger aus einer VM-Sicherung umfasst zwei Schritte:

* Auswählen eines Wiederherstellungspunkts für die Wiederherstellung
* Auswählen des Wiederherstellungstyps, Erstellen eines neuen virtuellen Computers oder Wiederherstellen eines Datenträgers und Angeben der erforderlichen Parameter

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
Die Wiederherstellung kann auf vielfältige Weise von diesem Blatt aus durchgeführt werden. Beachten Sie, dass dieses Blatt nur die Wiederherstellungspunkte der letzten 30 Tage auflistet.

    a) Klicken Sie mit der rechten Maustaste auf den Wiederherstellungspunkt auf diesem Blatt (weniger als 30 Tage) und starten Sie **Virtuellen Computer wiederherstellen**.

    b) Um Wiederherstellungspunkte für mehr als 30 Tage wiederherzustellen, können Sie das auf dem Blatt bereitgestellte „Hier klicken“ verwenden.

    c) **Virtuellen Computer wiederherstellen** im Menükopf bietet eine Option, um die virtuellen Computer für benutzerdefinierte Datumsbereiche wie gewünscht aufzulisten und zu filtern.

    Verwenden Sie die Option „Filter“, um den Zeitbereich der angezeigten Wiederherstellungspunkte zu ändern. Standardmäßig werden Wiederherstellungspunkte jeglicher Konsistenz angezeigt. Ändern Sie den Filter „Alle Wiederherstellungspunkte“, um eine bestimmte Konsistenz der Wiederherstellungspunkte auszuwählen. Weitere Informationen zu den einzelnen Typen von Wiederherstellungspunkten finden Sie unter [Datenkonsistenz](backup-azure-vms-introduction.md#data-consistency).

    Optionen für die Konsistenz des Wiederherstellungspunkts:
    - Absturzkonsistente Wiederherstellungspunkte
    - Anwendungskonsistente Wiederherstellungspunkte
    - Dateisystemkonsistente Wiederherstellungspunkte
    - Alle Wiederherstellungspunkte

  ![Wiederherstellungspunkte](./media/backup-azure-arm-restore-vms/vm-blade1.png)

    >  [!NOTE]
    > Der Wiederherstellungstyp gibt an, ob sie sich im Kundenspeicherkonto, im Tresor oder in beiden befindet. Erfahren Sie mehr über [Sofortiger Wiederherstellungspunkt](https://azure.microsoft.com/blog/large-disk-support/).

8. Klicken Sie auf dem Blatt **Wiederherstellen** auf **Wiederherstellungspunkt**.

    ![Auswählen eines Wiederherstellungspunkts](./media/backup-azure-arm-restore-vms/select-recovery-point1.png)

    Auf dem Blatt **Wiederherstellen** wird angezeigt, dass der Wiederherstellungspunkt festgelegt wird, wenn Sie auf **OK** klicken.
9. Wechseln Sie zum Blatt **Wiederherstellen**, sofern Sie sich nicht bereits dort befinden. Stellen Sie sicher, dass [ein Wiederherstellungspunkt ausgewählt ist](#select-a-restore-point-for-restore), und klicken Sie auf **Wiederherstellungskonfiguration**. Das Blatt **Wiederherstellungskonfiguration** wird geöffnet.

## <a name="choose-a-vm-restore-configuration"></a>Auswählen einer Konfiguration für die VM-Wiederherstellung
Wählen Sie nach der Auswahl des Wiederherstellungspunkts eine Konfiguration für die VM-Wiederherstellung aus. Zum Konfigurieren des wiederhergestellten virtuellen Computers können Sie das Azure-Portal oder PowerShell verwenden.

1. Wechseln Sie zum Blatt **Wiederherstellen**, sofern Sie sich nicht bereits dort befinden. Stellen Sie sicher, dass [ein Wiederherstellungspunkt ausgewählt ist](#select-a-restore-point-for-restore), und klicken Sie auf **Wiederherstellungskonfiguration**. Das Blatt **Wiederherstellungskonfiguration** wird geöffnet.
2. Dieses Blatt verfügt derzeit über zwei Optionen: **Neu erstellen** (Standardeinstellung) und **Vorhandene ersetzen**. Letztere Option ist eine direkte Wiederherstellung, um die Datenträger zu ersetzen, die nur die vorhandenen Konfigurationen und Erweiterungen beibehalten.

> [!NOTE]
> Wir arbeiten in den nächsten Monaten daran, den gesamten virtuellen Computer durch die Datenträger, Netzwerkeinstellungen, Konfigurationen und Erweiterungen zu ersetzen.
>
>

 Bei der Option **Neu erstellen**, die die Daten auf einem neuen virtuellen Computer oder einem neuen Datenträger wiederherstellt, haben Sie zwei Möglichkeiten:

 ![Assistent für die Wiederherstellungskonfiguration](./media/backup-azure-arm-restore-vms/restore-configuration-create-new.png)

 - **Virtuellen Computer erstellen**
 - **Datenträger wiederherstellen**

![Assistent für die Wiederherstellungskonfiguration](./media/backup-azure-arm-restore-vms/restore-configuration-create-new1.png)

Das Portal bietet für einen wiederhergestellten virtuellen Computer die Option **Schnellerfassung**. Wenn Sie die Konfiguration des virtuellen Computers oder die Namen der Ressourcen anpassen möchten, die beim Erstellen einer neuen VM-Auswahl erstellt wurden, verwenden Sie PowerShell oder das Portal zum Wiederherstellen gesicherter Datenträger. Fügen Sie sie mithilfe von PowerShell-Befehlen an eine Auswahl von VM-Konfigurationen an. Sie können auch die mit wiederhergestellten Datenträgern bereitgestellte Vorlage verwenden, um den wiederhergestellten virtuellen Computer anzupassen. Informationen zum Wiederherstellen von virtuellen Computern mit mehreren Netzwerkkarten oder einem Lastenausgleichsmodul finden Sie unter [Wiederherstellen virtueller Computer über das Azure-Portal](#restore-vms-with-special-network-configurations). Wenn der virtuelle Windows-Computer [HUB-Lizenzierung](../virtual-machines/windows/hybrid-use-benefit-licensing.md) verwendet, müssen Sie zum Erstellen des virtuellen Computers Datenträger wiederherstellen und PowerShell bzw. eine Vorlage verwenden, wie in diesem Artikel beschrieben. Stellen Sie sicher, dass Sie beim Erstellen des virtuellen Computers für **Lizenztyp** den Wert „Windows_Server“ angeben, um auf dem wiederhergestellten virtuellen Computer von den HUB-Vorteilen zu profitieren. Beachten Sie, dass dies auch später nach der Erstellung des virtuellen Computers erfolgen kann.

## <a name="create-a-new-vm-from-a-restore-point"></a>Erstellen eines neuen virtuellen Computer über einen Wiederherstellungspunkt
1. Geben Sie auf dem im vorherigen Abschnitt erwähnten Blatt **Konfiguration wiederherstellen** Werte für jedes der folgenden Felder ein oder wählen Sie sie aus:

    a. **Wiederherstellungstyp**. Erstellen Sie eine VM.

    b. **Name des virtuellen Computers**. Geben Sie den Namen des virtuellen Computers ein, der im Abonnement nicht vorhanden ist.

    c. **Ressourcengruppe**. Verwenden Sie eine vorhandene Ressourcengruppe, oder erstellen Sie eine neue Ressourcengruppe. Wenn Sie einen klassischen virtuellen Computer wiederherstellen möchten, verwenden Sie dieses Feld, um den Namen eines neuen Clouddiensts anzugeben. Wenn Sie eine neue Ressourcengruppe/einen neuen Clouddienst erstellen, muss der Name global eindeutig sein. In der Regel wird der Name des Clouddiensts einer öffentlich zugänglichen URL zugeordnet, z.B. [cloudservice].cloudapp.net. Wenn Sie versuchen, einen bereits verwendeten Namen für die Cloudressourcengruppe/den Clouddienst zu verwenden, weist Azure der Ressourcengruppe/dem Clouddienst den Namen des virtuellen Computers zu. Azure zeigt Ressourcengruppen/Clouddienste und virtuelle Computer an, die keiner Affinitätsgruppe zugeordnet sind. Weitere Informationen finden Sie unter [Migrieren eines (klassischen) virtuellen Netzwerks aus einer Affinitätsgruppe in eine Region](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).

    d. **Virtuelles Netzwerk**. Wählen Sie das virtuelle Netzwerk aus, wenn Sie einen virtuellen Computer erstellen. Das Feld enthält alle dem Abonnement zugeordneten virtuellen Netzwerke. Die Ressourcengruppe des virtuellen Computers wird in Klammern angezeigt.

    e. **Subnetz**. Wenn das virtuelle Netzwerk über Subnetze verfügt, ist das erste Subnetz standardmäßig ausgewählt. Wenn zusätzliche Subnetze vorhanden sind, wählen Sie das gewünschte Subnetz aus.

    f. **Speicherort**. Speicherkonten sind der Stagingort. In diesem Menü werden die Speicherkonten aufgelistet, die sich am gleichen Standort wie der Recovery Services-Tresor befinden. Zonenredundante Speicherkonten werden nicht unterstützt. Wenn keine Speicherkonten mit dem gleichen Standort wie der Recovery Services-Tresor vorhanden sind, müssen Sie vor dem Starten des Wiederherstellungsvorgangs eins erstellen. Der Replikationstyp des Speicherkontos wird in Klammern angezeigt.

    ![Assistent für die Wiederherstellungskonfiguration](./media/backup-azure-arm-restore-vms/recovery-configuration-wizard1.png)

    > [!NOTE]
    > * Ein virtuelles Netzwerk ist optional für einen klassische virtuellen Computer und obligatorisch für einem vom Resource Manager bereitgestellten virtuellen Computer.

    > * Der im Speicherkonto (Premium oder Standard) am Stagingort angegebene Speichertyp entscheidet über den Speichertyp des Wiederherstellungsdatenträgers. Derzeit wird der gemischte Modus beim Wiederherstellen von Datenträgern nicht unterstützt.
    >
    >

2. Klicken Sie auf dem Blatt **Wiederherstellungskonfiguration** auf **OK**, um die Wiederherstellungskonfiguration abzuschließen. Klicken Sie auf dem Blatt **Wiederherstellen** auf **Wiederherstellen**, um den Wiederherstellungsvorgang auszulösen.

## <a name="restore-backed-up-disks"></a>Wiederherstellen von gesicherten Datenträgern
Der Wiederherstellungstypwert **Datenträger wiederherstellen** auf dem Blatt **Konfiguration wiederherstellen** ermöglicht es, einen virtuellen Computer mit angepassten Konfigurationen zu erstellen. Beim Wiederherstellen von Datenträgern sollte sich das zu wählende Speicherkonto am selben Ort befinden wie der Recovery Services-Tresor. Wenn keine Speicherkonten mit dem gleichen Standort wie der Recovery Services-Tresor vorhanden sind, müssen Sie ein Speicherkonto erstellen. ZRS-Speicherkonten werden nicht unterstützt. Der Replikationstyp des Speicherkonto wird in Klammern angezeigt.

Vorgang nach der Wiederherstellung, siehe unten:
* [Verwenden der Vorlage, um den wiederhergestellten virtuellen Computer anzupassen](#use-templates-to-customize-restore-vm)
* [Verwenden der wiederhergestellten Datenträger, um sie einem vorhandenen virtuellen Computer anzufügen](../virtual-machines/windows/attach-managed-disk-portal.md)
* [Erstellen eines neuen virtuellen Computers aus wiederhergestellten Datenträgern mithilfe von PowerShell](./backup-azure-vms-automation.md#restore-an-azure-vm)

Klicken Sie auf dem Blatt **Wiederherstellungskonfiguration** auf **OK**, um die Wiederherstellungskonfiguration abzuschließen. Klicken Sie auf dem Blatt **Wiederherstellen** auf **Wiederherstellen**, um den Wiederherstellungsvorgang auszulösen.

![Konfiguration der Wiederherstellung abgeschlossen](./media/backup-azure-arm-restore-vms/trigger-restore-operation1.png)

Die **direkte Wiederherstellung** erfolgt über die Registerkarte **Vorhandene ersetzen**.

## <a name="replace-existing-disks-from-a-restore-point"></a>Ersetzen vorhandener Datenträger über einen Wiederherstellungspunkt
Die Option **Vorhandene ersetzen** hilft dabei, vorhandene Datenträger im aktuellen virtuellen Computer durch den ausgewählten Wiederherstellungspunkt zu ersetzen. Dieser Vorgang kann nur ausgeführt werden, wenn der aktuelle virtuelle Computer vorhanden ist. Wenn er aus irgendeinem Grund gelöscht wurde, kann dieser Vorgang nicht durchgeführt werden. Alternativ wird empfohlen, **Neu erstellen** für den virtuellen Computer oder die Datenträger durchzuführen, um mit den Wiederherstellungsvorgängen fortzufahren. Bei den Vorgängen zum Ersetzen vorhandener Datenträger werden als Vorsichtsmaßnahme die Daten vor dem Initiieren der Ersetzungsvorgänge gesichert. Wenn der Wiederherstellungspunkt mehr/weniger Datenträger als der aktuelle virtuelle Computer aufweist, dann wird die Anzahl der Datenträger im Wiederherstellungspunkt nur auf dem virtuellen Computer angezeigt. Die Option **Vorhandene ersetzen** wird derzeit nur für verwaltete Datenträger und unverschlüsselte virtuelle Computer unterstützt. Für [generalisierte VMs](https://docs.microsoft.com/azure/virtual-machines/windows/capture-image-resource) und für virtuelle Computer, die mit [benutzerdefinierte Images](https://azure.microsoft.com/resources/videos/create-a-custom-virtual-machine-image-in-azure-resource-manager-with-powershell/) erstellt wurden, wird sie nicht unterstützt.  

 Auf dem Blatt **Konfiguration wiederherstellen** muss als einzige Eingabe **Stagingort** ausgewählt werden.

   ![Assistent für die Wiederherstellungskonfiguration – Vorhandene ersetzen](./media/backup-azure-arm-restore-vms/restore-configuration-replace-existing.png)

 a. **Wiederherstellungstyp**. Ersetzen Sie die Datenträger, die angeben, dass der ausgewählte Wiederherstellungspunkt die Datenträger im vorhandenen virtuellen Computer ersetzt.

 b. **Stagingort**. Speicherkonten sind der Stagingort für verwaltete Datenträger. In diesem Menü werden die Speicherkonten aufgelistet, die sich am gleichen Standort wie der Recovery Services-Tresor befinden. Zonenredundante Speicherkonten werden nicht unterstützt. Wenn keine Speicherkonten mit dem gleichen Standort wie der Recovery Services-Tresor vorhanden sind, müssen Sie vor dem Starten des Wiederherstellungsvorgangs eins erstellen. Der Replikationstyp des Speicherkontos wird in Klammern angezeigt.

## <a name="track-the-restore-operation"></a>Nachverfolgen des Wiederherstellungsvorgangs
Nachdem Sie den Wiederherstellungsvorgang ausgelöst haben, erstellt der Sicherungsdienst einen Auftrag zum Nachverfolgen des Wiederherstellungsvorgangs. Der Sicherungsdienst erstellt außerdem die Benachrichtigung und zeigt sie vorübergehend im Bereich **Benachrichtigung** des Portals an. Wenn die Benachrichtigung nicht angezeigt wird, klicken Sie auf das Symbol **Benachrichtigungen**, um Ihre Benachrichtigungen anzuzeigen.

![Wiederherstellung ausgelöst](./media/backup-azure-arm-restore-vms/restore-notification1.png)

Klicken Sie auf den Link der Benachrichtigungen, um zur Liste **BackupJobs** zu wechseln. Alle Details zu den Vorgängen für einen bestimmten Auftrag finden Sie in den **BackupJobs**-Listen. Sie können vom Dashboard des Tresors aus zu **BackupJobs** wechseln, indem Sie auf die Kachel „Aufträge sichern“ klicken und **Azure Virtual Machine** auswählen, um die mit dem Tresor verbundenen Aufträge anzuzeigen.

Das Blatt **Sicherungsaufträge** wird geöffnet und zeigt die Liste der Aufträge an.

![Liste der virtuellen Computer in einem Tresor](./media/backup-azure-arm-restore-vms/restore-job-in-progress1.png)

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
