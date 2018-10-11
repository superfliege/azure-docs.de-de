---
title: Kubernetes in Azure – Cluster Autoscaler
description: In diesem Artikel erfahren Sie, wie Sie Ihren Cluster mit Cluster Autoscaler in Azure Kubernetes Service (AKS) automatisch zur Erfüllung Ihres Bedarfs skalieren lassen können.
services: container-service
author: sakthivetrivel
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 07/19/18
ms.author: sakthivetrivel
ms.custom: mvc
ms.openlocfilehash: 3bac6534f43d62e6eb9381b8513025ba9117ed04
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2018
ms.locfileid: "48857005"
---
# <a name="cluster-autoscaler-on-azure-kubernetes-service-aks---preview"></a>Cluster Autoscaler in Azure Kubernetes Service (AKS) – Vorschauversion

Azure Kubernetes Service (AKS) ist eine flexible Lösung, mit der Sie einen verwalteten Kubernetes-Cluster in Azure bereitstellen können. Bei steigenden Ressourcenanforderungen kann Ihr Cluster mit Cluster Autoscaler zur Erfüllung dieses Bedarfs automatisch skaliert werden – basierend auf Einschränkungen, die Sie festlegen. Cluster Autoscaler (CA) ermöglicht dies durch die Skalierung Ihrer Agent-Knoten basierend auf ausstehenden Pods. Die Lösung prüft den Cluster in regelmäßigen Abständen auf ausstehende Pods oder leere Knoten und erhöht wenn möglich deren Größe. Standardmäßig prüft die Zertifizierungsstelle alle 10 Sekunden auf ausstehende Pods und entfernt einen Knoten, wenn dieser seit mehr als 10 Minuten nicht benötigt wurde. Wird die Lösung mit [Horizontal Pod Autoscaler](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) (HPA) verwendet, aktualisiert HPA bei Bedarf Podreplikate und -ressourcen. Wenn nach dieser Podskalierung nicht genügend Knoten oder nicht benötigte Knoten vorhanden sind, reagiert die Zertifizierungsstelle und plant die Pods in der neuen Knotengruppe.

In diesem Artikel wird beschrieben, wie Sie Cluster Autoscaler auf den Agent-Knoten bereitstellen. Da Cluster Autoscaler im kube-system-Namespace bereitgestellt wird, skaliert Cluster Autoscaler den Knoten, der diesen Pod ausführt, nicht zentral herunter.

> [!IMPORTANT]
> Die Cluster Autoscaler-Integration in Azure Kubernetes Service (AKS) befindet sich derzeit in der **Vorschauversion**. Vorschauversionen werden Ihnen zur Verfügung gestellt, wenn Sie die [zusätzlichen Nutzungsbedingungen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) akzeptieren. Einige Aspekte dieses Features werden bis zur allgemeinen Verfügbarkeit unter Umständen noch geändert.
>

## <a name="prerequisites"></a>Voraussetzungen

In diesem Dokument wird davon ausgegangen, dass Sie über einen RBAC-fähigen AKS-Cluster verfügen. Wenn Sie einen AKS-Cluster benötigen, finden Sie weitere Informationen im [Schnellstart zu Azure Kubernetes Service (AKS)][aks-quick-start].

 Für die Verwendung von Cluster Autoscaler muss Ihr Cluster mit Kubernetes v1.10.x oder höher verwendet werden und RBAC-fähig sein. Informationen zum Durchführen eines Upgrades für Ihren Cluster finden Sie im Artikel zum [Durchführen eines Upgrades für AKS-Cluster][aks-upgrade].

## <a name="gather-information"></a>Sammeln von Informationen

Um die Berechtigungen für Cluster Autoscaler zum Ausführen in Ihrem Cluster zu generieren, führen Sie das folgende Bash-Skript aus:

