---
title: Erneutes Schützen von virtuellen Azure-Computern, für die mithilfe von Azure Site Recovery ein Failover zurück zur primären Azure-Region durchgeführt wurde | Microsoft-Dokumentation
description: Beschreibt das erneute Schützen von virtuellen Azure-Computern in einer sekundären Region nach einem Failover aus einer primären Region mithilfe von Azure Site Recovery.
services: site-recovery
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: rajanaki
ms.openlocfilehash: 4171a904626d3b624b39b8a3a261df0d342012df
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="reprotect-failed-over-azure-vms-to-the-primary-region"></a>Erneutes Schützen von virtuellen Azure-Computern, für die ein Failover zur primären Region durchgeführt wurde


>[!NOTE]
>
> Die Site Recovery-Replikation für Azure-VMs ist derzeit in der Vorschauphase.



Wenn Sie mithilfe von [Azure Site Recovery](site-recovery-overview.md) ein [Failover](site-recovery-failover.md) für virtuelle Azure-Computer von einer Region zu einer anderen durchführen, werden die virtuellen Computer in der sekundären Region in einem ungeschützten Zustand gestartet. Wenn Sie für die virtuellen Computer ein Failback zur primären Region durchführen, müssen Sie folgende Schritte ausführen:

- Schützen Sie die virtuellen Computer in der sekundären Region erneut, damit ihre Replizierung in der primären Region starten kann. 
- Nachdem das erneute Schützen abgeschlossen ist und die virtuellen Computer replizieren, können Sie ein Failover für sie von der sekundären zur primären Region durchführen.

