---
title: Architektur der VMware-zu-Azure-Replikation in Azure Site Recovery | Microsoft-Dokumentation
description: Dieser Artikel bietet eine Übersicht über die Komponenten und Architektur, die beim Replizieren von lokalen virtuellen VMware-Computern in Azure mit Azure Site Recovery verwendet werden.
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: raynew
ms.openlocfilehash: 61c283c178936c98a9a18509c1b46035e48f8f24
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36285269"
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

## <a name="configuration-steps"></a>Konfigurationsschritte

Die folgenden allgemeinen Schritte zum Einrichten von VMware für Azure-Notfallwiederherstellung oder -Migration sind erforderlich:

1. **Einrichten von Azure-Komponenten**. Sie benötigen ein Azure-Konto mit den richtigen Berechtigungen, ein Azure-Speicherkonto, ein virtuelles Azure-Netzwerk und einen Recovery Services-Tresor. [Weitere Informationen](tutorial-prepare-azure.md)
2. **Lokale Einrichtung**. Dazu gehören die Einrichtung eines Kontos auf dem VMware-Server, damit Site Recovery automatisch die zu replizierenden virtuellen Computer ermitteln kann, die Einrichtung eines Kontos, mit dem die Mobility Service-Komponente auf den zu replizierenden virtuellen Computern installiert werden kann, und die Überprüfung, ob VMware-Server und die virtuellen Computer die Voraussetzungen erfüllen. Sie können optional auch eine Verbindung mit diesen virtuellen Azure-Computern nach einem Failover vorbereiten. Site Recovery repliziert Daten virtueller Computer in ein Azure-Speicherkonto und erstellt virtuelle Azure-Computer unter Verwendung der Daten, wenn Sie ein Failover in Azure ausführen. [Weitere Informationen](vmware-azure-tutorial-prepare-on-premises.md)
3. **Einrichten der Replikation**. Sie wählen aus, wohin Sie replizieren möchten. Sie konfigurieren die Quellreplikationsumgebung, indem Sie einen einzelnen lokalen virtuellen VMware-Computer (den Konfigurationsserver) einrichten, der alle benötigten lokalen Site Recovery-Komponenten ausführt. Nach der Einrichtung registrieren Sie den Konfigurationsserver im Recovery Services-Tresor. Anschließend wählen Sie die Zieleinstellungen aus. [Weitere Informationen](vmware-azure-tutorial.md)
4. **Erstellen einer Replikationsrichtlinie**. Sie erstellen eine Replikationsrichtlinie, die angibt, wie die Replikation ausgeführt werden soll. 
    - **RPO-Schwellenwert**: Diese Überwachungseinstellung besagt, dass eine Warnung (und optional eine E-Mail) ausgegeben wird, wenn die Replikation nicht innerhalb der angegebenen Zeit erfolgt. Wenn Sie den RPO-Schwellenwert z.B. auf 30 Minuten festlegen und ein Problem die Replikation 30 Minuten lang verhindert, wird ein Ereignis generiert. Diese Einstellung besitzt keine Auswirkungen auf die Replikation. Die Replikation ist fortlaufend, und Wiederherstellungspunkte werden in Intervallen von wenigen Minuten erstellt.
    - **Aufbewahrung**: Der Aufbewahrungszeitraum des Wiederherstellungspunkts gibt an, wie lange Wiederherstellungspunkte in Azure beibehalten werden sollen. Sie können einen Wert zwischen 0 und 24 Stunden für Storage Premium oder bis zu 72 Stunden für Standardspeicher angeben. Sie können ein Failover zum letzten Wiederherstellungspunkt oder zu einem gespeicherten Punkt ausführen, wenn Sie einen Wert größer als 0 festlegen. Nach Ablauf des Aufbewahrungszeitraum-Fensters werden Wiederherstellungspunkte bereinigt.
    - **Ausfallsichere Momentaufnahmen**: Standardmäßig generiert Site Recovery ausfallsichere Momentaufnahmen und erstellt mit diesen im Abstand weniger Minuten Wiederherstellungspunkte. Ein Wiederherstellungspunkt ist ausfallsicher, wenn alle zusammenhängenden Datenkomponenten wie zum Zeitpunkt der Erstellung des Wiederherstellungspunkts in der Schreibreihenfolge konsistent sind. Zum besseren Verständnis stellen Sie sich den Status der Daten auf dem Festplattenlaufwerk Ihres PCs nach einem Stromausfall oder einem ähnlichen Ereignis vor. Ein ausfallsicherer Wiederherstellungspunkt ist in der Regel ausreichend, wenn Ihre Anwendung für die Wiederherstellung nach einem Ausfall ohne Dateninkonsistenzen konzipiert ist.
    - **App-konsistente Momentaufnahmen**: Wenn dieser Wert nicht 0 ist, versucht der Mobility Service, der auf dem virtuellen Computer ausgeführt wird, dateisystemkonsistente Momentaufnahmen und Wiederherstellungspunkte zu generieren. Die erste Momentaufnahme wird erstellt, nachdem die anfängliche Replikation abgeschlossen wurde. Momentaufnahmen werden dann mit der Häufigkeit erstellt, die Sie angeben. Ein Wiederherstellungspunkt ist anwendungskonsistent, wenn nicht nur die Schreibreihenfolge konsistent ist, sondern auch die aktuell ausgeführten Anwendungen alle ihre Vorgänge abschließen und ihre Puffer auf der Festplatte speichern können (Anwendungsstilllegung). App-konsistente Wiederherstellungspunkte werden für Datenbankanwendungen wie SQL, Oracle und Exchange empfohlen. Wenn eine ausfallsichere Momentaufnahme ausreicht, kann dieser Wert auf 0 festgelegt werden.  
    - **Multi-VM-Konsistenz**: Sie können optional eine Replikationsgruppe erstellen. Wenn Sie die Replikation aktivieren, können Sie dann virtuelle Computer in dieser Gruppe sammeln. Virtuelle Computer in einer Replikationsgruppe werden gemeinsam repliziert und verfügen beim Failover über gemeinsame ausfallsichere und app-konsistente Wiederherstellungspunkte. Sie sollten diese Option mit Vorsicht verwenden, da sie die Workloadleistung beeinträchtigen kann, da Momentaufnahmen über mehrere Computer hinweg gesammelt werden müssen. Gehen Sie nur dann so vor, wenn virtuelle Computer die gleiche Workload ausführen und konsistent sein müssen und die virtuellen Computer ähnliche Änderungen aufweisen. Sie können einer Gruppe bis zu 8 virtuelle Computer hinzufügen. 
