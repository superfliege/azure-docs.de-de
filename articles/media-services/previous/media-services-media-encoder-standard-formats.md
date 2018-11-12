---
title: Media Encoder Standard-Formate und -Codecs
description: Dieses Thema bietet eine Übersicht über Media Encoder Standard-Formate und -Codecs.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: f334b1ce-2f56-4968-a019-f0a2b0016d9f
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/30/2018
ms.author: juliako;anilmur
ms.openlocfilehash: 9c6e26b082fb52f7d23bb1a827af30483f7fbd4f
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51255709"
---
# <a name="media-encoder-standard-formats-and-codecs"></a>Media Encoder Standard-Formate und -Codecs
Dieses Dokument enthält eine Liste der gängigsten Import- und Exportdateiformate, die Sie mit Media Encoder Standard verwenden können.

## <a name="input-containerfile-formats"></a>Eingabecontainer/Dateiformate
| Dateiformate (Dateierweiterungen) | Unterstützt |
| --- | --- | --- | --- |
| FLV (mit H.264- und AAC-Codecs) (.flv) |JA |
| MXF (.mxf) |JA |
| GXF (.gxf) |JA |
| MPEG2-PS, MPEG2-TS, 3GP (.ts, .ps, .3gp, .3gpp, .mpg) |JA |
| Windows Media Video (WMV)/ASF (.wmv, .asf) |JA |
| AVI (unkomprimiert, 8-Bit/10-Bit) (.avi) |JA |
| MP4 (.mp4, .m4a, .m4v)/ISMV (.isma, .ismv) |JA |
| [Microsoft Digital Video Recording(DVR-MS)](https://msdn.microsoft.com/library/windows/desktop/dd692984) (.dvr-ms) |JA |
| Matroska/WebM (.mkv) |JA |
| WAVE/WAV (.wav) |JA |
| QuickTime (.mov) |JA |

> [!NOTE]
> In der obigen Liste sind nur die gängigeren Dateierweiterungen angegeben. Für Media Encoder Standard werden noch viele andere Erweiterungen unterstützt (z.B. .m2ts, .mpeg2video, .qt). Wenn Sie versuchen, eine Datei zu codieren, und eine Fehlermeldung zur fehlenden Unterstützung des Formats erhalten, können Sie uns [hier](https://feedback.azure.com/forums/169396-media-services/category/144411-encoding-and-processing/) Feedback zukommen lassen.
> 
> 

### <a name="audio-formats-in-input-containers"></a>Audioformate in Eingabecontainern
Media Encoder Standard unterstützt das Befördern der folgenden Audioformate in Eingabecontainern:

* MXF-, GXF- und QuickTime-Dateien, die Audiospuren mit verschachteltem Stereo- oder 5.1-Beispielen haben.

oder

* MXF-, GXF- und QuickTime-Dateien, in denen die Audiospuren als separate PCM-Tracks befördert, die Kanalzuordnung jedoch (für Stereo oder 5.1) aus den Dateimetadaten abgeleitet werden kann.

Der Support für die explizite oder vom Benutzer angegebene Kanalzuordnung wird in naher Zukunft bereitgestellt.

## <a name="input-video-codecs"></a>Codecs für Videoeingang
| Codecs für Videoeingang | Unterstützt |
| --- | --- | --- | --- |
| AVC 8-Bit/10-Bit, bis zu 4:2:2, einschließlich AVCIntra |8-Bit 4:2:0 und 4:2:2 |
| Avid DNxHD (in MXF) |JA |
| DVCPro/DVCProHD (in MXF) |JA |
| Digitales Video (DV) (in AVI-Dateien) |JA |
| JPEG 2000 |JA |
| MPEG-2 (bis zu 422 Profile und High Level; Varianten wie XDCAM, XDCAM HD, XDCAM IMX, CableLabs® und D10 eingeschlossen) |Bis zu 422 Profile |
| MPEG-1 |JA |
| VC-1/WMV9 |JA |
| Canopus HQ/HQX |Nein  |
| MPEG-4 Teil 2 |JA |
| [Theora](https://en.wikipedia.org/wiki/Theora) |JA |
| YUV420, nicht komprimiert oder Mezzanine |JA |
| Apple ProRes 422 |JA |
| Apple ProRes 422 LT |JA |
| Apple ProRes 422 HQ |JA |
| Apple ProRes Proxy |JA |
| Apple ProRes 4444 |JA |
| Apple ProRes 4444 XQ |JA |
| HEVC/H.265| Profil: Main|

## <a name="input-audio-codecs"></a>Codecs für Audioeingang
| Codecs für Audioeingang | Unterstützt |
| --- | --- | --- | --- |
| AAC (AAC-LC, AAC-HE und AAC-HEv2; bis 5.1) |JA |
| MPEG Layer 2 |JA |
| MP3 (MPEG-1 Audio Layer 3) |JA |
| Windows Media Audio |JA |
| WAV/PCM |JA |
| [FLAC](https://en.wikipedia.org/wiki/FLAC)</a> |JA |
| [Opus](https://go.microsoft.com/fwlink/?LinkId=822667) |JA |
| [Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a> |JA |
| AMR (Adaptive Multi-Rate) |JA |
| AES (SMPTE 331M und 302M, AES3-2003) |Nein  |
| Dolby® E |Nein  |
| Dolby® Digital (AC3) |Nein  |
| Dolby® Digital Plus (E-AC3) |Nein  |

## <a name="output-formats-and-codecs"></a>Ausgabeformate und Codecs
In der folgenden Tabelle werden die Codecs und Dateiformate aufgeführt, die für den Export unterstützt werden.

| Dateiformat | Videocodec | Audiocodec |
| --- | --- | --- |
| MP4  <br/><br/>(einschließlich Multi-Bitrate-MP4-Container) |H.264 (High, Main und Baseline Profile) |AAC-LC, HE-AAC v1, HE-AAC v2 |
| MPEG2-TS |H.264 (High, Main und Baseline Profile) |AAC-LC, HE-AAC v1, HE-AAC v2 |

## <a name="media-services-learning-paths"></a>Media Services-Lernpfade
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Weitere Informationen
[Codieren von On-Demand-Inhalten mit Azure Media Services](media-services-encode-asset.md)

[Gewusst wie: Codieren mit Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md)

