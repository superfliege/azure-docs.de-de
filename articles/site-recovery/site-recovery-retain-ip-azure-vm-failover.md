---
title: "Beibehalten von IP-Adressen beim Durchführen eines Failovers von virtuellen Azure-Computern in eine andere Azure-Region | Microsoft-Dokumentation"
description: "Es wird beschrieben, wie Sie IP-Adressen für Failoverszenarien von Azure zu Azure mit Azure Site Recovery beibehalten."
services: site-recovery
documentationcenter: 
author: mayanknayar
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2018
=======
<<<<<<< HEAD
ms.date: 02/27/2018
ms.author: manayar
ms.openlocfilehash: 28d772df384e620c7e82812adfa2bfa148401132
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
<<<<<<< HEAD
=======
ms.date: 02/06/2018
>>>>>>> bb0780f466c4ede2eb00246e1afe01b19bb40688
ms.author: manayar
ms.openlocfilehash: 28d772df384e620c7e82812adfa2bfa148401132
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: HT
ms.contentlocale: de-DE
<<<<<<< HEAD
ms.lasthandoff: 02/28/2018
=======
ms.lasthandoff: 02/09/2018
>>>>>>> 6def4612c80a1e9bab4008c57d68ccdbec8d0794
>>>>>>> bb0780f466c4ede2eb00246e1afe01b19bb40688
=======
>>>>>>> badf78d3a696bb4bc2277e8b2840da1a5bb1178d
---
# <a name="ip-address-retention-for-azure-virtual-machine-failover"></a>Beibehaltung von IP-Adressen für das Failover von virtuellen Azure-Computern

Azure Site Recovery ermöglicht die Notfallwiederherstellung für Azure-VMs. Bei einem Failover von einer Azure-Region in eine andere ist es für Kunden häufig erforderlich, die IP-Konfigurationen beizubehalten. Standardmäßig imitiert Site Recovery die Struktur des virtuellen Quellnetzwerks und des Subnetzes, wenn diese Ressourcen in der Zielregion erstellt werden. Für Azure-VMs, die mit statischen privaten IP-Adressen konfiguriert sind, stellt Site Recovery nach Möglichkeit außerdem die gleiche private IP-Adresse auf der Ziel-VM bereit, wenn diese IP nicht von einer Azure-Ressource oder einer replizierten VM blockiert wird.

Für einfache Anwendungen ist nur die obige Standardkonfiguration erforderlich. Für komplexere Unternehmensanwendungen müssen Kunden ggf. zusätzliche Netzwerkressourcen bereitstellen, um die Konnektivität nach dem Failover mit anderen Komponenten der Infrastruktur sicherzustellen. In diesem Artikel werden die Netzwerkanforderungen für das Failover von Azure-VMs von einer Region in eine andere mit Beibehaltung der VM-IP-Adressen beschrieben.

## <a name="azure-to-azure-connectivity"></a>Konnektivität von Azure zu Azure

Für das erste Szenario sehen wir uns **Unternehmen A** an, für das die gesamte Infrastruktur in Azure ausgeführt wird. Aus Gründen der Geschäftskontinuität und Konformität entscheidet sich **Unternehmen A**, Azure Site Recovery zum Schützen der Anwendungen zu verwenden.

Da die Anforderung zur IP-Beibehaltung besteht (z.B. für Anwendungsbindungen), verfügt Unternehmen A in der Zielregion in Bezug auf die Struktur über das gleiche virtuelle Netzwerk und Subnetz. Zur weiteren Reduzierung des RTO-Werts (Recovery Time Objective) nutzt **Unternehmen A** Replikatknoten für SQL Always On, Domänencontroller usw., und diese Replikatknoten werden in der Zielregion in einem anderen virtuellen Netzwerk angeordnet. Aufgrund der Verwendung eines anderen Adressraums für die Replikatknoten kann **Unternehmen A** eine Standort-zu-Standort-VPN-Konnektivität zwischen der Quell- und Zielregion erzielen. Dies wäre andernfalls nicht möglich, wenn auf beiden Seiten der gleiche Adressraum verwendet wird.

