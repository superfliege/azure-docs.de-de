---
title: Codierung in der Cloud mit Azure Media Services | Microsoft-Dokumentation
description: In diesem Thema wird der Codierungsvorgang bei Verwendung von Azure Media Services beschrieben
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 10/15/2018
ms.author: juliako
ms.openlocfilehash: bee74f0399def142915aa26d15ecfa671925f405
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50025581"
---
# <a name="encoding-with-azure-media-services"></a>Codierung mit Azure Media Services

Azure Media Services ermöglicht das Codieren Ihrer hochwertigen digitalen Mediendateien in Formate, die mit einer Vielzahl von Browsern und Geräten wiedergegeben werden können. So können Sie Ihre Inhalte beispielsweise im HLS-Format von Apple oder im MPEG DASH-Format streamen. Mit Media Services können Sie auch Ihre Video- oder Audioinhalte analysieren. In diesem Thema erhalten Sie Anleitungen zum Codieren Ihrer Inhalte mit Media Services v3.

Um mit Media Services v3 codieren zu können, müssen Sie eine Transformation und einen Auftrag erstellen. Eine Transformation definiert die Anweisung für die Codierungseinstellungen und -ausgaben, der Auftrag ist eine Instanz der Anweisung. Weitere Informationen finden Sie unter [Transformationen und Aufträge](transform-concept.md).

Bei der Codierung mit Azure Media Services verwenden Sie Voreinstellungen, um dem Encoder mitzuteilen, wie die eingegebenen Mediendateien verarbeitet werden sollen. Beispielsweise können Sie die gewünschte Videoauflösung und/oder die Anzahl der Audiokanäle für den codierten Inhalt angeben. 

Mit einer der empfohlenen integrierten Voreinstellungen basierend auf in der Branche bewährten Vorgehensweisen können Sie schnell einsteigen. Alternativ können Sie eine benutzerdefinierte Voreinstellung für Ihr spezielles Szenario oder Ihre Geräteanforderungen erstellen. Weitere Informationen finden Sie unter [Codieren mit einer benutzerdefinierten Transformation](customize-encoder-presets-how-to.md). 

## <a name="built-in-presets"></a>Integrierte Voreinstellungen

Media Services unterstützt derzeit die folgenden integrierten Codierungsvoreinstellungen:  

