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
ms.openlocfilehash: c9457e51858d4a073d8baffdd435c8100d95d566
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/05/2018
---
|**SKU**   | **S2S/VNet-zu-VNet-<br>Tunnels** | **P2S-<br>Verbindungen** | **Benchmark für<br>aggregierten Durchsatz** |
|---       | ---                             | ---                    | ---                         |
|**VpnGw1**| Maximal 30                         | Maximal 128               | 650 MBit/s                    |
|**VpnGw2**| Maximal 30                         | Maximal 128               | 1 GBit/s                      |
|**VpnGw3**| Maximal 30                         | Maximal 128               | 1,25 GBit/s                   |
|**Basic** | Maximal 10                         | Maximal 128               | 100 MBit/s                    | 
|          |                                 |                        |                             | 

- Der Benchmark für den aggregierten Durchsatz basiert auf Messungen von mehreren, durch ein Gateway aggregierten Tunneln. Aufgrund der Bedingungen für den Internetdatenverkehr und dem Verhalten Ihrer Anwendung kann der Durchsatz nicht garantiert werden.

- Informationen zu den Preisen finden Sie auf der Seite [Preise](https://azure.microsoft.com/pricing/details/vpn-gateway) .

- Informationen zum SLA (Vereinbarung zum Servicelevel) finden Sie auf der [SLA](https://azure.microsoft.com/support/legal/sla/vpn-gateway/)-Seite.

- VpnGw1, VpnGw2 und VpnGw3 werden nur für VPN-Gateways unterstützt, die das Resource Manager-Bereitstellungsmodell verwenden.