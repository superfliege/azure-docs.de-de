---
title: Erstellen eines internen Lastenausgleichs mit Azure Kubernetes Service (AKS)
description: Erfahren Sie, wie Sie einen internen Lastenausgleich erstellen und verwenden, um Ihre Dienste über Azure Kubernetes Service (AKS) verfügbar zu machen.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 07/12/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 123fc08995416e0ff9c7e12a526deadc34b3a4a2
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2018
ms.locfileid: "39001394"
---
# <a name="use-an-internal-load-balancer-with-azure-kubernetes-service-aks"></a>Verwenden eines internen Lastenausgleichs mit Azure Kubernetes Service (AKS)

Durch einen internen Lastenausgleich können Anwendungen, die im gleichen virtuellen Netzwerk wie der Kubernetes-Cluster ausgeführt werden, auf einen Kubernetes-Dienst zugreifen. In diesem Artikel erfahren Sie, wie Sie einen internen Lastenausgleich mit Azure Kubernetes Service (AKS) erstellen und verwenden. Azure Load Balancer ist in zwei SKUs verfügbar: Basic und Standard. AKS wird in der SKU „Basic“ angeboten.

## <a name="create-an-internal-load-balancer"></a>Erstellen eines internen Load Balancers

Erstellen Sie wie im folgenden Beispiel gezeigt ein Dienstmanifest mit dem Diensttyp *LoadBalancer* und der Anmerkung *azure-load-balancer-internal*, um einen internen Lastenausgleich zu erstellen:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-internal: "true"
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

Nach der Bereitstellung mit `kubetctl apply` wird ein Azure-Lastenausgleich erstellt und im gleichen virtuellen Netzwerk wie der AKS-Cluster verfügbar gemacht.

![Abbildung des internen AKS-Lastenausgleichs](media/internal-lb/internal-lb.png)

Beim Anzeigen der Dienstdetails entspricht die IP-Adresse in der Spalte *EXTERNAL-IP* wie im folgenden Beispiel gezeigt der IP-Adresse des internen Lastenausgleichs:

```
$ kubectl get service azure-vote-front

NAME               TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.248.59   10.240.0.7    80:30555/TCP   10s
```

## <a name="specify-an-ip-address"></a>Angeben einer IP-Adresse

Wenn Sie eine bestimmte IP-Adresse für den internen Lastenausgleich verwenden möchten, fügen Sie der Spezifikation des Lastenausgleichs die Eigenschaft *loadBalancerIP* hinzu. Die angegebene IP-Adresse muss sich im gleichen Subnetz wie der AKS-Cluster befinden und darf keiner Ressource zugewiesen sein.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-internal: "true"
spec:
  type: LoadBalancer
  loadBalancerIP: 10.240.0.25
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

Wenn Sie die Dienstdetails anzeigen, entspricht die IP-Adresse von *EXTERNAL-IP* der angegebenen IP-Adresse:

```
$ kubectl get service azure-vote-front

NAME               TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.184.168   10.240.0.25   80:30225/TCP   4m
```

## <a name="use-private-networks"></a>Verwenden privater Netzwerke

Beim Erstellen des AKS-Clusters können Sie erweiterte Netzwerkeinstellungen angeben. Dadurch können Sie den Cluster in einem vorhandenen virtuellen Azure-Netzwerk und in Subnetzen bereitstellen. In einem Szenario wird der AKS-Cluster in einem privaten Netzwerk bereitgestellt, das mit Ihrer lokalen Umgebung verbunden ist, und nur intern zugängliche Dienste werden ausgeführt. Weitere Informationen finden Sie unter [Erweiterte Netzwerkkonfiguration in AKS][advanced-networking].

Wenn Sie einen internen Lastenausgleich in einem AKS-Cluster bereitstellen möchten, der ein privates Netzwerk verwendet, sind keine Änderungen an den vorherigen Schritten erforderlich. Der Lastenausgleich wird in derselben Ressourcengruppe wie der AKS-Cluster erstellt. Dabei wird er jedoch wie im folgenden Beispiel gezeigt mit Ihrem privaten virtuellen Netzwerk und Subnetz verbunden:

```
$ kubectl get service azure-vote-front

NAME               TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.1.15.188   10.0.0.35     80:31669/TCP   1m
```

> [!NOTE]
> Möglicherweise müssen Sie dem Dienstprinzipal für den AKS-Cluster die Rolle *Netzwerkmitwirkender* für die Ressourcengruppe gewähren, in der die Ressourcen Ihres virtuellen Azure-Netzwerks bereitgestellt werden. Verwenden Sie zum Anzeigen des Dienstprinzipals den Befehl [az aks show][az-aks-show] wie im Beispiel `az aks show --resource-group myResourceGroup --name myAKSCluster --query "servicePrincipalProfile.clientId"`. Verwenden Sie zum Erstellen einer Rollenzuweisung den Befehl [az role assignment create][az-role-assignment-create].

## <a name="specify-a-different-subnet"></a>Angeben eines anderen Subnetzes

Fügen Sie zum Angeben eines Subnetzes für den Lastenausgleich Ihrem Dienst die Anmerkung *azure-load-balancer-internal-subnet* hinzu. Das angegebene Subnetz muss sich dabei im gleichen virtuellen Netzwerk wie Ihr AKS-Cluster befinden. Nach der Bereitstellung ist die Adresse *EXTERNAL-IP* des Lastenausgleichs Teil des angegebenen Subnetzes.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-internal: "true"
    service.beta.kubernetes.io/azure-load-balancer-internal-subnet: "apps-subnet"
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

## <a name="delete-the-load-balancer"></a>Löschen des Lastenausgleichs

Wenn alle Dienste, die den internen Lastenausgleich verwenden, gelöscht werden, wird auch der Lastenausgleich selbst gelöscht.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über Kubernetes-Dienste in der [Kubernetes-Dienstdokumentation][kubernetes-services].

<!-- LINKS - External -->
[kubernetes-services]: https://kubernetes.io/docs/concepts/services-networking/service/

<!-- LINKS - Internal -->
[advanced-networking]: networking-overview.md
[deploy-advanced-networking]: networking-overview.md#configure-networking---cli
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create