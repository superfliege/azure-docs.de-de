---
title: Extrahieren von Text mit optischer Zeichenerkennung (Optical Character Recognition, OCR) – Maschinelles Sehen
titleSuffix: Azure Cognitive Services
description: Konzepte im Zusammenhang mit dem Extrahieren von Text mittels optischer Zeichenerkennung (OCR) unter Verwendung der Maschinelles Sehen-API.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 02/11/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: deb73eb9fdd6879a5fbe1fed820bf92b2d627b65
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/15/2019
ms.locfileid: "56310439"
---
# <a name="extract-text-with-optical-character-recognition"></a>Extrahieren von Text mit optischer Zeichenerkennung

Die optische Zeichenerkennungstechnologie (OCR) der Funktion „Maschinellen Sehen“ erkennt Textinhalte in einem Bild und konvertiert den erkannten Text in einen computerlesbaren Zeichendatenstrom. Sie können das Ergebnis für die Suche und zahlreiche andere Zwecke wie Patientenakten, Sicherheit und im Bankwesen verwenden. 

OCR unterstützt 25 Sprachen: Arabisch, Chinesisch (traditionell), Chinesisch (vereinfacht), Dänisch, Deutsch, Englisch, Finnisch, Französisch, Griechisch, Italienisch, Japanisch, Koreanisch, Niederländisch, Norwegisch, Polnisch, Portugiesisch, Rumänisch, Russisch, Schwedisch, Serbisch (kyrillisch und lateinisch), Slowakisch, Spanisch, Tschechisch, Türkisch und Ungarisch. Die Sprache des Texts wird von OCR automatisch erkannt.

Bei Bedarf korrigiert die OCR die Drehung des erkannten Textes, indem sie den Drehversatz in Grad um die horizontale Bildachse dreht. OCR stellt außerdem, wie in der folgenden Abbildung gezeigt, die Framekoordinaten jedes Worts bereit.

![Diagramm, das zeigt, wie ein Bild gedreht und der darauf befindliche Text gelesen und eingegrenzt wird](./Images/vision-overview-ocr.png)

## <a name="image-requirements"></a>Anforderungen an Images

Maschinelles Sehen kann Text mithilfe von OCR aus Bildern extrahieren, die folgende Anforderungen erfüllen:

* Das Bild muss im JPEG-, PNG-, GIF- oder BMP-Format vorliegen.
* Die Größe des Eingabebilds muss zwischen 50 x 50 und 4200 x 4200 Pixel liegen.
* Der Text kann um ein beliebiges Vielfaches von 90 Grad plus einem kleinen Winkel von bis zu 40 Grad gedreht werden.

## <a name="improving-ocr-accuracy"></a>Verbesserung der OCR-Genauigkeit

Die Genauigkeit der Texterkennung hängt von der Qualität des Bilds ab. Ungenaue Lesevorgänge können durch Folgendes verursacht werden:

* Unscharfe Bilder
* Handschriftlicher oder kursiver Text
* Künstlerische Schriftarten
* Geringe Textgröße
* Komplexe Hintergründe, Schatten oder Blendung über dem Text oder eine perspektivische Verzerrung
* Übergroße oder fehlende Großbuchstaben am Anfang von Wörtern
* Tiefgestellter, hochgestellter oder durchgestrichener Text

### <a name="ocr-limitations"></a>OCR-Einschränkungen

Bei Bildern, auf denen Text dominiert, kann es durch teilweise erkannte Wörter zu falsch positiven Ergebnissen kommen. Bei einigen Bildern, insbesondere bei Fotos ohne Text, kann die Genauigkeit je nach Bildtyp sehr stark variieren.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in der [OCR-Referenzdokumentation](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc).
