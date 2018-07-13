---
title: Referenz zur vordefinierten Entität „dimension“ in LUIS – Azure | Microsoft-Dokumentation
titleSuffix: Azure
description: In diesem Artikel erhalten Sie Informationen zur vordefinierten Entität „dimension“ in LUIS (Language Understanding Intelligent Service).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: v-geberr
ms.openlocfilehash: 3b2758f1d68ae3659f2e43cad555d327b21f8732
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321702"
---
# <a name="dimension-entity"></a>Entität „dimension“
Die vordefinierte dimension-Entität erkennt verschiedene Arten von Maßen, unabhängig von der LUIS-App-Kultur. Da diese Entität bereits trainiert wurde, müssen Sie den Anwendungsabsichten keine Beispieläußerungen mit Maßen hinzufügen. Die dimension-Entität wird in [vielen Kulturen](luis-reference-prebuilt-entities.md) unterstützt. 

## <a name="types-of-dimension"></a>dimension-Typen

Die Entität „dimension“ wird über das GitHub-Repository [Recognizers-text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml) verwaltet.


## <a name="resolution-for-dimension-entity"></a>Auflösung der dimension-Entität
Im folgenden Beispiel wird die Auflösung der Entität **builtin.dimension** veranschaulicht.

```JSON
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