Vor dem Failover sieht die Netzwerkarchitektur wie folgt aus:
- Anwendungs-VMs werden in der Azure-Region „Asien, Osten“ gehostet, und es wird ein virtuelles Azure-Netzwerk mit dem Adressraum 10.1.0.0/16 verwendet. Dieses virtuelle Netzwerk hat den Namen **Quell-VNet**.
- Die Anwendungsworkloads sind auf die drei Subnetze 10.1.0.0/24, 10.1.1.0/24 und 10.1.2.0/24 verteilt, die als **Subnetz 1**, **Subnetz 2** und **Subnetz 3** bezeichnet werden.
- Die Azure-Zielregion ist „Asien, Südosten“. Sie verfügt über ein virtuelles Wiederherstellungsnetzwerk, mit dem der Adressraum und die Subnetzkonfiguration auf der Quelle imitiert werden. Dieses virtuelle Netzwerk hat den Namen **Wiederherstellungs-VNet**.
- Replikatknoten, z.B. für Always On, Domänencontroller usw., sind in einem virtuellen Netzwerk mit dem Adressraum 20.1.0.0/16 in Subnet 4 mit der Adresse 20.1.0.0/24 angeordnet. Das virtuelle Netzwerk hat den Namen **Azure VNet** und befindet sich in der Azure-Region „Asien, Südosten“.
- **Quell-VNet** und **Azure VNet** sind per Standort-zu-Standort-VPN-Konnektivität verbunden.
- Das **Wiederherstellungs-VNet** ist nicht mit einem anderen virtuellen Netzwerk verbunden.
- **Unternehmen A** weist die IP-Zieladresse für replizierte Elemente zu bzw. überprüft sie. In diesem Beispiel entspricht die Ziel-IP für jede VM jeweils der Quell-IP.

![Konnektivität von Azure zu Azure vor dem Failover](./media/site-recovery-retain-ip-azure-vm-failover/azure-to-azure-connectivity-before-failover.png)

### <a name="full-region-failover"></a>Failover der gesamten Region

Bei einem regionalen Ausfall kann **Unternehmen A** seine gesamte Bereitstellung schnell und einfach wiederherstellen, indem die leistungsfähigen [Wiederherstellungspläne](site-recovery-create-recovery-plans.md) von Azure Site Recovery verwendet werden. Da **Unternehmen A** bereits für jede VM vor dem Failover die IP-Zieladresse festgelegt hat, kann es das Failover zwischen dem Wiederherstellungs-VNet und dem Azure VNet orchestrieren und die Verbindungsherstellung automatisieren. Dies ist in der Abbildung unten dargestellt.

![Konnektivität von Azure zu Azure – Failover der gesamten Region](./media/site-recovery-retain-ip-azure-vm-failover/azure-to-azure-connectivity-full-region-failover.png)

Je nach den Anwendungsanforderungen können Verbindungen zwischen den beiden VNets in der Zielregion vor, während (als Zwischenschritt) oder nach dem Failover hergestellt werden. Verwenden Sie [Wiederherstellungspläne](site-recovery-create-recovery-plans.md), um Skripts hinzuzufügen und die Failoverreihenfolge zu definieren.

