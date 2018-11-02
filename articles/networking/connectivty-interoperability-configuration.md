---
title: 'Interoperabilität von ExpressRoute, Site-to-Site-VPN und VNET-Peering – Konfigurationsdetails: Interoperabilität der Azure-Back-End-Konnektivitätsfeatures | Microsoft-Dokumentation'
description: Auf dieser Seite werden die Konfigurationsdetails der Testeinrichtung beschrieben, die verwendet wird, um die Interoperabilität von ExpressRoute-, Site-to-Site-VPN- und VNET-Peering-Features zu analysieren.
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: expressroute,vpn-gateway,virtual-network
ms.topic: article
ms.workload: infrastructure-services
ms.date: 10/18/2018
ms.author: rambala
ms.openlocfilehash: d94900b764331c6fff0e0384e6edbebc88ac938b
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2018
ms.locfileid: "49946999"
---
# <a name="interoperability-of-expressroute-site-to-site-vpn-and-vnet-peering---test-configuration-details"></a>Interoperabilität von ExpressRoute, Site-to-Site-VPN und VNET-Peering – Testkonfigurationsdetails

In diesem Artikel werden die Konfigurationsdetails der Testeinrichtung beschrieben. Informationen zur Testeinrichtung finden Sie unter [Testeinrichtung][Setup]. 

##<a name="spoke-vnet-connectivity-using-vnet-peering"></a>Spoke-VNET-Konnektivität per VNET-Peering

Der folgende Screenshot zum Azure-Portal zeigt die Details zum VNET-Peering des Spoke-VNET. Einen ausführlichen Leitfaden zum Konfigurieren des VNET-Peerings zwischen zwei virtuellen Netzwerken finden Sie unter [Erstellen, Löschen oder Ändern des eines VNET-Peerings][VNet-Config]. Wenn das Spoke-VNET die Gateways verwenden soll, die mit dem Hub-VNET verbunden sind, müssen Sie *Remotegateways verwenden* aktivieren.

[![1]][1]

Der folgende Screenshot zum Azure-Portal zeigt die Details zum VNET-Peering des Hub-VNET. Wenn das Hub-VNET dem Spoke-VNET die Verwendung seiner Gateways erlauben soll, müssen Sie *Remotegateways verwenden* aktivieren.

[![2]][2]

##<a name="branch-vnet-connectivity-using-site-to-site-vpn"></a>Branch-VNET-Konnektivität per Site-to-Site-VPN

Site-to-Site-VPN-Konnektivität zwischen den Hub- und Branch-VNET wird mithilfe von VPN-Gateways konfiguriert. Standardmäßig werden VPN- und ExpressRoute-Gateways mit dem Wert der privaten ASN „65515“ konfiguriert. Bei VPN-Gateways können Sie den ASN-Wert ändern. Wie im folgenden Screenshot des Azure-Portals gezeigt, wird in der Testeinrichtung der ASN-Wert, der das VPN-Gateway des Branch-VNET in 65516 geändert, um das eBGP-Routing zwischen dem Hub- und Branch-VNET zu aktivieren.


[![3]][3]


##<a name="location-1-on-premises-connectivity-using-expressroute-and-site-to-site-vpn"></a>Lokale Konnektivität mit „Standort 1“ mithilfe von ExpressRoute und Site-to-Site-VPN-Konnektivität

###<a name="expressroute1-configuration-details"></a>Details der ExpressRoute1-Konfiguration

Der folgende Portalscreenshot zeigt die Konfiguration der ExpressRoute-Verbindung in der Azure-Region 1 mit den lokalen CE-Routern von Standort 1.

[![4]][4]

Der folgende Portalscreenshot zeigt die Verbindungskonfiguration zwischen der ExpressRoute1-Verbindung und dem Hub-VNET.

[![5]][5]

