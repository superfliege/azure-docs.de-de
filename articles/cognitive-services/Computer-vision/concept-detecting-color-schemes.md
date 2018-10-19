---
title: Erkennen von Farbschemas – Maschinelles Sehen
titleSuffix: Azure Cognitive Services
description: Konzepte zur Erkennung des Farbschemas in Bildern mithilfe der Maschinelles Sehen-API.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.openlocfilehash: 5d0cb6ca751c844846288e8fe26f6ae542e89831
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2018
ms.locfileid: "49339491"
---
# <a name="detecting-color-schemes"></a>Erkennen von Farbschemas

Das maschinelle Sehen extrahiert Farben aus einem Bild. Die Farben werden dann in drei verschiedenen Kontexten analysiert: die vorherrschende Vordergrundfarbe, die vorherrschende Hintergrundfarbe und die vorherrschenden Farben für das gesamte Bild. Sie werden in 12 vorherrschende Akzentfarben gruppiert. Diese Akzentfarben sind Schwarz, Blau, Brown, Grau, Grün, Orange, Rosa, Violett, Rot, Aquamarin, Weiß und Gelb. Das maschinelle Sehen analysiert die aus einem Bild extrahierten Farben, um dem Betrachter eine Akzentfarbe zurückzugeben, die die kräftigste Farbe für das Bild darstellt, durch eine Kombination aus vorherrschenden Farben und Sättigung. Je nach Farben in einem Bild können einfach Schwarz und Weiß oder Akzentfarben in hexadezimalen Farbcodes zurückgegeben werden. Das maschinelle Sehen gibt auch einen booleschen Wert zurück, der angibt, ob es sich um ein Schwarzweiß-Bild handelt.

## <a name="color-scheme-detection-examples"></a>Beispiele für die Farbschemaerkennung

Das folgende Beispiel veranschaulicht die vom maschinellen Sehen zurückgegebene JSON-Antwort bei der Erkennung des Farbschemas im Beispielbild. In diesem Fall ist das Beispielbild kein Schwarzweiß-Bild, aber die vorherrschenden Vorder- und Hintergrundfarben sind schwarz, und die vorherrschenden Farben für das gesamte Bild sind schwarzweiß.

![Outdoor Mountain](./Images/mountain_vista.png)

```json
{
    "color": {
        "dominantColorForeground": "Black",
        "dominantColorBackground": "Black",
        "dominantColors": ["Black", "White"],
        "accentColor": "BB6D10",
        "isBwImg": false
    },
    "requestId": "0dc394bf-db50-4871-bdcc-13707d9405ea",
    "metadata": {
        "height": 202,
        "width": 300,
        "format": "Jpeg"
    }
}
```

### <a name="dominant-color-examples"></a>Beispiele für die vorherrschende Farbe

In der folgenden Tabelle werden die vorherrschenden Vorder-, Hintergrund- und Bildfarben für jedes Beispielbild beschrieben, die vom maschinellen Sehen zurückgegeben werden.

| Image | Vorherrschende Farben |
|-------|-----------------|
|![Bildanalyse: Flower](./Images/flower.png)| Vordergrund: Schwarz<br/>Hintergrund: Weiß<br/>Farben: Schwarz, Weiß, Grün|
![Bildanalyse: Train Station](./Images/train_station.png) | Vordergrund: Schwarz<br/>Hintergrund: Schwarz<br/>Farben: Schwarz |

### <a name="accent-color-examples"></a>Beispiele für Akzentfarben

 In der folgenden Tabelle wird die Akzentfarbe als hexadezimaler HTML-Farbwert für jedes Beispielbild beschrieben, wie es vom maschinellen Sehen zurückgegeben wird.

| Image | Akzentfarbe |
|-------|--------------|
|![Outdoor Mountain](./Images/mountain_vista.png) | #BB6D10 |
|![Bildanalyse: Flower](./Images/flower.png) | #C6A205 |
|![Bildanalyse: Train Station](./Images/train_station.png) | #474A84 |

### <a name="black--white-detection-examples"></a>Beispiele für die Schwarzweiß-Erkennung

In der folgenden Tabelle ist angegeben, ob die einzelnen Beispielbilder schwarzweiß sind, wie vom maschinellen Sehen zurückgegeben.

| Image | Schwarzweiß? |
|-------|----------------|
|![Bildanalyse: Building](./Images/bw_buildings.png) | true |
|![Bildanalyse: House Yard](./Images/house_yard.png) | false |

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über Konzepte zum [Erkennen von Bildtypen](concept-detecting-image-types.md).