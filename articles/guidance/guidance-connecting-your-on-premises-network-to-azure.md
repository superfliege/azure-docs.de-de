---
title: Verbinden Ihres lokalen Netzwerks mit Azure | Microsoft-Dokumentation
description: Erläuterung und Vergleich der verschiedenen verfügbaren Methoden für das Herstellen einer Verbindung mit Microsoft Cloud Services wie Azure, Office 365 und Dynamics CRM Online.
services: ''
documentationcenter: na
author: cherylmc
manager: timlt
editor: ''
tags: ''
ms.assetid: 294d39ad-40e0-476a-a362-57911b1172b7
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2016
ms.author: cherylmc
ms.openlocfilehash: 9361c0b2ee84d7459bd432f5a8ce13fc5fd85ef8
ms.sourcegitcommit: f331186a967d21c302a128299f60402e89035a8d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58408326"
---
# <a name="connecting-your-on-premises-network-to-azure"></a>Verbinden Ihres lokalen Netzwerks mit Azure
Microsoft bietet mehrere Typen von Clouddiensten. Sie können sich zwar mit allen Diensten über das öffentliche Internet verbinden. Sie können aber auch mit einigen der Dienste durch einen VPN-Tunnel (Virtual Private Network) über das Internet oder über eine direkte, private Verbindung mit Microsoft eine Verbindung herstellen. In diesem Artikel erfahren Sie, welche Konnektivitätsoption Ihren Anforderungen basierend auf den Arten von Microsoft Cloud Services, die Sie nutzen, am ehesten entspricht. Die meisten Organisationen nutzen mehrere Verbindungstypen, die nachstehend beschrieben werden.

## <a name="connecting-over-the-public-internet"></a>Herstellen einer Verbindung über das öffentliche Internet
Dieser Verbindungstyp ermöglicht den Zugriff auf Microsoft Cloud Services direkt über das Internet (siehe unten).

![Internet](./media/guidance-connecting-your-on-premises-network-to-azure/internet.png "Internet")

Diese Verbindung ist üblicherweise der erste Typ, der für die Verbindung mit Microsoft Cloud Services verwendet wird. Die folgende Tabelle enthält die Vor- und Nachteile dieser Art von Verbindung.

| **Vorteile** | **Überlegungen** |
| --- | --- |
| Erfordert keine Änderungen an Ihrem lokalen Netzwerk, solange alle Clientgeräte uneingeschränkten Zugriff auf alle IP-Adressen und Ports im Internet haben. |Obwohl der Datenverkehr oft mit HTTPS verschlüsselt wird, kann er während der Übertragung abgefangen werden, da er das öffentliche Internet durchquert. |
| Verbindung mit allen Microsoft Cloud Services möglich, die über das öffentliche Internet verfügbar sind. |Unvorhersehbare Wartezeiten, da der Datenverkehr das Internet durchquert. |
| Ihre bestehende Internetverbindung wird verwendet. | |
| Erfordert keine Verwaltung von Konnektivitätsgeräten. | |

Bei dieser Verbindung fallen keine Konnektivitäts- oder Bandbreitenkosten an, da Sie Ihre bestehende Internetverbindung nutzen.

## <a name="connecting-with-a-point-to-site-connection"></a>Verbinden über eine Point-to-Site-Verbindung
Dieser Verbindungstyp ermöglicht den Zugriff auf einige Microsoft Cloud Services durch einen SSTP-Tunnel (Secure Socket Tunneling Protocol) über das Internet (siehe unten).

![P2S](./media/guidance-connecting-your-on-premises-network-to-azure/p2s.png "Point-to-Site-Verbindung")

Die Verbindung erfolgt über Ihre bestehende Internetverbindung, erfordert aber die Verwendung eines Azure-VPN-Gateways. Die folgende Tabelle enthält die Vor- und Nachteile dieser Art von Verbindung.

