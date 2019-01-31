---
title: Includedatei
description: Includedatei
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.subservice: luis
ms.topic: include
ms.custom: include file
ms.date: 08/16/2018
ms.author: diberry
ms.openlocfilehash: 46b588d6977aa6ffeb9d473e6bfe149cde7147ba
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55478741"
---
Die Datei mit Beispieläußerungen (**utterances.json**) weist ein bestimmtes Format auf. 

Das Feld `text` enthält den Text der Beispieläußerung. Das Feld `intentName` muss dem Namen einer vorhandenen Absicht in der LUIS-App entsprechen. `entityLabels` ist ein Pflichtfeld. Wenn Sie keine Bezeichnungen für Entitäten angeben möchten, geben Sie ein leeres Array an.

Ist das entityLabels-Array nicht leer, müssen `startCharIndex` und `endCharIndex` die Entität markieren, auf die im Feld `entityName` verwiesen wird. Der Index ist nullbasiert, was bedeutet, dass sich der Wert „6“ im Beispiel oben auf das „S“ von Seattle bezieht und nicht auf das Leereichen vor dem Großbuchstaben S. Wenn Sie die Bezeichnung mit einem Leerzeichen im Text beginnen oder beenden, schlägt der API-Aufruf zum Hinzufügen der Äußerungen fehl.

```JSON
[
  {
    "text": "go to Seattle today",
    "intentName": "BookFlight",
    "entityLabels": [
      {
        "entityName": "Location::LocationTo",
        "startCharIndex": 6,
        "endCharIndex": 12
      }
    ]
  },
  {
    "text": "purple dogs are difficult to work with",
    "intentName": "BookFlight",
    "entityLabels": []
  }
]
```
