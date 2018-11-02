---
title: 'Interoperabilität von ExpressRoute, Site-to-Site-VPN und VNET-Peering – Analyse der Steuerungsebene: Interoperabilität der Azure-Back-End-Konnektivitätsfeatures | Microsoft-Dokumentation'
description: Auf dieser Seite wird die Steuerungsebenenanalyse der Testeinrichtung beschrieben, die erstellt wird, um die Interoperabilität der Features ExpressRoute, Site-to-Site-VPN und VNET-Peering zu analysieren.
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: expressroute,vpn-gateway,virtual-network
ms.topic: article
ms.workload: infrastructure-services
ms.date: 10/18/2018
ms.author: rambala
ms.openlocfilehash: ee887da18b5666e61bc25365791b2e7dffb925e0
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2018
ms.locfileid: "49947119"
---
# <a name="interoperability-of-expressroute-site-to-site-vpn-and-vnet-peering---control-plane-analysis"></a>Interoperabilität von ExpressRoute, Site-to-Site-VPN und VNET-Peering – Analyse der Steuerungsebene

In diesem Artikel wird die Steuerungsebenenanalyse der Testeinrichtung beschrieben. Informationen zur Testeinrichtung finden Sie bei Bedarf unter [Testeinrichtung][Setup]. Die Details zur Testeinrichtungskonfiguration finden Sie unter [Testeinrichtungskonfiguration][Configuration].

Bei der Analyse der Steuerungsebene werden im Wesentlichen Routen untersucht, die zwischen Netzwerken einer Topologie ausgetauscht werden. Mit der Analyse der Steuerungsebene kann verdeutlicht werden, wie die Topologie von unterschiedlichen Netzwerken betrachtet wird.

##<a name="hub-and-spoke-vnet-perspective"></a>Hub-and-Spoke-VNET-Perspektive

Im folgenden Diagramm ist das Netzwerk aus Hub-and-Spoke-VNET-Perspektive (blau hervorgehoben) dargestellt. Darüber hinaus sind im Diagramm auch die Autonomous System Number (ASN) der unterschiedlichen Netzwerke und die zwischen den Netzwerken ausgetauschten Routen dargestellt. 

[![1]][1]

Beachten Sie, dass sich die ASN für das ExpressRoute-Gateway des VNET von der ASN für Microsoft Enterprise Edge-Router (MSEEs) unterscheidet. Für das ExpressRoute-Gateway wird eine private ASN (65515) und für MSEE-Einheiten global eine öffentliche ASN (12076) verwendet. Wenn Sie das ExpressRoute-Peering konfigurieren, weil die MSEE-Einheit als Peer fungiert, nutzen Sie 12076 als Peer-ASN. Auf Azure-Seite richtet die MSEE-Einheit das eBGP-Peering mit ExpressRoute-Gateway ein. Das duale eBGP-Peering, das die MSEE-Einheit für jedes ExpressRoute-Peering einrichtet, ist auf der Steuerungsebene transparent. Wenn eine ExpressRoute-Routingtabelle angezeigt wird, wird die ExpressRoute-Gateway-ASN des VNET für die VNET-Präfixe eingeblendet. Ein Screenshot mit einem Beispiel für eine ExpressRoute-Routingtabelle ist unten dargestellt: 

[![5]][5]

In Azure hat die ASN nur aus Peeringperspektive eine Bedeutung. Standardmäßig wird als ASN für das ExpressRoute-Gateway und das VPN-Gateway 65515 verwendet.

##<a name="on-premises-location-1-and-remote-vnet-perspective-via-expressroute-1"></a>Perspektive des lokalen Standorts 1 und des Remote-VNET über ExpressRoute 1

Der lokale Standort 1 und das Remote-VNET sind beide über ExpressRoute 1 mit dem Hub-VNET verbunden und verfügen deshalb über die gleiche Perspektive der Topologie. Dies ist unten im Diagramm dargestellt.

[![2]][2]

##<a name="on-premises-location-1-and-branch-vnet-perspective-via-site-to-site-vpn"></a>Perspektive des lokalen Standorts 1 und des Branch-VNET per Site-to-Site-VPN

