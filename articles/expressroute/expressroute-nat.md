---
title: "NAT-Anforderungen für ExpressRoute-Verbindungen | Microsoft Docs"
description: "Diese Seite enthält Informationen zu den Anforderungen für das Konfigurieren und Verwalten von NAT für ExpressRoute-Verbindungen."
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: 
ms.assetid: 867bf936-c851-485f-84c8-d8d6e33fee9f
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: cherylmc
ms.openlocfilehash: d3de566ff2825ef0c41d88d4a86157dc23d9f46b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="expressroute-nat-requirements"></a>ExpressRoute NAT-Anforderungen
Um Microsoft-Cloud-Diensten mithilfe von ExpressRoute eine Verbindung herstellen, müssen Sie einrichten und Verwalten von NATs. Einige Konnektivitätsanbietern bieten einrichten und Verwalten von NAT als verwalteter Dienst. Wenden Sie sich an Ihren Dienstanbieter, um festzustellen, ob sie einen solchen Dienst bieten. Wenn dies nicht der Fall ist, müssen Sie die unten beschriebenen Anforderungen entsprechen. 

Überprüfen Sie die [ExpressRoute-Verbindungen und Routingdomänen](expressroute-circuit-peerings.md) Seite, um einen Überblick über die verschiedenen Routingdomänen abzurufen. Um die öffentliche IP-Adresse für Öffentliche Azure und Microsoft-peering erfüllen, wird empfohlen, dass Sie NAT zwischen Ihrem Netzwerk und Microsoft einrichten. Dieser Abschnitt enthält eine ausführliche Beschreibung der NAT-Infrastruktur, die Sie einrichten möchten.

## <a name="nat-requirements-for-azure-public-peering"></a>NAT-Anforderungen für die öffentliche Azure-peering
Der öffentliche peeringpfad Azure können Sie auf alle Dienste, die in Azure gehostet wird, über die öffentlichen IP-Adressen zu verbinden. Dazu gehören Dienste aufgeführt, die der [ExpessRoute FAQ](expressroute-faqs.md) sowie alle Dienste, die durch ISVs in Microsoft Azure gehostet. 

> [!IMPORTANT]
> Konnektivität mit Microsoft Azure-Diensten für öffentliches peering wird immer mit dem Microsoft-Netzwerk aus Ihrem Netzwerk initiiert. Aus diesem Grund können Sitzungen von Microsoft Azure-Diensten mit Ihrem Netzwerk über ExpressRoute initiiert werden. Wenn versucht wird, werden Pakete, die für diese angekündigte IP-Adressen im Internet nicht ExpressRoute verwendet.
> 

Datenverkehr in Microsoft Azure für öffentliches peering muss gültige öffentliche IPv4-Adressen SNATed sein, bevor sie das Microsoft-Netzwerk erreichen. Die Abbildung unten zeigt ein Bild auf hoher Ebene wie das NAT-Gerät so eingerichtet werden konnte die oben genannten Voraussetzung zu erfüllen.

![](./media/expressroute-nat/expressroute-nat-azure-public.png) 

### <a name="nat-ip-pool-and-route-advertisements"></a>NAT IP-Pool und die Routenwerte Ankündigungen
Sie müssen sicherstellen, dass Datenverkehr den Azure öffentlichen peeringpfad mit gültige öffentliche IPv4-Adresse eingegeben ist. Microsoft muss den Besitz des IPv4-NAT-Adresspools für ein Land/Region routing Internet Registry (RIR) oder ein Internet-routing-Registrierung (IKV) validieren können. Basierend auf der AS-Nummer, die mit geprüft wird und die IP-Adressen für die NAT verwendet wird gesucht werden Finden Sie in der [ExpressRoute Routinganforderungen](expressroute-routing.md) Seite Informationen zu routing Registrierungen.

Es gibt keine Einschränkungen auf die Länge der NAT-IP-Adresspräfix, die über diese peering angekündigt. Sie müssen die NAT-Pool zu überwachen und stellen Sie sicher, dass Sie der NAT-Sitzungen nicht blockiert werden.

> [!IMPORTANT]
> Die NAT-IP-Adresspool an Microsoft angekündigt muss nicht mit dem Internet angekündigt werden. Dadurch wird die Verbindung mit anderen Microsoft-Diensten unterbrochen.
> 
> 

