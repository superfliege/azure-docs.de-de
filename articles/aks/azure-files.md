---
title: Verwenden von Azure Files mit AKS | Microsoft-Dokumentation
description: "Verwenden von Azure-Datenträgern mit AKS"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: aks, azure-container-service
keywords: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/30/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 6e88c590e11aa8d2f4ae17e8b5e164483f0a6820
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2017
---
# <a name="using-azure-files-with-kubernetes"></a>Verwenden von Azure Files mit Kubernetes

Containerbasierte Anwendungen müssen häufig auf Daten in einem externen Datenvolume zugreifen und diese dauerhaft speichern. Azure-Dateien können als ein solcher externer Datenspeicher verwendet werden. In diesem Artikel wird erläutert, wie Azure-Dateien als Kubernetes-Volume in Azure Container Service verwendet werden.

Weitere Informationen zu Kubernetes-Volumes finden Sie unter [Kubernetes-Volumes][kubernetes-volumes].

## <a name="creating-a-file-share"></a>Erstellen einer Dateifreigabe

Eine vorhandene Azure-Dateifreigabe kann mit Azure Container Service verwendet werden. Wenn Sie eine solche erstellen möchten, verwenden Sie die folgende Befehlsfolge.

Erstellen Sie mit dem Befehl [az group create][az-group-create] eine Ressourcengruppe für die Azure-Dateifreigabe. Die Ressourcengruppe des Speicherkontos und der Kubernetes-Cluster müssen sich in derselben Region befinden.

```azurecli-interactive
az group create --name myResourceGroup --location westus2
```

Verwenden Sie den Befehl [az storage account create][az-storage-create], um ein Azure Storage-Konto zu erstellen. Der Name des Speicherkontos muss eindeutig sein. Aktualisieren Sie den Wert des `--name`-Arguments mit einem eindeutigen Wert.

```azurecli-interactive
az storage account create --name mystorageaccount --resource-group myResourceGroup --sku Standard_LRS
```

Verwenden Sie den Befehl [az storage account keys list][az-storage-key-list], um den Speicherschlüssel zurückzugeben. Aktualisieren Sie den Wert des `--account-name`-Arguments mit dem eindeutigen Namen des Speicherkontos.

Notieren Sie sich einen der Schlüsselwerte, der in den nachfolgenden Schritten verwendet wird.

```azurecli-interactive
az storage account keys list --account-name mystorageaccount --resource-group myResourceGroup --output table
```

Verwenden Sie den Befehl [az storage share create][az-storage-share-create], um die Azure-Dateifreigabe zu erstellen. Aktualisieren Sie den `--account-key`-Wert mit dem Wert, der im letzten Schritt erfasst wurde.

```azurecli-interactive
az storage share create --name myfileshare --account-name mystorageaccount --account-key <key>
```

## <a name="create-kubernetes-secret"></a>Erstellen eines Kubernetes-Geheimnisses

Kubernetes benötigt Anmeldeinformationen für den Zugriff auf die Dateifreigabe. Statt den Azure Storage-Kontonamen und den Schlüssel mit jedem Pod zu speichern, werden die Informationen nur einmal in einem [Kubernetes-Geheimnis][kubernetes-secret] abgelegt und von den einzelnen Azure Files-Volumes referenziert. 

Die Werte in einem Kubernetes-Geheimnismanifest müssen Base64-codiert sein. Verwenden Sie die folgenden Befehle, um codierte Werte zurückzugeben.

Codieren Sie zuerst den Namen des Speicherkontos. Ersetzen Sie `storage-account` durch den Namen Ihres Azure-Speicherkontos.

```azurecli-interactive
echo -n <storage-account> | base64
```

Anschließend wird der Zugriffsschlüssel für das Speicherkonto benötigt. Führen Sie den folgenden Befehl aus, um den codierten Schlüssel zurückzugeben. Ersetzen Sie `storage-key` durch den Schlüssel, der in einem vorherigen Schritt erfasst wurde.

```azurecli-interactive
echo -n <storage-key> | base64
```

Erstellen Sie eine Datei namens „`azure-secret.yml`“, und fügen Sie den folgenden YAML-Code ein. Aktualisieren Sie die Werte `azurestorageaccountname` und `azurestorageaccountkey` mit den Base64-codierten Werte, die Sie im letzten Schritt abgerufen haben.

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: azure-secret
type: Opaque
data:
  azurestorageaccountname: <base64_encoded_storage_account_name>
  azurestorageaccountkey: <base64_encoded_storage_account_key>
```

Verwenden Sie den Befehl [kubectl apply][kubectl-apply] zum Erstellen des Geheimnisses.

```azurecli-interactive
kubectl apply -f azure-secret.yml
```

## <a name="mount-file-share-as-volume"></a>Einbinden einer Dateifreigabe als Volume

Sie können Ihre Azure Files-Freigabe in Ihren Pod einbinden, indem Sie das Volume in der entsprechenden Spezifikation konfigurieren. Erstellen Sie eine neue Datei namens „`azure-files-pod.yml`“ mit folgendem Inhalt. Aktualisieren Sie „`share-name`“ mit dem Namen der Azure Files-Freigabe.

```yaml
apiVersion: v1
kind: Pod
metadata:
 name: azure-files-pod
spec:
 containers:
  - image: kubernetes/pause
    name: azure
    volumeMounts:
      - name: azure
        mountPath: /mnt/azure
 volumes:
  - name: azure
    azureFile:
      secretName: azure-secret
      shareName: <share-name>
      readOnly: false
```

Verwenden Sie „kubectl“, um einen Pod zu erstellen.

```azurecli-interactive
kubectl apply -f azure-files-pod.yml
```

Sie haben jetzt einen aktiven Container, bei dem Ihre Azure-Dateifreigabe in das Verzeichnis `/mnt/azure` eingebunden ist. Sie können Ihre Volumeeinbindung anzeigen, wenn Sie Ihren Pod über `kubectl describe pod azure-files-pod` untersuchen.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über Kubernetes-Volumes anhand von Azure Files.

> [!div class="nextstepaction"]
> [Kubernetes-Plug-In für Azure Files](https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_file/README.md)

<!-- LINKS -->
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/volumes/
[az-storage-create]: /cli/azure/storage/account#az_storage_account_create
[az-storage-key-list]: /cli/azure/storage/account/keys#az_storage_account_keys_list
[az-storage-share-create]: /cli/azure/storage/share#az_storage_share_create
[kubectl-apply]: https://kubernetes.io/docs/user-guide/kubectl/v1.8/#apply
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[az-group-create]: /cli/azure/group#az_group_create