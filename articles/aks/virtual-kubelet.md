---
title: Ausführen von Virtual Kubelet in einem Azure Kubernetes Service-Cluster (AKS)
description: Erfahren Sie, wie Sie Virtual Kubelet mit Azure Kubernetes Service (AKS) verwenden, um Linux- und Windows-Container in Azure Container Instances auszuführen.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 08/14/2018
ms.author: iainfou
ms.openlocfilehash: f613fb9bd3e9cf6d070b34403bab617e23261c56
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47226444"
---
# <a name="use-virtual-kubelet-with-azure-kubernetes-service-aks"></a>Verwenden von Virtual Kubelet mit Azure Kubernetes Service (AKS)

Azure Container Instances (ACI) bietet eine gehostete Umgebung zum Ausführen von Containern in Azure. Mit ACI ist es nicht erforderlich, die zugrunde liegende Computeinfrastruktur zu verwalten, denn Azure übernimmt diese Verwaltung für Sie. Wenn Sie Container in ACI ausführen, wird sekundengenau jeder ausgeführte Container abgerechnet.

Bei Verwendung des Anbieters für virtuelle Kubelets in Azure Container Instances können sowohl Linux- als auch Windows-Container auf einer Containerinstanz so eingeplant werden, als ob es sich um einen Standard-Kubernetes-Knoten handelt. Mit dieser Konfiguration können Sie sowohl die Vorteile von Kubernetes als auch die Verwaltungsfunktionen und den Kostenvorteil von Containerinstanzen nutzen.

> [!NOTE]
> „Virtual Kubelet“ ist ein experimentelles Open Source-Projekt und sollte als solches verwendet werden. Weitere Informationen finden Sie im [Virtual Kubelet-GitHub-Projekt][vk-github]. Dort können Sie einen Beitrag leisten, Fragen stellen und mehr über virtuelle Kubelets erfahren.

Dieses Dokument beschreibt die Konfiguration des virtuellen Kubelets für Containerinstanzen in AKS.

## <a name="prerequisite"></a>Voraussetzung

In diesem Dokument wird davon ausgegangen, dass Sie über einen AKS-Cluster verfügen. Wenn Sie einen AKS-Cluster benötigen, finden Sie weitere Informationen im [Schnellstart zu Azure Kubernetes Service (AKS)][aks-quick-start].

Außerdem benötigen Sie mindestens die Azure CLI-Version **2.0.33** oder höher. Führen Sie `az --version` aus, um die Version zu finden. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sei bei Bedarf unter [Installieren der Azure CLI](/cli/azure/install-azure-cli).

