---
title: Kategorisieren von Bildern – Maschinelles Sehen
titleSuffix: Azure Cognitive Services
description: Konzepte zur Kategorisierung von Bildern mithilfe der Maschinelles Sehen-API.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.openlocfilehash: 602ea8028cf89b23df692d5c2fb9b781f64bcad4
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2018
ms.locfileid: "49341683"
---
# <a name="categorizing-images"></a>Kategorisieren von Bildern

Zusätzlich zu Tagging und Beschreibungen gibt das maschinelle Sehen auf der Taxonomie basierende Kategorien zurück, die in früheren Versionen definiert wurden. Diese Kategorien sind als eine Taxonomie mit über-/untergeordneten vererbbaren Hierarchien organisiert. Alle Kategorien sind in Englisch. Sie können einzeln oder in Kombination mit unseren neuen Taggingmodellen verwendet werden.

## <a name="the-86-category-concept"></a>Das 86-Kategorien-Konzept

Basierend auf einer Liste mit 86 Kategorien, die in der folgenden Abbildung dargestellt sind, kann ein Bild in allgemeine bis spezifische Kategorien eingeordnet werden. Die vollständige Taxonomie im Textformat finden Sie unter [Kategorietaxonomie](category-taxonomy.md).

![Analysieren von Kategorien](./Images/analyze_categories.png)

## <a name="image-categorization-examples"></a>Beispiele für die Kategorisierung von Bildern

Die folgende JSON-Antwort veranschaulicht, was das maschinelle Sehen bei der Kategorisierung des Beispielbilds anhand seiner visuellen Merkmale zurückgibt.

![Woman Roof](./Images/woman_roof.png)

```json
{
    "categories": [
        {
            "name": "people_",
            "score": 0.81640625
        }
    ],
    "requestId": "bae7f76a-1cc7-4479-8d29-48a694974705",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Jpeg"
    }
}
```

Die folgende Tabelle zeigt einen typischen Satz von Bildern und die vom maschinellen Sehen zurückgegebene Kategorie für jedes Bild.

| Image | Category (Kategorie) |
|-------|----------|
| ![Family Photo](./Images/family_photo.png) | people_group |
| ![Cute Dog](./Images/cute_dog.png) | animal_dog |
| ![Outdoor Mountain](./Images/mountain_vista.png) | outdoor_mountain |
| ![Bildanalyse: Food Bread](./Images/bread.png) | food_bread |

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über Konzepte zum [Taggen von Bildern](concept-tagging-images.md) und [Beschreiben von Bildern](concept-describing-images.md).