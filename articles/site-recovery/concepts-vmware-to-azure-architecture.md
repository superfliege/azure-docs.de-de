---
title: "Überprüfen der Architektur für die VMware-Replikation in Azure | Microsoft-Dokumentation"
description: "Dieser Artikel bietet einen Überblick über die Komponenten und Architektur, die beim Replizieren von lokalen virtuellen VMware-Computern in Azure mit dem Azure Site Recovery-Dienst verwendet werden."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d03d2dd3-2455-4ca8-a942-a342030ee6ce
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/10/2017
ms.author: raynew
ms.openlocfilehash: ac1151d15a88650f5845cb879cd210e9f7cba0fd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="vmware-to-azure-replication-architecture"></a>VMware in der Architektur für die Azure-Replikation

Dieser Artikel beschreibt die Architektur und Prozesse, die mithilfe des Dienstes [Azure Site Recovery](site-recovery-overview.md) bei der Replikation, bei der Ausführung eines Failovers und beim Wiederherstellen von virtuellen VMware-Computern (VMs) zwischen einem lokalen VMware-Standort und Azure verwendet werden.


## <a name="architectural-components"></a>Komponenten der Architektur

Die folgende Tabelle und Grafik bietet eine Übersicht der Komponenten, die für die VMware-Replikation in Azure verwendet werden.  

**Komponente** | **Anforderung** | **Details**
--- | --- | ---
**Azure** | Ein Azure-Abonnement, ein Azure-Speicherkonto und ein Azure-Netzwerk | Replizierte Daten von lokalen VMs werden im Speicherkonto gespeichert. Azure-VMs werden mit den replizierten Daten erstellt, wenn Sie ein Failover von lokalen VMs in Azure ausführen. Für die Azure-VMs wird eine Verbindung mit dem virtuellen Azure-Netzwerk hergestellt, wenn diese erstellt werden.
**Konfigurationsserver** | Eine einzelne lokale VMware-VM wird bereitgestellt, um alle lokalen Komponenten von Site Recovery auszuführen. Die VM führt den Konfigurationsserver, Prozessserver und Masterzielserver aus. | Der Konfigurationsserver koordiniert die Kommunikation zwischen der lokalen Umgebung und Azure und verwaltet die Datenreplikation.
 **Prozessserver**  | Wird standardmäßig zusammen mit dem Konfigurationsserver installiert. | Fungiert als Replikationsgateway. Empfängt Replikationsdaten, optimiert sie durch Zwischenspeicherung, Komprimierung und Verschlüsselung und sendet sie an Azure Storage.<br/><br/> Der Prozessserver installiert auch den Mobility Service auf VMs, die Sie replizieren möchten, und führt auf lokalen VMware-Servern eine automatische Ermittlung durch.<br/><br/> Bei zunehmender Größe der Bereitstellung können Sie zusätzlich separate Prozessserver hinzufügen, um größere Mengen von Replikationsdatenverkehr zu bewältigen.
 **Masterzielserver** | Wird standardmäßig zusammen mit dem Konfigurationsserver installiert. | Verarbeitet die Replikationsdaten während des Failbacks von Azure.<br/><br/> Bei größeren Bereitstellungen können Sie einen zusätzlichen, separaten Masterzielserver für das Failback hinzufügen.
**VMware-Server** | VMware-Server werden auf lokalen vSphere ESXi-Servern gehostet. Zum Verwalten der Hosts wird ein vCenter-Server empfohlen. | Sie fügen während der Bereitstellung von Site Recovery VMware-Server in den Recovery Services-Tresor hinzu.
**Replizierte Computer** | Der Mobility Service wird auf jedem virtuellen VMware-Computer installiert, den Sie replizieren. | Es wird empfohlen, dass Sie die automatische Installation vom Prozessserver zulassen. Alternativ können Sie den Dienst manuell installieren oder eine automatisierte Bereitstellungsmethode wie System Center Configuration Manager verwenden. 

**Vmware-zu-Azure-Architektur**

![Komponenten](./media/concepts-vmware-to-azure-architecture/arch-enhanced.png)

## <a name="replication-process"></a>Replikationsprozess

