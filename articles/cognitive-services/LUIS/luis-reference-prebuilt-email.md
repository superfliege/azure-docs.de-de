---
title: Referenz zur vorgefertigten E-Mail-Entität in LUIS – Azure | Microsoft-Dokumentation
titleSuffix: Azure
description: In diesem Artikel erhalten Sie Informationen zur vorgefertigten E-Mail-Entität in Language Understanding Intelligent Service (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: diberry
ms.openlocfilehash: c69fc9cb19871611b383ebf6603197fdcd781a03
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47030596"
---
# <a name="email-entity"></a>E-Mail-Entität
Die E-Mail-Entität extrahiert die vollständige E-Mail-Adresse aus einer Äußerung. Da diese Entität bereits trainiert wurde, müssen Sie den Anwendungsabsichten keine Beispieläußerungen mit E-Mail-Adressen hinzufügen. Die E-Mail-Entität wird nur in der Kultur `en-us` unterstützt. 

## <a name="resolution-for-prebuilt-email"></a>Auflösung der vorgefertigten E-Mail-Entität
Im folgenden Beispiel wird die Auflösung der Entität **builtin.email** veranschaulicht.

```JSON
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