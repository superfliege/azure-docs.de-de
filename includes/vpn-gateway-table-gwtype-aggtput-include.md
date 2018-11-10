---
title: Includedatei
description: Includedatei
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/29/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 03af5efcd4a37203a82db503f8bc602b33de734d
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2018
ms.locfileid: "50227133"
---
|**SKU**   | **S2S/VNet-zu-VNet-<br>Tunnels** | **P2S<br> SSTP-Verbindungen** | **P2S<br> IKEv2-Verbindungen** | **Benchmark für<br>aggregierten Durchsatz** |
|---       | ---        | ---       | ---            | ---       |
|**VpnGw1**| Maximal 30*   | Maximal 128  | Maximal 250       | 650 MBit/s  |
|**VpnGw2**| Maximal 30*   | Maximal 128  | Maximal 500       | 1 GBit/s    |
|**VpnGw3**| Maximal 30*   | Maximal 128  | Maximal 1000      | 1,25 GBit/s |
|**Basic** | Maximal 10    | Maximal 128  | Nicht unterstützt  | 100 MBit/s  | 

(\*) Verwenden Sie [Virtual WAN](../articles/virtual-wan/virtual-wan-about.md), wenn Sie mehr als 30 S2S-VPN-Tunnel benötigen.

* Der Benchmark für den aggregierten Durchsatz für ein VPN-Gateway besteht aus einer Kombination aus S2S und P2S. **Bei zahlreichen P2S-Verbindungen kann eine S2S-Verbindung aufgrund von Durchsatzeinschränkungen beeinträchtigt sein.** Der Benchmark für den aggregierten Durchsatz basiert auf Messungen von mehreren, durch ein Gateway aggregierten Tunneln. Aufgrund der Bedingungen für den Internetdatenverkehr und dem Verhalten Ihrer Anwendung kann der Durchsatz nicht garantiert werden.

* Diese Verbindungsgrenzwerte sind voneinander getrennt. Sie können beispielsweise 128 SSTP-Verbindungen und 250 IKEv2-Verbindungen in der SKU „VpnGw1“ nutzen.

* Informationen zu den Preisen finden Sie auf der Seite [Preise](https://azure.microsoft.com/pricing/details/vpn-gateway) .

* Informationen zum SLA (Vereinbarung zum Servicelevel) finden Sie auf der [SLA](https://azure.microsoft.com/support/legal/sla/vpn-gateway/)-Seite.

* VpnGw1, VpnGw2 und VpnGw3 werden nur für VPN-Gateways unterstützt, die das Resource Manager-Bereitstellungsmodell verwenden.
