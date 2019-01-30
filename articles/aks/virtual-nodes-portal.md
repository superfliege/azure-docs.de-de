---
title: Erstellen von virtuellen Knoten mit dem Portal in Azure Kubernetes Service (AKS)
description: Erfahren Sie, wie Sie das Azure-Portal verwenden, um einen Azure Kubernetes Service-Cluster (AKS) zu erstellen, der virtuelle Knoten zum Ausführen von Pods verwendet.
services: container-service
author: iainfoulds
ms.service: container-service
ms.date: 12/03/2018
ms.author: iainfou
ms.openlocfilehash: a47910083083787000b749a0b5b3256df5e702c8
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2019
ms.locfileid: "54845400"
---
# <a name="create-and-configure-an-azure-kubernetes-services-aks-cluster-to-use-virtual-nodes-in-the-azure-portal"></a>Erstellen und Konfigurieren eines AKS-Clusters zur Verwendung von virtuellen Knoten im Azure-Portal

Um Anwendungsworkloads in einem AKS-Cluster (Azure Kubernetes Service) schnell bereitzustellen, können Sie virtuelle Knoten verwenden. Mit virtuellen Knoten lassen sich Pods schnell bereitstellen, und Sie zahlen für die Ausführungsdauer nur auf Sekundenbasis. In einem Skalierungsszenario müssen Sie nicht warten, bis die Autoskalierung des Kubernetes-Clusters VM-Computeknoten bereitstellt, um die zusätzlichen Pods auszuführen. Dieser Artikel zeigt, wie Sie die virtuellen Netzwerkressourcen und einen AKS-Cluster mit aktivierten virtuellen Knoten erstellen und konfigurieren.

> [!IMPORTANT]
> Virtuelle Knoten für AKS befinden sich derzeit in der **Vorschau**. Vorschauversionen werden Ihnen zur Verfügung gestellt, wenn Sie die [zusätzlichen Nutzungsbedingungen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) akzeptieren. Einige Aspekte dieses Features werden bis zur allgemeinen Verfügbarkeit unter Umständen noch geändert.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.

## <a name="create-an-aks-cluster"></a>Erstellen eines AKS-Clusters

Klicken Sie im Azure-Portal oben links auf **Ressource erstellen** > **Kubernetes Service**.

Konfigurieren Sie auf der Seite **Grundlagen** die folgenden Optionen:

- *PROJEKTDETAILS*: Wählen Sie ein Azure-Abonnement und dann eine Azure-Ressourcengruppe aus, bzw. erstellen Sie eine Ressourcengruppe, z.B. *myResourceGroup*. Geben Sie unter **Kubernetes cluster name** (Name des Kubernetes-Clusters) einen Namen ein, etwa *myAKSCluster*.
- *CLUSTERDETAILS*: Wählen Sie eine Region, eine Kubernetes-Version und ein DNS-Namenspräfix für den AKS-Cluster aus.
- *SKALIERUNG*: Wählen Sie eine VM-Größe für die AKS-Knoten aus. Die VM-Größe kann **nicht** geändert werden, sobald ein AKS-Cluster bereitgestellt wurde.
    - Wählen Sie die Anzahl von Knoten für die Bereitstellung im Cluster aus. Legen Sie für diesen Artikel **Knotenanzahl** auf *1* fest. Die Knotenanzahl **kann** nach der Clusterbereitstellung angepasst werden.
    - Wählen Sie unter **Virtuelle Knoten** die Option *Aktiviert* aus.

![Erstellen von AKS-Clustern und Aktivieren der virtuellen Knoten](media/virtual-nodes-portal/enable-virtual-nodes.png)

Standardmäßig wird ein Azure Active Directory-Dienstprinzipal erstellt. Dieser Dienstprinzipal wird für die Clusterkommunikation und die Integration in andere Azure-Dienste verwendet.

