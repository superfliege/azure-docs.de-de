---
title: Streaminglocators in Azure Media Services | Microsoft-Dokumentation
description: Dieser Artikel erläutert, was Streaminglocators sind und wie sie in Azure Media Services verwendet werden.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 10/22/2018
ms.author: juliako
ms.openlocfilehash: 56e9a0b4eec347d2367c38ab00c6c9d9dca60752
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2018
ms.locfileid: "49986895"
---
# <a name="streaming-locators"></a>Streaminglocators

Um Ihren Clients eine URL bereitzustellen, mit der sie codierte Video- oder Audiodateien wiedergeben können, müssen Sie einen [Streaminglocator](https://docs.microsoft.com/rest/api/media/streaminglocators) und die Streaming-URLs erstellen. Weitere Informationen finden Sie unter [Streamen von Dateien](stream-files-dotnet-quickstart.md).

## <a name="streaminglocator-definition"></a>Streaminglocator: Definition

Die folgende Tabelle enthält die Eigenschaften von Streaminglocators und die jeweiligen Definitionen.

|NAME|Typ|BESCHREIBUNG|
|---|---|---|
|id |Zeichenfolge|Vollqualifizierte Ressourcen-ID für die Ressource.|
|name   |Zeichenfolge|Der Name der Ressource.|
|properties.alternativeMediaId  |Zeichenfolge|Alternative Medien-ID dieses Streaminglocators.|
|properties.assetName   |Zeichenfolge|Ressourcenname.|
|properties.contentKeys |StreamingLocatorContentKey[]|Die von diesem Streaminglocator verwendeten Inhaltsschlüssel.|
|properties.created |Zeichenfolge|Die Erstellungszeit des Streaminglocators.|
|properties.defaultContentKeyPolicyName |Zeichenfolge|Der Name der Standardrichtlinie des Inhaltsschlüssels, die dieser Streaminglocator verwendet.|
|properties.endTime |Zeichenfolge|Die Endzeit des Streaminglocators.|
|properties.startTime   |Zeichenfolge|Die Startzeit des Streaminglocators.|
|properties.streamingLocatorId  |Zeichenfolge|Die ID des Streaminglocators.|
|properties.streamingPolicyName |Zeichenfolge|Der Name der Streamingrichtlinie, die dieser Streaminglocator verwendet. Geben Sie entweder den Namen der von Ihnen erstellten Streamingrichtlinie an, oder verwenden Sie eine vordefinierte Streamingrichtlinie. Die folgenden vordefinierten Streamingrichtlinien sind verfügbar: „Predefined_DownloadOnly“, „Predefined_ClearStreamingOnly“, „Predefined_DownloadAndClearStreaming“, „Predefined_ClearKey“, „Predefined_MultiDrmCencStreaming“ und „Predefined_MultiDrmStreaming“.|
|type   |Zeichenfolge|Der Typ der Ressource.|

Die vollständige Definition finden Sie unter [Streaminglocators](https://docs.microsoft.com/rest/api/media/streaminglocators).

## <a name="filtering-ordering-paging"></a>Filterung, Sortierung, Paging

Media Services unterstützt die folgenden OData-Abfrageoptionen für Streaminglocators: 

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

Die folgende Tabelle zeigt, wie diese Optionen auf die Eigenschaften des Streaminglocators angewendet werden können: 

|NAME|Filter|Reihenfolge|
|---|---|---|
|id |||
|name|Eq, ne, ge, le, gt, lt|Aufsteigend und absteigend|
|properties.alternativeMediaId  |||
|properties.assetName   |||
|properties.contentKeys |||
|properties.created |Eq, ne, ge, le, gt, lt|Aufsteigend und absteigend|
|properties.defaultContentKeyPolicyName |||
|properties.endTime |Eq, ne, ge, le, gt, lt|Aufsteigend und absteigend|
|properties.startTime   |||
|properties.streamingLocatorId  |||
|properties.streamingPolicyName |||
|type   |||

### <a name="pagination"></a>Paginierung

Die Paginierung wird für jede der vier aktivierten Sortierreihenfolgen unterstützt. Derzeit ist die Seitengröße 10.

> [!TIP]
> Verwenden Sie immer den Link „Weiter“ zum Enumerieren der Auflistung und keine bestimmte Seitengröße als Referenz.

Wenn eine Abfrageantwort viele Elemente enthält, gibt der Dienst eine „\@odata.nextLink“-Eigenschaft zurück, um die nächste Seite der Ergebnisse abzurufen. Auf diese Weise kann das gesamte Resultset paginiert werden. Sie können die Seitengröße nicht konfigurieren. 

Wenn beim Paging der Auflistung Streaminglocators erstellt oder gelöscht werden, werden die Änderungen in den zurückgegebenen Ergebnissen übernommen (sofern sich diese Änderungen in dem Teil der Auflistung befinden, der nicht heruntergeladen wurde.) 

Das folgende C#-Beispiel zeigt, wie alle Streaminglocators im Konto enumeriert werden.

```csharp
var firstPage = await MediaServicesArmClient.StreamingLocators.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.StreamingLocators.ListNextAsync(currentPage.NextPageLink);
}
```

REST-Beispiele finden Sie unter [Streaminglocators – Liste](https://docs.microsoft.com/rest/api/media/streaminglocators/list).

## <a name="next-steps"></a>Nächste Schritte

[Streamen einer Datei](stream-files-dotnet-quickstart.md)
