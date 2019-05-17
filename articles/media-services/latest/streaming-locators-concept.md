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
ms.date: 05/08/2019
ms.author: juliako
ms.openlocfilehash: 24ee700e326ef61aa6a93aae725e85e7b4780edf
ms.sourcegitcommit: e6d53649bfb37d01335b6bcfb9de88ac50af23bd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2019
ms.locfileid: "65465039"
---
# <a name="streaming-locators"></a>Streaminglocators

Um Videos im Ausgabeobjekt für die Wiedergabe durch Clients verfügbar zu machen, müssen Sie einen [Streaminglocator](https://docs.microsoft.com/rest/api/media/streaminglocators) erstellen und dann die Streaming-URLs erstellen. Informationen zum .NET-Beispiel finden Sie unter [Abrufen eines Streaminglocators](stream-files-tutorial-with-api.md#get-a-streaming-locator).

Der Vorgang zum Erstellen eines **Streaminglocators** wird als „Veröffentlichen“ bezeichnet. Standardmäßig ist der **Streaminglocator** sofort nach dem Vornehmen der API-Aufrufe gültig und bleibt es auch, bis er gelöscht wird (es sei denn, Sie konfigurieren die optionalen Start- und Endzeiten). 

Beim Erstellen eines **Streaminglocators** müssen Sie den [Objektnamen](https://docs.microsoft.com/rest/api/media/assets) und den Namen der [Streamingrichtlinie](https://docs.microsoft.com/rest/api/media/streamingpolicies) angeben. Sie können entweder eine der vordefinierten Streamingrichtlinien verwenden oder eine benutzerdefinierte Richtlinie erstellen. Die folgenden vordefinierten Richtlinien sind aktuell verfügbar: „Predefined_DownloadOnly“, „Predefined_ClearStreamingOnly“, „Predefined_DownloadAndClearStreaming“, „Predefined_ClearKey“, „Predefined_MultiDrmCencStreaming“ und „Predefined_MultiDrmStreaming“. Wenn Sie eine benutzerdefinierte Streamingrichtlinie verwenden, sollten Sie eine begrenzte Sammlung solcher Richtlinien für Ihr Media Services-Konto erstellen und diese für Ihre Streaminglocators wiederverwenden, wenn dieselben Optionen und Protokolle benötigt werden. 

Wenn Sie Verschlüsselungsoptionen für Ihren Datenstrom festlegen möchten, erstellen Sie die [Inhaltsschlüsselrichtlinie](https://docs.microsoft.com/rest/api/media/contentkeypolicies), die mithilfe der Komponente zur Schlüsselbereitstellung von Media Services konfiguriert, wie der Inhaltsschlüssel an Endclients übermittelt wird. Verknüpfen Sie Ihren Streaminglocator mit der **Inhaltsschlüsselrichtlinie** und dem Inhaltsschlüssel. Sie können den Schlüssel automatisch von Media Services generieren lassen. Das folgende .NET-Beispiel veranschaulicht die Konfiguration von AES-Verschlüsselung mit einer Tokeneinschränkung in Media Services v3: [EncodeHTTPAndPublishAESEncrypted](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/tree/master/NETCore/EncodeHTTPAndPublishAESEncrypted). **Inhaltsschlüsselrichtlinien** sind aktualisierbar, es empfiehlt sich, die Richtlinie zu aktualisieren, wenn Sie eine Schlüsselrotation ausführen müssen. Es dauert bis zu 15 Minuten, die Schlüsselbereitstellungscaches zu aktualisieren und die aktualisierte Richtlinie zu übernehmen. Es ist nicht empfehlenswert, für jeden Streaminglocator eine neue Inhaltsschlüsselrichtlinie zu erstellen. Sie sollten nach Möglichkeit die vorhandenen Richtlinien immer dann wiederverwenden, wenn die gleichen Optionen erforderlich sind.

> [!IMPORTANT]
> * Eigenschaften von **Streaminglocators** vom Datetime-Typ liegen immer im UTC-Format vor.
> * Sie sollten eine begrenzte Sammlung von Richtlinien für Ihr Media Services-Konto erstellen und diese für Ihre Streaminglocators wiederverwenden, wenn dieselben Optionen benötigt werden. 

## <a name="associate-filters-with-streaming-locators"></a>Zuordnen von Filtern mit Streaminglocators

Sie können eine Liste von [Medienobjekt- oder Kontenfiltern](filters-concept.md) angeben, die für Ihren [Streaminglocator](https://docs.microsoft.com/rest/api/media/streaminglocators/create#request-body) gelten würden. Der [dynamische Packager](dynamic-packaging-overview.md) wendet diese Liste der Filter zusammen mit den Filtern an, die Ihr Client in der URL angibt. Diese Kombination erzeugt ein [dynamisches Manifest](filters-dynamic-manifest-overview.md), das auf Filtern in den URL + Filtern basiert, die Sie im Streaminglocator angeben. Es wird empfohlen, dieses Feature zu verwenden, wenn Sie Filter anwenden, aber nicht die Filternamen in der URL verfügbar machen möchten.

## <a name="filter-order-page-streaming-locator-entities"></a>Filtern, Sortieren, Paginieren von Streaminglocatorentitäten

Informationen finden Sie unter [Filterung, Sortierung, Paginierung von Media Services-Entitäten](entities-overview.md).

## <a name="next-steps"></a>Nächste Schritte

* [Tutorial: Hochladen, Codieren und Streamen von Videos mithilfe von .NET](stream-files-tutorial-with-api.md)
* [Verwenden der dynamischen DRM-Verschlüsselung und des Lizenzbereitstellungsdiensts](protect-with-drm.md)
