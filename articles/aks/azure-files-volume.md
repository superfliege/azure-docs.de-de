---
title: Erstellen eines statischen Volumes für mehrere Pods in Azure Kubernetes Service (AKS)
description: Erfahren Sie, wie Sie manuell ein Volume mit Azure Files für die Verwendung mit mehreren parallelen Pods in Azure Kubernetes Service (AKS) erstellen.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 10/08/2018
ms.author: iainfou
ms.openlocfilehash: 1a8609dbf5fa1c1e7d5f4e35b081ecaa09994eb6
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "49068076"
---
# <a name="manually-create-and-use-a-volume-with-azure-files-share-in-azure-kubernetes-service-aks"></a>Manuelles Erstellen und Verwenden eines Volumes mit Azure Files-Freigabe in Azure Kubernetes Service (AKS)

Containerbasierte Anwendungen müssen häufig auf Daten in einem externen Datenvolume zugreifen und diese dauerhaft speichern. Wenn mehrere Pods gleichzeitig Zugriff auf dasselbe Speichervolume benötigen, können Sie Azure Files verwenden, um mithilfe des [Server Message Block-Protokolls (SMB)][smb-overview] eine Verbindung herzustellen. In diesem Artikel wird das manuelle Erstellen einer Azure Files-Freigabe und Anfügen dieser an einen Pod in AKS veranschaulicht.

Weitere Informationen zu Kubernetes-Volumes finden Sie unter [Kubernetes-Volumes][kubernetes-volumes].

## <a name="before-you-begin"></a>Voraussetzungen

Es wird vorausgesetzt, dass Sie über ein AKS-Cluster verfügen. Wenn Sie noch einen AKS-Cluster benötigen, erhalten Sie weitere Informationen im AKS-Schnellstart. Verwenden Sie dafür entweder die [Azure CLI][aks-quickstart-cli] oder das [Azure-Portal][aks-quickstart-portal].

Außerdem muss die Version 2.0.46 oder höher der Azure-Befehlszeilenschnittstelle installiert und konfiguriert sein. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0][install-azure-cli] Informationen dazu.

## <a name="create-an-azure-file-share"></a>Erstellen einer Azure-Dateifreigabe

Bevor Sie eine Azure Files-Freigabe als Kubernetes-Volume verwenden können, müssen Sie ein Azure Storage-Konto und die Dateifreigabe erstellen. Das folgende Skript erstellt die Ressourcengruppe *myAKSShare*, ein Speicherkonto und die Files-Freigabe *aksshare*:

```azurecli
#!/bin/bash

# Change these four parameters
AKS_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM
AKS_PERS_RESOURCE_GROUP=myAKSShare
AKS_PERS_LOCATION=eastus
AKS_PERS_SHARE_NAME=aksshare

# Create the Resource Group
az group create --name $AKS_PERS_RESOURCE_GROUP --location $AKS_PERS_LOCATION

# Create the storage account
az storage account create -n $AKS_PERS_STORAGE_ACCOUNT_NAME -g $AKS_PERS_RESOURCE_GROUP -l $AKS_PERS_LOCATION --sku Standard_LRS

# Export the connection string as an environment variable, this is used when creating the Azure file share
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string -n $AKS_PERS_STORAGE_ACCOUNT_NAME -g $AKS_PERS_RESOURCE_GROUP -o tsv`

# Create the file share
az storage share create -n $AKS_PERS_SHARE_NAME

# Get storage account key
STORAGE_KEY=$(az storage account keys list --resource-group $AKS_PERS_RESOURCE_GROUP --account-name $AKS_PERS_STORAGE_ACCOUNT_NAME --query "[0].value" -o tsv)

