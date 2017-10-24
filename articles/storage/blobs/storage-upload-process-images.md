---
title: Hochladen von Bilddaten in die Cloud mit Azure Storage | Microsoft Docs
description: Verwenden von Azure Blob Storage mit einer Web-App zum Speichern von Anwendungsdaten
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
ms.openlocfilehash: a204498016ff837c5247009eaaffbd4f79285d0b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="upload-image-data-in-the-cloud-with-azure-storage"></a>Hochladen von Bilddaten in die Cloud mit Azure Storage

Dieses Tutorial ist der erste Teil einer Serie. In diesem Tutorial wird gezeigt, wie eine Webanwendung bereitgestellt wird, die die Azure Storage-Clientbibliothek zum Hochladen von Bildern in ein Speicherkonto verwendet. Nachdem Sie das Tutorial abgeschlossen haben, verfügen Sie über eine Web-App, die Bilder in Azure Storage speichern und anzeigen kann.

![Bildcontaineransicht](media/storage-upload-process-images/figure2.png)

Im ersten Teil der Serie lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen Sie ein Speicherkonto.
> * Erstellen eines Containers und Festlegen von Berechtigungen
> * Abrufen eines Zugriffsschlüssels
> * Konfigurieren von Anwendungseinstellungen
> * Bereitstellen einer Web-App in Azure
> * Interagieren mit der Webanwendung

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für dieses Tutorial die Azure CLI-Version 2.0.4 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe 

