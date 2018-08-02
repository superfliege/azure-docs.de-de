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
ms.openlocfilehash: 4f8df8e7004ca3cee832b6230dc153b21e2a6c18
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2018
ms.locfileid: "39186712"
---
# <a name="cluster-autoscaler-on-azure-kubernetes-service-aks---preview"></a>Cluster Autoscaler in Azure Kubernetes Service (AKS) – Vorschauversion

Azure Kubernetes Service (AKS) ist eine flexible Lösung, mit der Sie einen verwalteten Kubernetes-Cluster in Azure bereitstellen können. Bei steigenden Ressourcenanforderungen kann Ihr Cluster mit Cluster Autoscaler zur Erfüllung dieses Bedarfs automatisch skaliert werden – basierend auf Einschränkungen, die Sie festlegen. Cluster Autoscaler (CA) ermöglicht dies durch die Skalierung Ihrer Agent-Knoten basierend auf ausstehenden Pods. Die Lösung prüft den Cluster in regelmäßigen Abständen auf ausstehende Pods oder leere Knoten und erhöht wenn möglich deren Größe. Standardmäßig prüft die Zertifizierungsstelle alle 10 Sekunden auf ausstehende Pods und entfernt einen Knoten, wenn dieser seit mehr als 10 Minuten nicht benötigt wurde. Wird die Lösung mit Horizontal Pod Autoscaler (HPA) verwendet, aktualisiert HPA bei Bedarf Podreplikate und -ressourcen. Wenn nach dieser Podskalierung nicht genügend Knoten oder nicht benötigte Knoten vorhanden sind, reagiert die Zertifizierungsstelle und plant die Pods in der neuen Knotengruppe.

> [!IMPORTANT]
> Die Cluster Autoscaler-Integration in Azure Kubernetes Service (AKS) befindet sich derzeit in der **Vorschauversion**. Vorschauversionen werden Ihnen zur Verfügung gestellt, wenn Sie die [zusätzlichen Nutzungsbedingungen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) akzeptieren. Einige Aspekte dieses Features werden bis zur allgemeinen Verfügbarkeit unter Umständen noch geändert.
>

## <a name="prerequisites"></a>Voraussetzungen

In diesem Dokument wird davon ausgegangen, dass Sie über einen RBAC-fähigen AKS-Cluster verfügen. Wenn Sie einen AKS-Cluster benötigen, finden Sie weitere Informationen im [Schnellstart zu Azure Kubernetes Service (AKS)][aks-quick-start].

 Für die Verwendung von Cluster Autoscaler muss Ihr Cluster mit Kubernetes v1.10.x oder höher verwendet werden und RBAC-fähig sein. Informationen zum Durchführen eines Upgrades für Ihren Cluster finden Sie im Artikel zum [Durchführen eines Upgrades für AKS-Cluster][aks-upgrade].

## <a name="gather-information"></a>Sammeln von Informationen

Die folgende Liste enthält alle Informationen, die Sie in der Definition für die automatische Skalierung angeben müssen.

- *Abonnement-ID*: Die ID, die dem für diesen Cluster verwendeten Abonnement entspricht.
- *Ressourcengruppenname*: Der Name der Ressourcengruppe, der der Cluster angehört. 
- *Clustername*: Der Name des Clusters.
- *Client-ID*: Die App-ID, die beim Schritt zum Generieren von Berechtigungen gewährt wird.
- *Clientgeheimnis*: Das App-Geheimnis, das beim Schritt zur Generieren von Berechtigungen gewährt wird.
- *Mandanten-ID*: Die ID des Mandanten (Kontobesitzer).
- *Knotenressourcengruppe*: Der Name der Ressourcengruppe im Cluster, in der die Agent-Knoten enthalten sind.
- *Name des Knotenpools*: Der Name des Knotenpools, der skaliert werden soll.
- *Minimale Anzahl von Knoten*: Die minimale Anzahl von Knoten, die im Cluster vorhanden sein sollten.
- *Maximale Anzahl von Knoten*: Die maximale Anzahl von Knoten, die im Cluster vorhanden sein sollten.
- *VM-Typ*: Der Dienst, der zum Generieren des Kubernetes-Clusters verwendet wird.

