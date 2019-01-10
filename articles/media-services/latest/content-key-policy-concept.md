---
title: 'Richtlinien für Inhaltsschlüssel in Media Services: Azure | Microsoft-Dokumentation'
description: In diesem Artikel wird erläutert, was Richtlinien für Inhaltsschlüssel sind und wie sie in Azure Media Services verwendet werden.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 12/20/2018
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: f12632b20d516c81e21a50cfdda7e40d4163afc1
ms.sourcegitcommit: 21466e845ceab74aff3ebfd541e020e0313e43d9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/21/2018
ms.locfileid: "53742217"
---
# <a name="content-key-policies"></a>Richtlinien für Inhaltsschlüssel

Mit Azure Media Services können Sie Ihre Medien ab dem Zeitpunkt, an dem sie Ihren Computer verlassen, während des gesamten Prozesses der Speicherung, Verarbeitung und Übermittlung sichern. Mit Media Services können Sie Ihre zu übermittelnden Live- und On-Demand-Inhalte dynamisch mit Advanced Encryption Standard (AES-128) oder einem der drei wichtigsten DRM-Systeme verschlüsseln: Microsoft PlayReady, Google Widevine und Apple FairPlay. Media Services bietet auch einen Dienst für die Übermittlung von AES-Schlüsseln und DRM-Lizenzen (PlayReady, Widevine und FairPlay) an autorisierte Clients.

In Azure Media Services v3 können Sie mithilfe einer [Richtlinie für Inhaltsschlüssel](https://docs.microsoft.com/rest/api/media/contentkeypolicies) angeben, wie der Inhaltsschlüssel an Endclients über die Media Services-Schlüsselbereitstellungskomponente übermittelt werden soll. Weitere Informationen finden Sie unter [Übersicht über den Inhaltsschutz](content-protection-overview.md).

Es wird empfohlen, die gleiche ContentKeyPolicy für alle Medienobjekte wiederzuverwenden. ContentKeyPolicies können aktualisiert werden, sodass Sie für eine Schlüsselrotation der vorhandenen ContentKeyPolicy eine neue ContentKeyPolicyOption mit einer Tokeneinschränkung auf die neuen Schlüssel hinzufügen können. Sie können aber auch den primären Verifizierungsschlüssel und die Liste der alternativen Verifizierungsschlüssel in der vorhandenen Richtlinie und der Option ändern. Es dauert bis zu 15 Minuten, die Schlüsselbereitstellungscaches zu aktualisieren und die aktualisierte Richtlinie zu übernehmen.

## <a name="contentkeypolicy-definition"></a>ContentKeyPolicy-Definition

Die folgende Tabelle enthält die Eigenschaften von ContentKeyPolicy und die jeweiligen Definitionen.

|NAME|BESCHREIBUNG|
|---|---|
|id|Vollqualifizierte Ressourcen-ID für die Ressource.|
|name|Der Name der Ressource.|
|properties.created |Das Erstellungsdatum der Richtlinie.|
|properties.description |Eine Beschreibung der Richtlinie.|
|properties.lastModified|Das Datum der letzten Änderung der Richtlinie.|
|properties.options |Die Optionen für die Schlüsselrichtlinie.|
|properties.policyId|Die ID der Legacyrichtlinie.|
|type|Der Typ der Ressource.|

Die vollständige Definition finden Sie unter [Richtlinien für Inhaltsschlüssel](https://docs.microsoft.com/rest/api/media/contentkeypolicies).

## <a name="filtering-ordering-paging"></a>Filterung, Sortierung, Paging

Media Services unterstützt die folgenden OData-Abfrageoptionen für ContentKeyPolicies: 

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

Die folgende Tabelle zeigt, wie diese Optionen auf die ContentKeyPolicies-Eigenschaften angewandt werden können: 

|NAME|Filter|Reihenfolge|
|---|---|---|
|id|||
|name|Eq, ne, ge, le, gt, lt|Aufsteigend und absteigend|
|properties.created |Eq, ne, ge, le, gt, lt|Aufsteigend und absteigend|
|properties.description |Eq, ne, ge, le, gt, lt||
|properties.lastModified|Eq, ne, ge, le, gt, lt|Aufsteigend und absteigend|
|properties.options |||
|properties.policyId|Eq, ne||
|type|||

### <a name="pagination"></a>Paginierung

Die Paginierung wird für jede der vier aktivierten Sortierreihenfolgen unterstützt. Derzeit ist die Seitengröße 10.

> [!TIP]
> Verwenden Sie immer den Link „Weiter“ zum Enumerieren der Auflistung und keine bestimmte Seitengröße als Referenz.

Wenn eine Abfrageantwort viele Elemente enthält, gibt der Dienst eine „\@odata.nextLink“-Eigenschaft zurück, um die nächste Seite der Ergebnisse abzurufen. Auf diese Weise kann das gesamte Resultset paginiert werden. Sie können die Seitengröße nicht konfigurieren. 

Wenn beim Paging der Sammlung ContentKeyPolicies erstellt oder gelöscht werden, werden die Änderungen in den zurückgegebenen Ergebnissen übernommen (sofern sich diese Änderungen in dem Teil der Sammlung befinden, der nicht heruntergeladen wurde.) 

Im folgenden C#-Beispiel wird gezeigt, wie alle ContentKeyPolicies im Konto durchlaufen werden.

```csharp
var firstPage = await MediaServicesArmClient.ContentKeyPolicies.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.ContentKeyPolicies.ListNextAsync(currentPage.NextPageLink);
}
```

REST-Beispiele finden Sie unter [Richtlinien für Inhaltsschlüssel – Liste](https://docs.microsoft.com/rest/api/media/contentkeypolicies/list).

## <a name="next-steps"></a>Nächste Schritte

[Verwenden der dynamischen AES-128-Verschlüsselung und des Schlüsselübermittlungsdiensts](protect-with-aes128.md)

[Verwenden der dynamischen DRM-Verschlüsselung und des Lizenzbereitstellungsdiensts](protect-with-drm.md)
