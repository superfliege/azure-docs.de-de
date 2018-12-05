---
title: LiveEvent-Latenz in Azure Media Services | Microsoft-Dokumentation
description: Dieses Thema bietet eine Übersicht über die LiveEvent-Latenz und zeigt, wie Sie eine niedrige Latenz festlegen.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 11/16/2018
ms.author: juliako
ms.openlocfilehash: a74f2e53127b506f42ff49018c3df2985396646d
ms.sourcegitcommit: eba6841a8b8c3cb78c94afe703d4f83bf0dcab13
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/29/2018
ms.locfileid: "52619814"
---
# <a name="liveevent-latency-in-media-services"></a>LiveEvent-Latenz in Media Services

Dieser Artikel zeigt, wie Sie eine niedrige **LiveEvent**-Latenz festlegen. Außerdem werden typische Ergebnisse beschrieben, die bei der Verwendung der Einstellungen für geringe Latenz und verschiedenen Playern angezeigt werden. Die Ergebnisse variieren basierend auf dem CDN und der Netzwerklatenz. 

Um die neue Funktion **LowLatency** zu nutzen, legen Sie **StreamOptionsFlag** für **LiveEvent** auf **LowLatency** fest. Sobald der Stream bereit ist, können Sie auf der Demoseite des [Azure Media Players](http://ampdemo.azureedge.net/) (AMP) die Option für die Wiedergabe auf „Low Latency Heuristics Profile“ (Heuristische Profile mit geringer Latenz) festlegen.

Das folgende .NET-Beispiel zeigt, wie Sie **LowLatency** für **LiveEvent** festlegen:

```csharp
LiveEvent liveEvent = new LiveEvent(
            location: mediaService.Location, 
            description: "Sample LiveEvent for testing",
            vanityUrl: false,
            encoding: new LiveEventEncoding(
                        // Set this to Basic to enable a transcoding LiveEvent, and None to enable a pass-through LiveEvent
                        encodingType:LiveEventEncodingType.None, 
                        presetName:null
                    ),
            input: new LiveEventInput(LiveEventInputProtocol.RTMP,liveEventInputAccess), 
            preview: liveEventPreview,
            streamOptions: new List<StreamOptionsFlag?>()
            {
                // Set this to Default or Low Latency
                // To use low latency optimally, you should tune your encoder settings down to 1 second GOP size instead of 2 seconds.
                StreamOptionsFlag.LowLatency
            }
        );
```                

Das komplette Beispiel finden Sie unter [MediaV3LiveApp](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/Program.cs#L126).

## <a name="pass-through-liveevents-latency"></a>Latenz für LiveEvent-Typ „Passthrough“

Die folgende Tabelle zeigt übliche Latenzergebnisse (wenn das Flag „LowLatency“ aktiviert ist) in Media Services, gemessen ab dem Zeitpunkt, zu dem der Beitragsfeed den Dienst erreicht, bis zum Zeitpunkt, zu dem ein Player die Wiedergabe anfordern kann.

||Niedrige Latenz aktiviert (2 Sek. GOP)|Niedrige Latenz aktiviert (1 Sek. GOP)|
|---|---|---|
|DASH in AMP|10 Sek.|8 Sek.|
|HLS in nativem iOS-Player|14 Sek.|10 Sek.|

> [!NOTE]
> Die End-to-End-Latenz hängt vom lokalen Netzwerk ab und kann von einer CDN-Cachingebene beeinträchtigt werden. Testen Sie Ihre genauen Konfigurationen.

## <a name="next-steps"></a>Nächste Schritte

- [Übersicht über das Livestreaming](live-streaming-overview.md)
- [Tutorial zu Livestreaming](stream-live-tutorial-with-api.md)

