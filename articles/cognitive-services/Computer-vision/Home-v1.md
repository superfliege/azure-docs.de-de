---
title: Was ist die Maschinelles Sehen-API?
titlesuffix: Azure Cognitive Services
description: Über die Maschinelles Sehen-API haben Entwickler Zugriff auf erweiterte Algorithmen für die Bildverarbeitung und die Rückgabe von Informationen.
services: cognitive-services
author: KellyDF
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: overview
ms.date: 08/10/2017
ms.author: kefre
ms.custom: seodec18
ms.openlocfilehash: 35002ccfc294b59a13e6826b4ca75a1ab6c68b62
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55870298"
---
# <a name="what-is-computer-vision-api-version-10"></a>Worum handelt es sich bei der Maschinelles Sehen-API Version 1.0?

> [!IMPORTANT]
> Es ist nun eine neue Version der Maschinelles Sehen-API verfügbar. Informationen dazu finden Sie unter:
>- [Übersicht](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home)
>- [Maschinelles Sehen-API Version 2.0](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)

Über die cloudbasierte Maschinelles Sehen-API haben Entwickler Zugriff auf erweiterte Algorithmen zum Verarbeiten von Bildern und zum Zurückgeben von Informationen. Durch das Hochladen eines Bilds oder das Angeben einer Bild-URL können Microsoft-Algorithmen für maschinelles Sehen visuelle Inhalte basierend auf Eingaben und der Benutzerauswahl auf unterschiedliche Arten analysieren. Mit der Maschinelles Sehen-API können Benutzer Bilder zu folgenden Zwecken analysieren:
* Taggen von Bildern basierend auf dem Inhalt
* Kategorisieren von Bildern
* Identifizieren des Typs und der Qualität von Bildern
* [Erkennen menschlicher Gesichter und Zurückgeben ihrer Koordinaten ](#Faces)
* Erkennen domänenspezifischer Inhalte
* Generieren von Beschreibungen der Inhalte
* Verwenden der optischen Zeichenerkennung zum Identifizieren von gedrucktem Text in Bildern
* Erkennen von handschriftlichem Text
* Unterscheiden von Farbschemas
* Kennzeichnen nicht jugendfreier Inhalte
* Zuschneiden von Fotos zur Verwendung als Miniaturbilder

## <a name="requirements"></a>Requirements (Anforderungen)
* Unterstützte Eingabemethoden: Rohe Bildbinärdaten in Form einer Anwendung/eines Oktettdatenstroms oder einer Bild-URL
* Unterstützte Bildformate: JPEG, PNG, GIF, BMP
* Bilddateigröße: Maximal 4 MB
* Bildgröße: Mindestens 50 x 50 Pixel

## <a name="tagging-images"></a>Taggen von Bildern
Die Maschinelles Sehen-API gibt Tags basierend auf Tausenden erkennbaren Objekten, Lebewesen, Landschaften und Aktionen zurück. Wenn Tags nicht eindeutig sind oder nicht zum Allgemeinwissen gehören, enthält die API-Antwort „Hinweise“, um die Bedeutung des Tags in einem bekannten Kontext zu erläutern. Tags sind nicht als eine Taxonomie organisiert, und es sind keine Vererbungshierarchien vorhanden. Eine Auflistung von Inhaltstags bildet die Grundlage für eine „Bildbeschreibung“, die in einer für Menschen lesbaren Sprache in vollständigen Sätzen angezeigt wird. Beachten Sie, dass derzeit nur Englisch als Sprache für Bildbeschreibungen unterstützt wird.

Nachdem Sie ein Bild hochgeladen oder eine Bild-URL angegeben haben, geben die Algorithmen der Maschinelles Sehen-API Tags basierend auf den Objekten, Lebewesen und Aktionen aus, die im Bild erkannt werden. Das Tagging ist nicht auf den Hauptinhalt (z.B. eine Person im Vordergrund) beschränkt, sondern umfasst auch die Umgebung (Innen- oder Außenbereich), Möbel, Werkzeuge, Pflanzen, Tiere, Zubehör, Geräte usw.

### <a name="example"></a>Beispiel
![House_Yard](./Images/house_yard.png) '

```json
Returned Json
{
   'tags':[
      {
         "name":"grass",
         "confidence":0.999999761581421
      },
      {
         "name":"outdoor",
         "confidence":0.999970674514771
      },
      {
         "name":"sky",
         "confidence":999289751052856
      },
      {
         "name":"building",
         "confidence":0.996463239192963
      },
      {
         "name":"house",
         "confidence":0.992798030376434
      },
      {
         "name":"lawn",
         "confidence":0.822680294513702
      },
      {
         "name":"green",
         "confidence":0.641222536563873
      },
      {
         "name":"residential",
         "confidence":0.314032256603241
      },
   ],
}
```
## <a name="categorizing-images"></a>Kategorisieren von Bildern
Zusätzlich zu Tagging und Beschreibungen gibt die Maschinelles Sehen-API auf der Taxonomie basierende Kategorien zurück, die in früheren Versionen definiert wurden. Diese Kategorien sind als eine Taxonomie mit über-/untergeordneten vererbbaren Hierarchien organisiert. Alle Kategorien sind in Englisch. Sie können allein oder mit unseren neuen Modellen verwendet werden.

### <a name="the-86-category-concept"></a>Das 86-Kategorien-Konzept
Basierend auf einer Liste mit 86 Kategorien, die in der folgenden Abbildung dargestellt sind, können visuelle Merkmale, die in einem Bild gefunden werden, in allgemeine bis spezifische Kategorien eingeordnet werden. Die vollständige Taxonomie im Textformat finden Sie unter [Kategorietaxonomie](https://docs.microsoft.com/azure/cognitive-services/computer-vision/category-taxonomy).

![Analysieren von Kategorien](./Images/analyze_categories.png)

Image                                                  | response
------------------------------------------------------ | ----------------
![Woman Roof](./Images/woman_roof.png)                 | people
![Family Photo](./Images/family_photo.png)             | people_crowd
![Cute Dog](./Images/cute_dog.png)                     | animal_dog
![Outdoor Mountain](./Images/mountain_vista.png)       | outdoor_mountain
![Bildanalyse: Food Bread](./Images/bread.png)       | food_bread

## <a name="identifying-image-types"></a>Identifizieren von Bildtypen
Bilder können auf verschiedene Arten kategorisiert werden. Die Maschinelles Sehen-API kann ein boolesches Flag setzen, um anzugeben, ob ein Bild in Schwarzweiß oder Farbe vorliegt. Die API kann auch ein Flag setzen, um anzugeben, ob es sich bei einem Bild um eine Strichzeichnung handelt oder nicht. Zudem kann sie angeben, ob es sich bei einem Bild um ClipArt handelt oder nicht, und deren Qualität auf einer Skala von 0 bis 3 benennen.

### <a name="clip-art-type"></a>ClipArt-Typ
Erkennt, ob es sich bei einem Bild um ClipArt handelt oder nicht.  

Wert | Bedeutung
----- | --------------
0     | Keine ClipArt
1     | Nicht eindeutig
2     | Normale ClipArt
3     | Gute ClipArt

Image|response
----|----
![Bildanalyse: Cheese Clip Art](./Images/cheese_clipart.png)|3 good-clip-art
![Bildanalyse: House Yard](./Images/house_yard.png)|0 Non-clip-art

### <a name="line-drawing-type"></a>Strichzeichnungstyp
Erkennt, ob es sich bei einem Bild um eine Strichzeichnung handelt oder nicht.

Image|response
----|----
![Bildanalyse: Lion Drawing](./Images/lion_drawing.png)|True
![Bildanalyse: Flower](./Images/flower.png)|False

### <a name="faces"></a>Gesichtserkennung
Erkennt menschliche Gesichter in einem Bild und generiert die Gesichtskoordinaten, das Rechteck für das Gesicht, Geschlecht und Alter. Diese visuellen Merkmale sind eine Teilmenge der für die Gesichtserkennung generierten Metadaten. Zum Generieren umfassenderer Metadaten für die Gesichtserkennung (Gesichtsausdruck, Kopfhaltung usw.) verwenden Sie die Gesichtserkennungs-API.  

Image|response
----|----
![Bildanalyse: Woman Roof Face](./Images/woman_roof_face.png) | [ { "age": 23, "gender": "Female", "faceRectangle": { "left": 1379, "top": 320, "width": 310, "height": 310 } } ]
![Bildanalyse: Mom Daughter Face](./Images/mom_daughter_face.png) | [ { "age": 28, "gender": "Female", "faceRectangle": { "left": 447, "top": 195, "width": 162, "height": 162 } }, { "age": 10, "gender": "Male", "faceRectangle": { "left": 355, "top": 87, "width": 143, "height": 143 } } ]
![Bildanalyse: Family Photo Face](./Images/family_photo_face.png) | [ { "age": 11, "gender": "Male", "faceRectangle": { "left": 113, "top": 314, "width": 222, "height": 222 } }, { "age": 11, "gender": "Female", "faceRectangle": { "left": 1200, "top": 632, "width": 215, "height": 215 } }, { "age": 41, "gender": "Male", "faceRectangle": { "left": 514, "top": 223, "width": 205, "height": 205 } }, { "age": 37, "gender": "Female", "faceRectangle": { "left": 1008, "top": 277, "width": 201, "height": 201 } } ]


## <a name="domain-specific-content"></a>Domänenspezifischer Inhalt

Außer dem Tagging und der Kategorisierung auf oberster Ebene unterstützt die Maschinelles Sehen-API auch spezielle (oder domänenspezifische) Informationen. Spezielle Informationen kann als eigenständige Methode oder mit der allgemeinen Kategorisierung implementiert werden. Sie ermöglichen eine weitere Optimierung der 86-Kategorien-Taxonomie durch das Hinzufügen domänenspezifischer Modelle.

Die einzigen speziellen Informationen, die derzeit unterstützt werden, sind die Erkennung berühmter Personen und die Erkennung von Wahrzeichen. Dies sind domänenspezifische Verfeinerungen der Kategorien für Personen und Personengruppen sowie Wahrzeichen weltweit.

Es gibt zwei Optionen für die Verwendung der domänenspezifischen Modelle:

### <a name="option-one---scoped-analysis"></a>Option 1: Bereichsbezogene Analyse
Analyse nur eines ausgewählten Modells durch einen HTTP POST-Aufruf. Wenn Sie wissen, welches Modell Sie verwenden möchten, geben Sie den Namen des Modells an, und Sie erhalten nur Informationen, die für dieses Modell relevant sind. Sie können diese Option beispielsweise verwenden, um nur nach berühmten Personen zu suchen. Die Antwort enthält eine Liste möglicher Übereinstimmungen mit berühmten Personen zusammen mit der jeweiligen Zuverlässigkeitsbewertung.

### <a name="option-two---enhanced-analysis"></a>Option 2: Erweiterte Analyse
Analyse zur Bereitstellung zusätzlicher Details im Zusammenhang mit Kategorien der 86-Kategorien-Taxonomie. Diese Option steht zur Verwendung in Anwendungen bereit, bei denen Benutzer zusätzlich zu den Details aus einem oder mehreren domänenspezifischen Modellen auch eine generische Bildanalyse erhalten möchten. Bei Aufruf dieser Methode wird zuerst die Klassifizierung der 86-Kategorien-Taxonomie aufgerufen. Wenn eine der Kategorien mit der eines bekannten/übereinstimmenden Modells übereinstimmt, wird die Klassifizierung ein zweites Mal aufgerufen. Wenn beispielsweise „details=all“ angegeben ist oder „details“ die Angabe „celebrities“ enthält, ruft die Methode nach Aufruf der 86-Kategorien-Klassifizierung die Klassifizierung für berühmte Personen auf. Das Ergebnis enthält Tags, die mit „people_“ beginnen.

## <a name="generating-descriptions"></a>Generieren von Beschreibungen 
Algorithmen der Maschinelles Sehen-API analysieren den Inhalt eines Bilds. Diese Analyse bildet die Grundlage für eine „Beschreibung“, die in einer für Menschen lesbaren Sprache in vollständigen Sätzen angezeigt wird. Die Beschreibung ist eine Zusammenfassung dessen, was im Bild gefunden wird. Algorithmen der Maschinelles Sehen-API generieren verschiedene Beschreibungen auf Grundlage der im Bild erkannten Objekte. Die Beschreibungen werden jeweils ausgewertet, und eine Zuverlässigkeitsbewertung wird generiert. Dann wird eine Liste in der Reihenfolge von höchster Zuverlässigkeitsbewertung zu niedrigster zurückgegeben. Ein Beispiel für einen Bot, der diese Technologie zum Generieren von Bildbeschriftungen verwendet, finden Sie [hier](https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/intelligence-ImageCaption).  

### <a name="example-description-generation"></a>Beispiel für eine Beschreibungsgenerierung
![Schwarzweiße Gebäude](./Images/bw_buildings.png) '
```json
 Returned Json

'description':{
   "captions":[
      {
         "type":"phrase",
         'text':'a black and white photo of a large city',
         'confidence':0.607638706850331
      }
   ]   
   "captions":[
      {
         "type":"phrase",
         'text':'a photo of a large city',
         'confidence':0.577256764264197
      }
   ]   
   "captions":[
      {
         "type":"phrase",
         'text':'a black and white photo of a city',
         'confidence':0.538493271791207
      }
   ]   
   'description':[
      "tags":{
         "outdoor",
         "city",
         "building",
         "photo",
         "large",
      }
   ]
}
```

## <a name="perceiving-color-schemes"></a>Erkennen von Farbschemas
Der Maschinelles Sehen-Algorithmus extrahiert Farben aus einem Bild. Die Farben werden in drei verschiedenen Kontexten analysiert: Vordergrund, Hintergrund und Gesamtbild. Sie werden in 12 vorherrschende Akzentfarben gruppiert. Diese Akzentfarben sind Schwarz, Blau, Brown, Grau, Grün, Orange, Rosa, Violett, Rot, Aquamarin, Weiß und Gelb. Je nach Farben in einem Bild können einfach Schwarz und Weiß oder Akzentfarben in hexadezimalen Farbcodes zurückgegeben werden.

Image                                                       | Vordergrund |Hintergrund| Farben
----------------------------------------------------------- | --------- | ------- | ------
![Outdoor Mountain](./Images/mountain_vista.png)            | Schwarz     | Schwarz   | Weiß
![Bildanalyse: Flower](./Images/flower.png)               | Schwarz     | Weiß   | Weiß, Schwarz, Grün
![Bildanalyse: Train Station](./Images/train_station.png) | Schwarz     | Schwarz   | Schwarz

### <a name="accent-color"></a>Akzentfarbe
Farbe, die aus einem Bild extrahiert wurde und die für die Benutzer am stärksten ins Auge fallende Farbe durch eine Mischung von vorherrschenden Farben und Sättigung darstellen soll.

Image                                                       | response
----------------------------------------------------------- | ----
![Outdoor Mountain](./Images/mountain_vista.png)            | #BC6F0F
![Bildanalyse: Flower](./Images/flower.png)               | #CAA501
![Bildanalyse: Train Station](./Images/train_station.png) | #484B83


### <a name="black--white"></a>Schwarzweiß
Boolesches Flag, das angibt, ob ein Bild schwarzweiß ist oder nicht.

Image                                                      | response
---------------------------------------------------------- | ----
![Bildanalyse: Building](./Images/bw_buildings.png)      | True
![Bildanalyse: House Yard](./Images/house_yard.png)      | False

## <a name="flagging-adult-content"></a>Kennzeichnen nicht jugendfreier Inhalte
Die verschiedenen visuellen Kategorien umfassen auch die Gruppe der nicht jugendfreien und anzüglichen Inhalte, die eine Erkennung solcher jugendgefährdenden Inhalte ermöglicht und die Anzeige von Bildern mit sexuellen Inhalten einschränkt. Der Filter zur Erkennung nicht jugendfreier und anzüglicher Inhalte kann durch einen Schieberegler entsprechend den Anforderungen des Benutzers eingestellt werden.

## <a name="optical-character-recognition-ocr"></a>Optische Zeichenerkennung (OCR)
Mithilfe der OCR-Technologie wird Textinhalt in einem Bild erkannt und der erkannte Text in eine computerlesbare Zeichenfolge extrahiert. Sie können das Ergebnis für die Suche und zahlreiche andere Zwecke wie medizinische Aufzeichnungen, Sicherheit und das Bankwesen verwenden. Die Sprache wird automatisch erkannt. OCR spart Zeit und erleichtert Benutzern das Leben, da sie den Text nicht mehr transkribieren müssen, sondern ihn fotografieren können.

OCR unterstützt 25 Sprachen. Diese Sprachen sind: Arabisch, Chinesisch (traditionell), Chinesisch (vereinfacht), Dänisch, Deutsch, Englisch, Finnisch, Französisch, Griechisch, Italienisch, Japanisch, Koreanisch, Niederländisch, Norwegisch, Polnisch, Portugiesisch, Rumänisch, Russisch, Schwedisch, Serbisch (kyrillisch und lateinisch), Slowakisch, Spanisch, Tschechisch, Türkisch und Ungarisch.

Bei Bedarf korrigiert OCR die Drehung des erkannten Texts in Grad um die horizontale Bildachse. OCR stellt die Frame-Koordinaten jedes Worts bereit, wie es in der folgenden Abbildung dargestellt ist.

![Übersicht über OCR](./Images/vision-overview-ocr.png) Anforderungen für OCR:
- Die Größe des Eingabebilds muss zwischen 40 x 40 und 3200 x 3200 Pixel liegen.
- Das Bild darf nicht größer als 10 Megapixel sein.

Das Eingabebild kann um ein Vielfaches von 90 Grad plus einem kleinen Winkel von bis zu 40 Grad gedreht werden.

Die Genauigkeit der Texterkennung hängt von der Qualität des Bilds ab. Ein ungenaues Lesen kann durch Folgendes verursacht werden:
- Unscharfe Bilder
- Handschriftlicher oder kursiver Text
- Künstlerische Schriftarten
- Geringe Textgröße
- Komplexe Hintergründe, Schatten oder Blendung über dem Text oder eine perspektivische Verzerrung
- Übergroße oder fehlende Großbuchstaben am Anfang von Wörtern
- Tiefgestellter, hochgestellter oder durchgestrichener Text

 Einschränkungen: Bei Fotos, auf denen Text dominiert, kann es durch teilweise erkannte Wörter zu falsch positiven Ergebnissen kommen. Bei einigen Fotos, insbesondere bei Fotos ohne Text, kann die Genauigkeit je nach Bildtyp sehr stark variieren.

## <a name="recognize-handwritten-text"></a>Erkennen von handschriftlichem Text
Diese Technologie ermöglicht es Ihnen, handschriftlichen Text in Notizen, Briefen, Abhandlungen, Tafelbildern, Formularen usw. zu erkennen und zu extrahieren. Sie funktioniert mit verschiedenen Oberflächen und Hintergründen, z.B. weißem Papier, gelben Haftnotizen oder Tafeln.

Die Erkennung von handschriftlichem Text spart Zeit und Mühe und kann Ihre Produktivität steigern, indem Sie Bilder von Text aufnehmen können, anstatt diesen übertragen zu müssen. Mithilfe dieser Technologie können Notizen digitalisiert werden. Diese Digitalisierung ermöglicht das Implementieren schneller und einfacher Suchen. Außerdem wird die Papiermenge reduziert.

Eingabeanforderungen:
- Unterstützte Bildformate: JPEG, PNG und BMP
- Die Bilddateigröße muss weniger als 4 MB betragen.
- Die Bildgröße muss mindestens 40 x 40 Pixel und darf höchstens 3200 x 3200 Pixel betragen.

Hinweis: Diese Technologie befindet sich derzeit in der Vorschau und ist nur für englischsprachigen Text verfügbar.

## <a name="generating-thumbnails"></a>Generieren von Miniaturbildern
Ein Miniaturbild ist eine kleine Darstellung eines Bilds. Verschiedene Geräte wie Smartphones, Tablets und PCs machen verschiedene Layouts der Benutzeroberfläche sowie unterschiedliche Miniaturbildgrößen erforderlich. Durch intelligente Zuschneidefunktionen bietet diese Maschinelles Sehen-API Hilfe bei der Lösung dieses Problems.

Nach dem Hochladen eines Bilds wird ein qualitativ hochwertiges Miniaturbild generiert, und der Algorithmus der Maschinelles Sehen-API analysiert die Objekte im Bild. Anschließend wird das Bild auf den relevanten Bereich zugeschnitten. Die Ausgabe wird in einem speziellen Rahmen angezeigt, wie es in der folgenden Abbildung dargestellt ist. Das generierte Miniaturbild kann mit einem anderen Seitenverhältnis als dem des ursprünglichen Bilds angezeigt werden, um so den Anforderungen des Benutzers zu entsprechen.

Der Miniaturbildalgorithmus funktioniert wie folgt:

1. Störende Elemente werden aus dem Bild entfernt, und das Hauptobjekt (der relevante Bereich) wird erkannt.
2. Das Bild wird auf Grundlage des erkannten relevanten Bereichs zugeschnitten.
3. Das Seitenverhältnis wird entsprechend den Abmessungen des Zielminiaturbilds geändert.

![Miniaturbilder](./Images/thumbnail-demo.png)
