---
title: Architektur der VMware-zu-Azure-Replikation in Azure Site Recovery | Microsoft-Dokumentation
description: "Dieser Artikel bietet einen Überblick über die Komponenten und Architektur, die beim Replizieren von lokalen virtuellen VMware-Computern in Azure mit Azure Site Recovery verwendet werden."
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 02/27/2018
ms.author: raynew
ms.openlocfilehash: 3d20ce1da2ed9b6e3213c9689b49cc2d759e5376
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="vmware-to-azure-replication-architecture"></a>VMware in der Architektur für die Azure-Replikation

Dieser Artikel beschreibt die Architektur und Prozesse, die mithilfe von [Azure Site Recovery](site-recovery-overview.md) bei der Replikation, bei der Ausführung eines Failovers und beim Wiederherstellen von virtuellen VMware-Computern (VMs) zwischen einem lokalen VMware-Standort und Azure verwendet werden.


## <a name="architectural-components"></a>Komponenten der Architektur

Die folgende Tabelle und Grafik bietet eine Übersicht der Komponenten, die für die VMware-Replikation in Azure verwendet werden.

**Komponente** | **Anforderung** | **Details**
--- | --- | ---
**Azure** | Ein Azure-Abonnement, ein Azure Storage-Konto und ein Azure-Netzwerk. | Replizierte Daten von lokalen VMs werden im Speicherkonto gespeichert. Azure-VMs werden mit den replizierten Daten erstellt, wenn Sie ein Failover von lokalen VMs in Azure ausführen. Für die Azure-VMs wird eine Verbindung mit dem virtuellen Azure-Netzwerk hergestellt, wenn diese erstellt werden.
**Konfigurationsservercomputer** | Ein einzelner lokaler Computer. Es wird empfohlen, diesen als eine VMware-VM auszuführen, die aus einer heruntergeladenen OVF-Vorlage bereitgestellt werden kann.<br/><br/> Auf dem Computer werden alle lokalen Site Recovery-Komponenten ausgeführt, einschließlich Konfigurationsserver, Prozessserver und Masterzielserver. | **Konfigurationsserver:** koordiniert die Kommunikation zwischen der lokalen VMware-Umgebung und Azure und verwaltet die Datenreplikation.<br/><br/> **Prozessserver:** Wird standardmäßig auf dem Konfigurationsserver installiert. Er empfängt Replikationsdaten, optimiert sie durch Zwischenspeicherung, Komprimierung und Verschlüsselung und sendet sie an Azure Storage. Der Prozessserver installiert auch Azure Site Recovery Mobility Service auf VMs, die Sie replizieren möchten, und führt auf lokalen Computern eine automatische Ermittlung durch. Bei zunehmender Größe der Bereitstellung können Sie zusätzlich separate Prozessserver hinzufügen, um größere Mengen von Replikationsdatenverkehr zu bewältigen.<br/><br/> **Masterzielserver:** wird standardmäßig auf dem Konfigurationsserver installiert. Er verarbeitet die Replikationsdaten während des Failbacks von Azure. Bei größeren Bereitstellungen können Sie einen zusätzlichen, separaten Masterzielserver für das Failback hinzufügen.
**VMware-Server** | VMware-Server werden auf lokalen vSphere ESXi-Servern gehostet. Zum Verwalten der Hosts wird ein vCenter-Server empfohlen. | Sie fügen während der Bereitstellung von Site Recovery VMware-Server in den Recovery Services-Tresor hinzu.
**Replizierte Computer** | Mobility Service wird auf jedem virtuellen VMware-Computer installiert, den Sie replizieren. | Es wird empfohlen, dass Sie die automatische Installation vom Prozessserver zulassen. Alternativ können Sie den Dienst manuell installieren oder eine automatisierte Bereitstellungsmethode wie System Center Configuration Manager verwenden.

**Vmware-zu-Azure-Architektur**

![Komponenten](./media/vmware-azure-architecture/arch-enhanced.png)

## <a name="replication-process"></a>Replikationsprozess