Der lokale Standort 1 und das Branch-VNET sind beide über Site-to-Site-VPN-Verbindungen mit dem VPN-Gateway des Hub-VNET verbunden und verfügen deshalb über die gleiche Perspektive der Topologie. Dies ist unten im Diagramm dargestellt.

[![3]][3]

##<a name="on-premises-location-2-perspective"></a>Perspektive des lokalen Standorts 2

Der lokale Standort 2 ist über das private Peering von ExpressRoute 2 mit dem Hub-VNET verbunden. 

[![4]][4]

## <a name="further-reading"></a>Weitere nützliche Informationen

### <a name="using-expressroute-and-site-to-site-vpn-connectivity-in-tandem"></a>Gemeinsames Verwenden von ExpressRoute und Site-to-Site-VPN-Konnektivität

####  <a name="site-to-site-vpn-over-expressroute"></a>Site-to-Site-VPN über ExpressRoute

Site-to-Site-VPN kann mithilfe von ExpressRoute-/Microsoft-Peering konfiguriert werden, um Daten auf privatem Wege zwischen Ihrem lokalen Netzwerk und Ihren Azure VNETs auszutauschen und dabei Vertraulichkeit, Anti-Replay, Authentizität und Integrität sicherzustellen. Weitere Informationen zur Konfiguration eines Site-to-Site-IPSec-VPN im Tunnelmodus per ExpressRoute-/Microsoft-Peering finden Sie unter [Konfigurieren eines Site-to-Site-VPN über ExpressRoute-Microsoft-Peering][S2S-Over-ExR]. 

Die wichtigste Einschränkung bei der Konfiguration des S2S-VPN per Microsoft-Peering ist der Durchsatz. Der Durchsatz des IPSec-Tunnels wird durch die Kapazität des VPN-Gateways eingeschränkt. Der Durchsatz des VPN-Gateways ist im Vergleich zum ExpressRoute-Durchsatz niedriger. In Szenarien dieser Art stellt die Verwendung des IPSec-Tunnels für Datenverkehr mit hohem Schutzfaktor und des privaten Peerings für den gesamten anderen Datenverkehr einen Beitrag zur Optimierung der ExpressRoute-Bandbreitenauslastung dar.

#### <a name="site-to-site-vpn-as-a-secure-failover-path-for-expressroute"></a>Site-to-Site-VPN als sicherer Failoverpfad für ExpressRoute
Die ExpressRoute-Verbindung wird als redundantes Leitungspaar bereitgestellt, um die Hochverfügbarkeit sicherzustellen. Sie können die georedundante ExpressRoute-Konnektivität in unterschiedlichen Azure-Regionen konfigurieren. Wenn Sie wie in unserer Testeinrichtung in einer bestimmten Azure-Region einen Failoverpfad für Ihre ExpressRoute-Konnektivität einrichten möchten, können Sie hierfür ein Site-to-Site-VPN verwenden. Wenn über ExpressRoute und das Site-to-Site-VPN die gleichen Präfixe angekündigt werden, gibt Azure ExpressRoute den Vorzug vor dem Site-to-Site-VPN. Zur Vermeidung des asymmetrischen Routings zwischen ExpressRoute und dem Site-to-Site-VPN sollte in der lokalen Netzwerkkonfiguration ExpressRoute ebenfalls den Vorzug vor der Site-to-Site-VPN-Konnektivität erhalten.

Weitere Informationen zur Konfiguration von parallelen ExpressRoute- und Site-to-Site-VPN-Verbindungen finden Sie unter [Konfigurieren von parallel bestehenden ExpressRoute- und Standort-zu-Standort-Verbindungen mithilfe von PowerShell][ExR-S2S-CoEx].

### <a name="extending-backend-connectivity-to-spoke-vnets-and-branch-locations"></a>Erweitern der Back-End-Konnektivität auf Spoke-VNETs und Branchstandorte

#### <a name="spoke-vnet-connectivity-using-vnet-peering"></a>Spoke-VNET-Konnektivität per VNET-Peering

