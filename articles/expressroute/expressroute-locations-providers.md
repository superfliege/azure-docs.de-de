---
title: 'Standorte und Konnektivitätsanbieter: Azure ExpressRoute | Microsoft-Dokumentation'
description: Dieser Artikel enthält eine ausführliche Übersicht über die Standorte, an denen Dienste angeboten werden, sowie Informationen darüber, wie Sie eine Verbindung mit den Azure-Regionen herstellen können. Sortiert nach Standort.
services: expressroute
documentationcenter: na
author: cherylmc
manager: timlt
editor: ''
ms.assetid: feb67da3-5abc-4acb-bad4-f78e3c541ded
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/19/2018
ms.author: pareshmu
ms.openlocfilehash: 28c6382ba8749ae93125f73a14bef85e2477fb62
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54821939"
---
# <a name="expressroute-partners-and-peering-locations"></a>ExpressRoute-Partner und Peeringstandorte

> [!div class="op_single_selector"]
> * [Standorte nach Anbieter](expressroute-locations.md)
> * [Anbieter nach Standort](expressroute-locations-providers.md)


In den Tabellen in diesem Artikel finden Sie Informationen zu ExpressRoute-Konnektivitätsanbietern, zum geografischen Geltungsbereich von ExpressRoute, zu Microsoft-Clouddiensten, die über ExpressRoute unterstützt werden, und zu ExpressRoute-Systemintegratoren (SIs).

## <a name="partners"></a>ExpressRoute-Konnektivitätsanbieter
ExpressRoute wird in allen Azure-Regionen und an allen Standorten unterstützt. Die folgende Karte zeigt die Azure-Regionen und ExpressRoute-Standorte. ExpressRoute-Standorte beziehen sich auf jene Orte, an denen Microsoft eine Peeringkooperation mit mehreren Service Providern bietet.

![Standortkarte][0]

Wenn Sie mit mindestens einem ExpressRoute-Standort innerhalb der geopolitischen Region eine Verbindung hergestellt haben, haben Sie Zugriff auf die Azure-Dienste in allen Regionen dieser geopolitischen Region. 

### <a name="azure-regions-to-expressroute-locations-within-a-geopolitical-region"></a>Azure-Regionen mit ExpressRoute-Standorten in einer geopolitischen Region
Die folgende Tabelle bietet eine Übersicht über die Azure-Regionen mit ExpressRoute-Standorten in einer geopolitischen Region.

