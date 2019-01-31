---
title: Kategorisieren von Bildern – Maschinelles Sehen
titleSuffix: Azure Cognitive Services
description: Machen Sie sich mit Konzepten des Bildkategorisierungsfeatures der Maschinelles Sehen-API vertraut.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: f182110d150583ee1c241c23a2e1924d9f3e3bd4
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55158042"
---
# <a name="image-categorization-with-computer-vision"></a>Bildkategorisierung mit maschinellem Sehen

Zusätzlich zu Tagging und Beschreibungen gibt das maschinelle Sehen auf der Taxonomie basierende Kategorien zurück, die in früheren Versionen definiert wurden. Diese Kategorien sind als eine Taxonomie mit über-/untergeordneten vererbbaren Hierarchien organisiert. Alle Kategorien sind in Englisch. Sie können einzeln oder in Kombination mit unseren neuen Taggingmodellen verwendet werden.

## <a name="the-86-category-concept"></a>Das 86-Kategorien-Konzept

Basierend auf einer Liste mit 86 Kategorien, die in der folgenden Abbildung dargestellt sind, kann ein Bild in allgemeine bis spezifische Kategorien eingeordnet werden. Die vollständige Taxonomie im Textformat finden Sie unter [Kategorietaxonomie](category-taxonomy.md).

![Gruppierte Listen aller Kategorien in der Kategorietaxonomie](./Images/analyze_categories-v2.png)

## <a name="image-categorization-examples"></a>Beispiele für die Kategorisierung von Bildern

Die folgende JSON-Antwort veranschaulicht, was das maschinelle Sehen bei der Kategorisierung des Beispielbilds anhand seiner visuellen Merkmale zurückgibt.

![Eine Frau auf dem Dach eines Wohnblocks](./Images/woman_roof.png)

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
| ![Vier Personen, die zusammen als Familie posieren](./Images/family_photo.png) | people_group |
| ![Ein Welpe, der in einem Feld sitzt](./Images/cute_dog.png) | animal_dog |
| ![Eine Person, die bei Sonnenuntergang auf einem Gebirgsfelsen steht](./Images/mountain_vista.png) | outdoor_mountain |
| ![Mehrere Brötchen auf einem Tisch](./Images/bread.png) | food_bread |

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über Konzepte zum [Taggen von Bildern](concept-tagging-images.md) und [Beschreiben von Bildern](concept-describing-images.md).