Die folgende Konfiguration zeigt die Auflistung der primären CE-Routerkonfiguration (Cisco-ASR1000 Router als CE-Router in der Testeinrichtung) im Zusammenhang mit privater ExpressRoute-Peeringkonnektivität. Wenn sowohl das Site-to-Site-VPN als auch die ExpressRoute-Verbindung parallel konfiguriert sind, um ein lokales Netzwerk mit Azure herzustellen, gibt Azure standardmäßig der ExpressRoute-Verbindung den Vorzug. Zur Vermeidung eines asymmetrischen Routings sollte im lokalen Netzwerk auch ExpressRoute über das Site-to-Site-VPN für die Routen bevorzugt werden, die über das ExpressRoute- und das Site-to-Site-VPN empfangen werden. Dies wird durch die folgende Konfiguration mit dem BGP-Attribut „local-preference“ erreicht. 

    interface TenGigabitEthernet0/0/0.300
     description Customer 30 private peering to Azure
     encapsulation dot1Q 30 second-dot1q 300
     ip vrf forwarding 30
     ip address 192.168.30.17 255.255.255.252
    !
    interface TenGigabitEthernet1/0/0.30
     description Customer 30 to south bound LAN switch
     encapsulation dot1Q 30
     ip vrf forwarding 30
     ip address 192.168.30.0 255.255.255.254
     ip ospf network point-to-point
    !
    router ospf 30 vrf 30
     router-id 10.2.30.253
     redistribute bgp 65021 subnets route-map BGP2OSPF
     network 192.168.30.0 0.0.0.1 area 0.0.0.0
    default-information originate always
     default-metric 10
    !
    router bgp 65021
     !
     address-family ipv4 vrf 30
      network 10.2.30.0 mask 255.255.255.128
      neighbor 192.168.30.18 remote-as 12076
      neighbor 192.168.30.18 activate
      neighbor 192.168.30.18 next-hop-self
      neighbor 192.168.30.18 soft-reconfiguration inbound
      neighbor 192.168.30.18 route-map prefer-ER-over-VPN in
      neighbor 192.168.30.18 prefix-list Cust30_to_Private out
     exit-address-family
    !
    route-map prefer-ER-over-VPN permit 10
     set local-preference 200
    !
    ip prefix-list Cust30_to_Private seq 10 permit 10.2.30.0/25
    !

###<a name="site-to-site-vpn-configuration-details"></a>Details der Konfiguration einer Site-to-Site-VPN-Verbindung

Im Folgenden finden Sie die Liste der primären CE-Routerkonfiguration im Zusammenhang mit Site-to-Site-VPN-Konnektivität:

    crypto ikev2 proposal Cust30-azure-proposal
     encryption aes-cbc-256 aes-cbc-128 3des
     integrity sha1
     group 2
    !
    crypto ikev2 policy Cust30-azure-policy
     match address local 66.198.12.106
     proposal Cust30-azure-proposal
    !
    crypto ikev2 keyring Cust30-azure-keyring
     peer azure
      address 52.168.162.84
      pre-shared-key local IamSecure123
      pre-shared-key remote IamSecure123
    !
    crypto ikev2 profile Cust30-azure-profile
     match identity remote address 52.168.162.84 255.255.255.255
     identity local address 66.198.12.106
     authentication local pre-share
     authentication remote pre-share
     keyring local Cust30-azure-keyring
    !
    crypto ipsec transform-set Cust30-azure-ipsec-proposal-set esp-aes 256 esp-sha-hmac
     mode tunnel
    !
    crypto ipsec profile Cust30-azure-ipsec-profile
     set transform-set Cust30-azure-ipsec-proposal-set
     set ikev2-profile Cust30-azure-profile
    !
    interface Loopback30
     ip address 66.198.12.106 255.255.255.255
    !
    interface Tunnel30
     ip vrf forwarding 30
     ip address 10.2.30.125 255.255.255.255
     tunnel source Loopback30
     tunnel mode ipsec ipv4
     tunnel destination 52.168.162.84
     tunnel protection ipsec profile Cust30-azure-ipsec-profile
    !
    router bgp 65021
     !
     address-family ipv4 vrf 30
      network 10.2.30.0 mask 255.255.255.128
      neighbor 10.10.30.254 remote-as 65515
      neighbor 10.10.30.254 ebgp-multihop 5
      neighbor 10.10.30.254 update-source Tunnel30
      neighbor 10.10.30.254 activate
      neighbor 10.10.30.254 soft-reconfiguration inbound
     exit-address-family
    !
    ip route vrf 30 10.10.30.254 255.255.255.255 Tunnel30

