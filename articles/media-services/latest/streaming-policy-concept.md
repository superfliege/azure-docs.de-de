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
ms.date: 05/15/2019
ms.author: juliako
ms.openlocfilehash: 510899e44e4ea4a90e21473ee6af546744c2be2a
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/23/2019
ms.locfileid: "66120237"
---
# <a name="streaming-policies"></a>Streamingrichtlinien

In Azure Media Services v3 können Sie mithilfe von [Streamingrichtlinien](https://docs.microsoft.com/rest/api/media/streamingpolicies) Streamingprotokolle und Verschlüsselungsoptionen für Ihre [Streaminglocator](streaming-locators-concept.md) definieren. Media Services v3 bietet einige vordefinierte Streamingrichtlinien, die Sie direkt für Tests oder in der Produktion verwenden können. 

Die folgenden vordefinierten Streamingrichtlinien sind aktuell verfügbar:<br/>„Predefined_DownloadOnly“, „Predefined_ClearStreamingOnly“, „Predefined_DownloadAndClearStreaming“, „Predefined_ClearKey“, „Predefined_MultiDrmCencStreaming“ und „Predefined_MultiDrmStreaming“.

Wenn Sie besondere Anforderungen haben (wenn Sie z.B. verschiedene Protokolle angeben möchten oder einen benutzerdefinierten Schlüsselbereitstellungsdienst oder eine klare Audiospur verwenden müssen), können Sie eine benutzerdefinierte Streamingrichtlinie erstellen. 

 
> [!IMPORTANT]
> * Eigenschaften von **Streamingrichtlinien** vom Datetime-Typ liegen immer im UTC-Format vor.
> * Sie sollten eine begrenzte Sammlung von Richtlinien für Ihr Media Services-Konto erstellen und diese für Ihre Streaminglocators wiederverwenden, wenn dieselben Optionen benötigt werden. Weitere Informationen finden Sie unter [Kontingente und Einschränkungen](limits-quotas-constraints.md).

## <a name="decision-tree"></a>Entscheidungsstruktur

Die folgenden Entscheidungsstruktur hilft Ihnen bei der Auswahl einer vordefinierten Streamingrichtlinie für Ihr Szenario.

Klicken Sie auf Bild, um es in voller Größe anzeigen.  <br/>
<a href="./media/streaming-policy/large.png" target="_blank"><img src="./media/streaming-policy/small.png"></a> 

## <a name="examples"></a>Beispiele

### <a name="not-encrypted"></a>Nicht verschlüsselt

Wenn Sie Ihre Datei im Klartext (nicht verschlüsselt) streamen möchten, legen Sie die vordefinierte Richtlinie für unverschlüsseltes Streaming fest: auf „Predefined_ClearStreamingOnly“ (in .NET können Sie die Enumeration „PredefinedStreamingPolicy.ClearStreamingOnly“ verwenden).

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
