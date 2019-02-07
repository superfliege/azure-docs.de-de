---
title: Hochladen von Bilddaten in die Cloud mit Azure Storage | Microsoft Docs
description: Verwenden von Azure Blob Storage mit einer Web-App zum Speichern von App-Daten
services: storage
author: tamram
ms.service: storage
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 11/26/2018
ms.author: tamram
ms.custom: mvc
ms.openlocfilehash: 7c32a572f1090783e5da53ae2b6103ac8c9a8b77
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/06/2019
ms.locfileid: "55752550"
---
# <a name="tutorial-upload-image-data-in-the-cloud-with-azure-storage"></a>Tutorial: Hochladen von Bilddaten in die Cloud mit Azure Storage

Dieses Tutorial ist der erste Teil einer Serie. In diesem Tutorial wird beschrieben, wie eine Web-App bereitgestellt wird, für die die Azure Storage-Clientbibliothek zum Hochladen von Bildern in ein Speicherkonto verwendet wird. Nachdem Sie das Tutorial durchgearbeitet haben, verfügen Sie über eine Web-App, mit der Bilder in Azure-Speicher abgelegt und daraus angezeigt werden.

# <a name="nettabdotnet"></a>[\.NET](#tab/dotnet)
![Bildcontaineransicht](media/storage-upload-process-images/figure2.png)

# <a name="nodejstabnodejs"></a>[Node.js](#tab/nodejs)
![Bildcontaineransicht](media/storage-upload-process-images/upload-app-nodejs-thumb.png)

---

Im ersten Teil der Serie lernen Sie Folgendes:

> [!div class="checklist"]
> * Speicherkonto erstellen
> * Erstellen eines Containers und Festlegen von Berechtigungen
> * Abrufen eines Zugriffsschlüssels
> * Bereitstellen einer Web-App in Azure
> * Konfigurieren von App-Einstellungen
> * Interagieren mit der Web-App

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen ein Azure-Abonnement, um dieses Tutorial durcharbeiten zu können. Erstellen Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio), bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Falls Sie die CLI lokal installieren und verwenden möchten, müssen Sie für dieses Tutorial die Azure CLI-Version 2.0.4 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI]( /cli/azure/install-azure-cli). 

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe 

Erstellen Sie mit dem Befehl [az group create](/cli/azure/group) eine Ressourcengruppe. Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden.  

Im folgenden Beispiel wird eine Ressourcengruppe namens `myResourceGroup` erstellt.

```azurecli-interactive
az group create --name myResourceGroup --location southeastasia 
```

## <a name="create-a-storage-account"></a>Speicherkonto erstellen

Das Beispiel lädt Bilder in einen Blob-Container in einem Azure Storage-Konto hoch. Ein Speicherkonto stellt einen eindeutigen Namespace zum Speichern Ihrer Azure Storage-Datenobjekte sowie für den Zugriff darauf bereit. Erstellen Sie ein Speicherkonto in der Ressourcengruppe, die Sie erstellt haben, indem Sie den Befehl [az storage account create](/cli/azure/storage/account) verwenden.

> [!IMPORTANT]
> In Teil 2 des Tutorials verwenden Sie Azure Event Grid mit Blob Storage. Erstellen Sie Ihr Speicherkonto in einer Azure-Region, die Event Grid unterstützt. Eine Liste mit den unterstützten Regionen finden Sie unter [Azure-Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=event-grid&regions=all).

Ersetzen Sie im folgenden Befehl den Platzhalter `<blob_storage_account>` durch Ihren eigenen global eindeutigen Namen für das Blob Storage-Konto.  

```azurecli-interactive
az storage account create --name <blob_storage_account> \
--location southeastasia --resource-group myResourceGroup \
--sku Standard_LRS --kind blobstorage --access-tier hot 
```

## <a name="create-blob-storage-containers"></a>Erstellen von Blob Storage-Containern

Die App verwendet zwei Container im Blob Storage-Konto. Container ähneln Ordnern und Speicherblobs. In den Container *images* lädt die App Bilder mit voller Auflösung hoch. In einem späteren Teil der Reihe lädt eine Azure-Funktionen-App Miniaturansichten der Bilder mit angepasster Größe in den Container *thumbnails* hoch.

Rufen Sie mit dem Befehl [az storage account keys list](/cli/azure/storage/account/keys) den Speicherkontoschlüssel ab. Verwenden Sie anschließend diesen Schlüssel, um mit dem Befehl [az storage container create](/cli/azure/storage/container) zwei Container zu erstellen.  

In diesem Fall ist „`<blob_storage_account>`“ der Name des Blob Storage-Kontos, das Sie erstellt haben. Der öffentliche Zugriff für den Container *images* ist auf `off` festgelegt. Der öffentliche Zugriff für den Container *thumbnails* ist auf `container` festgelegt. Bei der Einstellung `container` für den öffentlichen Zugriff können Benutzer, die die Webseite besuchen, die Miniaturansichten anzeigen.

```azurecli-interactive
blobStorageAccount=<blob_storage_account>

blobStorageAccountKey=$(az storage account keys list -g myResourceGroup \
-n $blobStorageAccount --query [0].value --output tsv)

az storage container create -n images --account-name $blobStorageAccount \
--account-key $blobStorageAccountKey --public-access off

az storage container create -n thumbnails --account-name $blobStorageAccount \
--account-key $blobStorageAccountKey --public-access container

echo "Make a note of your Blob storage account key..."
echo $blobStorageAccountKey
```

Notieren Sie sich den Namen und Schlüssel des Blob Storage-Kontos. Die Beispiel-App verwendet diese Einstellungen zum Herstellen der Verbindung mit dem Speicherkonto zum Hochladen der Bilder. 

## <a name="create-an-app-service-plan"></a>Wie erstelle ich einen Plan?

Ein [App Service-Plan](../../app-service/overview-hosting-plans.md) gibt den Standort, die Größe und die Funktionen der Webserverfarm an, die Ihre App hostet.

Erstellen Sie mit dem Befehl [az appservice plan create](/cli/azure/appservice/plan) einen App Service-Plan.

Im folgenden Beispiel wird ein App Service-Plan namens `myAppServicePlan` mit dem Tarif **Free** erstellt:

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku Free
```

## <a name="create-a-web-app"></a>Erstellen einer Web-App

Die Web-App umfasst einen Hostingort für den Code der Beispiel-App, der aus dem GitHub-Beispielrepository bereitgestellt wird. Erstellen Sie eine [Web-App](../../app-service/overview.md) im App Service-Plan `myAppServicePlan` mit dem Befehl [az webapp create](/cli/azure/webapp).  

Ersetzen Sie im folgenden Befehl `<web_app>` durch einen eindeutigen Namen. Gültige Zeichen sind `a-z`, `0-9` und `-`. Ist `<web_app>` nicht eindeutig, wird die folgende Fehlermeldung angezeigt: _Eine Website mit dem angegebenen Namen `<web_app>` ist bereits vorhanden._ Die Standard-URL der Web-App lautet `https://<web_app>.azurewebsites.net`.  

```azurecli-interactive
az webapp create --name <web_app> --resource-group myResourceGroup --plan myAppServicePlan
```

## <a name="deploy-the-sample-app-from-the-github-repository"></a>Bereitstellen der Beispiel-App aus dem GitHub-Repository

# <a name="nettabdotnet"></a>[\.NET](#tab/dotnet)

App Service unterstützt verschiedene Möglichkeiten zum Bereitstellen von Inhalt für eine Web-App. In diesem Tutorial stellen Sie die Web-App aus einem [öffentlichen GitHub-Beispielrepository](https://github.com/Azure-Samples/storage-blob-upload-from-webapp) bereit. Konfigurieren Sie die GitHub-Bereitstellung für die Web-App mit dem Befehl [az webapp deployment source config](/cli/azure/webapp/deployment/source). Ersetzen Sie `<web_app>` durch den Namen der Web-App, die Sie im vorherigen Schritt erstellt haben.

Das Beispielprojekt enthält eine [ASP.NET MVC](https://www.asp.net/mvc)-App. Die App akzeptiert ein Bild, speichert es unter einem Speicherkonto und zeigt Bilder aus einem Container mit Miniaturansichten an. Die Web-App verwendet die Namespaces [Microsoft.WindowsAzure.Storage](/dotnet/api/microsoft.windowsazure.storage?view=azure-dotnet), [Microsoft.WindowsAzure.Storage.Blob](/dotnet/api/microsoft.windowsazure.storage.blob?view=azure-dotnet) und [Microsoft.WindowsAzure.Storage.Auth](/dotnet/api/microsoft.windowsazure.storage.auth?view=azure-dotnet) aus der Azure Storage-Clientbibliothek für die Interaktion mit Azure Storage.

# <a name="nodejstabnodejs"></a>[Node.js](#tab/nodejs)
App Service unterstützt verschiedene Möglichkeiten zum Bereitstellen von Inhalt für eine Web-App. In diesem Tutorial stellen Sie die Web-App aus einem [öffentlichen GitHub-Beispielrepository](https://github.com/Azure-Samples/storage-blob-upload-from-webapp-node) bereit. Konfigurieren Sie die GitHub-Bereitstellung für die Web-App mit dem Befehl [az webapp deployment source config](/cli/azure/webapp/deployment/source). Ersetzen Sie `<web_app>` durch den Namen der Web-App, die Sie im vorherigen Schritt erstellt haben.

---

```azurecli-interactive
az webapp deployment source config --name <web_app> \
--resource-group myResourceGroup --branch master --manual-integration \
--repo-url https://github.com/Azure-Samples/storage-blob-upload-from-webapp
```

## <a name="configure-web-app-settings"></a>Konfigurieren von Einstellungen für Web-Apps

Die Beispiel-Web-App verwendet die [Azure Storage-Clientbibliothek](/dotnet/api/overview/azure/storage?view=azure-dotnet) zum Anfordern von Zugriffstoken, die zum Hochladen von Bildern verwendet werden. Die vom Storage SDK verwendeten Anmeldeinformationen des Speicherkontos werden in den App-Einstellungen für die Web-App festgelegt. Fügen Sie der bereitgestellten App mit dem Befehl [az webapp config appsettings set](/cli/azure/webapp/config/appsettings) App-Einstellungen hinzu.

Ersetzen Sie im folgenden Befehl `<blob_storage_account>` durch den Namen Ihres Blob Storage-Kontos und `<blob_storage_key>` durch den zugehörigen Schlüssel. Ersetzen Sie `<web_app>` durch den Namen der Web-App, die Sie im vorherigen Schritt erstellt haben.

```azurecli-interactive
az webapp config appsettings set --name <web_app> --resource-group myResourceGroup \
--settings AzureStorageConfig__AccountName=<blob_storage_account> \
AzureStorageConfig__ImageContainer=images  \
AzureStorageConfig__ThumbnailContainer=thumbnails \
AzureStorageConfig__AccountKey=<blob_storage_key>  
```

Nachdem Sie die Web-App bereitgestellt und konfiguriert haben, können Sie die Funktionalität zum Hochladen von Bildern in der App testen.

## <a name="upload-an-image"></a>Hochladen von Images

Navigieren Sie zum Testen der Web-App zur URL Ihrer veröffentlichten App. Die Standard-URL der Web-App lautet `https://<web_app>.azurewebsites.net`.
Wählen Sie den Bereich **Fotos hochladen** aus, um eine Datei auszuwählen und hochzuladen, oder ziehen Sie eine Datei in den Bereich. Das Bild wird nicht mehr angezeigt, wenn es erfolgreich hochgeladen wurde.

# <a name="nettabdotnet"></a>[\.NET](#tab/dotnet)

![ImageResizer-App](media/storage-upload-process-images/figure1.png)

Im Beispielcode wird der Task `UploadFiletoStorage` in der Datei *Storagehelper.cs* verwendet, um die Bilder mit der [UploadFromStreamAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob.uploadfromstreamasync?view=azure-dotnet)-Methode in den Container *images* des Speicherkontos hochzuladen. Das folgende Codebeispiel enthält einen Task „`UploadFiletoStorage`“.

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

# <a name="nodejstabnodejs"></a>[Node.js](#tab/nodejs)

![Bildupload-App](media/storage-upload-process-images/upload-app-nodejs.png)

Im Beispielcode ist die Route `post` für den Upload des Bilds in einen Blobcontainer verantwortlich Die Route verwendet die Module, die bei der Verarbeitung des Uploads helfen:

- Mit [multer](https://github.com/expressjs/multer) wird die Uploadstrategie für den Routenhandler implementiert.
- [into-stream](https://github.com/sindresorhus/into-stream) konvertiert den Puffer so in einen Stream, wie es für [createBlockBlobFromStream](http://azure.github.io/azure-sdk-for-node/azure-storage-legacy/latest/BlobService.html#createBlockBlobFromStream) erforderlich ist.

Wenn die Datei an die Route gesendet wird, wird ihr Inhalt im Arbeitsspeicher beibehalten, bis die Datei in den Blobcontainer hochgeladen wurde.

> [!IMPORTANT]
> Das Laden von großen Dateien in den Arbeitsspeicher kann die Leistung Ihrer Web-App beeinträchtigen. Wenn Sie erwarten, dass Benutzer große Dateien übermitteln, sollten Sie einen Stagingprozess für Dateien im Dateisystem des Webservers in Betracht ziehen und die Uploads im Blobspeicher planen. Sobald sich die Dateien im Blobspeicher befinden, können Sie sie aus dem Dateisystem des Servers entfernen.

```javascript
const
      express = require('express')
    , router = express.Router()

    , multer = require('multer')
    , inMemoryStorage = multer.memoryStorage()
    , uploadStrategy = multer({ storage: inMemoryStorage }).single('image')

    , azureStorage = require('azure-storage')
    , blobService = azureStorage.createBlobService()

    , getStream = require('into-stream')
    , containerName = 'images'
;

const handleError = (err, res) => {
    res.status(500);
    res.render('error', { error: err });
};

const getBlobName = originalName => {
    const identifier = Math.random().toString().replace(/0\./, ''); // remove "0." from start of string
    return `${originalName}-${identifier}`;
};

router.post('/', uploadStrategy, (req, res) => {

    const
          blobName = getBlobName(req.file.originalname)
        , stream = getStream(req.file.buffer)
        , streamLength = req.file.buffer.length
    ;

    blobService.createBlockBlobFromStream(containerName, blobName, stream, streamLength, err => {

        if(err) {
            handleError(err);
            return;
        }

        res.render('success', { 
            message: 'File uploaded to Azure Blob storage.' 
        });
    });
});
```
---

## <a name="verify-the-image-is-shown-in-the-storage-account"></a>Sicherstellen, dass das Bild im Speicherkonto angezeigt wird

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. Wählen Sie im linken Menü **Speicherkonten** aus, und wählen Sie dann den Namen Ihres Speicherkontos aus. Wählen Sie unter **Blob-Dienst** die Option **Blobs** und dann den Container **images**.

Stellen Sie sicher, dass das Bild im Container angezeigt wird.

![Bildcontaineransicht](media/storage-upload-process-images/figure13.png)

## <a name="test-thumbnail-viewing"></a>Testen der Miniaturansichtenanzeige

Zum Testen der Miniaturansichtenanzeige laden Sie ein Bild in den Container **thumbnails** hoch. Auf diese Weise soll überprüft werden, ob die App den Container **thumbnails** lesen kann.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. Wählen Sie im linken Menü **Speicherkonten** aus, und wählen Sie dann den Namen Ihres Speicherkontos aus. Wählen Sie unter **Blob-Dienst** die Option **Blobs** und dann den Container **thumbnails**. Klicken Sie auf **Hochladen**, um den Bereich **Blob hochladen** zu öffnen.

Wählen Sie mit der Dateiauswahl eine Datei aus, und wählen Sie dann die Option **Hochladen**.

Navigieren Sie wieder zu Ihrer App, um zu überprüfen, ob das in den Container **thumbnails** hochgeladene Bild sichtbar ist.

# <a name="nettabdotnet"></a>[\.NET](#tab/dotnet)
![Bildcontaineransicht](media/storage-upload-process-images/figure2.png)

# <a name="nodejstabnodejs"></a>[Node.js](#tab/nodejs)
![Bildcontaineransicht](media/storage-upload-process-images/upload-app-nodejs-thumb.png)

---

Im zweiten Teil der Reihe automatisieren Sie die Erstellung der Miniaturansicht, sodass Sie dieses Image nicht benötigen. Wählen Sie im Azure-Portal im Container **thumbnails** das Bild aus, das Sie hochgeladen haben, und klicken Sie anschließend auf **Löschen**, um das Bild zu löschen. 

Sie können CDN aktivieren, um Inhalt aus Ihrem Azure-Speicherkonto zwischenzuspeichern. Weitere Informationen dazu, wie Sie CDN mit Ihrem Azure-Speicherkonto aktivieren, finden Sie unter [Integrieren eines Azure-Speicherkontos in CDN](../../cdn/cdn-create-a-storage-account-with-cdn.md).

## <a name="next-steps"></a>Nächste Schritte

Im ersten Teil der Reihe haben Sie erfahren, wie Sie eine Web-App für die Interaktion mit Speicher konfigurieren.

Fahren Sie mit dem zweiten Teil der Reihe fort, um sich darüber zu informieren, wie Event Grid zum Auslösen einer Azure-Funktion zur Größenänderung eines Bilds verwendet wird.

> [!div class="nextstepaction"]
> [Verwenden von Event Grid zum Auslösen einer Azure-Funktion zum Ändern der Größe eines hochgeladenen Bilds](../../event-grid/resize-images-on-storage-blob-upload-event.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