Der Cluster ist auch für den erweiterten Netzwerkbetrieb konfiguriert. Die virtuellen Knoten sind so konfiguriert, dass sie ihr eigenes virtuelles Azure-Netzwerksubnetz verwenden. Dieses Subnetz verfügt über delegierte Berechtigungen, um Azure-Ressourcen zwischen dem AKS-Cluster zu verbinden. Wenn Sie noch kein Subnetz delegiert haben, erstellt und konfiguriert das Azure-Portal das virtuelle Netzwerk und Subnetz von Azure für die Verwendung mit den virtuellen Knoten.

Klicken Sie auf **Überprüfen + erstellen**. Wählen Sie nach Abschluss der Validierung **Erstellen** aus.

Es dauert einige Minuten, bis der AKS-Clusters erstellt wurde und für die Verwendung bereit ist.

## <a name="connect-to-the-cluster"></a>Verbinden mit dem Cluster

Azure Cloud Shell ist eine kostenlose interaktive Shell, mit der Sie die Schritte in diesem Artikel ausführen können. Sie verfügt über allgemeine vorinstallierte Tools und ist für die Verwendung mit Ihrem Konto konfiguriert. Verwenden Sie zum Verwalten eines Kubernetes-Clusters den Kubernetes-Befehlszeilenclient [kubectl][kubectl]. Der `kubectl`-Client ist in Azure Cloud Shell vorinstalliert.