# Echo storage account name and key
echo Storage account name: $AKS_PERS_STORAGE_ACCOUNT_NAME
echo Storage account key: $STORAGE_KEY
```

Notieren Sie sich den Speicherkontonamen und den Schlüssel, die am Ende der Skriptausgabe angezeigt werden. Diese Werte werden bei der Erstellung des Kubernetes-Volumes in einem der folgenden Schritte benötigt.

## <a name="create-a-kubernetes-secret"></a>Erstellen eines Kubernetes-Geheimnisses

Kubernetes benötigt Anmeldeinformationen für den Zugriff auf die im vorherigen Schritt erstellte Dateifreigabe. Diese Anmeldeinformationen werden in einem [Kubernetes-Geheimnis][kubernetes-secret] gespeichert, auf das bei der Erstellung eines Kubernetes-Pod verwiesen wird.

Verwenden Sie den Befehl `kubectl create secret`, um das Geheimnis zu erstellen. Das folgende Beispiel erstellt die Freigabe *azure-secret* und füllt *azurestorageaccountname* und *azurestorageaccountkey* aus dem vorherigen Schritt aus. Um ein vorhandenes Azure Storage-Konto zu verwenden, geben Sie den Kontonamen und den Zugriffsschlüssel an.

```console
kubectl create secret generic azure-secret --from-literal=azurestorageaccountname=$AKS_PERS_STORAGE_ACCOUNT_NAME --from-literal=azurestorageaccountkey=$STORAGE_KEY
```

## <a name="mount-the-file-share-as-a-volume"></a>Einbinden der Dateifreigabe als Volume

Um die Azure Files-Freigabe in den Pod einzubinden, konfigurieren Sie das Volume in der Containerspezifikation. Erstellen Sie eine neue Datei namens „`azure-files-pod.yaml`“ mit folgendem Inhalt. Wenn Sie den Namen oder den geheimen Namen der Files-Freigabe geändert haben, aktualisieren Sie *shareName* und *secretName*. Aktualisieren Sie bei Bedarf den Wert `mountPath`. Dies ist der Pfad, unter dem die Files-Freigabe im Pod eingebunden wird.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
  - image: nginx:1.15.5
    name: mypod
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
      limits:
        cpu: 250m
        memory: 256Mi
    volumeMounts:
      - name: azure
        mountPath: /mnt/azure
  volumes:
  - name: azure
    azureFile:
      secretName: azure-secret
      shareName: aksshare
      readOnly: false
```

Um den Pod zu erstellen, verwenden Sie den Befehl `kubectl`.

```azurecli-interactive
kubectl apply -f azure-files-pod.yaml
```

Sie verfügen nun über einen ausgeführten Pod mit einer Azure Files-Freigabe, die unter */mnt/azure* eingebunden ist. Sie können mit `kubectl describe pod mypod` überprüfen, ob die Freigabe erfolgreich eingebunden wurde. In der folgenden verkürzten Beispielausgabe ist das im Container eingebundene Volume angegeben:

```
Containers:
  mypod:
    Container ID:   docker://86d244cfc7c4822401e88f55fd75217d213aa9c3c6a3df169e76e8e25ed28166
    Image:          nginx:1.15.5
    Image ID:       docker-pullable://nginx@sha256:9ad0746d8f2ea6df3a17ba89eca40b48c47066dfab55a75e08e2b70fc80d929e
    State:          Running
      Started:      Mon, 08 Oct 2018 19:28:34 +0000
    Ready:          True
    Mounts:
      /mnt/azure from azure (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-z5sd7 (ro)
[...]
Volumes:
  azure:
    Type:        AzureFile (an Azure File Service mount on the host and bind mount to the pod)
    SecretName:  azure-secret
    ShareName:   aksshare
    ReadOnly:    false
  default-token-z5sd7:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-z5sd7
[...]
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Interaktion von AKS-Clustern mit Azure Files finden Sie beim [Kubernetes-Plug-In für Azure Files][kubernetes-files].

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/user-guide/kubectl/v1.8/#create
[kubernetes-files]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_file/README.md
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/volumes/
[smb-overview]: /windows/desktop/FileIO/microsoft-smb-protocol-and-cifs-protocol-overview

<!-- LINKS - internal -->
[az-group-create]: /cli/azure/group#az-group-create
[az-storage-create]: /cli/azure/storage/account#az-storage-account-create
[az-storage-key-list]: /cli/azure/storage/account/keys#az-storage-account-keys-list
[az-storage-share-create]: /cli/azure/storage/share#az-storage-share-create
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