> [!WARNING]
> Wenn Sie Computer von der sekundären zur primären Region [migriert](migrate-overview.md#what-do-we-mean-by-migration), den virtuellen Computer in eine andere Ressourcengruppe verschoben oder den virtuellen Azure-Computer gelöscht haben, können Sie den virtuellen Computer weder erneut schützen noch ein Failback dafür ausführen.


## <a name="prerequisites"></a>Voraussetzungen
1. Das Failover des virtuellen Computers von der primären zur sekundären Region muss committet werden.
2. Der primäre Zielstandort muss verfügbar sein, und Sie müssen auf Ressourcen in dieser Region zugreifen bzw. sie erstellen können.

## <a name="reprotect-a-vm"></a>Erneutes Schützen eines virtuellen Computers

1. Klicken Sie unter **Tresor** > **Replizierte Elemente** mit der rechten Maustaste auf den virtuellen Computer, für den ein Failover ausgeführt wurde, und wählen Sie **Erneut schützen** aus. Die Richtung des erneuten Schützens sollte als von sekundär zu primär angezeigt werden. 

  ![Erneutes Schützen](./media/site-recovery-how-to-reprotect-azure-to-azure/reprotect.png)

2. Überprüfen Sie Ressourcengruppe, Netzwerk, Speicher und Verfügbarkeitsgruppen. Klicken Sie dann auf **OK**. Wenn als neu markierte Ressourcen vorhanden sind, werden sie im Rahmen des Vorgangs zum erneuten Schützen erstellt.
3. Diese Auftrag zum erneuten Schützen liefert die neuesten Daten an den Zielstandort. Nachdem dieser Vorgang abgeschlossen ist, erfolgt die Deltareplikation. Anschließend können Sie ein Failback zum primären Standort durchführen. Mit der Option zum Anpassen können Sie das Speicherkonto oder Netzwerk auswählen, das Sie während des Vorgangs zum erneuten Schützen verwenden möchten.

  ![Option zum Anpassen](./media/site-recovery-how-to-reprotect-azure-to-azure/customize.png)

### <a name="customize-reprotect-settings"></a>Anpassen der Einstellungen des erneuten Schützens

Sie können die folgenden Eigenschaften des virtuellen Zielcomputers beim erneuten Schützen anpassen.

![Anpassen](./media/site-recovery-how-to-reprotect-azure-to-azure/customizeblade.png)

|Eigenschaft |Notizen  |
|---------|---------|
|Zielressourcengruppe     | Ändern Sie die Zielressourcengruppe, in welcher der virtuelle Computer erstellt wird. Der virtuelle Zielcomputer wird im Rahmen des Vorgangs zum erneuten Schützen gelöscht. Sie können eine neue Ressourcengruppe auswählen, unter welcher der virtuelle Computer nach dem Failover erstellt wird.        |
|Virtuelles Zielnetzwerk     | Das Zielnetzwerk kann während des Austrags zum erneuten Schützen nicht geändert werden. Wiederholen Sie die Netzwerkzuordnung, um das Netzwerk zu ändern.         |
|Zielspeicher (sekundäre VM verwendet keine verwalteten Datenträger)     | Sie können das Speicherkonto ändern, das der virtuelle Computer nach einem Failover verwendet.         |
|Verwaltete Replikatdatenträger (sekundäre VM verwendet verwaltete Datenträger)    | Site Recovery erstellt verwaltete Replikatdatenträger in der primären Region, um die verwalteten Datenträger der sekundären VM zu spiegeln.         | 
|Zwischenspeicher     | Sie können ein Zwischenspeicherkonto angeben, das während der Replikation verwendet wird. Standardmäßig wird ein neues Zwischenspeicherkonto erstellt, wenn keines vorhanden ist.         |
|Verfügbarkeitsgruppe     |Wenn der virtuelle Computer in der sekundären Region Teil einer Verfügbarkeitsgruppe ist, können Sie eine Verfügbarkeitsgruppe für den virtuellen Zielcomputer in der primären Region auswählen. Standardmäßig versucht Site Recovery die vorhandene Verfügbarkeitsgruppe in der primären Region zu finden und verwenden. Während der Anpassung können Sie eine neue Verfügbarkeitsgruppe angeben.         |


### <a name="what-happens-during-reprotection"></a>Was geschieht beim erneuten Schützen?

Standardmäßig geschieht Folgendes:

1. In der primären Region wird ein neues Speicherkonto erstellt.
2. Wenn das Zielspeicherkonto (das ursprüngliche Speicherkonto in der primären Region) nicht vorhanden ist, wird ein neues erstellt. Der zugewiesene Name des Speicherkontos ist der vom sekundären virtuellen Computer verwendete Name des Speicherkontos mit dem Suffix „asr“.
3. Wenn Ihr virtueller Computer verwaltete Datenträger verwendet, werden verwaltete Replikatdatenträger in der primären Region zum Speichern der Daten erstellt, die von den Datenträgern der sekundären VM repliziert werden. 
4. Wenn die Zielverfügbarkeitsgruppe nicht vorhanden ist, wird im Rahmen des Auftrags zum erneuten Schützen s bei Bedarf eine neue erstellt. Wenn Sie die Einstellungen für das erneute Schützen angepasst haben, wird die ausgewählte Gruppe verwendet.

Wenn Sie einen Auftrag zum erneuten Schützen auslösen und der virtuelle Zielcomputer vorhanden ist, geschieht Folgendes:

1. Die erforderlichen Komponenten werden als Teil des Vorgangs zum erneuten Schützen erstellt. Wenn sie bereits vorhanden sind, werden sie wiederverwendet.
2. Falls der virtuelle Computer auf der Zielseite ausgeführt wird, wird er ausgeschaltet.
3. Der Datenträger des virtuellen Computers auf der Zielseite wird von Site Recovery als Seedingblob in einen Container kopiert.
4. Der virtuelle Computer auf der Zielseite wird dann gelöscht.
5. Das Seedingblob wird vom virtuellen Computer auf der aktuellen Quellseite (sekundär) für die Replikation verwendet. Dadurch wird sichergestellt, dass nur Deltas repliziert werden.
6. Wichtige Unterschiede zwischen dem Quelldatenträger und dem Seedingblob werden synchronisiert. Dieser Vorgang kann einige Zeit in Anspruch nehmen.
7. Nachdem der Auftrag zum erneuten Schützen abgeschlossen wurde, beginnt die Deltareplikation, mit der gemäß der Replikationsrichtlinie ein Wiederherstellungspunkt erstellt wird.
8. Nach erfolgreichem Abschluss des Auftrags zum erneuten Schützen wechselt der virtuelle Computer einen geschützten Zustand.

## <a name="next-steps"></a>Nächste Schritte

Nachdem der virtuelle Computer geschützt ist, können Sie ein Failover initiieren. Beim Failover wird der virtuelle Computer in der sekundären Region heruntergefahren, und nach einer kurzen Ausfallzeit in der primären Region erstellt und gestartet. Es wird empfohlen, einen dafür geeigneten Zeitpunkt zu wählen und ein Testfailover ausführen, aber ein vollständiges Failover zum primären Standort initiieren. [Weitere Informationen](site-recovery-failover.md) zum Failover.

