---
title: Verwalten des Ablaufs von Azure Blob Storage im Azure Content Delivery Network | Microsoft-Dokumentation
description: "Informationen zu den Optionen für das Festlegen der Gültigkeitsdauer von Blobs beim Azure CDN-Zwischenspeichern."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: ad4801e9-d09a-49bf-b35c-efdc4e6034e8
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 11/10/2017
ms.author: mazha
ms.openlocfilehash: 6f82ae396a17f903a522c716f73a5f7d2de660e7
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/21/2017
---
# <a name="manage-expiration-of-azure-blob-storage-in-azure-content-delivery-network"></a>Verwalten des Ablaufs von Azure Blob Storage im Azure Content Delivery Network
> [!div class="op_single_selector"]
> * [Azure-Webinhalt](cdn-manage-expiration-of-cloud-service-content.md)
> * [Azure Blob Storage](cdn-manage-expiration-of-blob-content.md)
> 
> 

Der [Blob Storage-Dienst](../storage/common/storage-introduction.md#blob-storage) in Azure Storage ist einer von verschiedenen Azure-basierten Ursprüngen, die in Azure Content Delivery Network (CDN) integriert wurden. Jeder öffentlich zugängliche Blobinhalt kann in Azure CDN zwischengespeichert werden, bis seine Gültigkeitsdauer abläuft. Die Gültigkeitsdauer wird durch den `Cache-Control`-Header in der HTTP-Antwort des Ursprungsservers bestimmt. In diesem Artikel werden verschiedene Methoden beschrieben, mit denen Sie den `Cache-Control`-Header für ein Blob in Azure Storage festlegen können.

Sie können die Cacheeinstellungen auch über das Portal durch Festlegen von [CDN-Cacheregeln](cdn-caching-rules.md) steuern. Wenn Sie Cacheregeln einrichten und ihr Zwischenspeicherverhalten auf **Außer Kraft setzen** oder **Cache umgehen** festlegen, werden die in diesem Artikel erörterten vom Ursprung angegebenen Cacheeinstellungen ignoriert. Weitere Informationen zu allgemeinen Cachekonzepten finden Sie unter [How caching works](cdn-how-caching-works.md) (Funktionsweise von Caching).

> [!TIP]
> Sie haben auch die Möglichkeit, keine Gültigkeitsdauer für ein Blob festzulegen. In diesem Fall wendet Azure CDN automatisch eine Standardgültigkeitsdauer von sieben Tagen an, es sei denn, Sie haben Cacheregeln im Azure-Portal eingerichtet. Diese Standardgültigkeitsdauer gilt nur für die Optimierung allgemeiner Webbereitstellungen. Bei der Optimierung großer Dateien beträgt die Standardgültigkeitsdauer einen Tag, bei der Optimierung für das Medienstreaming beträgt die Standardgültigkeitsdauer ein Jahr.
> 
> Weitere Informationen dazu, wie Azure CDN den Zugriff auf Blobs und andere Dateien beschleunigen kann, finden Sie in der [Übersicht über Azure Content Delivery Network](cdn-overview.md).
> 
> Weitere Informationen zu Azure Blob Storage finden Sie unter [Einführung in Blob Storage](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction).
 

## <a name="setting-cache-control-headers-by-using-azure-powershell"></a>Festlegen von Cache-Control-Headern unter Verwendung von Azure PowerShell
[Azure PowerShell](/powershell/azure/overview) ist eine der schnellsten und leistungsstärksten Möglichkeiten zum Verwalten Ihrer Azure-Dienste. Verwenden Sie das `Get-AzureStorageBlob`-Cmdlet zum Abrufen eines Verweises auf das Blob, und legen Sie dann die Eigenschaft `.ICloudBlob.Properties.CacheControl` fest. 

Beispiel:

```powershell
# Create a storage context
$context = New-AzureStorageContext -StorageAccountName "<storage account name>" -StorageAccountKey "<storage account key>"

# Get a reference to the blob
$blob = Get-AzureStorageBlob -Context $context -Container "<container name>" -Blob "<blob name>"

# Set the CacheControl property to expire in 1 hour (3600 seconds)
$blob.ICloudBlob.Properties.CacheControl = "max-age=3600"

# Send the update to the cloud
$blob.ICloudBlob.SetProperties()
```

> [!TIP]
> Sie können auch PowerShell zum [Verwalten Ihrer CDN-Profile und Endpunkte](cdn-manage-powershell.md) verwenden.
> 
>

## <a name="setting-cache-control-headers-by-using-net"></a>Festlegen von Cache-Control-Headern unter Verwendung von .NET
Um den `Cache-Control`-Header für ein Blob unter Verwendung von .NET-Code festzulegen, verwenden Sie die [Azure Storage-Clientbibliothek für .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) zum Festlegen der Eigenschaft [CloudBlob.Properties.CacheControl](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.blobproperties.cachecontrol.aspx).

Beispiel:

```csharp
class Program
{
    const string connectionString = "<storage connection string>";
    static void Main()
    {
        // Retrieve storage account information from connection string
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);

        // Create a blob client for interacting with the blob service.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        // Create a reference to the container
        CloudBlobContainer container = blobClient.GetContainerReference("<container name>");

        // Create a reference to the blob
        CloudBlob blob = container.GetBlobReference("<blob name>");

        // Set the CacheControl property to expire in 1 hour (3600 seconds)
        blob.Properties.CacheControl = "max-age=3600";

        // Update the blob's properties in the cloud
        blob.SetProperties();
    }
}
```

> [!TIP]
> Weitere .NET-Codebespiele finden Sie unter [Azure Blob Storage Samples for .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/)(Azure Blob Storage-Beispiele für .NET).
> 

## <a name="setting-cache-control-headers-by-using-other-methods"></a>Festlegen von Cache-Control-Headern mithilfe anderer Methoden

### <a name="azure-storage-explorer"></a>Azure-Speicher-Explorer
Mit dem [Azure Storage-Explorer](https://azure.microsoft.com/en-us/features/storage-explorer/) können Sie Ihre Blob Storage-Ressourcen anzeigen und bearbeiten, darunter beispielsweise Eigenschaften wie *CacheControl*. 

So aktualisieren Sie die *CacheControl*-Eigenschaft eines Blobs mit dem Azure Storage-Explorer:
   1. Wählen Sie ein Blob aus, und klicken Sie dann im Kontextmenü auf **Eigenschaften**. 
   2. Scrollen Sie nach unten zur *CacheControl*-Eigenschaft.
   3. Geben Sie einen Wert ein, und klicken Sie auf **Speichern**.


![Eigenschaften des Azure Storage-Explorers](./media/cdn-manage-expiration-of-blob-content/cdn-storage-explorer-properties.png)

### <a name="azure-command-line-interface"></a>Azure-Befehlszeilenschnittstelle
Mithilfe der [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/overview?view=azure-cli-latest) (CLI) können Sie Azure-Blobressourcen über die Befehlszeile verwalten. Legen Sie zum Festlegen des Cache-Control-Headers beim Hochladen eines Blobs mit der Azure-Befehlszeilenschnittstelle die Eigenschaft *cacheControl* mit dem `-p`-Switch fest. Im folgenden Beispiel wird gezeigt, wie die Gültigkeitsdauer auf eine Stunde (3.600 Sekunden) festgelegt wird:
  
```azurecli
azure storage blob upload -c <connectionstring> -p cacheControl="max-age=3600" .\test.txt myContainer test.txt
```

### <a name="azure-storage-services-rest-api"></a>REST-API für Azure-Speicherdienste
Sie können die [REST-API für Azure-Speicherdienste](https://msdn.microsoft.com/library/azure/dd179355.aspx) verwenden, um die Eigenschaft *x-ms-blob-cache-control* mithilfe der folgenden Vorgänge für eine Anforderung explizit festzulegen:
  
   - [Put Blob](https://msdn.microsoft.com/en-us/library/azure/dd179451.aspx)
   - [Put Block List](https://msdn.microsoft.com/en-us/library/azure/dd179467.aspx)
   - [Set Blob Properties](https://msdn.microsoft.com/library/azure/ee691966.aspx)

## <a name="testing-the-cache-control-header"></a>Testen des Cache-Control-Headers
Sie können die Einstellungen der Gültigkeitsdauer Ihrer Blobs einfach überprüfen. Mithilfe der [Entwicklertools](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/) Ihres Browsers können Sie überprüfen, ob Ihr Blob den `Cache-Control`-Antwortheader enthält. Sie können die Antwortheader außerdem über ein Tool wie [wget](https://www.gnu.org/software/wget/), [Postman](https://www.getpostman.com/) oder [Fiddler](http://www.telerik.com/fiddler) untersuchen.

## <a name="next-steps"></a>Nächste Schritte
* [Erfahren Sie, wie Sie den Ablauf von Clouddienstinhalten in Azure CDN verwalten können.](cdn-manage-expiration-of-cloud-service-content.md)
* [Informationen zu Cachekonzepten](cdn-how-caching-works.md)

