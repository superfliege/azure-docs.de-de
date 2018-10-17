---
title: Tutorial zu Kubernetes in Azure – Upgrade eines Clusters
description: In diesem Azure Kubernetes Service-Tutorial (AKS) erfahren Sie, wie Sie ein Upgrade eines vorhandenen AKS-Clusters auf die neueste verfügbare Kubernetes-Version durchführen.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: tutorial
ms.date: 08/14/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 1c0710be11b95b66d16661b5aff9cbf739ccda92
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "48901934"
---
# <a name="tutorial-upgrade-kubernetes-in-azure-kubernetes-service-aks"></a>Tutorial: Durchführen eines Upgrades für Kubernetes in Azure Kubernetes Service (AKS)

Im Rahmen des Anwendungs- und Clusterlebenszyklus möchten Sie möglicherweise ein Upgrade auf die neueste verfügbare Version von Kubernetes durchführen und neue Funktionen nutzen. Ein Upgrade für den Azure Kubernetes Service-Cluster (AKS) kann über die Azure CLI durchgeführt werden. Um Störungen für ausgeführte Anwendungen gering zu halten, werden Kubernetes-Knoten während des Upgradeprozesses sorgfältig [gesperrt und ausgeglichen][kubernetes-drain].

In diesem Tutorial (Teil 7 von 7) wird ein Upgrade für einen Kubernetes-Cluster durchgeführt. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Identifizieren der aktuellen und verfügbaren Kubernetes-Versionen
> * Aktualisieren der Kubernetes-Knoten
> * Überprüfen des erfolgreichen Upgrades

## <a name="before-you-begin"></a>Voraussetzungen

In vorherigen Tutorials wurde eine Anwendung in ein Containerimage gepackt, dieses Image wurde in Azure Container Registry hochgeladen, und es wurde ein Kubernetes-Cluster erstellt. Anschließend wurde die Anwendung im Kubernetes-Cluster ausgeführt. Wenn Sie diese Schritte nicht ausgeführt haben und dies jetzt nachholen möchten, kehren Sie zum [Tutorial 1 – Erstellen von Containerimages][aks-tutorial-prepare-app] zurück.

Für dieses Tutorial müssen Sie mindestens Version 2.0.44 der Azure CLI ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0][azure-cli-install] Informationen dazu.

## <a name="get-available-cluster-versions"></a>Abrufen verfügbarer Clusterversionen

Verwenden Sie vor dem Upgrade eines Clusters den Befehl [az aks get-upgrades][], um zu überprüfen, welche Kubernetes-Versionen als Upgrade verfügbar sind:

```azurecli
az aks get-upgrades --resource-group myResourceGroup --name myAKSCluster --output table
```

Im folgenden Beispiel lautet die aktuelle Version *1.9.6*, und die verfügbaren Versionen werden in der Spalte *Upgrades* angezeigt.

```
Name     ResourceGroup    MasterVersion    NodePoolVersion    Upgrades
-------  ---------------  ---------------  -----------------  ----------------------
default  myResourceGroup  1.9.9            1.9.9              1.10.3, 1.10.5, 1.10.6
```

## <a name="upgrade-a-cluster"></a>Aktualisieren eines Clusters

Verwenden Sie den Befehl [az aks upgrade][], um ein Upgrade des AKS-Clusters durchzuführen. Im folgenden Beispiel wird ein Upgrade des Clusters auf Kubernetes-Version *1.10.6* durchgeführt.

> [!NOTE]
> Upgrades sind jeweils nur auf die nächste Nebenversion möglich. Beispielsweise können Sie ein Upgrade von *1.9.6* auf *1.10.3* durchführen, aber nicht direkt von *1.9.6* auf *1.11.x*. Für ein Upgrade von *1.9.6* auf *1.11.x* müssen Sie zuerst ein Upgrade von *1.9.6* auf *1.10.3* und dann ein weiteres Upgrade von  *1.10.3* auf *1.11.x* durchführen.

