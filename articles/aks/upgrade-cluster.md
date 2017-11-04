---
title: Aktualisieren eines Azure Container Service-Clusters (AKS) | Microsoft-Dokumentation
description: Aktualisieren eines Azure Container Service-Clusters (AKS)
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
ms.openlocfilehash: 7b69f770354c9402a3248a1d5a27558ff9aea7c4
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2017
---
# <a name="upgrade-an-azure-container-service-aks-cluster"></a>Aktualisieren eines Azure Container Service-Clusters (AKS)

Azure Container Service (AKS) vereinfacht allgemeine Verwaltungsaufgaben wie die Aktualisierung von Kubernetes-Clustern.

## <a name="upgrade-an-aks-cluster"></a>Aktualisieren eines AKS-Clusters

Verwenden Sie vor dem Aktualisieren eines Clusters den Befehl `az aks get-versions`, um zu überprüfen, welche Kubernetes-Versionen als Upgrade verfügbar sind.

```azurecli-interactive
az aks get-versions --name myK8sCluster --resource-group myResourceGroup --output table
```

Ausgabe:

```console
Name     ResourceGroup    MasterVersion    MasterUpgrades    AgentPoolVersion    AgentPoolUpgrades
-------  ---------------  ---------------  ----------------  ------------------  -------------------
default  myResourceGroup  1.7.7            1.8.1             1.7.7               1.8.1
```

Es stehen zwei Versionen für das Upgrade zur Verfügung: 1.8.0 und 1.8.1.  Wir können den Befehl `az aks upgrade` verwenden, um auf die neueste verfügbare Version zu aktualisieren.  Während des Upgradevorgangs werden die Knoten sorgfältig [gesperrt und ausgeglichen](https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/), um die Unterbrechung ausgeführter Anwendungen zu minimieren.

```azurecli-interactive
az aks upgrade --name myK8sCluster --resource-group myResourceGroup --kubernetes-version 1.8.1
```

Ausgabe:

```json
{
  "id": "/subscriptions/4f48eeae-9347-40c5-897b-46af1b8811ec/resourcegroups/myResourceGroup/providers/Microsoft.ContainerService/managedClusters/myK8sCluster",
  "location": "westus2",
  "name": "myK8sCluster",
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
        "name": "myK8sCluster",
        "osDiskSizeGb": null,
        "osType": "Linux",
        "ports": null,
        "storageProfile": "ManagedDisks",
        "vmSize": "Standard_D2_v2",
        "vnetSubnetId": null
      }
    ],
    "dnsPrefix": "myK8sClust-myResourceGroup-4f48ee",
    "fqdn": "myk8sclust-myresourcegroup-4f48ee-406cc140.hcp.westus2.azmk8s.io",
    "kubernetesVersion": "1.8.1",
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
az aks show --name myK8sCluster --resource-group myResourceGroup --output table
```

Ausgabe:

```json
Name          Location    ResourceGroup    KubernetesVersion    ProvisioningState    Fqdn
------------  ----------  ---------------  -------------------  -------------------  ----------------------------------------------------------------
myK8sCluster  westus2     myResourceGroup  1.8.1                Succeeded            myk8sclust-myresourcegroup-3762d8-2f6ca801.hcp.westus2.azmk8s.io
```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die Bereitstellung und Verwaltung von AKS mit den AKS-Tutorials.

> [!div class="nextstepaction"]
> [AKS-Tutorial](./tutorial-kubernetes-prepare-app.md)