---
title: Medienobjekte in Azure Media Services | Microsoft-Dokumentation
description: In diesem Artikel wird erläutert, was Medienobjekte sind und wie sie von Azure Media Services verwendet werden.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/19/2018
ms.author: juliako
ms.openlocfilehash: 76ed74f2df62d478b83e109a492977ec2d580198
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/18/2018
ms.locfileid: "34305088"
---
# <a name="assets"></a>Objekte

Ein **Medienobjekt** enthält digitale Dateien (z.B. Video, Audio, Bilder, Sammlungen von Miniaturansichten, Textspuren und Untertiteldateien) sowie die Metadaten zu diesen Dateien. Nachdem die digitalen Dateien in ein Medienobjekt geladen wurden, können Sie in den Codierungs- und Streaming-Workflows der Media Services verwendet werden.

Ein Medienobjekt ist einem Blobcontainer im [Azure Storage-Konto](storage-account-concept.md) zugeordnet, und die im Medienobjekt enthaltenen Dateien werden als Blockblobs in diesem Container gespeichert. Sie können über die Storage SDK-Clients mit den Medienobjektdateien in den Containern interagieren.

Azure Media Services unterstützt Blobebenen, wenn das Konto einen Speicher vom Typ „Allgemein v2“ (GPv2) verwendet. In GPv2-Speichern können Sie Dateien auf die kalte Speicherebene verschieben. Der Cold Storage eignet sich für die Archivierung von Quelldateien, wenn diese nicht mehr benötigt werden (z.B. nach der Codierung).

In Media Services v3 kann die Auftragseingabe von Medienobjekten oder HTTP- bzw. HTTPS- URLs erstellt werden. Informationen dazu, wie Sie ein Medienobjekt erstellen, das als Eingabe für Ihren Auftrag verwendet werden kann, finden Sie unter [Erstellen einer Auftragseingabe aus einer lokalen Datei](job-input-from-local-file-how-to.md).

Weitere Informationen finden Sie auch unter [Speicherkonten in Media Services](storage-account-concept.md) und [Transformationen und Aufträge](transform-concept.md).

## <a name="asset-definition"></a>Medienobjektdefinition

Die folgende Tabelle enthält die Eigenschaften eines Medienobjekts und die jeweiligen Definitionen.

|NAME|Typ|BESCHREIBUNG|
|---|---|---|
|id|Zeichenfolge|Vollqualifizierte Ressourcen-ID für die Ressource.|
|name|Zeichenfolge|Der Name der Ressource.|
|properties.alternateId |Zeichenfolge|Die alternative ID des Medienobjekts.|
|properties.assetId |Zeichenfolge|Die Medienobjekt-ID.|
|properties.container |Zeichenfolge|Der Name des Medienobjekt-Blobcontainers.|
|properties.created |Zeichenfolge|Das Erstellungsdatum des Medienobjekts.|
|properties.description |Zeichenfolge|Die Medienobjektbeschreibung.|
|properties.lastModified |Zeichenfolge|Das Datum der letzten Änderung des Medienobjekts.|
|properties.storageAccountName |Zeichenfolge|Der Name des Speicherkontos.|
|properties.storageEncryptionFormat |AssetStorageEncryptionFormat |Das Verschlüsselungsformat des Medienobjekts. None oder MediaStorageEncryption.|
|type|Zeichenfolge|Der Typ der Ressource.|

Die vollständige Definition finden Sie unter [Medienobjekte](https://docs.microsoft.com/rest/api/media/assets).

## <a name="filtering-ordering-and-paging-support"></a>Unterstützung für Filter-, Sortier- und Paginierungsfunktionen

Media Services unterstützt die folgenden OData-Abfrageoptionen für Medienobjekte: 

* $filter 
* $orderby 
* $top 
* $skiptoken 

### <a name="filteringordering"></a>Filterung/Sortierung

Die folgende Tabelle zeigt, wie diese Optionen auf die Eigenschaften von Medienobjekten angewendet werden können: 

|NAME|Filter|Reihenfolge|
|---|---|---|
|id|Unterstützt:<br/>Equals<br/>Größer als<br/>Kleiner als|Unterstützt:<br/>Aufsteigend<br/>Absteigend|
|name|||
|properties.alternateId |Unterstützt:<br/>Equals||
|properties.assetId |Unterstützt:<br/>Equals||
|properties.container |||
|properties.created|Unterstützt:<br/>Equals<br/>Größer als<br/>Kleiner als|Unterstützt:<br/>Aufsteigend<br/>Absteigend|
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

Die Paginierung wird für jede der vier aktivierten Sortierreihenfolgen unterstützt. 

Wenn eine Abfrageantwort viele (derzeit über 1000) Elemente enthält, gibt der Dienst eine „@odata.nextLink“-Eigenschaft zurück, um die nächste Seite der Ergebnisse abzurufen. Auf diese Weise kann das gesamte Resultset paginiert werden. Die Seitengröße ist nicht vom Benutzer konfigurierbar. 

Wenn während der Paginierung der Sammlung Medienobjekte erstellt oder gelöscht werden, werden die Änderungen in den zurückgegebenen Ergebnissen übernommen (sofern sich diese Änderungen in dem Teil der Sammlung befinden, der nicht heruntergeladen wurde.) 

Im folgenden C#-Beispiel wird gezeigt, wie alle Medienobjekte im Konto durchlaufen werden.

```csharp
var firstPage = await MediaServicesArmClient.Assets.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.Assets.ListNextAsync(currentPage.NextPageLink);
}
```

Beispiele zu REST finden Sie unter [Medienobjekte – Liste](https://docs.microsoft.com/rest/api/media/assets/list).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Streamen einer Datei](stream-files-dotnet-quickstart.md)
