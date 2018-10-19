---
title: Includedatei
description: Includedatei
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: include
ms.date: 06/12/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: ad75b333fc7d5d37a4ac074727e4d54b67bd0321
ms.sourcegitcommit: 616e63d6258f036a2863acd96b73770e35ff54f8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/14/2018
ms.locfileid: "45637193"
---
#### <a name="expressroute-limits"></a>Einschränkungen für ExpressRoute
Für ExpressRoute-Ressourcen gelten pro Abonnement die folgenden Einschränkungen.

| Ressource | Standard-/maximales Limit |
| --- | --- |
| ExpressRoute-Verbindungen pro Abonnement |10 |
| ExpressRoute-Verbindungen pro Region und Abonnement (Azure Resource Manager) |10 |
| Maximale Anzahl von Routen für Private Peering in Azure mit ExpressRoute-Standard |4.000 |
| Maximale Anzahl von Routen für Private Peering in Azure mit ExpressRoute-Premium-Add-on |10.000 |
| Maximale Anzahl von Routen für Microsoft-Peering in Azure mit ExpressRoute-Standard |200 |
| Maximale Anzahl von Routen für Microsoft-Peering in Azure mit ExpressRoute-Premium-Add-on |200 |
| Maximale Anzahl von ExpressRoute-Verbindungen, die mit dem gleichen virtuellen Netzwerk an unterschiedlichen Peeringstandorten verknüpft sind |4 |
| Anzahl der pro ExpressRoute-Leitung zulässigen virtuellen Netzwerkverbindungen |siehe Tabelle unten |

#### <a name="number-of-virtual-networks-per-expressroute-circuit"></a>Anzahl der virtuellen Netzwerke pro ExpressRoute-Verbindung
| **Verbindungsgröße** | **Anzahl der VNet-Links – Standard** | **Anzahl der VNet-Links – Premium-Add-on** |
| --- | --- | --- |
| 50 MBit/s |10 |20 |
| 100 MBit/s |10 |25 |
| 200 MBit/s |10 |25 |
| 500 MBit/s |10 |40 |
| 1 GBit/s |10 |50 |
| 2 GBit/s |10 |60 |
| 5 GBit/s |10 |75 |
| 10 GBit/s |10 |100 |

