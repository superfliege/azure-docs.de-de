---
title: Vergleich der Voreinstellungen von Video Indexer und Azure Media Services v3 | Microsoft-Dokumentation
description: In diesem Thema werden die Voreinstellungen von Video Indexer und Azure Media Services v3 verglichen.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2018
ms.author: juliako
ms.openlocfilehash: e9b66721135456320134f41c614b194d1e22b10f
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49380599"
---
# <a name="compare-azure-media-services-v3-presets-and-video-indexer"></a>Vergleich der Voreinstellungen von Azure Media Services v3 und Video Indexer 

In diesem Artikel werden die Funktionen von **Video Indexer-APIs** und **Media Services v3-APIs** verglichen. 

Derzeit gibt es eine Überschneidung bei den Features der [APIs von Video Indexer v2](https://api-portal.videoindexer.ai/) und der [APIs von Media Services v3](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01/Encoding.json). In der folgenden Tabelle finden Sie die aktuelle Richtlinie zum Verständnis der Unterschiede und Gemeinsamkeiten. 

## <a name="compare"></a>Vergleichen

|Feature|Video Indexer-APIs |Voreinstellungen für das Video- und Audioanalysetool<br/>in APIs von Azure Media Services v3|
|---|---|---|
|Medienerkenntnisse|[Erweitert](../../cognitive-services/video-indexer/video-indexer-output-json-v2.md?toc=/azure/media-services/video-indexer/toc.json&bc=/azure/media-services/video-indexer/breadcrumb/toc.json) |[Grundlagen](../latest/intelligence-concept.md)|
|Erfahrungen|Die vollständige Liste der unterstützten Features finden Sie hier: <br/> [Übersicht](../../cognitive-services/video-indexer/video-indexer-overview.md?toc=/azure/media-services/video-indexer/toc.json&bc=/azure/media-services/video-indexer/breadcrumb/toc.json)|Gibt nur Videoerkenntnisse zurück.|
|Abrechnung|[Media Services – Preise](https://azure.microsoft.com/pricing/details/media-services/#analytics)|[Media Services – Preise](https://azure.microsoft.com/pricing/details/media-services/#analytics)|
|Compliance|TBD|Media Services ist mit vielen Zertifizierungen konform. Lesen Sie [Azure Compliance Offerings.pdf](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942/file/178110/23/Microsoft%20Azure%20Compliance%20Offerings.pdf), und suchen Sie nach „Media Services“, um zu prüfen, ob es mit einem bestimmten Zertifikat konform ist.|
|Kostenlose Testversion|USA (Ost)|Nicht verfügbar|
|Verfügbarkeit |USA (West), Asien (Osten), Europa (Norden)|Siehe [Azure-Status](https://azure.microsoft.com/global-infrastructure/services/?products=media-services).|

## <a name="next-steps"></a>Nächste Schritte

[What is Video Indexer? (preview)](../../cognitive-services/video-indexer/video-indexer-overview.md?toc=/azure/media-services/video-indexer/toc.json&bc=/azure/media-services/video-indexer/breadcrumb/toc.json) (Was ist Video Indexer? (Vorschauversion))

[Media Services v3 – Übersicht](../../media-services/latest/media-services-overview.md)
