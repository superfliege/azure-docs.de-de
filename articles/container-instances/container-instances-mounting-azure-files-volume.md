---
title: Einbinden eines Azure Files-Volumes in Azure Container Instances
description: Erfahren Sie, wie Sie ein Azure Files-Volume einbinden, sodass der Zustand bei Azure Container Instances beibehalten wird.
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 12/05/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: b2e8e27cecb4d1225e378690063b42f5d5242868
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2017
---
# <a name="mount-an-azure-file-share-with-azure-container-instances"></a>Einbinden einer Azure-Dateifreigabe in Azure Container Instances

Standardmäßig ist Azure Container Instances zustandslos. Wenn der Container abstürzt oder beendet wird, gehen alle Zustände verloren. Um den Zustand nach Ablauf der Lebensdauer des Containers beizubehalten, müssen Sie ein Volume aus einem externen Speicher einbinden. In diesem Artikel wird das Einbinden einer Azure-Dateifreigabe für die Verwendung mit Azure Container Instances veranschaulicht.

## <a name="create-an-azure-file-share"></a>Erstellen einer Azure-Dateifreigabe

Bevor Sie eine Azure-Dateifreigabe in Azure Container Instances einbinden, müssen Sie sie erstellen. Führen Sie das folgende Skript aus, um ein Speicherkonto zum Hosten der Dateifreigabe und die Freigabe selbst zu erstellen. Der Name des Speicherkontos muss global eindeutig sein, daher fügt das Skript der Basis-Zeichenfolge einen Zufallswert hinzu.

```azurecli-interactive
# Change these four parameters as needed
ACI_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM
ACI_PERS_RESOURCE_GROUP=myResourceGroup
ACI_PERS_LOCATION=eastus
ACI_PERS_SHARE_NAME=acishare

# Create the storage account with the parameters
az storage account create -n $ACI_PERS_STORAGE_ACCOUNT_NAME -g $ACI_PERS_RESOURCE_GROUP -l $ACI_PERS_LOCATION --sku Standard_LRS

# Export the connection string as an environment variable. The following 'az storage share create' command
# references this environment variable when creating the Azure file share.
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string -n $ACI_PERS_STORAGE_ACCOUNT_NAME -g $ACI_PERS_RESOURCE_GROUP -o tsv`

# Create the file share
az storage share create -n $ACI_PERS_SHARE_NAME
```

## <a name="acquire-storage-account-access-details"></a>Erhalten der Zugriffsinformationen für das Azure-Speicherkonto

Um eine Azure-Dateifreigabe als Volume in Azure Container Instances einzubinden, benötigen Sie drei Werte: den Namen des Speicherkontos, den Freigabenamen und den Speicherzugriffsschlüssel.

Wenn Sie das oben angegebene Skript verwendet haben, wurde der Name des Speicherkontos mit einem zufälligen Wert am Ende erstellt. Um die endgültige Zeichenfolge (einschließlich des zufälligen Teils) abzufragen, verwenden Sie die folgenden Befehle:

```azurecli-interactive
STORAGE_ACCOUNT=$(az storage account list --resource-group $ACI_PERS_RESOURCE_GROUP --query "[?contains(name,'$ACI_PERS_STORAGE_ACCOUNT_NAME')].[name]" -o tsv)
echo $STORAGE_ACCOUNT
```

Der Freigabename ist bereits bekannt (im Skript oben festgelegt als *acishare*), daher verbleibt nur der Speicherkontoschlüssel, der mit dem folgenden Befehl gesucht werden kann:

```azurecli-interactive
STORAGE_KEY=$(az storage account keys list --resource-group $ACI_PERS_RESOURCE_GROUP --account-name $STORAGE_ACCOUNT --query "[0].value" -o tsv)
echo $STORAGE_KEY
```

## <a name="deploy-the-container-and-mount-the-volume"></a>Bereitstellen des Containers und Einbinden des Volumes

Um eine Azure-Dateifreigabe als Volume in einem Container einzubinden, geben Sie die Freigabe und den Einbindungspunkt des Volumes bei der Erstellung des Containers mit [az container create](/cli/azure/container#az_container_create) an. Wenn Sie die vorherigen Schritte ausgeführt haben, können Sie die Freigabe, die Sie zuvor erstellt haben, einbinden, indem Sie mit dem folgenden Befehl einen Container erstellen:

```azurecli-interactive
az container create \
    --resource-group $ACI_PERS_RESOURCE_GROUP \
    --name hellofiles \
    --image seanmckenna/aci-hellofiles \
    --ip-address Public \
    --ports 80 \
    --azure-file-volume-account-name $ACI_PERS_STORAGE_ACCOUNT_NAME \
    --azure-file-volume-account-key $STORAGE_KEY \
    --azure-file-volume-share-name $ACI_PERS_SHARE_NAME \
    --azure-file-volume-mount-path /aci/logs/
```

## <a name="manage-files-in-mounted-volume"></a>Verwalten von Dateien in eingebundenen Datenträgern

Nachdem der Container gestartet wurde, können Sie mithilfe der einfachen Web-App, die über das Image [seanmckenna/aci-hellofiles](https://hub.docker.com/r/seanmckenna/aci-hellofiles/) bereitgestellt wird, die Dateien in der Azure-Dateifreigabe unter dem von Ihnen angegebenen Bereitstellungspfad verwalten. Rufen Sie die IP-Adresse für die Web-App mit dem Befehl [az container show](/cli/azure/container#az_container_show) ab:

```azurecli-interactive
az container show --resource-group $ACI_PERS_RESOURCE_GROUP --name hellofiles -o table
```

Mithilfe des [Azure-Portals](https://portal.azure.com) oder eines Tools wie [Microsoft Azure Storage-Explorer](https://storageexplorer.com) können Sie die in die Dateifreigabe geschriebene Datei abrufen und überprüfen.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über die [Beziehung zwischen Azure Container Instances und Containerorchestratoren](container-instances-orchestrator-relationship.md).
