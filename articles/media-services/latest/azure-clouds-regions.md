---
title: Clouds und Regionen, in denen Azure Media Services v3 verfügbar ist | Microsoft-Dokumentation
description: In diesem Artikel werden Azure-Clouds und -Regionen behandelt, in denen Azure Media Services v3 verfügbar ist.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 01/11/2019
ms.author: juliako
ms.openlocfilehash: 8eb49010d89c3039f46e5c84cd305b7d0b5ca025
ms.sourcegitcommit: 70471c4febc7835e643207420e515b6436235d29
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/15/2019
ms.locfileid: "54306978"
---
# <a name="clouds-and-regions-in-which-azure-media-services-v3-exists"></a>Clouds und Regionen, in denen Azure Media Services v3 verfügbar ist

Azure Media Services v3 ist über ein Azure Resource Manager-Manifest im globalen Azure, in Azure Government, Azure Deutschland und Azure China 21Vianet verfügbar. Allerdings sind nicht alle Media Services-Features in allen Azure-Clouds verfügbar. In diesem Dokument wird die Verfügbarkeit der Hauptkomponenten von Media Services v3 beschrieben.

## <a name="feature-availability-in-azure-clouds"></a>Featureverfügbarkeit in Azure-Clouds

| Feature|Globale Azure-Regionen | Azure Government|Azure Deutschland|Azure China 21Vianet|
| --- | --- | --- | --- | --- |
| [Azure EventGrid](reacting-to-media-services-events.md) | Verfügbar | Nicht verfügbar | Nicht verfügbar | Nicht verfügbar |
| [VideoAnalyzerPreset](analyzing-video-audio-files-concept.md) |  Verfügbar | Nicht verfügbar | Nicht verfügbar | Nicht verfügbar |
| [AudioAnalyzerPreset](analyzing-video-audio-files-concept.md) |  Verfügbar | Nicht verfügbar | Nicht verfügbar | Nicht verfügbar |
| [StandardEncoderPreset](encoding-concept.md) | Verfügbar | Verfügbar | Verfügbar | Verfügbar |
| [LiveEvents](live-streaming-overview.md) | Verfügbar | Verfügbar | Verfügbar | Verfügbar |
| [StreamingEndpoints](streaming-endpoint-concept.md) | Verfügbar | Verfügbar | Verfügbar | Verfügbar |

## <a name="regions"></a>Regionen 

Wenn Sie den Parameter **location** festlegen müssen, müssen Sie den Regionscodenamen als **location**-Wert angeben. Sie können die folgende Zeile in der [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) ausführen, um den Codenamen der Region abzurufen, in der sich Ihr Konto befindet und an die Ihr Aufruf weitergeleitet werden soll:

```bash
az account list-locations
```

Nach dem Ausführen der oben gezeigten Zeile erhalten Sie eine Liste mit allen Azure-Regionen. Navigieren Sie zu der Azure-Region, die den *displayName* aufweist, nach dem Sie suchen, und verwenden ihren Wert *name* für den Parameter **location**.

Verwenden Sie für die Azure-Region „USA, Westen 2“ (unten angezeigt) beispielsweise „westus2“ als Wert für den Parameter **location**.

```json
   {
      "displayName": "West US 2",
      "id": "/subscriptions/00000000-23da-4fce-b59c-f6fb9513eeeb/locations/westus2",
      "latitude": "47.233",
      "longitude": "-119.852",
      "name": "westus2",
      "subscriptionId": null
    }
```

## <a name="next-steps"></a>Nächste Schritte

[Media Services v3 – Übersicht](media-services-overview.md)
