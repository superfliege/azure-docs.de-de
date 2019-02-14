---
title: Vordefinierte PhoneNumber-Entitäten
titleSuffix: Azure
description: In diesem Artikel erhalten Sie Informationen zur vorgefertigten Telefonnummernentität in Language Understanding Intelligent Service (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 11/27/2018
ms.author: diberry
ms.openlocfilehash: fa2163b37bda5beba7c36bfdff0778c4062c5546
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55863409"
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
