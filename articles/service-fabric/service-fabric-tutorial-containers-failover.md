---
title: Failover und Skalierung einer Azure Service Fabric-Container-App | Microsoft-Dokumentation
description: "Erfahren Sie, wie ein Failover in einer Azure Service Fabric-Containeranwendung ausgeführt wird.  Zudem erhalten Sie Informationen darüber, wie die in einem Cluster ausgeführten Container und Dienste skaliert werden."
services: service-fabric
documentationcenter: 
author: suhuruli
manager: timlt
editor: suhuruli
tags: servicefabric
keywords: Docker, Container, Microservices, Service Fabric, Azure
ms.assetid: 
ms.service: service-fabric
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/12/2017
ms.author: suhuruli
ms.custom: mvc
ms.openlocfilehash: 21dd9dfbc90c26236c43e2c334305ca97f63d361
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="demonstrate-fail-over-and-scaling-of-container-services-with-service-fabric"></a>Failover und Skalierung von Containerdiensten mit Service Fabric

Dieses Tutorial ist der dritte Teil einer Reihe. In diesem Tutorial wird beschrieben, wie ein Failover in Service Fabric-Containeranwendungen ausgeführt wird. Zudem erfahren Sie, wie Sie Container skalieren. In diesem Tutorial haben Sie Folgendes durchgeführt:

> [!div class="checklist"]
> * Ausführen eines Failovers für einen Container in einem Service Fabric-Cluster  
> * Skalieren der Web-Front-End-Container in einer Anwendung

## <a name="prerequisites"></a>Voraussetzungen
Die Anwendung aus [Teil 2](service-fabric-tutorial-package-containers.md) wird in einem aktiven Service Fabric-Cluster ausgeführt.

## <a name="fail-over-a-container-in-a-cluster"></a>Ausführen eines Failovers für einen Container in einem Cluster
Mit Service Fabric wird sichergestellt, dass Ihre Containerinstanzen automatisch auf andere Knoten im Cluster verschoben werden, falls es zu einem Ausfall kommt. Sie können die Container eines Knotens auch manuell entfernen und diese geregelt auf andere Knoten im Cluster verschieben. Sie haben mehrere Möglichkeiten, um Ihre Dienste zu skalieren. In diesem Beispiel verwenden wir Service Fabric Explorer.

Führen Sie die folgenden Schritte aus, um für den Front-End-Container ein Failover auszuführen:

1. Öffnen Sie Service Fabric Explorer in Ihrem Cluster, z.B. `http://lin4hjim3l4.westus.cloudapp.azure.com:19080`.
2. Klicken Sie in der Strukturansicht auf den Knoten **fabric:/TestContainer/azurevotefront**, und erweitern Sie den Partitionsknoten (durch eine GUID dargestellt). Beachten Sie den Knotennamen in der Strukturansicht. Hier werden die Knoten angezeigt, auf denen der Container derzeit ausgeführt wird, z.B. `_nodetype_1`.
3. Erweitern Sie in der Strukturansicht den Knoten **Knoten**. Klicken Sie auf das Auslassungszeichen (drei Punkte) neben dem Knoten, auf dem der Container ausgeführt wird.
1. Wählen Sie **Neu starten** aus, um den Knoten neu zu starten, und bestätigen Sie die Neustartaktion. Der Neustart bewirkt, dass für den Container ein Failover auf einen anderen Knoten im Cluster ausgeführt wird.

![noderestart][noderestart]

Beachten Sie, wie der Knotenname, der angibt, wo die Front-End-Container ausgeführt werden, nun in den Namen eines anderen Knoten im Cluster geändert wird. Nach einigen Minuten sollten Sie wieder zu der Anwendung navigieren können und sehen, dass die Anwendung nun auf einem anderen Knoten ausgeführt wird.

## <a name="scale-containers-and-services-in-a-cluster"></a>Skalieren von Containern und Diensten in einem Cluster
Service Fabric-Container können für einen Cluster skaliert werden, um die Last für die Dienste auszugleichen. Sie skalieren einen Container, indem Sie die Anzahl der Instanzen ändern, die im Cluster ausgeführt werden.

Führen Sie die folgenden Schritte aus, um das Web-Front-End zu skalieren:

1. Öffnen Sie Service Fabric Explorer in Ihrem Cluster, z.B. `http://lin4hjim3l4.westus.cloudapp.azure.com:19080`.
2. Klicken Sie in der Strukturansicht auf das Auslassungszeichen (drei Punkte) neben dem Knoten **fabric:/TestContainer/azurevotefront**, und wählen Sie **Scale Service** (Dienst skalieren) aus.

![sfxscale][sfxscale]

Sie können jetzt angeben, wie viele Instanzen des Web-Front-Ends skaliert werden sollen.

3. Ändern Sie die Anzahl in **2**, und klicken Sie auf **Scale Service** (Dienst skalieren).
4. Klicken Sie in der Strukturansicht auf den Knoten **fabric:/TestContainer/azurevotefront**, und erweitern Sie den Partitionsknoten (durch eine GUID dargestellt).

![sfxscaledone][sfxscaledone]

Sie sehen jetzt, dass der Dienst über zwei Instanzen verfügt. In der Strukturansicht wird angezeigt, auf welchen Knoten die Instanzen ausgeführt werden.

Mit dieser einfachen Verwaltungsaufgabe haben wir die Ressourcen verdoppelt, die für unseren Front-End-Dienst zum Verarbeiten der Benutzerauslastung verfügbar sind. Es ist wichtig zu verstehen, dass Sie nicht mehrere Instanzen eines Diensts benötigen, damit er zuverlässig ausgeführt wird. Wenn ein Dienst ausfällt, wird von Service Fabric sichergestellt, dass im Cluster eine neue Dienstinstanz ausgeführt wird.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde ein Failover für einen Container sowie die Skalierung einer Anwendung veranschaulicht. Die folgenden Schritte wurden ausgeführt:

> [!div class="checklist"]
> * Ausführen eines Failovers für einen Container in einem Service Fabric-Cluster  
> * Skalieren der Web-Front-End-Container in einer Anwendung

In dieser Tutorialreihe haben Sie Folgendes gelernt: 
> [!div class="checklist"]
> * Erstellen von Containerimages
> * Pushen von Containerimages in Azure Container Registry
> * Packen von Containern für Service Fabric mithilfe von Yeoman
> * Erstellen und Ausführen einer Service Fabric-Anwendung mit Containern
> * Ausführen des Failovers und der Skalierung in Service Fabric

[noderestart]: ./media/service-fabric-tutorial-containers-failover/containersfailovertutorialnoderestart.png
[sfxscale]: ./media/service-fabric-tutorial-containers-failover/containersfailovertutorialscale.png
[sfxscaledone]: ./media/service-fabric-tutorial-containers-failover/containersfailovertutorialscaledone.png
