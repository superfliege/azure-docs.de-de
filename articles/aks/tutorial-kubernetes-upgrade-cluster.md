---
title: "Tutorial zu Kubernetes in Azure – Aktualisieren eines Clusters"
description: "Tutorial zu Kubernetes in Azure – Aktualisieren eines Clusters"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 11/15/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: d82232d590bcc5c578ebe8ed7c85d25aebcfe097
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/09/2018
---
# <a name="upgrade-kubernetes-in-azure-container-service-aks"></a>Aktualisieren von Kubernetes in Azure Container Service (AKS)

Ein Azure Container Service-Cluster (AKS) kann über die Azure-Befehlszeilenschnittstelle aktualisiert werden. Während des Upgradevorgangs werden Kubernetes-Knoten sorgfältig [gesperrt und ausgeglichen][kubernetes-drain], um die Unterbrechung ausgeführter Anwendungen zu minimieren.

In diesem Tutorial – Teil 8 von 8 – wird ein Kubernetes-Cluster aktualisiert. Sie führen folgende Aufgaben aus:

> [!div class="checklist"]
> * Identifizieren der aktuellen und verfügbaren Kubernetes-Versionen
> * Aktualisieren der Kubernetes-Knoten
> * Überprüfen des erfolgreichen Upgrades

## <a name="before-you-begin"></a>Voraussetzungen

In vorherigen Tutorials wurde eine Anwendung in ein Containerimage gepackt, dieses Image wurde in Azure Container Registry hochgeladen, und es wurde ein Kubernetes-Cluster erstellt. Anschließend wurde die Anwendung im Kubernetes-Cluster ausgeführt.

Wenn Sie diese Schritte nicht ausgeführt haben und dies jetzt nachholen möchten, kehren Sie zum [Tutorial 1 – Erstellen von Containerimages][aks-tutorial-prepare-app] zurück.


## <a name="get-cluster-versions"></a>Abrufen von Clusterversionen

Verwenden Sie vor dem Aktualisieren eines Clusters den Befehl `az aks get-versions`, um zu überprüfen, welche Kubernetes-Versionen als Upgrade verfügbar sind.

```azurecli-interactive
az aks get-versions --name myAKSCluster --resource-group myResourceGroup --output table
```

Hier können Sie sehen, dass die aktuelle Knotenversion `1.7.7` ist und dass die Versionen `1.7.9`, `1.8.1` und `1.8.2` verfügbar sind.

```
Name     ResourceGroup    MasterVersion    MasterUpgrades       NodePoolVersion     NodePoolUpgrades
-------  ---------------  ---------------  -------------------  ------------------  -------------------
default  myAKSCluster     1.7.7            1.8.2, 1.7.9, 1.8.1  1.7.7               1.8.2, 1.7.9, 1.8.1
```

## <a name="upgrade-cluster"></a>Aktualisieren des Clusters

Verwenden Sie den Befehl `az aks upgrade`, um die Clusterknoten zu aktualisieren. In den folgenden Beispielen wird der Cluster auf Version `1.8.2` aktualisiert.

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

## <a name="validate-upgrade"></a>Überprüfen des Upgrades

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

In diesem Tutorial haben Sie Kubernetes in einem AKS-Cluster aktualisiert. Die folgenden Aufgaben wurden ausgeführt:

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