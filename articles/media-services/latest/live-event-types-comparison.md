---
title: LiveEvent-Typen in Azure Media Services | Microsoft-Dokumentation
description: Dieser Artikel nimmt in einer detaillierten Tabelle einen Vergleich der LiveEvent-Typen vor.
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
ms.date: 01/28/2019
ms.author: juliako
ms.openlocfilehash: 9c8bff5a0a4f1599a3d23e0c7b07a1caca536a9b
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55153962"
---
# <a name="live-event-types-comparison"></a>Vergleich von Liveereignistypen

In Azure Media Services gibt es zwei Arten von [Liveereignissen](https://docs.microsoft.com/rest/api/media/liveevents): Livecodierung und Pass-Through. 

## <a name="types-comparison"></a>Typenvergleich 

In der folgenden Tabelle werden die Features der beiden Liveereignistypen verglichen.

| Feature | Liveereignis vom Typ „Pass-Through“ | Standardliveereignis |
| --- | --- | --- |
| Die Single-Bitrate-Eingabe wird in mehreren Bitraten in der Cloud codiert. |Nein  |JA |
| Maximale Videoauflösung für Beitragsfeeds |4K (4096 × 2160 bei 60 Frames/Sekunde) |1080p (1920 x 1088 bei 30 Frames/Sekunde)|
| Empfohlene maximale Anzahl von Ebenen für Beitragsfeeds|Bis zu 12|Eine Audioebene|
| Maximale Anzahl von Ebenen in der Ausgabe| Identisch mit der Eingabe|Bis zu 7|
| Maximale aggregierte Bandbreite von Beitragsfeeds|60 MBit/s|–|
| Maximale Bitrate für eine einzelne Ebene im Beitrag |20 MBit/s|20 MBit/s|
| Unterstützung von Audiotiteln mit mehreren Sprachen|JA|Nein |
| Unterstützte Codecs für Videoeingang |H.264/AVC und H.265/HEVC|H.264/AVC|
| Unterstützte Codecs für Videoausgang|Identisch mit der Eingabe|H.264/AVC|
| Unterstützte Videobittiefe, -eingang und -ausgang|Bis zu 10 Bit, einschließlich HDR 10/HLG|8 Bit|
| Unterstützte Codecs für Audioeingang|AAC-LC, HE-AAC v1, HE-AAC v2|AAC-LC, HE-AAC v1, HE-AAC v2|
| Unterstützte Codecs für Audioausgang|Identisch mit der Eingabe|AAC-LC|
| Maximale Videoauflösung des Ausgabevideos|Identisch mit der Eingabe|720p (30 Frames pro Sekunde)|
| Eingabeprotokolle|RTMP, fragmentiertes MP4 (Smooth Streaming)|RTMP, fragmentiertes MP4 (Smooth Streaming)|
| Preis|Informieren Sie sich auf der [Preisseite](https://azure.microsoft.com/pricing/details/media-services/) , und klicken Sie auf die Registerkarte „Live-Video“.|Informieren Sie sich auf der [Preisseite](https://azure.microsoft.com/pricing/details/media-services/) , und klicken Sie auf die Registerkarte „Live-Video“.|
| Maximale Laufzeit|24x365 lineares Livestreaming|Rund um die Uhr|
| Möglichkeit zum Übergeben von eingebetteten CEA-608/708-Untertiteldaten|JA|JA|
| Unterstützung für das Einfügen von Slates|Nein |Nein |
| Unterstützung für Werbeeinblendungen über API| Nein |Nein |
| Unterstützung für Werbeeinblendungen über SCTE-35 Inband|JA|JA|
| Möglichkeit zum Wiederherstellen nach kurzen Unterbrechungen im Beitrag|JA|Nein (Liveereignis beginnt Slating nach 6 Sekunden ohne Eingabedaten)|
| Unterstützung für nicht einheitliche Eingabe-GOPs|JA|Nein, Eingabe erfordert feste GOP-Dauer|
| Unterstützung für Eingaben mit variable Bildwiederholrate|JA|Nein, Eingabe muss eine feste Bildfrequenz aufweisen. Kleinere Abweichungen, beispielsweise bei Szenen mit viel Bewegung, werden toleriert. Der Beitragsfeed kann die Bildfrequenz jedoch nicht verringern (z.B. auf 15 Frames/Sekunde).|
| Automatische Abschaltung des Liveereignisses, wenn der Eingabefeed verloren geht|Nein |Nach 12 Stunden, wenn kein LiveOutput erfolgt|

## <a name="next-steps"></a>Nächste Schritte

[Übersicht über das Livestreaming](live-streaming-overview.md)
