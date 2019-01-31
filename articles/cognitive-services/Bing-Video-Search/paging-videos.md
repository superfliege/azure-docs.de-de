---
title: Durchblättern verfügbarer Videos – Bing-Videosuche
titlesuffix: Azure Cognitive Services
description: Zeigt, wie Sie alle Videos durchblättern, die Bing zurückgeben kann.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-video-search
ms.topic: conceptual
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: 26e706b054653b8f0ad1ea14d0a9c2ca97cd227f
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55181791"
---
# <a name="paging-videos"></a>Durchblättern von Videos

Wenn Sie die Videosuche-API aufrufen, gibt Bing eine Liste mit Ergebnissen zurück. Bei der Liste handelt es sich um eine Teilmenge der gesamten Ergebnisse, die für die Abfrage relevant sind. Um die geschätzte Gesamtzahl der verfügbaren Ergebnisse aufzurufen, greifen Sie auf das Feld [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#videos-totalestimatedmatches) des Antwortobjekts zu.  
  
Das folgende Beispiel zeigt das `totalEstimatedMatches`-Feld mit einer Videoantwort.  
  
```  
{
    "_type" : "Videos",
    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=81EF7545D56...",
    "totalEstimatedMatches" : 1000,
    "value" : [...]
}  
```  
  
Wenn Sie die verfügbaren Videos durchblättern möchten, verwenden Sie die Abfrageparameter [count](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#count) und [offset](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#offset).  
  
Der `count`-Parameter gibt die Anzahl der Ergebnisse an, die in der Antwort zurückgegeben werden sollen. Die maximale Anzahl an Ergebnissen, die Sie in der Antwort anfordern können, ist 105. Der Standardwert ist 35. Die tatsächlich gelieferte Anzahl kann geringer sein als angefordert.

Der `offset`-Parameter gibt die Anzahl der zu überspringenden Ergebnisse an. Der `offset`-Parameter ist nullbasiert und sollte kleiner sein als (`totalEstimatedMatches` - `count`).  
  
Wenn Sie pro Seite 20 Videos anzeigen möchten, setzen Sie `count` auf 20 und `offset` auf 0, um die erste Ergebnisseite anzuzeigen. Für jede nachfolgende Seite erhöhen Sie `offset` um 20 (z.B. 20, 40).  

Unten sehen Sie ein Beispiel, bei dem 20 Videos ab Offset 40 angefordert werden.  
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search?q=sailing+dinghies&count=20&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  

Wenn der `count`-Standardwert für Ihre Implementierung das gewünschte Ergebnis erzielt, müssen Sie nur den `offset`-Abfrageparameter angeben.  
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search?q=sailing+dinghies&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  

In der Regel würden Sie, wenn Sie 35 Videos gleichzeitig durchblättern, den Abfrageparameter `offset` bei Ihrer ersten Anforderung auf 0 setzen und `offset` dann bei jeder weiteren Anfragen um 35 erhöhen würden. Einige der Ergebnisse in der folgenden Antwort können jedoch Duplikate der vorherigen Antwort sein. Beispielsweise können die ersten beiden Videos in der Antwort die gleichen sein wie die letzten beiden Videos der vorherigen Antwort.

Um doppelte Ergebnisse zu eliminieren, verwenden Sie das [nextOffset](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#videos-nextoffset)-Feld des `Videos`-Objekts.

Wenn Sie beispielsweise 30 Videos gleichzeitig durchblättern möchten, setzen Sie `count` in Ihrer ersten Anforderung auf 30 und `offset` auf 0. In der nächsten Anforderung legen Sie den `offset`-Abfrageparameter auf `nextOffset` fest.


> [!NOTE]
> Das Durchblättern gilt nur für die Videosuche (/videos/search) und nicht Videoeinblicke (/videos/details) oder beliebte Videos (/videos/trending).

> [!NOTE]
> Das Feld `TotalEstimatedAnswers` gibt eine Schätzung der Gesamtzahl der Suchergebnisse an, die Sie für die aktuelle Abfrage abrufen können.  Wenn Sie die Parameter `count` und `offset` festlegen, kann sich der Wert von `TotalEstimatedAnswers` ändern. 
