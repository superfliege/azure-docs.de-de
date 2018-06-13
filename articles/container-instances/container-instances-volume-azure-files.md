---
title: Einbinden eines Azure Files-Volumes in Azure Container Instances
description: Erfahren Sie, wie Sie ein Azure Files-Volume einbinden, sodass der Zustand bei Azure Container Instances beibehalten wird.
services: container-instances
author: seanmck
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 02/20/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 239150c1e752ce6a4f2a19fa1192cd1a910ebea9
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
ms.locfileid: "32166795"
---
# <a name="mount-an-azure-file-share-in-azure-container-instances"></a>Einbinden einer Azure-Dateifreigabe in Azure Container Instances

Standardmäßig ist Azure Container Instances zustandslos. Wenn der Container abstürzt oder beendet wird, gehen alle Zustände verloren. Um den Zustand nach Ablauf der Lebensdauer des Containers beizubehalten, müssen Sie ein Volume aus einem externen Speicher einbinden. In diesem Artikel wird das Einbinden einer Azure-Dateifreigabe für die Verwendung mit Azure Container Instances veranschaulicht.

> [!NOTE]
> Zurzeit ist das Einbinden einer Azure-Dateifreigabe in Linux-Container eingeschränkt. Bis alle Features auch für Windows-Container verfügbar sind, finden Sie die aktuellen Plattformunterschiede unter [Kontingente und Regionsverfügbarkeit für Azure Container Instances](container-instances-quotas.md).

## <a name="create-an-azure-file-share"></a>Erstellen einer Azure-Dateifreigabe

Bevor Sie eine Azure-Dateifreigabe in Azure Container Instances einbinden, müssen Sie sie erstellen. Führen Sie das folgende Skript aus, um ein Speicherkonto zum Hosten der Dateifreigabe und die Freigabe selbst zu erstellen. Der Name des Speicherkontos muss global eindeutig sein, daher fügt das Skript der Basis-Zeichenfolge einen Zufallswert hinzu.

```azurecli-interactive
# Change these four parameters as needed
ACI_PERS_RESOURCE_GROUP=myResourceGroup
ACI_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM
ACI_PERS_LOCATION=eastus
ACI_PERS_SHARE_NAME=acishare

# Create the storage account with the parameters
az storage account create \
    --resource-group $ACI_PERS_RESOURCE_GROUP \
    --name $ACI_PERS_STORAGE_ACCOUNT_NAME \
    --location $ACI_PERS_LOCATION \
    --sku Standard_LRS

# Export the connection string as an environment variable. The following 'az storage share create' command
# references this environment variable when creating the Azure file share.
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string --resource-group $ACI_PERS_RESOURCE_GROUP --name $ACI_PERS_STORAGE_ACCOUNT_NAME --output tsv`

# Create the file share
az storage share create -n $ACI_PERS_SHARE_NAME
```

## <a name="get-storage-credentials"></a>Erhalten der Zugriffsinformationen für das Azure-Speicherkonto

Um eine Azure-Dateifreigabe als Volume in Azure Container Instances einzubinden, benötigen Sie drei Werte: den Namen des Speicherkontos, den Freigabenamen und den Speicherzugriffsschlüssel.

Wenn Sie das oben angegebene Skript verwendet haben, wurde der Name des Speicherkontos mit einem zufälligen Wert am Ende erstellt. Um die endgültige Zeichenfolge (einschließlich des zufälligen Teils) abzufragen, verwenden Sie die folgenden Befehle:

```azurecli-interactive
STORAGE_ACCOUNT=$(az storage account list --resource-group $ACI_PERS_RESOURCE_GROUP --query "[?contains(name,'$ACI_PERS_STORAGE_ACCOUNT_NAME')].[name]" --output tsv)
echo $STORAGE_ACCOUNT
```

Der Freigabename ist bereits bekannt (im Skript oben festgelegt als *acishare*), daher verbleibt nur der Speicherkontoschlüssel, der mit dem folgenden Befehl gesucht werden kann:

```azurecli-interactive
STORAGE_KEY=$(az storage account keys list --resource-group $ACI_PERS_RESOURCE_GROUP --account-name $STORAGE_ACCOUNT --query "[0].value" --output tsv)
echo $STORAGE_KEY
```

## <a name="deploy-container-and-mount-volume"></a>Bereitstellen des Containers und Einbinden des Volumes

Um eine Azure-Dateifreigabe als Volume in einen Container einzubinden, geben Sie die Freigabe und den Einbindungspunkt des Volumes bei der Erstellung des Containers mit [az container create][az-container-create] an. Wenn Sie die vorherigen Schritte ausgeführt haben, können Sie die Freigabe, die Sie zuvor erstellt haben, einbinden, indem Sie mit dem folgenden Befehl einen Container erstellen:

```azurecli-interactive
az container create \
    --resource-group $ACI_PERS_RESOURCE_GROUP \
    --name hellofiles \
    --image microsoft/aci-hellofiles \
    --dns-name-label aci-demo \
    --ports 80 \
    --azure-file-volume-account-name $ACI_PERS_STORAGE_ACCOUNT_NAME \
    --azure-file-volume-account-key $STORAGE_KEY \
    --azure-file-volume-share-name $ACI_PERS_SHARE_NAME \
    --azure-file-volume-mount-path /aci/logs/
