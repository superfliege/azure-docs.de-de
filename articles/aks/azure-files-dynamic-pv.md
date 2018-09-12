---
title: Verwenden von Azure Files mit AKS
description: Verwenden von Azure-Datenträgern mit AKS
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 08/15/2018
ms.author: iainfou
ms.openlocfilehash: dfc9171f54effe3da7a0f13695ab233d561357d4
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2018
ms.locfileid: "43285684"
---
# <a name="persistent-volumes-with-azure-files"></a>Persistente Volumes mit Azure Files

Ein persistentes Volume ist ein Speicherelement, das für die Verwendung in einem Kubernetes-Cluster erstellt wurde. Ein persistentes Volume kann von einem oder mehreren Pods verwendet und dynamisch oder statisch erstellt werden. In diesem Dokument wird die **dynamische Erstellung** einer Azure-Dateifreigabe als persistentes Volume beschrieben.

Weitere Informationen zu persistenten Kubernetes-Volumes, z.B. zur statischen Erstellung, finden Sie unter [Persistente Kubernetes-Volumes][kubernetes-volumes].

## <a name="create-a-storage-account"></a>Speicherkonto erstellen

Bei der dynamischen Erstellung einer Azure-Dateifreigabe als Kubernetes-Volume kann ein beliebiges Speicherkonto verwendet werden, solange es sich in der AKS-Ressourcengruppe des **Knotens** befindet. Dies ist die Gruppe mit dem Präfix *MC_*, die durch die Bereitstellung der Ressourcen für den AKS-Cluster erstellt wurde. Rufen Sie den Namen der Ressourcengruppe mit dem Befehl [az aks show][az-aks-show] ab.

```azurecli
$ az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv

MC_myResourceGroup_myAKSCluster_eastus
```

Verwenden Sie den Befehl [az storage account create][az-storage-account-create], um ein Speicherkonto zu erstellen.

Ersetzen Sie `--resource-group` durch den Namen der Ressourcengruppe aus dem letzten Schritt und `--name` durch einen Namen Ihrer Wahl. Geben Sie Ihren eigenen eindeutigen Speicherkontonamen ein:

```azurecli
az storage account create --resource-group MC_myResourceGroup_myAKSCluster_eastus --name mystorageaccount --sku Standard_LRS
```

> [!NOTE]
> Azure Files kann derzeit nur mit Storage Standard verwendet werden. Wenn Sie Storage Premium verwenden, treten beim Bereitstellen des Volumes Fehler auf.

## <a name="create-a-storage-class"></a>Erstellen einer Speicherklasse

Mit einer Speicherklasse wird festgelegt, wie eine Azure-Dateifreigabe erstellt wird. In der Klasse kann ein Speicherkonto angegeben werden. Wenn kein Speicherkonto angegeben ist, müssen *skuName* and *location* angegeben werden. Es werden alle Speicherkonten in der zugeordneten Ressourcengruppe auf eine Übereinstimmung ausgewertet. Weitere Informationen zu Kubernetes-Speicherklassen für Azure Files finden Sie unter [Kubernetes-Speicherklassen][kubernetes-storage-classes].

Erstellen Sie eine Datei mit dem Namen `azure-file-sc.yaml`, und fügen Sie das folgende Beispielmanifest ein. Aktualisieren Sie den Wert für *storageAccount* mit dem Namen Ihres Speicherkontos, das Sie im vorherigen Schritt erstellt haben. Weitere Informationen zu *mountOptions* finden Sie im Abschnitt [Einbindungsoptionen][mount-options].

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: azurefile
provisioner: kubernetes.io/azure-file
mountOptions:
  - dir_mode=0777
  - file_mode=0777
  - uid=1000
  - gid=1000
parameters:
  skuName: Standard_LRS
  storageAccount: mystorageaccount
