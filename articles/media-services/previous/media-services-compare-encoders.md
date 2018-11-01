---
title: Azure On-Demand Media Encoder – Vergleich | Microsoft-Dokumentation
description: In diesem Thema werden die Codierungsfunktionen von **Media Encoder Standard** und **Media Encoder Premium Workflow** verglichen.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: a79437c0-4832-423a-bca8-82632b2c47cc
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2018
ms.author: juliako;anilmur
ms.openlocfilehash: c08759f4682c6010c2338ff7aaf61cda92eb0484
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2018
ms.locfileid: "50232086"
---
# <a name="comparison-of-azure-on-demand-media-encoders"></a>Vergleich der Azure On-Demand Media Encoder

In diesem Thema werden die Codierungsfunktionen von **Media Encoder Standard** und **Media Encoder Premium Workflow** verglichen.

## <a name="video-and-audio-processing-capabilities"></a>Verarbeitungsfunktionen für Video und Audio

In der folgenden Tabelle werden die Funktionen von Media Encoder Standard (MES) und Media Encoder Premium Workflow (MEPW) verglichen. 

|Funktion|Media Encoder Standard|Media Encoder Premium Workflow|
|---|---|---|
|Anwenden bedingter Logik beim Codieren<br/>(z.B. wenn die Eingabe in HD erfolgt, dann in 5.1-Audio codieren)|Nein |JA|
|Untertitelung|Nein |[Ja](media-services-premium-workflow-encoder-formats.md#closed_captioning)|
|[Dolby® Professional Loudness Correction](http://www.dolby.com/us/en/technologies/dolby-professional-loudness-solutions.pdf)<br/> mit Dialogue Intelligence™|Nein |JA|
|Deinterlacing, Inverse Telecine|Basic|Broadcastqualität|
|Erkennen und Entfernen von schwarzen Rahmen <br/>(Pillarbox, Letterbox)|Nein |JA|
|Erstellen von Miniaturansichten|[Ja](media-services-dotnet-generate-thumbnail-with-mes.md)|[Ja](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4)|
|Schneiden/Kürzen und Zusammenfügen von Videos|[Ja](media-services-advanced-encoding-with-mes.md#trim_video)|JA|
|Überlagern von Audio oder Video|[Ja](media-services-advanced-encoding-with-mes.md#overlay)|[Ja](media-services-media-encoder-premium-workflow-multiplefilesinput.md#example-1--overlay-an-image-on-top-of-the-video)|
|Überlagern von Grafiken|Aus Bildquellen|Aus Bild- und Textquellen|
|Mehrere Audiosprachspuren|Eingeschränkt|[Ja](media-services-media-encoder-premium-workflow-multiplefilesinput.md#example-2--multiple-audio-language-encoding)|

## <a id="billing"></a>Von Encodern verwendetes Abrechnungsverfahren
| Medienprozessorname | Geltende Preise | Notizen |
| --- | --- | --- |
| **Media Encoder Standard** |ENCODER |Codierungsaufgaben werden basierend auf der Gesamtdauer in Minuten für die Ausgabe aller Mediendateien in Rechnung gestellt. Den entsprechenden Kostensatz finden Sie [hier][1] in der Spalte „ENCODER“. |
| **Media Encoder Premium Workflow** |PREMIUM ENCODER |Codierungsaufgaben werden basierend auf der Gesamtdauer in Minuten für die Ausgabe aller Mediendateien in Rechnung gestellt. Den entsprechenden Kostensatz finden Sie [hier][1] in der Spalte „PREMIUM ENCODER“. |

## <a name="input-containerfile-formats"></a>Eingabecontainer/Dateiformate
| Eingabecontainer/Dateiformate | Media Encoder Standard | Media Encoder Premium Workflow |
| --- | --- | --- |
| Adobe® Flash® F4V |JA |JA |
| MXF/SMPTE 377M |JA |JA |
| GXF |JA |JA |
| MPEG-2-Transportdatenstrom |JA |JA |
| MPEG-2-Programmdatenstrom |JA |JA |
| MPEG-4/MP4 |JA |JA |
| Windows Media/ASF |JA |JA |
| AVI (unkomprimiert, 8-Bit/10-Bit) |JA |JA |
| 3GPP/3GPP2 |JA |Nein  |
| Smooth Streaming-Dateiformat (PIFF 1.3) |JA |Nein  |
| [Microsoft Digital Video Recording (DVR-MS)](https://msdn.microsoft.com/library/windows/desktop/dd692984) |JA |Nein  |
| Matroska/WebM |JA |Nein  |
| QuickTime (.mov) |JA |Nein  |

## <a name="input-video-codecs"></a>Codecs für Videoeingang
| Codecs für Videoeingang | Media Encoder Standard | Media Encoder Premium Workflow |
| --- | --- | --- |
| AVC 8-Bit/10-Bit, bis zu 4:2:2, einschließlich AVCIntra |8-Bit 4:2:0 und 4:2:2 |JA |
| Avid DNxHD (in MXF) |JA |JA |
| DVCPro/DVCProHD (in MXF) |JA |JA |
| JPEG2000 |JA |JA |
| MPEG-2 (bis zu 422 Profile und High Level; Varianten wie XDCAM, XDCAM HD, XDCAM IMX, CableLabs® und D10 eingeschlossen) |Bis zu 422 Profile |JA |
| MPEG-1 |JA |JA |
| Windows Media Video/VC-1 |JA |JA |
| Canopus HQ/HQX |Nein  |Nein  |
| MPEG-4 Teil 2 |JA |Nein  |
| [Theora](https://en.wikipedia.org/wiki/Theora) |JA |Nein  |
| Apple ProRes 422 |JA |Nein  |
| Apple ProRes 422 LT |JA |Nein  |
| Apple ProRes 422 HQ |JA |Nein  |
| Apple ProRes Proxy |JA |Nein  |
| Apple ProRes 4444 |JA |Nein  |
| Apple ProRes 4444 XQ |JA |Nein  |
| HEVC/H.265|Profil: Main|Profile: Main und Main 10|

## <a name="input-audio-codecs"></a>Codecs für Audioeingang
| Codecs für Audioeingang | Media Encoder Standard | Media Encoder Premium Workflow |
| --- | --- | --- |
| AES (SMPTE 331M und 302M, AES3-2003) |Nein  |JA |
| Dolby® E |Nein  |JA |
| Dolby® Digital (AC3) |Nein  |JA |
| Dolby® Digital Plus (E-AC3) |Nein  |JA |
| AAC (AAC-LC, AAC-HE und AAC-HEv2; bis 5.1) |JA |JA |
| MPEG Layer 2 |JA |JA |
| MP3 (MPEG-1 Audio Layer 3) |JA |JA |
| Windows Media Audio |JA |JA |
| WAV/PCM |JA |JA |
| [FLAC](https://en.wikipedia.org/wiki/FLAC)</a> |JA |Nein  |
| [Opus](https://en.wikipedia.org/wiki/Opus_\(audio_format\)) |JA |Nein  |
| [Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a> |JA |Nein  |

## <a name="output-containerfile-formats"></a>Ausgabecontainer/Dateiformate
| Ausgabecontainer/Dateiformate | Media Encoder Standard | Media Encoder Premium Workflow |
| --- | --- | --- |
| Adobe® Flash® F4V |Nein  |JA |
| MXF (OP1a, XDCAM und AS02) |Nein  |JA |
| DPP (einschließlich AS11) |Nein  |JA |
| GXF |Nein  |JA |
| MPEG-4/MP4 |JA |JA |
| MPEG-TS |JA |JA |
| Windows Media/ASF |Nein  |JA |
| AVI (unkomprimiert, 8-Bit/10-Bit) |Nein  |JA |
| Smooth Streaming-Dateiformat (PIFF 1.3) |Nein  |JA |

## <a name="output-video-codecs"></a>Codecs für Videoausgang
| Codecs für Videoausgang | Media Encoder Standard | Media Encoder Premium Workflow |
| --- | --- | --- |
| AVC (H.264; 8-Bit; bis High Profile, Level 5.2; 4K Ultra HD; AVC Intra) |Nur 8-Bit 4:2:0 |JA |
| HEVC (H.265; 8 Bit und 10 Bit)  |Nein  |JA |
| Avid DNxHD (in MXF) |Nein  |JA |
| MPEG-2 (bis zu 422 Profile und High Level; Varianten wie XDCAM, XDCAM HD, XDCAM IMX, CableLabs® und D10 eingeschlossen) |Nein  |JA |
| MPEG-1 |Nein  |JA |
| Windows Media Video/VC-1 |Nein  |JA |
| Erstellung von JPEG-Miniaturansichten |JA |JA |
| Erstellung von PNG-Miniaturansichten |JA |JA |
| Erstellung von BMP-Miniaturansichten |JA |Nein  |

## <a name="output-audio-codecs"></a>Codecs für Audioausgang
| Codecs für Audioausgabe | Media Encoder Standard | Media Encoder Premium Workflow |
| --- | --- | --- |
| AES (SMPTE 331M und 302M, AES3-2003) |Nein  |JA |
| Dolby® Digital (AC3) |Nein  |JA |
| Dolby® Digital Plus (E-AC3) bis 7.1 |Nein  |JA |
| AAC (AAC-LC, AAC-HE und AAC-HEv2; bis 5.1) |JA |JA |
| MPEG Layer 2 |Nein  |JA |
| MP3 (MPEG-1 Audio Layer 3) |Nein  |JA |
| Windows Media Audio |Nein  |JA |

>[!NOTE]
>Wenn Sie in Dolby® Digital (AC3) codieren, kann die Ausgabe nur in eine ISO MP4-Datei geschrieben werden.

## <a name="media-services-learning-paths"></a>Media Services-Lernpfade
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a>Verwandte Artikel
* [Ausführen von Aufgaben für die erweiterte Codierung durch Anpassen der Media Encoder Standard-Voreinstellungen](media-services-custom-mes-presets-with-dotnet.md)
* [Kontingente und Einschränkungen](media-services-quotas-and-limitations.md)

<!--Reference links in article-->
[1]: http://azure.microsoft.com/pricing/details/media-services/
