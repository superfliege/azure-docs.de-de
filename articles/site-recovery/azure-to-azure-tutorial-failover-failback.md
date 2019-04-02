---
title: Ausführen eines Failovers und erneutes Schützen von in einer sekundären Azure-Region replizierten virtuellen Azure-Computern zur Notfallwiederherstellung mit dem Azure Site Recovery-Dienst
description: Hier erfahren Sie, wie Sie mit dem Azure Site Recovery-Dienst zur Notfallwiederherstellung ein Failover für in einer sekundären Azure-Region replizierte virtuelle Azure-Computer ausführen und diese Computer erneut schützen
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 03/18/2018
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: c19e554571927efa907350df1f1fbbd8ff491c42
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/21/2019
ms.locfileid: "58314755"
---
# <a name="fail-over-and-reprotect-azure-vms-between-regions"></a>Ausführen eines Failovers und erneutes Schützen von Azure-VMs zwischen Regionen

Der Dienst [Azure Site Recovery](site-recovery-overview.md) unterstützt Ihre Strategie zur Notfallwiederherstellung, indem Replikation, Failover und Failback von lokalen Computern und virtuellen Azure-Computern (Virtual Machines, VMs) verwaltet und orchestriert werden.

In diesem Tutorial wird beschrieben, wie Sie für einen einzelnen virtuellen Azure-Computer ein Failover zu einer sekundären Azure-Region ausführen. Nach dem Failover schützen Sie den virtuellen Computer erneut. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Ausführen eines Failovers der Azure-VM
> * Erneutes Schützen der sekundären Azure-VM, damit sie in der primären Region repliziert wird

> [!NOTE]
> In diesem Tutorial wird die einfachste Vorgehensweise mit Standardeinstellungen und minimaler Anpassung erläutert. Lesen Sie für komplexere Szenarien die Artikel unter „Anleitungen“ für virtuelle Azure-Computer.

## <a name="prerequisites"></a>Voraussetzungen

- Führen Sie unbedingt ein [Notfallwiederherstellungsverfahren](azure-to-azure-tutorial-dr-drill.md) durch, um zu überprüfen, ob alles wie erwartet funktioniert.
- Überprüfen Sie die VM-Eigenschaften, bevor Sie das Testfailover ausführen. Die VM muss den [Azure-Anforderungen](azure-to-azure-support-matrix.md#replicated-machine-operating-systems) entsprechen.

## <a name="run-a-failover-to-the-secondary-region"></a>Ausführen eines Failovers zur sekundären Region

1. Wählen Sie unter **Replizierten Elemente** die VM, für die ein Failover ausgeführt werden soll, > **Failover** aus.

   ![Failover](./media/azure-to-azure-tutorial-failover-failback/failover.png)

2. Wählen Sie unter **Failover** einen **Wiederherstellungspunkt** für das Failover aus. Sie können eine der folgenden Optionen auswählen:

   * **Neuester** (Standardeinstellung): Mit dieser Option werden alle Daten in Site Recovery verarbeitet, und sie verfügt über den niedrigsten RPO-Wert (Recovery Point Objective).
   * **Letzte Verarbeitung**: Mit dieser Option wird für den virtuellen Computer der Zustand des letzten Wiederherstellungspunkts wiederhergestellt, der von Site Recovery verarbeitet wurde.
   * **Benutzerdefiniert**: Verwenden Sie diese Option für ein Failover auf einen bestimmten Wiederherstellungspunkt. Diese Option eignet sich für ein Testfailover.

3. Klicken Sie auf **Der Computer wird vor Beginn des Failovers heruntergefahren**, wenn Site Recovery versuchen soll, Quell-VMs herunterzufahren, bevor das Failover ausgelöst wird. Das Failover wird auch dann fortgesetzt, wenn das Herunterfahren nicht erfolgreich ist. Site Recovery bereinigt die Quelle nach dem Failover nicht.

4. Der Fortschritt des Failovers wird auf der Seite **Aufträge** angezeigt.

5. Überprüfen Sie den virtuellen Computer nach Abschluss des Failovers, indem Sie sich am Computer anmelden. Falls Sie einen anderen Wiederherstellungspunkt für den virtuellen Computer verwenden möchten, können Sie die Option **Wiederherstellungspunkt ändern** wählen.

6. Nachdem Sie mit dem virtuellen Computer, für den das Failover durchgeführt wurde, zufrieden sind, können Sie für das Failover die Option **Commit** wählen.
   Dadurch werden alle mit dem Dienst verfügbaren Wiederherstellungspunkte gelöscht. Sie können den Wiederherstellungspunkt nun nicht mehr ändern.

## <a name="reprotect-the-secondary-vm"></a>Erneutes Schützen der sekundären VM

Nach dem Failover der VM müssen Sie sie erneut schützen, damit sie wieder zurück zur primären Region repliziert wird.

1. Stellen Sie sicher, dass sich die VM im Status **Commit für Failover ausgeführt** befindet, dass die primäre Region verfügbar ist und dass Sie neue Ressourcen darin erstellen und darauf zugreifen können.
2. Klicken Sie unter **Tresor** > **Replizierte Elemente** mit der rechten Maustaste auf die VM, für die ein Failover durchgeführt wurde, und dann auf **Erneut schützen**.

   ![Rechtsklick zum erneuten Schützen](./media/azure-to-azure-tutorial-failover-failback/reprotect.png)

2. Beachten Sie, dass die Wirkrichtung des Schutzes – von der sekundären zur primären Region – bereits ausgewählt ist.
3. Lesen Sie die Informationen zu **Ressourcengruppe, Netzwerk, Speicher und Verfügbarkeitsgruppen**, und klicken Sie auf „OK“. Alle als neu markierten Ressourcen werden während des Vorgangs zum erneuten Schützen erstellt.
4. Klicken Sie auf **OK**, um einen Auftrag zum erneuten Schützen auszulösen. Dieser Auftrag liefert die neuesten Daten an den Zielstandort. Anschließend werden die Deltas zur primären Region repliziert. Die VM befindet sich jetzt in einem geschützten Zustand.

## <a name="next-steps"></a>Nächste Schritte
- Lesen Sie nach dem erneuten Schützen [hier](azure-to-azure-tutorial-failback.md) nach, wie Sie ein Failback zur primären Region ausführen, wenn diese verfügbar ist.
- Erfahren Sie mehr über den [erneuten Schutz](azure-to-azure-how-to-reprotect.md#what-happens-during-reprotection).
