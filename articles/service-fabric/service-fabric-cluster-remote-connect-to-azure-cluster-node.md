---
title: Herstellen einer Remoteverbindung mit einem Azure Service Fabric-Clusterknoten | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie eine Remoteverbindung mit einer Skalierungsgruppeninstanz (Service Fabric-Clusterknoten) herstellen.
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: ''
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/23/2018
ms.author: chackdan
ms.openlocfilehash: 8c7d5446429089a0fc931175b55e81e1ad0c97a0
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="remote-connect-to-a-virtual-machine-scale-set-instance-or-a-cluster-node"></a>Herstellen einer Remoteverbindung mit einer VM-Skalierungsgruppeninstanz oder einem Clusterknoten
In einem Service Fabric-Cluster unter Azure richtet jeder Clusterknotentyp, den Sie definieren, [eine separate VM-Skalierungsgruppe](service-fabric-cluster-nodetypes.md) ein.  Sie können eine Remoteverbindung mit bestimmten Skalierungsgruppeninstanzen (oder Clusterknoten) herstellen.  Im Gegensatz zu Einzelinstanz-VMs besitzen Skalierungsgruppeninstanzen keine eigenen virtuellen IP-Adressen. Daher kann es schwierig sein, nach einer IP-Adresse und einem Port zum Herstellen einer Remoteverbindung mit einer bestimmten Instanz zu suchen.

Um eine IP-Adresse und einen Port für eine Remoteverbindung mit einer bestimmten Instanz zu finden, führen Sie die folgenden Schritte aus.

1. Ermitteln Sie die virtuelle IP-Adresse für den Knotentyp, indem Sie die NAT-Eingangsregeln für das Remotedesktopprotokoll (RDP) abrufen.

    Rufen Sie zunächst die NAT-Eingangsregelwerte ab, die als Teil der Ressourcendefinition für `Microsoft.Network/loadBalancers` definiert wurden.
    
    Wählen Sie im Azure-Portal auf der Seite für den Lastenausgleich **Einstellungen** > **NAT-Eingangsregeln** aus. Damit erhalten Sie die IP-Adresse und den Port zum Herstellen einer Remoteverbindung mit der ersten Skalierungsgruppeninstanz. 
    
    ![Load Balancer][LBBlade]
    
    In der folgenden Abbildung lautet die IP-Adresse **104.42.106.156** und der Port **3389**.
    
    ![NAT-Regeln][NATRules]

2. Ermitteln Sie den Port, über den eine Remoteverbindung mit der spezifischen Skalierungsgruppeninstanz bzw. mit dem spezifischen Knoten hergestellt werden kann.

    Skalierungsgruppeninstanzen sind Knoten zugeordnet. Verwenden Sie die Informationen zur Skalierungsgruppe, um den zu verwendenden Port zu ermitteln.
    
    Die Zuweisung der Ports erfolgt in der aufsteigenden Reihenfolge gemäß der Skalierungsgruppeninstanz. Im obigen Beispiel für den Knotentyp „FrontEnd“ enthält die folgende Tabelle die Ports der fünf Knoteninstanzen. Wenden Sie dieselbe Zuordnung auf Ihre Skalierungsgruppeninstanz an.
    
    | **VM-Skalierungsgruppeninstanz** | **Port** |
    | --- | --- |
    | FrontEnd_0 |3389 |
    | FrontEnd_1 |3390 |
    | FrontEnd_2 |3391 |
    | FrontEnd_3 |3392 |
    | FrontEnd_4 |3393 |
    | FrontEnd_5 |3394 |

3. Stellen Sie eine Remoteverbindung mit der spezifischen Skalierungsgruppeninstanz her.

    Die folgende Abbildung veranschaulicht das Herstellen einer Remotedesktopverbindung mit der Skalierungsgruppeninstanz „FrontEnd_1“:
    
    ![Remotedesktopverbindung][RDP]


Lesen Sie als Nächstes die folgenden Artikel:
* Weitere Informationen finden Sie unter [Übersicht über das Feature „Deploy Anywhere“ (umgebungsunabhängige Bereitstellung) und ein Vergleich mit Clustern, die von Azure verwaltet werden](service-fabric-deploy-anywhere.md).
* Erfahren Sie mehr über [Clustersicherheit](service-fabric-cluster-security.md).
* [Aktualisieren der RDP-Portbereichswerte](./scripts/service-fabric-powershell-change-rdp-port-range.md) für Cluster-VMs nach der Bereitstellung
* [Ändern von Administratorbenutzername und -kennwort](./scripts/service-fabric-powershell-change-rdp-user-and-pw.md) für Cluster-VMs

<!--Image references-->
[LBBlade]: ./media/service-fabric-cluster-remote-connect-to-azure-cluster-node/LBBlade.png
[NATRules]: ./media/service-fabric-cluster-remote-connect-to-azure-cluster-node/NATRules.png
[RDP]: ./media/service-fabric-cluster-remote-connect-to-azure-cluster-node/RDP.png
