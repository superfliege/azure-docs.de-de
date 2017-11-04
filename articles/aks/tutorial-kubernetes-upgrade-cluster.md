---
title: "Tutorial zu Kubernetes in Azure – Aktualisieren eines Clusters | Microsoft-Dokumentation"
description: "Tutorial zu Kubernetes in Azure – Aktualisieren eines Clusters"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: aks, azure-container-service
keywords: Docker, Container, Microservices, Kubernetes, DC/OS, Azure
ms.assetid: 
ms.service: container-service
ms.devlang: aurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/24/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: f203e01e6aaecf04944ee830df4f6adeb1c74d2f
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2017
---
# <a name="upgrade-kubernetes-in-azure-container-service-aks"></a>Aktualisieren von Kubernetes in Azure Container Service (AKS)

Ein Azure Container Service-Cluster (AKS) kann über die Azure-Befehlszeilenschnittstelle aktualisiert werden. Während des Upgradevorgangs werden Kubernetes-Knoten sorgfältig [gesperrt und ausgeglichen](https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/), um die Unterbrechung ausgeführter Anwendungen zu minimieren.

In diesem Tutorial – Teil 8 von 8 – wird ein Kubernetes-Cluster aktualisiert. Sie führen folgende Aufgaben aus:

> [!div class="checklist"]
> * Identifizieren der aktuellen und verfügbaren Kubernetes-Versionen
> * Aktualisieren der Kubernetes-Knoten
> * Überprüfen des erfolgreichen Upgrades

## <a name="before-you-begin"></a>Voraussetzungen

In vorherigen Tutorials wurde eine Anwendung in ein Containerimage gepackt, dieses Image wurde in Azure Container Registry hochgeladen, und es wurde ein Kubernetes-Cluster erstellt. Anschließend wurde die Anwendung im Kubernetes-Cluster ausgeführt. 

Wenn Sie diese Schritte nicht ausgeführt haben und dies jetzt nachholen möchten, kehren Sie zum [Tutorial 1 – Erstellen von Containerimages](./tutorial-kubernetes-prepare-app.md) zurück.


## <a name="get-cluster-versions"></a>Abrufen von Clusterversionen

Verwenden Sie vor dem Aktualisieren eines Clusters den Befehl `az aks get-versions`, um zu überprüfen, welche Kubernetes-Versionen als Upgrade verfügbar sind.

```azurecli-interactive
az aks get-versions --name myK8sCluster --resource-group myResourceGroup --output table
```

Hier können Sie sehen, dass die aktuelle Knotenversion `1.7.7` ist und dass Version `1.8.1` verfügbar ist.

```
Name     ResourceGroup    MasterVersion    MasterUpgrades    AgentPoolVersion    AgentPoolUpgrades
-------  ---------------  ---------------  ----------------  ------------------  -------------------
default  myResourceGroup  1.7.7            1.8.1             1.7.7               1.8.1
```

## <a name="upgrade-cluster"></a>Aktualisieren des Clusters

Verwenden Sie den Befehl `az aks upgrade`, um die Clusterknoten zu aktualisieren. In den folgenden Beispielen wird der Cluster auf Version `1.8.1` aktualisiert.

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

## <a name="validate-upgrade"></a>Überprüfen des Upgrades

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

In diesem Tutorial haben Sie Kubernetes in einem AKS-Cluster aktualisiert. Die folgenden Aufgaben wurden ausgeführt:

> [!div class="checklist"]
> * Identifizieren der aktuellen und verfügbaren Kubernetes-Versionen
> * Aktualisieren der Kubernetes-Knoten
> * Überprüfen des erfolgreichen Upgrades

Über den folgenden Link erhalten Sie weitere Informationen zu AKS.

> [!div class="nextstepaction"]
> [Übersicht über AKS](./intro-kubernetes.md)