```sh
#! /bin/bash
ID=`az account show --query id -o json`
SUBSCRIPTION_ID=`echo $ID | tr -d '"' `

TENANT=`az account show --query tenantId -o json`
TENANT_ID=`echo $TENANT | tr -d '"' | base64`

read -p "What's your cluster name? " cluster_name
read -p "Resource group name? " resource_group

CLUSTER_NAME=`echo $cluster_name | base64`
RESOURCE_GROUP=`echo $resource_group | base64`

PERMISSIONS=`az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/$SUBSCRIPTION_ID" -o json`
CLIENT_ID=`echo $PERMISSIONS | sed -e 's/^.*"appId"[ ]*:[ ]*"//' -e 's/".*//' | base64`
CLIENT_SECRET=`echo $PERMISSIONS | sed -e 's/^.*"password"[ ]*:[ ]*"//' -e 's/".*//' | base64`

SUBSCRIPTION_ID=`echo $ID | tr -d '"' | base64 `

NODE_RESOURCE_GROUP=`az aks show --name $cluster_name  --resource-group $resource_group -o tsv --query 'nodeResourceGroup' | base64`

echo "---
apiVersion: v1
kind: Secret
metadata:
    name: cluster-autoscaler-azure
    namespace: kube-system
data:
    ClientID: $CLIENT_ID
    ClientSecret: $CLIENT_SECRET
    ResourceGroup: $RESOURCE_GROUP
    SubscriptionID: $SUBSCRIPTION_ID
    TenantID: $TENANT_ID
    VMType: QUtTCg==
    ClusterName: $CLUSTER_NAME
    NodeResourceGroup: $NODE_RESOURCE_GROUP
---"
```

Nach der Durchführung der Schritte im Skript gibt das Skript Ihre Details in Form eines Geheimnisses aus. Dies wird im Folgenden veranschaulicht:

```yaml
---
apiVersion: v1
kind: Secret
metadata:
  name: cluster-autoscaler-azure
  namespace: kube-system
data:
  ClientID: <base64-encoded-client-id>
  ClientSecret: <base64-encoded-client-secret>$
  ResourceGroup: <base64-encoded-resource-group>  SubscriptionID: <base64-encode-subscription-id>
  TenantID: <base64-encoded-tenant-id>
  VMType: QUtTCg==
  ClusterName: <base64-encoded-clustername>
  NodeResourceGroup: <base64-encoded-node-resource-group>
