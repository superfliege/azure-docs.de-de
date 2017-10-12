---
title: "Netzwerk für VM-Konnektivität nach einem Failover zu Azure mit Azure Site Recovery | Microsoft-Dokumentation"
description: "Leitfaden für Netzwerke für die Verbindung mit Azure-VMs nach einem Failover von lokalen Computern mit Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: f02cdbea-0940-48bf-9fa5-f38d9e584fae
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 09/12/2017
ms.author: raynew
ms.openlocfilehash: 01c8e664465350b9dd382502c65cc3fda350797c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="networking-for-vm-connectivity-after-failover"></a>Netzwerk für die VM-Konnektivität nach einem Failover

In diesem Artikel werden die Netzwerkanforderungen für die Verbindung mit Azure-VMs erläutert, nachdem der Service [Azure Site Recovery](site-recovery-overview.md) für die Replikation und das Failover zu Azure verwendet wird.

In diesem Artikel lernen Sie Folgendes:

> [!div class="checklist"]
> * Die Verbindungsmethoden, die Sie verwenden können
> * Wie Sie eine andere IP-Adresse für replizierte Azure-VMs verwenden
> * Wie Sie nach einem Failover IP-Adressen für Azure-VMs beibehalten

## <a name="connecting-to-replica-vms"></a>Herstellen einer Verbindung mit Replikat-VMs

Bei der Planung der Replikation und Failoverstrategie ist eine der wichtigsten Fragen, wie eine Verbindung zum virtuellen Azure-Computer nach einem Failover hergestellt werden kann. Es gibt verschiedene Auswahlmöglichkeiten beim Entwerfen der Netzwerkstrategie für die virtuellen Azure-Replikatcomputer.

- **Verwenden einer anderen IP-Adresse**: Sie können sich entscheiden, einen anderen IP-Adressbereich für das replizierte Netzwerk des virtuellen Azure-Computers zu verwenden. In diesem Szenario erhält der virtuelle Computer eine neue IP-Adresse nach einem Failover. Dazu ist eine DNS-Aktualisierung erforderlich.
- **Behalten derselben IP-Adresse**: Sie möchten möglicherweise für das Azure-Netzwerk nach dem Failover denselben IP-Adressbereich verwenden, der sich an Ihrem lokalen Primärstandort befindet. Das Verwenden derselben IP-Adressen vereinfacht die Wiederherstellung, indem Probleme im Zusammenhang mit dem Netzwerk nach dem Failover reduziert werden. Wenn Sie in Azure replizieren, müssen Sie Routen mit dem neuen Speicherort der IP-Adressen nach dem Failover aktualisieren. 

## <a name="retaining-ip-addresses"></a>Beibehalten der IP-Adressen

Site Recovery bietet die Möglichkeit, feste IP-Adressen beizubehalten, wenn ein Failover in Azure mit einem Subnetzfailover erfolgt.

- Mit einem Subnetzfailover existiert ein bestimmtes Subnetz an Standort 1 oder 2, jedoch nie gleichzeitig an beiden Standorten.
- Um den IP-Adressraum im Falle eines Failovers beizubehalten, sorgen Sie programmgesteuert dafür, dass die Routerinfrastruktur die Subnetze von einem Standort an einen anderen verschiebt.
- Die Subnetze verschieben sich während des Failovers mit den zugeordneten geschützten virtuellen Computern. Der größte Nachteil ist, dass Sie im Fall eines Fehlers das gesamte Subnetz verschieben müssen.


### <a name="failover-example"></a>Beispiel für ein Failover

Sehen wir uns mithilfe eines fiktiven Unternehmen, Woodgrove Bank, ein Beispiel für ein Failover zu Azure an.

