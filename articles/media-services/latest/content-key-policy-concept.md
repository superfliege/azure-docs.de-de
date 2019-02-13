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
ms.date: 02/03/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: d9e86c45d535862e0c3d02b3f331bc40ebb7f6c7
ms.sourcegitcommit: 947b331c4d03f79adcb45f74d275ac160c4a2e83
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2019
ms.locfileid: "55745120"
---
# <a name="content-key-policies"></a>Richtlinien für Inhaltsschlüssel

Mit Media Services können Sie Ihre zu übermittelnden Live- und On-Demand-Inhalte dynamisch mit Advanced Encryption Standard (AES-128) oder einem der drei wichtigsten DRM-Systeme verschlüsseln: Microsoft PlayReady, Google Widevine und Apple FairPlay. Media Services bietet auch einen Dienst für die Übermittlung von AES-Schlüsseln und DRM-Lizenzen (PlayReady, Widevine und FairPlay) an autorisierte Clients.

Um Verschlüsselungsoptionen für Ihren Datenstrom festzulegen, müssen Sie die [Inhaltsschlüsselrichtlinie](https://docs.microsoft.com/rest/api/media/contentkeypolicies) erstellen und sie mit Ihrem **Streaminglocator** verknüpfen. Die **Inhaltsschlüsselrichtlinie** konfiguriert, wie der Inhaltsschlüssel mithilfe der Schlüsselbereitstellungskomponente von Media Services an Endclients übermittelt wird. Sie können den Inhaltsschlüssel automatisch von Media Services generieren lassen. Normalerweise würde ein langlebiger Schlüssel verwendet und das Vorhandensein der Richtlinie mithilfe von „Get“ überprüft. Um den Schlüssel abzurufen, müssen Sie eine separate Aktionsmethode aufrufen, um geheime Schlüssel oder Anmeldeinformationen abzurufen, wie im folgenden Beispiel gezeigt.

**Richtlinien für Inhaltsschlüssel** sind aktualisierbar. Es empfiehlt sich beispielsweise, die Richtlinie zu aktualisieren, wenn Sie eine Schlüsselrotation ausführen müssen. Sie können auch den primären Verifizierungsschlüssel und die Liste der alternativen Verifizierungsschlüssel in der vorhandenen Richtlinie ändern. Es dauert bis zu 15 Minuten, die Schlüsselbereitstellungscaches zu aktualisieren und die aktualisierte Richtlinie zu übernehmen. 

> [!IMPORTANT]
> * Eigenschaften von **Inhaltssschlüsselrichtlinien** vom Datetime-Typ liegen immer im UTC-Format vor.
> * Sie sollten eine begrenzte Sammlung von Richtlinien für Ihr Media Services-Konto erstellen und diese für Ihre Streaminglocators wiederverwenden, wenn dieselben Optionen benötigt werden. 

## <a name="example"></a>Beispiel

Um den Schlüssel abzurufen, verwenden Sie **GetPolicyPropertiesWithSecretsAsync**, wie im folgenden Beispiel gezeigt.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#GetOrCreateContentKeyPolicy)]

## <a name="filtering-ordering-paging"></a>Filterung, Sortierung, Paging

Informationen finden Sie unter [Filterung, Sortierung, Paginierung von Media Services-Entitäten](entities-overview.md).

## <a name="next-steps"></a>Nächste Schritte

* [Verwenden der dynamischen AES-128-Verschlüsselung und des Schlüsselübermittlungsdiensts](protect-with-aes128.md)
* [Verwenden der dynamischen DRM-Verschlüsselung und des Lizenzbereitstellungsdiensts](protect-with-drm.md)
* [EncodeHTTPAndPublishAESEncrypted](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/tree/master/NETCore/EncodeHTTPAndPublishAESEncrypted)
