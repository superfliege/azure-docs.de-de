---
title: Failover und Failback von Hyper-V-VMs, die mit Site Recovery in Azure repliziert werden | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie ein Failover für Hyper-V-VMs zu Azure und ein Failback zum lokalen Standort mit Azure Site Recovery durchführen.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 03/8/2018
ms.author: raynew
ms.openlocfilehash: 7863feb29fbb04f643aa3b7e1984209f44cdbe9a
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
ms.locfileid: "29852896"
---
# <a name="fail-over-and-fail-back-hyper-v-vms-replicated-to-azure"></a>Failover und Failback von Hyper-V-VMs, die in Azure repliziert werden

In diesem Tutorial wird das Ausführen eines Failovers einer Hyper-V-VM zu Azure beschrieben. Nach dem Failover erfolgt ein Failback zum lokalen Standort, wenn er verfügbar ist. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Überprüfen der Hyper-V-VM-Eigenschaften zum Feststellen der Konformität mit Azure-Anforderungen
> * Ausführen eines Failovers auf Azure
> * Erneutes Schützen von virtuellen Azure-Computern zum lokalen Standort
> * Failback von Azure zum lokalen Standort
> * Erneutes Schützen von lokalen-VMs, um das erneute Replizieren nach Azure zu starten

Dies ist das fünfte Tutorial in einer Reihe. In diesem Tutorial wird davon ausgegangen, dass Sie bereits die Aufgaben in den vorherigen Tutorials durchgearbeitet haben.

1. [Vorbereiten von Azure](tutorial-prepare-azure.md)
2. [Prepare on-premises VMware](tutorial-prepare-on-premises-hyper-v.md) (Lokales Vorbereiten von VMware)
3. Einrichten der Notfallwiederherstellung für [Hyper-V-VMs](tutorial-hyper-v-to-azure.md) oder [in System Center VMM-Clouds verwaltete Hyper-V-VMs](tutorial-hyper-v-vmm-to-azure.md)
4. [Durchführen eines Notfallwiederherstellungsverfahrens](tutorial-dr-drill-azure.md)

## <a name="prepare-for-failover-and-failback"></a>Vorbereiten für Failover und Failback

Stellen Sie sicher, dass auf dem virtuellen Computer keine Momentaufnahmen vorhanden sind und dass die lokalen virtuellen Computer während des erneuten Schützens deaktiviert sind. Dies hilft, die Konsistenz der Daten während der Replikation sicherzustellen. Schalten Sie den virtuellen Computer nach Abschluss des erneuten Schützens nicht ein. 

Failover und Failback weisen vier Phasen auf:

1. **Failover auf Azure**: Failover von Computern vom lokalen Standort auf Azure.
2. **Erneutes Schützen von virtuellen Azure-Computern:** erneutes Schützen der Azure-VMs, sodass sie wieder zurück auf die lokalen Hyper-V-VMs repliziert werden.
3. **Lokales Failover:** Ausführen eines Failovers von Azure an Ihren lokalen Standort, wenn dieser verfügbar ist.
4. **Erneutes Schützen von lokalen VMs:** Nachdem die Daten per Failback zurückgeführt wurden, werden die lokalen virtuellen Computer erneut geschützt, damit sie mit der Replikation nach Azure beginnen.

## <a name="verify-vm-properties"></a>Überprüfen von VM-Eigenschaften