```

Verwenden Sie den Befehl [kubectl apply][kubectl-apply] zum Erstellen der Speicherklasse:

```console
kubectl apply -f azure-file-sc.yaml
```

## <a name="create-a-cluster-role-and-binding"></a>Erstellen einer Clusterrolle und Bindung

AKS-Cluster verwenden die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) von Kubernetes zum Beschränken von Aktionen, die ausgeführt werden können. *Rollen* definieren die zu erteilenden Berechtigungen, und *Bindungen* wenden diese auf die gewünschten Benutzer an. Diese Zuweisungen können auf einen bestimmten Namespace oder im gesamten Cluster angewendet werden. Weitere Informationen finden Sie unter [Verwenden der RBAC-Autorisierung][kubernetes-rbac].

Damit die Azure-Plattform die erforderlichen Speicherressourcen erstellen kann, erstellen Sie eine *ClusterRole* und *ClusterRoleBinding*. Erstellen Sie eine Datei namens `azure-pvc-roles.yaml`, und fügen Sie den folgenden YAML-Code ein:

```yaml
---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRole
metadata:
  name: system:azure-cloud-provider
rules:
- apiGroups: ['']
  resources: ['secrets']
  verbs:     ['get','create']
---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: system:azure-cloud-provider
roleRef:
  kind: ClusterRole
  apiGroup: rbac.authorization.k8s.io
  name: system:azure-cloud-provider
subjects:
- kind: ServiceAccount
  name: persistent-volume-binder
  namespace: kube-system
```

Weisen Sie die Berechtigungen mit dem Befehl [kubectl apply][kubectl-apply] zu:

```console
kubectl apply -f azure-pvc-roles.yaml
```

## <a name="create-a-persistent-volume-claim"></a>Erstellen eines Anspruchs auf ein persistentes Volume

Ein Anspruch auf ein persistentes Volume (Persistent Volume Claim, PVC) verwendet das Speicherklassenobjekt, um eine Azure-Dateifreigabe dynamisch bereitzustellen. Der folgende YAML-Code kann verwendet werden, um einen Anspruch auf ein persistentes Volume der Größe *5GB* mit *ReadWriteMany*-Zugriff zu erstellen. Weitere Informationen zu Zugriffsmodi finden Sie in der Dokumentation zu [persistenten Kubernetes-Volumes][access-modes].

Erstellen Sie nun eine Datei mit dem Namen `azure-file-pvc.yaml`, und fügen Sie den folgenden YAML-Code ein. Stellen Sie sicher, dass *storageClassName* der Speicherklasse, die Sie im letzten Schritt erstellt haben, entspricht:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: azurefile
spec:
  accessModes:
    - ReadWriteMany
  storageClassName: azurefile
  resources:
    requests:
      storage: 5Gi
```

Erstellen Sie mit dem Befehl [kubectl apply][kubectl-apply] einen Anspruch auf ein persistentes Volume:

```console
kubectl apply -f azure-file-pvc.yaml
```

Nach Abschluss des Vorgangs wird die Dateifreigabe erstellt. Außerdem wird ein Kubernetes-Geheimnis erstellt, das die Verbindungs- und Anmeldeinformationen enthält. Mit dem Befehl [kubectl get][kubectl-get] können Sie den Status des PVC anzeigen:

```
$ kubectl get pvc azurefile

NAME        STATUS    VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
azurefile   Bound     pvc-8436e62e-a0d9-11e5-8521-5a8664dc0477   5Gi        RWX            azurefile      5m
```

## <a name="use-the-persistent-volume"></a>Verwenden des persistenten Volumes

Mit dem folgenden YAML-Code wird ein Pod erstellt, der den Anspruch auf das persistente Volume *azurefile* verwendet, um die Azure-Dateifreigabe im Pfad */mnt/azure* einzubinden.

Erstellen Sie eine Datei mit dem Namen `azure-pvc-files.yaml`, und fügen Sie den folgenden YAML-Code ein. Stellen Sie sicher, dass *claimName* dem PVC, den Sie im letzten Schritt erstellt haben, entspricht.

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: mypod
spec:
  containers:
    - name: myfrontend
      image: nginx
      volumeMounts:
      - mountPath: "/mnt/azure"
        name: volume
  volumes:
    - name: volume
      persistentVolumeClaim:
        claimName: azurefile
