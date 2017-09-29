---
title: "Durchführen einer Notfallwiederherstellungsübung an Ihrem sekundären lokalen Standort mit Azure Site Recovery | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie eine Notfallwiederherstellungsübung an Ihrem sekundären lokalen Standort mit Azure Site Recovery durchführen."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 445878e2-6682-49ba-914d-4c6824ab08a6
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 09/18/2017
ms.author: raynew
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 8737c2a22bd729cfc15d5448e1ec0becef643fd5
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---
# <a name="run-a-disaster-recovery-drill-for-hyper-v-vms-to-your-secondary-on-premises-site"></a>Durchführen einer Notfallwiederherstellungsübung für Hyper-V-VMs an Ihrem sekundären lokalen Standort

Der Dienst [Azure Site Recovery](site-recovery-overview.md) unterstützt Ihre Strategie zur Notfallwiederherstellung, indem Replikation, Failover und Failback von lokalen Computern und virtuellen Azure-Computern (Virtual Machines, VMs) verwaltet und orchestriert werden.

Dieses Tutorial veranschaulicht das Durchführen einer Notfallwiederherstellungsübung für Hyper-V-VMs an Ihrem sekundären lokalen Standort. Hyper-V-VMs müssen in einer privaten Cloud von System Center Virtual Machine Manager (VMM) verwaltet werden. Bei einer Übung wird Ihre Replikationsstrategie ohne Datenverlust oder Ausfallzeiten überprüft. Dies hat keinen Einfluss auf Ihre Produktionsumgebung. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Vorbereiten der Voraussetzungen für Testfailover
> * Ausführen eines Testfailovers für einen einzelnen Computer


## <a name="prerequisites"></a>Voraussetzungen

- Sie können ein Testfailover mit verschiedenen Optionen für Netzwerke am sekundären Standort ausführen. Sie können das Failover ohne Netzwerk oder mit einem vorhandenen Netzwerk ausführen oder Site Recovery automatisch ein Testnetzwerk erstellen lassen. 
**Wenn Sie ein vorhandenes Produktionsnetzwerk für das Testfailover verwenden möchten:**
    - Der primäre virtuelle Computer sollte beim Ausführen des Testfailovers heruntergefahren werden. Andernfalls werden zwei virtuelle Computer mit derselben Identität gleichzeitig im selben Netzwerk ausgeführt. 
    - Wenn Sie Änderungen auf virtuellen Testcomputern vornehmen, gehen diese Änderungen beim Bereinigen des Testfailovers verloren. Änderungen werden nicht zurück auf den primären virtuellen Computer repliziert.
    - Das Testen eines Produktionsnetzwerks führt zu einem Ausfall von Produktionsworkloads. Bitten Sie Ihre Benutzer, während der Notfallwiederherstellungsübung keine zugehörigen Apps zu verwenden. 
- Das Replikattestnetzwerk muss nicht dem für das Testfailover verwendeten Typ des logischen VMM-Netzwerks entsprechen. Einige Kombinationen funktionieren jedoch nicht. Wenn das Replikat beispielsweise auf DHCP und VLAN basierende Isolation verwendet, kann keine Windows-Netzwerkvirtualisierung für das Testfailover-Netzwerk verwendet werden, da dieses IP-Adresspools erfordert. 
- Es wird empfohlen, nicht das für die Netzwerkzuordnung ausgewählte Netzwerk für das Testfailover zu verwenden.


## <a name="run-a-test-failover-for-a-vm"></a>Ausführen eines Testfailovers für einen virtuellen Computer

1. Klicken Sie unter **Replizierte Elemente** auf die VM > **Testfailover**.
2. Geben Sie in **Testfailover** an, wie die virtuellen Computer nach dem Testfailover mit Netzwerken verbunden werden. Es wird empfohlen, im Rahmen dieses Tutorials mit Site Recovery automatisch ein Testnetzwerk zu erstellen. [Weitere Informationen](site-recovery-test-failover-vmm-to-vmm.md#network-options-in-site-recovery)
3. Klicken Sie auf **OK**, um den Failovervorgang zu starten. Überwachen Sie den Fortschritt auf der Registerkarte **Aufträge** .
4. Vergewissern Sie sich nach Abschluss des Failovers, dass die virtuellen Testcomputer erfolgreich gestartet werden.
5. Klicken Sie anschließend auf **Testfailover bereinigen**. Dadurch werden die Test-VMs und alle während des Testfailovers erstellten Netzwerke gelöscht.
6. Erfassen und speichern Sie unter **Notizen** alle Beobachtungen im Zusammenhang mit dem Testfailover. 


## <a name="next-steps"></a>Nächste Schritte

[Ausführen eines Produktionsfailovers](tutorial-vmm-to-vmm-failover-failback.md)







