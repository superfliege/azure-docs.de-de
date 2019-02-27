---
title: Erkennen von Gesichtern – Maschinelles Sehen
titleSuffix: Azure Cognitive Services
description: Machen Sie sich mit Konzepten des Gesichtserkennungsfeatures der Maschinelles Sehen-API vertraut.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 02/11/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 1056b8be113d56342aea8f83d5325737f7ecb93b
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/15/2019
ms.locfileid: "56308450"
---
# <a name="face-detection-with-computer-vision"></a>Gesichtserkennung mit maschinellem Sehen

Maschinelles Sehen erkennt menschliche Gesichter auf einem Bild und generiert für jedes erkannte Gesicht das Alter, Geschlecht und Rechteck. 

> [!NOTE]
> Dieses Feature wird auch vom Azure-Dienst [Gesichtserkennung](/azure/cognitive-services/face/) geboten. Diese Alternative bietet eine detailliertere Gesichtsanalyse, einschließlich Gesichtsidentifikation und Posenerkennung. 

## <a name="face-detection-examples"></a>Beispiele für die Gesichtserkennung

Das folgende Beispiel zeigt die JSON-Antwort, die von Maschinelles Sehen für ein Bild mit einem einzelnen menschlichen Gesicht zurückgegeben wird.

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

Das nächste Beispiel zeigt die JSON-Antwort, die für ein Bild mit mehreren menschlichen Gesichtern zurückgegeben wird.

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

In der Referenzdokumentation unter [Analyze Image](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) erfahren Sie mehr zum Einsatz der Gesichtserkennungsfunktion.
