---
title: 'Interoperabilität von ExpressRoute, Site-to-Site-VPN und VNET-Peering – Testeinrichtung: Interoperabilität der Azure-Back-End-Konnektivitätsfeatures | Microsoft-Dokumentation'
description: Auf dieser Seite wird eine Testeinrichtung beschrieben, die verwendet wird, um die Interoperabilität von ExpressRoute-, Site-to-Site-VPN- und VNET-Peering-Features zu analysieren.
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: expressroute,vpn-gateway,virtual-network
ms.topic: article
ms.workload: infrastructure-services
ms.date: 10/18/2018
ms.author: rambala
ms.openlocfilehash: e859a0a3ac35a9d9f2dab579b7609192e599f90f
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2018
ms.locfileid: "49947109"
---
# <a name="interoperability-of-expressroute-site-to-site-vpn-and-vnet-peering---test-setup"></a>Interoperabilität von ExpressRoute, Site-to-Site-VPN und VNET-Peering – Testeinrichtung
In diesem Artikel wird eine Testeinrichtung ermittelt, mit der analysiert werden kann, wie die verschiedenen Features sowohl auf Steuerungsebene als auch auf Datenebene interagieren. Bevor wir auf die Testeinrichtung eingehen, sehen wir uns kurz an, welche Folgen die unterschiedlichen Netzwerkfeatures von Azure mit sich bringen.

ExpressRoute: Mit dem privaten ExpressRoute-Peering können Sie eine direkte Verbindung zwischen privaten IP-Adressbereichen Ihres lokalen Netzwerks und Ihren Azure-VNET-Bereitstellungen herstellen.  Mithilfe von ExpressRoute können Sie eine höhere Bandbreite erzielen und private Verbindungen herstellen. Es gibt viele ExpressRoute-Partnern, die ExpressRoute-Konnektivität mit SLA bieten. Weitere Informationen zu ExpressRoute und zur Konfiguration finden Sie unter [Übersicht über ExpressRoute][ExpressRoute].

Site-to-Site-VPN: Diese Site-to-Site-VPN-Option (S2S) ist dazu gedacht, über das Internet oder über ExpressRoute eine sichere Verbindung zwischen einem lokalen Netzwerk und Azure herzustellen. Informationen zur Konfiguration des S2S-VPN zum Verbindungsaufbau mit Azure finden Sie unter [Was ist ein VPN-Gateway?][VPN].

VNET-Peering: VNET-Peering dient zum Herstellen von Konnektivität zwischen virtuellen Netzwerken (VNETs). Weitere Informationen zum VNET-Peering finden Sie unter [Tutorial: Herstellen einer Verbindung zwischen virtuellen Netzwerken mittels Peering virtueller Netzwerke über das Azure-Portal][VNet].

##<a name="test-setup"></a>Testeinrichtung

Die folgende Abbildung veranschaulicht die Testeinrichtung.

[![1]][1]

Das Herzstück einer Testeinrichtung bildet das Hub-VNET in der Azure-Region 1. Das Hub-VNET wird mit anderen im Folgenden aufgeführten Netzwerken verbunden:

1.  Mit dem Spoke-VNET über das VNET-Peering. Das Spoke-VNET hat Remotezugriff auf beide Gateways im Hub-VNET.
2.  Mit dem Branch-VNET über das Site-to-Site-VPN. Bei der Konnektivität wird eBGP zum Austausch von Routen verwendet.
3.  Mit dem lokalen Netzwerk „Standort 1“ über das private ExpressRoute-Peering als primären Pfad und Site-to-Site-VPN-Konnektivität als Sicherungspfad. Im vorliegenden Dokument wird diese ExpressRoute-Verbindung als „ExpressRoute1“ bezeichnet. Standardmäßig bieten ExpressRoute-Verbindungen redundante Konnektivität zur Bereitstellung von Hochverfügbarkeit. Bei ExpressRoute1 ist die Unterschnittstelle des sekundären CE-Routers für den sekundären MSEE deaktiviert. Dies ist in der obigen Abbildung durch den Pfeil mit zwei Linien gekennzeichnet, bei dem eine der Linien rot gefärbt ist.
4.  Mit dem lokalen Netzwerk „Standort 2“ über ein anderes privates ExpressRoute-Peering. Im vorliegenden Dokument wird die zweite ExpressRoute-Verbindung als „ExpressRoute2“ bezeichnet.
5.  ExpressRoute1 stellt zudem eine Verbindung zwischen dem Hub-VNET sowie dem lokalen Netzwerk „Standort 1“ und einem Remote-VNET in der Azure-Region 2 her.

