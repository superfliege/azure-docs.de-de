---
title: Übersicht über die dynamische Paketerstellung mit Azure Media Services | Microsoft-Dokumentation
description: In diesem Thema finden Sie eine Übersicht über die dynamische Paketerstellung in Media Services.
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/19/2019
ms.author: juliako
ms.openlocfilehash: d1d07402bca5f01cf63d0b039c085e46bb0f0d62
ms.sourcegitcommit: 6cab3c44aaccbcc86ed5a2011761fa52aa5ee5fa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2019
ms.locfileid: "56447921"
---
# <a name="dynamic-packaging"></a>Dynamische Paketerstellung

Microsoft Azure Media Services kann verwendet werden, um zahlreiche Medien-Quelldateiformate, Medienstreamingformate und Inhaltsschutzformate in verschiedenen Clienttechnologien wie iOS und XBOX bereitzustellen. Von den Clients werden verschiedene Protokolle verarbeitet. So ist für iOS ein HLS-Format (HTTP Live Streaming) erforderlich, für Xbox dagegen Smooth Streaming. Wenn Sie über einen Satz MP4-Dateien (ISO Base Media 14496-12) mit adaptiver Bitrate (Multi-Bitrate) oder einen Satz von Smooth Streaming-Dateien mit adaptiver Bitrate verfügen, die Sie Clients bereitstellen möchten, von denen HLS, MPEG DASH oder Smooth Streaming verarbeitet wird, sollten Sie die Vorteile der dynamischen Paketerstellung von Media Services nutzen.

Bei der dynamischen Paketerstellung müssen Sie lediglich ein Medienobjekt erstellen, das einen Satz von MP4-Dateien mit adaptiver Bitrate enthält. Dann wird durch den bedarfsgesteuerten Streamingserver auf Basis des in der Manifest- oder Fragmentanforderung angegebenen Formats sichergestellt, dass Sie den Datenstrom im ausgewählten Protokoll erhalten. So müssen Sie die Dateien nur in einem Speicherformat speichern und bezahlen. Die entsprechende Antwort wird von Media Services basierend auf Clientanforderungen erstellt und verfügbar gemacht.

Im folgenden Diagramm wird der herkömmliche Workflow zur Codierung und statischen Paketerstellung dargestellt.

![Statische Codierung](./media/dynamic-packaging-overview/media-services-static-packaging.png)

Im folgenden Diagramm wird der Workflow zur dynamischen Paketerstellung dargestellt.

![Dynamische Codierung](./media/dynamic-packaging-overview/media-services-dynamic-packaging.png)

## <a name="dynamic-packaging-workflow"></a>Workflow zur dynamischen Paketerstellung

1. Laden Sie eine Eingabedatei (auch Mezzaninedatei genannt) hoch. Beispielformate: H.264, MP4 oder WMV (eine Liste unterstützter Formate finden Sie unter [Von Media Encoder Standard unterstützte Formate](media-encoder-standard-formats.md)).
2. Codieren Sie Ihre Mezzaninedatei zu H.264-MP4-Sätzen mit adaptiver Bitrate.
3. Veröffentlichen Sie das Medienobjekt, das den MP4-Satz mit adaptiver Bitrate enthält.
4. Erstellen Sie die Streaming-URLs zum Zugreifen und Streamen Ihrer Inhalte.

## <a name="audio-codecs-supported-by-dynamic-packaging"></a>Von der dynamischen Paketerstellung unterstützte Audiocodecs

Die dynamische Paketerstellung unterstützt MP4-Dateien mit Audio, das mit [AAC](https://en.wikipedia.org/wiki/Advanced_Audio_Coding) (AAC-LC, HE-AAC v1, HE-AAC v2), [Dolby Digital Plus](https://en.wikipedia.org/wiki/Dolby_Digital_Plus) (Enhanced AC-3 oder E-AC3) oder [DTS](https://en.wikipedia.org/wiki/DTS_%28sound_system%29) (DTS Express, DTS LBR, DTS HD, DTS HD Lossless) codiert wurde.

> [!NOTE]
> Dateien mit [Dolby Digital](https://en.wikipedia.org/wiki/Dolby_Digital)-Audio (AC3) werden von der dynamischen Paketerstellung nicht unterstützt, da es sich dabei um einen Legacy-Codec handelt.

## <a name="next-steps"></a>Nächste Schritte

[Hochladen, Codieren und Streamen von Videos](stream-files-tutorial-with-api.md)
