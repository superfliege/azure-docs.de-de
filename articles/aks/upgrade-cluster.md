---
title: Durchführen eines Upgrades für einen Azure Kubernetes Service-Cluster (AKS)
description: Erfahren Sie, wie Sie ein Upgrade für einen AKS-Cluster (Azure Kubernetes Service) vornehmen.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 02/12/2019
ms.author: iainfou
ms.openlocfilehash: 59d52db8c3f5f8968eae1a544abe1e5c6bbaacca
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56185980"
---
# <a name="upgrade-an-azure-kubernetes-service-aks-cluster"></a>Durchführen eines Upgrades für einen Azure Kubernetes Service-Cluster (AKS)

Im Verlauf des Lebenszyklus eines AKS-Clusters müssen Sie häufig ein Upgrade auf die neueste Kubernetes-Version vornehmen. Es ist wichtig, jeweils die aktuelle Kubernetes-Sicherheitsversion anzuwenden oder bei einem Upgrade die neuesten Features zu erhalten. In diesem Artikel wird gezeigt, wie Sie ein Upgrade für einen vorhandenen AKS-Cluster durchführen.

## <a name="before-you-begin"></a>Voraussetzungen

Der Artikel setzt voraus, dass Sie mindestens Version 2.0.56 der Azure-Befehlszeilenschnittstelle (Azure CLI) ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0][azure-cli-install] Informationen dazu.

## <a name="check-for-available-aks-cluster-upgrades"></a>Suchen nach verfügbaren AKS-Clusterupgrades

Überprüfen Sie mithilfe des Befehls [az aks get-upgrades][az-aks-get-upgrades], welche Kubernetes-Versionen für Ihren Cluster verfügbar sind. Im folgenden Beispiel werden für den Cluster namens *myAKSCluster* in der Ressourcengruppe namens *myResourceGroup* verfügbare Upgrades gesucht:

```azurecli-interactive
az aks get-upgrades --resource-group myResourceGroup --name myAKSCluster --output table
```

> [!NOTE]
> Beim Upgrade eines AKS-Clusters können Nebenversionen von Kubernetes nicht übersprungen werden. Zum Beispiel sind Upgrades von *1.10.x* -> *1.11.x* oder *1.11.x* -> *1.12.x* zulässig, ein Upgrade von *1.10.x* -> *1.12.x* ist jedoch nicht möglich.
>
> Für ein Upgrade von *1.10.x* -> *1.12.x* müssen Sie zuerst ein Upgrade von *1.10.x* -> *1.11.x* und dann ein Upgrade von *1.11.x* -> *1.12.x* durchführen.

Die Ausgabe im folgenden Beispiel zeigt, dass der Cluster auf Version *1.11.5* oder *1.11.6* aktualisiert werden kann:

```console
Name     ResourceGroup    MasterVersion    NodePoolVersion    Upgrades
-------  ---------------  ---------------  -----------------  --------------
default  myResourceGroup  1.10.12          1.10.12            1.11.5, 1.11.6
```

## <a name="upgrade-an-aks-cluster"></a>Aktualisieren eines AKS-Clusters

Wenn Sie die Liste der verfügbaren Versionen für Ihren AKS-Cluster angezeigt haben, verwenden Sie den Befehl [az aks upgrade][az-aks-upgrade], um den Cluster zu aktualisieren. Während des Upgradevorgangs fügt AKS dem Cluster einen neuen Knoten hinzu. Anschließend werden die Knoten nacheinander sorgfältig [abgesperrt und ausgeglichen][kubernetes-drain], um die Unterbrechung ausgeführter Anwendungen auf ein Minimum zu beschränken. Im folgenden Beispiel wird der Cluster auf Version *1.11.6* aktualisiert:

```azurecli-interactive
az aks upgrade --resource-group myResourceGroup --name myAKSCluster --kubernetes-version 1.11.6
```

Die Dauer des Clusterupgrades hängt von der Anzahl der vorhanden Knoten ab und kann einige Minuten in Anspruch nehmen.

Überprüfen Sie nun mit dem Befehl [az aks show][az-aks-show], ob das Upgrade erfolgreich durchgeführt wurde:

```azurecli-interactive
az aks show --resource-group myResourceGroup --name myAKSCluster --output table
```

Die Ausgabe im folgenden Beispiel zeigt, dass im Cluster nun Version *1.11.6* ausgeführt wird:

```json
Name          Location    ResourceGroup    KubernetesVersion    ProvisioningState    Fqdn
------------  ----------  ---------------  -------------------  -------------------  ---------------------------------------------------------------
myAKSCluster  eastus      myResourceGroup  1.11.6               Succeeded            myaksclust-myresourcegroup-19da35-90efab95.hcp.eastus.azmk8s.io
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurde gezeigt, wie Sie einen vorhandenen AKS-Cluster aktualisieren. In verschiedenen Tutorials können Sie mehr über die Bereitstellung und Verwaltung von AKS-Clustern erfahren.

> [!div class="nextstepaction"]
> [AKS-Tutorials][aks-tutorial-prepare-app]

<!-- LINKS - external -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-get-upgrades]: /cli/azure/aks#az-aks-get-upgrades
[az-aks-upgrade]: /cli/azure/aks#az-aks-upgrade
[az-aks-show]: /cli/azure/aks#az-aks-show
