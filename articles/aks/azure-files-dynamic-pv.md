---
title: Verwenden von Azure Files mit AKS
description: Verwenden von Azure-Datenträgern mit AKS
services: container-service
author: neilpeterson
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 03/06/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 21245688076cf0a21164b549eb68bc6f55d6ec6c
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/11/2018
---
# <a name="persistent-volumes-with-azure-files"></a>Persistente Volumes mit Azure Files

Ein persistentes Volume stellt ein Speicherelement dar, das für die Verwendung in einem Kubernetes-Cluster bereitgestellt wurde. Ein persistentes Volume kann von einem oder mehreren Pods verwendet und dynamisch oder statisch bereitgestellt werden. Dieses Dokument beschreibt die dynamische Bereitstellung einer Azure-Dateifreigabe als persistentes Kubernetes-Volume in einem AKS-Cluster.

Weitere Informationen zu persistenten Kubernetes-Volumes finden Sie unter [Persistente Kubernetes-Volumes][kubernetes-volumes].

## <a name="create-storage-account"></a>Speicherkonto erstellen

Bei der dynamischen Bereitstellung einer Azure-Dateifreigabe als Kubernetes-Volume kann jedes Speicherkonto verwendet werden, solange es in derselben Ressourcengruppe wie der AKS-Cluster enthalten ist. Erstellen Sie bei Bedarf ein Speicherkonto in derselben Ressourcengruppe wie der AKS-Cluster.

Um die richtige Ressourcengruppe zu bestimmen, verwenden Sie den Befehl [az group list][az-group-list].

```azurecli-interactive
az group list --output table
```

Sie suchen eine Ressourcengruppe mit einem ähnlichen Namen wie `MC_clustername_clustername_locaton`, wobei „clustername“ der Name Ihres AKS-Clusters ist und „location“ die Azure-Region, in der der Cluster bereitgestellt wurde.

```
Name                                 Location    Status
-----------------------------------  ----------  ---------
MC_myAKSCluster_myAKSCluster_eastus  eastus      Succeeded
myAKSCluster                         eastus      Succeeded
```

Verwenden Sie den Befehl [az storage account create][az-storage-account-create], um ein Speicherkonto zu erstellen.

Ersetzen Sie in diesem Beispiel den Wert von `--resource-group` durch den Namen der Ressourcengruppe und den Wert von `--name` durch einen Namen Ihrer Wahl.

```azurecli-interactive
az storage account create --resource-group MC_myAKSCluster_myAKSCluster_eastus --name mystorageaccount --location eastus --sku Standard_LRS
```

## <a name="create-storage-class"></a>Erstellen einer Speicherklasse

Mit einer Speicherklasse wird festgelegt, wie eine Azure-Dateifreigabe erstellt wird. In der Klasse kann ein bestimmtes Speicherkonto angegeben werden. Wenn kein Speicherkonto angegeben ist, müssen ein `skuName` und eine `location` angegeben werden. Es werden alle Speicherkonten in der zugeordneten Ressourcengruppe auf eine Übereinstimmung ausgewertet.

Weitere Informationen zu Kubernetes-Speicherklassen für Azure Files finden Sie unter [Kubernetes-Speicherklassen][kubernetes-storage-classes].

Erstellen Sie eine Datei namens `azure-file-sc.yaml`, und fügen Sie das folgende Manifest ein. Ersetzen Sie `storageAccount` durch den Namen Ihres Zielspeicherkontos.

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: azurefile
provisioner: kubernetes.io/azure-file
parameters:
  storageAccount: mystorageaccount
```

Verwenden Sie den Befehl [kubectl apply][kubectl-apply] zum Erstellen der Speicherklasse.

```azurecli-interactive
kubectl apply -f azure-file-sc.yaml
```

## <a name="create-persistent-volume-claim"></a>Erstellen eines Anspruchs auf ein persistentes Volume

Ein Anspruch auf ein persistentes Volume (Persistent Volume Claim, PVC) verwendet das Speicherklassenobjekt, um eine Azure-Dateifreigabe dynamisch bereitzustellen.

Das folgende Manifest kann verwendet werden, um einen Anspruch auf ein persistentes Volume der Größe `5GB` mit `ReadWriteOnce`-Zugriff zu erstellen.

Erstellen Sie eine Datei namens `azure-file-pvc.yaml`, und fügen Sie das folgende Manifest ein. Stellen Sie sicher, dass `storageClassName` der Speicherklasse, die Sie im letzten Schritt erstellt haben, entspricht.

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: azurefile
spec:
  accessModes:
    - ReadWriteOnce
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

Das folgende Manifest erstellt einen Pod, der den Anspruch auf das persistente Volume `azurefile` verwendet, um die Azure-Dateifreigabe im Pfad `/mnt/azure` einzubinden.

Erstellen Sie eine Datei namens `azure-pvc-files.yaml`, und fügen Sie das folgende Manifest ein. Stellen Sie sicher, dass `claimName` dem PVC, den Sie im letzten Schritt erstellt haben, entspricht.

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

Bei Verwendung eines Clusters der Version 1.8.5 oder höher können Einbindungsoptionen für das Speicherklassenobjekt angegeben werden. Im folgenden Beispiel wird `0777` festgelegt.

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

Bei Verwendung eines Clusters der Version 1.8.0-1.8.4 kann ein Sicherheitskontext angegeben werden, indem der Wert `runAsUser` auf `0` festgelegt wird. Weitere Informationen zum Sicherheitskontext für Pods finden Sie unter [Konfigurieren eines Sicherheitskontexts][kubernetes-security-context].

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über persistente Kubernetes-Volumes bei Verwendung von Azure Files.

> [!div class="nextstepaction"]
> [Kubernetes-Plug-In für Azure Files][kubernetes-files]

<!-- LINKS - external -->
[access-modes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-describe]: https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_describe/
[kubernetes-files]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_file/README.md
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubernetes-security-context]: https://kubernetes.io/docs/tasks/configure-pod-container/security-context/
[kubernetes-storage-classes]: https://kubernetes.io/docs/concepts/storage/storage-classes/#azure-file
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/

<!-- LINKS - internal -->
[az-group-create]: /cli/azure/group#az_group_create
[az-group-list]: /cli/azure/group#az_group_list
[az-storage-account-create]: /cli/azure/storage/account#az_storage_account_create
[az-storage-create]: /cli/azure/storage/account#az_storage_account_create
[az-storage-key-list]: /cli/azure/storage/account/keys#az_storage_account_keys_list
[az-storage-share-create]: /cli/azure/storage/share#az_storage_share_create