```azurecli
az aks upgrade --resource-group myResourceGroup --name myAKSCluster --kubernetes-version 1.10.6
```

In der folgenden verkürzten Beispielausgabe wird für *kubernetesVersion* jetzt die Versionsangabe *1.10.6* angezeigt:

```json
{
  "agentPoolProfiles": [
    {
      "count": 3,
      "maxPods": 110,
      "name": "nodepool1",
      "osType": "Linux",
      "storageProfile": "ManagedDisks",
      "vmSize": "Standard_DS1_v2",
    }
  ],
  "dnsPrefix": "myAKSClust-myResourceGroup-19da35",
  "enableRbac": false,
  "fqdn": "myaksclust-myresourcegroup-19da35-bd54a4be.hcp.eastus.azmk8s.io",
  "id": "/subscriptions/<Subscription ID>/resourcegroups/myResourceGroup/providers/Microsoft.ContainerService/managedClusters/myAKSCluster",
  "kubernetesVersion": "1.10.6",
  "location": "eastus",
  "name": "myAKSCluster",
  "type": "Microsoft.ContainerService/ManagedClusters"
}
```

## <a name="validate-an-upgrade"></a>Überprüfen eines Upgrades

Überprüfen mit dem Befehl [az aks show][] folgendermaßen, ob das Upgrade erfolgreich durchgeführt wurde:

```azurecli
az aks show --resource-group myResourceGroup --name myAKSCluster --output table
```

Die folgende Beispielausgabe zeigt, dass der AKS-Cluster *KubernetesVersion 1.10.6* ausführt:

```
Name          Location    ResourceGroup    KubernetesVersion    ProvisioningState    Fqdn
------------  ----------  ---------------  -------------------  -------------------  ----------------------------------------------------------------
myAKSCluster  eastus      myResourceGroup  1.10.6               Succeeded            myaksclust-myresourcegroup-19da35-bd54a4be.hcp.eastus.azmk8s.io
```

## <a name="delete-the-cluster"></a>Löschen des Clusters

Da dies der letzte Teil der Tutorialreihe ist, ist es ratsam, den AKS-Cluster zu löschen. Da die Kubernetes-Knoten auf virtuellen Azure-Computern (VMs) ausgeführt werden, fallen dafür auch dann weiter Gebühren an, wenn Sie den Cluster nicht nutzen. Führen Sie den Befehl [az group delete][az-group-delete] aus, um die Ressourcengruppe, den Containerdienst und alle dazugehörigen Ressourcen zu entfernen.

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

> [!NOTE]
> Wenn Sie den Cluster löschen, wird der vom AKS-Cluster verwendete Azure Active Directory-Dienstprinzipal nicht entfernt. Schritte zum Entfernen des Dienstprinzipals finden Sie unter den [Überlegungen zum AKS-Dienstprinzipal und dessen Löschung][sp-delete].

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Kubernetes in einem AKS-Cluster aktualisiert. Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Identifizieren der aktuellen und verfügbaren Kubernetes-Versionen
> * Aktualisieren der Kubernetes-Knoten
> * Überprüfen des erfolgreichen Upgrades

Über den folgenden Link erhalten Sie weitere Informationen zu AKS.

> [!div class="nextstepaction"]
> [Übersicht über AKS][aks-intro]

<!-- LINKS - external -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/

<!-- LINKS - internal -->
[aks-intro]: ./intro-kubernetes.md
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[az aks show]: /cli/azure/aks#az-aks-show
[az aks get-upgrades]: /cli/azure/aks#az-aks-get-upgrades
[az aks upgrade]: /cli/azure/aks#az-aks-upgrade
[azure-cli-install]: /cli/azure/install-azure-cli
[az-group-delete]: /cli/azure/group#az-group-delete
[sp-delete]: kubernetes-service-principal.md#additional-considerations
