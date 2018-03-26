---
title: Includedatei
description: Includedatei
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: fa9c27457b1da4d233aaea2a6621af9f5d01149d
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
|  | **Punkt-zu-Standort** | **Standort-zu-Standort** | **ExpressRoute** |
| --- | --- | --- | --- |
| **Von Azure unterstützte Dienste** |Cloud Services und Virtual Machines |Cloud Services und Virtual Machines |[Dienstliste](../articles/expressroute/expressroute-faqs.md#supported-services) |
| **Typische Bandbreiten** |In der Regel insgesamt < 100 MBit/s |In der Regel insgesamt < 1 GBit/s |50 MBit/s, 100 MBit/s, 200 MBit/s, 500 MBit/s, 1 GBit/s, 2 GBit/s, 5 GBit/s, 10 GBit/s |
| **Unterstützte Protokolle** |Secure Sockets Tunneling Protocol (SSTP) |IPsec |Direkte Verbindung über VLANs, VPN-Technologien des NSP (MPLS, VPLS ...) |
| **Routing** |RouteBased (dynamisch) |Wir unterstützen PolicyBased-VPNs (statisches Routing) und RouteBased-VPNs (dynamisches Routing) |BGP |
| **Verbindungsstabilität** |Aktiv-passiv |Aktiv-passiv oder aktiv-aktiv |Aktiv-aktiv |
| **Typisches Anwendungsbeispiel** |Erstellen von Prototypen, Entwicklungs-/Test-/Laborszenarios für Cloud Services und Virtual Machines |Entwicklungs-/Test-/Laborszenarios und kleine Produktionsworkloads für Cloud Services und Virtual Machines |Zugriff auf alle Azure-Dienste (überprüfte Liste), unternehmensbezogene und wichtige Workloads, Sicherung, Big Data, Azure als DR-Standort |
| **SLA** |[SLA](https://azure.microsoft.com/support/legal/sla/) |[SLA](https://azure.microsoft.com/support/legal/sla/) |[SLA](https://azure.microsoft.com/support/legal/sla/) |
| **Preise** |[Preise](https://azure.microsoft.com/pricing/details/vpn-gateway/) |[Preise](https://azure.microsoft.com/pricing/details/vpn-gateway/) |[Preise](https://azure.microsoft.com/pricing/details/expressroute/) |
| **Technische Dokumentation** |[VPN Gateway-Dokumentation](https://azure.microsoft.com/documentation/services/vpn-gateway/) |[VPN Gateway-Dokumentation](https://azure.microsoft.com/documentation/services/vpn-gateway/) |[ExpressRoute-Dokumentation](https://azure.microsoft.com/documentation/services/expressroute/) |
| **HÄUFIG GESTELLTE FRAGEN** |[Häufig gestellte Fragen zum VPN-Gateway](../articles/vpn-gateway/vpn-gateway-vpn-faq.md) |[Häufig gestellte Fragen zum VPN-Gateway](../articles/vpn-gateway/vpn-gateway-vpn-faq.md) |[ExpressRoute – FAQ](../articles/expressroute/expressroute-faqs.md) |
