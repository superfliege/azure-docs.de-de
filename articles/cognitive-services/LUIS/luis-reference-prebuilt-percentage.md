---
title: Referenz zur vorgefertigten Prozentsatzentität in LUIS – Azure | Microsoft-Dokumentation
titleSuffix: Azure
description: In diesem Artikel erhalten Sie Informationen zur vorgefertigten Prozentsatzentität in Language Understanding Intelligent Service (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: diberry
ms.openlocfilehash: 6929fc3cc41db4e4bd4a1f8bda62c953a3722eff
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47041164"
---
# <a name="percentage-entity"></a>Prozentsatzentität
Prozentzahlen können als Bruchzahlen (`3 1/2`) oder als Prozentzahlen (`2%`) angezeigt werden. Da diese Entität bereits trainiert wurde, müssen Sie den Anwendungsabsichten keine Beispieläußerungen mit Prozentsätzen hinzufügen. Die Prozentsatzentität wird in [vielen Kulturen](luis-reference-prebuilt-entities.md) unterstützt. 

## <a name="types-of-percentage"></a>Prozentsatztypen
Die Prozentsatzentität wird über das GitHub-Repository [Recognizers-text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-Numbers.yaml#L114) verwaltet.

## <a name="resolution-for-prebuilt-percentage-entity"></a>Auflösung der vorgefertigten Prozentsatzentität
Im folgenden Beispiel wird die Auflösung der Entität **builtin.percentage** veranschaulicht.

```JSON
{
  "query": "set a trigger when my stock goes up 2%",
  "topScoringIntent": {
    "intent": "SetTrigger",
    "score": 0.971157849
  },
  "intents": [
    {
      "intent": "SetTrigger",
      "score": 0.971157849
    }
  ],
  "entities": [
    {
      "entity": "2%",
      "type": "builtin.percentage",
      "startIndex": 36,
      "endIndex": 37,
      "resolution": {
        "value": "2%"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die Entitäten [ordinal](luis-reference-prebuilt-ordinal.md), [number](luis-reference-prebuilt-number.md) und [temperature](luis-reference-prebuilt-temperature.md). 