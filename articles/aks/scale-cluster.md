---
title: Skalieren eines Azure Kubernetes Service-Clusters (AKS)
description: Erfahren Sie, wie Sie die Anzahl der Knoten in einem Azure Kubernetes Service-Cluster (AKS) skalieren.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 01/10/2019
ms.author: iainfoulds
ms.openlocfilehash: 558a3b6dc15293ab9a0895aa4f9f709ba2d0a51f
ms.sourcegitcommit: e7312c5653693041f3cbfda5d784f034a7a1a8f1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2019
ms.locfileid: "54214622"
---
# <a name="scale-the-node-count-in-an-azure-kubernetes-service-aks-cluster"></a>Skalieren der Anzahl der Knoten in einem Azure Kubernetes Service-Cluster (AKS)

Wenn sich die Ressourcenanforderungen Ihrer Anwendungen ändern, können Sie einen AKS-Cluster manuell zur Ausführung einer anderen Anzahl von Knoten skalieren. Beim Herunterskalieren werden die Knoten sorgfältig [gesperrt und ausgeglichen][kubernetes-drain], um die Unterbrechung ausgeführter Anwendungen zu minimieren. Beim Hochskalieren wartet der Befehl `az`, bis die Knoten vom Kubernetes-Cluster als `Ready` markiert worden sind.

## <a name="scale-the-cluster-nodes"></a>Skalieren der Clusterknoten

Rufen Sie zuerst den *Namen* Ihres Knotenpools mithilfe des [az aks show][az-aks-show]-Befehls ab. Im folgenden Beispiel wird der Knotenpoolnamen für den Cluster namens *myAKSCluster* in der Ressourcengruppe *myResourceGroup* abgerufen:

```azurecli-interactive
az aks show --resource-group myResourceGroup --name myAKSCluster --query agentPoolProfiles
```

Die folgende Beispielausgabe zeigt, dass der *Name* *nodepool1* lautet:

```console
$ az aks show --resource-group myResourceGroup --name myAKSCluster --query agentPoolProfiles

[
  {
    "count": 1,
    "maxPods": 110,
    "name": "nodepool1",
    "osDiskSizeGb": 30,
    "osType": "Linux",
    "storageProfile": "ManagedDisks",
    "vmSize": "Standard_DS2_v2"
  }
]
```

Verwenden Sie den Befehl `az aks scale`, um die Clusterknoten zu skalieren. Das folgende Beispiel skaliert einen Cluster namens *myAKSCluster* auf einen einzelnen Knoten. Stellen Sie Ihren eigenen *--nodepool-name* aus dem vorherigen Befehl bereit, z.B. *nodepool1*:

```azurecli-interactive
az aks scale --resource-group myResourceGroup --name myAKSCluster --node-count 1 --nodepool-name <your node pool name>
```

Die folgende Beispielausgabe zeigt, dass der Cluster erfolgreich auf einen einzigen Knoten skaliert wurde, wie im Abschnitt *agentPoolProfiles* gezeigt:

```json
{
  "aadProfile": null,
  "addonProfiles": null,
  "agentPoolProfiles": [
    {
      "count": 1,
      "maxPods": 110,
      "name": "nodepool1",
      "osDiskSizeGb": 30,
      "osType": "Linux",
      "storageProfile": "ManagedDisks",
      "vmSize": "Standard_DS2_v2",
      "vnetSubnetId": null
    }
  ],
  "dnsPrefix": "myAKSClust-myResourceGroup-19da35",
  "enableRbac": true,
  "fqdn": "myaksclust-myresourcegroup-19da35-0d60b16a.hcp.eastus.azmk8s.io",
  "id": "/subscriptions/<guid>/resourcegroups/myResourceGroup/providers/Microsoft.ContainerService/managedClusters/myAKSCluster",
  "kubernetesVersion": "1.9.11",
  "linuxProfile": {
    "adminUsername": "azureuser",
    "ssh": {
      "publicKeys": [
        {
          "keyData": "[...]"
        }
      ]
    }
  },
  "location": "eastus",
  "name": "myAKSCluster",
  "networkProfile": {
    "dnsServiceIp": "10.0.0.10",
    "dockerBridgeCidr": "172.17.0.1/16",
    "networkPlugin": "kubenet",
    "networkPolicy": null,
    "podCidr": "10.244.0.0/16",
    "serviceCidr": "10.0.0.0/16"
  },
  "nodeResourceGroup": "MC_myResourceGroup_myAKSCluster_eastus",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "servicePrincipalProfile": {
    "clientId": "[...]",
    "secret": null
  },
  "tags": null,
  "type": "Microsoft.ContainerService/ManagedClusters"
}
```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die Bereitstellung und Verwaltung von AKS mit den AKS-Tutorials.

> [!div class="nextstepaction"]
> [AKS-Tutorial][aks-tutorial]

<!-- LINKS - external -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/

<!-- LINKS - internal -->
[aks-tutorial]: ./tutorial-kubernetes-prepare-app.md
[az-aks-show]: /cli/azure/aks#az-aks-show
