---
title: 'Media Encoder Standard-Formate und -Codecs: Azure'
description: Dieses Thema bietet eine Übersicht über Media Encoder Standard-Formate und -Codecs.
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
ms.date: 12/12/2018
ms.author: juliako;anilmur
ms.openlocfilehash: 22603de40e2ebafb1354377898f5a0432e4f6129
ms.sourcegitcommit: 85d94b423518ee7ec7f071f4f256f84c64039a9d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2018
ms.locfileid: "53388338"
---
# <a name="media-encoder-standard-formats-and-codecs"></a>Media Encoder Standard-Formate und -Codecs

Dieser Artikel enthält eine Liste der gängigsten Import- und Exportdateiformate, die Sie mit [StandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#standardencoderpreset) verwenden können. Informationen zum Erstellen benutzerdefinierter Voreinstellungen mit **StandardEncoderPreset** finden Sie unter [Erstellen einer Transformation mit einer benutzerdefinierten Voreinstellung](customize-encoder-presets-how-to.md).

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

## <a name="next-steps"></a>Nächste Schritte

[Erstellen einer Transformation mit einer benutzerdefinierten Voreinstellung](customize-encoder-presets-how-to.md)