## <a name="further-reading"></a>Weitere nützliche Informationen

### <a name="using-expressroute-and-site-to-site-vpn-connectivity-in-tandem"></a>Gemeinsames Verwenden von ExpressRoute und Site-to-Site-VPN-Konnektivität

#### <a name="site-to-site-vpn-over-expressroute"></a>Site-to-Site-VPN über ExpressRoute 

Site-to-Site-VPN kann mithilfe von ExpressRoute-Microsoft-Peering konfiguriert werden, um Daten auf privatem Wege zwischen Ihrem lokalen Netzwerk und Ihren Azure-VNETs auszutauschen und dabei Vertraulichkeit, Anti-Replay, Authentizität und Integrität sicherzustellen. Weitere Informationen zur Konfiguration eines Site-to-Site-IPSec-VPN im Tunnelmodus per ExpressRoute-Microsoft-Peering finden Sie unter [Konfigurieren eines Site-to-Site-VPN über ExpressRoute-Microsoft-Peering][S2S-Over-ExR]. 

Die größte Einschränkung bei der Konfiguration des S2S-VPN per Microsoft-Peering ist der Durchsatz. Der Durchsatz des IPSec-Tunnels wird durch die Kapazität des VPN-Gateways eingeschränkt. Der Durchsatz des VPN-Gateways ist im Vergleich zum ExpressRoute-Durchsatz niedriger. In Szenarien dieser Art stellt die Verwendung des IPSec-Tunnels für Datenverkehr mit hohem Schutzfaktor und des privaten Peerings für den gesamten anderen Datenverkehr einen Beitrag zur Optimierung der ExpressRoute-Bandbreitenauslastung dar.

#### <a name="site-to-site-vpn-as-a-secure-failover-path-for-expressroute"></a>Site-to-Site-VPN als sicherer Failoverpfad für ExpressRoute
ExpressRoute wird als redundantes Verbindungspaar bereitgestellt, um Hochverfügbarkeit sicherzustellen. Sie können die georedundante ExpressRoute-Konnektivität in unterschiedlichen Azure-Regionen konfigurieren. Wenn Sie wie in unserer Testeinrichtung in einer bestimmten Azure-Region einen Failoverpfad für Ihre ExpressRoute-Konnektivität einrichten möchten, können Sie hierfür ein Site-to-Site-VPN verwenden. Wenn über ExpressRoute und das S2S-VPN die gleichen Präfixe angekündigt werden, gibt Azure ExpressRoute den Vorzug vor dem S2S-VPN. Zur Vermeidung eines asymmetrischen Routings zwischen ExpressRoute und dem S2S-VPN sollte in der lokalen Netzwerkkonfiguration ExpressRoute ebenfalls den Vorzug vor der S2S-VPN-Konnektivität erhalten.

Weitere Informationen zur Konfiguration von parallelen ExpressRoute- und Site-to-Site-VPN-Verbindungen finden Sie unter [Konfigurieren von parallel bestehenden ExpressRoute- und Site-to-Site-Verbindungen mithilfe von PowerShell][ExR-S2S-CoEx].

### <a name="extending-backend-connectivity-to-spoke-vnets-and-branch-locations"></a>Erweitern der Back-End-Konnektivität auf Spoke-VNETs und Branchstandorte

