---
title: Remoting eines Knotentyps in Azure Service Fabric | Microsoft-Dokumentation
description: Erfahren Sie, wie ein Knotentyp in Azure Service Fabric entfernt wird.
services: service-fabric
documentationcenter: .net
author: v-steg
manager: JeanPaul.Connick
editor: vturecek
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/26/2018
ms.author: v-steg
ms.openlocfilehash: 3704a356763b16a30285baee1aabffdd3aa3f8aa
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/02/2019
ms.locfileid: "53981619"
---
# <a name="remove-a-service-fabric-node-type"></a>Entfernen eines Service Fabric-Knotentyps

Verwenden Sie [Remove-AzureRmServiceFabricNodeType](https://docs.microsoft.com/powershell/module/azurerm.servicefabric/remove-azurermservicefabricnodetype), um einen Service Fabric-Knotentyp zu entfernen.

Die folgenden beiden Vorgänge treten beim Aufruf von Remove-AzureRmServiceFabricNodeType auf:
1.  Die VM-Skalierungsgruppe (Virtual Machine Scale Set, VMSS) hinter dem Knotentyp wird gelöscht.
2.  Für alle Knoten in diesen Knotentyp wird der gesamte Zustand für diesen Knoten aus dem System entfernt. Wenn Dienste auf diesem Knoten vorhanden sind, werden sie zuerst auf einen anderen Knoten verschoben. Wenn der Cluster-Manager keinen Knoten für das Replikat bzw. den Dienst finden kann, wird der Vorgang verzögert/blockiert.

> [!NOTE]
> Es wird nicht empfohlen, regelmäßig Remove-AzureRmServiceFabricNodeType zum Entfernen eines Knotentyps aus einem Produktionscluster zu verwenden. In diesem Fall handelt es sich um einen sehr gefährlichen Befehl, da er im Grunde die VM-Skalierungsgruppenressource hinter dem Knotentyp löscht. Wenn Sie Remove-AzureRmServiceFabricNodeType aufrufen, kann das System nicht wissen, ob eine sichere Entfernung für Sie relevant ist. 

## <a name="durability-characteristics"></a>Dauerhaftigkeitsmerkmale
Bei der Verwendung von Remove-AzureRmServiceFabricNodeType wird die Sicherheit über die Geschwindigkeit gestellt. [Dauerhaftigkeitsmerkmale](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster) vom Typ „Silber“ oder „Gold“ werden aus den folgenden Gründen empfohlen:
- „Bronze“ gibt Ihnen keine Garantie für die Speicherung von Zustandsinformationen.
- Die Dauerhaftigkeit von Typ „Silber“ und „Gold“ fängt alle Änderungen an der Skalierungsgruppe ab.
- Mit „Gold“ erhalten Sie außerdem Kontrolle über die Azure-Updates, die im Hintergrund der Skalierungsgruppe erfolgen.

Service Fabric „orchestriert“ die zugrunde liegenden Änderungen und Updates, damit keine Daten verloren gehen. Wenn Sie einen Knoten mit der Dauerhaftigkeit „Bronze“ entfernen, können Sie jedoch Zustandsinformationen verlieren. Wenn Sie einen primären Knotentyp entfernen und Ihre Anwendung zustandslos ist, ist „Bronze“ akzeptabel. Wenn Sie zustandsbehaftete Workloads in der Produktion ausführen, sollte die Mindestkonfiguration „Silber“ sein. Analog dazu sollte für Produktionsszenarien der primären Knotentyp immer „Silber“ oder „Gold“ sein.

### <a name="more-about-bronze-durability"></a>Weitere Informationen zur Dauerhaftigkeitsstufe „Bronze“

Wenn Sie einen Knotentyp „Bronze“ entfernen, fallen alle Knoten im Knotentyp sofort aus. Service Fabric fängt keine Knotenupdates von VM-Skalierungsgruppen vom Typ „Bronze“ ab, daher fallen alle virtuellen Computer sofort aus. Wenn zustandsbehaftete Elemente auf diesen Knoten vorhanden waren, gehen die Daten verloren. Selbst als zustandslose Elemente nehmen alle Knoten in Service Fabric am Ring teil, sodass eine ganze Umgebung verloren gehen kann, was Auswirkungen auf den Cluster selbst haben könnte.

Anstatt einen einzelnen Knoten zu entfernen (denn theoretisch können Sie einen Knoten nach dem anderen entfernen), sollten Sie warten, bis die Replikate und Dienste verschoben wurden und sich das System stabilisiert hat, bevor Sie einen weiteren Knoten entfernen usw.  Wenn Sie jedoch mehrere Knoten gleichzeitig entfernen, kann Ihr Cluster ausfallen (da Service Fabric keine VMSS-Updates mit Bronze-Dauerhaftigkeit abfangen kann).

## <a name="recommended-node-type-removal-process"></a>Empfohlener Knotentypen-Entfernungsvorgang

So entfernen Sie den Knotentyp auf die sicherste und schnellste Weise:
1.  Wenn Sie Bronze-Dauerhaftigkeit verwenden oder nicht möchten, dass das System Anwendungen verschiebt, die Zustandsinformationen enthalten, die im Rahmen der Löschung des Knotentyps verloren gehen, entfernen Sie zuerst zustandsbehaftete Daten von den Knoten, die von der Entfernung des Knotentyps betroffen sein werden.
2.  Führen Sie [Remove-ServiceFabricNodeState](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricnodestate?view=azureservicefabricps) auf allen Knoten aus, die entfernt werden sollen.
3.  Führen Sie [Remove-AzureRmVmss](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#remove-vms-from-a-scale-set) für virtuelle Computer aus, die von der Entfernung des Knotentyps betroffen sind.
4. Führen Sie [Remove-AzureRmServiceFabricNodeType](https://docs.microsoft.com/powershell/module/azurerm.servicefabric/remove-azurermservicefabricnodetype) aus, um den Knotentyp zu entfernen.

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über die [Dauerhaftigkeitsmerkmale](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster) eines Clusters.
- Erfahren Sie mehr über [Knotentypen und VM-Skalierungsgruppen](service-fabric-cluster-nodetypes.md).
- Erfahren Sie mehr über [Service Fabric-Clusterskalierung](service-fabric-cluster-scaling.md).