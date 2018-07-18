---
title: Verwenden von kubectl mit Azure Dev Spaces | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: article
description: Schnelle Kubernetes-Entwicklung mit Containern und Microservices in Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, Container
manager: douge
ms.openlocfilehash: 3dc0dd4b571f716bcabb67c4cbef1ea6d762eb94
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35248657"
---
# <a name="use-kubectl-with-an-azure-dev-space"></a>Verwenden von kubectl mit einem Azure Dev Space

Sie können auf den Kubernetes-Cluster in einem Azure Dev Space zugreifen und vorhandene Kubernetes-Tools wie `kubectl` verwenden.

Beim Ausführen des Befehls `az aks use-dev-spaces` wird automatisch ein `kubectl`-Konfigurationskontext hinzugefügt, sodass kubectl bereits mit Ihrem Azure Dev Spaces-Kubernetes-Cluster verbunden sein sollte. Beispiele:
- Bestätigen des aktuellen Kontexts: `kubectl config current-context`
- Auflisten aller verfügbaren Kontexte: `kubectl config get-contexts` 
- Ändern des Kontexts: `kubectl config use-context <context-name>`
- Anzeigen des Kubernetes-Dashboards: Führen Sie `kubectl proxy` aus, und öffnen Sie dann in Ihrem Browser die Adresse, die dieser Befehl ausgibt (fügen Sie `/ui` an die URL an, um zum Kubernetes-Dashboard zu navigieren).
- Auflisten der ausgeführten Dienste im Azure Dev Spaces-Standardbereich namens *default*: `kubectl get services --namespace=default`

