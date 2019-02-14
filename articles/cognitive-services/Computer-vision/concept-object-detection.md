---
title: Objekterkennung – maschinelles Sehen
titleSuffix: Azure Cognitive Services
description: Konzepte zur Erkennung von Objekten mithilfe der Maschinelles Sehen-API.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 12/03/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: ee85e6bd171fc9415e5c7606d6e18a7a22fa6570
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55866915"
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

## <a name="limitations"></a>Einschränkungen

Es ist wichtig, die Einschränkungen bei der Objekterkennung zu beachten, damit Sie die Auswirkungen von falsch negativen Ergebnissen (ausgelassene Objekten) und begrenzten Details vermeiden oder minimieren können.
* Objekte werden in der Regel nicht erkannt, wenn sie sehr klein (kleiner als 5% des Bilds) sind.
* Objekte werden in der Regel nicht erkannt, wenn sie sehr eng beieinander liegen (z.B. ein Stapel Teller).
* Objekte werden nicht nach Marken- oder Produktnamen unterschieden (z.B. verschiedene Arten von Mineralwasser in einem Verkaufsregal). Mithilfe der Funktion [Markenerkennung](concept-brand-detection.md) können Sie jedoch Markeninformationen aus einem Bild auslesen.

## <a name="use-the-api"></a>Verwenden der API
Die Funktion zur Erkennung von Objekten ist Teil der [Bildanalyse](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa)-API. Sie können diese API über ein natives SDK oder REST-Aufrufe aufrufen. Nachdem Sie die vollständige JSON-Antwort erhalten haben, analysieren Sie einfach die Zeichenfolge auf den Inhalt im Abschnitt `"objects"`.

* [Schnellstart: Analysieren eines Bilds (.NET SDK)](./quickstarts-sdk/csharp-analyze-sdk.md)
* [Schnellstart: Analysieren eines Bilds (REST-API)](./quickstarts/csharp-analyze.md)