##<a name="location-2-on-premises-connectivity-using-expressroute"></a>Lokale Konnektivität mit „Standort 2“ mithilfe von ExpressRoute

Eine zweite ExpressRoute-Verbindung in größerer Nähe zu Standort 2 lokal wird zwischen dem lokalen Netzwerk „Standort 2“ und dem Hub-VNET aufgebaut. Das folgende Portalscreenshot zeigt die zweite ExpressRoute-Konfiguration.

[![6]][6]

Der folgende Portalscreenshot zeigt die Verbindungskonfiguration zwischen der zweiten ExpressRoute-Verbindung und dem Hub-VNET.

[![7]][7]

ExpressRoute1 stellt eine Verbindung zwischen dem Hub-VNET sowie dem lokalen Netzwerk „Standort 1“ und einem Remote-VNET in einer anderen Azure-Region her.

[![8]][8]

## <a name="further-reading"></a>Weitere nützliche Informationen

### <a name="using-expressroute-and-site-to-site-vpn-connectivity-in-tandem"></a>Gemeinsames Verwenden von ExpressRoute und Site-to-Site-VPN-Konnektivität

#### <a name="site-to-site-vpn-over-expressroute"></a>Site-to-Site-VPN über ExpressRoute

Site-to-Site-VPN kann mithilfe von ExpressRoute-Microsoft-Peering konfiguriert werden, um Daten auf privatem Wege zwischen Ihrem lokalen Netzwerk und Ihren Azure-VNETs auszutauschen und dabei Vertraulichkeit, Anti-Replay, Authentizität und Integrität sicherzustellen. Weitere Informationen zur Konfiguration eines Site-to-Site-IPSec-VPN im Tunnelmodus per ExpressRoute-Microsoft-Peering finden Sie unter [Konfigurieren eines Site-to-Site-VPN über ExpressRoute-Microsoft-Peering][S2S-Over-ExR]. 

Die wichtigste Einschränkung bei der Konfiguration des S2S-VPN per Microsoft-Peering ist der Durchsatz. Der Durchsatz des IPSec-Tunnels wird durch die Kapazität des VPN-Gateways eingeschränkt. Der Durchsatz des VPN-Gateways ist im Vergleich zum ExpressRoute-Durchsatz niedriger. In Szenarien dieser Art stellt die Verwendung des IPSec-Tunnels für Datenverkehr mit hohem Schutzfaktor und des privaten Peerings für den gesamten anderen Datenverkehr einen Beitrag zur Optimierung der ExpressRoute-Bandbreitenauslastung dar.

#### <a name="site-to-site-vpn-as-a-secure-failover-path-for-expressroute"></a>Site-to-Site-VPN als sicherer Failoverpfad für ExpressRoute
Die ExpressRoute-Verbindung wird als redundantes Verbindungspaar bereitgestellt, um Hochverfügbarkeit sicherzustellen. Sie können die georedundante ExpressRoute-Konnektivität in unterschiedlichen Azure-Regionen konfigurieren. Wenn Sie wie in unserer Testeinrichtung in einer bestimmten Azure-Region einen Failoverpfad für Ihre ExpressRoute-Konnektivität einrichten möchten, können Sie hierfür ein Site-to-Site-VPN verwenden. Wenn über ExpressRoute und das S2S-VPN die gleichen Präfixe angekündigt werden, gibt Azure ExpressRoute den Vorzug vor dem S2S-VPN. Zur Vermeidung des asymmetrischen Routings zwischen ExpressRoute und dem S2S-VPN sollte in der lokalen Netzwerkkonfiguration ExpressRoute ebenfalls den Vorzug vor der S2S-VPN-Konnektivität erhalten.

Weitere Informationen zur Konfiguration von parallelen ExpressRoute- und Site-to-Site-VPN-Verbindungen finden Sie unter [Konfigurieren von parallel bestehenden ExpressRoute- und Site-to-Site-Verbindungen mithilfe von PowerShell][ExR-S2S-CoEx].