#### <a name="spoke-vnet-connectivity-using-vnet-peering"></a>Spoke-VNET-Konnektivität per VNET-Peering

Die Hub-Spoke-VNET-Architektur wird häufig genutzt. Bei einem Hub handelt es sich um ein virtuelles Netzwerk (VNET) in Azure, das als zentraler Konnektivitätspunkt zwischen Ihren Spoke-VNETs und für Ihr lokales Netzwerk fungiert. Die Spokes sind VNETs, die eine Peeringverbindung mit dem Hub herstellen und zur Isolierung von Workloads verwendet werden können. Der Datenverkehr wird über eine ExpressRoute- oder VPN-Verbindung zwischen dem lokalen Rechenzentrum und dem Hub weitergeleitet. Weitere Informationen zur Architektur finden Sie unter [Implementieren einer Hub-Spoke-Netzwerktopologie in Azure][Hub-n-Spoke].

Beim VNET-Peering in einer Region können Spoke-VNETs Hub-VNET-Gateways verwenden (sowohl VPN- als auch ExpressRoute-Gateways), um mit Remotenetzwerken zu kommunizieren.

#### <a name="branch-vnet-connectivity-using-site-to-site-vpn"></a>Branch-VNET-Konnektivität per Site-to-Site-VPN

Wenn Branch-VNETs (in unterschiedlichen Regionen) und lokale Netzwerke über ein Hub-VNET miteinander kommunizieren sollen, wird als native Azure-Lösung die Site-to-Site-VPN-Konnektivität per VPN verwendet. Eine andere Möglichkeit ist die Verwendung einer NVA für das Routing im Hub.

Informationen zur Konfiguration von VPN-Gateways finden Sie unter [Was ist ein VPN-Gateway?][VPN]. Informationen zur Bereitstellung von hochverfügbaren NVAs finden Sie unter [Bereitstellen von hochverfügbaren virtuellen Netzwerkappliances][Deploy-NVA].

## <a name="next-steps"></a>Nächste Schritte

Details zur Konfiguration der Testtopologie finden Sie unter [Konfigurationsdetails][Configuration].

Eine Analyse der Testeinrichtung auf Steuerungsebene und einen Überblick über die Ansichten der unterschiedlichen VNETs bzw. VLANs der Topologie finden Sie unter [Analyse auf Steuerungsebene][Control-Analysis].

Die Analyse der Testeinrichtung auf Datenebene und die Ansichten der Überwachungsfeatures für Azure-Netzwerke können Sie unter [Analyse auf Datenebene][Data-Analysis] einsehen.

Informationen dazu, wie viele ExpressRoute-Verbindungen Sie mit einem ExpressRoute-Gateway (oder umgekehrt) verbinden können, oder zu anderen Skalierungslimits von ExpressRoute finden Sie unter [ExpressRoute – FAQ][ExR-FAQ].



<!--Image References-->
[1]: ./media/backend-interoperability/TestSetup.png "Die Testtopologie"

<!--Link References-->
[ExpressRoute]: https://docs.microsoft.com/azure/expressroute/expressroute-introduction
[VPN]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways
[VNet]: https://docs.microsoft.com/azure/virtual-network/tutorial-connect-virtual-networks-portal
[Configuration]: https://docs.microsoft.com/azure/connectivty-interoperability-configuration
[Control-Analysis]:https://docs.microsoft.com/azure/connectivty-interoperability-control-plane
[Data-Analysis]: https://docs.microsoft.com/azure/connectivty-interoperability-data-plane
[ExR-FAQ]: https://docs.microsoft.com/azure/expressroute/expressroute-faqs
[S2S-Over-ExR]: https://docs.microsoft.com/azure/expressroute/site-to-site-vpn-over-microsoft-peering
[ExR-S2S-CoEx]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-coexist-resource-manager
[Hub-n-Spoke]: https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke
[Deploy-NVA]: https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha




