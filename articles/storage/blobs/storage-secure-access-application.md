---
title: Sicherer Zugriff auf Anwendungsdaten in der Cloud mit Azure Storage | Microsoft-Dokumentation
description: "Verwenden von SAS-Token, Verschlüsselung und HTTPS zum Schutz Ihrer Anwendungsdaten in der Cloud"
services: storage
documentationcenter: 
author: georgewallace
manager: timlt
editor: 
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: csharp
ms.topic: tutorial
ms.date: 09/19/2017
ms.author: gwallace
ms.custom: mvc
ms.openlocfilehash: 11b141617bea5962c45e1b91cf6629c2edd26c3d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="secure-access-to-an-applications-data-in-the-cloud"></a>Sichern des Zugriffs auf die Daten einer Anwendung in der Cloud

Dieses Tutorial ist der dritte Teil einer Reihe. Hier erfahren Sie, wie Sie den Zugriff auf das Speicherkonto sichern. 

Im dritten Teil der Serie lernen Sie Folgendes:

> [!div class="checklist"]
> * Verwenden von SAS-Token, um auf Miniaturbilder zuzugreifen
> * Aktivieren der serverseitigen Verschlüsselung
> * Aktivieren der Übertragung nur über HTTPS

[Azure Blob Storage](../common/storage-introduction.md#blob-storage) ist ein zuverlässiger Dienst zum Speichern von Dateien für Anwendungen. Dieses Tutorial baut auf das [vorhergehende Thema][previous-tutorial] auf und veranschaulicht, wie Sie den Zugriff auf Ihr Speicherkonto aus einer Webanwendung sichern. Nach Abschluss sind die Bilder verschlüsselt, und die Web-App verwendet sichere SAS-Token für den Zugriff auf die Miniaturbilder.

## <a name="prerequisites"></a>Voraussetzungen

Um dieses Tutorial durcharbeiten zu können, müssen Sie das vorhergehende Storage-Tutorial absolviert haben: [Automatisieren der Größenänderung von hochgeladenen Bildern mit Event Grid][previous-tutorial]. 

## <a name="set-container-public-access"></a>Festlegen des öffentlichen Zugriffs auf den Container

In diesem Teil der Tutorialreihe werden SAS-Token für den Zugriff auf die Miniaturbilder verwendet. In diesem Schritt legen Sie den öffentlichen Zugriff auf den Container _thumbs_ auf `off` fest.

```azurecli-interactive 
blobStorageAccount=<blob_storage_account>

blobStorageAccountKey=$(az storage account keys list -g myResourceGroup \
-n $blobStorageAccount --query [0].value --output tsv) 

az storage container set-permission \ --account-name $blobStorageAccount \ --account-key $blobStorageAccountKey \ --name thumbs  \
--public-access off
``` 

## <a name="configure-sas-tokens-for-thumbnails"></a>Konfigurieren von SAS-Token für Miniaturbilder

In Teil 1 dieser Tutorialreihe wurden Bilder aus einem öffentlichen Container in der Webanwendung angezeigt. In diesem Teil der Reihe verwenden Sie [SAS-Token (Secure Access Signature)](../common/storage-dotnet-shared-access-signature-part-1.md#what-is-a-shared-access-signature), um die Miniaturbilder abzurufen. SAS-Token ermöglichen den eingeschränkten Zugriff auf einen Container oder ein Blob in Abhängigkeit von IP, Protokoll, Zeitintervall und gewährten Rechten.

In diesem Beispiel wird der Branch `sasTokens` im Quellcoderepository verwendet, der ein aktualisiertes Codebeispiel enthält. Löschen Sie die vorhandene GitHub-Bereitstellung mit [az webapp deployment source delete](/cli/azure/webapp/deployments/source#delete). Anschließend konfigurieren Sie die GitHub-Bereitstellung für die Web-App mit dem Befehl [az webapp deployment source config](/cli/azure/webapp/deployment/source#config).  

Im folgenden Befehl ist `<web-app>` der Name Ihrer Web-App.  

```azurecli-interactive 
az webapp deployment source delete --name <web-app> --resource-group myResourceGroup

az webapp deployment source config --name <web_app> \
--resource-group myResourceGroup --branch sasTokens --manual-integration \
--repo-url https://github.com/Azure-Samples/storage-blob-upload-from-webapp
``` 

Mit dem Branch `sasTokens` des Repositorys wird die Datei `StorageHelper.cs` aktualisiert. Diese ersetzt den Task `GetThumbNailUrls` durch das Codebeispiel unten. Der aktualisierte Task ruft die Miniaturbild-URLs ab, indem eine [SharedAccessBlobPolicy](/dotnet/api/microsoft.windowsazure.storage.blob.sharedaccessblobpolicy?view=azure-dotnet) so festgelegt wird, dass sie die Anfangszeit, die Ablaufzeit und die Berechtigungen für das SAS-Token angibt. Nach der Bereitstellung der Web-App werden die Miniaturbilder über eine URL mit einem SAS-Token abgerufen. Der aktualisierte Task ist im folgenden Beispiel dargestellt:
    
```csharp
public static async Task<List<string>> GetThumbNailUrls(AzureStorageConfig _storageConfig)
{
    List<string> thumbnailUrls = new List<string>();

    // Create storagecredentials object by reading the values from the configuration (appsettings.json)
    StorageCredentials storageCredentials = new StorageCredentials(_storageConfig.AccountName, _storageConfig.AccountKey);

    // Create cloudstorage account by passing the storagecredentials
    CloudStorageAccount storageAccount = new CloudStorageAccount(storageCredentials, true);

    // Create blob client
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get reference to the container
    CloudBlobContainer container = blobClient.GetContainerReference(_storageConfig.ThumbnailContainer);

    // Set the permission of the container to public
    await container.SetPermissionsAsync(new BlobContainerPermissions { PublicAccess = BlobContainerPublicAccessType.Blob });

    BlobContinuationToken continuationToken = null;

    BlobResultSegment resultSegment = null;

    //Call ListBlobsSegmentedAsync and enumerate the result segment returned, while the continuation token is non-null.
    //When the continuation token is null, the last page has been returned and execution can exit the loop.
    do
    {
        //This overload allows control of the page size. You can return all remaining results by passing null for the maxResults parameter,
        //or by calling a different overload.
        resultSegment = await container.ListBlobsSegmentedAsync("", true, BlobListingDetails.All, 10, continuationToken, null, null);

        foreach (var blobItem in resultSegment.Results)
        {
            CloudBlockBlob blob = blobItem as CloudBlockBlob;
            //Set the expiry time and permissions for the blob.
            //In this case, the start time is specified as a few minutes in the past, to mitigate clock skew.
            //The shared access signature will be valid immediately.
            SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();

            sasConstraints.SharedAccessStartTime = DateTimeOffset.UtcNow.AddMinutes(-5);

            sasConstraints.SharedAccessExpiryTime = DateTimeOffset.UtcNow.AddHours(24);

            sasConstraints.Permissions = SharedAccessBlobPermissions.Read;

            //Generate the shared access signature on the blob, setting the constraints directly on the signature.
            string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

            //Return the URI string for the container, including the SAS token.
            thumbnailUrls.Add(blob.Uri + sasBlobToken);

        }

        //Get the continuation token.
        continuationToken = resultSegment.ContinuationToken;
    }

    while (continuationToken != null);

    return await Task.FromResult(thumbnailUrls);
}
```

Die folgenden Klassen, Eigenschaften und Methoden werden im vorhergehenden Task verwendet:

|Klasse  |Eigenschaften| Methoden  |
|---------|---------|---------|
|[StorageCredentials](/dotnet/api/microsoft.windowsazure.storage.auth.storagecredentials?view=azure-dotnet)    |         |
|[CloudStorageAccount](/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount?view=azure-dotnet)     | |[CreateCloudBlobClient](/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount.createcloudblobclient?view=azure-dotnet#Microsoft_WindowsAzure_Storage_CloudStorageAccount_CreateCloudBlobClient)        |
|[CloudBlobClient](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobclient?view=azure-dotnet)     | |[GetContainerReference](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobclient.getcontainerreference?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlobClient_GetContainerReference_System_String_)         |
|[CloudBlobContainer](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer?view=azure-dotnet)     | |[SetPermissionsAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.setpermissionsasync?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlobContainer_SetPermissionsAsync_Microsoft_WindowsAzure_Storage_Blob_BlobContainerPermissions_) <br> [ListBlobsSegmentedAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobssegmentedasync?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlobContainer_ListBlobsSegmentedAsync_System_String_System_Boolean_Microsoft_WindowsAzure_Storage_Blob_BlobListingDetails_System_Nullable_System_Int32__Microsoft_WindowsAzure_Storage_Blob_BlobContinuationToken_Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_Microsoft_WindowsAzure_Storage_OperationContext_)       |
|[BlobContinuationToken](/dotnet/api/microsoft.windowsazure.storage.blob.blobcontinuationtoken?view=azure-dotnet)     |         |
|[BlobResultSegment](/dotnet/api/microsoft.windowsazure.storage.blob.blobresultsegment?view=azure-dotnet)    | [Ergebnisse](/dotnet/api/microsoft.windowsazure.storage.blob.blobresultsegment.results?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobResultSegment_Results)         |
|[CloudBlockBlob](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob?view=azure-dotnet)    |         | [GetSharedAccessSignature](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob.getsharedaccesssignature?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlob_GetSharedAccessSignature_Microsoft_WindowsAzure_Storage_Blob_SharedAccessBlobPolicy_)
|[SharedAccessBlobPolicy](/dotnet/api/microsoft.windowsazure.storage.blob.sharedaccessblobpolicy?view=azure-dotnet)     | [SharedAccessStartTime](/dotnet/api/microsoft.windowsazure.storage.blob.sharedaccessblobpolicy.sharedaccessstarttime?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_SharedAccessBlobPolicy_SharedAccessStartTime)<br>[SharedAccessExpiryTime](/dotnet/api/microsoft.windowsazure.storage.blob.sharedaccessblobpolicy.sharedaccessexpirytime?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_SharedAccessBlobPolicy_SharedAccessExpiryTime)<br>[Berechtigungen](/dotnet/api/microsoft.windowsazure.storage.blob.sharedaccessblobpolicy.permissions?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_SharedAccessBlobPolicy_Permissions) |        |

## <a name="server-side-encryption"></a>Serverseitige Verschlüsselung

[Azure Storage Service Encryption (SSE)](../common/storage-service-encryption.md) vereinfacht das Sichern und Schützen Ihrer Daten. SSE verschlüsselt ruhende Daten und übernimmt die Verschlüsselung, die Entschlüsselung und die Schlüsselverwaltung. Sämtliche Daten werden mittels 256-Bit- [AES-Verschlüsselung](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)verschlüsselt, einem der sichersten verfügbaren Blockverschlüsselungsverfahren.

Im folgenden Beispiel wird die Verschlüsselung für Blobs aktiviert. Vorhandene Blobs, die vor dem Aktivieren der Verschlüsselung erstellt wurden, werden nicht verschlüsselt. Im Header `x-ms-server-encrypted` einer Blobanforderung ist der Verschlüsselungsstatus des Blobs angegeben.

```azurecli-interactive
az storage account update --encryption-services blob --name <storage-account-name> --resource-group myResourceGroup
```

Nachdem die Verschlüsselung aktiviert wurde, laden Sie ein neues Bild in die Webanwendung hoch.

Verwenden Sie `curl` mit dem Schalter `-I`, um nur die Header abzurufen, und geben Sie Ihre eigenen Werte für `<storage-account-name>`, `<container>` und `<blob-name>` an.  

```azurecli-interactive
sasToken=$(az storage blob generate-sas \
    --account-name <storage-account-name> \
    --account-key <storage-account-key> \
    --container-name <container> \
    --name <blob-name> \
    --permissions r \
    --expiry `date --date="next day" +%Y-%m-%d` \
    --output tsv)

curl https://<storage-account-name>.blob.core.windows.net/<container>/<blob-name>?$sasToken -I
```

Beachten Sie, dass der Header `x-ms-server-encrypted` in der Antwort den Wert `true` hat. Dieser Header gibt an, dass die Daten jetzt mit SSE verschlüsselt sind.

```
HTTP/1.1 200 OK
Content-Length: 209489
Content-Type: image/png
Last-Modified: Mon, 11 Sep 2017 19:27:42 GMT
Accept-Ranges: bytes
ETag: "0x8D4F94B2BE76D45"
Server: Windows-Azure-Blob/1.0 Microsoft-HTTPAPI/2.0
x-ms-request-id: 57047db3-001e-0050-3e34-2ba769000000
x-ms-version: 2017-04-17
x-ms-lease-status: unlocked
x-ms-lease-state: available
x-ms-blob-type: BlockBlob
x-ms-server-encrypted: true
Date: Mon, 11 Sep 2017 19:27:46 GMT
```

## <a name="enable-https-only"></a>Aktivieren der Übertragung nur über HTTPS

Damit Anforderungen von Daten in und aus einem Speicherkonto geschützt sind, können Sie die Anforderungen auf HTTPS beschränken. Aktualisieren Sie das für das Speicherkonto erforderliche Protokoll mit dem Befehl [az storage account update](/cli/azure/storage/account#update).

```azurecli-interactive
az storage account update --resource-group myresourcegroup --name <storage-account-name> --https-only true
```

Testen Sie die Verbindung mit `curl` über das `HTTP`-Protokoll.

```azurecli-interactive
curl http://<storage-account-name>.blob.core.windows.net/<container>/<blob-name> -I
```

Da jetzt die sichere Übertragung aktiviert ist, erhalten Sie die folgende Meldung:

```
HTTP/1.1 400 The account being accessed does not support http.
```

## <a name="next-steps"></a>Nächste Schritte

In Teil 3 der Reihe haben Sie gelernt, wie Sie den Zugriff auf das Speicherkonto über folgende Vorgänge sichern:

> [!div class="checklist"]
> * Verwenden von SAS-Token, um auf Miniaturbilder zuzugreifen
> * Aktivieren der serverseitigen Verschlüsselung
> * Aktivieren der Übertragung nur über HTTPS

Fahren Sie mit Teil 4 der Reihe fort, um zu erfahren, wie Sie Überwachung und Problembehandlung für eine Cloudspeicheranwendung durchführen.

> [!div class="nextstepaction"]
> [Überwachung und Problembehandlung für eine Cloudspeicheranwendung](storage-monitor-troubleshoot-storage-application.md)

[previous-tutorial]: ../../event-grid/resize-images-on-storage-blob-upload-event.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json