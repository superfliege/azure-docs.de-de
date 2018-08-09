---
title: Verwenden von Azure Files mit AKS
description: Verwenden von Azure-Datenträgern mit AKS
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/21/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: bac80e354a4d629bfbfc8207b9fed7c69c765839
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39429266"
---
# <a name="persistent-volumes-with-azure-files"></a>Persistente Volumes mit Azure Files

Ein persistentes Volume ist ein Speicherelement, das für die Verwendung in einem Kubernetes-Cluster erstellt wurde. Ein persistentes Volume kann von einem oder mehreren Pods verwendet und dynamisch oder statisch erstellt werden. In diesem Dokument wird die **dynamische Erstellung** einer Azure-Dateifreigabe als persistentes Volume beschrieben.

Weitere Informationen zu persistenten Kubernetes-Volumes, z.B. zur statischen Erstellung, finden Sie unter [Persistente Kubernetes-Volumes][kubernetes-volumes].

## <a name="create-storage-account"></a>Speicherkonto erstellen

Bei der dynamischen Erstellung einer Azure-Dateifreigabe als Kubernetes-Volume kann ein beliebiges Speicherkonto verwendet werden, solange es sich in der AKS-Ressourcengruppe des **Knotens** befindet. Dies ist diejenige mit dem Präfix `MC_`, die durch die Bereitstellung der Ressourcen für den AKS-Cluster erstellt wurde. Rufen Sie den Namen der Ressourcengruppe mit dem Befehl [az resource show][az-resource-show] ab.

```azurecli-interactive
$ az resource show --resource-group myResourceGroup --name myAKSCluster --resource-type Microsoft.ContainerService/managedClusters --query properties.nodeResourceGroup -o tsv

MC_myResourceGroup_myAKSCluster_eastus
```

Verwenden Sie den Befehl [az storage account create][az-storage-account-create], um ein Speicherkonto zu erstellen.

Ersetzen Sie `--resource-group` durch den Namen der Ressourcengruppe aus dem letzten Schritt und `--name` durch einen Namen Ihrer Wahl.

```azurecli-interactive
az storage account create --resource-group MC_myResourceGroup_myAKSCluster_eastus --name mystorageaccount --location eastus --sku Standard_LRS
```

> Azure Files kann derzeit nur mit Storage Standard verwendet werden. Wenn Sie Storage Premium verwenden, treten beim Bereitstellen des Volumes Fehler auf.

## <a name="create-storage-class"></a>Erstellen einer Speicherklasse

Mit einer Speicherklasse wird festgelegt, wie eine Azure-Dateifreigabe erstellt wird. In der Klasse kann ein bestimmtes Speicherkonto angegeben werden. Wenn kein Speicherkonto angegeben ist, müssen ein `skuName` und eine `location` angegeben werden. Es werden alle Speicherkonten in der zugeordneten Ressourcengruppe auf eine Übereinstimmung ausgewertet.

Weitere Informationen zu Kubernetes-Speicherklassen für Azure Files finden Sie unter [Kubernetes-Speicherklassen][kubernetes-storage-classes].

Erstellen Sie eine Datei namens `azure-file-sc.yaml`, und fügen Sie das folgende Manifest ein. Ersetzen Sie `storageAccount` durch den Namen Ihres Zielspeicherkontos. Im Abschnitt [Einbindungsoptionen] finden Sie weitere Informationen zu `mountOptions`.

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

Verwenden Sie den Befehl [kubectl apply][kubectl-apply] zum Erstellen der Speicherklasse.

```azurecli-interactive
kubectl apply -f azure-file-sc.yaml
```

## <a name="create-persistent-volume-claim"></a>Erstellen eines Anspruchs auf ein persistentes Volume

Ein Anspruch auf ein persistentes Volume (Persistent Volume Claim, PVC) verwendet das Speicherklassenobjekt, um eine Azure-Dateifreigabe dynamisch bereitzustellen.

Der folgende YAML-Code kann verwendet werden, um einen Anspruch auf ein persistentes Volume der Größe `5GB` mit `ReadWriteMany`-Zugriff zu erstellen. Weitere Informationen zu Zugriffsmodi finden Sie in der Dokumentation zu [persistenten Kubernetes-Volumes][access-modes].

