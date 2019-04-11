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
ms.openlocfilehash: ee987448f9f6d95e6f97317bec2f696e734cba20
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2019
ms.locfileid: "58632801"
---
#### <a name="expressroute-limits"></a>Einschränkungen für ExpressRoute
Für Azure ExpressRoute-Ressourcen gelten pro Abonnement die folgenden Einschränkungen.

| Ressource | Standard/maximales Limit |
| --- | --- |
| ExpressRoute-Verbindungen pro Abonnement |10 |
| ExpressRoute-Verbindungen pro Region und Abonnement mit Azure Resource Manager |10 |
| Maximale Anzahl von Routen für Private Peering in Azure mit ExpressRoute-Standard |4.000 |
| Maximale Anzahl von Routen für Private Peering in Azure mit ExpressRoute-Premium-Add-on |10.000 |
| Maximale Anzahl von Routen für privates Azure-Peering aus dem VNet-Adressraum für eine ExpressRoute-Verbindung |200 | 
| Maximale Anzahl von Routen für Microsoft Azure-Peering mit ExpressRoute-Standard |200 |
| Maximale Anzahl von Routen für Peering in Microsoft Azure mit ExpressRoute-Premium-Add-on |200 |
| Maximale Anzahl von ExpressRoute-Verbindungen, die mit dem gleichen virtuellen Netzwerk an unterschiedlichen Peeringstandorten verknüpft sind |4 |
| Anzahl der pro ExpressRoute-Leitung zulässigen virtuellen Netzwerkverbindungen |Siehe hierzu die folgende Tabelle. |

#### <a name="number-of-virtual-networks-per-expressroute-circuit"></a>Anzahl der virtuellen Netzwerke pro ExpressRoute-Verbindung
| **Verbindungsgröße** | **Anzahl der Verknüpfungen eines virtuellen Netzwerks für Standard** | **Anzahl der Verknüpfungen eines virtuellen Netzwerks mit Premium-Add-on** |
| --- | --- | --- |
| 50 MBit/s |10 |20 |
| 100 MBit/s |10 |25 |
| 200 MBit/s |10 |25 |
| 500 MBit/s |10 |40 |
| 1 GBit/s |10 |50 |
| 2 GBit/s |10 |60 |
| 5 GBit/s |10 |75 |
| 10 GBit/s |10 |100 |

