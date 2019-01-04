---
title: Objekterkennung – maschinelles Sehen
titleSuffix: Azure Cognitive Services
description: Konzepte zur Erkennung von Objekten mithilfe der Maschinelles Sehen-API.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 12/03/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 89323e584b4020613fe3ff8411df50f2ab96f156
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/18/2018
ms.locfileid: "53582727"
---
# <a name="object-detection"></a>Objekterkennung

Die Objekterkennung ist vergleichbar mit dem [Tagging](concept-tagging-images.md), die API gibt aber die Koordinaten des umgebenden Felds (in Pixel) für jedes gefundene Objekt zurück. Wenn ein Bild z.B. einen Hund, eine Katze und eine Person enthält, werden diese Objekte bei der Erkennung zusammen mit ihren Koordinaten im Bild aufgelistet. Sie können diese Funktion verwenden, um die Beziehungen zwischen den Objekten in einem Bild zu verarbeiten. Außerdem können Sie ermitteln, ob mehrere Instanzen des gleichen Tags in einem Bild enthalten sind.

Die Erkennungs-API wendet Tags basierend auf den Objekten oder Lebewesen, die im Bild identifiziert wurden, an. Beachten Sie, dass es zu diesem Zeitpunkt keine formelle Beziehung zwischen der Taxonomie für das Tagging und der Taxonomie für die Erkennung von Objekten gibt. Auf konzeptioneller Ebene findet die Erkennungs-API nur Objekte und lebende Dinge, während die Tag-API auch kontextbezogene Begriffe wie „drinnen“ einfügen kann, die über die umgebenden Felder nicht gefunden werden können.

## <a name="object-detection-example"></a>Beispiel für die Objekterkennung

Die folgende JSON-Antwort veranschaulicht, was vom maschinellen Sehen beim Erkennen von Objekten im Beispielbild zurückgegeben wird.

![Eine Frau mit einem Microsoft Surface-Gerät in einer Küche](./Images/windows-kitchen.jpg)

```json
{
   "objects":[
      {
         "rectangle":{
            "x":730,
            "y":66,
            "w":135,
            "h":85
         },
         "object":"kitchen appliance",
         "confidence":0.501
      },
      {
         "rectangle":{
            "x":523,
            "y":377,
            "w":185,
            "h":46
         },
         "object":"computer keyboard",
         "confidence":0.51
      },
      {
         "rectangle":{
            "x":471,
            "y":218,
            "w":289,
            "h":226
         },
         "object":"Laptop",
         "confidence":0.85,
         "parent":{
            "object":"computer",
            "confidence":0.851
         }
      },
      {
         "rectangle":{
            "x":654,
            "y":0,
            "w":584,
            "h":473
         },
         "object":"person",
         "confidence":0.855
      }
   ],
   "requestId":"a7fde8fd-cc18-4f5f-99d3-897dcd07b308",
   "metadata":{
      "width":1260,
      "height":473,
      "format":"Jpeg"
   }
}
```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über Konzepte zum [Kategorisieren von Bildern](concept-categorizing-images.md) und [Beschreiben von Bildern](concept-describing-images.md).