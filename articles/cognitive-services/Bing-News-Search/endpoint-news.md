---
title: Endpunkte für die Bing-News-Suche | Microsoft-Dokumentation
description: Überblick über den API-Endpunkt für die News-Suche.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: article
ms.date: 11/28/2017
ms.author: v-gedod
ms.openlocfilehash: b8ae99698926a818bf22b0b4027af8610413aaa6
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35372907"
---
# <a name="news-search-endpoints"></a>News-Suche-Endpunkte
Die **News-Suche-API** gibt Nachrichtenartikel, Webseiten, Bilder, Videos und [Entitäten](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/search-the-web) zurück. Entitäten enthalten zusammenfassende Informationen zur einer Person, einem Ort oder einem Thema. 
## <a name="endpoints"></a>Endpunkte
Um mithilfe der Bing-API News-Suchergebnisse zu erhalten, senden Sie eine `GET`-Anforderung an einen der folgenden Endpunkte. Die Header und die URL-Parameter definieren weitere Spezifikationen.

**Endpunkt 1:** gibt Nachrichten auf Grundlage der Suchabfrage des Benutzers zurück. Wenn die Suchabfrage leer ist, gibt der Aufruf die obersten Nachrichtenartikel zurück. Die Abfrageoption `?q=""` kann auch mit der `/news`-URL verwendet werden. Informationen zur Verfügbarkeit finden Sie unter [Unterstützte Länder und Märkte](supported-countries-markets.md#supported-markets-for-news-search-endpoint).
```
GET https://api.cognitive.microsoft.com/bing/v7.0/news/search 
``` 

**Endpunkt 2:** gibt die obersten Nachrichten geordnet nach Kategorie zurück. Mit `category=business`, `category=sports` oder `category=entertainment` können Sie insbesondere die obersten Artikel aus den Bereichen „Wirtschaft“, „Sport“ oder „Unterhaltung“ anfordern.  Der `category`-Parameter kann nur mit der `/news`-URL verwendet werden. Es gibt einige formale Anforderungen für die Angabe von Kategorien; siehe `category` in der Dokumentation der [Abfrageparameter](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#query-parameters). Informationen zur Verfügbarkeit finden Sie unter [Unterstützte Länder und Märkte](supported-countries-markets.md#supported-markets-for-news-endpoint). 
```
GET https://api.cognitive.microsoft.com/bing/v7.0/news  
```

**Endpunkt 3:** gibt Nachrichtenthemen zurück, die derzeit in sozialen Netzwerken beliebt sind. Wenn die `/trendingtopics`-Option enthalten ist, ignoriert die Bing-Suche einige andere Parameter, wie z.B. `freshness` und `?q=""`. Informationen zur Verfügbarkeit finden Sie unter [Unterstützte Länder und Märkte](supported-countries-markets.md#supported-markets-for-news-trending-endpoint).
```
GET https://api.cognitive.microsoft.com/bing/v7.0/news/trendingtopics 
``` 

Weitere Informationen zu Headern, Parametern, Marktcodes, Antwortobjekten, Fehlern usw. finden Sie in der Referenz [API für die Bing-News-Suche v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference).
## <a name="response-json"></a>JSON-Antwort
Die Antwort auf eine News-Suche-Anforderung enthält die Ergebnisse als JSON-Objekten. Für eine Analyse sind Prozeduren zum Verarbeiten der einzelnen Elementtypen erforderlich. Beispiele finden Sie unter [Tutorial](https://docs.microsoft.com/azure/cognitive-services/bing-news-search/tutorial-bing-news-search-single-page-app) und [Quellcode](https://docs.microsoft.com/azure/cognitive-services/bing-news-search/tutorial-bing-news-search-single-page-app-source).

## <a name="next-steps"></a>Nächste Schritte
Die **Bing**-APIs unterstützen Suchaktionen, die Ergebnisse gemäß ihrem Typ zurückgeben. Alle Suchendpunkte geben Ergebnisse als JSON-Antwortobjekte zurück.  Alle Endpunkte unterstützen Abfragen, die eine bestimmte Sprache und/oder einen bestimmten Ort nach Längengrad, Breitengrad und Suchradius zurückgeben.

Die umfassenden Informationen zu den Parametern, die von jedem Endpunkt unterstützt werden, finden Sie auf den Referenzseiten des jeweiligen Typs.
Beispiele für grundlegende Anforderung mithilfe der Videosuche-API finden Sie in den [Bing-News-Suche-Schnellstarts](https://docs.microsoft.com/azure/cognitive-services/bing-news-search).