Überprüfen Sie die VM-Eigenschaften, und stellen Sie sicher, dass der virtuelle Computer die [Azure-Anforderungen](hyper-v-azure-support-matrix.md#replicated-vms) erfüllt.

1. Klicken Sie unter **Geschützte Elemente** auf **Replizierte Elemente** > <VM-Name>.

2. Überprüfen Sie im Bereich **Repliziertes Element** die VM-Informationen, den Integritätsstatus sowie die letzten verfügbaren Wiederherstellungspunkte. Klicken Sie auf **Eigenschaften**, um weitere Details anzuzeigen.
     - Unter **Compute und Netzwerk** können Sie die VM-Einstellungen und Netzwerkeinstellungen ändern, darunter das Netzwerk/Subnetz, in dem sich die Azure-VM nach dem Failover befindet, und die IP-Adresse, die ihr zugewiesen werden soll. Verwaltete Datenträger werden für das Failback von Azure nach Hyper-V nicht unterstützt.
   nach dem Failover befindet, und die IP-Adresse, die ihr zugewiesen werden soll.
    - Unter **Datenträger** finden Sie Informationen über das Betriebssystem und die Datenträger auf dem virtuellen Computer.

## <a name="fail-over-to-azure"></a>Failover in Azure

1. Klicken Sie unter **Einstellungen** > **Replizierte Elemente** auf VM > **Failover**.
2. Wählen Sie unter **Failover** den **letzten** Wiederherstellungspunkt aus. 
3. Wählen Sie **Computer vor Beginn des Failovers herunterfahren** aus. Site Recovery versucht, die Quell-VMs herunterzufahren, bevor das Failover ausgelöst wird. Das Failover wird auch dann fortgesetzt, wenn das Herunterfahren nicht erfolgreich ist. Der Fortschritt des Failovers wird auf der Seite **Aufträge** angezeigt.
4. Nachdem Sie das Failover überprüft haben, klicken Sie auf **Commit ausführen**. Dadurch werden alle verfügbaren Wiederherstellungspunkte gelöscht.

> [!WARNING]
> **Brechen Sie ein Failover in Bearbeitung nicht ab:** Vor dem Starten des Failovers wird die VM-Replikation beendet. Wenn Sie den Fortschritt abbrechen, wird das Failover beendet, die Replikation der VM wird jedoch nicht erneut durchgeführt.

## <a name="reprotect-azure-vms"></a>Erneutes Schützen der virtuellen Azure-Computer

1. Klicken Sie unter **AzureVMVault** > **Replizierte Elemente** mit der rechten Maustaste auf den virtuellen Computer, für den ein Failover ausgeführt wurde, und wählen Sie **Erneut schützen** aus.
2. Vergewissern Sie sich, dass Schutzrichtung auf **Azure auf lokal** festgelegt ist.
3. Der lokale virtuelle Computer wird während des erneuten Schützens deaktiviert, um Datenkonsistenz sicherzustellen. Aktivieren Sie ihn nach Abschluss des erneuten Schützens nicht.
4. Nach dem erneuten Schützen beginnt der virtuelle Computer mit der Replikation von Azure auf den lokalen Standort.



## <a name="fail-over-from-azure-and-reprotect-the-on-premises-vm"></a>Ausführen eines Failovers von Azure und erneutes Schützen des lokalen virtuellen Computers

Führen Sie ein Failover von Azure zum lokalen Standort durch, und starten Sie die Replikation von virtuellen Computern vom lokalen Standort in Azure.

1. Klicken Sie unter **Einstellungen** > **Replizierte Elemente** auf VM > **Geplantes Failover**.
2. Überprüfen Sie in **Geplantes Failover bestätigen** die Failoverrichtung (von Azure), und wählen Sie Quell- und Zielspeicherort aus.
3. Wählen Sie **Daten vor Failover synchronisieren (nur Deltaänderungen synchronisieren)** aus. Diese Option minimiert VM-Ausfallzeiten, da hierbei eine Synchronisierung durchgeführt wird, ohne den virtuellen Computer herunterzufahren.
4. Initiieren Sie das Failover. Der Fortschritt des Failovers wird auf der Registerkarte **Aufträge** angezeigt.
5. Wenn die erste Datensynchronisierung abgeschlossen ist und Sie zum Herunterfahren der Azure-VMs bereit sind, klicken Sie auf **Aufträge** > Auftragsname des geplanten Failovers > **Failover abschließen**. Dadurch wird die Azure-VM heruntergefahren, die neuesten Änderungen werden an den lokalen Standort übertragen, und der lokale virtuelle Computer wird gestartet.
6. Melden Sie sich auf dem lokalen virtuellen Computer an, um zu überprüfen, ob er wie erwartet verfügbar ist.
7. Der lokale virtuelle Computer befindet sich nun im Status **Commit ausstehend**. Klicken Sie auf **Commit ausführen**. Dies löscht die Azure-VMs und die zugehörigen Datenträger und bereitet den lokalen virtuellen Computer auf die umgekehrte Replikation vor.
Aktivieren Sie zum Starten der Replikation des lokalen virtuellen Computers in Azure **Umgekehrt replizieren**. Dadurch wird die Replikation von Deltaänderungen seit dem Ausschalten des virtuellen Azure-Computers ausgelöst.  
