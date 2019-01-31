---
title: Vordefinierte PersonName-Entität
titleSuffix: Azure Cognitive Services
description: In diesem Artikel erhalten Sie Informationen zur vordefinierten personName-Entität in Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cjgronlund
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 01/23/2019
ms.author: diberry
ms.openlocfilehash: 342397264c364a4c07d8311cc1eda636e8a20e8d
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55211426"
---
# <a name="personname-prebuilt-entity-for-a-luis-app"></a>Vordefinierte PersonName-Entität für eine LUIS-App
Die vordefinierte personName-Entität erkennt die Namen von Personen. Da diese Entität bereits trainiert wurde, müssen Sie den Anwendungsabsichten keine Beispieläußerungen mit personName hinzufügen. Die personName-Entität wird in den [Kulturen](luis-reference-prebuilt-entities.md) Englisch und Chinesisch unterstützt.

## <a name="resolution-for-personname-entity"></a>Auflösung für die personName-Entität
Im folgenden Beispiel wird die Auflösung der Entität **builtin.personName** veranschaulicht.

```json
{
  "query": "Is Jill Jones in Cairo?",
  "topScoringIntent": {
    "intent": "WhereIsEmployee",
    "score": 0.762141049
  }
  "entities": [
    {
      "entity": "Jill Jones",
      "type": "builtin.personName",
      "startIndex": 3,
      "endIndex": 12
    }
  ]
}
```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die Entitäten [email](luis-reference-prebuilt-email.md), [number](luis-reference-prebuilt-number.md) und [ordinal](luis-reference-prebuilt-ordinal.md). 
