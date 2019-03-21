---
title: Vordefinierte Dimension-Entitäten
titleSuffix: Azure
description: In diesem Artikel erhalten Sie Informationen zur vordefinierten Entität „dimension“ in LUIS (Language Understanding Intelligent Service).
services: cognitive-services
ms.custom: seodec18
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 02/28/2019
ms.author: diberry
ms.openlocfilehash: 9099cdbb91e41998065d953b9d48b3b501df7c10
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2019
ms.locfileid: "57336936"
---
# <a name="dimension-prebuilt-entity-for-a-luis-app"></a>Vordefinierte Dimension-Entität für eine LUIS-App
Die vordefinierte dimension-Entität erkennt verschiedene Arten von Maßen, unabhängig von der LUIS-App-Kultur. Da diese Entität bereits trainiert wurde, müssen Sie den Anwendungsabsichten keine Beispieläußerungen mit Maßen hinzufügen. Die dimension-Entität wird in [vielen Kulturen](luis-reference-prebuilt-entities.md) unterstützt. 

## <a name="types-of-dimension"></a>dimension-Typen

Die Entität „dimension“ wird über das GitHub-Repository [Recognizers-text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml) verwaltet.


## <a name="resolution-for-dimension-entity"></a>Auflösung der dimension-Entität
Im folgenden Beispiel wird die Auflösung der Entität **builtin.dimension** veranschaulicht.

```json
{
  "query": "it takes more than 10 1/2 miles of cable and wire to hook it all up , and 23 computers.",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.762141049
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.762141049
    }
  ],
  "entities": [
    {
      "entity": "10 1/2 miles",
      "type": "builtin.dimension",
      "startIndex": 19,
      "endIndex": 30,
      "resolution": {
        "unit": "Mile",
        "value": "10.5"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die Entitäten [email](luis-reference-prebuilt-email.md), [number](luis-reference-prebuilt-number.md) und [ordinal](luis-reference-prebuilt-ordinal.md). 
