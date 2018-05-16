---
title: Erstellen eines internen Lastenausgleichs mit Azure Kubernetes Service (AKS)
description: Verwenden eines internen Lastenausgleichs mit Azure Kubernetes Service (AKS)
services: container-service
author: neilpeterson
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 3/29/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: a1a5d6d455086ee34767e92b277936840717bcd6
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
---
# <a name="use-an-internal-load-balancer-with-azure-kubernetes-service-aks"></a>Verwenden eines internen Lastenausgleichs mit Azure Kubernetes Service (AKS)

Durch einen internen Lastenausgleich können Anwendungen, die im gleichen virtuellen Netzwerk wie der Kubernetes-Cluster ausgeführt werden, auf einen Kubernetes-Dienst zugreifen. Dieses Dokument bietet Informationen zum Erstellen eines internen Lastenausgleichs mit Azure Kubernetes Service (AKS).

## <a name="create-internal-load-balancer"></a>Erstellen eines internen Lastenausgleichs

Um einen internen Lastenausgleich zu erstellen, erstellen Sie ein Dienstmanifest mit dem Diensttyp `LoadBalancer` und der Anmerkung `azure-load-balancer-internal`, wie im folgenden Beispiel dargestellt.

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

Nach der Bereitstellung wird ein Azure-Lastenausgleich erstellt und im gleichen virtuellen Netzwerk wie der AKS-Cluster verfügbar gemacht.

![Abbildung des internen AKS-Lastenausgleichs](media/internal-lb/internal-lb.png)

Beim Abrufen der Dienstdetails ist die IP-Adresse in der Spalte `EXTERNAL-IP` die IP-Adresse des internen Lastenausgleichs.

```console
$ kubectl get service azure-vote-front

NAME               TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.248.59   10.240.0.7    80:30555/TCP   10s
```

## <a name="specify-an-ip-address"></a>Angeben einer IP-Adresse

Wenn Sie eine bestimmte IP-Adresse für den internen Lastenausgleich verwenden möchten, fügen Sie der Spezifikation des Lastenausgleichs die Eigenschaft `loadBalancerIP` hinzu. Die angegebene IP-Adresse muss sich im gleichen Subnetz wie der AKS-Cluster befinden und darf keiner Ressource zugewiesen sein.

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

Wenn Sie die Dienstdetails abrufen, sollte die IP-Adresse von `EXTERNAL-IP` der angegebenen IP-Adresse entsprechen.

```console
$ kubectl get service azure-vote-front

NAME               TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.184.168   10.240.0.25   80:30225/TCP   4m
```

## <a name="delete-the-load-balancer"></a>Löschen des Lastenausgleichs

Wenn alle Dienste, die den internen Lastenausgleich verwenden, gelöscht wurden, wird auch der Lastenausgleich selbst gelöscht.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über Kubernetes-Dienste in der [Kubernetes-Dienstdokumentation][kubernetes-services].

<!-- LINKS - External -->
[kubernetes-services]: https://kubernetes.io/docs/concepts/services-networking/service/