```

Verwenden Sie den Befehl [kubectl apply][kubectl-apply] zum Erstellen des Pods.

```console
kubectl apply -f azure-pvc-files.yaml
```

Sie verfügen nun über einen ausgeführten Pod mit Ihrem Azure-Datenträger, der im Verzeichnis */mnt/azure* eingebunden wurde. Diese Konfiguration wird angezeigt, wenn Sie Ihren Pod über `kubectl describe pod mypod` auswerten. In der folgenden verkürzten Beispielausgabe ist das im Container eingebundene Volume angegeben:

```
Containers:
  myfrontend:
    Container ID:   docker://053bc9c0df72232d755aa040bfba8b533fa696b123876108dec400e364d2523e
    Image:          nginx
    Image ID:       docker-pullable://nginx@sha256:d85914d547a6c92faa39ce7058bd7529baacab7e0cd4255442b04577c4d1f424
    State:          Running
      Started:      Wed, 15 Aug 2018 22:22:27 +0000
    Ready:          True
    Mounts:
      /mnt/azure from volume (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-8rv4z (ro)
[...]
Volumes:
  volume:
    Type:       PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
    ClaimName:  azurefile2
    ReadOnly:   false
[...]
```

## <a name="mount-options"></a>Einbindungsoptionen

Die Standardwerte für *fileMode* und *dirMode* unterscheiden sich je nach Kubernetes-Version, wie in der folgenden Tabelle beschrieben.

| Version | value |
| ---- | ---- |
| v1.6.x, v1.7.x | 0777 |
| v1.8.0-v1.8.5 | 0700 |
| v1.8.6 und höher | 0755 |
| v1.9.0 | 0700 |
| v1.9.1 und höher | 0755 |

Wenn Sie einen Cluster der Version 1.8.5 oder höher verwenden und das persistente Volume dynamisch mit einer Speicherklasse erstellen, können Einbindungsoptionen im Speicherklassenobjekt angegeben werden. Im folgenden Beispiel wird *0777* festgelegt:

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: azurefile
provisioner: kubernetes.io/azure-file
mountOptions:
  - dir_mode=0777
  - file_mode=0777
  - uid=1000
  - gid=1000
parameters:
  skuName: Standard_LRS
```

Wenn Sie einen Cluster der Version 1.8.5 oder höher verwenden und das persistente Volume statisch erstellen, müssen die Bereitstellungsoptionen im *PersistentVolume*-Objekt angegeben werden. Weitere Informationen zum statischen Erstellen eines persistenten Volumes finden Sie unter [Statische persistente Volumes][pv-static].

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: azurefile
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteMany
  azureFile:
    secretName: azure-secret
    shareName: azurefile
    readOnly: false
  mountOptions:
  - dir_mode=0777
  - file_mode=0777
  - uid=1000
  - gid=1000
  ```

Bei Verwendung eines Clusters der Version 1.8.0 bis 1.8.4 kann ein Sicherheitskontext angegeben werden, indem der Wert *runAsUser* auf *0* festgelegt wird. Weitere Informationen zum Sicherheitskontext für Pods finden Sie unter [Konfigurieren eines Sicherheitskontexts][kubernetes-security-context].

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über persistente Kubernetes-Volumes bei Verwendung von Azure Files.

> [!div class="nextstepaction"]
> [Kubernetes-Plug-In für Azure Files][kubernetes-files]

<!-- LINKS - external -->
[access-modes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubernetes-files]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_file/README.md
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubernetes-security-context]: https://kubernetes.io/docs/tasks/configure-pod-container/security-context/
[kubernetes-storage-classes]: https://kubernetes.io/docs/concepts/storage/storage-classes/#azure-file
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/
[pv-static]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#static
[kubernetes-rbac]: https://kubernetes.io/docs/reference/access-authn-authz/rbac/

<!-- LINKS - internal -->
[az-group-create]: /cli/azure/group#az-group-create
[az-group-list]: /cli/azure/group#az-group-list
[az-resource-show]: /cli/azure/aks#az-aks-show
[az-storage-account-create]: /cli/azure/storage/account#az-storage-account-create
[az-storage-create]: /cli/azure/storage/account#az-storage-account-create
[az-storage-key-list]: /cli/azure/storage/account/keys#az-storage-account-keys-list
[az-storage-share-create]: /cli/azure/storage/share#az-storage-share-create
[mount-options]: #mount-options
