---
title: Medienobjekte in Media Services – Azure| Microsoft-Dokumentation
description: In diesem Artikel wird erläutert, was Medienobjekte sind und wie sie von Azure Media Services verwendet werden.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 01/01/2018
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 8507d51f0d4d49d89fc24b38ed73df7488261daa
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/02/2019
ms.locfileid: "53969574"
---
# <a name="assets"></a>Objekte

In Azure Media Services enthält ein [Medienobjekt](https://docs.microsoft.com/rest/api/media/assets) digitale Dateien (z.B. Video- und Audiodateien, Bilder, Sammlungen von Miniaturansichten, Textspuren und Untertiteldateien) sowie die Metadaten zu diesen Dateien. Nachdem die digitalen Dateien in ein Medienobjekt geladen wurden, können sie in den Media Services-Workflows zum Codieren, Streamen und Analysieren von Inhalten verwendet werden. Weitere Informationen finden Sie unten im Abschnitt [Hochladen von digitalen Dateien in Medienobjekte](#upload-digital-files-into-assets).

Ein Medienobjekt ist einem Blobcontainer im [Azure Storage-Konto](storage-account-concept.md) zugeordnet, und die im Medienobjekt enthaltenen Dateien werden als Blockblobs in diesem Container gespeichert. Azure Media Services unterstützt Blobebenen, wenn das Konto einen Speicher vom Typ „Allgemein v2“ (GPv2) verwendet. In GPv2-Speichern können Sie Dateien auf die [kalte Speicherebene oder die Archivspeicherebene](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers) verschieben. Der **Archivspeicher** eignet sich für die Archivierung von Quelldateien, wenn diese nicht mehr benötigt werden (z.B. nach der Codierung).

Die **Archivspeicherebene** wird nur empfohlen, wenn Sie über sehr große Quelldateien verfügen, die bereits codiert wurden, und wenn der Codierungsauftrag in einem Ausgabeblobcontainer platziert wurde. Die Blobs in dem Ausgabecontainer, den Sie einem Medienobjekt zuordnen und zum Streamen oder Analysieren Ihrer Inhalte verwenden möchten, müssen auf den Speicherebenen **heiß** oder **kalt** vorhanden sein.

## <a name="asset-definition"></a>Medienobjektdefinition

Die folgende Tabelle enthält die Eigenschaften eines Medienobjekts und die jeweiligen Definitionen.

|NAME|BESCHREIBUNG|
|---|---|
|id|Vollqualifizierte Ressourcen-ID für die Ressource.|
|name|Der Name der Ressource.|
|properties.alternateId |Die alternative ID des Medienobjekts.|
|properties.assetId |Die Medienobjekt-ID.|
|properties.container |Der Name des Medienobjekt-Blobcontainers.|
|properties.created |Das Erstellungsdatum des Medienobjekts.<br/> „datetime“ weist immer das UTC-Format auf.|
|properties.description|Die Medienobjektbeschreibung.|
|properties.lastModified |Das Datum der letzten Änderung des Medienobjekts. <br/> „datetime“ weist immer das UTC-Format auf.|
|properties.storageAccountName |Der Name des Speicherkontos.|
|properties.storageEncryptionFormat |Das Verschlüsselungsformat des Medienobjekts. None oder MediaStorageEncryption.|
|type|Der Typ der Ressource.|

Eine vollständige Definition finden Sie unter [Medienobjekte](https://docs.microsoft.com/rest/api/media/assets).

## <a name="upload-digital-files-into-assets"></a>Hochladen von digitalen Dateien in Medienobjekte

Einer der gängigsten Media Services-Workflows besteht aus dem Hochladen, Codieren und Streamen einer Datei. Dieser Abschnitt erläutert die allgemeinen Schritte.

1. Verwenden Sie die Media Services v3-API, um ein neues Medienobjekt für die Eingabe zu erstellen. Dieser Vorgang erstellt einen Container in dem Speicherkonto, das Ihrem Media Services-Konto zugeordnet ist. Die API gibt den Containernamen zurück (z.B. `"container": "asset-b8d8b68a-2d7f-4d8c-81bb-8c7bbbe67ee4"`).
   
    Wenn Sie bereits über einen Blobcontainer verfügen, den Sie einem Medienobjekt zuordnen möchten, können Sie den Containernamen beim Erstellen des Medienobjekts angeben. Media Services unterstützt derzeit nur Blobs im Containerstamm, nicht mit Pfaden im Dateinamen. Daher funktioniert ein Container mit dem Dateinamen „input.mp4“. Ein Container mit dem Dateinamen „videos/inputs/input.mp4“ dagegen funktioniert nicht.

    Sie können die Azure CLI verwenden, um Dateien direkt in ein Speicherkonto und einen Container hochzuladen, für das bzw. den Sie in Ihrem Abonnement Berechtigungen besitzen. <br/>Der Containername muss eindeutig sein und Benennungsrichtlinien für Speicher befolgen. Der Name muss nicht der Formatierung für Containernamen für Media Services-Medienobjekte folgen (Asset-GUID). 
    
    ```azurecli
    az storage blob upload -f /path/to/file -c MyContainer -n MyBlob
    ```
2. Rufen Sie eine SAS-URL mit Lese-/Schreibberechtigungen ab, die verwendet wird, um digitale Dateien in den Medienobjektcontainer hochzuladen. Sie können die Media Services-API verwenden, um [die Medienobjektcontainer-URLs aufzulisten](https://docs.microsoft.com/rest/api/media/assets/listcontainersas).
3. Verwenden Sie die Azure Storage-APIs oder SDKs (z.B. [Storage-REST-API](../../storage/common/storage-rest-api-auth.md), [Java SDK](../../storage/blobs/storage-quickstart-blobs-java-v10.md) oder [.NET SDK](../../storage/blobs/storage-quickstart-blobs-dotnet.md)), um Dateien in den Medienobjektcontainer hochzuladen. 
4. Verwenden Sie Media Services v3-APIs, um eine Transformation und einen Auftrag zur Verarbeitung Ihres Eingabemedienobjekts zu erstellen. Weitere Informationen finden Sie unter [Transformationen und Aufträge](transform-concept.md).
5. Streamen Sie die Inhalte des Ausgabemedienobjekts.

> [!TIP]
> Unter [Erstellen einer Auftragseingabe aus einer lokalen Datei](job-input-from-local-file-how-to.md) finden Sie ein vollständiges .NET-Beispiel für folgende Aufgaben: Erstellen des Medienobjekts, Abrufen einer beschreibbaren SAS-URL zum Container des Medienobjekts im Container und Hochladen der Datei in den Container im Speicher über die SAS-URL.

### <a name="create-a-new-asset"></a>Erstellen des neuen Medienobjekts

#### <a name="rest"></a>REST

```
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Media/mediaServices/{amsAccountName}/assets/{assetName}?api-version=2018-07-01
```

Ein REST-Beispiel finden Sie unter [Erstellen eines Medienobjekts mit REST](https://docs.microsoft.com/rest/api/media/assets/createorupdate#examples).

Dieses Beispiel zeigt, wie Sie den **Anforderungstext** erstellen, in dem Sie hilfreiche Informationen wie Beschreibung, Containername, Speicherkonto und weitere Informationen angeben können.

#### <a name="curl"></a>cURL

```cURL
curl -X PUT \
  'https://management.azure.com/subscriptions/00000000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Media/mediaServices/amsAccountName/assets/myOutputAsset?api-version=2018-07-01' \
  -H 'Accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "properties": {
    "description": "",
  }
}'
```

#### <a name="net"></a>.NET

```csharp
 Asset asset = await client.Assets.CreateOrUpdateAsync(resourceGroupName, accountName, assetName, new Asset());
```

Ein vollständiges Beispiel finden Sie unter [Erstellen einer Auftragseingabe aus einer lokalen Datei](job-input-from-local-file-how-to.md). In Media Services v3 kann die Eingabe eines Auftrags auch über HTTPS-URLs erstellt werden (siehe [Erstellen einer Auftragseingabe aus einer HTTPS-URL](job-input-from-http-how-to.md)).

## <a name="filtering-ordering-paging"></a>Filterung, Sortierung, Paging

Media Services unterstützt die folgenden OData-Abfrageoptionen für Medienobjekte: 

* $filter 
* $orderby 
* $top 
* $skiptoken 

Operatorbeschreibung:

* Eq = equal to (ist gleich)
* Ne = not equal to (ist nicht gleich)
* Ge = Greater than or equal to (ist größer als oder gleich)
* Le = Less than or equal to (ist kleiner als oder gleich)
* Gt = Greater than (größer als)
* Lt = Less than (kleiner als)

### <a name="filteringordering"></a>Filterung/Sortierung

Die folgende Tabelle zeigt, wie diese Optionen auf die Eigenschaften von Medienobjekten angewendet werden können: 

|NAME|Filter|Reihenfolge|
|---|---|---|
|id|||
|name|Unterstützt: Eq, Gt, Lt|Unterstützt: Aufsteigend und absteigend|
|properties.alternateId |Unterstützt: Eq||
|properties.assetId |Unterstützt: Eq||
|properties.container |||
|properties.created|Unterstützt: Eq, Gt, Lt| Unterstützt: Aufsteigend und absteigend|
|properties.description |||
|properties.lastModified |||
|properties.storageAccountName |||
|properties.storageEncryptionFormat | ||
|type|||

Im folgenden C#-Beispiel wird nach dem Erstellungsdatum gefiltert:

```csharp
var odataQuery = new ODataQuery<Asset>("properties/created lt 2018-05-11T17:39:08.387Z");
var firstPage = await MediaServicesArmClient.Assets.ListAsync(CustomerResourceGroup, CustomerAccountName, odataQuery);
```

### <a name="pagination"></a>Paginierung

Die Paginierung wird für jede der vier aktivierten Sortierreihenfolgen unterstützt. Derzeit ist die Seitengröße 1000.

> [!TIP]
> Sie sollten immer den Link „Weiter“ verwenden, um die Sammlung zu durchlaufen, und keine bestimmte Seitengröße als Referenz verwenden.

Wenn eine Abfrageantwort viele Elemente enthält, gibt der Dienst eine „\@odata.nextLink“-Eigenschaft zurück, um die nächste Seite der Ergebnisse abzurufen. Auf diese Weise kann das gesamte Resultset paginiert werden. Sie können die Seitengröße nicht konfigurieren. 

Wenn während der Paginierung der Sammlung Medienobjekte erstellt oder gelöscht werden, werden die Änderungen in den zurückgegebenen Ergebnissen reflektiert (sofern sich diese Änderungen in dem Teil der Sammlung befinden, der nicht heruntergeladen wurde). 

#### <a name="c-example"></a>C#-Beispiel

Im folgenden C#-Beispiel wird gezeigt, wie alle Medienobjekte im Konto durchlaufen werden.

```csharp
var firstPage = await MediaServicesArmClient.Assets.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.Assets.ListNextAsync(currentPage.NextPageLink);
}
```

#### <a name="rest-example"></a>Beispiel für REST

Betrachten Sie das folgende Beispiel für die Verwendung von „$skiptoken“. Stellen Sie sicher, dass Sie *amstestaccount* durch Ihren Kontonamen ersetzen und den Wert für *api-version* auf die neueste Version festlegen.

Fordern Sie folgendermaßen eine Liste von Medienobjekten an:

```
GET  https://management.azure.com/subscriptions/00000000-3761-485c-81bb-c50b291ce214/resourceGroups/mediaresources/providers/Microsoft.Media/mediaServices/amstestaccount/assets?api-version=2018-07-01 HTTP/1.1
x-ms-client-request-id: dd57fe5d-f3be-4724-8553-4ceb1dbe5aab
Content-Type: application/json; charset=utf-8
```

Sie erhalten eine Antwort, die etwa wie folgt aussieht:

```
HTTP/1.1 200 OK
 
{
"value":[
{
"name":"Asset 0","id":"/subscriptions/00000000-3761-485c-81bb-c50b291ce214/resourceGroups/mediaresources/providers/Microsoft.Media/mediaservices/amstestaccount/assets/Asset 0","type":"Microsoft.Media/mediaservices/assets","properties":{
"assetId":"00000000-5a4f-470a-9d81-6037d7c23eff","created":"2018-12-11T22:12:44.98Z","lastModified":"2018-12-11T22:15:48.003Z","container":"asset-98d07299-5a4f-470a-9d81-6037d7c23eff","storageAccountName":"amsdevc1stoaccount11","storageEncryptionFormat":"None"
}
},
// lots more assets
{
"name":"Asset 517","id":"/subscriptions/00000000-3761-485c-81bb-c50b291ce214/resourceGroups/mediaresources/providers/Microsoft.Media/mediaservices/amstestaccount/assets/Asset 517","type":"Microsoft.Media/mediaservices/assets","properties":{
"assetId":"00000000-912e-447b-a1ed-0f723913b20d","created":"2018-12-11T22:14:08.473Z","lastModified":"2018-12-11T22:19:29.657Z","container":"asset-fd05a503-912e-447b-a1ed-0f723913b20d","storageAccountName":"amsdevc1stoaccount11","storageEncryptionFormat":"None"
}
}
],"@odata.nextLink":"https:// management.azure.com/subscriptions/00000000-3761-485c-81bb-c50b291ce214/resourceGroups/mediaresources/providers/Microsoft.Media/mediaServices/amstestaccount/assets?api-version=2018-07-01&$skiptoken=Asset+517"
}
```

Danach fordern Sie die nächste Seite an, indem Sie eine get-Anforderung senden:

```
https://management.azure.com/subscriptions/00000000-3761-485c-81bb-c50b291ce214/resourceGroups/mediaresources/providers/Microsoft.Media/mediaServices/amstestaccount/assets?api-version=2018-07-01&$skiptoken=Asset+517
```

Weitere REST-Beispiele finden Sie unter [Medienobjekte – Liste](https://docs.microsoft.com/rest/api/media/assets/list).

## <a name="storage-side-encryption"></a>Speicherseitige Verschlüsselung

Zum Schutz Ihrer im Ruhezustand befindlichen Ressourcen sollten die Ressourcen durch die speicherseitige Verschlüsselung verschlüsselt werden. Die folgende Tabelle zeigt, wie die speicherseitige Verschlüsselung in Media Services funktioniert:

|Verschlüsselungsoption|BESCHREIBUNG|Media Services v2|Media Services v3|
|---|---|---|---|
|Media Services-Speicherverschlüsselung|AES-256-Verschlüsselung, Schlüssel von Media Services verwaltet|Unterstützt<sup>(1)</sup>|Nicht unterstützt<sup>(2)</sup>|
|[Speicherdienstverschlüsselung für ruhende Daten](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)|Durch Azure Storage angebotene serverseitige Verschlüsselung – Schlüssel wird von Azure oder vom Kunden verwaltet|Unterstützt|Unterstützt|
|[Clientseitige Storage-Verschlüsselung](https://docs.microsoft.com/azure/storage/common/storage-client-side-encryption)|Durch Azure Storage angebotene clientseitige Verschlüsselung – Schlüssel wird vom Kunden in Key Vault verwaltet|Nicht unterstützt|Nicht unterstützt|

<sup>1</sup> Media Services unterstützt zwar die Behandlung von Inhalten in Klartext/ohne jede Form der Verschlüsselung, doch wird davon abgeraten.

<sup>2</sup> In Media Services v3 wird Speicherverschlüsselung (AES-256-Verschlüsselung) nur für die Abwärtskompatibilität unterstützt, wenn Ihre Ressourcen mit Media Services v2 erstellt wurden. Dies bedeutet, dass v3 mit vorhandenen speicherverschlüsselten Ressourcen funktioniert, jedoch nicht die Erstellung neuer zulässt.

## <a name="next-steps"></a>Nächste Schritte

[Streamen einer Datei](stream-files-dotnet-quickstart.md)

[Unterschiede zwischen Media Services v2 und v3](migrate-from-v2-to-v3.md)