|**Name der Voreinstellung**|**Szenario**|**Details**|
|---|---|---|
|**AudioAnalyzerPreset**|Analysieren von Audioinhalten|Die Voreinstellung wendet einen vordefinierten Satz von AI-basierten Analysevorgängen, einschließlich Sprachtranskription, an. Die Voreinstellung unterstützt derzeit die Verarbeitung von Inhalten mit einer einzelnen Audiospur.<br/>Sie können die Sprache für die Audio-Nutzlast in der Eingabe angeben. Dazu verwenden Sie das BCP-47-Format des Tags zur Identifizierung von Sprache und Region (z. B. „en-US“). Folgende Sprachen werden unterstützt: „en-US“, „en-GB“, „es-ES“, „es-MX“, „fr-FR“, „It-IT“, „ja-JP“, „pt-BR“, „zh-CN“.|
|**VideoAnalyzerPreset**|Analysieren von Audio und Video|Extrahiert Erkenntnisse (umfangreiche Metadaten) von Audio- und Videoinhalten und gibt eine Datei im JSON-Format aus. Beim Verarbeiten einer Videodatei können Sie angeben, ob Sie nur Audioerkenntnisse erhalten möchten. Weitere Informationen finden Sie unter [Analysieren von Videos](analyze-videos-tutorial-with-api.md).|
|**BuiltInStandardEncoderPreset**|Streaming|Wird zum Festlegen einer integrierten Voreinstellung für die Codierung des Eingabevideos mit dem Standard-Encoder verwendet. <br/>Die folgenden Voreinstellungen werden derzeit unterstützt:<br/>**EncoderNamedPreset.AdaptiveStreaming** (empfohlen). Weitere Informationen finden Sie unter [Automatisches Generieren einer Reihe von Bitraten-/Auflösungspaaren](autogen-bitrate-ladder.md).<br/>**EncoderNamedPreset.AACGoodQualityAudio**: Erzeugt eine einzelne MP4-Datei, die nur mit 192 Kbit/s codierte Stereo-Audiodaten enthält.<br/>**EncoderNamedPreset.H264MultipleBitrate1080p**: Erzeugt einen Satz von acht MP4-Dateien mit GOP-Ausrichtung mit Werten zwischen 6000 Kbit/s und 400 Kbit/s sowie Stereo-AAC-Audio. Die Auflösung beginnt bei 1080p und reduziert sich auf 360p.<br/>**EncoderNamedPreset.H264MultipleBitrate720p**: Erzeugt einen Satz von sechs MP4-Dateien mit GOP-Ausrichtung mit Werten zwischen 3400 Kbit/s und 400 Kbit/s sowie Stereo-AAC-Audio. Die Auflösung beginnt bei 720p und reduziert sich auf 360p.<br/>**EncoderNamedPreset.H264MultipleBitrateSD**: Erzeugt einen Satz von fünf MP4-Dateien mit GOP-Ausrichtung mit Werten zwischen 1600 Kbit/s und 400 Kbit/s sowie Stereo-AAC-Audio. Die Auflösung beginnt bei 480p und reduziert sich auf 360p.<br/><br/>Weitere Informationen finden Sie unter [Hochladen, Codieren und Streamen von Dateien](stream-files-tutorial-with-api.md).|
|**StandardEncoderPreset**|Streaming|Beschreibt die Einstellungen, die beim Codieren des Eingabevideos mit dem Standard-Encoder verwendet werden sollen. <br/>Verwenden Sie diese Voreinstellung, wenn Sie Transformationseinstellungen anpassen. Weitere Informationen finden Sie unter [Anpassen von Transformationseinstellungen](customize-encoder-presets-how-to.md).|

## <a name="custom-presets"></a>Benutzerdefinierte Voreinstellungen

Media Services unterstützt die vollständige Anpassung aller Werte in Voreinstellungen zum Erfüllen Ihrer spezifischen Codierungsanforderungen. Verwenden Sie die Voreinstellung **StandardEncoderPreset**, wenn Sie Transformationsvoreinstellungen anpassen. Ausführliche Erläuterungen und ein Beispiel finden Sie unter [Anpassen von Encoder-Voreinstellungen](customize-encoder-presets-how-to.md).

## <a name="scaling-encoding-in-v3"></a>Skalieren der Codierung in v3

Derzeit müssen Kunden zum Festlegen von RUs das Azure-Portal oder APIs von AMS v2 verwenden (wie unter [Skalieren der Medienverarbeitung](../previous/media-services-scale-media-processing-overview.md) beschrieben). 

## <a name="next-steps"></a>Nächste Schritte

### <a name="tutorials"></a>Lernprogramme

Die folgenden Tutorials zeigen, wie Sie Ihre Inhalte mit Media Services codieren:

* [Hochladen, Codieren und Streamen mit Azure Media Services](stream-files-tutorial-with-api.md)
* [Analysieren von Videos mit Azure Media Services](analyze-videos-tutorial-with-api.md)

### <a name="code-samples"></a>Codebeispiele

Die folgenden Codebeispiele enthalten Code, der das Codieren mit Media Services veranschaulicht:

* [.NET Core](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/tree/master/NETCore)
* [Azure-Befehlszeilenschnittstelle](https://github.com/Azure/azure-docs-cli-python-samples/tree/master/media-services)

### <a name="sdks"></a>SDKs

Zum Codieren Ihrer Inhalte können Sie die folgenden unterstützten SDKs von Media Services v3 verwenden.

* [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
* [REST](https://docs.microsoft.com/rest/api/media/transforms)
* [.NET](https://docs.microsoft.com/dotnet/api/overview/azure/mediaservices/management?view=azure-dotnet)
* [Java](https://docs.microsoft.com/java/api/overview/azure/mediaservices)
* [Python](https://docs.microsoft.com/python/api/overview/azure/media-services?view=azure-python)

