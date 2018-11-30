---
title: Referenz zur vorgefertigten Telefonnummernentität in LUIS – Azure | Microsoft-Dokumentation
titleSuffix: Azure
description: In diesem Artikel erhalten Sie Informationen zur vorgefertigten Telefonnummernentität in Language Understanding Intelligent Service (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 11/27/2018
ms.author: diberry
ms.openlocfilehash: 9a8fcbaf946f936d7a6d6d883a0416fce9d0c158
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2018
ms.locfileid: "52441709"
---
# <a name="phonenumber-entity"></a>Telefonnummernentität
Die `phonenumber` extrahiert verschiedene Telefonnummern mit Ländervorwahl. Da diese Entität bereits trainiert wurde, müssen Sie der Anwendung keine Beispieläußerungen mit Telefonnummern hinzufügen. Die `phonenumber` wird nur in der Kultur `en-us` unterstützt. 

## <a name="types-of-phonenumber"></a>Telefonnummertypen
Die Telefonnummernentität wird über das GitHub-Repository [Recognizers-text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/Base-PhoneNumbers.yaml) verwaltet.

## <a name="resolution-for-prebuilt-phonenumber-entity"></a>Auflösung der vorgefertigten Telefonnummernentität
Im folgenden Beispiel wird die Auflösung der Entität **builtin.phonenumber** veranschaulicht.

```JSON
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