Rufen Sie mit folgendem Befehl Ihre Abonnement-ID ab: 

``` azurecli
az account show --query id
```

Generieren Sie eine Gruppe von Azure-Anmeldeinformationen, indem Sie den folgenden Befehl ausführen:

```console
$ az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/<subscription-id>" --output json

"appId": <app-id>,
"displayName": <display-name>,
"name": <name>,
"password": <app-password>,
"tenant": <tenant-id>
```

In den folgenden Schritten stehen clientID, clientSecret und tenantID für die App-ID, das Kennwort bzw. die Mandanten-ID.

Rufen Sie den Namen Ihres Knotenpools ab, indem Sie den folgenden Befehl ausführen: 

```console
$ kubectl get nodes --show-labels
```

Ausgabe:

```console
NAME                       STATUS    ROLES     AGE       VERSION   LABELS
aks-nodepool1-37756013-0   Ready     agent     1h        v1.10.3   agentpool=nodepool1,beta.kubernetes.io/arch=amd64,beta.kubernetes.io/instance-type=Standard_DS1_v2,beta.kubernetes.io/os=linux,failure-domain.beta.kubernetes.io/region=eastus,failure-domain.beta.kubernetes.io/zone=0,kubernetes.azure.com/cluster=MC_[resource-group]\_[cluster-name]_[location],kubernetes.io/hostname=aks-nodepool1-37756013-0,kubernetes.io/role=agent,storageprofile=managed,storagetier=Premium_LRS
 ```

Extrahieren Sie dann den Wert der Bezeichnung **agentpool**. Der Standardname für den Knotenpool eines Clusters lautet „nodepool1“.

Um den Namen Ihrer Knotenressourcengruppe abzurufen, extrahieren Sie den Wert der Bezeichnung **kubernetes.azure.com<span></span>/cluster**. Der Name der Knotenressourcengruppe weist allgemein das folgende Format auf: „MC_[Ressourcengruppe]\_[Clustername]_[Standort]“.

Der vmType-Parameter bezieht sich auf den verwendeten Dienst, in diesem Fall auf AKS.

Nun sollten Sie die folgenden Informationen bereithalten:

- SubscriptionID
- ResourceGroup
- ClusterName
- ClientID
- ClientSecret
- Mandanten-ID
- NodeResourceGroup
- VMType

Codieren Sie als Nächstes all diese Werte mit „base64“. Codieren Sie beispielsweise wie folgt den VMType-Wert mit „base64“:

```console
$ echo AKS | base64
QUtTCg==
```

## <a name="create-secret"></a>Erstellen eines Geheimnisses
Anhand dieser Daten erstellen Sie ein Geheimnis für die Bereitstellung, indem Sie die in den vorherigen Schritten ermittelten Werte im folgenden Format verwenden:

```yaml
---
apiVersion: v1
kind: Secret
metadata:
  name: cluster-autoscaler-azure
  namespace: kube-system
data:
  ClientID: <base64-encoded-client-id>
  ClientSecret: <base64-encoded-client-secret>
  ResourceGroup: <base64-encoded-resource-group>
  SubscriptionID: <base64-encode-subscription-id>
  TenantID: <base64-encoded-tenant-id>
  VMType: QUtTCg==
  ClusterName: <base64-encoded-clustername>
  NodeResourceGroup: <base64-encoded-node-resource-group>
---
```

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
      - image: k8s.gcr.io/cluster-autoscaler:{{ ca_version }}
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
kubectl create -f cluster-autoscaler-containerservice.yaml
```

Um festzustellen, ob Cluster Autoscaler ausgeführt wird, verwenden Sie den folgenden Befehl, und überprüfen Sie die Liste von Pods. Ist einem Pod das Präfix „cluster-autoscaler“ vorangestellt, wurde Ihre Cluster Autoscaler-Instanz bereitgestellt.

```console
kubectl -n kube-system get pods
```

Führen Sie zum Anzeigen des Status von Cluster Autoscaler Folgendes aus:

```console
kubectl -n kube-system describe configmap cluster-autoscaler-status
```

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