| **Geopolitische Region** | **Zone** | **Azure-Regionen** | **ExpressRoute-Standorte** |
| --- | --- | --- | --- |
| **Australische Behörden** | 1 | Australien, Mitte; Australien, Mitte 2 |Canberra, Canberra2 |
| **Europa** | 1 |„Frankreich, Mitte“, „Frankreich, Süden“, „Europa, Norden“, „Europa, Westen“, „Vereinigtes Königreich, Westen“, „Vereinigtes Königreich, Süden“ |Amsterdam, Amsterdam2, Dublin, London, London2, Marseille, Newport(Wales), Paris |
| **Nordamerika** | 1 |USA, Osten; USA, Westen; USA, Osten 2; USA, Westen 2; USA, Mitte; USA, Süden-Mitte; USA, Norden-Mitte; USA, Westen-Mitte; Kanada, Mitte; Kanada, Osten |Atlanta, Chicago, Dallas, Denver, Las Vegas, Los Angeles, Miami, New York, San Antonio, Seattle, Silicon Valley, Washington DC, Montreal, Quebec City, Toronto |
| **Asien** | 2 |Ostasien, Südostasien |Hongkong, Kuala Lumpur, Singapur, Singapur2 |
| **Australien** | 2 |Südostaustralien, Ostaustralien |Melbourne, Sydney | 
| **Indien** | 2 |Indien, Westen, Indien, Mitte, Indien, Süden |Chennai, Chennai2, Mumbai, Mumbai2 |
| **Japan** | 2 |Japan West, Japan Ost |Osaka, Tokio |
| **Südkorea** | 2 |Korea, Mitte, Korea, Süden |Busan, Seoul|
| **Südafrika** | 3 |[Südafrika, Westen+, Südafrika, Norden+](https://blogs.microsoft.com/blog/2017/05/18/microsoft-deliver-microsoft-cloud-datacenters-africa/) |Kapstadt, Johannesburg |
| **Südamerika** | 3 |Brasilien Süd |Sao Paulo |

 **+** steht für "In Kürze"


### <a name="regions-and-geopolitical-boundaries-for-national-clouds"></a>Regionen und geopolitische Grenzen für nationale Clouds
In der folgenden Tabelle finden Sie Informationen zu Regionen und geopolitischen Grenzen für nationale Clouds.

| **Geopolitische Region** | **Azure-Regionen** | **ExpressRoute-Standorte** |
| --- | --- | --- |
| **US-Government Cloud** |„US Gov Arizona“, „US Gov Iowa“, „US Gov Texas“, „US Gov Virginia“, „US DoD, Mitte“, „US DoD, Osten“  |Chicago, Dallas, New York, Phoenix, San Antonio, Seattle, Silicon Valley, Washington DC |
| **China, Osten** |China, Osten; China, Osten2 |Shanghai, Shanghai2 |
| **China, Norden** |China, Norden; China, Norden2 |Peking, Peking2 |
| **Deutschland** |Deutschland, Mitte, Deutschland, Ost |Berlin, Frankfurt |

Konnektivität zwischen geopolitischen Regionen wird bei der ExpressRoute-Standard-SKU nicht unterstützt. Sie müssen das ExpressRoute Premium-Add-On aktivieren, um Unterstützung für globale Konnektivität zu erhalten. Verbindungen mit nationalen Cloudumgebungen werden nicht unterstützt. Wenden Sie sich an Ihren Konnektivitätsanbieter, wenn Sie derartige Verbindungen implementieren möchten.

## <a name="locations"></a>Standorte von Konnektivitätsanbietern

Die folgende Tabelle enthält die Konnektivitätsstandorte und die dazugehörigen Service Providers. Sie können auch eine Tabelle anzeigen ([Standorte nach Service Provider](expressroute-locations.md#locations)), die nach Service Providern mit den jeweiligen Standorten sortiert ist. 


### <a name="production-azure"></a>Azure-Produktionsumgebungen
| **Location** | **Besitzer des Peeringstandorts** | **Service Providers** |
| --- | --- | --- |
| **Amsterdam** | Equinix | Aryaka Networks, AT&T NetBond, British Telecom, Colt, Equinix, euNetworks, GÉANT, InterCloud, Interxion, KPN, IX Reach, Level 3 Communications, Megaport, NTT Communications, Orange, Tata Communications, TeleCity Group, Telefonica, Telenor, Telia Carrier, Verizon, Zayo |
| **Amsterdam2** | Interxion | Interxion |
| **Atlanta** | Equinix | Equinix, Megaport |
| **Busan** |LG CNS | LG CNS |
| **Canberra** | CDC | CDC |
| **Canberra2** | CDC | CDC |
| **Kapstadt** | Teraco | Internet Solutions – Cloud Connect, Liquid Telecom, Teraco |
| **Chennai** | Tata Communications | Global CloudXchange (GCX), SIFY, Tata Communications |
| **Chennai2** | Airtel | Airtel |
| **Chicago** | Equinix | Aryaka Networks, AT&T NetBond, Cologix, Comcast, Coresite, Equinix, Internet2, Level 3 Communications, Megaport, PacketFabric, PCCW Global Limited, Sprint, Telia Carrier, Verizon, Zayo |
| **Dallas** | Equinix | Aryaka Networks, AT&T NetBond, Cologix, Equinix, Internet2, Level 3 Communications, Megaport, Neutrona Networks, Telmex Uninet+, Telia Carrier, Verizon, Zayo|
| **Denver** | CoreSite | CoreSite, Megaport |
| **Dublin** | Equinix | Colt, eir, Equinix, Interxion, Megaport |
| **Hongkong** | Equinix | Aryaka Networks, British Telecom, China Telecom Global, Equinix, Megaport, NTT Communications, Orange, PCCW Global Limited, Tata Communications, Verizon |
| **Johannesburg** | Teraco | Internet Solutions – Cloud Connect, Liquid Telecom, Teraco |
| **Kuala Lumpur** | TIME dotCom | TIME dotCom |
| **Las Vegas** | Switch | CenturyLink Cloud Connect, Megaport |
| **London** | Equinix | AT&T NetBond, British Telecom, Colt, Equinix, InterCloud, Internet Solutions – Cloud Connect, Interxion, Jisc, Level 3 Communications, Megaport, MTN, NTT Communications, Orange, PCCW Global Limited, Tata Communications, Telehouse – KDDI, Telenor, Telia Carrier, Verizon, Vodafone, Zayo |
| **London2** | Telehouse | Equinix
| **Los Angeles** | CoreSite | CoreSite, Equinix, Megaport, NTT, Zayo |
| **Marseille** |Interxion | Interxion |
| **Melbourne** | NextDC | AARNet, Equinix, Megaport, NEXTDC, Optus, Telstra Corporation |
| **Miami** | Equinix | C3ntro+, Equinix, Megaport, Neutrona Networks |
| **Montreal** | Cologix | Bell Canada, Cologix, Telus, Zayo |
| **Mumbai** | Tata Communications | Global CloudXchange (GCX), Sify, Tata Communications, Vodafone Idea |
| **Mumbai2** | Airtel | Airtel, Sify, Vodafone Idea |
| **New York** | Equinix | CenturyLink Cloud Connect, Coresite, Equinix, InterCloud, Megaport, Zayo |
| **Newport(Wales)** | Daten der nächsten Generation | Level 3 Communications, Daten der nächsten Generation |
| **Osaka** | Equinix | Equinix, Internet Initiative Japan Inc. - IIJ, NTT Communications, NTT SmartConnect, Softbank |
| **Paris** | Interxion | Colt, Intercloud, Interxion, Equinix, Orange |
| **Quebec City** | 4Degrees | Bell Canada, Megaport |
| **San Antonio** | CyrusOne | CenturyLink Cloud Connect, Megaport |
| **Sao Paulo** | Equinix | Aryaka Networks, Ascenty Data Centers, British Telecom, Equinix, Level 3 Communications, Neutrona Networks, Orange, Tata Communications, Telefonica, UOLDIVEO |
| **Seattle** | Equinix | Aryaka Networks, Equinix, Level 3 Communications, Megaport, Zayo |
| **Seoul** | KINX | KINX, LG CNS, Sejong Telecom |
| **Silicon Valley** | Equinix | Aryaka Networks, AT&T NetBond, British Telecom, CenturyLink Cloud Connect, Comcast, Coresite, Equinix, InterCloud, IX Reach, PacketFabric, Level 3 Communications, Megaport, Orange, Sprint, Tata Communications, Verizon, Zayo |
| **Singapur** | Equinix | Aryaka Networks, AT&T NetBond, British Telecom, Epsilon Global Communications, Equinix, InterCloud, Level 3 Communications, Megaport, NTT Communications, Orange, SingTel, Tata Communications, Telstra Corporation, Verizon, Vodafone |
| **Singapur2** | Global Switch | Megaport, SingTel |
| **Sydney** | Equinix | AARNet, AT&T NetBond, British Telecom, Equinix, Megaport, NEXTDC, NTT Communications, Optus, Orange, Telstra Corporation, Verizon |
| **Tokio** | Equinix | Aryaka Networks, AT&T NetBond, British Telecom, CenturyLink Cloud Connect, Colt, Equinix, Internet Initiative Japan Inc. - IIJ, NTT Communications, NTT EAST, Softbank, Verizon |
| **Toronto** | Cologix | AT&T NetBond, Bell Canada, CenturyLink Cloud Connect, Cologix, Equinix, Megaport, Telus, Zayo |
| **Washington DC** | Equinix | Aryaka Networks, AT&T NetBond, British Telecom, Cologix, Comcast, Coresite, Equinix, Internet2, InterCloud, Level 3 Communications, Megaport, NTT Communications, Orange, PacketFabric, Sprint, Tata Communications, Telia Carrier, Verizon, Zayo |

 **+** steht für "In Kürze"

### <a name="national-cloud-environments"></a>Nationale Cloudumgebungen

### <a name="us-government-cloud"></a>US-Government Cloud
| **Location** | **Service Providers** |
| --- | --- |
| **Chicago** |AT&T NetBond, Equinix, Level 3 Communications, Verizon |
| **Dallas** |Equinix, Megaport, Verizon |
| **New York** |Equinix, CenturyLink Cloud Connect, Verizon |
| **Phoenix** | CenturyLink Cloud Connect |
| **San Antonio** | Megaport |
| **Silicon Valley** | Equinix, Level 3 Communications, Verizon |
| **Seattle** | Equinix, Megaport |
| **Washington DC** |AT&T NetBond, Equinix, Level 3 Communications, Verizon |

### <a name="china"></a>China
| **Location** | **Service Providers** |
| --- | --- |
| **Peking** |China Telecom |
| **Peking2** | GDS |
| **Shanghai** |China Telecom |
| **Shanghai2** | GDS |

Weitere Informationen finden Sie unter [ExpressRoute in China](http://www.windowsazure.cn/home/features/expressroute/)

### <a name="germany"></a>Deutschland
| **Standort** | **Service Providers** |
| --- | --- |
| **Berlin** |e-shelter, Megaport+, T-Systems |
| **Frankfurt** |Colt, Equinix, Interxion |

## <a name="c1partners"></a>Konnektivität über Exchange-Anbieter
Wenn Ihr Konnektivitätsanbieter nicht in den vorherigen Abschnitten enthalten ist, können Sie dennoch eine Verbindung erstellen.

* Fragen Sie Ihren Konnektivitätsanbieter, ob eine Verbindung mit einem der Exchange-Standorte aus der vorherigen Tabelle bereitgestellt werden kann. Sie können auch die folgenden Links überprüfen, um weitere Informationen über die von den Exchange-Anbietern angebotenen Dienste zu erhalten. Viele Konnektivitätsanbieter sind bereits mit Ethernet-Exchanges verbunden.
  * [Cologix](http://www.cologix.com/)
  * [CoreSite](http://www.coresite.com/)
  * [Equinix Cloud Exchange](http://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
  * [InterXion](http://www.interxion.com/)
  * [NextDC](http://www.nextdc.com/)
  * [Megaport](https://www.megaport.com/services/microsoft-expressroute/)
  * [PacketFabric](https://www.packetfabric.com/packetcor/microsoft-azure/)
  
* Bitten Sie Ihren Konnektivitätsanbieter, Ihr Netzwerk auf den Peeringstandort Ihrer Wahl zu erweitern.
  * Stellen Sie sicher, dass Ihr Konnektivitätsanbieter Ihre Konnektivität mit hoher Verfügbarkeit erweitert, sodass es keine einzelnen Fehlerquellen mehr gibt.
* Bestellen Sie eine ExpressRoute-Verbindung mit der Exchange als Konnektivitätsanbieter für die Verbindung mit Microsoft.
  * Führen Sie die Schritte in [Erstellen einer ExpressRoute-Verbindung](expressroute-howto-circuit-classic.md) aus, um die Verbindung einzurichten.

## <a name="c1partners"></a>Konnektivität über zusätzliche Dienstanbieter
| **Location** | **Exchange** | **Konnektivitätsanbieter** |
| --- | --- | --- |
| **Amsterdam** | Equinix, Telecity | BICS, CloudXpress, Eurofiber, Fastweb S.p.A, Gulf Bridge International, MainOne, Nianet, Post, Proximus, Telecom Italia Sparkle, Telia |
| **Kapstadt** | Teraco | MTN |
| **Chicago** | Equinix | Lightower, Windstream |
| **Dallas** | Equinix, Megaport | Axtel, C3ntro Telecom, Cox Business, Data Foundry, Transtelco |
| **Frankfurt** | Telecity | BICS, Cinia, Nianet, QSC AG |
| **Hamburg** | Equinix | Cinia |
| **Hongkong** | Equinix | Chief, Macroview Telecom |
| **Johannesburg** | Teraco | MTN |
| **London** | BICS, Equinix, euNetworks, Telecity | Bezeq International Ltd., Epsilon Telecommunications Limited, Exponential E, HSO, NexGen Networks, Proximus, Tamares Telecom, Zain |
| **Los Angeles** | Equinix |Transtelco |
| **Madrid** | Level3 | Zertia |
| **Montreal** | Cologix, Equinix | Airgate Technologies, Inc. Cogeco Peer 1, Rogers, Zirro |
| **New York** |Equinix, Megaport | Altice Business, Lightower, Webair |
| **Paris** | Equinix | Proximus |
| **Quebec City** | Megaport | Fibrenoire |
| **São Paulo** | Equinix | Venha Pra Nuvem |
| **Seattle** |Equinix | Alaska Communications |
| **Silicon Valley** |Equinix | Cox Business, Windstream |
| **Singapur** |Equinix |1CLOUDSTAR, BICS, Epsilon Telecommunications Limited, LGA Telecom, United Information Highway (UIH) |
| **Slough** | Equinix | HSO|
| **Sydney** | Megaport | Macquarie Telecom Group|
| **Tokio** | Equinix | ARTERIA Networks Corporation, BroadBand Tower, Inc. |
| **Toronto** | Equinix | Airgate Technologies, Inc. Cogeco Peer 1, IVedha Inc, Rogers, Thinktel, Zirro|
| **Washington DC** |Equinix | Altice Business, BICS, Cox Business, Gtt Communications Inc, Epsilon Telecommunications Limited, Lightower, Masergy, Windstream |

## <a name="expressroute-system-integrators"></a>ExpressRoute-Systemintegratoren
Je nach Ihrer Netzwerkgröße kann das Aktivieren privater Verbindungen für Ihre Anforderungen problematisch sein. Alle Systemintegratoren, die in der folgenden Tabelle aufgeführt sind, können Ihnen beim Integrieren von ExpressRoute helfen.

| **Kontinent** | **Systemintegratoren** |
| --- | --- |
| **Asien** |Avanade Inc., OneAs1a |
| **Australien** | Ensyst, IT Consultancy, MOQdigital, Vigilant.IT |
| **Europa** |Avanade Inc., Altogee, Bright Skies GmbH, Inframon, MSG Services, New Signature, Nelite, Orange Networks, sol-tec |
| **Nordamerika** |Avanade Inc., Equinix Professional Services, FlexManage, Lightstream, Perficient, Presidio |
| **Südamerika** |Avanade Inc., Venha Pra Nuvem |
## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen über ExpressRoute finden Sie unter [ExpressRoute – FAQ](expressroute-faqs.md).
* Stellen Sie sicher, dass alle Voraussetzungen erfüllt sind. Informationen finden Sie unter [ExpressRoute-Voraussetzungen](expressroute-prerequisites.md).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Standortkarte"
