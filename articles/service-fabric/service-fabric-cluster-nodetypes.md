---
title: Azure Service Fabric-Knotentypen und VM-Skalierungsgruppen | Microsoft-Dokumentation
description: Erfahren Sie, wie Azure Service Fabric-Knotentypen mit VM-Skalierungsgruppen in Zusammenhang stehen und wie eine Remoteverbindung mit einer Skalierungsgruppeninstanz oder einem Clusterknoten hergestellt wird.
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: ''
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/23/2018
ms.author: chackdan
ms.openlocfilehash: 84d7f407781f09fed4667a22f0a46bc72c6e02a9
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2018
---
# <a name="azure-service-fabric-node-types-and-virtual-machine-scale-sets"></a>Azure Service Fabric-Knotentypen und VM-Skalierungsgruppen
[VM-Skalierungsgruppen](/azure/virtual-machine-scale-sets) sind eine Azure Computeressource. Sie können Skalierungsgruppen verwenden, um eine Sammlung virtueller Computer als Gruppe bereitzustellen und zu verwalten. Jeder Knotentyp, den Sie in einem Azure Service Fabric-Cluster definieren, richtet eine separate Skalierung ein.  Die Service Fabric-Runtime ist auf allen virtuellen Computern der Skalierungsgruppe installiert. Sie können jeden Knotentyp einzeln zentral hoch- oder herunterskalieren, auf jedem Clusterknoten die ausgeführte Betriebssystem-SKU ändern, bei jedem Typ unterschiedliche Portgruppen öffnen und verschiedene Kapazitätsmetriken verwenden.

Die folgende Abbildung zeigt einen Cluster mit den beiden Knotentypen FrontEnd und BackEnd. Jeder Knotentyp verfügt über fünf Knoten.

![Ein Cluster mit zwei Knotentypen][NodeTypes]

## <a name="map-virtual-machine-scale-set-instances-to-nodes"></a>Zuordnen von VM-Skalierungsgruppeninstanzen zu Knoten
Wie in der Abbildung oben gezeigt, beginnen Skalierungsgruppeninstanzen mit der Instanz 0. Die Instanznummer wird dann jeweils um 1 erhöht. Die Knotennamen spiegeln die Nummerierung wider. Der Knoten „BackEnd_0“ ist z.B. die Instanz 0 der Skalierungsgruppe „BackEnd“. Diese bestimmte Skalierungsgruppe hat fünf Instanzen mit den Namen „BackEnd_0“, „BackEnd_1“, „BackEnd_2“, „BackEnd_3“ und „BackEnd_4“.

Wenn Sie eine Skalierungsgruppe zentral hochskalieren, wird eine neue Instanz erstellt. Der neue Skalierungsgruppen-Instanzname hat in der Regel folgendes Format: Skalierungsgruppenname + Nummer der nächsten Instanz. In diesem Beispiel lautet er „BackEnd_5“.

## <a name="map-scale-set-load-balancers-to-node-types-and-scale-sets"></a>Zuordnen des Lastenausgleichs von Skalierungsgruppen zu Knotentypen und Skalierungsgruppen
Wenn Sie Ihren Cluster aus dem Azure-Portal bereitgestellt oder die Azure Resource Manager-Beispielvorlage verwendet haben, werden alle Ressourcen aufgelistet, die zu einer Ressourcengruppe gehören. Der Lastenausgleich jeder Skalierungsgruppe oder jedes Knotentyps wird angezeigt. Für den Namen des Lastenausgleichs wird folgendes Format verwendet: **LB-&lt;Knotentypname&gt;**. Ein Beispiel ist „LB-sfcluster4doc-0“, wie in der folgenden Abbildung gezeigt:

![angeben][Resources]


## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen finden Sie unter [Übersicht über das Feature „Deploy Anywhere“ (umgebungsunabhängige Bereitstellung) und ein Vergleich mit Clustern, die von Azure verwaltet werden](service-fabric-deploy-anywhere.md).
* Erfahren Sie mehr über [Clustersicherheit](service-fabric-cluster-security.md).
* Stellen Sie eine [Remoteverbindung](service-fabric-cluster-remote-connect-to-azure-cluster-node.md) mit der spezifischen Skalierungsgruppeninstanz her.
* [Aktualisieren der RDP-Portbereichswerte](./scripts/service-fabric-powershell-change-rdp-port-range.md) für Cluster-VMs nach der Bereitstellung
* [Ändern von Administratorbenutzername und -kennwort](./scripts/service-fabric-powershell-change-rdp-user-and-pw.md) für Cluster-VMs

<!--Image references-->
[NodeTypes]: ./media/service-fabric-cluster-nodetypes/NodeTypes.png
[Resources]: ./media/service-fabric-cluster-nodetypes/Resources.png
[InboundNatPools]: ./media/service-fabric-cluster-nodetypes/InboundNatPools.png
