---
title: Azure Dev Spaces | Microsoft-Dokumentation
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 06/01/2018
ms.topic: tutorial
description: Schnelle Kubernetes-Entwicklung mit Containern und Microservices in Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, Container
manager: douge
ms.openlocfilehash: 14b51cc2ad2e8e0f294e5e73e542001e30d21c9d
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/15/2018
ms.locfileid: "41919514"
---
# <a name="azure-dev-spaces"></a>Azure Dev Spaces
Azure Dev Spaces ermöglicht eine schnelle, iterative Kubernetes-Bereitstellung für Teams. Mit minimalem Einrichtungsaufwand auf dem Entwicklungscomputer können Sie Container direkt in Azure Kubernetes Service (AKS) iterativ ausführen und debuggen. Entwickeln Sie unter Windows, Mac und Linux, und verwenden Sie vertraute Tools wie Visual Studio, Visual Studio Code oder die Befehlszeile.

[!INCLUDE[](includes/dev-spaces-preview.md)]

## <a name="how-azure-dev-spaces-simplifies-kubernetes-development"></a>So vereinfacht Azure Dev Spaces die Kubernetes-Entwicklung 

Azure Dev Spaces bietet Entwicklerteams in Kubernetes folgende Produktivitätsvorteile:
- Minimieren des Einrichtungsaufwands auf dem lokalen Entwicklungscomputer für jedes Teammitglied und direktes Arbeiten in AKS (einem verwalteten Kubernetes-Cluster in Azure)
- Schnelles Durchlaufen und Debuggen von Code direkt in Kubernetes unter Verwendung von Visual Studio 2017 oder Visual Studio Code
- Generieren von Docker- und Kubernetes-basierten Konfiguration-als-Code-Ressourcen, die von der Entwicklungs- bis zur Produktionsphase verwendet werden können 
- Gemeinsames Verwenden eines verwalteten Kubernetes-Clusters mit Ihrem Team und gemeinschaftliches Zusammenarbeiten Entwickeln von Code in Isolation und Ausführen von End-to-End-Tests mit anderen Komponenten ohne Replizieren oder Simulieren von Abhängigkeiten

[!INCLUDE[](includes/get-started.md)]

![](media/azure-dev-spaces/vscode-overview.png)

## <a name="see-also"></a>Siehe auch

[Azure Kubernetes Service](/azure/aks)