Wählen Sie zum Öffnen von Cloud Shell oben rechts in einem Codeblock die Option **Ausprobieren** aus. Sie können Cloud Shell auch auf einer separaten Browserregisterkarte starten, indem Sie zu [https://shell.azure.com/bash](https://shell.azure.com/bash) navigieren. Wählen Sie **Kopieren**, um die Blöcke mit dem Code zu kopieren. Fügen Sie ihn anschließend in Cloud Shell ein, und drücken Sie die EINGABETASTE, um ihn auszuführen.

Mit dem Befehl [az aks get-credentials][az-aks-get-credentials] können Sie `kubectl` für die Verbindungsherstellung mit Ihrem Kubernetes-Cluster konfigurieren. Im folgenden Beispiel werden Anmeldeinformationen für den Clusternamen *myAKSCluster* in der Ressourcengruppe *myResourceGroup* abgerufen:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Verwenden Sie zum Überprüfen der Verbindung mit Ihrem Cluster den Befehl [kubectl get][kubectl-get], um eine Liste der Clusterknoten zu erhalten.

```azurecli-interactive
kubectl get nodes
```

Die folgende Beispielausgabe zeigt den erstellten einzelnen VM-Knoten und dann den virtuellen Knoten für Linux, *virtual-node-aci-linux*:

```
$ kubectl get nodes

NAME                           STATUS    ROLES     AGE       VERSION
virtual-node-aci-linux         Ready     agent     28m       v1.11.2
aks-agentpool-14693408-0       Ready     agent     32m       v1.11.2
```

## <a name="deploy-a-sample-app"></a>Bereitstellen einer Beispiel-App

Erstellen Sie in der Azure Cloud Shell eine Datei mit dem Namen `virtual-node.yaml`, und fügen Sie den folgenden YAML-Code ein. Um den Container auf dem Knoten zu planen, werden Werte für [nodeSelector][node-selector] und [toleration][toleration] definiert. Mit dieser Einstellungen kann der Pod auf dem virtuellen Knoten geplant und bestätigt werden, dass das Feature erfolgreich aktiviert wurde.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: aci-helloworld
spec:
  replicas: 1
  selector:
    matchLabels:
      app: aci-helloworld
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
        kubernetes.io/role: agent
        beta.kubernetes.io/os: linux
        type: virtual-kubelet
      tolerations:
      - key: virtual-kubelet.io/provider
        operator: Exists
      - key: azure.com/aci
        effect: NoSchedule
```

Führen Sie die Anwendung mithilfe des Befehls [kubectl apply][kubectl-apply] aus.

```azurecli-interactive
kubectl apply -f virtual-node.yaml
```

Verwenden Sie den Befehl [kubectl get pods][kubectl-get] mit dem `-o wide`-Argument, um eine Liste von Pods und den geplanten Knoten auszugeben. Beachten Sie, dass der `virtual-node-helloworld`-Pod auf dem `virtual-node-linux`-Knoten geplant wurde.

```
$ kubectl get pods -o wide

NAME                                     READY     STATUS    RESTARTS   AGE       IP           NODE
virtual-node-helloworld-9b55975f-bnmfl   1/1       Running   0          4m        10.241.0.4   virtual-node-aci-linux
```

Dem Pod wird von dem Subnetz des virtuellen Azure-Netzwerks, das für die Verwendung mit virtuellen Knoten delegiert wurde, eine interne IP-Adresse zugewiesen.

> [!NOTE]
> Wenn Sie Images verwenden, die in der Azure Container Registry gespeichert sind, konfigurieren und verwenden Sie [ein Kubernetes-Geheimnis][acr-aks-secrets]. Eine aktuelle Einschränkung der Vorschau der virtuellen Knoten ist, dass Sie die integrierte Authentifizierung des Azure AD-Dienstprinzipals nicht verwenden können. Wenn Sie kein Geheimnis verwenden, können auf virtuellen Knoten gespeicherte Pods nicht starten und melden den Fehler `HTTP response status code 400 error code "InaccessibleImage"`.

## <a name="test-the-virtual-node-pod"></a>Testen des Pods des virtuellen Knotens

Um den Pod zu testen, der auf dem virtuellen Knoten ausgeführt werden soll, wechseln Sie in einem Webclient zur Demoanwendung. Da dem Pod eine interne IP-Adresse zugewiesen wurde, können Sie diese Konnektivität schnell von einem anderen Pod im AKS-Cluster aus testen. Erstellen Sie einen Testpod, und fügen Sie an diesen eine Terminalsitzung an:

```azurecli-interactive
kubectl run -it --rm virtual-node-test --image=debian
```

Installieren Sie `curl` mit `apt-get` im Pod:

```azurecli-interactive
apt-get update && apt-get install -y curl
```

Greifen Sie mithilfe von `curl` auf die Adresse Ihres Pods zu, z.B. *http://10.241.0.4*. Geben Sie Ihre eigene interne IP-Adresse an, die im vorherigen `kubectl get pods`-Befehl gezeigt wurde:

```azurecli-interactive
curl -L http://10.241.0.4
```

Die Demoanwendung wird angezeigt, wie in der folgenden verkürzten Beispielausgabe veranschaulicht:

```
$ curl -L 10.241.0.4

<html>
<head>
  <title>Welcome to Azure Container Instances!</title>
</head>
[...]
```

Schließen Sie die Terminalsitzung mit Ihrem Testpod mit `exit`. Wenn die Sitzung beendet ist, wird der Pod gelöscht.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurde ein Pod im virtuellen Knoten geplant, und dem Pod wurde ein private interne IP-Adresse zugewiesen. Sie können stattdessen auch eine Dienstbereitstellung erstellen und den Datenverkehr über ein Lastenausgleichsmodul oder einen Eingangscontroller an Ihren Pod weiterleiten. Weitere Informationen finden Sie unter [Create a basic ingress controller in AKS (Erstellen eines einfachen Eingangscontrollers in AKS)][aks-basic-ingress].

Virtuelle Knoten sind eine Komponente einer Skalierungslösung in AKS. Weitere Informationen zu Skalierungslösungen finden Sie in den folgenden Artikeln:

- [Verwenden der horizontalen automatischen Kubernetes-Podskalierung][aks-hpa]
- [Verwenden der automatischen Kubernetes-Clusterskalierung][aks-cluster-autoscaler]

<!-- LINKS - external -->
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[node-selector]:https://kubernetes.io/docs/concepts/configuration/assign-pod-node/
[toleration]: https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/
[azure-cni]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md

<!-- LINKS - internal -->
[aks-network]: ./networking-overview.md
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[aks-hpa]: tutorial-kubernetes-scale.md
[aks-cluster-autoscaler]: autoscaler.md
[aks-basic-ingress]: ingress-basic.md
[acr-aks-secrets]: ../container-registry/container-registry-auth-aks.md#access-with-kubernetes-secret

