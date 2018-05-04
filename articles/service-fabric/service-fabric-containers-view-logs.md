---
title: Anzeigen von Containerprotokollen in Azure Service Fabric | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Containerprotokolle für einen ausgeführten Service Fabric-Containerdienst mithilfe von Service Fabric Explorer angezeigt werden.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/09/2018
ms.author: ryanwi
ms.openlocfilehash: 48ee54460454368deef44c8f84624e32856efafa
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="view-logs-for-a-service-fabric-container-service"></a>Anzeigen von Protokollen für einen Service Fabric-Containerdienst
Azure Service Fabric ist ein Containerorchestrator und unterstützt [Linux- und Windows-Container](service-fabric-containers-overview.md).  In diesem Artikel wird beschrieben, wie Containerprotokolle eines ausgeführten Containerdiensts angezeigt werden, sodass Sie Probleme diagnostizieren und beheben können.

## <a name="access-container-logs"></a>Zugriff auf Containerprotokolle
Der Zugriff auf Containerprotokolle ist über [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) möglich.  Öffnen Sie in einem Webbrowser Service Fabric Explorer über den Verwaltungsendpunkt des Clusters. Navigieren Sie dazu zu [http://mycluster.region.cloudapp.azure.com:19080/Explorer](http://mycluster.region.cloudapp.azure.com:19080/Explorer).  

Die Containerprotokolle befinden sich auf dem Clusterknoten, auf dem die Containerdienstinstanz ausgeführt wird. Rufen Sie beispielsweise die Protokolle für den Front-End-Webcontainer der [Linux-Voting-Beispielanwendung](service-fabric-quickstart-containers-linux.md) ab. Erweitern Sie in der Strukturansicht **Cluster**>**Anwendungen**>**VotingType**>**fabric:/Voting/azurevotefront**.  Erweitern Sie anschließend die Partition (in diesem Beispiel d1aa737e-f22a-e347-be16-eec90be24bc1), und überprüfen Sie, ob der Container auf dem Clusterknoten *_lnxvm_0* ausgeführt wird.

Suchen Sie in der Strukturansicht das Codepaket auf dem Knoten *_lnxvm_0*, indem Sie **Knoten**>**_lnxvm_0**>**fabric:/Voting**>**azurevotfrontPkg**>**Codepakete**>**Code** erweitern.  Wählen Sie dann die Option **Containerprotokolle** aus, um die Containerprotokolle anzuzeigen.

![Service Fabric-Plattform][Image1]


## <a name="next-steps"></a>Nächste Schritte
- Durcharbeiten des Tutorials [Erstellen einer Linux-Containeranwendung](service-fabric-tutorial-create-container-images.md).
- Weitere Informationen zu [Service Fabric und Containern](service-fabric-containers-overview.md).

[Image1]: media/service-fabric-containers-view-logs/view-container-logs-sfx.png
