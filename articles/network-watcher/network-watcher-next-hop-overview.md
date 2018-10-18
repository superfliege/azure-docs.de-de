---
title: Einführung in Nächster Hop in Azure Network Watcher | Microsoft Docs
description: Dieser Artikel enthält eine Übersicht über die Network Watcher-Funktion „Nächster Hop“.
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: febf7bca-e0b7-41d5-838f-a5a40ebc5aac
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 28eacdce922e26d391cf34f78cb03ead9c6887a1
ms.sourcegitcommit: 794bfae2ae34263772d1f214a5a62ac29dcec3d2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2018
ms.locfileid: "44391265"
---
# <a name="use-next-hop-to-diagnose-virtual-machine-routing-problems"></a>Diagnostizieren Sie mit „Nächster Hop“ VM-Routingprobleme.

Der Datenverkehr wird von einem virtuellen Computer (VM) basierend auf den gültigen Routen, die einer Netzwerkschnittstelle zugeordnet sind, an ein Ziel gesendet. „Nächster Hop“ ruft den Typ und die IP-Adresse des nächsten Hops eines Pakets von einer angegebenen VM und der Netzwerkschnittstelle ab. Wenn Sie den nächsten Hop kennen, können Sie leichter festzustellen, ob Datenverkehr an das beabsichtigte Ziel geleitet wird oder nicht. Eine falsche Konfiguration von Routen, bei der Datenverkehr an einen lokalen Standort oder ein virtuelles Gerät geleitet wird, kann zu Verbindungsproblemen führen. Nächster Hop gibt auch die Routentabelle, die dem nächsten Hop zugeordnet ist, zurück. Wenn die Route als benutzerdefinierte Route definiert ist, wird diese Route zurückgegeben. Andernfalls gibt „Nächster Hop“ die **Systemroute** zurück.

![Übersicht über Nächster Hop](./media/network-watcher-next-hop-overview/figure1.png)

Die nächsten Hops, die durch die nächste Hop-Funktion zurückgegeben werden könnten, lauten wie folgt:

* Internet
* VirtualAppliance
* VirtualNetworkGateway
* VirtualNetwork
* VirtualNetworkPeering
* VirtualNetworkServiceEndpoint 
* MicrosoftEdge
* Keine

Weitere Informationen zu jedem Typ des nächsten Hops finden Sie unter [Routing von Datenverkehr für virtuelle Netzwerke](../virtual-network/virtual-networks-udr-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).

## <a name="next-steps"></a>Nächste Schritte

Wie Sie mit dem nächsten Hop VM-Netzwerkroutingprobleme diagnostizieren, erfahren Sie unter [Tutorial: Diagnostizieren von VM-Netzwerkroutingproblemen mit dem Azure-Portal](diagnose-vm-network-routing-problem.md).
