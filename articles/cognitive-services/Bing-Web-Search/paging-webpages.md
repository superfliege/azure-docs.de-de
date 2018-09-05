---
title: Durchblättern von Ergebnissen der Bing-Websuche-API | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Ergebnisse der Bing-Websuche-API durchblättern.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 26CA595B-0866-43E8-93A2-F2B5E09D1F3B
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 08/20/2018
ms.author: erhopf
ms.openlocfilehash: cd03b3af08746674dd2ba2d4af593e19e066efca
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/24/2018
ms.locfileid: "42888240"
---
# <a name="how-to-page-through-bing-web-search-api-results"></a>Durchblättern von Ergebnissen der Bing-Websuche-API

Wenn Sie die API für die Websuche aufrufen, gibt Bing eine Liste mit Ergebnissen zurück. Bei der Liste handelt es sich um eine Teilmenge der gesamten Ergebnisse, die für die Abfrage relevant sein könnten. Um die geschätzte Gesamtzahl der verfügbaren Ergebnisse aufzurufen, greifen Sie auf das Feld [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#totalestimatedmatches) des Antwortobjekts zu.  
  
Das folgende Beispiel zeigt das `totalEstimatedMatches`-Feld mit einer Webantwort.  
  
```
{
    "_type" : "SearchResponse",
    "webPages" : {
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=3A43CA...",
        "totalEstimatedMatches" : 262000,
        "value" : [...]
    }
}  
```
  
Wenn Sie die verfügbaren Webseiten durchblättern möchten, verwenden Sie die Abfrageparameter [count](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#count) und [offset](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#offset).  
  
Der `count`-Parameter gibt die Anzahl der Ergebnisse an, die in der Antwort zurückgegeben werden sollen. Die maximale Anzahl an Ergebnissen, die Sie in der Antwort anfordern können, ist 50. Der Standardwert ist 10. Die tatsächlich gelieferte Anzahl kann geringer sein als angefordert.

Der `offset`-Parameter gibt die Anzahl der zu überspringenden Ergebnisse an. Der `offset`-Parameter ist nullbasiert und sollte kleiner sein als (`totalEstimatedMatches` - `count`).  
  
Wenn Sie pro Seite 15 Webseiten anzeigen möchten, setzen Sie `count` auf 15 und `offset` auf 0, um die erste Ergebnisseite anzuzeigen. Für jede nachfolgende Seite erhöhen Sie `offset` um 15 (z.B. 15, 30).  
  
Im Beispiel unten werden 15 Webseiten ab Offset 45 angefordert.  
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+dinghies&count=15&offset=45&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```

Wenn der `count`-Standardwert für Ihre Implementierung das gewünschte Ergebnis erzielt, müssen Sie nur den `offset`-Abfrageparameter angeben.  
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+dinghies&offset=45&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```

Die Websuche-API gibt Ergebnisse mit Webseiten und ggf. auch Bildern, Videos und Nachrichten zurück. Wenn Sie die Suchergebnisse durchblättern, blättern Sie die [WebAnswer](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#webanswer)-Antwort und nicht die anderen Antworten wie Bilder oder Nachrichten durch. Wenn Sie `count` z.B. auf 50 festlegen, erhalten Sie als Ergebnis 50 Webseiten, die Antwort kann aber auch Ergebnisse für die anderen Antworten enthalten. Die Antwort kann beispielsweise auch 15 Bilder und 4 Nachrichtenartikeln enthalten. Es ist auch möglich, dass die Ergebnisse Nachrichten auf der ersten Seite enthalten, aber nicht auf der zweiten Seite oder umgekehrt.   
    
Wenn Sie den `responseFilter`-Abfrageparameter angeben, aber Webseiten nicht in die Filterliste aufnehmen, verwenden Sie nicht die Parameter `count` und `offset`.  
