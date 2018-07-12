---
title: Verwenden von ExpressRoute zur Notfallwiederherstellung von virtuellen Azure-Computern | Microsoft-Dokumentation
description: Beschreibt, wie Sie Azure ExpressRoute mit Azure Site Recovery für die Notfallwiederherstellung von virtuellen Azure-Computern verwenden
services: site-recovery
documentationcenter: ''
author: mayanknayar
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: manayar
ms.openlocfilehash: 73514b524f554affb9730ba63ccd608491497af2
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2018
ms.locfileid: "37920469"
---
# <a name="using-expressroute-with-azure-virtual-machine-disaster-recovery"></a>Verwenden von ExpressRoute für die Notfallwiederherstellung von virtuellen Azure-Computern

Mit Microsoft Azure ExpressRoute können Sie Ihre lokalen Netzwerke über eine private Verbindung, die von einem Konnektivitätsanbieter bereitgestellt wird, in die Microsoft Cloud erweitern. Dieser Artikel beschreibt, wie Sie ExpressRoute mit Azure Site Recovery zur Notfallwiederherstellung von virtuellen Azure-Computern verwenden.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie beginnen, stellen Sie sicher, dass Sie mit folgenden Konzepten vertraut sind:
-   ExpressRoute-[Verbindungen](../expressroute/expressroute-circuit-peerings.md)
-   ExpressRoute-[Routingdomänen](../expressroute/expressroute-circuit-peerings.md#expressroute-routing-domains)
-   [Architektur der Replikation](azure-to-azure-architecture.md) von virtuellen Azure-Computern
-   [Einrichten der Replikation](azure-to-azure-tutorial-enable-replication.md) für virtuelle Azure-Computer
-   [Ausführen des Failovers](azure-to-azure-tutorial-failover-failback.md) von virtuellen Azure-Computern

## <a name="expressroute-and-azure-virtual-machine-replication"></a>ExpressRoute und Replikation von virtuellen Azure-Computern

Beim Schutz von virtuellen Azure-Computern mit Azur Site Recovery werden Replikationsdaten an ein Azure Storage-Konto gesendet oder ein verwalteter Replikatdatenträger in der Azure-Zielregion erstellt, je nachdem, ob Ihre virtuellen Azure-Computer [Azure Managed Disks](../virtual-machines/windows/managed-disks-overview.md) verwenden. Obwohl die Replikationsendpunkte öffentlich sind, läuft der Replikationsdatenverkehr für die Replikation von Azure VM nicht über das Internet, unabhängig davon, in welcher Azure-Region sich das virtuelle Quellnetzwerk befindet.

Die Replikationsdaten bleiben bei der Azure VM-Notfallwiederherstellung innerhalb der Azure-Begrenzung. Daher ist ExpressRoute für die Replikation nicht erforderlich. Nach dem Failover der virtuellen Computer zur Azure-Zielregion können Sie darauf über [privates Peering](../expressroute/expressroute-circuit-peerings.md#azure-private-peering) zugreifen.

## <a name="replicating-azure-deployments"></a>Replizieren von Azure-Bereitstellungen

Ein früherer [Artikel](site-recovery-retain-ip-azure-vm-failover.md#on-premises-to-azure-connectivity) beschreibt eine einfache Einrichtung, bei der ein einzelnes virtuelles Azure-Netzwerk über ExpressRoute mit dem lokalen Kundendatencenter verbunden wird. Die Workloads herkömmlicher Enterprise-Bereitstellungen sind auf mehrere virtuelle Azure-Netzwerke verteilt. Ein zentraler Konnektivitätshub stellt dabei externe Konnektivität sowohl mit dem Internet als auch mit lokalen Bereitstellungen her.

In diesem Beispiel werden eine Hub- und Spoke-Topologie beschrieben, wie sie bei Enterprise-Bereitstellungen verbreitet sind:
-   Die Bereitstellung erfolgt in der **Azure-Asien-Pazifik-**-Region. Das lokale Datencenter besitzt eine ExpressRoute-Verbindung über einen Partner-Edge in Hongkong.
-   Die Anwendungen werden über zwei virtuelle Spoke-Netzwerke bereitgestellt: das **Quell-VNet1** mit dem Adressraum 10.1.0.0/24 und das **Quell-VNet2** mit dem Adressraum 10.2.0.0/24.
-   Das virtuelle Hubnetzwerk, der **Quellhub VNet** mit dem Adressraum 10.10.10.0/24, fungiert als Gatekeeper. Die gesamte Kommunikation über mehrere Subnetze hinweg erfolgt über den Hub.
-   Das virtuelle Hubnetzwerk besteht aus zwei Subnetzen: das **NVA-Subnetz** mit dem Adressraum 10.10.10.0/25 und das **Gatewaysubnetz** mit dem Adressraum 10.10.10.128/25.
-   Das **NVA-Subnetz** verfügt über eine virtuelle Netzwerkanwendung mit der IP-Adresse 10.10.10.10.
-   Das **Gatewaysubnetz** verfügt über ein ExpressRoute-Gateway mit einer ExpressRoute-Verbindung. Diese ist über eine private Peering-Routingdomäne mit dem lokalen Kundendatencenter verbunden.
-   Jedes einzelne virtuelle Spoke-Netzwerk ist mit dem virtuellen Hubnetzwerk verbunden. Das gesamte Routing innerhalb dieser Netzwerktopologie wird über Azure-Routingtabellen (UDR) gesteuert. Der gesamte ausgehende Datenverkehr von einem VNet zum einem anderen VNet bzw. zu einem lokalen Datencenter wird über die NVA (Network Virtual Appliance, virtuelle Netzwerk-Appliance) weitergeleitet.

![Konnektivität zwischen lokalem Datencenter und Azure mit ExpressRoute vor einem Failover](./media/azure-vm-disaster-recovery-with-expressroute/site-recovery-with-expressroute-before-failover.png)

### <a name="hub-and-spoke-peering"></a>Hub- und Spoke-Peering

Das Spoke-zu-Hub-Peering weist folgende Konfiguration auf:
-   Allow virtual network address: Enabled (Virtuelle Netzwerkadresse zulassen: Aktiviert)
-   Weitergeleiteten Datenverkehr zulassen: Aktiviert
-   Gatewaytransit zulassen: Deaktiviert
-   Use remove gateways: Enabled („Gateways entfernen“ verwenden: Aktiviert)

 ![Konfiguration des Spoke-zu-Hub-Peerings](./media/azure-vm-disaster-recovery-with-expressroute/spoke-to-hub-peering-configuration.png)

Das Hub-zu-Spoke-Peering weist folgende Konfiguration auf:
-   Allow virtual network address: Enabled (Virtuelle Netzwerkadresse zulassen: Aktiviert)
-   Weitergeleiteten Datenverkehr zulassen: Aktiviert
-   Gatewaytransit zulassen: Aktiviert
-   Use remove gateways: Enabled („Gateways entfernen“ verwenden: Deaktiviert)

 ![Konfiguration des Hub-zu-Spoke-Peerings](./media/azure-vm-disaster-recovery-with-expressroute/hub-to-spoke-peering-configuration.png)

### <a name="enabling-replication-for-the-deployment"></a>Aktivieren der Replikation für die Bereitstellung

Für die zuvor beschriebene Einrichtung führen Sie zunächst folgenden Schritt aus: [Einrichten der Notfallwiederherstellung](azure-to-azure-tutorial-enable-replication.md) für jeden virtuellen Computer mithilfe von Site Recovery. Azure Site Recovery kann das Replikat virtueller Netzwerke (einschließlich Subnetze und Gatewaysubnetze) in der Zielregion erstellen und das erforderlichen Mapping zwischen den virtuellen Quell- und Zielnetzwerken ausführen. Sie können die Zielnetzwerke und -subnetze jedoch auch im Voraus erstellen und sie beim Aktivieren der Replikation verwenden.

Azure Site Recovery repliziert weder Routingtabellen, Gateways des virtuellen Netzwerks, virtuelle Netzwerk-Gateway-Verbindungen noch virtuelles Netzwerk-Peering oder andere Netzwerkressourcen oder -verbindungen. Diese und andere Ressourcen, die nicht Teil des [Replikationsprozesses](azure-to-azure-architecture.md#replication-process) sind, müssen während oder vor dem Failover erstellt und mit den relevanten Ressourcen verbunden werden. Sie können die leistungsstarken [Wiederherstellungspläne](site-recovery-create-recovery-plans.md) von Azure Site Recovery nutzen, um das Erstellen und Verbinden zusätzlicher Ressourcen mithilfe von Automatisierungsskripts zu automatisieren.

Standardmäßig bleibt der Replikationsdatenverkehr innerhalb der Azure-Begrenzung. Üblicherweise wird bei NVA-Bereitstellungen eine Standardroute (0.0.0.0/0) festgelegt, die die Übermittlung von ausgehendem Internetdatenverkehr durch die NVA erzwingt. In diesem Fall kann die Appliance gedrosselt werden, wenn der gesamte Replikationsdatenverkehr durch die NVA läuft. Ähnliches gilt bei der Verwendung von Standardrouten für das Routing des gesamten Azure VM-Datenverkehrs zu lokalen Bereitstellungen. Sie können [einen virtuellen Netzwerk-Dienstendpunkt](azure-to-azure-about-networking.md#create-network-service-endpoint-for-storage) in Ihrem virtuellen Netzwerk für „Storage“ erstellen, damit der Replikationsdatenverkehr innerhalb der Azure-Begrenzung bleibt.

## <a name="failover-models-with-expressroute"></a>Failover-Modelle mit ExpressRoute

Beim Failover von virtuellen Azure-Computern zu einer anderen Region wird die vorhandene ExpressRoute-Verbindung mit dem virtuellen Quellnetzwerk nicht automatisch an das virtuelle Zielnetzwerk in der Wiederherstellungsregion übertragen. Um ExpressRoute mit dem virtuellen Zielnetzwerk zu verbinden, ist zunächst eine neue Verbindung erforderlich.

Sie können virtuelle Azure-Computer zu einer beliebigen Azure-Region innerhalb des gleichen geografischen Clusters replizieren, wie [hier](azure-to-azure-support-matrix.md#region-support) beschrieben. Wenn sich die ausgewählte Azure-Zielregion nicht innerhalb der gleichen geopolitischen Region wie die Quelle befindet, müssen Sie ExpressRoute Premium aktivieren, wenn Sie eine einzelne ExpressRoute-Verbindung für Quell- und Zielregionskonnektivität verwenden. Weitere Informationen finden Sie unter [ExpressRoute-Standorte](../expressroute/expressroute-locations.md#azure-regions-to-expressroute-locations-within-a-geopolitical-region) und [ExpressRoute – Preise](https://azure.microsoft.com/pricing/details/expressroute/).

### <a name="two-expressroute-circuits-in-two-different-expressroute-peering-locations"></a>Zwei ExpressRoute-Verbindungen in zwei unterschiedlichen ExpressRoute-Peering-Standorten
-   Diese Konfiguration bietet sich an, wenn Sie einem Ausfall der ExpressRoute-Hauptverbindung sowie umfangreichen regionalen Notfällen vorbeugen möchten, die die ExpressRoute-Peering-Standorte beeinträchtigen und Ihre ExpressRoute-Hauptverbindung unterbrechen könnten.
-   Die Verbindung mit der Produktionsumgebung wird normalerweise als primäre Verbindung verwendet, während die sekundäre Verbindung als Ausfallsicherheit dient und in der Regel eine geringere Bandbreite aufweist. Die Bandbreite der sekundären Verbindung kann in einem Notfall erhöht werden, wenn die sekundäre Verbindung die primäre ersetzen muss.
-   Mit dieser Konfiguration können Sie nach einem Failover Verbindungen zwischen der sekundären ExpressRoute-Verbindung und dem virtuellen Zielnetzwerk herstellen. Sie können die Verbindungen aber auch für eine Notfallwiederherstellung einrichten und damit Ihre Wiederherstellungszeit reduzieren. Mit gleichzeitigen Verbindungen zu sowohl primären virtuellen Netzwerken als auch virtuellen Netzwerken der Zielregion stellen Sie sicher, dass Ihr lokales Routing die sekundäre Verbindung nur nach einem Failover verwendet.
-   Die durch Azure Site Recovery geschützten virtuellen Quell- und Zielnetzwerke für virtuelle Computer können beim Failover je nach Anforderung die gleichen oder unterschiedliche IP-Adressen haben. In beiden Fällen können die sekundären Verbindungen vor dem Failover eingerichtet werden.

### <a name="two-expressroute-circuits-in-the-same-expressroute-peering-location"></a>Zwei ExpressRoute-Verbindungen am gleichen ExpressRoute-Peering-Standort
-   Mit dieser Konfiguration können Sie einem Ausfall der primären ExpressRoute-Verbindung vorbeugen. Sie hilft allerdings nicht bei umfangreichen regionalen Notfällen, die die ExpressRoute-Peering-Standorte beeinträchtigen können. In diesem Fall wären sowohl die primären als auch die sekundären Verbindungen betroffen.
-   Für die IP-Adressen und Verbindungen gelten weiterhin die gleichen Bedingungen wie in der zuvor beschriebenen Situation. Sie können gleichzeitig Verbindungen zwischen dem lokalen Datencenter und dem virtuellen Quellnetzwerk über die primären Verbindung und mit dem virtuellen Zielnetzwerk über die sekundäre Verbindung nutzen. Mit gleichzeitigen Verbindungen zu sowohl primären virtuellen Netzwerken als auch virtuellen Netzwerken der Zielregion stellen Sie sicher, dass Ihr lokales Routing die sekundäre Verbindung nur nach einem Failover verwendet.
-   Wenn die beiden Verbindungen am gleichen Peering-Standort erstellt wurden, lassen sie sich nicht mit dem gleichen virtuellen Netzwerk verknüpfen.

### <a name="single-expressroute-circuit"></a>Einzelne ExpressRoute-Verbindung
-   Diese Konfiguration ist keine Lösung für einen umfangreichen regionalen Notfall, der den ExpressRoute-Peering-Standort beeinträchtigen könnte.
-   Mit einer einzigen ExpressRoute-Verbindung können Sie virtuelle Quell- und Zielnetzwerke nicht gleichzeitig verbinden, wenn in der Zielregion der gleiche IP-Adressraum verwendet wird.
-   Bei Verwendung des gleichen IP-Adressbereichs für die Zielregion sollte die Verbindung mit der Datenquelle zunächst getrennt und die Zielverbindung erst im Anschluss eingerichtet werden. Diese Verbindungsänderung kann als Teil eines Wiederherstellungsplans geskriptet werden.
-   Wenn bei einem regionalen Ausfall kein Zugriff auf die primäre Region möglich ist, kann der Trennvorgang fehlschlagen. Ein solcher Ausfall könnte sich negativ auf die Erstellung der Verbindung zur Zielregion auswirken, wenn derselbe IP-Adressraum im virtuellen Zielnetzwerk verwendet wird.
-   Wenn die Verbindung zur Zielregion erfolgreich erstellt wurde und die primäre Region danach wiederhergestellt ist, kann es zu Paketverlusten kommen, da zwei gleichzeitige Verbindungen versuchen, auf den gleichen Adressraum zuzugreifen. Um solche Paketverluste zu vermeiden, sollte die primäre Verbindung sofort beendet werden. Nach einem Failback von virtuellen Computern in der primären Region kann die primäre Verbindung nach dem Trennen der sekundären Verbindung erneut hergestellt werden.
-   Wenn im virtuellen Zielnetzwerk unterschiedlicher Adressraum verwendet wird, lassen sich von der gleichen ExpressRoute-Verbindung gleichzeitig Verbindungen mit den virtuellen Quell- und Zielnetzwerken erstellen.

## <a name="recovering-azure-deployments"></a>Wiederherstellen von Azure-Bereitstellungen
Betrachten wir das Failovermodell mit zwei verschiedenen ExpressRoute-Verbindungen an zwei unterschiedlichen Peering-Standorten, wobei für die geschützten virtuellen Azure-Computer private IP-Adressen beibehalten werden. Die Zielregion für die Wiederherstellung ist Azure Südostasien, und eine sekundäre ExpressRoute-Verbindung wird über einen Partner-Edge in Singapur hergestellt.

Um die Wiederherstellung der gesamten Bereitstellung zu automatisieren und zusätzlich virtuelle Computer und virtuelle Netzwerke zu replizieren, müssen noch andere relevante Netzwerkressourcen und Verbindungen erstellt werden. Bei der oben beschriebenen Hub- und Spoke-Topologie müssen Sie die folgenden zusätzlichen Schritte während oder nach dem [Failover](azure-to-azure-tutorial-failover-failback.md)-Vorgang ausführen:
1.  Erstellen Sie das Azure ExpressRoute-Gateway im virtuellen Hubnetzwerk der Zielregion. Das ExpressRoute-Gateway ist erforderlich, um das virtuelle Hubnetzwerk der Zielregion mit der ExpressRoute-Verbindung zu verbinden.
2.  Erstellen Sie die Verbindung des virtuellen Netzwerks aus dem Zielhub zur ExpressRoute-Zielverbindung.
3.  Richten Sie die VNet-Peerings zwischen den virtuellen Hubnetzwerken der Zielregion und den virtuellen Spoke-Netzwerken ein. Die Peering-Eigenschaften in der Zielregion sind identisch mit denen in der Quellregion.
4.  Richten Sie die UDRs im VNet-Hub und die beiden Spoke-VNets ein. Die Eigenschaften der zielseitigen UDRs sind identisch mit denen auf der Quellseite, wenn die gleichen IP-Adressen verwendet werden. Bei unterschiedlichen IP-Adressen sollten die UDRs entsprechend geändert werden.

Erstellen Sie ein Skript für diese Verbindungsänderung als Teil eines [Wiederherstellungsplans](site-recovery-create-recovery-plans.md). Je nach Anwendungskonnektivität und Zeitaufwand für die Wiederherstellung können die oben genannten Schritte auch vor dem Start des Failovers abgeschlossen werden.

Nach der Wiederherstellung der virtuellen Computer und der abschließenden Ausführung der anderen Schritte zur Konnektivität stellt sich die Wiederherstellungsumgebung wie folgt dar: ![Konnektivität zwischen lokalem Datencenter und Azure mit ExpressRoute nach einem Failover](./media/azure-vm-disaster-recovery-with-expressroute/site-recovery-with-expressroute-after-failover.png)

Einzelheiten für ein Beispiel einer einfachen Topologie zur Notfallwiederherstellung für Azure-VM mit einer einzelnen ExpressRoute-Verbindung und derselben IP-Adresse auf virtuellen Zielcomputern finden Sie [hier](site-recovery-retain-ip-azure-vm-failover.md#on-premises-to-azure-connectivity).

## <a name="recovery-time-objective-rto-considerations"></a>Überlegungen zur Recovery Time Objective (RTO)
Um die Wiederherstellungszeit für Ihre Bereitstellung insgesamt zu reduzieren, wird eine Bereitstellung der zusätzlichen [Netzwerkkomponenten](azure-vm-disaster-recovery-with-expressroute.md#enabling-replication-for-the-deployment) der Zielregion, z.B. Gateways des virtuellen Netzwerks, im Voraus empfohlen. Mit der Bereitstellung zusätzlicher Ressourcen ist eine kurze Ausfallzeit verbunden, die sich auf die gesamte Wiederherstellungszeit auswirken kann, wenn sie bei der Planung unberücksichtigt bleibt.

Es wird empfohlen, regelmäßig [Notfallwiederherstellungstests](azure-to-azure-tutorial-dr-drill.md) für geschützte Bereitstellungen auszuführen. Bei einer solchen Übung wird Ihre Replikationsstrategie ohne Datenverlust oder Ausfallzeiten überprüft. Ihre Produktionsumgebung bleibt davon unbeeinträchtigt. Ein Test vermeidet außerdem Konfigurationsprobleme in letzter Minute, die sich negativ auf die RTO-Zielvorgabe auswirken können. Es wird empfohlen, ein separates Azure-VM-Netzwerk für das Testfailover zu nutzen und nicht das Standardnetzwerk, das bei der Aktivierung der Replikation eingerichtet wurde.

Wenn Sie eine einzelne ExpressRoute-Verbindung verwenden, wird empfohlen, mit einem anderen IP-Adressraum für das virtuelle Zielnetzwerk zu arbeiten, um Probleme bei der Einrichtung einer Verbindung während regionaler Notfälle zu vermeiden. Wenn eine Verwendung verschiedener IP-Adressen in der wiederhergestellten Produktionsumgebung nicht möglich ist, sollte das Testfailover für die Notfallwiederherstellung auf einem separaten Testnetzwerk mit unterschiedlichen IP-Adressen erfolgen. Der Grund dafür ist, dass sich zwei virtuelle Netzwerke mit doppeltem IP-Adressraum nicht mit der gleichen ExpressRoute-Verbindung verbinden lassen.

## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zu [ExpressRoute-Verbindungen](../expressroute/expressroute-circuit-peerings.md)
- Weitere Informationen zu [ExpressRoute-Routingdomänen](../expressroute/expressroute-circuit-peerings.md#expressroute-routing-domains)
- Weitere Informationen zu [ExpressRoute-Standorten](../expressroute/expressroute-locations.md)
- Informieren Sie sich ausführlicher über [Wiederherstellungspläne](site-recovery-create-recovery-plans.md) zum Automatisieren des Anwendungsfailovers.