Unternehmen A kann wählen, ob es das VNet-Peering oder die Standort-zu-Standort-VPN-Verbindung verwendet, um die Konnektivität zwischen dem Wiederherstellungs-VNet und dem Azure VNet herzustellen. Beim VNet-Peering wird kein VPN-Gateway verwendet, und es gelten andere Einschränkungen. Außerdem werden die [Preise für VNet-Peering](https://azure.microsoft.com/pricing/details/virtual-network) anders berechnet als die [Preise für VPN Gateway (VNet-zu-VNet)](https://azure.microsoft.com/pricing/details/vpn-gateway). Für Failover ist es generell ratsam, die Quellkonnektivität, einschließlich des Verbindungstyps, zu imitieren. So wird die Wahrscheinlichkeit verringert, dass aus Netzwerkänderungen unvorhersehbare Vorfälle entstehen.

### <a name="isolated-application-failover"></a>Failover einer isolierten Anwendung

Unter bestimmten Umständen müssen Benutzer ggf. für Teile ihrer Anwendungsinfrastruktur ein Failover durchführen. Ein Beispiel hierfür ist das Failover einer bestimmten Anwendung oder Ebene, die sich in einem dedizierten Subnetz befindet. Ein Subnetzfailover mit IP-Beibehaltung ist zwar möglich, aber für die meisten Situationen nicht ratsam, da dies zu einer erheblichen Zunahme von Konnektivitätsinkonsistenzen führt. Sie verlieren zusätzlich die Subnetzkonnektivität mit anderen Subnetzen in demselben virtuellen Azure-Netzwerk.

Eine bessere Möglichkeit zur Erfüllung der Anforderungen eines Anwendungsfailovers auf Subnetzebene besteht darin, unterschiedliche IP-Zieladressen für das Failover zu verwenden (falls Konnektivität mit anderen Subnetzen im virtuellen Quellnetzwerk erforderlich ist) oder jede Anwendung auf der Quelle in einem eigenen dedizierten virtuellen Netzwerk zu isolieren. Mit dem letzteren Ansatz können Sie die Konnektivität zwischen Netzwerken auf der Quelle einrichten und dies emulieren, wenn ein Failover in die Zielregion durchgeführt wird.

Zum Erstellen der Architektur von einzelnen Anwendungen im Hinblick auf Resilienz ist es ratsam, eine Anwendung in einem eigenen dedizierten virtuellen Netzwerk anzuordnen und die Konnektivität zwischen diesen virtuellen Netzwerken je nach Bedarf herzustellen. Auf diese Weise wird das Failover einer isolierten Anwendung ermöglicht, während die ursprünglichen privaten IP-Adressen beibehalten werden.

Die Konfiguration vor dem Failover sieht dann wie folgt aus:
- Die Anwendungs-VMs werden in der Azure-Region „Asien, Osten“ gehostet, und es wird ein virtuelles Azure-Netzwerk mit dem Adressraum 10.1.0.0/16 für die erste Anwendung und 15.1.0.0/16 für die zweite Anwendung verwendet. Die virtuellen Netzwerke haben die Namen **Quell-VNet1** und **Quell-VNet2** für die erste bzw. zweite Anwendung.
- Jedes VNet ist zudem jeweils in zwei Subnetze unterteilt.
- Die Azure-Zielregion ist „Asien, Südosten“ mit den virtuellen Wiederherstellungsnetzwerken „Wiederherstellungs-VNet1“ und „Wiederherstellungs-VNet2“.
- Replikatknoten, z.B. für Always On, Domänencontroller usw., sind in einem virtuellen Netzwerk mit dem Adressraum 20.1.0.0/16 in **Subnetz 4** mit der Adresse 20.1.0.0/24 angeordnet. Das virtuelle Netzwerk hat den Namen „Azure VNet“ und befindet sich in der Azure-Region „Asien, Südosten“.
- **Quell-VNet1** und **Azure VNet** sind per Standort-zu-Standort-VPN-Konnektivität verbunden. **Quell-VNet2** und **Azure VNet** sind ebenfalls per Standort-zu-Standort-VPN-Konnektivität verbunden.
- **Quell-VNet1** und **Quell-VNet2** sind in diesem Beispiel auch per S2S-VPN-Verbindung verbunden. Da sich die beiden VNets in derselben Region befinden, kann anstelle der S2S-VPN-Verbindung auch das VNet-Peering verwendet werden.
- **Wiederherstellungs-VNet1** und **Wiederherstellungs-VNet2** sind nicht mit einem anderen virtuellen Netzwerk verbunden.
- Zur Reduzierung des RTO-Werts (Recovery Time Objective) werden vor dem Failover VPN-Gateways für **Wiederherstellungs-VNet1** und **Wiederherstellungs-VNet2** konfiguriert.

![Konnektivität von Azure zu Azure – Isolierte Anwendung vor dem Failover](./media/site-recovery-retain-ip-azure-vm-failover/azure-to-azure-connectivity-isolated-application-before-failover.png)

In einem Notfall, der nur eine Anwendung betrifft (in diesem Beispiel in Quell-VNet2), kann Unternehmen A die Anwendung wie folgt wiederherstellen:
- VPN-Verbindungen zwischen **Quell-VNet1** und **Quell-VNet2** und zwischen **Quell-VNet2** und **Azure VNet** werden getrennt.
- VPN-Verbindungen werden zwischen **Quell-VNet1** und **Wiederherstellungs-VNet2** und zwischen **Wiederherstellungs-VNet2** und **Azure VNet** hergestellt.
- Für VMs aus **Quell-VNet2** wird ein Failover in **Wiederherstellungs-VNet2** durchgeführt.

![Konnektivität von Azure zu Azure – Isolierte Anwendung nach dem Failover](./media/site-recovery-retain-ip-azure-vm-failover/azure-to-azure-connectivity-isolated-application-after-failover.png)

Das obige Beispiel für ein isoliertes Failover kann auf mehr Anwendungen und Netzwerkverbindungen erweitert werden. Die Empfehlung besteht darin, bei Failovern von der Quelle zum Ziel, falls möglich, ein „Like-Like“-Verbindungsmodell zu verwenden.

### <a name="further-considerations"></a>Weitere Aspekte

Für VPN-Gateways werden öffentliche IP-Adressen und Gatewayhops verwendet, um Verbindungen herzustellen. Falls Sie keine öffentlichen IP-Adressen verwenden bzw. zusätzliche Hops vermeiden möchten, können Sie jetzt das globale VNet-Peering verwenden, um virtuelle Netzwerke über Azure-Regionen hinweg zu verknüpfen.

Dieses Feature befindet sich derzeit in der öffentlichen Vorschauphase und wird auf weitere Supportregionen erweitert, um die direkte Konnektivität von VM zu VM ohne Einbeziehung des öffentlichen Internets oder zusätzlicher Hops zu ermöglichen.

Weitere Informationen finden Sie in der [Dokumentation zum Peering](../virtual-network/virtual-network-create-peering.md#register) und unter [Preise](https://azure.microsoft.com/en-us/pricing/details/virtual-network/).

## <a name="on-premises-to-azure-connectivity"></a>Konnektivität vom lokalen Standort mit Azure

Für das zweite Szenario sehen wir uns **Unternehmen B** an, bei dem ein Teil der Anwendungsinfrastruktur in Azure und der Rest lokal ausgeführt wird. Aus Gründen der Geschäftskontinuität und Konformität entscheidet sich **Unternehmen B**, Azure Site Recovery zum Schützen der in Azure ausgeführten Anwendungen zu verwenden.

Vor dem Failover sieht die Netzwerkarchitektur wie folgt aus:
- Anwendungs-VMs werden in der Azure-Region „Asien, Osten“ gehostet, und es wird ein virtuelles Azure-Netzwerk mit dem Adressraum 10.1.0.0/16 verwendet. Dieses virtuelle Netzwerk hat den Namen **Quell-VNet**.
- Die Anwendungsworkloads sind auf die drei Subnetze 10.1.0.0/24, 10.1.1.0/24 und 10.1.2.0/24 verteilt, die als **Subnetz 1**, **Subnetz 2** und **Subnetz 3** bezeichnet werden.
- Die Azure-Zielregion ist „Asien, Südosten“. Sie verfügt über ein virtuelles Wiederherstellungsnetzwerk, mit dem der Adressraum und die Subnetzkonfiguration auf der Quelle imitiert werden. Dieses virtuelle Netzwerk hat den Namen **Wiederherstellungs-VNet**.
- Für VMs in der Azure-Region „Asien, Osten“ wird per ExpressRoute oder Standort-zu-Standort-VPN eine Verbindung mit dem lokalen Rechenzentrum hergestellt.
- Zur Reduzierung des RTO-Werts (Recovery Time Objective) stellt Unternehmen B vor dem Failover Gateways im Wiederherstellungs-VNet in der Azure-Region „Asien, Südosten“ bereit.
- **Unternehmen B** weist die IP-Zieladresse für replizierte Elemente zu bzw. überprüft sie. In diesem Beispiel entspricht die Ziel-IP für jede VM jeweils der Quell-IP.

![Konnektivität vom lokalen Standort zu Azure vor dem Failover](./media/site-recovery-retain-ip-azure-vm-failover/on-premises-to-azure-connectivity-before-failover.png)

### <a name="full-region-failover"></a>Failover der gesamten Region

Bei einem regionalen Ausfall kann **Unternehmen B** seine gesamte Bereitstellung schnell und einfach wiederherstellen, indem die leistungsfähigen [Wiederherstellungspläne](site-recovery-create-recovery-plans.md) von Azure Site Recovery verwendet werden. Da **Unternehmen B** bereits für jede VM vor dem Failover die IP-Zieladresse festgelegt hat, kann es das Failover zwischen dem Wiederherstellungs-VNet und dem lokalen Rechenzentrum orchestrieren und die Verbindungsherstellung automatisieren. Dies ist in der Abbildung unten dargestellt.

Die ursprüngliche Verbindung zwischen der Azure-Region „Asien, Osten“ und dem lokalen Rechenzentrum sollte getrennt werden, bevor die Verbindung zwischen der Azure-Region „Asien, Südosten“ und dem lokalen Rechenzentrum hergestellt wird. Das lokale Routing wird ebenfalls neu konfiguriert, um nach dem Failover auf die Zielregion und die Gateways zu verweisen.

![Konnektivität vom lokalen Standort zu Azure nach dem Failover](./media/site-recovery-retain-ip-azure-vm-failover/on-premises-to-azure-connectivity-after-failover.png)

### <a name="subnet-failover"></a>Subnetzfailover

Im Gegensatz zum Szenario „Azure zu Azure“, das für **Unternehmen A** beschrieben wurde, ist ein Failover auf Subnetzebene in diesem Fall für **Unternehmen B** nicht möglich. Der Grund ist, dass der Adressraum auf der Quelle und in den virtuellen Wiederherstellungsnetzwerken identisch ist und die Verbindung von der ursprünglichen Quelle zum lokalen Standort aktiv ist.

Zur Erzielung von Anwendungsresilienz wird empfohlen, jede Anwendung in einem eigenen dedizierten Azure VNet anzuordnen. Für Anwendungen können dann isolierte Failover durchgeführt werden, und die erforderlichen Verbindungen vom lokalen Standort mit der Quelle können wie oben beschrieben an die Zielregion geleitet werden.

## <a name="next-steps"></a>Nächste Schritte
- Informieren Sie sich weiter über [Wiederherstellungspläne](site-recovery-create-recovery-plans.md).