1. Bereiten Sie Azure-Ressourcen und lokale Komponenten vor.
2. Geben Sie im Recovery Services-Tresor die Replikationseinstellungen für die Quelle an. Richten Sie im Rahmen dieses Prozesses den lokalen Konfigurationsserver ein. Um diesen Server als eine VMware-VM bereitzustellen, laden Sie eine vorbereitete OVF-Vorlage herunter und importieren sie in VMware, um den virtuellen Computer zu erstellen.
3. Geben Sie die Replikationseinstellungen für das Ziel an, erstellen Sie eine Replikationsrichtlinie, und aktivieren Sie die Replikation für Ihre VMware-VMs.
4. Die Replikation der Computer beginnt gemäß der Replikationsrichtlinie, und eine erste Kopie der VM-Daten wird in Azure Storage repliziert.
5. Die Replikation von Deltaänderungen in Azure beginnt, nachdem die erste Replikation abgeschlossen wurde. Nachverfolgte Änderungen für einen Computer werden in einer HRL-Datei gespeichert.

    * Computer kommunizieren mit dem Konfigurationsserver über den Port HTTPS 443 für eingehenden Datenverkehr, um die Replikationsverwaltung durchzuführen.

    * Computer senden Replikationsdaten an den Prozessserver über den Port HTTPS 9443 für eingehenden Datenverkehr (Konfiguration möglich).

    * Der Konfigurationsserver orchestriert die Replikationsverwaltung mit Azure über Port HTTPS 443 für ausgehenden Datenverkehr.

    * Der Prozessserver empfängt Daten von Quellcomputern, optimiert und verschlüsselt sie und sendet sie über Port 443 für ausgehenden Datenverkehr an Azure Storage.

    * Wenn Sie die Multi-VM-Konsistenz aktivieren, kommunizieren Computer in der Replikationsgruppe über den Port 20004 miteinander. Multi-VM wird verwendet, wenn Sie mehrere Computer in Replikationsgruppen zusammenfassen, für die bei einem Failover gemeinsame ausfallsichere und anwendungskonsistente Wiederherstellungspunkte verwendet werden. Dieses Verfahren ist hilfreich, wenn auf Computern dieselbe Workload ausgeführt wird und die Computer einheitlich sein müssen.

6. Der Datenverkehr wird über das Internet auf öffentliche Endpunkte von Azure Storage repliziert. Alternativ hierzu können Sie das [öffentliche Peering](../expressroute/expressroute-circuit-peerings.md#azure-public-peering) von Azure ExpressRoute verwenden. Das Replizieren von Datenverkehr über ein virtuelles privates Site-to-Site-Netzwerk von einem lokalen Standort nach Azure wird nicht unterstützt.


**Replikationsprozess von VMware zu Azure**

![Replikationsprozess](./media/vmware-azure-architecture/v2a-architecture-henry.png)

## <a name="failover-and-failback-process"></a>Failover- und Failbackprozesse

Nachdem die Replikation eingerichtet ist und Sie einen Notfallwiederherstellungsdrill (Testfailover) ausgeführt haben, um zu überprüfen, ob alles wie erwartet funktioniert, können Sie bei Bedarf ein Failover und Failback ausführen.

1. Sie können ein Failover für einen einzelnen Computer ausführen oder Wiederherstellungspläne erstellen, um ein Failover für mehrere Computer auszuführen.

2. Wenn Sie ein Failover ausführen, werden Azure-VMs aus replizierten Daten im Azure-Speicher erstellt.

3. Nachdem das erste Failover ausgelöst wird, committen Sie es, um von der Azure-VM auf die Workload zuzugreifen.

Wenn Ihr primärer lokaler Standort wieder verfügbar ist, können Sie das Failback durchführen.
1. Sie müssen eine Failbackinfrastruktur festlegen, die Folgendes enthält:

    * **Temporärer Prozessserver in Azure**: Sie legen für ein Failback von Azure eine Azure-VM fest, die als Prozessserver fungiert, um die Replikation von Azure zu verarbeiten. Nach Beendigung des Failbacks können Sie diese VM löschen.

    * **VPN-Verbindung**: Für das Failback benötigen Sie eine VPN-Verbindung (oder ExpressRoute-Verbindung) zwischen dem Azure-Netzwerk und dem lokalen Standort.

    * **Separater Masterzielserver**: Der lokale Masterzielserver, der mit dem Konfigurationsserver in der lokalen VMware-VM installiert wurde, führt standardmäßig das Failback durch. Wenn Sie ein Failback für große Volumen von Datenverkehr ausführen müssen, richten Sie einen separaten lokalen Masterzielserver ein.

    * **Failbackrichtlinie**: Für die Replikation zurück an den lokalen Standort benötigen Sie eine Failbackrichtlinie. Diese Richtlinie wurde automatisch erstellt, als Sie die Replikationsrichtlinie aus lokalen VMs in Azure erstellt haben.
2. Nachdem die Komponenten vorhanden sind, tritt das Failback in drei Phasen auf:

    a. Phase 1: Schützen Sie die Azure VMs erneut, sodass Sie die Replikation von Azure zurück zu den lokalen VMware-VMs durchführen.

    b. Phase 2: Führen Sie ein Failover zum lokalen Standort aus.

    c. Phase 3: Nachdem für Workloads das Failback ausgeführt wurde, aktivieren Sie erneut Replikation für die lokalen VMs.

**VMware-Failback von Azure**

![Failback](./media/vmware-azure-architecture/enhanced-failback.png)


## <a name="next-steps"></a>Nächste Schritte

Absolvieren Sie [dieses Tutorial](vmware-azure-tutorial.md), um eine VMware-zu-Azure-Replikation zu ermöglichen.
