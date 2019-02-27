---
title: Erkennen von gedrucktem und handschriftlichem Text – maschinelles Sehen
titleSuffix: Azure Cognitive Services
description: Konzepte zur Erkennung von gedrucktem und handschriftlichem Text in Bildern mithilfe der Maschinelles Sehen-API.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 48ce15a11c3e3282535420f3e1bb1915276d70f5
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/15/2019
ms.locfileid: "56313176"
---
# <a name="recognizing-printed-and-handwritten-text"></a>Erkennen von gedrucktem und handschriftlichem Text

Maschinelles Sehen kann gedruckten und handschriftlichen Text in Bildern von verschiedensten Objekten mit unterschiedlichen Oberflächen und Hintergründen erkennen. Hierzu zählen z. B. Belege, Poster, Visitenkarten, Briefe und Whiteboards.

Die Texterkennungsfunktion ist der [optischen Zeichenerkennung (OCR)](concept-extracting-text-ocr.md) sehr ähnlich, erfolgt aber im Gegensatz zur OCR asynchron und verwendet aktualisierte Erkennungsmodelle.

> [!NOTE]
> Diese Technologie befindet sich derzeit in der Vorschau und ist nur für englischsprachigen Text verfügbar.

## <a name="text-recognition-requirements"></a>Anforderungen an die Texterkennung

Das maschinelle Sehen kann gedruckten und handschriftlichen Text in Bildern erkennen, die die folgenden Anforderungen erfüllen:

- Das Bild muss im JPEG-, PNG- oder BMP-Format vorliegen.
- Die Dateigröße muss weniger als 4 MB betragen.
- Die Größe des Bilds muss zwischen 50 x 50 und 4200 x 4200 Pixel liegen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in der [Referenzdokumentation zur Texterkennung](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200).