---
title: Übersicht über Azure Video Indexer | Microsoft-Dokumentation
description: Dieses Thema bietet eine Übersicht über den Video Indexer-Dienst.
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.component: video-indexer
ms.topic: overview
ms.date: 07/25/2018
ms.author: nolachar
ms.openlocfilehash: f52c4af29d0c7de8b5edbe869640ffc5dddb5c5e
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2018
ms.locfileid: "39397890"
---
# <a name="what-is-video-indexer-preview"></a>Was ist Video Indexer? (Vorschauversion)

Video Indexer ist eine Cloudanwendung, die mit Azure Media Analytics, Cognitive Services (beispielsweise Gesichtserkennungs-API, Microsoft Translator, Maschinelles Sehen-API und Custom Speech Service) sowie Azure Search erstellt wurde. Sie ermöglicht es Ihnen, mithilfe von KI-Technologien (Künstliche Intelligenz) die folgenden Erkenntnisse aus Ihren Videos zu extrahieren:

- **Automatische Sprachenerkennung**: Video Indexer kann die Sprache des Videos automatisch erkennen. Die automatische Sprachenerkennung unterstützt derzeit Englisch, Spanisch, Französisch, Deutsch, Italienisch, Chinesisch (vereinfacht), Japanisch und Russisch. Wenn die Sprache nicht erkannt werden kann, wird Englisch als Fallbacksprache verwendet.
- **Audiotranskription**: Video Indexer verfügt über eine Sprache-in-Text-Funktion, mit der Kunden ein Transkript des gesprochenen Textes abrufen können. Zu den unterstützten Sprachen zählen Englisch, Spanisch, Französisch, Deutsch, Italienisch, Chinesisch (vereinfacht), Portugiesisch (Brasilien), Japanisch und Russisch (viele weitere Sprachen werden in Zukunft hinzukommen). 
- **Gesichtserkennung und -identifikation**: Gesichtserkennungstechnologien ermöglichen die Erkennung von Gesichtern in einem Video. Die erkannten Gesichter werden mit einer Datenbank von Prominenten abgeglichen, um auszuwerten, ob berühmte Personen im Video zu sehen sind. Kunden können auch Gesichter, die keinem Prominenten entsprechen, mit einer Bezeichnung versehen. Video Indexer erstellt basierend auf diesen Bezeichnungen ein Gesichtsmodell und kann die entsprechenden Gesichter in künftig übermittelten Videos erkennen.
- **Sprecherindizierung**: Video Indexer erkennt, wann welche Worte von welchem Sprecher gesprochen wurde, und kann entsprechende Zuordnungen vornehmen.
- **Erkennung von sichtbarem Text**: Mit dieser Technologie extrahiert der Video Indexer-Dienst in den Videos angezeigten Text.  
- **Sprechaktivitätserkennung**: Mithilfe dieser Funktion kann Video Indexer Hintergrundgeräusche und Sprachaktivität voneinander trennen. 
- **Szenenerkennung**: Video Indexer kann eine visuelle Analyse des Videos durchführen, um Änderungen der Szene in einem Video zu ermitteln.
- **Keyframeextraktion**: Video Indexer erkennt Keyframes in einem Video automatisch. 
- **Stimmungsanalyse**: Video Indexer führt mithilfe der Sprache-in-Text-Funktion und der optischen Zeichenerkennung eine Stimmungsanalyse für den extrahierten Text durch und stellt diese Informationen zusammen mit Zeitcodes in Form von positiven, negativen oder neutralen Stimmungen bereit.
- **Übersetzung**: Video Indexer bietet die Möglichkeit, das Audiotranskript von einer Sprache in eine andere zu übersetzen. Die folgenden Sprachen werden unterstützt: Englisch, Spanisch, Französisch, Deutsch, Italienisch, Chinesisch (vereinfacht), Portugiesisch (Brasilien), Japanisch und Russisch. Nach der Übersetzung kann der Benutzer sogar Untertitel im Videoplayer in anderen Sprachen abrufen.
- **Moderation von Videoinhalten**: Diese Technologie ermöglicht die Erkennung von nicht jugendfreiem und/oder freizügigem Material im Video und kann zur Inhaltsfilterung verwendet werden. 
- **Extraktion von Schlüsselwörtern**: Video Indexer extrahiert Schlüsselwörter basierend auf dem Transkript des gesprochenen Textes und Text, der von der Erkennung von sichtbarem Text erkannt wird.
- **Bezeichnungen**: Video Indexer bietet Bezeichnungen für visuelle Objekte wie Katze, Hund, Tisch und Auto sowie Aktionen wie Stehen, Laufen oder Fliegen.
- **Marken**: Video Indexer extrahiert Unternehmensmarken basierend auf dem Transkript des gesprochenen Textes und Text, der von der Erkennung von sichtbarem Text erkannt wird.

Nachdem Video Indexer die Verarbeitung und Analyse abgeschlossen hat, können Sie die Erkenntnisse aus dem Video überprüfen, zusammenstellen, suchen und veröffentlichen.

Ganz gleich, ob Sie ein Inhaltsmanager oder ein Entwickler sind – der Video Indexer-Dienst kann all Ihren Anforderungen gerecht werden. Inhaltsmanager können den Dienst über das Video Indexer-Webportal nutzen, ohne eine einzige Codezeile schreiben zu müssen. Weitere Informationen finden Sie unter [Get started using the Video Indexer portal](video-indexer-get-started.md) (Erste Schritte mit dem Video Indexer-Portal). Entwickler können APIs zur bedarfsorientierten Verarbeitung von Inhalten nutzen. Weitere Informationen finden Sie unter [Verwenden der Video Indexer-REST-API](video-indexer-use-apis.md). Der Dienst bietet Kunden auch die Möglichkeit, mithilfe von Widgets Videodatenströme und extrahierte Erkenntnisse in ihren eigenen Anwendungen zu veröffentlichen. Weitere Informationen finden Sie unter [Embed visual widgets in your application](video-indexer-embed-widgets.md) (Einbetten von visuellen Widgets in Ihre Anwendung).

Sie können sich mit einem vorhandenen AAD-, LinkedIn-, Facebook-, Google- oder MSA-Konto für den Dienst registrieren. Weitere Informationen finden Sie unter [Getting Started](video-indexer-get-started.md) (Erste Schritte).

## <a name="scenarios"></a>Szenarien

Im Folgenden finden Sie einige Szenarien, in denen Video Indexer nützlich sein kann.

- Suche: Aus dem Video extrahierte Erkenntnisse können verwendet werden, um die Suchfunktion für eine Videobibliothek zu verbessern. Beispielsweise kann die Indizierung von gesprochenem Text und Gesichtern die Suche nach Stellen in einem Video ermöglichen, in denen eine Person bestimmte Wörter benutzt hat oder zwei Personen zusammen gezeigt werden. Die Suche auf Grundlage solcher Erkenntnisse ist für Nachrichtenagenturen, Bildungseinrichtungen, Rundfunkanstalten, Besitzer von Unterhaltungsinhalten, Branchenanwendungen und im Allgemeinen für alle Branchen von Nutzen, die über eine Videobibliothek verfügen, in der Benutzer eine Suche durchführen müssen.

- Monetarisierung: Video Indexer kann Sie dabei unterstützen, den kommerziellen Wert von Videos zu verbessern. Beispielsweise können Unternehmen aus Branchen, die auf Werbeeinnahmen angewiesen sind (beispielsweise Nachrichtenmedien, soziale Medien usw.) wirkungsvollere Anzeigen schalten, indem sie die extrahierten Erkenntnisse als zusätzliche Signale für den Anzeigenserver nutzen (die Präsentation einer Sportschuhanzeige ist z. B. während eines Fußballspiels sinnvoller als bei einem Schwimmwettkampf).

- Kundenbindung: Die aus Videos gewonnenen Erkenntnisse können genutzt werden, um die Kundenbindung zu verbessern, indem für die Benutzer relevante Stellen in einem Video präsentiert werden. Angenommen, in einem Lehrvideo mit mathematischen Themen wird in den ersten 30 Minuten über Kugeln und in den nächsten 30 Minuten über Pyramiden gesprochen. Ein Schüler/Student, der Informationen zu Pyramiden sucht, würde mehr profitieren, wenn das Video an der 30-Minuten-Markierung positioniert wird.

Weitere Informationen finden Sie in diesem [Blog](http://aka.ms/videoindexerblog).

## <a name="next-steps"></a>Nächste Schritte

Sind Sie bereit für die ersten Schritte mit Video Indexer? Weitere Informationen finden Sie in den folgenden Artikeln:

- [Get started with the Video Indexer portal](video-indexer-get-started.md) (Erste Schritte mit dem Video Indexer-Portal)
- [Verarbeiten von Inhalten mit der Video Indexer-REST-API](video-indexer-use-apis.md)
- [Embed visual widgets in your application](video-indexer-embed-widgets.md) (Einbetten von visuellen Widgets in Ihre Anwendung)
