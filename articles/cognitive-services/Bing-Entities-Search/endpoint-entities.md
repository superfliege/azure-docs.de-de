---
title: Endpunkt für die Entitätssuche | Microsoft-Dokumentation
description: Überblick über den API-Endpunkt für die Entitätssuche.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: article
ms.date: 12/05/2017
ms.author: v-gedod
ms.openlocfilehash: a2557c6000445544b3b47a05d7d356ccaa9928b4
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35372763"
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
Die **Bing**-APIs unterstützen Suchaktionen, die Ergebnisse entsprechend ihrem Typ zurückgeben. Alle Suchendpunkte geben Ergebnisse als JSON-Antwortobjekte zurück.  Alle Endpunkte unterstützt Abfragen, die eine bestimmte Sprache und/oder die Position von Längengrad, Breitengrad und Suchradius zurückgeben.

Vollständige Informationen zu den Parametern, die von jedem Endpunkt unterstützt werden, finden Sie auf den Referenzseiten für jeden Typ.
Beispiele für die Verwendung der API für die Entitätssuche finden Sie unter [Erste Schritte](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/quick-start) und [Anpassen der Größe und Zuschneiden von Miniaturansichten](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/resize-and-crop-thumbnails).