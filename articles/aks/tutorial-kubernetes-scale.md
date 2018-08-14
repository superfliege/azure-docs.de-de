---
title: Tutorial zu Kubernetes in Azure – Skalieren von Anwendungen
description: AKS-Tutorial – Skalieren einer Anwendung
services: container-service
author: dlepow
manager: jeconnoc
ms.service: container-service
ms.topic: tutorial
ms.date: 02/22/2018
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 61654ae972965800909544554cc93dae511e1ff1
ms.sourcegitcommit: fc5555a0250e3ef4914b077e017d30185b4a27e6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/03/2018
ms.locfileid: "39480271"
---
# <a name="tutorial-scale-application-in-azure-kubernetes-service-aks"></a>Tutorial: Skalieren einer Anwendung in Azure Kubernetes Service (AKS)

Wenn Sie die Tutorials durchgearbeitet haben, verfügen Sie über einen funktionsfähigen Kubernetes-Cluster in AKS und haben die Azure Voting App bereitgestellt.

In diesem Tutorial – Teil 5 von 7 – skalieren Sie die Pods in der App horizontal hoch und testen das Feature der automatischen Skalierung von Pods. Sie erfahren auch, wie Sie die Anzahl von Azure-VM-Knoten skalieren, um die Clusterkapazität für das Hosten von Workloads zu ändern. Folgende Aufgaben wurden ausgeführt:

> [!div class="checklist"]
> * Skalieren der Kubernetes-Azure-Knoten
> * Manuelles Skalieren von Kubernetes-Pods
> * Konfigurieren der automatischen Skalierung von Pods, die das Front-End einer App ausführen

In den nachfolgenden Tutorials wird die Anwendung Azure Vote auf eine neue Version aktualisiert.

## <a name="before-you-begin"></a>Voraussetzungen

In vorherigen Tutorials wurde eine Anwendung in ein Containerimage gepackt, dieses Image wurde in Azure Container Registry hochgeladen, und es wurde ein Kubernetes-Cluster erstellt. Anschließend wurde die Anwendung im Kubernetes-Cluster ausgeführt.

Wenn Sie diese Schritte nicht ausgeführt haben und dies jetzt nachholen möchten, kehren Sie zum [Tutorial 1 – Erstellen von Containerimages][aks-tutorial-prepare-app] zurück.

## <a name="manually-scale-pods"></a>Manuelles Skalieren von Pods

Bisher wurden die Front-End-Instanz und die Redis-Instanz der Azure Voting-App mit jeweils einem Replikat bereitgestellt. Führen Sie zum Überprüfen den Befehl [kubectl get][kubectl-get] aus.

```azurecli
kubectl get pods
```

Ausgabe:

```
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2549686872-4d2r5   1/1       Running   0          31m
azure-vote-front-848767080-tf34m   1/1       Running   0          31m
```

Ändern Sie die Anzahl von Pods in der `azure-vote-front`-Bereitstellung manuell mithilfe des Befehls [kubectl scale][kubectl-scale]. In diesem Beispiel wird die Anzahl auf 5 erhöht.

```azurecli
kubectl scale --replicas=5 deployment/azure-vote-front
```

Führen Sie [kubectl get pods][kubectl-get] aus, um zu überprüfen, ob Kubernetes die Pods erstellt. Nach ca. einer Minute werden die zusätzlichen Pods ausgeführt:

```azurecli
kubectl get pods
```

Ausgabe:

```
NAME                                READY     STATUS    RESTARTS   AGE
azure-vote-back-2606967446-nmpcf    1/1       Running   0          15m
azure-vote-front-3309479140-2hfh0   1/1       Running   0          3m
azure-vote-front-3309479140-bzt05   1/1       Running   0          3m
azure-vote-front-3309479140-fvcvm   1/1       Running   0          3m
azure-vote-front-3309479140-hrbf2   1/1       Running   0          15m
azure-vote-front-3309479140-qphz8   1/1       Running   0          3m
```

## <a name="autoscale-pods"></a>Automatisches Skalieren von Pods