## <a name="nat-requirements-for-microsoft-peering"></a>NAT-Anforderungen für Microsoft-peering
Die Microsoft-peeringpfad können Sie die Verbindung mit Microsoft Cloud-Dienste, die nicht über den Azure öffentlichen peeringpfad unterstützt werden. Die Liste der Dienste enthält Office 365-Dienste wie Exchange Online, SharePoint Online, Skype for Business und Dynamics 365. Microsoft erwartet, die Microsoft beim Peering bidirektionale Konnektivität zu unterstützen. Datenverkehr an den Microsoft-Clouddienste muss gültige öffentliche IPv4-Adressen SNATed sein, bevor sie das Microsoft-Netzwerk erreichen. Datenverkehr für Ihr Netzwerk mit Microsoft Cloud-Diensten muss SNATed an Ihre Internet-Edge, um zu verhindern, dass [asymmetrischen routing](expressroute-asymmetric-routing.md). Die Abbildung unten zeigt ein Bild auf hoher Ebene wie die NAT-Setup für Microsoft-peering werden sollen.

![](./media/expressroute-nat/expressroute-nat-microsoft.png) 

### <a name="traffic-originating-from-your-network-destined-to-microsoft"></a>Datenverkehr, der aus Ihrem Netzwerk bestimmt ist, an Microsoft
* Sie müssen sicherstellen, dass Datenverkehr mit einer gültigen öffentlichen IPv4-Adresse den Microsoft-peeringpfad eintritt. Microsoft muss den Besitzer der IPv4-NAT-Adresspool für das Land/Region routing Internet Registry (RIR) oder ein Internet-routing-Registrierung (IKV) validieren können. Basierend auf der AS-Nummer, die mit geprüft wird und die IP-Adressen für die NAT verwendet wird gesucht werden Finden Sie in der [ExpressRoute Routinganforderungen](expressroute-routing.md) Seite Informationen zu routing Registrierungen.
* Für das Einrichten von Azure public peering und andere ExpressRoute-Verbindungen verwendete IP-Adressen müssen nicht an Microsoft über die BGP-Sitzung angekündigt werden. Es gibt keine Einschränkung für die Länge der NAT-IP-Adresspräfix, die über diese peering angekündigt.
  
  > [!IMPORTANT]
  > Die NAT-IP-Adresspool an Microsoft angekündigt muss nicht mit dem Internet angekündigt werden. Dadurch wird die Verbindung mit anderen Microsoft-Diensten unterbrochen.
  > 
  > 

### <a name="traffic-originating-from-microsoft-destined-to-your-network"></a>Datenverkehr, der von Microsoft, die bestimmt, mit dem Netzwerk
* Bestimmte Szenarien erfordern Microsoft an Dienstendpunkte, die in Ihrem Netzwerk gehostet Netzwerkkonnektivität zu initiieren. Ein typisches Beispiel für das Szenario wäre die Verbindung mit AD FS-Server in Ihrem Netzwerk von Office 365 gehostet. In solchen Fällen müssen Sie entsprechende Präfixe aus Ihrem Netzwerk in der Microsoft-peering preisgeben. 
* In Ihrem Netzwerk, um zu verhindern, müssen Sie SNAT Microsoft-Datenverkehr am Rand Internet für Dienstendpunkte [asymmetrischen routing](expressroute-asymmetric-routing.md). Anforderungen **und Antworten** mit einem Ziel werden IP, die eine Route, die über ExpressRoute empfangen entsprechen immer über ExpressRoute gesendet werden. Asymmetrische routing ist vorhanden, wenn die Anforderung, über das Internet mit der Antwort über ExpressRoute gesendet empfangen wird. SNATing der eingehende Datenverkehr von Microsoft am Rand Internet erzwingt Antwort Datenverkehr an den Internet-Edge Behebung des Problems.

![Asymmetrische routing mit ExpressRoute](./media/expressroute-asymmetric-routing/AsymmetricRouting2.png)

## <a name="next-steps"></a>Nächste Schritte
* Verweisen auf die Anforderungen für [Routing](expressroute-routing.md) und [QoS](expressroute-qos.md).
* Workflowinformationen finden Sie unter [ExpressRoute-circuit Bereitstellung Workflows und Zustände circuit](expressroute-workflows.md).
* Konfigurieren Sie Ihre ExpressRoute-Verbindung.
  
  * [Erstellen Sie eine ExpressRoute-Verbindung](expressroute-howto-circuit-classic.md)
  * [Konfigurieren des Routings](expressroute-howto-routing-classic.md)
  * [Verknüpfen Sie eine VNet mit einer ExpressRoute-Verbindung](expressroute-howto-linkvnet-classic.md)

