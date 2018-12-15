---
title: Zentrales Hoch- oder Herunterskalieren eines Service Fabric-Clusters | Microsoft Docs
description: Skalieren Sie ein Service Fabric-Cluster bedarfsgesteuert herunter oder hoch, indem Sie die automatischen Skalierungsregeln für jeden Knotentyp bzw. jede VM-Skalierungsgruppe festlegen. Hinzufügen oder Entfernen von Knoten für einen Service Fabric-Cluster
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: timlt
editor: ''
ms.assetid: aeb76f63-7303-4753-9c64-46146340b83d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/22/2017
ms.author: aljo
ms.openlocfilehash: 91516e3284ebf3588c2dba31b67cc583e4d395db
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/12/2018
ms.locfileid: "53309415"
---
# <a name="read-before-you-scale"></a>Vor der Skalierung unbedingt zu beachten
Die Skalierung von Computeressourcen zum Ermitteln der Arbeitsauslastung Ihrer Anwendung erfordert eine absichtliche Planung, dauert fast immer länger als eine Stunde für eine Produktionsumgebung und setzt voraus, dass Sie Ihre Arbeitsauslastung und Ihren Geschäftskontext verstehen. Wenn Sie diese Aktivität noch nie zuvor ausgeführt haben, wird empfohlen, dass Sie zunächst die [Überlegungen zur Kapazitätsplanung von Service Fabric-Clustern](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity) lesen und verstehen, bevor Sie sich mit dem Rest dieses Dokuments beschäftigen. Diese Empfehlung dient dazu, unbeabsichtigte LiveSite-Probleme zu vermeiden. Es wird außerdem empfohlen, die Vorgänge, die Sie für eine Nichtproduktionsumgebung ausführen möchten, erfolgreich zu testen. Sie können jederzeit [Produktionsprobleme melden oder bezahlte Unterstützung für Azure anfordern](https://docs.microsoft.com/azure/service-fabric/service-fabric-support#report-production-issues-or-request-paid-support-for-azure). Für Engineers, die für die Ausführung dieser Vorgänge vorgesehen sind und den entsprechendem Kontext kennen, werden in diesem Artikel Skalierungsvorgänge beschrieben. Sie müssen aber entscheiden und verstehen, welche Vorgänge für Ihren Anwendungsfall geeignet sind, z.B. welche Ressourcen skaliert werden sollen (CPU, Speicher, Arbeitsspeicher), in welche Richtung skaliert werden soll (vertikal oder horizontal) und welche Vorgänge ausgeführt werden sollen (Bereitstellung von Ressourcenvorlagen, Portal, PowerShell/CLI).

## <a name="scale-a-service-fabric-cluster-in-or-out-using-auto-scale-rules-or-manually"></a>Zentrales Hoch- oder Herunterskalieren eines Service Fabric-Clusters mithilfe von Regeln für die automatische Skalierung oder eines manuellen Verfahrens
VM-Skalierungsgruppen sind eine Azure-Computeressource, mit der Sie eine Sammlung von virtuellen Computern bereitstellen und verwalten können. Jeder Knotentyp, der in einem Service Fabric-Cluster definiert ist, wird als separate Skalierungsgruppe eines virtuellen Computers eingerichtet. Jeden Knotentyp kann dann unabhängig zentral hoch- oder herunterskaliert werden, bei jedem Typ können unterschiedliche Portgruppen geöffnet sein, und die Typen können verschiedene Kapazitätsmetriken aufweisen. Weitere Informationen finden Sie im Dokument über die [Service Fabric-Knotentypen](service-fabric-cluster-nodetypes.md). Da die Service Fabric-Knotentypen in Ihrem Cluster am Back-End aus VM-Skalierungsgruppen bestehen, müssen Sie für jeden Knotentyp bzw. jede VM-Skalierungsgruppe Regeln für die automatische Skalierung einrichten.

> [!NOTE]
> Ihr Abonnement muss ausreichend Kerne zum Hinzufügen der neuen virtuellen Computer umfassen, aus denen dieser Cluster besteht. Gegenwärtig erfolgt keine Modellvalidierung. Daher wird für die Bereitstellungszeit ein Fehler ausgegeben, wenn eine der Kontingentgrenzen erreicht wird. Ein einzelner Knotentyp kann außerdem nicht einfach 100 Knoten pro VM-Skalierungsgruppe überschreiten. Möglicherweise müssen Sie VM-Skalierungsgruppen hinzufügen, um die gewünschte Skalierung zu erreichen. Die automatische Skalierung kann nicht automatisch VM-Skalierungsgruppen hinzufügen. Das Hinzufügen von VM-Skalierungsgruppen zu einem aktiven Cluster ist eine anspruchsvolle Aufgabe, die in der Regel dazu führt, dass Benutzer neue Cluster mit den entsprechenden Knotentypen bereitstellen, die zum Zeitpunkt der Erstellung bereitgestellt werden. [Planen Sie die Clusterkapazität](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity) entsprechend. 
> 
> 

## <a name="choose-the-node-typevirtual-machine-scale-set-to-scale"></a>Auswählen des zu skalierenden Knotentyps bzw. der VM-Skalierungsgruppe
Zurzeit können Sie über das Portal keine Regeln für die automatische Skalierung für VM-Skalierungsgruppen angeben, um einen Service Fabric-Cluster zu erstellen. Daher wird Azure PowerShell (1.0 und höher) verwendet, um die Knotentypen aufzulisten und anschließend automatische Skalierungsregeln hinzuzufügen.

Führen Sie die folgenden Cmdlets aus, um eine Liste mit den VM-Skalierungsgruppen abzurufen, aus denen Ihr Cluster besteht:

```powershell
Get-AzureRmResource -ResourceGroupName <RGname> -ResourceType Microsoft.Compute/VirtualMachineScaleSets

Get-AzureRmVmss -ResourceGroupName <RGname> -VMScaleSetName <Virtual Machine scale set name>
```

## <a name="set-auto-scale-rules-for-the-node-typevirtual-machine-scale-set"></a>Festlegen von automatischen Skalierungsregeln für Knotentypen bzw. VM-Skalierungsgruppen
Wenn Ihr Cluster mehrere Knotentypen enthält, müssen Sie diesen Vorgang für jeden der Knotentypen bzw. jede der VM-Skalierungsgruppen durchführen, die Sie zentral hoch- oder herunterskalieren möchten. Sie sollten dabei die Anzahl der Knoten berücksichtigen, die Sie benötigen, bevor Sie die automatische Skalierung einrichten. Die minimale Anzahl von Knoten, die für den primären Knotentyp benötigt wird, wird von der Zuverlässigkeitsstufe gesteuert, die Sie ausgewählt haben. Erfahren Sie mehr über [Zuverlässigkeitsstufen](service-fabric-cluster-capacity.md).

> [!NOTE]
> Wenn Sie den primären Knotentyp auf eine geringere Anzahl als das Minimum zentral herunterskalieren, wird der Cluster instabil, oder er schaltet sich ab. Dies könnte für Ihre Anwendung und die Systemdienste zu Datenverlust führen.
> 
> 

Die automatische Skalierungsfunktion wird derzeit nicht durch die Lasten gesteuert, die Ihre Anwendung an Service Fabric meldet. Die bereitgestellte automatische Skalierung wird derzeit ausschließlich durch Leistungsindikatoren gesteuert, die von jeder Instanz der VM-Skalierungsgruppen ausgegeben werden.  

Befolgen Sie diese Anweisungen, um eine [automatische Skalierung für jede VM-Skalierungsgruppe einzurichten](../virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview.md).

> [!NOTE]
> In einem Szenario, in dem zentral herunterskaliert wird, müssen Sie das Cmdlet [Remove-ServiceFabricNodeState](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricnodestate) mit dem entsprechenden Knotennamen aufrufen, es sei denn, Ihr Knotentyp hat die Dauerhaftigkeitsstufe „Gold“ oder „Silber“. Für die Dauerhaftigkeitsstufe „Bronze“ ist es nicht empfehlenswert, mehr als einen Knoten auf einmal zentral herunterzuskalieren.
> 
> 

## <a name="manually-add-vms-to-a-node-typevirtual-machine-scale-set"></a>Manuelles Hinzufügen von virtuellen Computern zu einem Knotentyp oder einer VM-Skalierungsgruppe
Befolgen Sie das Beispiel bzw. die Anweisungen im [Vorlagenkatalog für den Schnellstart](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing), um die Anzahl der VMs in jedem Knotentyp zu ändern. 

> [!NOTE]
> Das Hinzufügen virtueller Computer braucht Zeit. Rechnen Sie also nicht damit, dass das Hinzufügen unmittelbar erfolgt. Planen Sie deshalb das Hinzufügen von Kapazität vorausschauend, um mehr als 10 Minuten einzuräumen, bis die VM-Kapazität für die Platzierung der Replikate und Dienstinstanzen verfügbar ist.
> 
> 

## <a name="manually-remove-vms-from-the-primary-node-typevirtual-machine-scale-set"></a>Manuelles Entfernen von virtuellen Computern aus dem primären Knotentyp oder der primären VM-Skalierungsgruppe
> [!NOTE]
> Die Service Fabric-Systemdienste werden auf dem Knotentyp „Primär“ in Ihrem Cluster ausgeführt. Sie dürfen also niemals die Anzahl der Instanzen auf diesem Knotentyp herunterfahren oder auf einen Wert herunterskalieren, der unter dem liegt, den die Zuverlässigkeitsstufe verlangt. [Hier finden Sie die Details zu den Zuverlässigkeitsstufen](service-fabric-cluster-capacity.md). 
> 
> 

Sie müssen für eine VM-Instanz die folgenden Schritte nacheinander ausführen. Dies ermöglicht, dass die Systemdienste (und Ihre statusbehafteten Dienste) ordnungsgemäß auf der VM-Instanz heruntergefahren werden, die Sie entfernen möchten, und neue Replikate auf anderen Knoten erstellt werden.

1. Führen Sie [Disable-ServiceFabricNode](https://docs.microsoft.com/powershell/module/servicefabric/disable-servicefabricnode?view=azureservicefabricps) mit der Absicht „Knoten entfernen“ aus, um den Knoten zu deaktivieren, der entfernt werden soll (die höchste Instanz auf diesem Knotentyp).
2. Führen Sie [Get-ServiceFabricNode](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) aus, um sicherzustellen, dass sich der Status des Knotens tatsächlich in „Deaktiviert“ geändert hat. Falls nicht, warten Sie, bis der Knoten deaktiviert ist. Sie können diesen Schritt nicht beschleunigen.
3. Befolgen Sie das Beispiel bzw. die Anweisungen im [Vorlagenkatalog für den Schnellstart](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing), um die Anzahl von VMs in diesem Knotentyp um 1 zu ändern. Die entfernte Instanz ist die höchste VM-Instanz. 
4. Wiederholen Sie den Anforderungen entsprechend die Schritte 1 bis 3. Skalieren Sie allerdings auf keinen Fall die Anzahl der Instanzen auf den primären Knotentypen auf einen Wert herunter, der unter dem liegt, den die Zuverlässigkeitsstufe verlangt. [Hier finden Sie die Details zu den Zuverlässigkeitsstufen](service-fabric-cluster-capacity.md). 

## <a name="manually-remove-vms-from-the-non-primary-node-typevirtual-machine-scale-set"></a>Manuelles Entfernen von virtuellen Computern aus dem primären Knotentyp oder der primären VM-Skalierungsgruppe
> [!NOTE]
> Für einen zustandsbehafteten Dienst muss eine bestimmte Anzahl von Knoten stets aktiv sein, um Verfügbarkeit sicherzustellen und den Status Ihres Diensts beizubehalten. Sie benötigen als äußerstes Minimum eine Anzahl von Knoten, die der Anzahl der Zielreplikatgruppen der Partition oder des Diensts entspricht. 
> 
> 

Sie müssen für eine VM-Instanz die folgenden Schritte nacheinander ausführen. Dies ermöglicht, dass die Systemdienste (und Ihre statusbehafteten Dienste) ordnungsgemäß auf der VM-Instanz heruntergefahren werden, die Sie entfernen möchten, und neue Replikate an anderer Stelle erstellt werden.

1. Führen Sie [Disable-ServiceFabricNode](https://docs.microsoft.com/powershell/module/servicefabric/disable-servicefabricnode?view=azureservicefabricps) mit der Absicht „Knoten entfernen“ aus, um den Knoten zu deaktivieren, der entfernt werden soll (die höchste Instanz auf diesem Knotentyp).
2. Führen Sie [Get-ServiceFabricNode](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) aus, um sicherzustellen, dass sich der Status des Knotens tatsächlich in „Deaktiviert“ geändert hat. Falls nicht, warten Sie, bis der Knoten deaktiviert ist. Sie können diesen Schritt nicht beschleunigen.
3. Befolgen Sie das Beispiel bzw. die Anweisungen im [Vorlagenkatalog für den Schnellstart](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing), um die Anzahl von VMs in diesem Knotentyp um 1 zu ändern. Die höchste VM-Instanz wird jetzt entfernt. 
4. Wiederholen Sie den Anforderungen entsprechend die Schritte 1 bis 3. Skalieren Sie allerdings auf keinen Fall die Anzahl der Instanzen auf den primären Knotentypen auf einen Wert herunter, der unter dem liegt, den die Zuverlässigkeitsstufe verlangt. [Hier finden Sie die Details zu den Zuverlässigkeitsstufen](service-fabric-cluster-capacity.md).

## <a name="behaviors-you-may-observe-in-service-fabric-explorer"></a>Verhaltensweisen von Service Fabric Explorer, die Sie möglicherweise beobachten
Wenn Sie einen Cluster zentral hochskalieren, zeigt Service Fabric Explorer die Anzahl von Knoten (Instanzen der VM-Skalierungsgruppen) an, die zum Cluster gehören.  Wenn Sie einen Cluster jedoch zentral herunterskalieren, wird der entfernte Knoten bzw. die entfernte VM-Instanz in einem fehlerhaften Zustand angezeigt, es sei denn, Sie rufen das Cmdlet [Remove-ServiceFabricNodeState](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricnodestate) mit dem entsprechenden Knotennamen auf.   

Dieses Verhalten erklärt sich wie folgt:

Die in Service Fabric Explorer aufgeführten Knoten sind ein Abbild der Informationen, die die Service Fabric-Systemdienste (besonders FM) über die derzeitige und frühere Anzahl der Knoten des Clusters besitzen. Wenn Sie eine VM-Skalierungsgruppe zentral herunterskalieren, wird der virtuelle Computer gelöscht. Der FM-Systemdienst geht aber weiterhin davon aus, dass der Knoten, der dem gelöschten virtuellen Computer zugeordnet war, dem Cluster wieder zur Verfügung stehen wird. Service Fabric Explorer zeigt diesen Knoten daher weiterhin an (der Integritätszustand kann „fehlerhaft“ oder „unbekannt“ lauten).

Sie verfügen über zwei Optionen, um sicherzustellen, dass ein Knoten beim Entfernen eines virtuellen Computers entfernt wird:

1. Wählen Sie die Dauerhaftigkeitsstufe „Gold“ oder „Silber“ für die Knotentypen in Ihrem Cluster aus, die die Infrastrukturintegration für Sie bereitstellt. Wenn Sie zentral herunterskalieren, werden so automatisch die Knoten aus den Systemdiensten (FM) entfernt.
[hier mit den Details der Dauerhaftigkeitsstufen](service-fabric-cluster-capacity.md)

2. Sobald die VM-Instanz zentral herunterskaliert wurde, müssen Sie das Cmdlet [Remove-ServiceFabricNodeState](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricnodestate) aufrufen.

> [!NOTE]
> Um Verfügbarkeit sicherzustellen und den Zustand beizubehalten, muss eine bestimmte Anzahl von Knoten in einem Service Fabric-Cluster stets in Betrieb sein. Dies wird auch als „Aufrechterhalten eines Quorums“ bezeichnet. Daher ist es für gewöhnlich nicht sicher, alle Computer innerhalb des Clusters herunterzufahren, sofern Sie nicht zuvor eine [vollständige Sicherung des Zustands](service-fabric-reliable-services-backup-restore.md) durchgeführt haben.
> 
> 

## <a name="next-steps"></a>Nächste Schritte
Lesen Sie die folgenden Artikel, um mehr über die Kapazitätsplanung von Clustern, das Upgraden von Clusters und das Partitionieren von Diensten zu erfahren:

* [Planen Sie Ihre Clusterkapazität](service-fabric-cluster-capacity.md)
* [Clusterupgrades](service-fabric-cluster-upgrade.md)
* [Die Partitionierung statusbehafteter Dienste für maximale Skalierung](service-fabric-concepts-partitioning.md)

<!--Image references-->
[BrowseServiceFabricClusterResource]: ./media/service-fabric-cluster-scale-up-down/BrowseServiceFabricClusterResource.png
[ClusterResources]: ./media/service-fabric-cluster-scale-up-down/ClusterResources.png
