---
title: "Einführung in Azure Container Service für Kubernetes | Microsoft-Dokumentation"
description: "Azure Container Service für Kubernetes vereinfacht die Bereitstellung und Verwaltung containerbasierter Anwendungen in Azure."
services: container-service
documentationcenter: 
author: gabrtv
manager: timlt
editor: 
tags: aks, azure-container-service
keywords: Kubernetes, Docker, Container, Microservices, Azure
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/24/2017
ms.author: gamonroy
ms.custom: mvc
ms.openlocfilehash: fa46d16e2105c0354cc533c58fb9e0093c129ced
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2017
---
# <a name="introduction-to-azure-container-service-aks"></a>Einführung in Azure Container Service (AKS)

Azure Container Service (AKS) vereinfacht das Erstellen, Konfigurieren und Verwalten eines Clusters mit virtuellen Computern, die für die Ausführung von Anwendungen in Containern vorkonfiguriert sind. So können Sie Ihre vorhandenen Kenntnisse nutzen, bzw. auf einen großen und wachsenden Pool von Communityfachkenntnissen zur Bereitstellung und Verwaltung von containerbasierten Anwendungen in Microsoft Azure zurückgreifen.

Mit AKS können Sie die professionellen Features von Azure nutzen und müssen dank Kubernetes und Docker-Imageformat trotzdem nicht auf Anwendungsportabilität verzichten.

## <a name="using-azure-container-service-aks"></a>Verwenden von Azure Container Service (AKS)
Mit AKS verfolgen wir das Ziel, mit Open Source-Tools und -Technologien, die heutzutage bei unseren Kunden beliebt sind, eine Umgebung für das Containerhosting bereitzustellen. Zu diesem Zweck machen wir die standardmäßigen Kubernetes-API-Endpunkte verfügbar. Mithilfe dieser Standardendpunkte können Sie jede Software nutzen, die mit einem Kubernetes-Cluster kommunizieren kann. Zur Auswahl stehen beispielsweise [kubectl](https://kubernetes.io/docs/user-guide/kubectl-overview/), [helm](https://helm.sh/) und [draft](https://github.com/Azure/draft).

## <a name="creating-a-kubernetes-cluster-using-azure-container-service-aks"></a>Erstellen eines Kubernetes-Clusters mithilfe von Azure Container Service (AKS)
Stellen Sie zum Verwenden von AKS zunächst mithilfe der [Azure CLI](./kubernetes-walkthrough.md) oder über das Portal einen AKS-Cluster bereit. (Suchen Sie im Marketplace nach **Azure Container Service**.) Erfahrene Benutzer, die mehr Kontrolle über Azure Resource Manager-Vorlagen benötigen, können mithilfe des Open-Source-Projekts [acs-engine](https://github.com/Azure/acs-engine) einen eigenen benutzerdefinierten Kubernetes-Cluster erstellen und über die `az`-Befehlszeilenschnittstelle bereitstellen.

### <a name="using-kubernetes"></a>Verwenden von Kubernetes
Kubernetes automatisiert die Bereitstellung, Skalierung und Verwaltung von Anwendungen in Containern. Das Tool bietet zahlreiche Funktionen, darunter:
* Automatisches Bin Packing
* Selbstreparatur
* Horizontale Skalierung
* Dienstermittlung und Lastenausgleich
* Automatisierte Rollouts und Rollbacks
* Geheimnis- und Konfigurationsverwaltung
* Speicherorchestrierung
* Batchausführung

## <a name="videos"></a>Videos

Kubernetes-Unterstützung in Azure Container Service (Azure Friday, Januar 2017):

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Kubernetes-Support-in-Azure-Container-Services/player]
>
>

Tools für die Entwicklung und Bereitstellung von Anwendungen in Kubernetes (Azure OpenDev, Juni 2017):

> [!VIDEO https://channel9.msdn.com/Events/AzureOpenDev/June2017/Tools-for-Developing-and-Deploying-Applications-on-Kubernetes/player]
>
>

Erfahren Sie mehr über die Bereitstellung und Verwaltung von AKS mit der AKS-Schnellstartanleitung.

> [!div class="nextstepaction"]
> [AKS-Tutorial](./kubernetes-walkthrough.md)