Erstellen Sie eine Datei namens „`azure-file-pvc.yaml`“, und fügen Sie den folgenden YAML-Code ein. Stellen Sie sicher, dass `storageClassName` der Speicherklasse, die Sie im letzten Schritt erstellt haben, entspricht.

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

Erstellen Sie mit dem Befehl [kubectl apply][kubectl-apply] einen Anspruch auf ein persistentes Volume.

```azurecli-interactive
kubectl apply -f azure-file-pvc.yaml
```

Nach Abschluss des Vorgangs wird die Dateifreigabe erstellt. Außerdem wird ein Kubernetes-Geheimnis erstellt, das die Verbindungs- und Anmeldeinformationen enthält.

## <a name="using-the-persistent-volume"></a>Verwenden des persistenten Volumes

Mit dem folgenden YAML-Code wird ein Pod erstellt, der den Anspruch auf das persistente Volume `azurefile` verwendet, um die Azure-Dateifreigabe im Pfad `/mnt/azure` einzubinden.

Erstellen Sie eine Datei mit dem Namen `azure-pvc-files.yaml`, und fügen Sie den folgenden YAML-Code ein. Stellen Sie sicher, dass `claimName` dem PVC, den Sie im letzten Schritt erstellt haben, entspricht.

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

```azurecli-interactive
kubectl apply -f azure-pvc-files.yaml
```

Sie verfügen nun über einen ausgeführten Pod mit Ihrem Azure-Datenträger, der im Verzeichnis `/mnt/azure` eingebunden wurde. Diese Konfiguration wird angezeigt, wenn Sie Ihren Pod über `kubectl describe pod mypod` auswerten.

## <a name="mount-options"></a>Einbindungsoptionen

Die Standardwerte für „fileMode“ und „dirMode“ unterscheiden sich je nach Kubernetes-Version, wie in der folgenden Tabelle beschrieben.

| Version | value |
| ---- | ---- |
| v1.6.x, v1.7.x | 0777 |
| v1.8.0-v1.8.5 | 0700 |
| v1.8.6 und höher | 0755 |
| v1.9.0 | 0700 |
| v1.9.1 und höher | 0755 |

Wenn Sie einen Cluster der Version 1.8.5 oder höher verwenden und das persistente Volume dynamisch mit einer Speicherklasse erstellen, können Einbindungsoptionen im Speicherklassenobjekt angegeben werden. Im folgenden Beispiel wird `0777` festgelegt.

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

Wenn Sie einen Cluster der Version 1.8.5 oder höher verwenden und das persistente Volume statisch erstellen, müssen die Bereitstellungsoptionen im `PersistentVolume`-Objekt angegeben werden. Weitere Informationen zum statischen Erstellen eines persistenten Volumes finden Sie unter [Statische persistente Volumes][pv-static].

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

Bei Verwendung eines Clusters der Version 1.8.0-1.8.4 kann ein Sicherheitskontext angegeben werden, indem der Wert `runAsUser` auf `0` festgelegt wird. Weitere Informationen zum Sicherheitskontext für Pods finden Sie unter [Konfigurieren eines Sicherheitskontexts][kubernetes-security-context].

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über persistente Kubernetes-Volumes bei Verwendung von Azure Files.

> [!div class="nextstepaction"]
> [Kubernetes-Plug-In für Azure Files][kubernetes-files]

<!-- LINKS - external -->
[access-modes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-describe]: https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_describe/
[kubernetes-files]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_file/README.md
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubernetes-security-context]: https://kubernetes.io/docs/tasks/configure-pod-container/security-context/
[kubernetes-storage-classes]: https://kubernetes.io/docs/concepts/storage/storage-classes/#azure-file
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/
[pv-static]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#static

<!-- LINKS - internal -->
[az-group-create]: /cli/azure/group#az-group-create
[az-group-list]: /cli/azure/group#az-group-list
[az-resource-show]: /cli/azure/resource#az-resource-show
[az-storage-account-create]: /cli/azure/storage/account#az-storage-account-create
[az-storage-create]: /cli/azure/storage/account#az-storage-account-create
[az-storage-key-list]: /cli/azure/storage/account/keys#az-storage-account-keys-list
[az-storage-share-create]: /cli/azure/storage/share#az-storage-share-create
[mount-options]: #mount-options
