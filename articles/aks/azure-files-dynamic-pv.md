---
title: Verwenden von Azure Files mit AKS
description: "Verwenden von Azure-Datenträgern mit AKS"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 1/04/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 4873b98c8ba4f1e574be20baebef3b6860341529
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/10/2018
---
# <a name="persistent-volumes-with-azure-files---dynamic-provisioning"></a>Persistente Volumes mit Azure Files: Dynamische Bereitstellung

Ein persistentes Volume stellt ein Speicherelement dar, das für die Verwendung in einem Kubernetes-Cluster bereitgestellt wurde. Ein persistentes Volume kann von einem oder mehreren Pods verwendet und dynamisch oder statisch bereitgestellt werden. Dieses Dokument beschreibt die dynamische Bereitstellung einer Azure-Dateifreigabe als persistentes Kubernetes-Volume in einem AKS-Cluster. 

Weitere Informationen zu persistenten Kubernetes-Volumes finden Sie unter [Persistente Kubernetes-Volumes][kubernetes-volumes].

## <a name="prerequisites"></a>Voraussetzungen

Bei der dynamischen Bereitstellung einer Azure-Dateifreigabe als Kubernetes-Volume kann jedes Speicherkonto verwendet werden, solange es in derselben Ressourcengruppe wie der AKS-Cluster enthalten ist. Erstellen Sie bei Bedarf ein Speicherkonto in derselben Ressourcengruppe wie der AKS-Cluster. 

Um die richtige Ressourcengruppe zu bestimmen, verwenden Sie den Befehl [az group list][az-group-list].

```azurecli-interactive
az group list --output table
```

Die folgende Beispielausgabe zeigt die Ressourcengruppen, die beide einem AKS-Cluster zugeordnet sind. Die Ressourcengruppe mit einem Namen wie *MC_myAKSCluster_myAKSCluster_eastus* enthält die Ressourcen des AKS-Clusters und ist der Ort, an dem das Speicherkonto erstellt werden muss. 

```
Name                                 Location    Status
-----------------------------------  ----------  ---------
MC_myAKSCluster_myAKSCluster_eastus  eastus      Succeeded
myAKSCluster                         eastus      Succeeded
```

Nachdem die Ressourcengruppe bestimmt wurde, erstellen Sie mit dem Befehl [az storage account create][az-storage-account-create] das Speicherkonto.

```azurecli-interactive
az storage account create --resource-group  MC_myAKSCluster_myAKSCluster_eastus --name mystorageaccount --location eastus --sku Standard_LRS
```

## <a name="create-storage-class"></a>Erstellen einer Speicherklasse

Eine Speicherklasse dient zum Definieren der Konfiguration eines dynamisch erstellten persistenten Volumes. Elemente wie der Name des Azure-Speicherkontos, die SKU und Region werden im Speicherklassenobjekt definiert. Weitere Informationen zu Kubernetes-Speicherklassen finden Sie unter [Kubernetes-Speicherklassen][kubernetes-storage-classes].

Das folgende Beispiel gibt an, dass jedes Speicherkonto des SKU-Typs `Standard_LRS` in der Region `eastus` bei Anforderung von Speicher verwendet werden kann. 

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: azurefile
provisioner: kubernetes.io/azure-file
parameters:
  skuName: Standard_LRS
```

Um ein bestimmtes Speicherkonto zu verwenden, kann der Parameter `storageAccount` angegeben werden.

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: azurefile
provisioner: kubernetes.io/azure-file
parameters:
  storageAccount: azure_storage_account_name
```

## <a name="create-persistent-volume-claim"></a>Erstellen eines Anspruchs auf ein persistentes Volume

Ein Anspruch auf ein persistentes Volume verwendet das Speicherklassenobjekt, um eine Speicherkomponente dynamisch bereitzustellen. Wenn Sie Azure Files verwenden, wird eine Azure-Dateifreigabe im ausgewählten Speicherkonto erstellt oder im Speicherklassenobjekt angegeben.

>  [!NOTE]
>   Stellen Sie sicher, dass ein geeignetes Speicherkonto in der gleichen Ressourcengruppe wie die AKS-Clusterressourcen angelegt wurde. Diese Ressourcengruppe weist einen Namen wie *MC_myAKSCluster_myAKSCluster_eastus* auf. Der Anspruch auf ein persistentes Volume kann die Azure-Dateifreigabe nicht bereitstellen, wenn kein Speicherkonto verfügbar ist. 

Das folgende Manifest kann verwendet werden, um einen Anspruch auf ein persistentes Volume der Größe `5GB` mit `ReadWriteOnce`-Zugriff zu erstellen. Weitere Informationen zu PVC-Zugriffsmodi finden Sie unter [Zugriffsmodi][access-modes].

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

## <a name="using-the-persistent-volume"></a>Verwenden des persistenten Volumes

Sobald der Anspruch auf ein persistentes Volumes erstellt und der Speicher erfolgreich bereitgestellt wurde, kann ein Pod mit Zugriff auf das Volume erstellt werden. Das folgende Manifest erstellt einen Pod, der den Anspruch auf das persistente Volume `azurefile` verwendet, um die Azure-Dateifreigabe im Pfad `/var/www/html` einzubinden. 

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
      - mountPath: "/var/www/html"
        name: volume
  volumes:
    - name: volume
      persistentVolumeClaim:
        claimName: azurefile
```

## <a name="mount-options"></a>Einbindungsoptionen

Die Standardwerte für „fileMode“ und „dirMode“ unterscheiden sich je nach Kubernetes-Version, wie in der folgenden Tabelle beschrieben. 

| Version | Wert |
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
[kubectl-create]: https://kubernetes.io/docs/user-guide/kubectl/v1.8/#create
[kubectl-describe]: https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_describe/
[kubernetes-files]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_file/README.md
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubernetes-security-context]: https://kubernetes.io/docs/tasks/configure-pod-container/security-context/
[kubernetes-storage-classes]: https://kubernetes.io/docs/concepts/storage/storage-classes/
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/

<!-- LINKS - internal -->
[az-group-create]: /cli/azure/group#az_group_create
[az-group-list]: /cli/azure/group#az_group_list
[az-storage-account-create]: /cli/azure/storage/account#az_storage_account_create
[az-storage-create]: /cli/azure/storage/account#az_storage_account_create
[az-storage-key-list]: /cli/azure/storage/account/keys#az_storage_account_keys_list
[az-storage-share-create]: /cli/azure/storage/share#az_storage_share_create