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
ms.openlocfilehash: 5de227e5de5ef9b41f6e0f64db86b7195259f7d6
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "30196775"
---
* **PolicyBased:** PolicyBased-VPNs wurden im klassischen Bereitstellungsmodell bisher als Gateways mit statischem Routing bezeichnet. Bei richtlinienbasierten VPNs werden Pakete verschlüsselt und durch IPsec-Tunnel geleitet. Grundlage hierfür sind die IPsec-Richtlinien, die jeweils per Kombination aus Adresspräfixen zwischen Ihrem lokalen Netzwerk und dem Azure-VNet konfiguriert werden. Die Richtlinie (auch Datenverkehrsselektor genannt) wird in der Regel als Zugriffsliste in der VPN-Gerätekonfiguration definiert. Der Wert für einen PolicyBased-VPN-Typ lautet *PolicyBased*. Wenn Sie ein PolicyBased-VPN verwenden, beachten Sie die folgenden Einschränkungen:
  
  * PolicyBased-VPNs können **nur** in der Basic-Gateway-SKU verwendet werden. Dieser VPN-Typ ist nicht mit anderen Gateway-SKUs kompatibel.
  * Bei Verwendung eines PolicyBased-VPNs können Sie nur einen einzigen Tunnel verwenden.
  * Für S2S-Verbindungen können Sie nur PolicyBased-VPNs verwenden, und auch dies nur bei bestimmten Konfigurationen. Die meisten VPN-Gatewaykonfigurationen erfordern ein RouteBased-VPN.
* **RouteBased**: RouteBased-VPNs wurden im klassischen Bereitstellungsmodell bisher als Gateways mit dynamischem Routing bezeichnet. Bei RouteBased-VPNs werden Pakete auf der Grundlage der Routen der IP-Weiterleitungstabelle oder -Routingtabelle an die entsprechenden Tunnelschnittstellen weitergeleitet. An den Tunnelschnittstellen werden die Pakete dann ver- bzw. entschlüsselt. Die Richtlinie (bzw. der Datenverkehrsselektor) für RouteBased-VPNs wird im Any-to-Any-Format (bzw. als Platzhalter) konfiguriert. Der Wert für einen RouteBased-VPN-Typ lautet *RouteBased*.
