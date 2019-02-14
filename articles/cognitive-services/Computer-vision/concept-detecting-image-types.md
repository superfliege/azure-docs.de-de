---
title: Erkennen von Bildtypen – Maschinelles Sehen
titleSuffix: Azure Cognitive Services
description: Machen Sie sich mit Konzepten der Bildtyperkennung der Maschinelles Sehen-API vertraut.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 6cd7b2a8a70a315b05c0824a863803bbc6ffabb2
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55872134"
---
# <a name="detecting-image-types-with-computer-vision"></a>Erkennen von Bildtypen mit maschinellem Sehen

Das maschinelle Sehen kann den Inhaltstyp von Bildern analysieren, indem es angibt, ob es sich bei einem Bild um eine ClipArt handelt, indem die Wahrscheinlichkeit anhand einer Skala oder einer Linienzeichnung bewertet wird.

## <a name="detecting-clip-art"></a>Erkennen von ClipArts

Das maschinelle Sehen analysiert ein Bild und bewertet die Wahrscheinlichkeit, dass es sich bei dem Bild um eine ClipArt handelt auf einer Skala von 0 bis 3, wie in der folgenden Tabelle beschrieben.

| Wert | Bedeutung |
|-------|---------|
| 0 | Keine ClipArt |
| 1 | Nicht eindeutig |
| 2 | Normale ClipArt |
| 3 | Gute ClipArt |

### <a name="clip-art-detection-examples"></a>Beispiele für die Erkennung von ClipArts

Die folgende JSON-Antwort veranschaulicht, was das maschinelle Sehen bei der Bewertung der Wahrscheinlichkeit, dass es sich bei den Beispielbildern um ClipArts handelt, zurückgibt.

![Ein ClipArt-Bild einer Käsescheibe](./Images/cheese_clipart.png)

```json
{
    "imageType": {
        "clipArtType": 3,
        "lineDrawingType": 0
    },
    "requestId": "88c48d8c-80f3-449f-878f-6947f3b35a27",
    "metadata": {
        "height": 225,
        "width": 300,
        "format": "Jpeg"
    }
}
```

![Ein blaues Haus mit Vorgarten](./Images/house_yard.png)

```json
{
    "imageType": {
        "clipArtType": 0,
        "lineDrawingType": 0
    },
    "requestId": "a9c8490a-2740-4e04-923b-e8f4830d0e47",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Jpeg"
    }
}
```

## <a name="detecting-line-drawings"></a>Erkennen von Strichzeichnungen

Das maschinelle Sehen analysiert ein Bild und gibt einen booleschen Wert zurück, der angibt, ob es sich bei dem Bild um eine Strichzeichnung handelt.

### <a name="line-drawing-detection-examples"></a>Beispiele für die Erkennung von Strichzeichnungen

Die folgende JSON-Antwort veranschaulicht, was das maschinelle Sehen zurückgibt, um anzugeben, ob es sich bei den Beispielbildern um Strichzeichnungen handelt.

![Eine Strichzeichnung eines Löwen](./Images/lion_drawing.png)

```json
{
    "imageType": {
        "clipArtType": 2,
        "lineDrawingType": 1
    },
    "requestId": "6442dc22-476a-41c4-aa3d-9ceb15172f01",
    "metadata": {
        "height": 268,
        "width": 300,
        "format": "Jpeg"
    }
}
```

![Eine weiße Blume vor einem grünen Hintergrund](./Images/flower.png)

```json
{
    "imageType": {
        "clipArtType": 0,
        "lineDrawingType": 0
    },
    "requestId": "98437d65-1b05-4ab7-b439-7098b5dfdcbf",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Jpeg"
    }
}
```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über Konzepte zum [Taggen von Bildern](concept-tagging-images.md) und [Kategorisieren von Bildern](concept-categorizing-images.md).
