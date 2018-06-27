---
title: Failover und Failback von Hyper-V-VMs, die mit Site Recovery in Azure repliziert werden | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie ein Failover für Hyper-V-VMs zu Azure und ein Failback zum lokalen Standort mit Azure Site Recovery durchführen.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 06/20/2018
ms.author: raynew
ms.openlocfilehash: 6a34187a87c6ecda461357a1c2fc8747ddf4b056
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36294291"
---
# <a name="failover-and-failback-hyper-v-vms-replicated-to-azure"></a>Failover und Failback von Hyper-V-VMs, die in Azure repliziert werden

In diesem Tutorial wird das Ausführen eines Failovers einer Hyper-V-VM zu Azure beschrieben. Nach dem Failover erfolgt ein Failback zum lokalen Standort, wenn er verfügbar ist. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Überprüfen der Hyper-V-VM-Eigenschaften zum Feststellen der Konformität mit Azure-Anforderungen
> * Ausführen eines Failovers auf Azure
> * Failback von Azure zu lokal
> * Umgekehrtes Replizieren von lokalen VMs, um das erneute Replizieren nach Azure zu starten

Dies ist das fünfte Tutorial in einer Reihe. Es wird davon ausgegangen, dass Sie bereits die Aufgaben in den vorherigen Tutorials durchgearbeitet haben.    

1. [Prepare Azure](tutorial-prepare-azure.md) (Vorbereiten von Azure)
2. [Vorbereiten lokaler Hyper-V-Instanzen](tutorial-prepare-on-premises-hyper-v.md)
3. Einrichten der Notfallwiederherstellung für [Hyper-V-VMs](tutorial-hyper-v-to-azure.md) oder [in System Center VMM-Clouds verwaltete Hyper-V-VMs](tutorial-hyper-v-vmm-to-azure.md)
4. [Durchführen eines Notfallwiederherstellungsverfahrens](tutorial-dr-drill-azure.md)

## <a name="prepare-for-failover-and-failback"></a>Vorbereiten für Failover und Failback

Stellen Sie sicher, dass auf dem virtuellen Computer keine Momentaufnahmen vorhanden sind und dass die lokalen virtuellen Computer während des Failbacks deaktiviert sind. Dies hilft, die Konsistenz der Daten während der Replikation sicherzustellen. Aktivieren Sie während des Failbacks keine lokalen virtuellen Computer. 

Failover und Failback weisen drei Phasen auf:

1. **Failover zu Azure**: Failover von Hyper-V-VMs vom lokalen Standort zu Azure.
2. **Lokales Failover**: Failover von Azure-VMs auf Ihren lokalen Standort, wenn dieser verfügbar ist. Dabei werden die VMs zunächst erneut zu den lokalen Hyper-V-VMs repliziert. Nach der anfänglichen Datensynchronisierung wird das Failover der Azure-VMs auf den lokalen Standort ausgeführt.  
3. **Umgekehrtes Replizieren von lokalen VMs**: Nachdem die Daten per Failback zurückgeführt wurden, werden die lokalen virtuellen Computer umgekehrt repliziert, damit sie mit der Replikation nach Azure beginnen.

## <a name="verify-vm-properties"></a>Überprüfen von VM-Eigenschaften

Überprüfen Sie vor dem Ausführen des Failovers die VM-Eigenschaften, und stellen Sie sicher, dass der virtuelle Computer die [Azure-Anforderungen](hyper-v-azure-support-matrix.md#replicated-vms) erfüllt.

1. Klicken Sie unter **Geschützte Elemente** auf **Replizierte Elemente** > <VM-Name>.

2. Überprüfen Sie im Bereich **Repliziertes Element** die VM-Informationen, den Integritätsstatus sowie die letzten verfügbaren Wiederherstellungspunkte. Klicken Sie auf **Eigenschaften**, um weitere Details anzuzeigen.
     - Unter **Compute und Netzwerk** können Sie VM- und Netzwerkeinstellungen einschließlich des Netzwerks/Subnetzes, in dem der virtuelle Azure-Computer nach dem Failover platziert wird, sowie der IP-Adresse, die ihm zugewiesen wird, ändern. Verwaltete Datenträger werden für das Failback von Azure nach Hyper-V nicht unterstützt.
      - Unter **Datenträger** finden Sie Informationen über das Betriebssystem und die Datenträger auf dem virtuellen Computer.

## <a name="failover-to-azure"></a>Failover zu Azure

1. Klicken Sie unter **Einstellungen** > **Replizierte Elemente** auf VM > **Failover**.
2. Wählen Sie unter **Failover** den **letzten** Wiederherstellungspunkt aus. 
3. Wählen Sie **Computer vor Beginn des Failovers herunterfahren** aus. Site Recovery versucht, die Quell-VMs herunterzufahren, bevor das Failover ausgelöst wird. Das Failover wird auch dann fortgesetzt, wenn das Herunterfahren nicht erfolgreich ist. Der Fortschritt des Failovers wird auf der Seite **Aufträge** angezeigt.
4. Nachdem Sie das Failover überprüft haben, klicken Sie auf **Commit ausführen**. Daraufhin werden alle verfügbaren Wiederherstellungspunkte gelöscht.

> [!WARNING]
> **Brechen Sie ein Failover in Bearbeitung nicht ab**: Wenn Sie ein Failover in Bearbeitung abbrechen, wird das Failover beendet, die Replikation der VM wird jedoch nicht erneut durchgeführt.

## <a name="failback-azure-vm-to-on-premises-and-reverse-replicate-the-on-premises-vm"></a>Failback von Azure-VMs auf lokale Standorte und umgekehrtes Replizieren von lokalen VMs

Der Failbackvorgang ist im Grunde genommen ein Failover von Azure auf den lokalen Standort und impliziert ein umgekehrtes Replizieren der VMs vom lokalen Standort nach Azure.

1. Klicken Sie unter **Einstellungen** > **Replizierte Elemente** auf VM > **Geplantes Failover**.
2. Überprüfen Sie in **Geplantes Failover bestätigen** die Failoverrichtung (von Azure), und wählen Sie Quell- und Zielspeicherort aus.
3. Wählen Sie **Daten vor Failover synchronisieren (nur Deltaänderungen synchronisieren)** aus. Diese Option minimiert VM-Ausfallzeiten, da hierbei eine Synchronisierung durchgeführt wird, ohne den virtuellen Computer herunterzufahren.
4. Initiieren Sie das Failover. Der Fortschritt des Failovers wird auf der Registerkarte **Aufträge** angezeigt.
5. Wenn die erste Datensynchronisierung abgeschlossen ist und Sie zum Herunterfahren der Azure-VMs bereit sind, klicken Sie auf **Aufträge** > Auftragsname des geplanten Failovers > **Failover abschließen**. Daraufhin wird die Azure-VM heruntergefahren, die neuesten Änderungen werden an den lokalen Standort übertragen, und der lokale virtuelle Computer wird gestartet.
6. Melden Sie sich auf dem lokalen virtuellen Computer an, um zu überprüfen, ob er wie erwartet verfügbar ist.
7. Der lokale virtuelle Computer befindet sich nun im Status **Commit ausstehend**. Klicken Sie auf **Commit ausführen**. Daraufhin werden die Azure-VMs und die zugehörigen Datenträger gelöscht, und der lokale virtuelle Computer wird auf die umgekehrte Replikation vorbereitet.
Aktivieren Sie zum Starten der Replikation des lokalen virtuellen Computers in Azure **Umgekehrt replizieren**. Daraufhin wird die Replikation von Deltaänderungen seit dem Ausschalten des virtuellen Azure-Computers ausgelöst.  