---
```

Rufen Sie anschließend den Namen Ihres Knotenpools ab, indem Sie den folgenden Befehl ausführen. 

```console
$ kubectl get nodes --show-labels
```

Ausgabe:

```console
NAME                       STATUS    ROLES     AGE       VERSION   LABELS
aks-nodepool1-37756013-0   Ready     agent     1h        v1.10.3   agentpool=nodepool1,beta.kubernetes.io/arch=amd64,beta.kubernetes.io/instance-type=Standard_DS1_v2,beta.kubernetes.io/os=linux,failure-domain.beta.kubernetes.io/region=eastus,failure-domain.beta.kubernetes.io/zone=0,kubernetes.azure.com/cluster=MC_[resource-group]\_[cluster-name]_[location],kubernetes.io/hostname=aks-nodepool1-37756013-0,kubernetes.io/role=agent,storageprofile=managed,storagetier=Premium_LRS
 ```

Extrahieren Sie dann den Wert der Bezeichnung **agentpool**. Der Standardname für den Knotenpool eines Clusters lautet „nodepool1“.

Nun können Sie anhand Ihres Geheimnisses und Knotenpools einen Ressourcenplan erstellen.

## <a name="create-a-deployment-chart"></a>Erstellen eines Ressourcenplans

Erstellen Sie eine Datei mit dem Namen `aks-cluster-autoscaler.yaml`, und fügen Sie den folgenden YAML-Code ein:

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  labels:
    k8s-addon: cluster-autoscaler.addons.k8s.io
    k8s-app: cluster-autoscaler
  name: cluster-autoscaler
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRole
metadata:
  name: cluster-autoscaler
  labels:
    k8s-addon: cluster-autoscaler.addons.k8s.io
    k8s-app: cluster-autoscaler
rules:
- apiGroups: [""]
  resources: ["events","endpoints"]
  verbs: ["create", "patch"]
- apiGroups: [""]
  resources: ["pods/eviction"]
  verbs: ["create"]
- apiGroups: [""]
  resources: ["pods/status"]
  verbs: ["update"]
- apiGroups: [""]
  resources: ["endpoints"]
  resourceNames: ["cluster-autoscaler"]
  verbs: ["get","update"]
- apiGroups: [""]
  resources: ["nodes"]
  verbs: ["watch","list","get","update"]
- apiGroups: [""]
  resources: ["pods","services","replicationcontrollers","persistentvolumeclaims","persistentvolumes"]
  verbs: ["watch","list","get"]
- apiGroups: ["extensions"]
  resources: ["replicasets","daemonsets"]
  verbs: ["watch","list","get"]
- apiGroups: ["policy"]
  resources: ["poddisruptionbudgets"]
  verbs: ["watch","list"]
- apiGroups: ["apps"]
  resources: ["statefulsets"]
  verbs: ["watch","list","get"]
- apiGroups: ["storage.k8s.io"]
  resources: ["storageclasses"]
  verbs: ["get", "list", "watch"]

---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: Role
metadata:
  name: cluster-autoscaler
  namespace: kube-system
  labels:
    k8s-addon: cluster-autoscaler.addons.k8s.io
    k8s-app: cluster-autoscaler
rules:
- apiGroups: [""]
  resources: ["configmaps"]
  verbs: ["create"]
- apiGroups: [""]
  resources: ["configmaps"]
  resourceNames: ["cluster-autoscaler-status"]
  verbs: ["delete","get","update"]

---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: cluster-autoscaler
  labels:
    k8s-addon: cluster-autoscaler.addons.k8s.io
    k8s-app: cluster-autoscaler
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-autoscaler
subjects:
  - kind: ServiceAccount
    name: cluster-autoscaler
    namespace: kube-system

---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: RoleBinding
metadata:
  name: cluster-autoscaler
  namespace: kube-system
  labels:
    k8s-addon: cluster-autoscaler.addons.k8s.io
    k8s-app: cluster-autoscaler
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: cluster-autoscaler
subjects:
  - kind: ServiceAccount
    name: cluster-autoscaler
    namespace: kube-system

---
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  labels:
    app: cluster-autoscaler
  name: cluster-autoscaler
  namespace: kube-system
spec:
  replicas: 1
  selector:
    matchLabels:
      app: cluster-autoscaler
  template:
    metadata:
      labels:
        app: cluster-autoscaler
    spec:
      serviceAccountName: cluster-autoscaler
      containers:
      - image: gcr.io/google-containers/cluster-autoscaler:v1.2.2
        imagePullPolicy: Always
        name: cluster-autoscaler
        resources:
          limits:
            cpu: 100m
            memory: 300Mi
          requests:
            cpu: 100m
            memory: 300Mi
        command:
        - ./cluster-autoscaler
        - --v=3
        - --logtostderr=true
        - --cloud-provider=azure
        - --skip-nodes-with-local-storage=false
        - --nodes=1:10:nodepool1
        env:
        - name: ARM_SUBSCRIPTION_ID
          valueFrom:
            secretKeyRef:
              key: SubscriptionID
              name: cluster-autoscaler-azure
        - name: ARM_RESOURCE_GROUP
          valueFrom:
            secretKeyRef:
              key: ResourceGroup
              name: cluster-autoscaler-azure
        - name: ARM_TENANT_ID
          valueFrom:
            secretKeyRef:
              key: TenantID
              name: cluster-autoscaler-azure
        - name: ARM_CLIENT_ID
          valueFrom:
            secretKeyRef:
              key: ClientID
              name: cluster-autoscaler-azure
        - name: ARM_CLIENT_SECRET
          valueFrom:
            secretKeyRef:
              key: ClientSecret
              name: cluster-autoscaler-azure
        - name: ARM_VM_TYPE
          valueFrom:
            secretKeyRef:
              key: VMType
              name: cluster-autoscaler-azure
        - name: AZURE_CLUSTER_NAME
          valueFrom:
            secretKeyRef:
              key: ClusterName
              name: cluster-autoscaler-azure
        - name: AZURE_NODE_RESOURCE_GROUP
          valueFrom:
            secretKeyRef:
              key: NodeResourceGroup
              name: cluster-autoscaler-azure
      restartPolicy: Always
```