- Woodgrove Bank hostet die Business-Apps an einem lokalen Standort. Sie hosten ihre mobilen Apps in Azure.
- Es ist eine Standort-zu-Standort-VPN-Konnektivität zwischen deren lokalen Edgenetzwerk und dem virtuellen Azure-Netzwerk vorhanden. Aufgrund dieser VPN-Konnektivität erscheint das virtuelle Netzwerk in Azure als Erweiterung des lokalen Netzwerks.
- Woodgrove möchte die lokalen Workloads in Azure mit Site Recovery replizieren.
 - Woodgrove verfügt über Apps, die von hartcodierten IP-Adressen abhängig sind, daher müssen die IP-Adressen der Apps nach einem Failover zu Azure beibehalten werden.
 - In Azure ausgeführte Ressourcen verwenden den IP-Adressbereich 172.16.1.0/24, 172.16.2.0/24.

![Vor dem Subnetzfailover](./media/site-recovery-network-design/network-design7.png)

**Infrastruktur vor dem Failover**


Da Woodgrove in der Lage sein muss, seine virtuellen Computer zu Azure zu replizieren und gleichzeitig die IP-Adressen beizubehalten, muss das Unternehmen folgende Schritte ausführen:


1. Erstellen eines virtuellen Azure-Netzwerks, in dem die Azure-VMs nach einem Failover der lokalen Computer erstellt werden. Dies sollte eine Erweiterung des lokalen Netzwerks sein, sodass für Anwendungen ein nahtloses Failover ausgeführt werden kann.
2. In Site Recovery weisen sie den Computereigenschaften vor dem Failover die gleiche IP-Adresse zu. Nach dem Failover weist Site Recovery diese Adresse der Azure-VM zu.
3. Nachdem das Failover ausgeführt wird und die Azure-VMs mit der gleichen IP-Adresse erstellt wurden, verbinden sie sich mithilfe einer [VNet-zu-VNet-Verbindung](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) mit dem Netzwerk. Für diese Aktion kann ein Skript erstellt werden.
4. Sie müssen Routen ändern, um darauf zu reagieren, dass 192.168.1.0/24 sich jetzt in Azure befindet.


**Infrastruktur nach einem Failover**

![Nach dem Subnetzfailover](./media/site-recovery-network-design/network-design9.png)

#### <a name="site-to-site-connection"></a>Standort-zu-Standort-Verbindung

Nach einem Failover kann Woodgrove zusätzlich zu einer VNet-zu-VNet-Verbindung eine Standort-zu-Standort-VPN-Konnektivität einrichten:
- Beim Einrichten einer Standort-zu-Standort-Verbindung im Azure-Netzwerk können Sie nur dann Datenverkehr an den lokalen Standort (local-ntwork) weiterleiten, wenn sich der IP-Adressbereich vom lokalen IP-Adressbereich unterscheidet. Der Grund dafür ist, dass Azure gestreckte Subnetze nicht unterstützt. Sie können also im Azure-Netzwerk das lokale Netzwerk 192.168.1.0/24 nicht hinzufügen, wenn Sie über das lokale Subnetz 192.168.1.0/24 verfügen. Dies ist zu erwarten, da Azure nicht weiß, dass keine aktiven Computer im Subnetz vorhanden sind und dass das Subnetz nur für die Notfallwiederherstellung erstellt wird.
- Es dürfen keine Konflikte zwischen den Subnetzen im Netzwerk und dem lokalen Netzwerk vorliegen, damit ein korrekten Netzwerkdatenverkehr aus einem Azure-Netzwerk gewährleistet wird.




## <a name="assigning-new-ip-addresses"></a>Zuweisen von neuen IP-Adressen

In diesem [Blogbeitrag](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) wird erläutert, wie die Azure-Netzwerkinfrastruktur festgelegt wird, wenn Sie IP-Adressen nach dem Failover nicht beibehalten müssen. Es wird mit der Beschreibung einer Anwendung begonnen, danach wird die Erstellung eines lokalen Netzwerks in Azure erklärt, und schließlich finden Sie Informationen zum Ausführen von Failoverszenarios. 

## <a name="next-steps"></a>Nächste Schritte
[Ausführen eines Failovers](site-recovery-failover.md)




