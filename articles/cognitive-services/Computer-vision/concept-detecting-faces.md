---
title: Erkennen von Gesichtern – Maschinelles Sehen
titleSuffix: Azure Cognitive Services
description: Machen Sie sich mit Konzepten des Gesichtserkennungsfeatures der Maschinelles Sehen-API vertraut.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 0c6485bff4ad11aab37139cd2aa2d3660bddac0e
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/18/2018
ms.locfileid: "53580535"
---
# <a name="face-detection-with-computer-vision"></a>Gesichtserkennung mit maschinellem Sehen

Das maschinelle Sehen erkennt menschliche Gesichter auf einem Bild und generiert Alter, Geschlecht und Rechteck für jedes erkannte Gesicht. Maschinelles Sehen bietet einen Teil der Funktionen aus der [Gesichtserkennung](/azure/cognitive-services/face/). Der Gesichtserkennungsdienst ermöglicht eine detailliertere Analyse zur Erkennung von Gesichtsausdruck, Kopfhaltung und Ähnlichem.  

## <a name="face-detection-examples"></a>Beispiele für die Gesichtserkennung

Das erste Beispiel zeigt die JSON-Antwort, die vom maschinellen Sehen für ein Bild mit einem einzelnen menschlichen Gesicht zurückgegeben wird.

![Bildanalyse: Woman Roof Face](./Images/woman_roof_face.png)

```json
{
    "faces": [
        {
            "age": 23,
            "gender": "Female",
            "faceRectangle": {
                "top": 45,
                "left": 194,
                "width": 44,
                "height": 44
            }
        }
    ],
    "requestId": "8439ba87-de65-441b-a0f1-c85913157ecd",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Png"
    }
}
```

Das zweite Beispiel zeigt die JSON-Antwort, die für ein Bild mit mehreren menschlichen Gesichtern zurückgegeben wird.

![Bildanalyse: Family Photo Face](./Images/family_photo_face.png)

```json
{
    "faces": [
        {
            "age": 11,
            "gender": "Male",
            "faceRectangle": {
                "top": 62,
                "left": 22,
                "width": 45,
                "height": 45
            }
        },
        {
            "age": 11,
            "gender": "Female",
            "faceRectangle": {
                "top": 127,
                "left": 240,
                "width": 42,
                "height": 42
            }
        },
        {
            "age": 37,
            "gender": "Female",
            "faceRectangle": {
                "top": 55,
                "left": 200,
                "width": 41,
                "height": 41
            }
        },
        {
            "age": 41,
            "gender": "Male",
            "faceRectangle": {
                "top": 45,
                "left": 103,
                "width": 39,
                "height": 39
            }
        }
    ],
    "requestId": "3a383cbe-1a05-4104-9ce7-1b5cf352b239",
    "metadata": {
        "height": 230,
        "width": 300,
        "format": "Png"
    }
}
```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die Konzepte zum [Erkennen domänenspezifischer Inhalte](concept-detecting-domain-content.md).