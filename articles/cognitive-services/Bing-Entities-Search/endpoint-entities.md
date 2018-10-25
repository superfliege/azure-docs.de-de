---
title: Endpunkt für die Bing-Entitätssuche
titlesuffix: Azure Cognitive Services
description: Überblick über den API-Endpunkt für die Bing-Entitätssuche.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: conceptual
ms.date: 12/05/2017
ms.author: v-gedod
ms.openlocfilehash: d781a4b3cd0119f5624b4dd20b514894ea339414
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48816581"
---
# <a name="entity-search-endpoint"></a>Endpunkt für die Entitätssuche
Die **API für die Entitätssuche** umfasst einen Endpunkt, der basierend auf einer Abfrage Entitäten aus dem Internet zurückgibt.

## <a name="endpoint"></a>Endpunkt
Um mithilfe der **Bing-API** Entitätsergebnisse zu erhalten, senden Sie eine `GET`-Anforderung an den folgenden Endpunkt. Verwenden Sie die Header und die URL-Parameter, um Spezifikationen genauer zu definieren.

**Endpunkt:** gibt Entitäten zurück, die für die von `?q=""` definierte Suchabfrage des Benutzers relevant sind
```
 GET https://api.cognitive.microsoft.com/bing/v7.0/entities
```

Weitere Informationen zu Headern, Parametern, Marktcodes, Antwortobjekten, Fehlern usw. finden Sie in der Referenz [API für die Bing-Entitätssuche v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference).

## <a name="response-json"></a>JSON-Antwort
Die Antwort auf eine Entitätssuchanforderung enthält die Ergebnisse als JSON-Objekten. Beispiele für Ergebnisse finden Sie unter [Erste Schritte](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/quick-start).

## <a name="next-steps"></a>Nächste Schritte
Die **Bing**-APIs unterstützen Suchaktionen, die Ergebnisse gemäß ihrem Typ zurückgeben. Alle Suchendpunkte geben Ergebnisse als JSON-Antwortobjekte zurück.  Alle Endpunkte unterstützen Abfragen, die eine bestimmte Sprache und/oder einen bestimmten Ort nach Längengrad, Breitengrad und Suchradius zurückgeben.

Vollständige Informationen zu den Parametern, die von jedem Endpunkt unterstützt werden, finden Sie auf den Referenzseiten für jeden Typ.
Beispiele für die Verwendung der API für die Entitätssuche finden Sie unter [Erste Schritte](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/quick-start) und [Anpassen der Größe und Zuschneiden von Miniaturansichten](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/resize-and-crop-thumbnails).