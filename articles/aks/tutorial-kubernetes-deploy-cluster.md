---
title: "Tutorial zu Kubernetes in Azure – Bereitstellen eines Clusters | Microsoft-Dokumentation"
description: "AKS-Tutorial – Bereitstellen eines Clusters"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: aks, azure-container-service
keywords: Docker, Container, Microservices, Kubernetes, DC/OS, Azure
ms.assetid: 
ms.service: container-service
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/24/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 9e48d490b998fb57c604f2f5b2717e65d28dce1a
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2017
---
# <a name="deploy-an-azure-container-service-aks-cluster"></a>Bereitstellen eines Azure Container Service-Clusters (AKS)

Kubernetes bietet eine verteilte Plattform für containerbasierte Anwendungen. Mit AKS ist die Bereitstellung eines produktionsbereiten Kubernetes-Clusters schnell und einfach. In diesem Tutorial – Teil 3 von 8 – wird ein Kubernetes-Cluster in AKS bereitgestellt. Folgende Schritte werden ausgeführt:

> [!div class="checklist"]
> * Bereitstellen eines Kubernetes-AKS-Clusters
> * Installation der Kubernetes-Befehlszeilenschnittstelle (kubectl)
> * Konfiguration von kubectl

In den nachfolgenden Tutorials wird die Azure Vote-Anwendung im Cluster bereitgestellt, skaliert und aktualisiert. Außerdem wird die Operations Management Suite für die Überwachung des Kubernetes-Clusters konfiguriert.

## <a name="before-you-begin"></a>Voraussetzungen

In vorherigen Tutorials wurde ein Containerimage erstellt und in eine Azure Container Registry-Instanz hochgeladen. Wenn Sie diese Schritte nicht ausgeführt haben und dies jetzt nachholen möchten, kehren Sie zum [Tutorial 1 – Erstellen von Containerimages](./tutorial-kubernetes-prepare-app.md) zurück.

## <a name="create-kubernetes-cluster"></a>Erstellen eines Kubernetes-Clusters

Das folgende Beispiel erstellt einen Cluster mit dem Namen `myK8sCluster` in einer Ressourcengruppe namens `myResourceGroup`. Diese Ressourcengruppe wurde im [vorherigen Tutorial](./tutorial-kubernetes-prepare-acr.md) erstellt.

```azurecli
az aks create --resource-group myResourceGroup --name myK8sCluster --agent-count 1 --generate-ssh-keys
```

Nach einigen Minuten ist die Bereitstellung abgeschlossen, und es werden Informationen zur AKS-Bereitstellung im JSON-Format zurückgegeben.

## <a name="install-the-kubectl-cli"></a>Installieren der Befehlszeilenschnittstelle „kubectl“

Zum Herstellen der Verbindung mit dem Kubernetes-Cluster auf Ihrem Clientcomputer verwenden Sie den Kubernetes-Befehlszeilenclient [kubectl](https://kubernetes.io/docs/user-guide/kubectl/). 

Bei Verwendung von Azure Cloud Shell ist „kubectl“ bereits installiert. Führen Sie für eine lokale Installation den folgenden Befehl aus:

```azurecli
az aks install-cli 
```

## <a name="connect-with-kubectl"></a>Verbinden mit kubectl

Führen Sie den folgenden Befehl aus, um „kubectl“ für die Verbindungsherstellung mit Ihrem Kubernetes-Cluster zu konfigurieren:

```azurecli
az aks get-credentials --resource-group=myResourceGroup --name=myK8sCluster
```

Um die Verbindung mit dem Cluster zu überprüfen, führen Sie den Befehl [kubectl get nodes](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) aus.

```azurecli
kubectl get nodes
```

Ausgabe:

```
NAME                          STATUS    AGE       VERSION
k8s-myk8scluster-36346190-0   Ready     49m       v1.7.7
```

Nach Abschluss des Tutorials verfügen Sie über einen AKS-Cluster, der für die Verarbeitung von Workloads bereit ist. In nachfolgenden Tutorials wird in diesem Cluster eine Anwendung mit mehreren Containern bereitgestellt, horizontal hochskaliert, aktualisiert und überwacht.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde ein Kubernetes-Cluster in AKS bereitgestellt. Die folgenden Schritte wurden ausgeführt:

> [!div class="checklist"]
> * Bereitstellen eines Kubernetes-AKS-Clusters
> * Installieren der Kubernetes-Befehlszeilenschnittstelle (kubectl)
> * Konfigurieren von kubectl

Fahren Sie mit dem nächsten Tutorial fort, um mehr über die Ausführung von Anwendungen im Cluster zu erfahren.

> [!div class="nextstepaction"]
> [Bereitstellen einer Anwendung in Kubernetes](./tutorial-kubernetes-deploy-application.md)
