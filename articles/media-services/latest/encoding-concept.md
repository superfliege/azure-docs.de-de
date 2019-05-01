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
ms.date: 04/21/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: ed798995807f4037f0127b08e25e04bdd0340d42
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2019
ms.locfileid: "64724307"
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

- **EncoderNamedPreset.AACGoodQualityAudio**: Erzeugt eine einzelne MP4-Datei, die nur mit 192 Kbit/s codierte Stereo-Audiodaten enthält.
- **EncoderNamedPreset.AdaptiveStreaming** (empfohlen). Weitere Informationen finden Sie unter [Automatisches Generieren einer Reihe von Bitraten-/Auflösungspaaren](autogen-bitrate-ladder.md).
- **EncoderNamedPreset.ContentAwareEncodingExperimental**: Stellt eine experimentelle Voreinstellung für die inhaltsbezogene Codierung zur Verfügung. Anhand der eingegebenen Inhalte versucht der Dienst, automatisch die optimale Anzahl der Ebenen, die geeignete Bitrate und die Auflösungseinstellungen für die Bereitstellung durch adaptives Streaming zu bestimmen. Die zugrunde liegenden Algorithmen werden im Laufe der Zeit weiter entwickelt. Die Ausgabe enthält MP4-Dateien mit überlappendem Video und Audio. Weitere Informationen finden Sie unter [Experimentelle Voreinstellung für inhaltsbezogene Codierung](cae-experimental.md).
- **EncoderNamedPreset.H264MultipleBitrate1080p**: Erzeugt einen Satz von acht MP4-Dateien mit GOP-Ausrichtung mit Werten zwischen 6000 Kbit/s und 400 Kbit/s sowie Stereo-AAC-Audio. Die Auflösung beginnt bei 1080p und reduziert sich auf 360p.
- **EncoderNamedPreset.H264MultipleBitrate720p**: Erzeugt einen Satz von sechs MP4-Dateien mit GOP-Ausrichtung mit Werten zwischen 3400 Kbit/s und 400 Kbit/s sowie Stereo-AAC-Audio. Die Auflösung beginnt bei 720p und reduziert sich auf 360p.
- **EncoderNamedPreset.H264MultipleBitrateSD**: Erzeugt einen Satz von fünf MP4-Dateien mit GOP-Ausrichtung mit Werten zwischen 1600 Kbit/s und 400 Kbit/s sowie Stereo-AAC-Audio. Die Auflösung beginnt bei 480p und reduziert sich auf 360p.
- **EncoderNamedPreset.H264SingleBitrate1080p**: Erzeugt eine MP4-Datei, bei der das Video mit dem H.264-Codec bei 6750 KBit/s und einer Bildhöhe von 1080 Pixeln codiert wird, während das Stereo-Audio mit dem AAC-LC-Codec bei 64 KBit/s codiert wird.
- **EncoderNamedPreset.H264SingleBitrate720p**: Erzeugt eine MP4-Datei, bei der das Video mit dem H.264-Codec bei 4500 KBit/s und einer Bildhöhe von 720 Pixeln codiert wird, während das Stereo-Audio mit dem AAC-LC-Codec bei 64 KBit/s codiert wird.
- **EncoderNamedPreset.H264SingleBitrateSD**: Erzeugt eine MP4-Datei, bei der das Video mit dem H.264-Codec bei 2200 KBit/s und einer Bildhöhe von 480 Pixeln codiert wird, während das Stereo-Audio mit dem AAC-LC-Codec bei 64 KBit/s codiert wird.

Die aktuellste Liste der Voreinstellungen finden Sie unter [Integrierte Voreinstellungen zum Codieren von Videos](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#encodernamedpreset).

Informationen zur Verwendung der Voreinstellungen finden Sie unter [Hochladen, Codieren und Streamen von Dateien](stream-files-tutorial-with-api.md).

### <a name="standardencoderpreset-preset"></a>Voreinstellung „StandardEncoderPreset“

[StandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#standardencoderpreset) beschreibt die Einstellungen, die beim Codieren des Eingabevideos mit dem Standard-Encoder verwendet werden sollen. Verwenden Sie diese Voreinstellung, wenn Sie Transformationseinstellungen anpassen. 

#### <a name="considerations"></a>Überlegungen

Beim Erstellen von benutzerdefinierten Voreinstellungen gelten die folgenden Überlegungen:

- Alle Werte für Höhe und Breite in AVC-Inhalt müssen ein Vielfaches von 4 sein.
- In Azure Media Services v3 werden alle Codierungsbitraten in Bits pro Sekunde angegeben. Dies unterscheidet sich von den Voreinstellungen bei unseren v2-APIs. Dort wurden Kilobits pro Sekunde (KBit/s) als Einheit verwendet. Wenn beispielsweise die Bitrate in v2 als 128 (Kilobits/Sekunde) angegeben wurde, würde sie in v3 auf 128.000 (Bits/Sekunde) festgelegt.

#### <a name="examples"></a>Beispiele

Media Services unterstützt die vollständige Anpassung aller Werte in Voreinstellungen zum Erfüllen Ihrer spezifischen Codierungsanforderungen. Beispiele zum Anpassen von Encodervoreinstellungen finden Sie unter:

- [Anpassen von Voreinstellungen mit .NET](customize-encoder-presets-how-to.md)
- [Anpassen von Voreinstellungen mit der CLI](custom-preset-cli-howto.md)
- [Anpassen von Voreinstellungen mit REST](custom-preset-rest-howto.md)

## <a name="scaling-encoding-in-v3"></a>Skalieren der Codierung in v3

Informationen zum Skalieren der Medienverarbeitung finden Sie unter [Skalieren mit CLI](media-reserved-units-cli-how-to.md).

## <a name="provide-feedback"></a>Feedback geben

Im Artikel [Azure Media Services-Community](media-services-community.md) finden Sie verschiedene Möglichkeiten, Fragen zu stellen, Feedback zu geben und Updates zu Media Services zu bekommen.

## <a name="next-steps"></a>Nächste Schritte

* [Codieren aus einer HTTPS-URL mithilfe von integrierten Voreinstellungen](job-input-from-http-how-to.md)
* [Codieren einer lokalen Datei mithilfe von integrierten Voreinstellungen](job-input-from-local-file-how-to.md)
* [Entwickeln einer benutzerdefinierten Voreinstellung für Ihr spezielles Szenario oder Ihre Geräteanforderungen](customize-encoder-presets-how-to.md)
* [Hochladen, Codieren und Streamen mit Media Services](stream-files-tutorial-with-api.md)