| **Vorteile** | **Überlegungen** |
| --- | --- |
| Erfordert keine Änderungen an Ihrem lokalen Netzwerk, solange alle Clientgeräte uneingeschränkten Zugriff auf alle IP-Adressen und Ports im Internet haben. |Obwohl der Datenverkehr mit SSTP verschlüsselt wird, kann er während der Übertragung abgefangen werden, da er das öffentliche Internet durchquert. |
| Ihre bestehende Internetverbindung wird verwendet. |Unvorhersehbare Wartezeiten, da der Datenverkehr das Internet durchquert. |
| Durchsatz von bis zu 200 Mb/s pro Gateway. |Erfordert die Erstellung und Verwaltung separater Verbindungen zwischen jedem Gerät in Ihrem lokalen Netzwerk und jedem Gateway, mit dem sich jedes Gerät verbinden muss. |
| Kann verwendet werden, um eine Verbindung mit Azure-Diensten herzustellen, die mit einem Azure Virtual Network (VNet) verbunden werden können, wie beispielsweise Azure Virtual Machines und Azure Cloud Services. |Erfordert eine minimale laufende Verwaltung eines Azure-VPN-Gateways. |
| Kann nicht verwendet werden, um eine Verbindung mit Microsoft Office 365 oder Dynamics CRM Online herzustellen. | |
| Kann nicht verwendet werden, um eine Verbindung mit Azure-Diensten herzustellen, die nicht mit einem VNet verbunden werden können. | |