```

Der Wert `--dns-name-label` muss innerhalb der Azure-Region, in der Sie die Containerinstanz erstellen, eindeutig sein. Aktualisieren Sie den Wert im vorherigen Befehl, wenn Sie beim Ausführen des Befehls eine Fehlermeldung bezüglich der **DNS-Namensbezeichnung** erhalten.

## <a name="manage-files-in-mounted-volume"></a>Verwalten von Dateien in eingebundenen Datenträgern

Nachdem der Container gestartet wurde, können Sie mithilfe der einfachen Web-App, die über das Image [microsoft/aci-hellofiles][aci-hellofiles] bereitgestellt wird, die Dateien in der Azure-Dateifreigabe unter dem von Ihnen angegebenen Einbindungspfad verwalten. Beziehen Sie den vollqualifizierten Domänennamen (FQDN) für die Web-App mit dem Befehl [az container show][az-container-show]:

```azurecli-interactive
az container show --resource-group $ACI_PERS_RESOURCE_GROUP --name hellofiles --query ipAddress.fqdn
```

Mithilfe des [Azure-Portals][portal] oder eines Tools wie [Microsoft Azure Storage-Explorer][storage-explorer] können Sie die in die Dateifreigabe geschriebene Datei abrufen und überprüfen.

## <a name="mount-multiple-volumes"></a>Einbinden mehrerer Volumes

Sie können mehrere Volumes in eine Containerinstanz einbinden, indem Sie zum Bereitstellen eine [Azure Resource Manager-Vorlage](/azure/templates/microsoft.containerinstance/containergroups) verwenden.

Stellen Sie hierzu zuerst die Freigabedetails bereit und legen die Volumes fest, indem Sie das Array `volumes` im Abschnitt `properties` der Vorlage auffüllen. Wenn Sie z.B. zwei Azure Files-Freigaben mit den Namen *share1* und *share2* im Speicherkonto *MyStorageAccount* erstellt haben, wird das Array `volumes` ähnlich wie im folgenden Beispiel aussehen:

```json
"volumes": [{
  "name": "myvolume1",
  "azureFile": {
    "shareName": "share1",
    "storageAccountName": "myStorageAccount",
    "storageAccountKey": "<storage-account-key>"
  }
},
{
  "name": "myvolume2",
  "azureFile": {
    "shareName": "share2",
    "storageAccountName": "myStorageAccount",
    "storageAccountKey": "<storage-account-key>"
  }
}]
```

Füllen Sie als Nächstes für jeden Container in der Containergruppe, in den Sie die Volumes einbinden möchten, das Array `volumeMounts` im Abschnitt `properties` der Containerdefinition auf. Dadurch werden beispielsweise die beiden zuvor definierten Volumes *myvolume1* und *myvolume2* eingebunden:

```json
"volumeMounts": [{
  "name": "myvolume1",
  "mountPath": "/mnt/share1/"
},
{
  "name": "myvolume2",
  "mountPath": "/mnt/share2/"
}]
```

Ein Beispiel für die Bereitstellung der Containerinstanzen mit einer Azure Resource Manager-Vorlage finden Sie unter [Bereitstellen von Gruppen mit mehreren Containern in Azure Container Instances](container-instances-multi-container-group.md).

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie andere Volumetypen in Azure Container Instances bereitgestellt werden:

* [Einbinden eines emptyDir-Volumes in Azure Container Instances](container-instances-volume-emptydir.md)
* [Einbinden eines gitRepo-Volumes in Azure Container Instances](container-instances-volume-gitrepo.md)
* [Einbinden eines geheimen Volumes in Azure Container Instances](container-instances-volume-secret.md)

<!-- LINKS - External -->
[aci-hellofiles]: https://hub.docker.com/r/microsoft/aci-hellofiles/
[portal]: https://portal.azure.com
[storage-explorer]: https://storageexplorer.com

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az_container_create
[az-container-show]: /cli/azure/container#az_container_show