---
title: Endpunkte für die Bing-Entitätssuche
titlesuffix: Azure Cognitive Services
description: Überblick über den API-Endpunkt für die Entitätssuche.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: conceptual
ms.date: 12/04/2017
ms.author: aahi
ms.openlocfilehash: 578791c8beb34b08b28037977147d41db83f4a4d
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2018
ms.locfileid: "52164914"
---
# <a name="entity-search-endpoints"></a>Endpunkte für die Entitätssuche
Die **API für die Entitätssuche** umfasst einen Endpunkt.

## <a name="endpoint"></a>Endpunkt
Um Entitätssuchergebnisse anzufordern, senden Sie eine Anforderung an den folgenden Endpunkt. Verwenden Sie die Header und die URL-Parameter, um Spezifikationen genauer zu definieren.

Endpunkt `GET`: 
``` 
https://api.cognitive.microsoft.com/bing/v7.0/entities
```

Die folgenden URL-Parameter sind erforderlich:
- mkt. Der Markt, aus dem die Ergebnisse stammen. 
- q. Die Entitätssuchabfrage

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Bing-Entitätssuche – Schnellstarts](quickstarts/csharp.md)

## <a name="see-also"></a>Weitere Informationen 

[Bing-Entitätssuche – Übersicht](search-the-web.md )
[API-Referenz](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference)
