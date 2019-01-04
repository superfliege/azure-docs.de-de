---
title: Vordefinierte PhoneNumber-Entitäten
titleSuffix: Azure
description: In diesem Artikel erhalten Sie Informationen zur vorgefertigten Telefonnummernentität in Language Understanding Intelligent Service (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 11/27/2018
ms.author: diberry
ms.openlocfilehash: 3a538886b0137e1069af1fa680374b749a9b3f92
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2018
ms.locfileid: "53165407"
---
# <a name="phonenumber-prebuilt-entity-for-a-luis-app"></a>Vordefinierte PhoneNumber-Entität für eine LUIS-App
Die `phonenumber` extrahiert verschiedene Telefonnummern mit Ländervorwahl. Da diese Entität bereits trainiert wurde, müssen Sie der Anwendung keine Beispieläußerungen mit Telefonnummern hinzufügen. Die `phonenumber` wird nur in der Kultur `en-us` unterstützt. 

## <a name="types-of-phonenumber"></a>Telefonnummertypen
Die Entität „phonenumber“ wird über das GitHub-Repository [Recognizers-text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/Base-PhoneNumbers.yaml) verwaltet.

## <a name="resolution-for-prebuilt-phonenumber-entity"></a>Auflösung der vorgefertigten Telefonnummernentität
Im folgenden Beispiel wird die Auflösung der Entität **builtin.phonenumber** veranschaulicht.

```json
{
  "query": "my mobile is 00 44 161 1234567",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.8448457
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.8448457
    }
  ],
  "entities": [
    {
      "entity": "00 44 161 1234567",
      "type": "builtin.phonenumber",
      "startIndex": 13,
      "endIndex": 29,
      "resolution": {
        "value": "00 44 161 1234567"
      }
    }
  ]
}
```


## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zu den [Prozentsatz](luis-reference-prebuilt-percentage.md)-, [Anzahl](luis-reference-prebuilt-number.md)- und [Temperaturentitäten](luis-reference-prebuilt-temperature.md). 