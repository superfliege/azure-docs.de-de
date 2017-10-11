---
title: Azure Deutschland Netzwerkdienste | Microsoft Docs
description: "Bietet einen Vergleich der Features und Richtlinien für private Verbindungen auf Azure-Deutschland"
services: germany
cloud: na
documentationcenter: na
author: gitralf
manager: rainerst
ms.assetid: na
ms.service: germany
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/13/2017
ms.author: ralfwi
ms.openlocfilehash: dca424bd7f894ae3d3e343fde7fc13469ab62729
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-germany-networking-services"></a>Azure Deutschland Netzwerkdienste
## <a name="expressroute-private-connectivity"></a>ExpressRoute (private Verbindungen)
Azure ExpressRoute ist im Allgemeinen in Azure Deutschland verfügbar. Weitere Informationen (einschließlich von Partnern und Speicherorte peering) finden Sie unter der [globale ExpressRoute-Dokumentation](../expressroute/index.md).

### <a name="variations"></a>Variationen

* Kunden von Azure Deutschland Verbinden mit einer physisch isoliertes Kapazität über eine dedizierte Azure Deutschland ExpressRoute-Verbindung.
* Azure Deutschland bietet eine erhöhte Verfügbarkeit und Dauerhaftigkeit mit mehrere Paare von Region befindet sich mindestens 400 km auseinander. 
* Standardmäßig alle Azure Deutschland ExpressRoute-Verbindung als aktiv / aktiv-redundante mit Unterstützung für die Erweiterung konfiguriert ist, und bietet bis zu 10G Circuit Kapazität.
* Azure ExpressRoute Deutschland für Ortsangaben bereitstellen optimierte Pfade zur Verfügung (einschließlich kürzeste Hops, niedrige Latenz und hoher Leistung) für Kunden und geografisch redundanten Deutschland Azure-Regionen.
* Die private Azure Deutschland ExpressRoute-Verbindung nicht, durchlaufen, oder richten sich nach dem Internet.
* Die physische und logische Infrastruktur von Azure Deutschland wird physisch dediziert und aus dem internationalen Microsoft Cloud-Netzwerk getrennt.
* Azure ExpressRoute für Deutschland enthält private Verbindungen in Microsoft Azure Cloud Services, aber nicht auf Office 365 oder Dynamics 365 Clouddienste.

### <a name="considerations"></a>Weitere Überlegungen
Zwei grundlegende Dienste stellen Sie die VPN-Konnektivität Azure Deutschland: ExpressRoute und ein VPN (Site-to-Site für eine typische Organisation).

Sie können ExpressRoute zum Erstellen privater Verbindungen zwischen Azure-Deutschland-Datencentern und Ihrer lokalen Infrastruktur oder in einer Colocation-Umgebung verwenden. ExpressRoute-Verbindungen gehen Sie nicht über das öffentliche Internet. Sie bieten mehr Zuverlässigkeit, schnellere Geschwindigkeiten und geringere Wartezeiten als normale Internet-Verbindungen. In einigen Fällen verwenden ExpressRoute-Verbindungen zum Übertragen von Daten zwischen lokalen und Azure ergibt, die erhebliche Kosten Vorteile.   

Mit ExpressRoute können einrichten Sie Verbindungen mit Azure an ExpressRoute-Standorten, z. B. eine ExpressRoute-Exchange-Anbieterstandort. Oder Sie direkt eine Verbindung herstellen mit Azure aus Ihrem vorhandenen WAN wie z. B. ein Multiprotokoll bezeichnen switching (MPLS)-VPN, die von einem Netzwerkdienstanbieter bereitgestellt wird.

Netzwerkdienste, Lösungen und Azure Deutschland kundenanwendungen zu unterstützen wird dringend empfohlen, dass Sie ExpressRoute (private Connectivity) zur Verbindung mit Azure Deutschland implementieren. Wenn Sie VPN-Verbindungen verwenden, beachten Sie Folgendes:

