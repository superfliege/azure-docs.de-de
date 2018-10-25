---
title: Endpunkte für die Bing-Entitätssuche
titlesuffix: Azure Cognitive Services
description: Überblick über den API-Endpunkt für die Entitätssuche.
services: cognitive-services
author: v-jaswel
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: conceptual
ms.date: 12/04/2017
ms.author: v-jaswel
ms.openlocfilehash: b1b89cbacf43df544261847d4e3fae396a52f423
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48816275"
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
