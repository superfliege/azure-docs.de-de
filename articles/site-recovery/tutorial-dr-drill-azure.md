---
title: Durchführen einer Notfallwiederherstellungsübung für lokale Computer in Azure mit Azure Site Recovery | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mit Azure Site Recovery eine Notfallwiederherstellungsübung von lokalen Computern nach Azure durchführen.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 05/02/2018
ms.author: raynew
ms.openlocfilehash: 98612730405e63c251bbba2daf770b5394db1514
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2018
---
# <a name="run-a-disaster-recovery-drill-to-azure"></a>Durchführen eines Notfallwiederherstellungsverfahrens in Azure

Dieses Tutorial veranschaulicht das Durchführen einer Notfallwiederherstellungsübung für lokale Computer nach Azure mit einem Testfailover. Eine Übung dient der Überprüfung Ihrer Replikationsstrategie ohne Datenverlust. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Einrichten eines isolierten Netzwerks für das Testfailover
> * Vorbereiten der Verbindungsherstellung mit Azure-VMs nach dem Failover
> * Ausführen eines Testfailovers für einen einzelnen Computer

Dies ist das vierte Tutorial in einer Reihe. In diesem Tutorial wird davon ausgegangen, dass Sie bereits die Aufgaben in den vorherigen Tutorials durchgearbeitet haben.

1. [Vorbereiten von Azure](tutorial-prepare-azure.md)
2. [Lokales Vorbereiten von VMware](tutorial-prepare-on-premises-vmware.md)
3. [Einrichten der Notfallwiederherstellung](tutorial-vmware-to-azure.md)

## <a name="verify-vm-properties"></a>Überprüfen von VM-Eigenschaften

Überprüfen Sie vor dem Ausführen eines Testfailovers die VM-Eigenschaften, und stellen Sie sicher, dass die Hyper-V-VM [hyper-v-azure-support-matrix.md#replicated-vms], die [VMware-VM oder der physische Server](vmware-physical-azure-support-matrix.md#replicated-machines) die Azure-Anforderungen erfüllt.

1. Klicken Sie unter **Geschützte Elemente** auf **Replizierte Elemente** > VM.
2. Im Bereich **Repliziertes Element** finden Sie eine Zusammenfassung der Informationen zu virtuellen Computern, den Integritätsstatus sowie die neuesten verfügbaren Wiederherstellungspunkte. Klicken Sie auf **Eigenschaften**, um weitere Details anzuzeigen.
3. In **Compute und Netzwerk** können Sie den Azure-Namen, die Ressourcengruppe, die Zielgröße, die [Verfügbarkeitsgruppe](../virtual-machines/windows/tutorial-availability-sets.md) und die Einstellungen verwalteter Datenträger ändern.
   
      >[!NOTE]
      Ein Failback von Azure-VMs mit verwalteten Datenträgern auf lokale Hyper-V-Computer wird derzeit nicht unterstützt. Sie dürfen die Option für verwaltete Datenträger nur dann für ein Failover verwenden, wenn Sie eine Migration lokaler VMs zu Azure ohne ein Failback planen.
   
4. Sie können Netzwerkeinstellungen einschließlich des Netzwerks/Subnetzes, in dem der virtuelle Azure-Computer nach dem Failover platziert wird, sowie der IP-Adresse, die ihm zugewiesen wird, anzeigen und ändern.
5. Unter **Datenträger** finden Sie Informationen über das Betriebssystem und die Datenträger auf dem virtuellen Computer.

## <a name="run-a-test-failover-for-a-single-vm"></a>Ausführen eines Testfailovers für eine einzelne VM

Beim Durchführen eines Testfailovers geschieht Folgendes:

1. Eine Überprüfung der erforderlichen Komponenten wird ausgeführt, um sicherzustellen, dass alle Bedingungen für ein Failover erfüllt sind.
2. Durch das Failover werden die Daten verarbeitet, sodass eine Azure-VM erstellt werden kann. Wenn Sie den letzten Wiederherstellungspunkt auswählen, wird ein Wiederherstellungspunkt auf der Grundlage der Daten erstellt.
3. Eine Azure-VM wird anhand der im vorherigen Schritt verarbeiteten Daten erstellt.

Führen Sie das Testfailover wie folgt aus:

1. Klicken Sie unter **Einstellungen** > **Replizierte Elemente** auf die VM > **+Testfailover**.
2. Wählen Sie für dieses Tutorial **Letzte Verarbeitung** für den Wiederherstellungspunkt aus. Bei Auswahl dieser Option wird für die VM ein Failover auf den letzten verfügbaren Zeitpunkt durchgeführt. Der Zeitstempel wird angezeigt. Mit dieser Option wird keine Zeit für die Verarbeitung von Daten aufgewendet, und der RTO-Wert (Recovery Time Objective) wird niedrig gehalten.
3. Wählen Sie unter **Testfailover** das Azure-Zielnetzwerk aus, mit dem Azure-VMs nach dem Failover verbunden werden.
4. Klicken Sie auf **OK**, um den Failovervorgang zu starten. Sie können den Fortschritt des Vorgangs verfolgen, indem Sie auf die VM klicken, um ihre Eigenschaften zu öffnen. Alternativ können Sie auf den Auftrag **Testfailover** unter dem Tresornamen und dann auf **Einstellungen** > **Aufträge** >
   **Site Recovery-Aufträge** klicken.
5. Nach Abschluss des Failovers wird der virtuelle Azure-Replikatcomputer im Azure-Portal unter **Virtuelle Computer** angezeigt. Vergewissern Sie sich, dass die VM die richtige Größe hat, mit dem richtigen Netzwerk verbunden ist und ausgeführt wird.
6. Sie sollten nun eine Verbindung mit der replizierten VM in Azure herstellen können.
7. Klicken Sie zum Löschen der während des Testfailovers erstellten Azure-VMs auf **Testfailover bereinigen** für die VM. Erfassen und speichern Sie unter **Notizen** alle Beobachtungen im Zusammenhang mit dem Testfailover.

In einigen Szenarien erfordert ein Failover zusätzliche Verarbeitungsschritte, die etwa 8 bis 10 Minuten dauern können. Bei VMware-Linux-Computern, VMware-VMs ohne aktivierten DHCP-Dienst und VMware-VMs ohne die Starttreiber storvsc, vmbus, storflt, intelide und atapi kann das Testfailover länger dauern.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Durchführen von Failover und Failback für lokale VMware-VMs](vmware-azure-tutorial-failover-failback.md)
