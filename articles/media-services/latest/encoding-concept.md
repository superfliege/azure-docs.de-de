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
ms.date: 02/27/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: de2c60d4449762c4a8fcc3e2f486130f3df37c7c
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2019
ms.locfileid: "57243618"
---
# <a name="encoding-with-media-services"></a>Codierung mit Media Services

Azure Media Services ermöglicht das Codieren Ihrer hochwertigen digitalen Mediendateien in MP4-Dateien mit adaptiven Bitraten, sodass Ihre Inhalte mit einer Vielzahl von Browsern und Geräten wiedergegeben werden können. Durch einen erfolgreichen Media Services-Codierungsauftrag wird ein Ausgabemedienobjekt mit einer Gruppe von MP4-Dateien mit adaptiven Bitraten und Streaming-Konfigurationsdateien erstellt. Die Konfigurationsdateien enthalten ISM-, ISMC-, MPI- und andere Dateien, die Sie nicht ändern sollten. Sobald der Codierungsauftrag abgeschlossen ist, können Sie von [dynamischer Paketerstellung](dynamic-packaging-overview.md) profitieren und mit dem Streaming beginnen.

Um Videos im Ausgabemedienobjekt für die Wiedergabe durch Kunden verfügbar zu machen, müssen Sie einen **Streaminglocator** und Streaming-URLs erstellen. Basierend auf dem im Manifest angegebenen Format, empfangen Ihre Kunden dann den Stream im ausgewählten Protokoll.

Das nachstehende Diagramm zeigt das On-Demand-Streaming mit dem Workflow zur dynamischen Paketerstellung.

![Dynamische Paketerstellung](./media/dynamic-packaging-overview/media-services-dynamic-packaging.svg)

In diesem Thema erhalten Sie Anleitungen zum Codieren Ihrer Inhalte mit Media Services v3.

## <a name="transforms-and-jobs"></a>Transformationen und Aufträge

Um mit Media Services v3 codieren zu können, müssen Sie eine [Transformation](https://docs.microsoft.com/rest/api/media/transforms) und einen [Auftrag](https://docs.microsoft.com/rest/api/media/jobs) erstellen. Eine Transformation definiert die Anweisung für die Codierungseinstellungen und -ausgaben, der Auftrag ist eine Instanz der Anweisung. Weitere Informationen finden Sie unter [Transformationen und Aufträge](transforms-jobs-concept.md).

Bei der Codierung mit Media Services verwenden Sie Voreinstellungen, um dem Encoder mitzuteilen, wie die eingegebenen Mediendateien verarbeitet werden sollen. Beispielsweise können Sie die gewünschte Videoauflösung und/oder die Anzahl der Audiokanäle für den codierten Inhalt angeben. 

Mit einer der empfohlenen integrierten Voreinstellungen basierend auf in der Branche bewährten Vorgehensweisen können Sie schnell einsteigen. Alternativ können Sie eine benutzerdefinierte Voreinstellung für Ihr spezielles Szenario oder Ihre Geräteanforderungen erstellen. Weitere Informationen finden Sie unter [Codieren mit einer benutzerdefinierten Transformation](customize-encoder-presets-how-to.md). 

Ab Januar 2019 wird bei der Codierung mit Media Encoder Standard zum Erzeugen von MP4-Dateien eine neue MPI-Datei generiert und dem Ausgabeasset hinzugefügt. Diese MPI-Datei dient zum Verbessern der Leistung für die [dynamische Paketerstellung](dynamic-packaging-overview.md) und Streamingszenarios.

> [!NOTE]
> Sie sollten weder die MPI-Datei ändern oder entfernen noch beliebige Abhängigkeiten vom Vorhandensein (oder Nichtvorhandensein) einer solchen Datei in Ihren Dienst integrieren.

## <a name="built-in-presets"></a>Integrierte Voreinstellungen

Media Services unterstützt derzeit die folgenden integrierten Codierungsvoreinstellungen:  

### <a name="builtinstandardencoderpreset-preset"></a>Voreinstellung „BuiltInStandardEncoderPreset“

[BuiltInStandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#builtinstandardencoderpreset) wird zum Festlegen einer integrierten Voreinstellung für die Codierung des Eingabevideos mit dem Standard-Encoder verwendet. 

Die folgenden Voreinstellungen werden derzeit unterstützt:

- **EncoderNamedPreset.AdaptiveStreaming** (empfohlen). Weitere Informationen finden Sie unter [Automatisches Generieren einer Reihe von Bitraten-/Auflösungspaaren](autogen-bitrate-ladder.md).
- **EncoderNamedPreset.AACGoodQualityAudio**: Erzeugt eine einzelne MP4-Datei, die nur mit 192 Kbit/s codierte Stereo-Audiodaten enthält.
- **EncoderNamedPreset.H264MultipleBitrate1080p**: Erzeugt einen Satz von acht MP4-Dateien mit GOP-Ausrichtung mit Werten zwischen 6000 Kbit/s und 400 Kbit/s sowie Stereo-AAC-Audio. Die Auflösung beginnt bei 1080p und reduziert sich auf 360p.
- **EncoderNamedPreset.H264MultipleBitrate720p**: Erzeugt einen Satz von sechs MP4-Dateien mit GOP-Ausrichtung mit Werten zwischen 3400 Kbit/s und 400 Kbit/s sowie Stereo-AAC-Audio. Die Auflösung beginnt bei 720p und reduziert sich auf 360p.
- **EncoderNamedPreset.H264MultipleBitrateSD**: Erzeugt einen Satz von fünf MP4-Dateien mit GOP-Ausrichtung mit Werten zwischen 1600 Kbit/s und 400 Kbit/s sowie Stereo-AAC-Audio. Die Auflösung beginnt bei 480p und reduziert sich auf 360p.<br/><br/>Weitere Informationen finden Sie unter [Hochladen, Codieren und Streamen von Dateien](stream-files-tutorial-with-api.md).

### <a name="standardencoderpreset-preset"></a>Voreinstellung „StandardEncoderPreset“

[StandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#standardencoderpreset) beschreibt die Einstellungen, die beim Codieren des Eingabevideos mit dem Standard-Encoder verwendet werden sollen. Verwenden Sie diese Voreinstellung, wenn Sie Transformationseinstellungen anpassen. 

#### <a name="custom-presets"></a>Benutzerdefinierte Voreinstellungen

Media Services unterstützt die vollständige Anpassung aller Werte in Voreinstellungen zum Erfüllen Ihrer spezifischen Codierungsanforderungen. Verwenden Sie die Voreinstellung **StandardEncoderPreset**, wenn Sie Transformationsvoreinstellungen anpassen. Ausführliche Erläuterungen und ein Beispiel finden Sie unter [Anpassen von Encoder-Voreinstellungen](customize-encoder-presets-how-to.md).

## <a name="scaling-encoding-in-v3"></a>Skalieren der Codierung in v3

Informationen zum Skalieren der Medienverarbeitung finden Sie unter [Skalieren mit CLI](media-reserved-units-cli-how-to.md).

## <a name="next-steps"></a>Nächste Schritte

* [Codieren aus einer HTTPS-URL mithilfe von integrierten Voreinstellungen](job-input-from-http-how-to.md)
* [Codieren einer lokalen Datei mithilfe von integrierten Voreinstellungen](job-input-from-local-file-how-to.md)
* [Entwickeln einer benutzerdefinierten Voreinstellung für Ihr spezielles Szenario oder Ihre Geräteanforderungen](customize-encoder-presets-how-to.md)
* [Hochladen, Codieren und Streamen mit Media Services](stream-files-tutorial-with-api.md)
