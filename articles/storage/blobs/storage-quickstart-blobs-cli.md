---
title: 'Azure-Schnellstart: Erstellen eines Blobs im Objektspeicher mithilfe der Azure CLI | Microsoft-Dokumentation'
description: In dieser Schnellstartanleitung verwenden Sie die Azure CLI im Objektspeicher (Blob). Anschließend verwenden Sie die CLI, um ein Blob in Azure Storage hochzuladen, ein Blob herunterzuladen und die Blobs in einem Container aufzulisten.
services: storage
author: roygara
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 04/09/2018
ms.author: rogarana
ms.openlocfilehash: 106c84a6eaff3766c5308ccb130dc7c21e7ec4d8
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2018
ms.locfileid: "39397011"
---
# <a name="quickstart-upload-download-and-list-blobs-using-the-azure-cli"></a>Schnellstart: Hochladen, Herunterladen und Auflisten von Blobs per Azure CLI

Die Azure CLI ist die Befehlszeilenumgebung von Azure und dient zum Verwalten von Azure-Ressourcen. Sie können sie in Ihrem Browser mit Azure Cloud Shell verwenden. Außerdem können Sie sie unter macOS, Linux oder Windows installieren und über die Befehlszeile ausführen. In diesem Schnellstart wird beschrieben, wie Sie die Azure CLI zum Hoch- und Herunterladen von Daten für Azure Blob Storage verwenden.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für diesen Schnellstart die Azure CLI-Version 2.0.4 oder höher ausführen. Führen Sie `az --version` aus, um Ihre Version zu ermitteln. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0](/cli/azure/install-azure-cli) Informationen dazu.

[!INCLUDE [storage-quickstart-tutorial-intro-include-cli](../../../includes/storage-quickstart-tutorial-intro-include-cli.md)]

## <a name="create-a-container"></a>Erstellen eines Containers

Blobs werden immer in einen Container hochgeladen. Sie können Gruppen von Blobs ähnlich wie Dateien in Ordnern auf Ihrem Computer organisieren.

Erstellen Sie mit dem Befehl [az storage container create](/cli/azure/storage/container#az_storage_container_create) einen Container zum Speichern von Blobs.

```azurecli-interactive
az storage container create --name mystoragecontainer
```

## <a name="upload-a-blob"></a>Hochladen eines Blobs

Blobspeicher unterstützt Block-, Anfüge- und Seitenblobs. Die meisten Dateien, die in Blob Storage gespeichert werden, werden als Blockblobs gespeichert. Anfügeblobs werden verwendet, wenn Daten einem vorhandenen Blob hinzugefügt werden müssen, ohne den vorhandenen Inhalt zu ändern, z.B. bei der Protokollierung. Seitenblobs sind die Grundlage für VHD-Dateien von virtuellen IaaS-Computern.

Erstellen Sie zunächst eine Datei für den Upload in ein Blob.
Verwenden Sie bei Verwendung von Azure Cloud Shell Folgendes, um eine Datei zu erstellen: `vi helloworld`. Wenn die Datei geöffnet wird, drücken Sie **EINFG**, geben Sie „Hello world“ ein, drücken Sie **ESC**, geben Sie `:x` ein, und drücken Sie die **EINGABETASTE**.

In diesem Beispiel laden Sie mithilfe des Befehls [az storage blob upload](/cli/azure/storage/blob#az_storage_blob_upload) ein Blob in den Container hoch, den Sie im letzten Schritt erstellt haben.

```azurecli-interactive
az storage blob upload \
    --container-name mystoragecontainer \
    --name blobName \
    --file ~/path/to/local/file
```

Wenn Sie mithilfe der zuvor beschriebenen Methode eine Datei in Azure Cloud Shell erstellt haben, können Sie stattdessen den folgenden CLI-Befehl verwenden. (Beachten Sie, dass Sie keinen Pfad angeben müssen, da die Datei im Basisverzeichnis erstellt wurde. Normalerweise muss ein Pfad angegeben werden.)

```azurecli-interactive
az storage blob upload \
    --container-name mystoragecontainer \
    --name helloworld
    --file helloworld
```

Dabei wird das Blob erstellt, falls es nicht vorhanden ist, oder überschrieben, falls es bereits vorhanden ist. Laden Sie beliebig viele Dateien hoch, bevor Sie fortfahren.

Wenn Sie mehrere Dateien gleichzeitig hochladen möchten, können Sie den Befehl [az storage blob upload-batch](/cli/azure/storage/blob#az_storage_blob_upload_batch) verwenden.

## <a name="list-the-blobs-in-a-container"></a>Auflisten der Blobs in einem Container

Sie können die Blobs im Container mit dem Befehl [az storage blob list](/cli/azure/storage/blob#az_storage_blob_list) auflisten.

```azurecli-interactive
az storage blob list \
    --container-name mystoragecontainer \
    --output table
```

## <a name="download-a-blob"></a>Herunterladen eines Blobs

Mit dem Befehl [az storage blob download](/cli/azure/storage/blob#az_storage_blob_download) können Sie das zuvor hochgeladene Blob herunterladen.

```azurecli-interactive
az storage blob download \
    --container-name mystoragecontainer \
    --name blobName \
    --file ~/destination/path/for/file
```

## <a name="data-transfer-with-azcopy"></a>Übertragen von Daten mit AzCopy

Das Dienstprogramm [AzCopy](../common/storage-use-azcopy-linux.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) ist eine weitere Option für eine leistungsstarke, skriptfähige Datenübertragung für Azure Storage. Sie können AzCopy zum Übertragen von Daten in und aus Blob-, Datei- und Tabellenspeicher verwenden.

Als kurzes Beispiel sehen Sie hier den AzCopy-Befehl für das Hochladen der Datei *myfile.txt* in den Container *mystoragecontainer*.

```bash
azcopy \
    --source /mnt/myfiles \
    --destination https://mystorageaccount.blob.core.windows.net/mystoragecontainer \
    --dest-key <storage-account-access-key> \
    --include "myfile.txt"
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Ressourcen in der Ressourcengruppe, einschließlich des in dieser Schnellstartanleitung erstellten Speicherkontos, nicht mehr benötigen, löschen Sie die Ressourcengruppe mit dem Befehl [az group delete](/cli/azure/group#az_group_delete).

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie gelernt, wie Sie Dateien zwischen lokalen Festplatten und einem Container in Azure Blob Storage übertragen. Weitere Informationen zum Arbeiten mit Blobs in Azure Storage finden Sie im Tutorial zum Arbeiten mit Azure Blob Storage.

> [!div class="nextstepaction"]
> [Gewusst wie: Blob Storage-Vorgänge mit der Azure-Befehlszeilenschnittstelle](storage-how-to-use-blobs-cli.md)
