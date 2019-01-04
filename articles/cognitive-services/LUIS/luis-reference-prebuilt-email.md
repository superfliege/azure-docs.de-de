---
title: Referenz zur vorgefertigten E-Mail-Entität in LUIS – Azure | Microsoft-Dokumentation
titleSuffix: Azure
description: In diesem Artikel erhalten Sie Informationen zur vorgefertigten E-Mail-Entität in Language Understanding Intelligent Service (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 11/27/2018
ms.author: diberry
ms.openlocfilehash: 1cb12bdc362955da907fb5a5ed64c2a1a43fdc32
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2018
ms.locfileid: "53140862"
---
# <a name="email-prebuilt-entity-for-a-luis-app"></a>Vordefinierte E-Mail-Entität für eine LUIS-App
Die E-Mail-Entität extrahiert die vollständige E-Mail-Adresse aus einer Äußerung. Da diese Entität bereits trainiert wurde, müssen Sie den Anwendungsabsichten keine Beispieläußerungen mit E-Mail-Adressen hinzufügen. Die E-Mail-Entität wird nur in der Kultur `en-us` unterstützt. 

## <a name="resolution-for-prebuilt-email"></a>Auflösung der vorgefertigten E-Mail-Entität
Im folgenden Beispiel wird die Auflösung der Entität **builtin.email** veranschaulicht.

```json
{
  "query": "please send the information to patti.owens@microsoft.com",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.811592042
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.811592042
    }
  ],
  "entities": [
    {
      "entity": "patti.owens@microsoft.com",
      "type": "builtin.email",
      "startIndex": 31,
      "endIndex": 55
    }
  ]
}
```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zu den [Anzahl](luis-reference-prebuilt-number.md)-, [Ordinal](luis-reference-prebuilt-ordinal.md)- und [Prozentsatzentitäten](luis-reference-prebuilt-percentage.md). 