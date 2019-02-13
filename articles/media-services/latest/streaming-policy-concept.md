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
ms.date: 02/03/2019
ms.author: juliako
ms.openlocfilehash: 10600d8f3ff4e08b8d90f28ec15d3cb0c56bcae0
ms.sourcegitcommit: 947b331c4d03f79adcb45f74d275ac160c4a2e83
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2019
ms.locfileid: "55746743"
---
# <a name="streaming-policies"></a>Streamingrichtlinien

In Azure Media Services v3 können Sie mithilfe von [Streamingrichtlinien](https://docs.microsoft.com/rest/api/media/streamingpolicies) Streamingprotokolle und Verschlüsselungsoptionen für Ihre [Streaminglocator](streaming-locators-concept.md) definieren. Sie können entweder eine der vordefinierten Streamingrichtlinien verwenden oder eine benutzerdefinierte Richtlinie erstellen. Die folgenden vordefinierten Streamingrichtlinien sind aktuell verfügbar: „Predefined_DownloadOnly“, „Predefined_ClearStreamingOnly“, „Predefined_DownloadAndClearStreaming“, „Predefined_ClearKey“, „Predefined_MultiDrmCencStreaming“ und „Predefined_MultiDrmStreaming“.

> [!IMPORTANT]
> * Eigenschaften von **Streamingrichtlinien** vom Datetime-Typ liegen immer im UTC-Format vor.
> * Sie sollten eine begrenzte Sammlung von Richtlinien für Ihr Media Services-Konto erstellen und diese für Ihre Streaminglocators wiederverwenden, wenn dieselben Optionen benötigt werden. 

## <a name="examples"></a>Beispiele

### <a name="not-encrypted"></a>Nicht verschlüsselt

Wenn Sie Ihre Datei im Klartext (nicht verschlüsselt) streamen möchten, legen Sie die vordefinierte Richtlinie für unverschlüsseltes Streaming fest: auf ‚Predefined_ClearStreamingOnly‘ (in .NET können Sie ‚PredefinedStreamingPolicy.ClearStreamingOnly‘ verwenden).

```csharp
StreamingLocator locator = await client.StreamingLocators.CreateAsync(
    resourceGroup,
    accountName,
    locatorName,
    new StreamingLocator
    {
        AssetName = assetName,
        StreamingPolicyName = PredefinedStreamingPolicy.ClearStreamingOnly
    });
```

### <a name="encrypted"></a>Verschlüsselt 

Wenn Sie Ihren Inhalt mit Umschlag- und CENC-Verschlüsselung verschlüsseln müssen, legen Sie die Richtlinie auf ‚Predefined_MultiDrmCencStreaming‘ fest. Diese Richtlinie gibt an, dass zwei Inhaltsschlüssel (Umschlag und CENC) generiert und für den Locator festgelegt werden sollen. Daher werden die Umschlag-, PlayReady- und Widevine-Verschlüsselungen angewendet (der Schlüssel wird dem Client basierend auf den konfigurierten DRM-Lizenzen bereitgestellt).

```csharp
StreamingLocator locator = await client.StreamingLocators.CreateAsync(
    resourceGroup,
    accountName,
    locatorName,
    new StreamingLocator
    {
        AssetName = assetName,
        StreamingPolicyName = "Predefined_MultiDrmCencStreaming",
        DefaultContentKeyPolicyName = contentPolicyName
    });
```

Wenn Sie den Stream auch mit CBCS (FairPlay) verschlüsseln möchten, verwenden Sie ‚Predefined_MultiDrmStreaming‘.

## <a name="filtering-ordering-paging"></a>Filterung, Sortierung, Paging

Informationen finden Sie unter [Filterung, Sortierung, Paginierung von Media Services-Entitäten](entities-overview.md).

## <a name="next-steps"></a>Nächste Schritte

* [Streamen einer Datei](stream-files-dotnet-quickstart.md)
* [Verwenden der dynamischen AES-128-Verschlüsselung und des Schlüsselübermittlungsdiensts](protect-with-aes128.md)
* [Verwenden der dynamischen DRM-Verschlüsselung und des Lizenzbereitstellungsdiensts](protect-with-drm.md)