Zum Installieren von Virtual Kubelet ist außerdem [Helm](https://docs.helm.sh/using_helm/#installing-helm) erforderlich.

### <a name="for-rbac-enabled-clusters"></a>Für RBAC-fähige Cluster

Wenn Ihr AKS-Cluster RBAC-fähig ist, müssen Sie ein Dienstkonto und eine Rollenbindung für die Verwendung mit Tiller erstellen. Weitere Informationen finden Sie unter [Helm: Rollenbasierte Zugriffssteuerung][helm-rbac]. Um ein Dienstkonto und eine Rollenbindung zu erstellen, erstellen Sie eine Datei mit dem Namen *rbac-virtual-kubelet.yaml*, und fügen Sie dann die folgende Definition ein:

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tiller
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: tiller
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
  - kind: ServiceAccount
    name: tiller
    namespace: kube-system
```

Wenden Sie das Dienstkonto und die Bindung mit [kubectl apply][kubectl-apply] an, und geben Sie Ihre Datei *rbac-virtual-kubelet.yaml* an, wie im folgenden Beispiel gezeigt:

```
$ kubectl apply -f rbac-virtual-kubelet.yaml

clusterrolebinding.rbac.authorization.k8s.io/tiller created
```

Konfigurieren Sie Helm für die Verwendung des tiller-Dienstkontos:

```console
helm init --service-account tiller
```

Sie können nun mit der Installation von Virtual Kubelet in Ihrem AKS-Cluster fortfahren.

## <a name="installation"></a>Installation

Verwenden Sie den Befehl [az so Install-Connector][aks-install-connector], um Virtual Kubelet zu installieren. Im folgenden Beispiel wird ein Linux- und ein Windows-Connector bereitgestellt.

```azurecli-interactive
az aks install-connector --resource-group myAKSCluster --name myAKSCluster --connector-name virtual-kubelet --os-type Both
```

Diese Argumente sind für den `aks install-connector`-Befehl verfügbar.

| Argument: | BESCHREIBUNG | Erforderlich |
|---|---|:---:|
| `--connector-name` | Name des ACI-Connectors| JA |
| `--name``-n` | Name des verwalteten Clusters | JA |
| `--resource-group``-g` | Name der Ressourcengruppe | JA |
| `--os-type` | Betriebssystemtyp der Containerinstanzen. Zulässige Werte: beide, Linux, Windows. Standard: Linux | Nein  |
| `--aci-resource-group` | Ressourcengruppe, in der die ACI-Containergruppen erstellt werden sollen. | Nein  |
| `--location``-l` | Speicherort zum Erstellen der ACI-Containergruppen | Nein  |
| `--service-principal` | Dienstprinzipal für die Authentifizierung bei Azure-APIs | Nein  |
| `--client-secret` | Dem Dienstprinzipal zugeordnetes Geheimnis | Nein  |
| `--chart-url` | URL eines Helm-Diagramms zur Installation eines ACI-Connectors | Nein  |
| `--image-tag` | Imagetag des Containerimages für das virtuelle Kubelet | Nein  |

## <a name="validate-virtual-kubelet"></a>Überprüfen der Virtual Kubelet-Installation

Um zu überprüfen, ob Virtual Kubelet installiert wurde, geben Sie eine Liste der Kubernetes-Knoten mit dem Befehl [kubectl get nodes][kubectl-get] zurück.

```
$ kubectl get nodes

NAME                                    STATUS    ROLES     AGE       VERSION
aks-nodepool1-23443254-0                Ready     agent     16d       v1.9.6
aks-nodepool1-23443254-1                Ready     agent     16d       v1.9.6
aks-nodepool1-23443254-2                Ready     agent     16d       v1.9.6
virtual-kubelet-virtual-kubelet-linux   Ready     agent     4m        v1.8.3
virtual-kubelet-virtual-kubelet-win     Ready     agent     4m        v1.8.3
```

## <a name="run-linux-container"></a>Ausführen eines Linux-Containers

Erstellen Sie eine Datei namens „`virtual-kubelet-linux.yaml`“, und fügen Sie den folgenden YAML-Code ein. Ersetzen Sie den Wert `kubernetes.io/hostname` durch den Namen des Virtual Kubelet-Linux-Knotens. Beachten Sie, dass [nodeSelector][node-selector] und [toleration][toleration] verwendet werden, um den Container auf dem Knoten zu planen.

```yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: aci-helloworld
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: aci-helloworld
    spec:
      containers:
      - name: aci-helloworld
        image: microsoft/aci-helloworld
        ports:
        - containerPort: 80
      nodeSelector:
        kubernetes.io/hostname: virtual-kubelet-virtual-kubelet-linux
      tolerations:
      - key: virtual-kubelet.io/provider
        operator: Equal
        value: azure
        effect: NoSchedule
```

Führen Sie die Anwendung mithilfe des Befehls [kubectl create][kubectl-create] aus.

```console
kubectl create -f virtual-kubelet-linux.yaml
```

Verwenden Sie den Befehl [kubectl get pods][kubectl-get] mit dem `-o wide`-Argument, um eine Liste von Pods mit dem geplanten Knoten auszugeben. Beachten Sie, dass der `aci-helloworld`-Pod auf dem `virtual-kubelet-virtual-kubelet-linux`-Knoten geplant wurde.

```
$ kubectl get pods -o wide

NAME                                READY     STATUS    RESTARTS   AGE       IP             NODE
aci-helloworld-2559879000-8vmjw     1/1       Running   0          39s       52.179.3.180   virtual-kubelet-virtual-kubelet-linux
```

## <a name="run-windows-container"></a>Ausführen eines Windows-Containers

Erstellen Sie eine Datei namens „`virtual-kubelet-windows.yaml`“, und fügen Sie den folgenden YAML-Code ein. Ersetzen Sie den Wert `kubernetes.io/hostname` durch den Namen des Virtual Kubelet-Windows-Knotens. Beachten Sie, dass [nodeSelector][node-selector] und [toleration][toleration] verwendet werden, um den Container auf dem Knoten zu planen.

```yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: nanoserver-iis
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: nanoserver-iis
    spec:
      containers:
      - name: nanoserver-iis
        image: microsoft/iis:nanoserver
        ports:
        - containerPort: 80
      nodeSelector:
        kubernetes.io/hostname: virtual-kubelet-virtual-kubelet-win
      tolerations:
      - key: virtual-kubelet.io/provider
        operator: Equal
        value: azure
        effect: NoSchedule
```

Führen Sie die Anwendung mithilfe des Befehls [kubectl create][kubectl-create] aus.

```console
kubectl create -f virtual-kubelet-windows.yaml
```

Verwenden Sie den Befehl [kubectl get pods][kubectl-get] mit dem `-o wide`-Argument, um eine Liste von Pods mit dem geplanten Knoten auszugeben. Beachten Sie, dass der `nanoserver-iis`-Pod auf dem `virtual-kubelet-virtual-kubelet-win`-Knoten geplant wurde.

```
$ kubectl get pods -o wide

NAME                                READY     STATUS    RESTARTS   AGE       IP             NODE
nanoserver-iis-868bc8d489-tq4st     1/1       Running   8         21m       138.91.121.91   virtual-kubelet-virtual-kubelet-win
```

## <a name="remove-virtual-kubelet"></a>Entfernen von Virtual Kubelet

Verwenden Sie den Befehl [az aks remove-connector][aks-remove-connector], um Virtual Kubelet zu entfernen. Ersetzen Sie die Argumentwerte durch die Namen des Connectors, des AKS-Clusters und der AKS-Clusterressourcengruppe.

```azurecli-interactive
az aks remove-connector --resource-group myAKSCluster --name myAKSCluster --connector-name virtual-kubelet
```

> [!NOTE]
> Wenn beim Entfernen der beiden Betriebssystem-Connectors Fehler auftreten oder wenn Sie nur den Windows- oder Linux-Betriebssystem-Connector entfernen möchten, können Sie manuell den Betriebssystemtyp angeben. Fügen Sie den `--os-type`-Parameter zum vorherigen `az aks remove-connector`-Befehl hinzu, und geben Sie `Windows` oder `Linux` an.

## <a name="next-steps"></a>Nächste Schritte

Informationen zu möglichen Problemen mit dem virtuellen Kubelet finden Sie unter [Bekannte Probleme und Problemumgehungen][vk-troubleshooting]. Wenn Sie Probleme mit dem virtuellen Kubelet melden möchten, [öffnen Sie ein GitHub-Problem][vk-issues].

Weitere Informationen zu virtuellen Kubelets finden Sie im [Virtual Kubelet-GitHub-Projekt][vk-github].

<!-- LINKS - internal -->
[aks-quick-start]: ./kubernetes-walkthrough.md
[aks-remove-connector]: /cli/azure/aks#az-aks-remove-connector
[az-container-list]: /cli/azure/aks#az-aks-list
[aks-install-connector]: /cli/azure/aks#az-aks-install-connector

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-get]: https://kubernetes.io/docs/user-guide/kubectl/v1.8/#get
[node-selector]:https://kubernetes.io/docs/concepts/configuration/assign-pod-node/
[toleration]: https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/
[vk-github]: https://github.com/virtual-kubelet/virtual-kubelet
[helm-rbac]: https://docs.helm.sh/using_helm/#role-based-access-control
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[vk-troubleshooting]: https://github.com/virtual-kubelet/virtual-kubelet#known-quirks-and-workarounds
[vk-issues]: https://github.com/virtual-kubelet/virtual-kubelet/issues
