---
title: Referenz zur vordefinierten Entität „currency“ in LUIS – Azure | Microsoft-Dokumentation
titleSuffix: Azure
description: In diesem Artikel erhalten Sie Informationen zur vordefinierten Entität „currency“ in LUIS (Language Understanding Intelligent Service).
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 11/26/2018
ms.author: diberry
ms.openlocfilehash: 6346ce4e3c9e51852a9a98904b797b6eb64970e4
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/27/2018
ms.locfileid: "52334005"
---
# <a name="currency-entity"></a>Entität „currency“
Die vordefinierte currency-Entität erkennt Währungen in vielen Bezeichnungen und Ländern, unabhängig von der LUIS-App-Kultur. Da diese Entität bereits trainiert wurde, müssen Sie den Anwendungsabsichten keine Beispieläußerungen mit Währungen hinzufügen. Die currency-Entität wird in [vielen Kulturen](luis-reference-prebuilt-entities.md) unterstützt. 

## <a name="types-of-currency"></a>Arten von Währung
Die currency-Entität wird über das GitHub-Repository [Recognizers-text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml#L26) verwaltet.

## <a name="resolution-for-currency-entity"></a>Auflösung der currency-Entität
Im folgenden Beispiel wird die Auflösung der Entität **builtin.currency** veranschaulicht.

```JSON
{
  "query": "search for items under $10.99",
  "topScoringIntent": {
    "intent": "SearchForItems",
    "score": 0.926173568
  },
  "intents": [
    {
      "intent": "SearchForItems",
      "score": 0.926173568
    },
    {
      "intent": "None",
      "score": 0.07376878
    }
  ],
  "entities": [
    {
      "entity": "$10.99",
      "type": "builtin.currency",
      "startIndex": 23,
      "endIndex": 28,
      "resolution": {
        "unit": "Dollar",
        "value": "10.99"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die Entitäten [datetimeV2](luis-reference-prebuilt-datetimev2.md), [dimension](luis-reference-prebuilt-dimension.md) und [email](luis-reference-prebuilt-email.md). 