Kubernetes unterstützt [die automatische horizontale Skalierung von Pods][kubernetes-hpa], um die Anzahl von Pods in einer Bereitstellung je nach CPU-Nutzung und anderen ausgewählten Metriken anzupassen. Der [Metrikserver][metrics-server] wird verwendet, um die Ressourcennutzung für Kubernetes bereitzustellen. Zum Installieren des Metrikservers klonen Sie das GitHub-Repository `metrics-server` und installieren die Beispielressourcendefinitionen. Informationen zum Anzeigen des Inhalts dieser YAML-Definitionen finden Sie unter [Metrics Server for Kubernetes 1.8+][metrics-server-github] (Metrikserver für Kubernetes 1.8+).

```console
git clone https://github.com/kubernetes-incubator/metrics-server.git
kubectl create -f metrics-server/deploy/1.8+/
```

Um die automatische Skalierungsfunktion zu verwenden, müssen CPU-Anforderungen und -Grenzwerte Ihrer Pods definiert sein. In der `azure-vote-front`-Bereitstellung fordert der Front-End-Container 0,25 CPUs an, und es gilt ein Grenzwert von 0,5 CPUs. Die Einstellungen sehen folgendermaßen aus:

```YAML
resources:
  requests:
     cpu: 250m
  limits:
     cpu: 500m
```

Das folgende Beispiel verwendet den Befehl [kubectl autoscale][kubectl-autoscale], um die Anzahl von Pods in der `azure-vote-front`-Bereitstellung automatisch zu skalieren. Wenn die CPU-Nutzung 50 % übersteigt, erhöht die automatische Skalierungsfunktion die Anzahl von Pods auf den Maximalwert 10.

```azurecli
kubectl autoscale deployment azure-vote-front --cpu-percent=50 --min=3 --max=10
```

Führen Sie den folgenden Befehl aus, um den Status der automatischen Skalierungsfunktion anzuzeigen:

```azurecli
kubectl get hpa
```

Ausgabe:

```
NAME               REFERENCE                     TARGETS    MINPODS   MAXPODS   REPLICAS   AGE
azure-vote-front   Deployment/azure-vote-front   0% / 50%   3         10        3          2m
```

Nach einigen Minuten mit minimaler Last in der Azure Voting-App sinkt die Anzahl von Podreplikaten automatisch auf 3.

## <a name="manually-scale-aks-nodes"></a>Manuelles Skalieren von AKS-Knoten

Wenn Sie Ihren Kubernetes-Cluster mithilfe der Befehle im vorherigen Tutorial erstellt haben, verfügt er über einen Knoten. Sie können die Anzahl der Knoten manuell anpassen, wenn Sie größere oder kleinere Containerworkloads in Ihrem Cluster planen.

Im folgenden Beispiel wird die Anzahl von Knoten im Kubernetes-Cluster *myAKSCluster* auf drei erhöht. Diese Ausführung dieses Befehls dauert einige Minuten.

```azurecli
az aks scale --resource-group=myResourceGroup --name=myAKSCluster --node-count 3
```

Die Ausgabe sieht in etwa wie folgt aus:

```
"agentPoolProfiles": [
  {
    "count": 3,
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
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie verschiedene Skalierungsfunktionen in Ihrem Kubernetes-Cluster verwendet. Sie haben folgende Aufgaben ausgeführt:

> [!div class="checklist"]
> * Manuelles Skalieren von Kubernetes-Pods
> * Konfigurieren der automatischen Skalierung von Pods, die das Front-End einer App ausführen
> * Skalieren der Kubernetes-Azure-Knoten

Fahren Sie mit dem nächsten Tutorial fort, um mehr über die Aktualisierung von Anwendungen im Cluster zu erfahren.

> [!div class="nextstepaction"]
> [Aktualisieren einer Anwendung in Kubernetes][aks-tutorial-update-app]

<!-- LINKS - external -->
[kubectl-autoscale]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#autoscale
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-scale]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#scale
[kubernetes-hpa]: https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/
[metrics-server-github]: https://github.com/kubernetes-incubator/metrics-server/tree/master/deploy/1.8%2B
[metrics-server]: https://kubernetes.io/docs/tasks/debug-application-cluster/core-metrics-pipeline/

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-update-app]: ./tutorial-kubernetes-app-update.md
