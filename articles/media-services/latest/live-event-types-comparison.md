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
ms.date: 03/01/2019
ms.author: juliako
ms.openlocfilehash: 9952a7bbac1eb79de0d3425f839e3bd30196844e
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2019
ms.locfileid: "57243889"
---
# <a name="live-event-types-comparison"></a>Vergleich von Liveereignistypen

In Azure Media Services gibt es zwei Arten von [Liveereignissen](https://docs.microsoft.com/rest/api/media/liveevents): Livecodierung und Pass-Through. 

## <a name="types-comparison"></a>Typenvergleich 

In der folgenden Tabelle werden die Features der beiden Liveereignistypen verglichen.

| Feature | Liveereignis vom Typ „Pass-Through“ | Standardliveereignis |
| --- | --- | --- |
| Die Single-Bitrate-Eingabe wird in mehreren Bitraten in der Cloud codiert. |Nein  |Ja |
| Maximale Videoauflösung für Beitragsfeeds |4K (4096 × 2160 bei 60 Frames/Sekunde) |1080p (1920 x 1088 bei 30 Frames/Sekunde)|
| Empfohlene maximale Anzahl von Ebenen für Beitragsfeeds|Bis zu 12|Eine Audioebene|
| Maximale Anzahl von Ebenen in der Ausgabe| Identisch mit der Eingabe|Bis zu 6 (siehe unten „Systemvoreinstellungen“)|
| Maximale aggregierte Bandbreite von Beitragsfeeds|60 MBit/s|–|
| Maximale Bitrate für eine einzelne Ebene im Beitrag |20 MBit/s|20 MBit/s|
| Unterstützung von Audiotiteln mit mehreren Sprachen|Ja|Nein |
| Unterstützte Codecs für Videoeingang |H.264/AVC und H.265/HEVC|H.264/AVC|
| Unterstützte Codecs für Videoausgang|Identisch mit der Eingabe|H.264/AVC|
| Unterstützte Videobittiefe, -eingang und -ausgang|Bis zu 10 Bit, einschließlich HDR 10/HLG|8 Bit|
| Unterstützte Codecs für Audioeingang|AAC-LC, HE-AAC v1, HE-AAC v2|AAC-LC, HE-AAC v1, HE-AAC v2|
| Unterstützte Codecs für Audioausgang|Identisch mit der Eingabe|AAC-LC|
| Maximale Videoauflösung des Ausgabevideos|Identisch mit der Eingabe|720p (30 Frames pro Sekunde)|
| Eingabeprotokolle|RTMP, fragmentiertes MP4 (Smooth Streaming)|RTMP, fragmentiertes MP4 (Smooth Streaming)|
| Preis|Informieren Sie sich auf der [Preisseite](https://azure.microsoft.com/pricing/details/media-services/) , und klicken Sie auf die Registerkarte „Live-Video“.|Informieren Sie sich auf der [Preisseite](https://azure.microsoft.com/pricing/details/media-services/) , und klicken Sie auf die Registerkarte „Live-Video“.|
| Maximale Laufzeit| 24 Stunden × 365 Tage, live linear | Bis zu 24 Stunden|
| Möglichkeit zum Übergeben von eingebetteten CEA-608/708-Untertiteldaten|Ja|Ja|
| Unterstützung für das Einfügen von Slates|Nein |Nein |
| Unterstützung für Werbeeinblendungen über API| Nein |Nein |
| Unterstützung für Werbeeinblendungen über SCTE-35 Inband|Ja|Ja|
| Möglichkeit zum Wiederherstellen nach kurzen Unterbrechungen im Beitrag|Ja|Nein (Liveereignis beginnt Slating nach 6 Sekunden ohne Eingabedaten)|
| Unterstützung für nicht einheitliche Eingabe-GOPs|Ja|Nein, Eingabe erfordert feste GOP-Dauer|
| Unterstützung für Eingaben mit variable Bildwiederholrate|Ja|Nein, Eingabe muss eine feste Bildfrequenz aufweisen. Kleinere Abweichungen, beispielsweise bei Szenen mit viel Bewegung, werden toleriert. Der Beitragsfeed kann die Bildfrequenz jedoch nicht verringern (z.B. auf 15 Frames/Sekunde).|
| Automatische Abschaltung des Liveereignisses, wenn der Eingabefeed verloren geht|Nein |Nach 12 Stunden, wenn kein LiveOutput erfolgt|

## <a name="system-presets"></a>Systemvoreinstellungen

Bei Verwendung der Livecodierung (Einstellung **Standard** für Liveereignis) wird mit der Codierungsvoreinstellung definiert, wie der eingehende Datenstrom in mehrere Bitraten oder Ebenen codiert wird. Derzeit lautet der einzig zulässige Wert für die Voreinstellung *Default720p* (Standard).

Mit **Default720p** wird das Video in die folgenden 6 Ebenen codiert.

### <a name="output-video-stream"></a>Ausgabe-Videodatenstrom

| BitRate | Breite | Höhe | Max. Bilder/s | Profil | Name des Ausgabedatenstroms |
| --- | --- | --- | --- | --- | --- |
| 3500 |1280 |720 |30 |Hoch |Video_1280x720_3500kbps |
| 2200 |960 |540 |30 |Hoch |Video_960x540_2200kbps |
| 1350 |704 |396 |30 |Hoch |Video_704x396_1350kbps |
| 850 |512 |288 |30 |Hoch |Video_512x288_850kbps |
| 550 |384 |216 |30 |Hoch |Video_384x216_550kbps |
| 200 |340 |192 |30 |Hoch |Video_340x192_200kbps |

> [!NOTE]
> Falls Sie eine benutzerdefinierte Voreinstellung für die Livecodierung verwenden müssen, wenden Sie sich an amshelp@microsoft.com. Geben Sie hierbei die gewünschte Tabelle mit den Angaben zur Auflösung und zu den Bitraten an. Vergewissern Sie sich, dass nur eine Ebene mit 720p und maximal sechs Ebenen vorhanden sind.

### <a name="output-audio-stream"></a>Ausgabe-Audiodatenstrom

Audio wird mit einer Samplingrate von 48 kHz in Stereo-AAC-LC mit 128 KBit/s codiert.

## <a name="next-steps"></a>Nächste Schritte

[Übersicht über das Livestreaming](live-streaming-overview.md)