Kopieren Sie das Geheimnis, das Sie im vorherigen Schritt erstellt haben, und fügen Sie es am Anfang der Datei ein.

Um den Bereich von Knoten festzulegen, füllen Sie als Nächstes das Argument für `--nodes` unter `command` im Format „MIN:MAX:NAME_DES_NODEPOOLS“ aus. Beispiel: Mit `--nodes=3:10:nodepool1` wird die minimale Anzahl von Knoten auf „3“, die maximale Anzahl von Knoten auf „10“ und der Name des Knotenpools auf „nodepool1“ festgelegt.

Füllen Sie anschließend das image-Feld unter **containers** mit der Cluster Autoscaler-Version aus, die Sie verwenden möchten. Für AKS ist v1.2.2 oder höher erforderlich. In diesem Beispiel wird v1.2.2 verwendet.

## <a name="deployment"></a>Bereitstellung

Stellen Sie Cluster Autoscaler bereit, indem Sie Folgendes ausführen:

```console
kubectl create -f aks-cluster-autoscaler.yaml
```

Um festzustellen, ob Cluster Autoscaler ausgeführt wird, verwenden Sie den folgenden Befehl, und überprüfen Sie die Liste von Pods. Es sollte ein Pod ausgeführt werden, bei dem „cluster-autoscaler“ vorangestellt ist. Wenn dies der Fall ist, wurde Cluster Autoscaler bereitgestellt.

```console
kubectl -n kube-system get pods
```

Führen Sie zum Anzeigen des Status von Cluster Autoscaler Folgendes aus:

```console
kubectl -n kube-system describe configmap cluster-autoscaler-status
```

## <a name="interpreting-the-cluster-autoscaler-status"></a>Interpretieren des Status von Cluster Autoscaler

```console
$ kubectl -n kube-system describe configmap cluster-autoscaler-status
Name:         cluster-autoscaler-status
Namespace:    kube-system
Labels:       <none>
Annotations:  cluster-autoscaler.kubernetes.io/last-updated=2018-07-25 22:59:22.661669494 +0000 UTC

Data
====
status:
----
Cluster-autoscaler status at 2018-07-25 22:59:22.661669494 +0000 UTC:
Cluster-wide:
  Health:      Healthy (ready=1 unready=0 notStarted=0 longNotStarted=0 registered=1 longUnregistered=0)
               LastProbeTime:      2018-07-25 22:59:22.067828801 +0000 UTC
               LastTransitionTime: 2018-07-25 00:38:36.41372897 +0000 UTC
  ScaleUp:     NoActivity (ready=1 registered=1)
               LastProbeTime:      2018-07-25 22:59:22.067828801 +0000 UTC
               LastTransitionTime: 2018-07-25 00:38:36.41372897 +0000 UTC
  ScaleDown:   NoCandidates (candidates=0)
               LastProbeTime:      2018-07-25 22:59:22.067828801 +0000 UTC
               LastTransitionTime: 2018-07-25 00:38:36.41372897 +0000 UTC

NodeGroups:
  Name:        nodepool1
  Health:      Healthy (ready=1 unready=0 notStarted=0 longNotStarted=0 registered=1 longUnregistered=0 cloudProviderTarget=1 (minSize=1, maxSize=5))
               LastProbeTime:      2018-07-25 22:59:22.067828801 +0000 UTC
               LastTransitionTime: 2018-07-25 00:38:36.41372897 +0000 UTC
  ScaleUp:     NoActivity (ready=1 cloudProviderTarget=1)
               LastProbeTime:      2018-07-25 22:59:22.067828801 +0000 UTC
               LastTransitionTime: 2018-07-25 00:38:36.41372897 +0000 UTC
  ScaleDown:   NoCandidates (candidates=0)
               LastProbeTime:      2018-07-25 22:59:22.067828801 +0000 UTC
               LastTransitionTime: 2018-07-25 00:38:36.41372897 +0000 UTC


Events:  <none>
```

