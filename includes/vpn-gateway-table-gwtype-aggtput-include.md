---
title: Includedatei
description: Includedatei
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 08/02/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 4cbbb64489acf23c1248e35269e1441dd2a6878e
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/03/2018
ms.locfileid: "39514068"
---
|**SKU**   | **S2S/VNet-zu-VNet-<br>Tunnels** | **P2S-<br>Verbindungen** | **Benchmark für<br>aggregierten Durchsatz** |
|---       | ---                             | ---                    | ---                         |
|**VpnGw1**| Maximal 30*                         | Maximal 128\*\*             | 650 MBit/s                    |
|**VpnGw2**| Maximal 30*                         | Maximal 128\*\*             | 1 GBit/s                      |
|**VpnGw3**| Maximal 30*                         | Maximal 128\*\*             | 1,25 GBit/s                   |
|**Basic** | Maximal 10                         | Maximal 128               | 100 MBit/s                    | 

* (\*) Verwenden Sie [Virtual WAN](../articles/virtual-wan/virtual-wan-about.md), wenn Sie mehr als 30 S2S-VPN-Tunnel benötigen.

* (\*\*) Wenden Sie sich an den Support, wenn zusätzliche Verbindungen erforderlich sind. Dies gilt nur für IKEv2. Die Anzahl von Verbindungen für SSTP kann nicht erhöht werden.

* Der Benchmark für den aggregierten Durchsatz basiert auf Messungen von mehreren, durch ein Gateway aggregierten Tunneln. Aufgrund der Bedingungen für den Internetdatenverkehr und dem Verhalten Ihrer Anwendung kann der Durchsatz nicht garantiert werden.

* Informationen zu den Preisen finden Sie auf der Seite [Preise](https://azure.microsoft.com/pricing/details/vpn-gateway) .

* Informationen zum SLA (Vereinbarung zum Servicelevel) finden Sie auf der [SLA](https://azure.microsoft.com/support/legal/sla/vpn-gateway/)-Seite.

* VpnGw1, VpnGw2 und VpnGw3 werden nur für VPN-Gateways unterstützt, die das Resource Manager-Bereitstellungsmodell verwenden.