---
title: 'Codierung in der Cloud mit Media Services: Azure | Microsoft-Dokumentation'
description: In diesem Thema wird der Codierungsvorgang bei Verwendung von Azure Media Services beschrieben
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 01/22/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: d236f00e70e08c7bce2a94c5bd4fb64f1fa99bbc
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54826597"
---
# <a name="encoding-with-media-services"></a>Codierung mit Media Services

Azure Media Services ermöglicht das Codieren Ihrer hochwertigen digitalen Mediendateien in Formate, die mit einer Vielzahl von Browsern und Geräten wiedergegeben werden können. So können Sie Ihre Inhalte beispielsweise im HLS-Format von Apple oder im MPEG DASH-Format streamen. In diesem Thema erhalten Sie Anleitungen zum Codieren Ihrer Inhalte mit Media Services v3.

Um mit Media Services v3 codieren zu können, müssen Sie eine Transformation und einen Auftrag erstellen. Eine Transformation definiert die Anweisung für die Codierungseinstellungen und -ausgaben, der Auftrag ist eine Instanz der Anweisung. Weitere Informationen finden Sie unter [Transformationen und Aufträge](transform-concept.md).

Bei der Codierung mit Media Services verwenden Sie Voreinstellungen, um dem Encoder mitzuteilen, wie die eingegebenen Mediendateien verarbeitet werden sollen. Beispielsweise können Sie die gewünschte Videoauflösung und/oder die Anzahl der Audiokanäle für den codierten Inhalt angeben. 

Mit einer der empfohlenen integrierten Voreinstellungen basierend auf in der Branche bewährten Vorgehensweisen können Sie schnell einsteigen. Alternativ können Sie eine benutzerdefinierte Voreinstellung für Ihr spezielles Szenario oder Ihre Geräteanforderungen erstellen. Weitere Informationen finden Sie unter [Codieren mit einer benutzerdefinierten Transformation](customize-encoder-presets-how-to.md). 

Ab Januar 2019 wird bei der Codierung mit Media Encoder Standard zum Erzeugen von MP4-Dateien eine neue MPI-Datei generiert und dem Ausgabeasset hinzugefügt. Diese MPI-Datei dient zum Verbessern der Leistung für die dynamische Paketerstellung und Streamingszenarios.

> [!NOTE]
> Sie sollten weder die MPI-Datei ändern oder entfernen noch beliebige Abhängigkeiten vom Vorhandensein (oder Nichtvorhandensein) einer solchen Datei in Ihren Dienst integrieren.

## <a name="built-in-presets"></a>Integrierte Voreinstellungen

Media Services unterstützt derzeit die folgenden integrierten Codierungsvoreinstellungen:  

|**Name der Voreinstellung**|**Szenario**|**Details**|
|---|---|---|
|**BuiltInStandardEncoderPreset**|Streaming|Wird zum Festlegen einer integrierten Voreinstellung für die Codierung des Eingabevideos mit dem Standard-Encoder verwendet. <br/>Die folgenden Voreinstellungen werden derzeit unterstützt:<br/>**EncoderNamedPreset.AdaptiveStreaming** (empfohlen). Weitere Informationen finden Sie unter [Automatisches Generieren einer Reihe von Bitraten-/Auflösungspaaren](autogen-bitrate-ladder.md).<br/>**EncoderNamedPreset.AACGoodQualityAudio**: Erzeugt eine einzelne MP4-Datei, die nur mit 192 Kbit/s codierte Stereo-Audiodaten enthält.<br/>**EncoderNamedPreset.H264MultipleBitrate1080p**: Erzeugt einen Satz von acht MP4-Dateien mit GOP-Ausrichtung mit Werten zwischen 6000 Kbit/s und 400 Kbit/s sowie Stereo-AAC-Audio. Die Auflösung beginnt bei 1080p und reduziert sich auf 360p.<br/>**EncoderNamedPreset.H264MultipleBitrate720p**: Erzeugt einen Satz von sechs MP4-Dateien mit GOP-Ausrichtung mit Werten zwischen 3400 Kbit/s und 400 Kbit/s sowie Stereo-AAC-Audio. Die Auflösung beginnt bei 720p und reduziert sich auf 360p.<br/>**EncoderNamedPreset.H264MultipleBitrateSD**: Erzeugt einen Satz von fünf MP4-Dateien mit GOP-Ausrichtung mit Werten zwischen 1600 Kbit/s und 400 Kbit/s sowie Stereo-AAC-Audio. Die Auflösung beginnt bei 480p und reduziert sich auf 360p.<br/><br/>Weitere Informationen finden Sie unter [Hochladen, Codieren und Streamen von Dateien](stream-files-tutorial-with-api.md).|
|**StandardEncoderPreset**|Streaming|Beschreibt die Einstellungen, die beim Codieren des Eingabevideos mit dem Standard-Encoder verwendet werden sollen. <br/>Verwenden Sie diese Voreinstellung, wenn Sie Transformationseinstellungen anpassen. Weitere Informationen finden Sie unter [Anpassen von Transformationseinstellungen](customize-encoder-presets-how-to.md).|

## <a name="custom-presets"></a>Benutzerdefinierte Voreinstellungen

Media Services unterstützt die vollständige Anpassung aller Werte in Voreinstellungen zum Erfüllen Ihrer spezifischen Codierungsanforderungen. Verwenden Sie die Voreinstellung **StandardEncoderPreset**, wenn Sie Transformationsvoreinstellungen anpassen. Ausführliche Erläuterungen und ein Beispiel finden Sie unter [Anpassen von Encoder-Voreinstellungen](customize-encoder-presets-how-to.md).

## <a name="scaling-encoding-in-v3"></a>Skalieren der Codierung in v3

Derzeit müssen Kunden zum Festlegen von RUs das Azure-Portal oder APIs von Media Services v2 verwenden (wie unter [Skalieren der Medienverarbeitung](../previous/media-services-scale-media-processing-overview.md) beschrieben). 

## <a name="next-steps"></a>Nächste Schritte

### <a name="tutorials"></a>Lernprogramme

Im folgenden Tutorial wird veranschaulicht, wie Sie Ihre Inhalte mit Media Services codieren:

* [Hochladen, Codieren und Streamen mit Media Services](stream-files-tutorial-with-api.md)

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

