---
title: Vordefinierte GeographyV2-Entität
titleSuffix: Azure Cognitive Services
description: In diesem Artikel erhalten Sie Informationen zur vorgefertigten geographyV2-Entität in Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cjgronlund
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/24/2018
ms.author: diberry
ms.openlocfilehash: a81c97cf8883d2a3629d1eb1b9f6137609c6edda
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2018
ms.locfileid: "53135099"
---
# <a name="geographyv2-prebuilt-entity-for-a-luis-app"></a>Vordefinierte GeographyV2-Entität für eine LUIS-App
Die vordefinierte geographyV2-Entität erkennt Orte. Da diese Entität bereits trainiert wurde, müssen Sie den Anwendungsabsichten keine Beispieläußerungen mit geographyV2 hinzufügen. Die geographyV2-Entität wird in der [Kultur](luis-reference-prebuilt-entities.md) Englisch unterstützt.

## <a name="subtypes"></a>Untertypen
Die geografischen Standorte weisen Untertypen auf:

|Subtype|Zweck|
|--|--|
|`poi`|Point of Interest|
|`city`|Name der Stadt|
|`countryRegion`|Name des Lands oder der Region|
|`continent`|Name des Kontinents|
|`state`|Name des Bundeslands oder Kantons|


## <a name="resolution-for-geographyv2-entity"></a>Auflösung für die geographyV2-Entität
Im folgenden Beispiel wird die Auflösung der Entität **builtin.geographyV2** veranschaulicht.

```json
{
    "query": "Carol is visiting the sphinx in gizah egypt in africa before heading to texas",
    "topScoringIntent": {
        "intent": "None",
        "score": 0.8008023
    },
    "intents": [
        {
            "intent": "None",
            "score": 0.8008023
        }
    ],
    "entities": [
        {
            "entity": "the sphinx",
            "type": "builtin.geographyV2.poi",
            "startIndex": 18,
            "endIndex": 27
        },
        {
            "entity": "gizah",
            "type": "builtin.geographyV2.city",
            "startIndex": 32,
            "endIndex": 36
        },
        {
            "entity": "egypt",
            "type": "builtin.geographyV2.countryRegion",
            "startIndex": 38,
            "endIndex": 42
        },
        {
            "entity": "africa",
            "type": "builtin.geographyV2.continent",
            "startIndex": 47,
            "endIndex": 52
        },
        {
            "entity": "texas",
            "type": "builtin.geographyV2.state",
            "startIndex": 72,
            "endIndex": 76
        },
        {
            "entity": "carol",
            "type": "builtin.personName",
            "startIndex": 0,
            "endIndex": 4
        }
    ]
} 
```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die Entitäten [email](luis-reference-prebuilt-email.md), [number](luis-reference-prebuilt-number.md) und [ordinal](luis-reference-prebuilt-ordinal.md). 