Den Status von Cluster Autoscaler können Sie auf zwei verschiedenen Ebenen einsehen: innerhalb des gesamten Clusters und innerhalb der einzelnen Knotengruppen. Da AKS derzeit nur einen Knotenpool unterstützt, sind diese Metriken identisch.

* „Health“ gibt die Gesamtintegrität der Knoten an. Wenn beim Erstellen oder Entfernen von Knoten im Cluster Fehler in Cluster Autoscaler auftreten, wechselt dieser Status zu „Unhealthy“. Auch der Status der verschiedenen Knoten wird aufgeschlüsselt:
    * „Ready“ bedeutet, dass für einen Knoten Pods geplant werden können.
    * „Unready“ bedeutet, dass ein Knoten ausgefallen ist, nachdem er gestartet wurde.
    * „NotStarted“ bedeutet, dass ein Knoten noch nicht vollständig gestartet ist.
    * „LongNotStarted“ bedeutet, dass ein Knoten nicht innerhalb eines angemessenen Grenzwerts gestartet werden konnte.
    * „Registered“ bedeutet, dass ein Knoten in der Gruppe registriert ist.
    * „Unregistered“ bedeutet, dass ein Knoten zwar aufseiten des Clusteranbieters vorhanden ist, jedoch nicht in Kubernetes registriert werden konnte.
  
* Mit ScaleUp können Sie eine Überprüfung durchführen, wann der Cluster ermittelt hat, dass ein Vorgang zum zentralen Hochskalieren in Ihrem Cluster erfolgen soll.
    * Ein Übergang liegt vor, wenn sich die Anzahl der Knoten im Cluster oder der Status eines Knotens ändert.
    * Die Anzahl der Knoten mit dem Status „Ready“ entspricht der Anzahl der Knoten, die im Cluster verfügbar und bereit sind. 
    * Das cloudProviderTarget enthält die Anzahl der Knoten, die der Cluster laut Cluster Autoscaler zur Verarbeitung seiner Workload benötigt.

* Mit ScaleDown können Sie überprüfen, ob Kandidaten zum zentralen Herunterskalieren vorhanden sind. 
    * Ein Kandidat zum zentralen Herunterskalieren ist ein Knoten, der laut Cluster Autoscaler entfernt werden kann, ohne dass die Möglichkeit des Clusters zur Verarbeitung seiner Workload beeinträchtigt wird. 
    * Die angegebenen Zeiten geben den Zeitpunkt, an dem der Cluster zuletzt auf Kandidaten zum zentralen Herunterskalieren überprüft wurde, sowie den Zeitpunkt des letzten Übergangs an.

Unter Events finden Sie zu guter Letzt fehlerhafte oder erfolgreiche Skalierungsereignisse jeglicher Art oder Ereignisse zum zentralen Herunterskalieren sowie den jeweiligen Zeitpunkt, an dem Cluster Autoscaler diese ausgeführt hat.

## <a name="next-steps"></a>Nächste Schritte

Um Cluster Autoscaler mit Horizontal Pod Autoscaler zu verwenden, gehen Sie das [Tutorial: Skalieren einer Anwendung in Azure Kubernetes Service (AKS)][aks-tutorial-scale] durch.

Erfahren Sie mehr über die Bereitstellung und Verwaltung von AKS mit den AKS-Tutorials.

> [!div class="nextstepaction"]
> [AKS-Tutorial][aks-tutorial-prepare-app]

<!-- LINKS - internal -->
[aks-quick-start]: ./kubernetes-walkthrough.md
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-scale]: ./tutorial-kubernetes-scale.md
[aks-upgrade]: ./upgrade-cluster.md

<!-- LINKS - external -->
[cluster-autoscale]: https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md
