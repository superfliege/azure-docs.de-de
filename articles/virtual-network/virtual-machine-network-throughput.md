---
title: Netzwerkdurchsatz virtueller Azure-Computer | Microsoft-Dokumentation
description: Informationen zum Netzwerkdurchsatz virtueller Azure-Computer.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/13/2017
ms.author: jdial
ms.openlocfilehash: 536a779d7de51180aa6410911dea2b6c47780c2f
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2017
---
# <a name="virtual-machine-network-throughput"></a>Netzwerkdurchsatz virtueller Computer

Azure bietet eine Vielzahl von Größen und Typen virtueller Computer mit einer jeweils unterschiedlichen Kombination von Leistungsaspekten. Zu diesen Leistungsaspekten gehört der Netzwerkdurchsatz (oder Bandbreite), der in Megabit pro Sekunde (Mbit/s) gemessen wird. Da virtuelle Computer auf gemeinsam genutzter Hardware gehostet werden, muss die Netzwerkkapazität zwischen den virtuellen Computern, die dieselbe Hardware verwenden, gerecht aufgeteilt werden. Größeren virtuellen Computern wird verhältnismäßig mehr Bandbreite als kleineren virtuellen Computern zugeordnet.
 
Die Netzwerkbandbreite, die dem jeweiligen virtuellen Computer zugeordnet ist, wird anhand des ausgehenden Datenverkehrs vom virtuellen Computer gemessen. Der gesamte Netzwerkdatenverkehr, der vom virtuellen Computer ausgeht, wird unabhängig vom Ziel auf den zugewiesenen Grenzwert angerechnet. Wenn der Grenzwert eines virtuellen Computers beispielsweise bei 1.000 MBit/s liegt, gilt dieser Grenzwert unabhängig davon, ob das Ziel des ausgehenden Datenverkehrs ein anderer virtueller Computer in demselben virtuellen Netzwerk ist oder ob es sich außerhalb von Azure befindet.
 
Der eingehende Datenverkehr wird nicht gemessen oder direkt beschränkt. Es gibt jedoch andere Faktoren, wie z. B. CPU- und Speichergrenzwerte, die sich auf die Fähigkeit eines virtuellen Computers zur Verarbeitung eingehender Daten auswirken können.

[Beschleunigter Netzwerkbetrieb](virtual-network-create-vm-accelerated-networking.md) ist ein Feature, das der Verbesserung der Netzwerkleistung, einschließlich Latenz, Durchsatz und CPU-Auslastung, dient. Zwar kann durch den beschleunigten Netzwerkbetrieb der Durchsatz eines virtuellen Computers verbessert werden, doch ist dies nur im Rahmen der Bandbreite möglich, die dem virtuellen Computer zugewiesen ist.
 
Virtuellen Azure-Computern muss eine Netzwerkschnittstelle angefügt sein, doch können es auch mehrere sein. Die einem virtuellen Computer zugewiesene Bandbreite umfasst die Summe des gesamten ausgehenden Datenverkehrs aller Netzwerkschnittstellen, die an einen virtuellen Computer angefügt sind. Anders ausgedrückt: Die zugewiesene Bandbreite gilt pro virtuellem Computer, unabhängig davon, wie viele Netzwerkschnittstellen an den virtuellen Computer angefügt sind. Informationen dazu, wie viele Netzwerkschnittstellen von virtuellen Azure-Computern verschiedener Größen unterstützt werden, finden Sie unter den Größen für virtuelle [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json)- und [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json)-Computer in Azure. 

## <a name="expected-network-throughput"></a>Erwartete Netzwerkdurchsatz

Der erwartete ausgehende Durchsatz und die Anzahl der Netzwerkschnittstellen, die von der jeweiligen Größe des virtuellen Computers unterstützt werden, ist unter den Größen für virtuelle [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json)- und [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json)-Computer in Azure angegeben. Wählen Sie einen Typ aus (z. B. „Allgemeiner Zweck“), und wählen Sie dann auf der daraufhin angezeigten Seite eine Größenserie aus (z. B. die Dv2-Serie). Jede Serie weist eine Tabelle mit Netzwerkspezifikationen in der letzten Spalte mit dem Titel **Maximale Anzahl NICs/Erwartete Netzwerkbandbreite (Mbps)** auf. 

Die Durchsatzbegrenzung gilt für den virtuellen Computer. Der Durchsatz wird durch folgende Faktoren nicht beeinflusst:
- **Anzahl der Netzwerkschnittstellen**: Der Bandbreitengrenzwert ist ein kumulativer Wert des gesamten ausgehenden Datenverkehrs vom virtuellen Computer.
- **Beschleunigter Netzwerkbetrieb**: Obwohl das Feature beim Erreichen des veröffentlichten Grenzwerts hilfreich sein kann, ändert sich dadurch der Grenzwert nicht.
- **Datenverkehrsziel**: Alle Ziele werden auf den Grenzwert für ausgehenden Datenverkehr angerechnet.
- **Protokoll**: Der gesamte ausgehende Datenverkehr über alle Protokolle wird auf den Grenzwert angerechnet.

## <a name="next-steps"></a>Nächste Schritte

- [Optimieren des Netzwerkdurchsatzes für das Betriebssystem eines virtuellen Computers](virtual-network-optimize-network-bandwidth.md)
- [Testen des Netzwerkdurchsatzes](virtual-network-bandwidth-testing.md) für einen virtuellen Computer