### <a name="extending-backend-connectivity-to-spoke-vnets-and-branch-locations"></a>Erweitern der Back-End-Konnektivität auf Spoke-VNETs und Branchstandorte

#### <a name="spoke-vnet-connectivity-using-vnet-peering"></a>Spoke-VNET-Konnektivität per VNET-Peering

Die Hub-and-Spoke-VNET-Architektur wird häufig genutzt. Bei einem Hub handelt es sich um ein virtuelles Netzwerk (VNET) in Azure, das als zentraler Konnektivitätspunkt zwischen Ihren Spoke-VNETs und für Ihr lokales Netzwerk fungiert. Die Spokes sind VNETs, die eine Peeringverbindung mit dem Hub herstellen und zur Isolierung von Workloads verwendet werden können. Der Datenverkehr wird über eine ExpressRoute- oder VPN-Verbindung zwischen dem lokalen Rechenzentrum und dem Hub weitergeleitet. Weitere Informationen zur Architektur finden Sie unter [Implementieren einer Hub-Spoke-Netzwerktopologie in Azure][Hub-n-Spoke].

Beim VNET-Peering in einer Region können Spoke-VNETs Hub-VNET-Gateways verwenden (sowohl VPN- als auch ExpressRoute-Gateways), um mit Remotenetzwerken zu kommunizieren.

#### <a name="branch-vnet-connectivity-using-site-to-site-vpn"></a>Branch-VNET-Konnektivität per Site-to-Site-VPN

Wenn Branch-VNETs (in unterschiedlichen Regionen) und lokale Netzwerke über ein Hub-VNET miteinander kommunizieren sollen, wird als native Azure-Lösung die Site-to-Site-VPN-Konnektivität per VPN verwendet. Eine andere Möglichkeit ist die Nutzung eines virtuellen Netzwerkgeräts für das Routing im Hub.

Informationen zur Konfiguration von VPN-Gateways finden Sie unter [Konfigurieren von VPN Gateway][VPN]. Informationen zur Bereitstellung von hochverfügbaren NVAs finden Sie unter [Bereitstellen von hochverfügbaren virtuellen Netzwerkappliances][Deploy-NVA].

## <a name="next-steps"></a>Nächste Schritte

Eine Analyse der Testeinrichtung auf Steuerungsebene und einen Überblick über die Ansichten der unterschiedlichen VNETs bzw. VLANs der Topologie finden Sie unter [Analyse auf Steuerungsebene][Control-Analysis].

Die Analyse der Testeinrichtung auf Datenebene und die Ansichten der Überwachungsfeatures für Azure-Netzwerke können Sie unter [Analyse auf Datenebene][Data-Analysis] einsehen.

Informationen dazu, wie viele ExpressRoute-Verbindungen Sie mit einem ExpressRoute-Gateway (oder umgekehrt) verbinden können, oder zu anderen Skalierungslimits von ExpressRoute finden Sie unter [ExpressRoute – FAQ][ExR-FAQ].


<!--Image References-->
[1]: ./media/backend-interoperability/SpokeVNet_peering.png "VNET-Peering des Spoke-VNET"
[2]: ./media/backend-interoperability/HubVNet-peering.png "VNET-Peering des Hub-VNET"
[3]: ./media/backend-interoperability/BranchVNet-VPNGW.png "VPN-GW-Konfiguration des Branch-VNET"
[4]: ./media/backend-interoperability/ExR1.png "ExpressRoute1-Konfiguration"
[5]: ./media/backend-interoperability/ExR1-Hub-Connection.png "Verbindungskonfiguration zwischen ExpressRoute1 und dem ExpressRoute-Gateway des Hub-VNET"
[6]: ./media/backend-interoperability/ExR2.png "ExpressRoute2-Konfiguration"
[7]: ./media/backend-interoperability/ExR2-Hub-Connection.png "Konfiguration der Verbindung zwischen ExpressRoute2 und dem ExpressRoute-Gateway des Hub-VNET"
[8]: ./media/backend-interoperability/ExR2-Remote-Connection.png "Konfiguration der Verbindung zwischen ExpressRoute2 und dem ExpressRoute-Gateway des Remote-VNET"

<!--Link References-->
[Setup]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-preface
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




