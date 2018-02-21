---
title: Aktualisieren eines Azure Container Service-Clusters (AKS)
description: Aktualisieren eines Azure Container Service-Clusters (AKS)
services: container-service
author: gabrtv
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 11/15/2017
ms.author: gamonroy
ms.custom: mvc
ms.openlocfilehash: 6eaa0128c37d74fd2fd4c4bdb377ca76d7c37669
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/09/2018
---
# <a name="upgrade-an-azure-container-service-aks-cluster"></a>Aktualisieren eines Azure Container Service-Clusters (AKS)

Azure Container Service (AKS) vereinfacht allgemeine Verwaltungsaufgaben wie die Aktualisierung von Kubernetes-Clustern.

## <a name="upgrade-an-aks-cluster"></a>Aktualisieren eines AKS-Clusters

Verwenden Sie vor dem Aktualisieren eines Clusters den Befehl `az aks get-versions`, um zu überprüfen, welche Kubernetes-Versionen als Upgrade verfügbar sind.

```azurecli-interactive
az aks get-versions --name myAKSCluster --resource-group myResourceGroup --output table
```

Ausgabe:

```console
Name     ResourceGroup    MasterVersion    MasterUpgrades       NodePoolVersion     NodePoolUpgrades
-------  ---------------  ---------------  -------------------  ------------------  -------------------
default  myResourceGroup  1.7.7            1.8.2, 1.7.9, 1.8.1  1.7.7               1.8.2, 1.7.9, 1.8.1
```

Es stehen drei Versionen für das Upgrade zur Verfügung: 1.7.9, 1.8.1 und 1.8.2. Wir können den Befehl `az aks upgrade` verwenden, um auf die neueste verfügbare Version zu aktualisieren.  Während des Upgradevorgangs werden die Knoten sorgfältig [gesperrt und ausgeglichen][kubernetes-drain], um die Unterbrechung ausgeführter Anwendungen zu minimieren.  Stellen Sie vor dem Initiieren eines Clusterupgrades sicher, dass ausreichend zusätzliche Computekapazität für die Verarbeitung der Workload vorhanden ist, wenn Clusterknoten hinzugefügt und entfernt werden.

```azurecli-interactive
az aks upgrade --name myAKSCluster --resource-group myResourceGroup --kubernetes-version 1.8.2
```

Ausgabe:

```json
{
  "id": "/subscriptions/<Subscription ID>/resourcegroups/myResourceGroup/providers/Microsoft.ContainerService/managedClusters/myAKSCluster",
  "location": "eastus",
  "name": "myAKSCluster",
  "properties": {
    "accessProfiles": {
      "clusterAdmin": {
        "kubeConfig": "..."
      },
      "clusterUser": {
        "kubeConfig": "..."
      }
    },
    "agentPoolProfiles": [
      {
        "count": 1,
        "dnsPrefix": null,
        "fqdn": null,
        "name": "myAKSCluster",
        "osDiskSizeGb": null,
        "osType": "Linux",
        "ports": null,
        "storageProfile": "ManagedDisks",
        "vmSize": "Standard_D2_v2",
        "vnetSubnetId": null
      }
    ],
    "dnsPrefix": "myK8sClust-myResourceGroup-4f48ee",
    "fqdn": "myk8sclust-myresourcegroup-4f48ee-406cc140.hcp.eastus.azmk8s.io",
    "kubernetesVersion": "1.8.2",
    "linuxProfile": {
      "adminUsername": "azureuser",
      "ssh": {
        "publicKeys": [
          {
            "keyData": "..."
          }
        ]
      }
    },
    "provisioningState": "Succeeded",
    "servicePrincipalProfile": {
      "clientId": "e70c1c1c-0ca4-4e0a-be5e-aea5225af017",
      "keyVaultSecretRef": null,
      "secret": null
    }
  },
  "resourceGroup": "myResourceGroup",
  "tags": null,
  "type": "Microsoft.ContainerService/ManagedClusters"
}
```

Sie können nun mit dem Befehl `az aks show` überprüfen, ob das Upgrade erfolgreich durchgeführt wurde.

```azurecli-interactive
az aks show --name myAKSCluster --resource-group myResourceGroup --output table
```

Ausgabe:

```json
Name          Location    ResourceGroup    KubernetesVersion    ProvisioningState    Fqdn
------------  ----------  ---------------  -------------------  -------------------  ----------------------------------------------------------------
myAKSCluster  eastus     myResourceGroup  1.8.2                Succeeded            myk8sclust-myresourcegroup-3762d8-2f6ca801.hcp.eastus.azmk8s.io
```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die Bereitstellung und Verwaltung von AKS mit den AKS-Tutorials.

> [!div class="nextstepaction"]
> [AKS-Tutorial][aks-tutorial-prepare-app]

<!-- LINKS - external -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md