* Wenden Sie sich an Ihre autorisieren offiziellen/Versender zu bestimmen, ob Sie private Verbindungen oder einen anderen Mechanismus für die sichere Verbindung benötigen und keine zusätzlichen Einschränkungen zu erkennen.
* Entscheiden Sie, ob festlegen, dass der Standort-zu-Standort-VPN über eine private Verbindungen Zone weitergeleitet wird.
* Rufen Sie eine MPLS-Verbindung oder ein VPN mit einer lizenzierten privaten Zugriff Verbindungsanbieter an.

Wenn Sie eine private Verbindungen-Architektur verwenden, überprüfen Sie, dass eine geeignete Implementierung eingerichtet und werden, für die Verbindung zum Gateway Netzwerk/Internet verwaltet (GN / I) edge Router Abgrenzung Punkt für Azure-Deutschland. Auf ähnliche Weise muss Ihre Organisation-Netzwerkkonnektivität zwischen Ihrer lokalen Umgebung und Gateway Netzwerk/Kundennummer (GN/C) Edge Router Abgrenzung Punkt für Deutschland Azure herstellen.

Wenn Sie mit Microsoft über ExpressRoute auf jedem peering Speicherort in Azure (Deutschland)-Region herstellen, müssen Sie den Zugriff auf alle Microsoft Azure-Cloud-Dienste in allen Regionen innerhalb der Begrenzung des Deutsch. Z. B. Wenn Sie an Microsoft in Berlin über ExpressRoute eine Verbindung herstellen, haben Sie Zugriff auf alle Microsoft-Cloud-Dienste, die in Deutschland von Azure gehostet.

Ausführliche Informationen zu Speicherorten Partnern und eine detaillierte Liste der ExpressRoute für Azure Deutschland peering Speicherorte, finden Sie unter der **Übersicht über** Registerkarte die [globale ExpressRoute-Dokumentation](../expressroute/index.md).

Sie können mehrere ExpressRoute-Leitung erwerben. Mehrere Verbindungen bietet erhebliche Vorteile für hohe Verfügbarkeit aufgrund Geo-Redundanz. In Fällen, in denen mehrere ExpressRoute-Verbindungen haben, erhalten Sie den gleichen Satz von Präfixen, die von Microsoft auf die public peering Pfade angekündigt. Dies bedeutet, dass mehrere Pfade zwischen Ihrem Netzwerk müssen Sie die Microsoft. Diese Situation kann dazu führen, nicht optimalen Routingentscheidungen in Ihrem Netzwerk vorgenommen werden. Daher möglicherweise nicht optimal Konnektivität Erfahrungen mit anderen Diensten angezeigt. Weitere Informationen finden Sie unter der **Einstieg in die** Registerkarte der [globale ExpressRoute-Dokumentation](../expressroute/index.md) , und wählen Sie **optimieren routing**.

## <a name="support-for-load-balancer"></a>Unterstützung für den Lastenausgleich
Azure-Lastenausgleich ist in der Regel in Azure Deutschland verfügbar. Weitere Informationen finden Sie unter der [Dokumentation für den Load Balancer globalen](../load-balancer/load-balancer-overview.md). 

## <a name="support-for-traffic-manager"></a>Unterstützung für Traffic Manager
Azure Traffic Manager steht in der Regel in Azure Deutschland. Weitere Informationen finden Sie unter der [global Traffic Manager-Dokumentation](../traffic-manager/traffic-manager-overview.md). 

## <a name="support-for-virtual-network-peering"></a>Unterstützung für das virtuelle Netzwerk peering 
Virtuelles Netzwerk peering ist im Allgemeinen in Azure Deutschland verfügbar. Weitere Informationen finden Sie unter der [virtuellen Netzwerk peering Dokumentation für den globalen](../virtual-network/virtual-network-peering-overview.md). 

## <a name="support-for-vpn-gateway"></a>Unterstützung für VPN-Gateway 
Azure-VPN-Gateway ist im Allgemeinen in Azure Deutschland verfügbar. Weitere Informationen finden Sie unter der [Dokumentation für VPN-Gateway den globalen](../vpn-gateway/vpn-gateway-about-vpngateways.md). 

## <a name="next-steps"></a>Nächste Schritte
Zusätzliche Informationen und Updates, abonnieren Sie die [Deutschland Azure Blog](https://blogs.msdn.microsoft.com/azuregermany/).
