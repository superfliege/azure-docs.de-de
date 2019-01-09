---
title: Was ist die Maschinelles Sehen-API? Maschinelles Sehen
titlesuffix: Azure Cognitive Services
description: Über den Dienst für maschinelles Sehen haben Entwickler Zugriff auf erweiterte Algorithmen für die Bildverarbeitung und die Rückgabe von Informationen.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: overview
ms.date: 08/22/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 4c952c847aa56b385065a878b664b3d1d98763f4
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/18/2018
ms.locfileid: "53584019"
---
# <a name="what-is-computer-vision"></a>Worum handelt es sich bei maschinellem Sehen?

Über den cloudbasierten Dienst für maschinelles Sehen haben Entwickler Zugriff auf erweiterte Algorithmen für die Bildverarbeitung und die Rückgabe von Informationen. Maschinelles Sehen ist mit gängigen Bildformaten wie JPEG und PNG kompatibel. Für die Bildanalyse können Sie entweder ein Bild hochladen oder eine Bild-URL angeben. Algorithmen für maschinelles Sehen können den Inhalt eines Bilds auf unterschiedliche Weise analysieren – je nachdem, für welche visuellen Merkmale Sie sich interessieren. So kann maschinelles Sehen beispielsweise erkennen, ob ein Bild anzügliche oder nicht jugendfreie Inhalte enthält, oder alle Gesichter in einem Bild suchen.

Wenn Sie maschinelles Sehen in Ihrer Anwendung verwenden möchten, können Sie den Dienst entweder unter Verwendung unserer [Clientbibliotheken](quickstarts-sdk/csharp-analyze-sdk.md) aufrufen oder direkt die [REST-API](vision-api-how-to-topics/howtocallvisionapi.md) aufrufen. Maschinelles Sehen ermöglicht Folgendes:

- [Analysieren von Bildern, um Erkenntnisse zu gewinnen](#analyzing-images-for-insight)
- [Extrahieren von Text aus Bildern](#extracting-text-from-images)
- [Moderieren von Bildinhalten](#moderating-content-in-images)

## <a name="analyzing-images-for-insight"></a>Analysieren von Bildern, um Erkenntnisse zu gewinnen

Sie können Bilder mithilfe von maschinellem Sehen analysieren, um Erkenntnisse zu visuellen Merkmalen und Eigenschaften Ihrer Bilder zu gewinnen und bereitzustellen. Sie können entweder den Inhalt eines Bilds hochladen, um lokale Bilder zu analysieren, oder die URL eines Bilds angeben, um Remotebilder zu analysieren.

Maschinelles Sehen bietet folgende Analyseaktionen:

| Aktion | BESCHREIBUNG |
| ------ | ----------- |
|**[Markieren visueller Merkmale](concept-tagging-images.md)**|Erkennen und markieren Sie visuelle Merkmale in einem Bild auf der Grundlage von Tausenden von erkennbaren Objekten, Lebewesen, Landschaften und Aktionen. Wenn die Tags nicht eindeutig sind oder nicht zum Allgemeinwissen gehören, enthält die API-Antwort „Hinweise“, um die Bedeutung des Tags in einem bekannten Kontext zu erläutern. Die Markierung ist nicht auf den Hauptinhalt (etwa eine Person im Vordergrund) beschränkt, sondern bezieht auch die Umgebung (Innen- oder Außenbereich), Möbel, Werkzeuge, Pflanzen, Tiere, Zubehör, Geräte und Ähnliches mit ein.|
|**[Erkennen von Objekten](concept-object-detection.md)**| Die Objekterkennung ähnelt dem Tagging, die API gibt jedoch die Koordinaten des Begrenzungsrahmens für jedes angewendete Tag zurück. Wenn ein Bild z. B. einen Hund, eine Katze sowie eine Person enthält, werden diese Objekte bei der Erkennung zusammen mit ihren Koordinaten im Bild aufgelistet. Sie können diese Funktion verwenden, um weitere Beziehungen zwischen den Objekten in einem Bild zu verarbeiten. Außerdem können Sie feststellen, ob mehrere Instanzen des gleichen Tags in einem Bild enthalten sind.|
|**[Kategorisieren eines Bilds](concept-categorizing-images.md)**|Erkennen und kategorisieren Sie ein gesamtes Bild unter Verwendung einer [Kategorietaxonomie](Category-Taxonomy.md) mit über-/untergeordneten vererbbaren Hierarchien. Kategorien können einzeln oder in Kombination mit unseren neuen Markierungsmodellen verwendet werden.<br/>Als Sprache für die Markierung und Kategorisierung von Bildern wird derzeit nur Englisch unterstützt.|
|**[Beschreiben eines Bilds](concept-describing-images.md)**|Generieren Sie eine Beschreibung eines gesamten Bilds mit vollständigen Sätzen in lesbarer Sprache. Algorithmen für maschinelles Sehen generieren verschiedene Beschreibungen auf der Grundlage der im Bild erkannten Objekte. Die Beschreibungen werden jeweils ausgewertet, und eine Zuverlässigkeitsbewertung wird generiert. Dann wird eine Liste in der Reihenfolge von höchster Zuverlässigkeitsbewertung zu niedrigster zurückgegeben.|
|**[Erkennen von Gesichtern](concept-detecting-faces.md)** |Erkennen Sie Gesichter in einem Bild, und stellen Sie Informationen zu den einzelnen Gesichtern bereit. Maschinelles Sehen gibt für jedes erkannte Gesicht die Koordinaten, ein Rechteck, das Geschlecht und das Alter zurück.<br/>Maschinelles Sehen bietet einen Teil der Funktionen aus der [Gesichtserkennung](/azure/cognitive-services/face/). Der Gesichtserkennungsdienst ermöglicht eine detailliertere Analyse zur Erkennung von Gesichtsausdruck, Kopfhaltung und Ähnlichem.|
|**[Erkennen von Bildtypen](concept-detecting-image-types.md)**|Erkennen Sie Merkmale eines Bilds – also beispielsweise, ob es sich bei dem Bild um eine Strichzeichnung handelt oder wie wahrscheinlich es ist, dass es sich bei dem Bild um ein ClipArt handelt.|
|**[Erkennen domänenspezifischer Inhalte](concept-detecting-domain-content.md)**|Verwenden Sie Domänenmodelle, um domänenspezifische Inhalte (etwa berühmte Personen und Orientierungspunkte) in einem Bild zu erkennen. Wenn ein Bild also beispielsweise Personen enthält, kann maschinelles Sehen auf ein im Dienst enthaltenes Domänenmodell für berühmte Personen zurückgreifen und so ermitteln, ob es sich bei den Personen auf dem Bild um berühmte Personen handelt.|
|**[Erkennen des Farbschemas](concept-detecting-color-schemes.md)**|Analysieren Sie die Farben eines Bilds. Maschinelles Sehen kann ermitteln, ob es sich um ein Schwarzweißbild oder um ein Farbbild handelt. Bei Farbbildern kann maschinelles Sehen außerdem die dominante Farbe sowie Akzentfarben erkennen.|
|**[Generieren einer Miniaturansicht](concept-generating-thumbnails.md)**|Analysieren Sie den Inhalt eines Bilds, um eine geeignete Miniaturansicht für das Bild zu generieren. Die Maschinelles Sehen-API generiert zunächst eine hochwertige Miniaturansicht und analysiert dann die Objekte im Bild, um den *relevanten Bereich* zu bestimmen. Anschließend wird das Bild auf den relevanten Bereich zugeschnitten. Das Seitenverhältnis der generierten Miniaturansicht kann sich bei Bedarf vom Seitenverhältnis des ursprünglichen Bilds unterscheiden.|
|**[Abrufen des relevanten Bereichs](concept-generating-thumbnails.md#area-of-interest)**|Analysieren Sie den Inhalt eines Bilds, um die Koordinaten des *relevanten Bereichs* zurückzugeben. Hierbei handelt es sich um die gleiche Funktion, die auch zum Generieren einer Miniaturansicht verwendet wird. Anstatt das Bild zuzuschneiden, gibt die Maschinelles Sehen-API jedoch die Koordinaten des Begrenzungsrahmens des Bereichs zurück, sodass die aufrufende Anwendung das ursprüngliche Bild nach Bedarf ändern kann.|

## <a name="extracting-text-from-images"></a>Extrahieren von Text aus Bildern

Mit maschinellem Sehen können Sie [Text unter Verwendung von optischer Zeichenerkennung aus einem Bild in eine computerlesbare Zeichenfolge extrahieren](concept-extracting-text-ocr.md). Bei Bedarf korrigiert die optische Zeichenerkennung die Drehung des erkannten Texts (in Grad) um die horizontale Bildachse und liefert die Frame-Koordinaten der einzelnen Wörter. Die optische Zeichenerkennung unterstützt 25 Sprachen und erkennt automatisch die Sprache des extrahierten Texts.

Sie können auch [gedruckten und handschriftlichen Text in einem Bild erkennen](concept-recognizing-text.md). Maschinelles Sehen kann sowohl gedruckten als auch handschriftlichen Text in Bildern von verschiedensten Objekten mit unterschiedlichen Oberflächen und Hintergründen erkennen. Hierzu zählen beispielsweise Belege, Poster, Visitenkarten, Briefe und Whiteboards. Die Erkennung gedruckter und handschriftlicher Texte befindet sich derzeit in der Vorschauphase und wird nur für Englisch unterstützt.  

## <a name="moderating-content-in-images"></a>Moderieren von Bildinhalten

Maschinelles Sehen kann [anzügliche und nicht jugendfreie Inhalte in einem Bild erkennen](concept-detecting-adult-content.md). Dabei wird bewertet, mit welcher Wahrscheinlichkeit das Bild anzügliche oder nicht jugendfreie Inhalte enthält, und eine Zuverlässigkeitsbewertung für beides generiert. Der Filter für die Erkennung anzüglicher und nicht jugendfreier Inhalte kann mithilfe eines Schiebereglers nach Bedarf angepasst werden.

## <a name="using-containers"></a>Verwenden von Containern

[Verwenden Sie Container für maschinelles Sehen](computer-vision-how-to-install-containers.md), um gedruckten und handschriftlichen Text lokal zu erkennen. Installieren Sie dazu einen standardisierten Docker-Container, der sich näher bei Ihren Daten befindet.

## <a name="image-requirements"></a>Anforderungen an Images

Maschinelles Sehen kann Bilder analysieren, die folgende Anforderungen erfüllen:

- Das Bild muss im JPEG-, PNG-, GIF- oder BMP-Format vorliegen.
- Die Dateigröße muss weniger als 4 MB betragen.
- Das Bild muss größer als 50 x 50 Pixel sein.  
  Für OCR muss die Größe des Bilds zwischen 50 x 50 und 4.200 x 4.200 Pixel liegen.

## <a name="next-steps"></a>Nächste Schritte

Schnellstartanleitungen für die ersten Schritte mit maschinellem Sehen:

- [Analysieren eines Bilds](quickstarts-sdk/csharp-analyze-sdk.md)
- [Extrahieren von handschriftlichem Text](quickstarts-sdk/csharp-hand-text-sdk.md)
- [Generieren einer Miniaturansicht](quickstarts-sdk/csharp-thumb-sdk.md)