1. Sie richten die Bereitstellung ein, einschließlich lokaler Komponenten und Azure-Komponenten. Im Recovery Services-Tresor geben Sie die Quelle und das Ziel der Replikation an, richten den Konfigurationsserver ein, erstellen eine Replikationsrichtlinie und aktivieren die Replikation.
2. Die Replikation der Computer beginnt gemäß der Replikationsrichtlinie, und eine erste Kopie der VM-Daten wird im Azure-Speicher repliziert.
3. Die Replikation von Deltaänderungen in Azure beginnt, nachdem die erste Replikation abgeschlossen wurde. Nachverfolgte Änderungen für einen Computer werden in einer HRL-Datei gespeichert.
    - Computer kommunizieren mit dem Konfigurationsserver über den Port HTTPS 443 für eingehenden Datenverkehr, um die Replikationsverwaltung durchzuführen.
    - Computer senden Replikationsdaten an den Prozessserver über den Port HTTPS 9443 für eingehenden Datenverkehr (Konfiguration möglich).
    - Der Konfigurationsserver orchestriert die Replikationsverwaltung mit Azure über Port HTTPS 443 für ausgehenden Datenverkehr.
    - Der Prozessserver empfängt Daten von Quellcomputern, optimiert und verschlüsselt sie und sendet sie über Port 443 für ausgehenden Datenverkehr an den Azure-Speicher.
    - Wenn Sie die Multi-VM-Konsistenz aktivieren, kommunizieren Computer in der Replikationsgruppe über den Port 20004 miteinander. Multi-VM wird verwendet, wenn Sie mehrere Computer in Replikationsgruppen zusammenfassen, für die bei einem Failover gemeinsame ausfallsichere und anwendungskonsistente Wiederherstellungspunkte verwendet werden. Dies ist hilfreich, wenn auf Computern dieselbe Workload ausgeführt wird und die Computer einheitlich sein müssen.
4. Der Datenverkehr wird auf öffentlichen Endpunkten des Azure-Speichers über das Internet repliziert. Alternativ hierzu können Sie das [öffentliche Peering](../expressroute/expressroute-circuit-peerings.md#azure-public-peering) von Azure ExpressRoute verwenden. Das Replizieren von Datenverkehr über ein Site-to-Site-VPN von einem lokalen Standort nach Azure wird nicht unterstützt.


**Replikationsprozess von VMware zu Azure**

![Replikationsprozess](./media/concepts-vmware-to-azure-architecture/v2a-architecture-henry.png)

## <a name="failover-and-failback-process"></a>Failover- und Failbackprozesse

Nachdem die Replikation eingerichtet ist und Sie einen Notfallwiederherstellungsdrill (Testfailover) ausgeführt haben, um zu prüfen, ob alles wie erwartet arbeitet, können Sie bei Bedarf ein Failover und Failback ausführen.

1. Sie können ein Failover für einen einzelnen Computer ausführen oder Wiederherstellungspläne erstellen, um ein Failover für mehrere Computer auszuführen.
2. Wenn Sie ein Failover ausführen, werden Azure-VMs aus replizierten Daten im Azure-Speicher erstellt.
3. Nachdem das erste Failover ausgelöst wird, committen Sie es, um von der Azure-VM auf die Workload zuzugreifen.

Wenn Ihr primärer lokaler Standort wieder verfügbar ist, können Sie das Failback durchführen.
1. Sie müssen eine Failbackinfrastruktur festlegen, die Folgendes enthält:
    - **Temporärer Prozessserver in Azure**: Sie legen für ein Failback von Azure eine Azure-VM fest, die als Prozessserver agiert, um die Replikation von Azure zu verarbeiten. Nach Beendigung des Failbacks können Sie diese VM löschen.
    - **VPN-Verbindung**: Für das Failback benötigen Sie eine VPN-Verbindung (oder Azure ExpressRoute-Verbindung) zwischen dem Azure-Netzwerk und dem lokalen Standort.
    - **Separater Masterzielserver**: Der lokale Masterzielserver, der mit dem Konfigurationsserver in der lokalen VMware-VM installiert wurde, führt standardmäßig das Failback durch. Wenn Sie jedoch ein Failback für große Mengen an Datenverkehr ausführen müssen, sollten Sie einen separaten lokalen Masterzielserver einrichten.
    - **Failbackrichtlinie**: Für die Replikation zurück an den lokalen Standort benötigen Sie eine Failbackrichtlinie. Sie wurde automatisch erstellt, als Sie die Replikationsrichtlinie aus lokalen VMs in Azure erstellt haben.
2. Nachdem die Komponenten vorhanden sind, tritt das Failback in drei Phasen auf:
    - Phase 1: Schützen Sie die Azure VMs erneut, sodass Sie die Replikation von Azure zurück zu den lokalen VMware-VMs durchführen.
    - Phase 2: Führen Sie ein Failover zum lokalen Standort aus.
    - Phase 3: Nachdem für Workloads das Failback ausgeführt wurde, aktivieren Sie erneut das Replikat für die lokalen VMs.

**VMware-Failback von Azure**

![Failback](./media/concepts-vmware-to-azure-architecture/enhanced-failback.png)


## <a name="next-steps"></a>Nächste Schritte

Überprüfen Sie die Supportmatrix. Folgen Sie dem Tutorial, um VMware in der Azure-Replikation zu aktivieren.
Führen Sie ein Failover und ein Failback aus.