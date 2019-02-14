---
title: Vordefinierte Number-Entität
titleSuffix: Azure
description: In diesem Artikel erhalten Sie Informationen zur vorgefertigten Nummernentität in Language Understanding Intelligent Service (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 11/26/2018
ms.author: diberry
ms.openlocfilehash: 5628c0fa3d6b3554fe096a323899770f3a14f97b
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55857191"
---
# <a name="number-prebuilt-entity-for-a-luis-app"></a>Vordefinierte Number-Entität für eine LUIS-App
Es gibt verschiedene Möglichkeiten, numerische Werte zu verwenden, um Informationen zu quantifizieren, auszudrücken und zu beschreiben. In diesem Artikel werden nur einige Beispiele aufgeführt. LUIS interpretiert die Variationen verschiedener Benutzeräußerungen und gibt einheitliche numerische Werte zurück. Da diese Entität bereits trainiert wurde, müssen Sie den Anwendungsabsichten keine Beispieläußerungen mit Nummern hinzufügen. 

## <a name="types-of-number"></a>Nummerntypen
Die Entität „number“ wird über das GitHub-Repository [Recognizers-text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-Numbers.yaml) verwaltet.

## <a name="examples-of-number-resolution"></a>Beispiele für Nummernauflösungen

| Äußerung        | Entität   | Lösung |
| ------------- |:----------------:| --------------:|
| ```one thousand times```  | ```"one thousand"``` |   ```"1000"```      | 
| ```1,000 people```        | ```"1,000"```    |   ```"1000"```      |
| ```1/2 cup```         | ```"1 / 2"```    |    ```"0.5"```      |
|  ```one half the amount```     | ```"one half"```     |    ```"0.5"```      |
| ```one hundred fifty orders``` | ```"one hundred fifty"``` | ```"150"``` |
| ```one hundred and fifty books``` | ```"one hundred and fifty"``` | ```"150"```|
| ```a grade of one point five```| ```"one point five"``` |  ```"1.5"``` |
| ```buy two dozen eggs```    | ```"two dozen"``` | ```"24"``` |


LUIS fügt den anerkannten Wert einer **`builtin.number`**-Entität in das `resolution`-Feld der JSON-Antwort ein, die zurückgegeben wird.

## <a name="resolution-for-prebuilt-number"></a>Auflösung der vorgefertigten Nummer
Im folgenden Beispiel wird eine JSON-Antwort von LUIS gezeigt, die die Auflösung des Werts „24“ für die Äußerung „two dozen“ enthält.

```json
{
  "query": "order two dozen eggs",
  "topScoringIntent": {
    "intent": "OrderFood",
    "score": 0.105443209
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.105443209
    },
    {
      "intent": "OrderFood",
      "score": 0.9468431361
    },
    {
      "intent": "Help",
      "score": 0.000399122015
    },
  ],
  "entities": [
    {
      "entity": "two dozen",
      "type": "builtin.number",
      "startIndex": 6,
      "endIndex": 14,
      "resolution": {
        "value": "24"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zu den [Währungs](luis-reference-prebuilt-currency.md)-, [Ordinal](luis-reference-prebuilt-ordinal.md)- und [Prozentsatzentitäten](luis-reference-prebuilt-percentage.md). 
