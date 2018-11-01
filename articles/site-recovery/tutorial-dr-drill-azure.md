---
title: Durchführen einer Notfallwiederherstellungsübung für lokale Computer in Azure mit Azure Site Recovery | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mit Azure Site Recovery eine Notfallwiederherstellungsübung von lokalen Computern nach Azure durchführen.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 10/29/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: b0344095cd7c9aedd360d44f2649f27dfd78cd30
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2018
ms.locfileid: "50214408"
---
# <a name="run-a-disaster-recovery-drill-to-azure"></a>Durchführen eines Notfallwiederherstellungsverfahrens in Azure

Dieser Artikel veranschaulicht das Durchführen einer Notfallwiederherstellungsübung für lokale Computer in Azure mit einem Testfailover. Eine Übung dient der Überprüfung Ihrer Replikationsstrategie ohne Datenverlust.

Dies ist das vierte Tutorial in einer Reihe, welche die Einrichtung der Notfallwiederherstellung in Azure für lokale virtuelle VMware-Computer oder virtuelle Hyper-V-Computer veranschaulicht.

In diesem Tutorial wird davon ausgegangen, dass Sie die ersten drei Tutorials abgeschlossen haben:
    - Im [ersten Tutorial](tutorial-prepare-azure.md) haben wir die erforderlichen Azure-Komponenten für die VMware-Notfallwiederherstellung eingerichtet.
    - Im [zweiten Tutorial](vmware-azure-tutorial-prepare-on-premises.md) haben wir lokale Komponenten für die Notfallwiederherstellung vorbereitet und die Anforderungen überprüft.
    - Im [dritten Tutorial](vmware-azure-tutorial.md) haben wir die Replikation für den lokalen virtuellen VMware-Computer eingerichtet und aktiviert.
    - Tutorials dienen zur Veranschaulichung des **einfachsten Bereitstellungspfads für ein Szenario**. Sie verwenden nach Möglichkeit Standardoptionen und zeigen nicht alle möglichen Einstellungen und Pfade. Ausführlichere Informationen zu den Schritten des Testfailovers finden Sie in [dieser Anleitung](site-recovery-test-failover-to-azure.md).

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Einrichten eines isolierten Netzwerks für das Testfailover
> * Vorbereiten der Verbindungsherstellung mit Azure-VMs nach dem Failover
> * Ausführen eines Testfailovers für einen einzelnen Computer



## <a name="verify-vm-properties"></a>Überprüfen von VM-Eigenschaften

Bevor Sie ein Testfailover ausführen, überprüfen Sie die VM-Eigenschaften, und stellen Sie sicher, dass der virtuelle [Hyper-V](hyper-v-azure-support-matrix.md#replicated-vms)- oder [VMware](vmware-physical-azure-support-matrix.md#replicated-machines)-Computer die Azure-Anforderungen erfüllt.

1. Klicken Sie unter **Geschützte Elemente** auf **Replizierte Elemente** und dann auf den virtuellen Computer.
2. Im Bereich **Repliziertes Element** finden Sie eine Zusammenfassung der Informationen zu virtuellen Computern, den Integritätsstatus sowie die neuesten verfügbaren Wiederherstellungspunkte. Klicken Sie auf **Eigenschaften**, um weitere Details anzuzeigen.
3. In **Compute und Netzwerk** können Sie den Azure-Namen, die Ressourcengruppe, die Zielgröße, die Verfügbarkeitsgruppe und die Einstellungen verwalteter Datenträger ändern.
4. Sie können Netzwerkeinstellungen einschließlich des Netzwerks/Subnetzes, in dem der virtuelle Azure-Computer nach dem Failover platziert wird, sowie der IP-Adresse, die ihm zugewiesen wird, anzeigen und ändern.
5. Unter **Datenträger** finden Sie Informationen über das Betriebssystem und die Datenträger auf dem virtuellen Computer.

## <a name="create-a-network-for-test-failover"></a>Erstellen eines Netzwerks für Testfailover

Für Testfailover sollten Sie ein Netzwerk wählen, das von dem in den Einstellungen **Compute und Netzwerk** für jede VM angegebenen Produktionswiederherstellungsstandort-Netzwerk isoliert ist. Standardmäßig ist ein virtuelles Azure-Netzwerk beim Erstellen von anderen Netzwerken isoliert. Das Testnetzwerk sollte Ihr Produktionsnetzwerk imitieren:

- Das Testnetzwerk muss dieselbe Anzahl von Subnetzen wie das Produktionsnetzwerk besitzen. Subnetze müssen die gleichen Namen aufweisen.
- Das Testnetzwerk muss den gleichen IP-Adressbereich verwenden.
- Aktualisieren Sie das DNS des Testnetzwerks mit der IP-Adresse, die in den Einstellungen **Compute und Netzwerk** für die DNS-VM angegeben ist. Unter [Überlegungen zum Test-Failover](site-recovery-active-directory.md#test-failover-considerations) finden Sie weitere Details.

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

## <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Vorbereiten der Verbindungsherstellung mit Azure-VMs nach dem Failover

Wenn Sie nach dem Failover per RDP/SSH eine Verbindung mit Azure-VMs herstellen möchten, müssen Sie die in dieser [Tabelle](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover) zusammengefassten Anforderungen erfüllen.

Führen Sie die [hier](site-recovery-failover-to-azure-troubleshoot.md) beschriebenen Schritte aus, um nach dem Failover ggf. Konnektivitätsprobleme zu beheben.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Durchführen von Failover und Failback für lokale VMware-VMs](vmware-azure-tutorial-failover-failback.md)
> [Durchführen von Failover und Failback für lokale Hyper-V-VMs](hyper-v-azure-failover-failback-tutorial.md)
