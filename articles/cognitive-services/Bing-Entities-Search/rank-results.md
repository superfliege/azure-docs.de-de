---
title: Verwenden der Rangfolge zum Anzeigen von Antworten – Bing-Entitätssuche
titlesuffix: Azure Cognitive Services
description: Hier wird gezeigt, wie Sie die Rangfolge zum Anzeigen der von der Bing-Entitätssuche-API zurückgegebenen Antworten verwenden.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: conceptual
ms.date: 12/12/2017
ms.author: aahi
ms.openlocfilehash: 56ca56afc799658c12293fdacaa1ccd5ca17bbf9
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2018
ms.locfileid: "52160884"
---
# <a name="using-ranking-to-display-results"></a>Verwenden der Rangfolge zum Anzeigen von Ergebnissen  

Jedes Ergebnis einer Entitätssuche enthält eine [RankingResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#rankingresponse)-Antwort, die der im Ergebnis einer Bing-Websuche ähnelt und angibt, wie die Suchergebnisse angezeigt werden müssen. Anhand der Rangfolgeantwort werden die Ergebnisse in Spitzenpositions-, Hauptbereichs- und Randleisteninhalte gruppiert. Das Spitzenpositionsergebnis ist das wichtigste oder hervorstechendste Ergebnis und sollte zuerst angezeigt werden. Wenn Sie die übrigen Ergebnisse nicht in einem herkömmlichen Hauptbereichs- und Randleistenformat anzeigen, müssen Sie dem Hauptbereichsinhalt höhere Sichtbarkeit als dem Randleisteninhalt zuweisen. 
  
Innerhalb jeder Gruppe gibt das [Items](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#rankinggroup-items)-Array die Reihenfolge an, in der die Inhalte angezeigt werden müssen. Jedes Element bietet zwei Möglichkeiten zur Angabe des Ergebnisses in einer Antwort.  
  
-   `answerType` und `resultIndex`: Das `answerType`-Feld bezeichnet die Antwort (entweder eine Entität oder ein Ort) und `resultIndex` bezeichnet ein Ergebnis in der Antwort (z.B. eine Entität). Der Index ist nullbasiert.  
  
-   `value`: Das `value`-Feld enthält eine ID, die entweder der ID einer Antwort oder eines Ergebnisses in der Antwort entspricht. Die ID ist entweder in der Antwort oder den Ergebnissen enthalten, jedoch nicht in beidem.  
  
Bei Verwendung der ID muss die Rangfolgen-ID mit der ID einer Antwort oder eines ihrer Ergebnisse übereinstimmen. Wenn ein Antwortobjekt ein `id`-Feld enthält, werden alle Ergebnisse der Antwort zusammen angezeigt. Enthält beispielsweise das `Entities`-Objekt das `id`-Feld, werden alle Entitätsartikel zusammen angezeigt. Wenn das `Entities`-Objekt das `id`-Feld nicht enthält, weist jede Entität ein `id`-Feld auf, und in der Rangfolgeantwort sind die Entitäten mit den Ergebnissen für Orte gemischt.  
  
Die Verwendung von `answerType` und `resultIndex` ist ein zweistufiger Prozess. Zuerst verwenden Sie `answerType`, um die Antwort anzugeben, in der die anzuzeigenden Ergebnisse enthalten sind. Dann verwenden Sie `resultIndex` zum Indizieren in die Ergebnisse dieser Antwort, um das anzuzeigende Ergebnis abzurufen. (Der Wert für `answerType` ist der Name des Felds im [SearchResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#searchresponse)-Objekt.) Wenn alle Ergebnisse der Antwort zusammen angezeigt werden sollen, weist das Element der Rangfolgeantwort kein `resultIndex`-Feld auf.

## <a name="ranking-response-example"></a>Beispiel einer Rangfolgeantwort

Nachfolgend sehen Sie ein Beispiel für [RankingResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#rankingresponse).
  
```json
{
  "_type": "SearchResponse",
  "queryContext": {
    "originalQuery": "Jimi Hendrix"
  },
  "entities": { ... },
  "rankingResponse": {
    "sidebar": {
      "items": [
        {
          "answerType": "Entities",
          "resultIndex": 0,
          "value": {
            "id": "https://www.bingapis.com/api/v7/#Entities.0"
          }
        },
        {
          "answerType": "Entities",
          "resultIndex": 1,
          "value": {
            "id": "https://www.bingapis.com/api/v7/#Entities.1"
          }
        }
      ]
    }
  }
}
```

Auf Grundlage dieser Rangfolgeantwort werden in der Randleiste die beiden Entitätsergebnisse im Zusammenhang mit Jimi Hendrix angezeigt.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Tutorial zur Bing-Entitätssuche](tutorial-bing-entities-search-single-page-app.md)
