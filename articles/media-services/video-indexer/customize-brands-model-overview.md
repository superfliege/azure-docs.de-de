---
title: 'Anpassen eines Markenmodells in Video Indexer: Azure'
titlesuffix: Azure Media Services
description: Dieser Artikel gibt einen Überblick darüber, was ein Markenmodell in Video Indexer ist und wie Sie es anpassen können.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.topic: article
ms.date: 12/05/2018
ms.author: anzaman
ms.openlocfilehash: 7df709adbd8e45712c112b52fc76920f8b67fe91
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/12/2018
ms.locfileid: "53282956"
---
# <a name="customize-a-brands-model-in-video-indexer"></a>Anpassen eines Markenmodells in Video Indexer

Video Indexer unterstützt die Markenerkennung aus Sprache und visuellem Text während der Indizierung und Neuindizierung von Video- und Audioinhalten. Das Feature zur Erkennung von Marken identifiziert Erwähnungen von Produkten, Diensten und Unternehmen, die von der Markendatenbank von Bing vorgeschlagen werden. Wenn Microsoft beispielsweise in einem Video- oder Audioinhalt erwähnt wird oder im visuellen Text in einem Video angegeben wird, erkennt Video Indexer dies als Marke im Inhalt. Marken werden über den Kontext eindeutig von anderen Begriffen unterschieden.

Die Markenerkennung ist in einer Vielzahl von Geschäftsszenarien nützlich, z. B. bei der Archivierung und Erkennung von Inhalten, bei kontextabhängiger Werbung, Social Media-Analysen, Wettbewerbsanalyse im Einzelhandel und vielem mehr. Die Markenerkennung von Video Indexer ermöglicht es Ihnen, die Erwähnungen von Marken in Sprache und visuellem Text über die Markendatenbank von Bing und durch Anpassungen zu indizieren, indem Sie ein benutzerdefiniertes Markenmodell für jedes Video Indexer-Konto erstellen. Das Feature für benutzerdefinierte Markenmodelle ermöglicht das Auswählen, ob Video Indexer Marken aus der Bing-Markendatenbank erkennt, bestimmte Marken von der Erkennung ausschließt (im Wesentlichen eine schwarze Liste von Marken erstellt) und Marken einbezieht, die Teil Ihres Modells sein sollten, die möglicherweise nicht in der Bing-Markendatenbank enthalten sind (im Wesentlichen Erstellung einer Whitelist von Marken).

## <a name="out-of-the-box-detection-example"></a>Beispiel zur vorkonfigurierten Erkennung

In der Präsentation [Microsoft Build 2017 Day 2](https://www.videoindexer.ai/media/ed6ede78ad/) wird die Marke „Microsoft Windows“ mehrfach verwendet. Gelegentlich im Transkript, manchmal als sichtbarer Text, aber nie buchstäblich. Video Indexer erkennt mit hoher Genauigkeit auf der Grundlage des Kontexts, dass ein Begriff tatsächlich eine Marke ist, wobei über 90.000 Marken vorkonfiguriert sind und ständig aktualisiert werden. Video Indexer erkennt die Marke bei 02:25 in den Sprachinhalten und dann erneut bei 02:40 im sichtbaren Text, der Teil des Windows-Logos ist.

![Markenübersicht](./media/content-model-customization/brands-overview.png)

Beim Erwähnen von Fenstern (windows) im Kontext des Bauwesens wird das Wort „Windows“ nicht als Marke erkannt. Dasselbe gilt basierend auf fortschrittlichen Machine Learning-Algorithmen auch für „Box“, „Apple“, „Fox“ usw., da diese Algorithmen wissen, wie diese Wörter eindeutig anhand des Kontexts unterschieden werden. Die Markenerkennung funktioniert für alle unterstützten Sprachen. Klicken Sie hier, um das vollständige [Keynote-Video und den Index für Microsoft Build 2017 Day 2](http://www.videoindexer.ai/media/ed6ede78ad/) anzuzeigen.

Lesen Sie [Nächste Schritte](#next-stpes), um zu erfahren, wie Sie bei eigenen Marken vorgehen.

## <a name="next-steps"></a>Nächste Schritte

[Anpassen des Markenmodells mit APIs](customize-brands-model-with-api.md)

[Anpassen des Markenmodells über die Website](customize-brands-model-with-website.md)
