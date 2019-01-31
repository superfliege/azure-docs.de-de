---
title: Endpunkte für die Bing-Entitätssuche
titlesuffix: Azure Cognitive Services
description: Überblick über den API-Endpunkt für die Entitätssuche.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: conceptual
ms.date: 12/04/2017
ms.author: aahi
ms.openlocfilehash: 561c64db2b97ed8792acab6cc87de861ecc30fe9
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55183984"
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