Erfahren Sie mehr über den [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md)-Dienst, seine [Preise](https://azure.microsoft.com/pricing/details/vpn-gateway) und die [Preise](https://azure.microsoft.com/pricing/details/data-transfers) für ausgehende Datenübertragungen.

## <a name="connecting-with-a-site-to-site-connection"></a>Verbinden über eine Site-to-Site-Verbindung
Dieser Verbindungstyp ermöglicht den Zugriff auf einige Microsoft Cloud Services durch einen IPSec-Tunnel über das Internet (siehe unten).

![S2S](./media/guidance-connecting-your-on-premises-network-to-azure/s2s.png "Site-to-Site-Verbindung")

Die Verbindung erfolgt über Ihre bestehende Internetverbindung, erfordert aber die Verwendung eines Azure-VPN-Gateways, wobei für das Gateway selbst und ausgehende Datenübertragungen Kosten anfallen. Die folgende Tabelle enthält die Vor- und Nachteile dieser Art von Verbindung.

| **Vorteile** | **Überlegungen** |
| --- | --- |
| Alle Geräte in Ihrem lokalen Netzwerk können mit Azure-Diensten kommunizieren, die mit einem VNet verbunden sind, weshalb es nicht notwendig ist, einzelne Verbindungen für jedes Gerät zu konfigurieren. |Obwohl der Datenverkehr mit IPSec verschlüsselt wird, kann er während der Übertragung abgefangen werden, da er das öffentliche Internet durchquert. |
| Ihre bestehende Internetverbindung wird verwendet. |Unvorhersehbare Wartezeiten, da der Datenverkehr das Internet durchquert. |
| Kann verwendet werden, um eine Verbindung mit Azure-Diensten herzustellen, die mit einem Azure Virtual Network (VNet) verbunden werden können, wie beispielsweise Azure Virtual Machines und Azure Cloud Services. |Ein überprüftes VPN-Gerät* muss lokal konfiguriert und verwaltet werden. |
| Durchsatz von bis zu 200 Mb/s pro Gateway. |Erfordert eine minimale laufende Verwaltung eines Azure-VPN-Gateways. |
| Kann erzwingen, dass von virtuellen Computern in der Cloud ausgelöster ausgehender Datenverkehr über benutzerdefinierte Routen oder das Border Gateway Protocol (BGP)** zur Inspektion und Protokollierung durch das lokale Netzwerk geleitet wird. |Kann nicht verwendet werden, um eine Verbindung mit Microsoft Office 365 oder Dynamics CRM Online herzustellen. |
| Kann nicht verwendet werden, um eine Verbindung mit Azure-Diensten herzustellen, die nicht mit einem VNet verbunden werden können. | |
| Wenn Sie Dienste verwenden, die Verbindungen mit lokalen Geräten einleiten, und Ihre Sicherheitsrichtlinien dies vorsehen, benötigen Sie möglicherweise eine Firewall zwischen dem lokalen Netzwerk und Azure. | |

* *Hier finden Sie eine Liste [überprüfter VPN-Geräte](../vpn-gateway/vpn-gateway-about-vpn-devices.md#devicetable).
* **Erfahren Sie mehr über die Verwendung von [benutzerdefinierten Routen](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md) oder [BGP](../vpn-gateway/vpn-gateway-bgp-overview.md), um das Routing von Azure VNets zu einem lokalen Gerät zu erzwingen.

## <a name="connecting-with-a-dedicated-private-connection"></a>Verbinden über eine dedizierte private Verbindung
Dieser Verbindungstyp ermöglicht den Zugriff auf alle Microsoft Cloud Services über eine dedizierte private Verbindung mit Microsoft, die nicht über das Internet erfolgt (siehe unten).

![ER](./media/guidance-connecting-your-on-premises-network-to-azure/er.png "ExpressRoute-Verbindung")

Die Verbindung erfordert die Nutzung des ExpressRoute-Diensts und eine Verbindung mit einem Konnektivitätsanbieter. Die folgende Tabelle enthält die Vor- und Nachteile dieser Art von Verbindung.

| **Vorteile** | **Überlegungen** |
| --- | --- |
| Datenverkehr kann während der Übertragung über das öffentliche Internet nicht abgefangen werden, da eine dedizierte Verbindung über einen Dienstanbieter zum Einsatz kommt. |Erfordert eine lokale Routerverwaltung. |
| Bis zu 10 Gb/s Bandbreite pro ExpressRoute-Leitung und bis zu 2 Gb/s Durchsatz pro Gateway. |Erfordert eine dedizierte Verbindung mit einem Konnektivitätsanbieter. |
| Vorhersehbare Latenz, da es sich um eine dedizierte Verbindung mit Microsoft handelt, die nicht über das Internet führt. |Erfordert möglicherweise eine minimale laufende Verwaltung eines oder mehrerer Azure-VPN-Gateways (wenn die Leitung mit VNets verbunden ist). |
| Erfordert keine verschlüsselte Kommunikation, wenngleich Sie den Datenverkehr auf Wunsch verschlüsseln können. |Wenn Sie Clouddienste verwenden, die Verbindungen mit lokalen Geräten einleiten, benötigen Sie möglicherweise eine Firewall zwischen dem lokalen Netzwerk und Azure. |
| Direkte Verbindung mit allen Microsoft Cloud Services bis auf wenige Ausnahmen* möglich. |Erfordert die Netzwerkadressübersetzung (NAT) von lokalen IP-Adressen, die in die Microsoft-Rechenzentren gelangen, für Dienste, die nicht mit einem VNet verbunden werden können.** |
| Kann erzwingen, dass von virtuellen Computern in der Cloud ausgelöster ausgehender Datenverkehr über BGP zur Inspektion und Protokollierung durch das lokale Netzwerk geleitet wird. | |

* *Hier finden Sie eine [Liste der Dienste](../expressroute/expressroute-faqs.md#supported-services), die nicht mit ExpressRoute genutzt werden können. Ihr Azure-Abonnement muss eine Verbindung mit Office 365 herstellen können dürfen.  Im Artikel [Azure ExpressRoute für Office 365](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd?ui=en-US&rs=en-US&ad=US&fromAR=1) finden Sie Einzelheiten.
* **Erfahren Sie mehr zu den [NAT](../expressroute/expressroute-nat.md)-Anforderungen von ExpressRoute.

Erfahren Sie mehr über [ExpressRoute](../expressroute/expressroute-introduction.md), die zugehörigen [Preise](https://azure.microsoft.com/pricing/details/expressroute) und [Konnektivitätsanbieter](../expressroute/expressroute-locations.md#locations).

## <a name="additional-considerations"></a>Zusätzliche Überlegungen
* Bei einigen der oben genannten Optionen gelten unterschiedliche Höchstgrenzen, die für VNet- und Gatewayverbindungen unterstützt werden können, sowie andere Kriterien. Es wird empfohlen, sich mit den [Grenzwerten für Netzwerke](../azure-subscription-service-limits.md#networking-limits) in Azure zu befassen, um zu verstehen, ob sich einer davon auf die von Ihnen gewählten Verbindungstypen auswirkt.
* Wenn Sie planen, ein Gateway über eine Site-to-Site-VPN-Verbindung mit dem gleichen VNet wie ein ExpressRoute-Gateway zu verbinden, sollten Sie sich zunächst mit wichtigen Einschränkungen vertraut machen. Im Artikel [Konfigurieren von parallel bestehenden ExpressRoute- und Site-to-Site-Verbindungen](../expressroute/expressroute-howto-coexist-resource-manager.md#limits-and-limitations) erfahren Sie mehr dazu.

## <a name="next-steps"></a>Nächste Schritte
In den folgenden Ressourcen wird erläutert, wie Sie die in diesem Artikel behandelten Verbindungstypen implementieren.

* [Implementieren einer Point-to-Site-Verbindung](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)
* [Implementieren einer Site-to-Site-Verbindung](guidance-hybrid-network-vpn.md)
* [Implementieren einer dedizierten privaten Verbindung](guidance-hybrid-network-expressroute.md)
* [Implementieren einer dedizierten privaten Verbindung mit einer Site-zu-Site-Verbindung für Hochverfügbarkeit](guidance-hybrid-network-expressroute-vpn-failover.md)