Erstellen Sie mit dem Befehl [az group create](/cli/azure/group#create) eine Ressourcengruppe. Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden.
 
Im folgenden Beispiel wird eine Ressourcengruppe namens `myResourceGroup` erstellt.
 
```azurecli-interactive 
az group create --name myResourceGroup --location westcentralus 
``` 

## <a name="create-a-storage-account"></a>Erstellen Sie ein Speicherkonto.
 
Das Beispiel lädt Bilder in einen Blob-Container in einem Azure Storage-Konto hoch. Ein Speicherkonto stellt einen eindeutigen Namespace zum Speichern Ihrer Azure Storage-Datenobjekte sowie für den Zugriff darauf bereit. Erstellen Sie ein Speicherkonto in der Ressourcengruppe, die Sie erstellt haben, indem Sie den Befehl [az storage account create](/cli/azure/storage/account#create) verwenden. 

> [!IMPORTANT] 
> In Teil 2 des Tutorials verwenden Sie Ereignisabonnements für Blob Storage. Ereignisabonnements werden zurzeit nur für Blob Storage-Konten in Teilen der USA (Westen-Mitte und Westen 2) unterstützt. Aufgrund dieser Einschränkung müssen Sie ein Blob Storage-Konto erstellen, das von der Beispiel-App zum Speichern von Bildern und Miniaturansichten verwendet wird.   

Ersetzen Sie im folgenden Befehl den Platzhalter `<blob_storage_account>` durch Ihren eigenen global eindeutigen Namen für das Blob Storage-Konto.  

```azurecli-interactive 
az storage account create --name <blob_storage_account> \
--location westcentralus --resource-group myResourceGroup \
--sku Standard_LRS --kind blobstorage --access-tier hot 
``` 
 
## <a name="create-blob-storage-containers"></a>Erstellen von Blob Storage-Containern
 
Die App verwendet zwei Container im Blob Storage-Konto. Container ähneln Ordnern und werden zum Speichern von Blobs verwendet. In den Container _images_ lädt die App Bilder mit voller Auflösung hoch. In einem späteren Teil der Serie lädt eine Azure-Funktions-App Miniaturansichten der Bilder mit geänderter Größe in den Container _thumbs_ hoch. 

Rufen Sie mit dem Befehl [az storage account keys list](/cli/azure/storage/account/keys#list) den Speicherkontoschlüssel ab. Sie verwenden diesen Schlüssel dann zum Erstellen von zwei Containern mit dem Befehl [az storage container create](/cli/azure/storage/container#create).  
 
In diesem Fall ist „`<blob_storage_account>`“ der Name des Blob Storage-Kontos, das Sie erstellt haben. Der öffentliche Zugriff des Containers _images_ ist auf `off` festgelegt, der öffentliche Zugriff des Containers _thumbs_ auf `container`. Die Einstellung `container` für den öffentlichen Zugriff ermöglicht die Anzeige der Miniaturansichten durch Personen, die die Webseite besuchen.
 
```azurecli-interactive 
blobStorageAccount=<blob_storage_account>

blobStorageAccountKey=$(az storage account keys list -g myResourceGroup \
-n $blobStorageAccount --query [0].value --output tsv) 

az storage container create -n images --account-name $blobStorageAccount \
--account-key $blobStorageAccountKey --public-access off 

az storage container create -n thumbs --account-name $blobStorageAccount \
--account-key $blobStorageAccountKey --public-access container

echo "Make a note of your blob storage account key..." 
echo $blobStorageAccountKey 
``` 

Notieren Sie sich den Namen und Schlüssel des Blob Storage-Kontos. Die Beispiel-App verwendet diese Einstellungen zum Herstellen der Verbindung mit dem Speicherkonto zum Hochladen von Bildern. 

## <a name="create-an-app-service-plan"></a>Wie erstelle ich einen Plan? 

Ein [App Service-Plan](../../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) gibt den Standort, die Größe und die Funktionen der Webserverfarm an, die Ihre App hostet. 

Erstellen Sie mit dem Befehl [az appservice plan create](/cli/azure/appservice/plan#create) einen App Service-Plan. 

Im folgenden Beispiel wird ein App Service-Plan namens `myAppServicePlan` mit dem Tarif **Free** erstellt: 

```azurecli-interactive 
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku FREE 
``` 

## <a name="create-a-web-app"></a>Erstellen einer Web-App 

Die Web-App bietet einen Hostingort für den Code der Beispiel-App, der aus dem GitHub-Beispielrepository bereitgestellt wird. Erstellen Sie eine [Web-App](../../app-service/app-service-web-overview.md) im App Service-Plan `myAppServicePlan` mit dem Befehl [az webapp create](/cli/azure/webapp#create).  
 
Ersetzen Sie im folgenden Befehl `<web_app>` durch einen eindeutigen Namen (gültige Zeichen sind `a-z`, `0-9` und `-`). Wenn `<web_app>` nicht eindeutig ist, wird die folgende Fehlermeldung angezeigt: _Eine Website mit dem Namen `<web_app>` ist bereits vorhanden._ Die Standard-URL der Web-App lautet `https://<web_app>.azurewebsites.net`.  

```azurecli-interactive 
az webapp create --name <web_app> --resource-group myResourceGroup --plan myAppServicePlan 
``` 

## <a name="deploy-the-sample-app-from-the-github-repository"></a>Bereitstellen der Beispiel-App aus dem GitHub-Repository 

App Service unterstützt verschiedene Möglichkeiten zum Bereitstellen von Inhalt für eine Web-App. In diesem Tutorial stellen Sie die Web-App aus einem öffentlichen GitHub-Beispielrepository bereit: [https://github.com/Azure-Samples/storage-blob-upload-from-webapp](https://github.com/Azure-Samples/storage-blob-upload-from-webapp). Konfigurieren Sie die GitHub-Bereitstellung für die Web-App mit dem Befehl [az webapp deployment source config](/cli/azure/webapp/deployment/source#config). Ersetzen Sie `<web_app>` durch den Namen der Web-App, die Sie im vorherigen Schritt erstellt haben.

Das Beispielprojekt enthält eine [ASP.NET MVC](https://www.asp.net/mvc)-App, die ein Bild akzeptiert, es in einem Speicherkonto speichert und Bilder aus einem Miniaturansichtencontainer anzeigt. Die Webanwendung verwendet die Namespaces [Microsoft.WindowsAzure.Storage](/dotnet/api/microsoft.windowsazure.storage?view=azure-dotnet), [Microsoft.WindowsAzure.Storage.Blob](/dotnet/api/microsoft.windowsazure.storage.blob?view=azure-dotnet) und [Microsoft.WindowsAzure.Storage.Auth](/dotnet/api/microsoft.windowsazure.storage.auth?view=azure-dotnet) aus der Azure Storage-Clientbibliothek für die Interaktion mit Azure Storage. 

```azurecli-interactive 
az webapp deployment source config --name <web_app> \
--resource-group myResourceGroup --branch master --manual-integration \
--repo-url https://github.com/Azure-Samples/storage-blob-upload-from-webapp
``` 

## <a name="configure-web-app-settings"></a>Konfigurieren von Einstellungen für Web-Apps 

Die Beispiel-Web-App verwendet die [Azure Storage-Clientbibliothek](/dotnet/api/overview/azure/storage?view=azure-dotnet) zum Anfordern von Zugriffstoken, die zum Hochladen von Bildern verwendet werden. Die vom Storage SDK verwendeten Anmeldeinformationen des Speicherkontos werden in den Anwendungseinstellungen für die Web-App festgelegt. Fügen Sie der bereitgestellte App Anwendungseinstellungen mit dem Befehl [az webapp config appsettings set](/cli/azure/webapp/config/appsettings#set) hinzu. 

Im folgenden Befehl ist `<blob_storage_account>` der Name des Blob Storage-Kontos, und `<blob_storage_key>` ist der zugehörige Schlüssel. Ersetzen Sie `<web_app>` durch den Namen der Web-App, die Sie im vorherigen Schritt erstellt haben.     

```azurecli-interactive 
az webapp config appsettings set --name <web_app> --resource-group myResourceGroup \
--settings AzureStorageConfig__AccountName=<blob_storage_account> \
AzureStorageConfig__ImageContainer=images  \
AzureStorageConfig__ThumbnailContainer=thumbs \
AzureStorageConfig__AccountKey=<blob_storage_key>  
``` 

Nachdem die Web-App bereitgestellt und konfiguriert wurde, können Sie die Funktionalität zum Hochladen von Bildern in der App testen.   

## <a name="upload-an-image"></a>Hochladen von Images 

Navigieren Sie zum Testen der Web-App zur URL Ihrer veröffentlichten App. Die Standard-URL der Web-App lautet `https://<web_app>.azurewebsites.net`. Wählen Sie den Bereich **Fotos hochladen** aus, um eine Datei auszuwählen und hochzuladen, oder legen Sie eine Datei mithilfe von Drag & Drop im Bereich ab. Das Bild wird nicht mehr angezeigt, wenn es erfolgreich hochgeladen wurde.

![ImageResizer-App](media/storage-upload-process-images/figure1.png)

Im Beispielcode wird der Task „`UploadFiletoStorage`“ in der Datei „`Storagehelper.cs`“ verwendet, um die Bilder in den Container „`images`“ im Speicherkonto mithilfe der Methode [UploadFromStreamAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob.uploadfromstreamasync?view=azure-dotnet) hochzuladen. Das folgende Codebeispiel enthält einen Task „`UploadFiletoStorage`“. 

```csharp
public static async Task<bool> UploadFileToStorage(Stream fileStream, string fileName, AzureStorageConfig _storageConfig)
{
    // Create storagecredentials object by reading the values from the configuration (appsettings.json)
    StorageCredentials storageCredentials = new StorageCredentials(_storageConfig.AccountName, _storageConfig.AccountKey);

    // Create cloudstorage account by passing the storagecredentials
    CloudStorageAccount storageAccount = new CloudStorageAccount(storageCredentials, true);

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get reference to the blob container by passing the name by reading the value from the configuration (appsettings.json)
    CloudBlobContainer container = blobClient.GetContainerReference(_storageConfig.ImageContainer);

    // Get the reference to the block blob from the container
    CloudBlockBlob blockBlob = container.GetBlockBlobReference(fileName);

    // Upload the file
    await blockBlob.UploadFromStreamAsync(fileStream);

    return await Task.FromResult(true);
}
```

Die folgenden Klassen und Methoden werden in den vorhergehenden Tasks verwendet:

|Klasse  |Methode  |
|---------|---------|
|[StorageCredentials](/dotnet/api/microsoft.windowsazure.storage.auth.storagecredentials?view=azure-dotnet)     |         |
|[CloudStorageAccount](/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount?view=azure-dotnet)    |  [CreateCloudBlobClient](/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount.createcloudblobclient?view=azure-dotnet#Microsoft_WindowsAzure_Storage_CloudStorageAccount_CreateCloudBlobClient)       |
|[CloudBlobClient](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobclient?view=azure-dotnet)     |[GetContainerReference](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobclient.getcontainerreference?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlobClient_GetContainerReference_System_String_)         |
|[CloudBlobContainer](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer?view=azure-dotnet)    | [GetBlockBlobReference](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.getblockblobreference?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlobContainer_GetBlockBlobReference_System_String_)        |
|[CloudBlockBlob](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob?view=azure-dotnet)     | [UploadFromStreamAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob.uploadfromstreamasync?view=azure-dotnet)        |

## <a name="verify-the-image-is-shown-in-the-storage-account"></a>Sicherstellen, dass das Bild im Speicherkonto angezeigt wird

Melden Sie sich bei https://portal.azure.com an. Wählen Sie im linken Menü **Speicherkonten** aus, und wählen Sie dann den Namen Ihres Speicherkontos aus. Wählen Sie unter **Übersicht** den Container **images** aus.

Stellen Sie sicher, dass das Bild im Container angezeigt wird.

![Bildcontaineransicht](media/storage-upload-process-images/figure13.png)

## <a name="test-thumbnail-viewing"></a>Testen der Miniaturansichtenanzeige

Um die Miniaturansichtenanzeige zu testen, laden Sie ein Bild in den Miniaturansichtencontainer hoch, um sicherzustellen, dass die Anwendung den Miniaturansichtencontainer lesen kann.

Melden Sie sich bei https://portal.azure.com an. Wählen Sie im linken Menü **Speicherkonten** aus, und wählen Sie dann den Namen Ihres Speicherkontos aus. Wählen Sie unter **Blob-Dienst** die Option **Container** aus, und wählen Sie dann den Container **thumbs** aus. Klicken Sie auf **Hochladen**, um den Bereich **Blob hochladen** zu öffnen.

Wählen Sie eine Datei mit der Dateiauswahl aus, und wählen Sie dann **Hochladen** aus.

Navigieren Sie zurück zu Ihrer App, um zu überprüfen, ob das in den Container **thumbs** hochgeladene Bild sichtbar ist.

![Bildcontaineransicht](media/storage-upload-process-images/figure2.png)

Wählen Sie im Azure-Portal im Container **thumbs** das Bild aus, das Sie hochgeladen haben, und wählen Sie dann **Löschen** aus, um dieses Bild zu löschen. Im zweiten Teil der Serie automatisieren Sie das Erstellen der Miniaturansichten. Daher wird dieses Testbild nicht mehr benötigt.

CDN kann zum Zwischenspeichern von Inhalt aus Ihrem Azure-Speicherkonto aktiviert werden. Weitere Informationen zum Aktivieren von CDN mit dem Azure-Speicherkonto (in diesem Tutorial nicht beschrieben) finden Sie hier: [Integrieren eines Azure-Speicherkontos in Azure CDN](../../cdn/cdn-create-a-storage-account-with-cdn.md).

## <a name="next-steps"></a>Nächste Schritte

In Teil eins der Serie haben Sie erfahren, wie eine Web-App konfiguriert wird, die mit dem Speicher interagiert, und Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen Sie ein Speicherkonto.
> * Erstellen eines Containers und Festlegen von Berechtigungen
> * Abrufen eines Zugriffsschlüssels
> * Konfigurieren von Anwendungseinstellungen
> * Bereitstellen einer Web-App in Azure
> * Interagieren mit der Webanwendung

Fahren Sie mit Teil zwei der Serie fort, um zu erfahren, wie Event Grid zum Auslösen einer Azure-Funktion zur Größenänderung eines Bilds verwendet wird.

> [!div class="nextstepaction"]
> [Verwenden von Event Grid zum Auslösen einer Azure-Funktion zum Ändern der Größe eines hochgeladenen Bilds](../../event-grid/resize-images-on-storage-blob-upload-event.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
