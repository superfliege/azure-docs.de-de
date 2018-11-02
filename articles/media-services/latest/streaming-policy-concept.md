---
title: Streamingrichtlinien in Azure Media Services | Microsoft-Dokumentation
description: In diesem Artikel wird erläutert, was Streamingrichtlinien sind und wie sie in Azure Media Services verwendet werden.
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
ms.openlocfilehash: c5f441fef95989e5c82586d96fc6c10e00a9627c
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50085494"
---
# <a name="streaming-policies"></a>Streamingrichtlinien

In Azure Media Services v3 können Sie mithilfe von Streamingrichtlinien Streamingprotokolle und Verschlüsselungsoptionen für Ihre StreamingLocators definieren. Sie können entweder den Namen der von Ihnen erstellten Streamingrichtlinie angeben oder eine der vordefinierten Streamingrichtlinien verwenden. Derzeit sind folgende vordefinierte Streamingrichtlinien verfügbar: „Predefined_DownloadOnly“, „Predefined_ClearStreamingOnly“, „Predefined_DownloadAndClearStreaming“, „Predefined_ClearKey“, „Predefined_MultiDrmCencStreaming“ und „Predefined_MultiDrmStreaming“.

> [!IMPORTANT]
> Wenn Sie eine benutzerdefinierte [Streamingrichtlinie](https://docs.microsoft.com/rest/api/media/streamingpolicies) verwenden, sollten Sie eine begrenzte Sammlung solcher Richtlinien für Ihr Media Services-Konto erstellen und diese für Ihre StreamingLocators wiederverwenden, wenn dieselben Verschlüsselungsoptionen und Protokolle benötigt werden. Ihr Media Services-Konto weist ein Kontingent für die Anzahl von StreamingPolicy-Einträgen auf. Sie sollten nicht für jeden StreamingLocator eine neue StreamingPolicy erstellen.

## <a name="streamingpolicy-definition"></a>StreamingPolicy-Definition

Die folgende Tabelle enthält die Eigenschaften von StreamingPolicy und die jeweiligen Definitionen.

|NAME|BESCHREIBUNG|
|---|---|
|id|Vollqualifizierte Ressourcen-ID für die Ressource.|
|name|Der Name der Ressource.|
|properties.commonEncryptionCbcs|Konfiguration von CommonEncryptionCbcs|
|properties.commonEncryptionCenc|Konfiguration von CommonEncryptionCenc|
|properties.created |Zeitpunkt der Erstellung der Streamingrichtlinie|
|properties.defaultContentKeyPolicyName |Standardmäßiger ContentKey, der von der aktuellen Streamingrichtlinie verwendet wird.|
|properties.envelopeEncryption  |Konfiguration von EnvelopeEncryption|
|properties.noEncryption|Konfiguration von NoEncryption|
|type|Der Typ der Ressource.|

Die vollständige Definition finden Sie unter [Streamingrichtlinien](https://docs.microsoft.com/rest/api/media/streamingpolicies).

## <a name="filtering-ordering-paging"></a>Filterung, Sortierung, Paging

Media Services unterstützt die folgenden OData-Abfrageoptionen für Streamingrichtlinien: 

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

Die folgende Tabelle zeigt, wie diese Optionen auf die StreamingPolicy-Eigenschaften angewendet werden können: 

|NAME|Filter|Reihenfolge|
|---|---|---|
|id|||
|name|Eq, ne, ge, le, gt, lt|Aufsteigend und absteigend|
|properties.commonEncryptionCbcs|||
|properties.commonEncryptionCenc|||
|properties.created |Eq, ne, ge, le, gt, lt|Aufsteigend und absteigend|
|properties.defaultContentKeyPolicyName |||
|properties.envelopeEncryption|||
|properties.noEncryption|||
|type|||

### <a name="pagination"></a>Paginierung

Die Paginierung wird für jede der vier aktivierten Sortierreihenfolgen unterstützt. Derzeit ist die Seitengröße 10.

> [!TIP]
> Sie sollten immer den Link „Weiter“ verwenden, um die Sammlung zu durchlaufen, und keine bestimmte Seitengröße als Referenz verwenden.

Wenn eine Abfrageantwort viele Elemente enthält, gibt der Dienst eine \@odata.nextLink-Eigenschaft zurück, um die nächste Seite der Ergebnisse abzurufen. Auf diese Weise kann das gesamte Resultset paginiert werden. Sie können die Seitengröße nicht konfigurieren. 

Wenn während des Durchlaufens der Sammlung StreamingPolicy-Objekte erstellt oder gelöscht werden, werden die Änderungen in den zurückgegebenen Ergebnissen übernommen (sofern sich diese Änderungen in dem Teil der Sammlung befinden, der nicht heruntergeladen wurde.) 

Im folgenden C#-Beispiel wird gezeigt, wie alle StreamingPolicies im Konto durchlaufen werden.

```csharp
var firstPage = await MediaServicesArmClient.StreamingPolicies.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.StreamingPolicies.ListNextAsync(currentPage.NextPageLink);
}
```

REST-Beispiele finden Sie unter [Streamingrichtlinien – Liste](https://docs.microsoft.com/rest/api/media/streamingpolicies/list).

## <a name="next-steps"></a>Nächste Schritte

[Streamen einer Datei](stream-files-dotnet-quickstart.md)