Die Hub-and-Spoke-VNET-Architektur wird häufig genutzt. Bei einem Hub handelt es sich um ein virtuelles Netzwerk (VNET) in Azure, das als zentraler Konnektivitätspunkt zwischen Ihren Spoke-VNETs und für Ihr lokales Netzwerk fungiert. Die Spokes sind VNETs, die eine Peeringverbindung mit dem Hub herstellen und zur Isolierung von Workloads verwendet werden können. Der Datenverkehr wird über eine ExpressRoute- oder VPN-Verbindung zwischen dem lokalen Rechenzentrum und dem Hub weitergeleitet. Weitere Informationen zur Architektur finden Sie unter [Implementieren einer Hub-Spoke-Netzwerktopologie in Azure][Hub-n-Spoke].

Beim VNET-Peering in einer Region können Spoke-VNETs Hub-VNET-Gateways verwenden (sowohl VPN- als auch ExpressRoute-Gateways), um mit Remotenetzwerken zu kommunizieren.

#### <a name="branch-vnet-connectivity-using-site-to-site-vpn"></a>Branch-VNET-Konnektivität per Site-to-Site-VPN

Wenn Branch-VNETs (in unterschiedlichen Regionen) und lokale Netzwerke über ein Hub-VNET miteinander kommunizieren sollen, wird als native Azure-Lösung die Site-to-Site-VPN-Konnektivität per VPN verwendet. Eine andere Möglichkeit ist die Nutzung eines virtuellen Netzwerkgeräts für das Routing im Hub.

Informationen zur Konfiguration von VPN-Gateways finden Sie unter [Konfigurieren von VPN Gateway][VPN]. Informationen zur Bereitstellung von hoch verfügbaren virtuellen Netzwerkgeräten finden Sie unter [Bereitstellen hoch verfügbarer virtueller Netzwerkgeräte][Deploy-NVA].

## <a name="next-steps"></a>Nächste Schritte

Die Analyse der Testeinrichtung auf Datenebene und die Ansichten der Überwachungsfeatures für Azure-Netzwerke können Sie unter [Analyse auf Datenebene][Data-Analysis] einsehen.

Informationen dazu, wie viele ExpressRoute-Leitungen Sie mit einem ExpressRoute-Gateway (oder umgekehrt) verbinden können, oder zu anderen Skalierungslimits von ExpressRoute finden Sie unter [ExpressRoute – FAQ][ExR-FAQ].


<!--Image References-->
[1]: ./media/backend-interoperability/HubView.png "Hub-and-Spoke-VNET-Perspektive der Topologie"
[2]: ./media/backend-interoperability/Loc1ExRView.png "Perspektive von Standort 1 und des Remote-VNET über ExpressRoute 1 der Topologie"
[3]: ./media/backend-interoperability/Loc1VPNView.png "Perspektive von Standort 1 und des Branch-VNET per S2S-VPN der Topologie"
[4]: ./media/backend-interoperability/Loc2View.png "Perspektive des Standorts 2 der Topologie"
[5]: ./media/backend-interoperability/ExR1-RouteTable.png "Routingtabelle von ExpressRoute 1"

<!--Link References-->
[Setup]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-preface
[Configuration]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-config
[ExpressRoute]: https://docs.microsoft.com/azure/expressroute/expressroute-introduction
[VPN]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways
[VNet]: https://docs.microsoft.com/azure/virtual-network/tutorial-connect-virtual-networks-portal
[Configuration]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-configuration
[Control-Analysis]:https://docs.microsoft.com/azure/networking/connectivty-interoperability-control-plane
[Data-Analysis]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-data-plane
[ExR-FAQ]: https://docs.microsoft.com/azure/expressroute/expressroute-faqs
[S2S-Over-ExR]: https://docs.microsoft.com/azure/expressroute/site-to-site-vpn-over-microsoft-peering
[ExR-S2S-CoEx]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-coexist-resource-manager
[Hub-n-Spoke]: https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke
[Deploy-NVA]: https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha
[VNet-Config]: https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering




