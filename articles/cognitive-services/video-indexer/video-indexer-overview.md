---
title: Was ist Video Indexer?
titlesuffix: Azure Cognitive Services
description: Dieses Thema bietet eine Übersicht über den Video Indexer-Dienst.
services: cognitive services
author: juliako
manager: cgronlun
ms.service: cognitive-services
ms.component: video-indexer
ms.topic: overview
ms.date: 10/10/2018
ms.author: nolachar
ms.openlocfilehash: cf761342bb434100920c3888f2f441b9f8c8de08
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "49077916"
---
# <a name="what-is-video-indexer"></a>Was ist Video Indexer?

Microsoft Video Indexer ist eine Cloudanwendung, die auf Azure Media Analytics, Azure Search, Cognitive Services (beispielsweise Gesichtserkennungs-API, Microsoft Translator, Maschinelles Sehen-API und Custom Speech Service) basiert. Sie ermöglicht Ihnen, mithilfe der folgenden Video Indexer-Modelle Erkenntnisse aus Ihren Videos zu gewinnen:
 
- **Automatische Spracherkennung**: Die Lösung identifiziert automatisch die vorherrschend gesprochene Sprache. Unterstützt werden Chinesisch (vereinfacht), Deutsch, Englisch, Französisch, Italienisch, Japanisch, Portugiesisch (Brasilien), Spanisch und Russisch. Wenn die Sprache nicht erkannt werden kann, wird Englisch als Fallbacksprache verwendet.
- **Audiotranskription**: Wandelt Spracherkennung in 10 Sprachen um und lässt Erweiterungen zu. Unterstützt werden Arabisch, Chinesisch (vereinfacht), Deutsch, Englisch, Französisch, Italienisch, Japanisch, Portugiesisch (Brasilien), Spanisch und Russisch.
- **Untertitel für Hörgeschädigte**: Erstellt Untertitel in den drei Formaten VTT, TTML und SRT.
- **Verarbeitung von zwei Kanälen**: Erkennt automatisch ein getrenntes Transkript und sorgt für eine Zusammenführung auf einer einzelnen Zeitachse.
- **Rauschunterdrückung**: Bereinigt (basierend auf Skype-Filtern) Telefonieaudio oder verrauschte Aufnahmen.
- **Transkriptanpassung (Custom Recognition Intelligent Service, CRIS)**: Ermöglicht das Trainieren und Ausführen erweiterter benutzerdefinierter Spracherkennungsmodelle zur Erstellung branchenspezifischer Transkripte.
- **Sprecheraufzählung**: Kann erkennen und zuordnen, welcher Sprecher wann was gesagt hat.
- **Sprecherstatistik**: Bietet Statistiken zum Verhältnis zwischen Sprechern und ihrem Anteil an der Konversation.
- **Optische Zeichenerkennung (OCR)**: Extrahiert im Video angezeigten Text.
- **Keyframeextraktion**: Erkennt stabile Keyframes in einem Video.
- **Standpunktanalyse**: Erkennt anhand von Sprache und sichtbarem Text positive, negative und neutrale Stimmungen.
- **Moderation von Videoinhalten**: Erkennt jugendgefährdende und/oder anzügliche visuelle Elemente.
- **Extraktion von Stichwörtern**: Extrahiert Stichwörter aus Sprache und sichtbarem Text.
- **Erkennung von Bezeichnungen**: Identifiziert angezeigte visuelle Objekte und Aktionen.
- **Extraktion von Marken**: Extrahiert Marken aus Sprache und sichtbarem Text.
- **Gesichtserkennung**: Erkennt und gruppiert im Video gezeigte Gesichter.
- **Extraktion von Gesichtern als Miniaturbild („bestes Gesicht“)**: Identifiziert automatisch das am besten aufgenommene Gesicht in der jeweiligen Gruppe von Gesichtern (basierend auf Qualität, Größe und frontaler Position) und extrahiert es als Bildobjekt.
- **Prominentenerkennung**: Video Indexer kann automatisch mehr als 1 Million Prominente erkennen, z.B. wichtige Politiker/Politikerinnen, Schauspieler/Schauspielerinnen, Athleten/Athletinnen, Forscher/Forscherinnen und Führungskräfte aus dem Wirtschafts- und Technologiebereich. Die Daten zu diesen Prominenten sind auch auf verschiedenen bekannten Websites zu finden, z.B. IMDB und Wikipedia.
- **Kontobasierte Gesichtsidentifikation**: Video Indexer trainiert ein Modell für ein bestimmtes Konto. Gesichter im Video werden dann basierend auf dem Modell erkannt, das speziell für Videos dieses Kontos trainiert wurde.
- **Moderation von Textinhalten**: Erkennt im Transkript des Audios anstößigen Text.
- **Szenenwechselerkennung**: Erkennt, wenn sich im Video eine Szene ändert.
- **Erkennung schwarzer Frames**: Erkennt schwarze Frames im Video.
- **Audioeffekte**: Erkennt Audioeffekte wie Händeklatschen, Sprache und Stille.
- **Rückschluss auf Themen**: Zieht in den Transkripten einen Rückschluss auf Hauptthemen. Eine [IPTC](https://iptc.org/standards/media-topics/)-Taxonomie erster Ebene ist eingeschlossen.
- **Erkennung von Emotionen**: Erkennt Emotionen basierend auf Sprache und Audiosignalen. Bei den Emotionen kann es sich um Freude, Trauer, Wut oder Angst handeln.
- **Artefakte**: Extrahiert für jedes der Modelle eine umfangreiche Menge von Artefakten mit höherem Detailgrad.
- **Übersetzung**: Erstellt Übersetzungen des Audiotranskripts in 54 Sprachen.

Nachdem Video Indexer die Verarbeitung und Analyse abgeschlossen hat, können Sie die Erkenntnisse aus dem Video überprüfen, zusammenstellen, suchen und veröffentlichen.

Ganz gleich, ob Sie ein Inhaltsmanager oder ein Entwickler sind – der Video Indexer-Dienst kann all Ihren Anforderungen gerecht werden. Inhaltsverantwortliche können den Dienst über das Video Indexer-Webportal nutzen, ohne eine einzige Codezeile schreiben zu müssen. Weitere Informationen finden Sie unter [Tutorial: Registrieren und Hochladen Ihres ersten Videos](video-indexer-get-started.md). Entwickler können APIs zur bedarfsorientierten Verarbeitung von Inhalten nutzen. Weitere Informationen finden Sie unter [Verwenden der Video Indexer-REST-API](video-indexer-use-apis.md). Der Dienst bietet Kunden auch die Möglichkeit, mithilfe von Widgets Videodatenströme und extrahierte Erkenntnisse in ihren eigenen Anwendungen zu veröffentlichen. Weitere Informationen finden Sie unter [Embed visual widgets in your application](video-indexer-embed-widgets.md) (Einbetten von visuellen Widgets in Ihre Anwendung).

Sie können sich mit einem vorhandenen AAD-, LinkedIn-, Facebook-, Google- oder MSA-Konto für den Dienst registrieren. Weitere Informationen finden Sie unter [Getting Started](video-indexer-get-started.md) (Erste Schritte).

## <a name="scenarios"></a>Szenarien

Im Folgenden finden Sie einige Szenarien, in denen Video Indexer nützlich sein kann.

- Suche: Aus dem Video extrahierte Erkenntnisse können verwendet werden, um die Suchfunktion für eine Videobibliothek zu verbessern. Beispielsweise kann die Indizierung von gesprochenem Text und Gesichtern die Suche nach Stellen in einem Video ermöglichen, in denen eine Person bestimmte Wörter benutzt hat oder zwei Personen zusammen gezeigt werden. Die Suche auf Grundlage solcher Erkenntnisse ist für Nachrichtenagenturen, Bildungseinrichtungen, Rundfunkanstalten, Besitzer von Unterhaltungsinhalten, Branchenanwendungen und im Allgemeinen für alle Branchen von Nutzen, die über eine Videobibliothek verfügen, in der Benutzer eine Suche durchführen müssen.

- Monetarisierung: Video Indexer kann Sie dabei unterstützen, den kommerziellen Wert von Videos zu verbessern. Beispielsweise können Unternehmen aus Branchen, die auf Werbeeinnahmen angewiesen sind (beispielsweise Nachrichtenmedien, soziale Medien usw.) wirkungsvollere Anzeigen schalten, indem sie die extrahierten Erkenntnisse als zusätzliche Signale für den Anzeigenserver nutzen (die Präsentation einer Sportschuhanzeige ist z. B. während eines Fußballspiels sinnvoller als bei einem Schwimmwettkampf).

- Kundenbindung: Die aus Videos gewonnenen Erkenntnisse können genutzt werden, um die Kundenbindung zu verbessern, indem für die Benutzer relevante Stellen in einem Video präsentiert werden. Angenommen, in einem Lehrvideo mit mathematischen Themen wird in den ersten 30 Minuten über Kugeln und in den nächsten 30 Minuten über Pyramiden gesprochen. Ein Schüler/Student, der Informationen zu Pyramiden sucht, würde mehr profitieren, wenn das Video an der 30-Minuten-Markierung positioniert wird.

Weitere Informationen finden Sie in diesem [Blog](http://aka.ms/videoindexerblog).

## <a name="next-steps"></a>Nächste Schritte

Sind Sie bereit für die ersten Schritte mit Video Indexer? Weitere Informationen finden Sie in den folgenden Artikeln:

- [Tutorial: Registrieren und Hochladen Ihres ersten Videos](video-indexer-get-started.md)
- [Verarbeiten von Inhalten mit der Video Indexer-REST-API](video-indexer-use-apis.md)
- [Einbetten von visuellen Widgets in Ihre Anwendung](video-indexer-embed-widgets.md)