5. **Aktivieren der Replikation virtueller Computer**. Schließlich aktivieren Sie die Replikation für Ihre lokalen virtuellen VMware-Computer. Wenn Sie ein Konto für die Installation von Mobility Service erstellt und angegeben haben, dass Site Recovery eine Pushinstallation ausführen soll, wird Mobility Service auf jedem virtuellen Computer installiert, für den Sie Replikation aktivieren. [Weitere Informationen](vmware-azure-tutorial.md#enable-replication) Wenn Sie eine Replikationsgruppe für Multi-VM-Konsistenz erstellt haben, können Sie dieser Gruppe virtuelle Computer hinzufügen.
6. **Testfailover**. Nachdem alles eingerichtet wurde, können Sie ein Testfailover ausführen, um zu überprüfen, ob die virtuellen Computer wie erwartet ein Failover in Azure ausführen. [Weitere Informationen](tutorial-dr-drill-azure.md)
7. **Failover**. Wenn Sie nur die virtuellen Computer zu Azure migrieren: Sie führen zu diesem Zweck ein vollständiges Failover aus. Wenn Sie Notfallwiederherstellung einrichten, können Sie wie erforderlich ein vollständiges Failover ausführen. Für vollständige Notfallwiederherstellung können Sie nach dem Failover in Azure ein Failback auf Ihren lokalen Standort ausführen, wenn dieser verfügbar ist. [Weitere Informationen](vmware-azure-tutorial-failover-failback.md)

## <a name="replication-process"></a>Replikationsprozess

1. Wenn Sie die Replikation für einen virtuellen Computer aktivieren, beginnt dieser gemäß der Replikationsrichtlinie mit der Replikation. 
2. Der Datenverkehr wird über das Internet auf öffentliche Endpunkte von Azure Storage repliziert. Alternativ hierzu können Sie Azure ExpressRoute mit [öffentlichem Peering](../expressroute/expressroute-circuit-peerings.md#azure-public-peering) verwenden. Das Replizieren von Datenverkehr über ein virtuelles privates Site-to-Site-Netzwerk (VPN) von einem lokalen Standort nach Azure wird nicht unterstützt.
3. Eine erste Kopie der Daten der virtuellen Computer wird in den Azure-Speicher repliziert.
4. Die Replikation von Deltaänderungen in Azure beginnt, nachdem die erste Replikation abgeschlossen wurde. Nachverfolgte Änderungen für einen Computer werden in einer HRL-Datei gespeichert.
5. Die Kommunikation erfolgt folgendermaßen:

    - Virtuelle Computer kommunizieren mit dem lokalen Konfigurationsserver über den Port HTTPS 443 für eingehenden Datenverkehr, um die Replikationsverwaltung auszuführen.
    - Der Konfigurationsserver orchestriert die Replikation mit Azure über Port HTTPS 443 für ausgehenden Datenverkehr.
    - Virtuelle Computer senden Replikationsdaten an den Prozessserver (der auf dem Konfigurationsserver ausgeführt wird) über Port HTTPS 9443 für eingehenden Datenverkehr. Dieser Port kann geändert werden.
    - Der Prozessserver empfängt Replikationsdaten, optimiert und verschlüsselt sie und sendet sie über den ausgehenden Port 443 an den Azure-Speicher.


**Replikationsprozess von VMware zu Azure**

![Replikationsprozess](./media/vmware-azure-architecture/v2a-architecture-henry.png)

## <a name="failover-and-failback-process"></a>Failover- und Failbackprozesse

Nachdem die Replikation eingerichtet ist und Sie einen Notfallwiederherstellungsdrill (Testfailover) ausgeführt haben, um zu überprüfen, ob alles wie erwartet funktioniert, können Sie bei Bedarf ein Failover und ein Failback ausführen.

1. Sie können ein Failover für einen einzelnen Computer ausführen oder Wiederherstellungspläne erstellen, um ein Failover für mehrere virtuelle Computer gleichzeitig auszuführen. Ein Wiederherstellungsplan besitzt gegenüber dem Failover eines einzelnen Computers die folgenden Vorteile:
    - Sie können App-Abhängigkeiten modellieren, indem Sie alle virtuellen Computer in der App in einen einzelnen Wiederherstellungsplan einschließen.
    - Sie können Skripts und Azure-Runbooks hinzufügen und den Vorgang für manuelle Aktionen anhalten.
2. Nachdem das erste Failover ausgelöst wird, committen Sie es, um von der Azure-VM auf die Workload zuzugreifen.
3. Wenn Ihr primärer lokaler Standort wieder verfügbar ist, können Sie sich auf ein Failback vorbereiten. Damit das Failback ausgeführt werden kann, müssen Sie eine Failbackinfrastruktur festlegen, die Folgendes enthält:

    * **Temporärer Prozessserver in Azure:** Sie legen für ein Failback von Azure eine Azure-VM fest, die als Prozessserver fungiert, um die Replikation von Azure zu verarbeiten. Nach Beendigung des Failbacks können Sie diese VM löschen.
    * **VPN-Verbindung:** Für das Failback benötigen Sie eine VPN-Verbindung (oder ExpressRoute-Verbindung) zwischen dem Azure-Netzwerk und dem lokalen Standort.
    * **Separater Masterzielserver:** Der lokale Masterzielserver, der mit dem Konfigurationsserver in der lokalen VMware-VM installiert wurde, führt standardmäßig das Failback aus. Wenn Sie ein Failback für große Volumen von Datenverkehr ausführen müssen, richten Sie einen separaten lokalen Masterzielserver ein.
    * **Failbackrichtlinie**: Für die Replikation zurück an den lokalen Standort benötigen Sie eine Failbackrichtlinie. Diese Richtlinie wurde automatisch erstellt, als Sie die Replikationsrichtlinie aus lokalen VMs in Azure erstellt haben.
4. Nachdem die Komponenten vorhanden sind, erfolgt das Failback in drei Phasen:

    - Phase 1: Schützen Sie die Azure VMs erneut, sodass Sie die Replikation von Azure zurück zu den lokalen VMware-VMs durchführen.
    -  Phase 2: Führen Sie ein Failover zum lokalen Standort aus.
    - Phase 3: Nachdem für Workloads ein Failback ausgeführt wurde, aktivieren Sie erneut die Replikation für die lokalen VMs.
    
 
**VMware-Failback von Azure**

![Failback](./media/vmware-azure-architecture/enhanced-failback.png)


## <a name="next-steps"></a>Nächste Schritte

Absolvieren Sie [dieses Tutorial](vmware-azure-tutorial.md), um eine VMware-zu-Azure-Replikation zu ermöglichen.
