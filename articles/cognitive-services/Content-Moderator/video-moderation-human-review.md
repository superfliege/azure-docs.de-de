---
title: Azure Content Moderator – Videomoderation | Microsoft-Dokumentation
description: Verwenden Sie die computergestützte Videomoderation und Tools für die Überprüfung durch Personen, um unangemessene Inhalte zu moderieren.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/20/2018
ms.author: sajagtap
ms.openlocfilehash: fb26c9af55381c80a3f520b1a0068d8f72c91061
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/25/2018
ms.locfileid: "35372938"
---
# <a name="video-moderation"></a>Videomoderation

Verwenden Sie die in Content Moderator integrierten computergestützten Tools für die [Videomoderation](video-moderation-api.md) und [Überprüfungen durch Personen](Review-Tool-User-Guide/human-in-the-loop.md), um Videos und Transkripte für Inhalte für Erwachsene (explizite) und freizügige Inhalte (Anzüglichkeiten) zu moderieren, um die besten Ergebnisse für Ihr Unternehmen zu erzielen.

## <a name="video-trained-classifier"></a>Durch Videos trainierte Klassifizierung

Die computergestützte Videoklassifizierung erfolgt entweder mit durch Bilder oder Videos trainierte Modelle. Im Gegensatz zu durch Bilder trainierte Klassifizierungen wird die Klassifizierung für Inhalte für Erwachsene und freizügige Inhalte von Microsoft mit Videos trainiert. Dies führt zu besseren Übereinstimmungen.

## <a name="shot-detection"></a>Szenenwechselerkennung

Bei der Ausgabe der Klassifizierungsdetails ermöglicht zusätzliche Videointelligenz die flexiblere Analyse von Videos. Anstatt nur die Frames auszugeben, bietet der Video Moderation Service von Microsoft auch Informationen auf Szenenebene. Sie haben nun die Möglichkeit, Ihre Videos auf der Ebene der Szenen und der Frames zu analysieren.
 
## <a name="key-frame-detection"></a>Keyframeerkennung

Anstatt in regelmäßigen Abständen Frames auszugeben, identifiziert der Video Moderation Service nur potenziell vollständige (gute) Frames und gibt nur diese aus. Diese Funktion ermöglicht eine effiziente Frame-Generierung für Analyse von Inhalten für Erwachsene und von freizügigen Inhalten auf Frameebene.

Der folgende Auszug zeigt eine Teilantwort mit potentiellen Aufnahmen, Keyframes, sowie Bewertungen von Inhalten für Erwachsene und von freizügigen Inhalten:

    "fragments": [
    {
      "start": 0,
      "duration": 18000
    },
    {
      "start": 18000,
      "duration": 3600,
      "interval": 3600,
      "events": [
        [
          {
            "reviewRecommended": false,
            "adultScore": 0.00001,
            "racyScore": 0.03077,
            "index": 5,
            "timestamp": 18000,
            "shotIndex": 0
          }
        ]
      ]
    },
    {
      "start": 18386372,
      "duration": 119149,
      "interval": 119149,
      "events": [
        [
          {
            "reviewRecommended": true,
            "adultScore": 0.00000,
            "racyScore": 0.91902,
            "index": 5085,
            "timestamp": 18386372,
            "shotIndex": 62
          }
        ]
      ]


## <a name="visualization-for-human-reviews"></a>Visualisierung für Überprüfungen durch Personen

Für differenzierte Fälle benötigen Unternehmen eine Lösung für Überprüfungen durch Personne für das Rendern des Videos, seiner Frames und der von Computern zugewiesenen Tags. Die menschlichen Moderatoren, die Videos und Frames überprüfen, erhalten einen vollständigen Überblick über die Einsichten, ändern Tags und reichen ihre Entscheidungen ein.

![Videoprüfungstool – Standardansicht](images/video-review-default-view.png)

## <a name="player-view-for-video-level-review"></a>Playeransicht zur Überprüfung auf Videoebene

Binäre Entscheidungen auf Videoebene werden durch eine Videoplayeransicht ermöglicht, die potenzielle Frames für Erwachsene und freizügige Frames zeigt. Der menschliche Prüfer kann mit verschiedenen Geschwindigkeitsoptionen im Video navigieren und die Szenen untersuchen. Um ihre Entscheidungen zu bestätigen, schalten Sie die Tags um.

![Videoprüfungstool – Playeransicht](images/video-review-player-view.PNG)

## <a name="frames-view-for-detailed-reviews"></a>Framesansicht für detaillierte Überprüfungen

Die framebasierte Ansicht ermöglicht eine detaillierte Videoüberprüfung für die Frame-für-Frame-Analyse. Die menschlichen Prüfer überprüfen und wählen einen oder mehrere Frames und schalten die Tags um, um ihre Entscheidungen zu bestätigen. Ein optionaler nächsten Schritt ist die Bearbeitung der anstößigen Frames oder Inhalte.

![Videoprüfungstool – Framesansicht](images/video-review-frames-view-apply-tags.PNG)

## <a name="transcript-moderation"></a>Transkriptmoderation

Videos haben in der Regel einen Begleitkommentar, der ebenfalls hinsichtlich anstößiger Sprache moderiert werden muss. Sie verwenden den Azure Media Indexer-Dienst, um Sprache in Text umzuwandeln, und übermitteln das Transkript zur Textmoderation innerhalb des Prüfungstools an die Überprüfungs-API von Content Moderator.

![Videoprüfungstool – Transkriptansicht](images/video-review-transcript-view.png)

## <a name="next-steps"></a>Nächste Schritte

Beginnen Sie mit dem [Schnellstart für die Videomoderation](video-moderation-api.md). 

Erfahren Sie mehr über das Generieren von [Videoüberprüfungen](video-reviews-quickstart-dotnet.md) für Ihre menschlichen Prüfer aus Ihrer moderierten Ausgabe.

Fügen Sie [Überprüfungen von Videotranskripten](video-transcript-reviews-quickstart-dotnet.md) zu Ihren Videoüberprüfungen hinzu.

Sehen Sie sich das ausführliche Tutorial zur Entwicklung einer [vollständigen Lösung für die Videomoderation an](video-transcript-moderation-review-tutorial-dotnet.md). 
