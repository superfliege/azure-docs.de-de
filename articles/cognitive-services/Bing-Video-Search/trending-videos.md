---
title: Durchsuchen des Internets nach beliebten Videos – Bing-Videosuche
titlesuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie das Internet mithilfe der Bing-Videosuche-API nach beliebten Videos durchsuchen.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: conceptual
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: 8a6ccc9ea8cf9468d7638360c9db8131bc6dc5be
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47222045"
---
# <a name="get-trending-videos"></a>Abrufen von beliebten Videos  

Um die heutigen beliebten Videos zu erhalten, senden Sie die folgenden GET-Anforderung:  
  
```
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/trending?mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```

Die folgenden Märkte unterstützen beliebte Videos.  
 
-   en-AU (Englisch, Australien)  
-   en-CA (Englisch, Kanada)  
-   En-GB (Englisch, Großbritannien)  
-   en-ID (Englisch, Indonesien)  
-   en-IE (Englisch, Irland)  
-   en-IN (Englisch, Indien)  
-   en-NZ (Englisch, Neuseeland)  
-   en-PH (Englisch, Philippinen)  
-   en-SG (Englisch, Singapur)  
-   en-US (Englisch, USA)  
-   en-WW (English, weltweit aggregierter Code)  
-   en-ZA (Englisch, Südafrika)  
-   zh-CN (Chinesisch, China)

  
Das folgende Beispiel zeigt eine Antwort, die beliebte Videos enthält.  

```  
{  
    "_type" : "TrendingVideos",  
    "bannerTiles" : [
        {  
            "query" : {  
                "text" : "Hello - Smith",  
                "displayText" : "Hello - Smith",  
                "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=3E8F5..."
            },  
            "image" : {  
                "description" : "Image from: contosowallpapers.com",  
                "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?id=RsA%2fdPlTmx4zS...",  
                "headLine" : "\"Hello\" is a song by...",  
                "contentUrl" : "http:\/\/www.contosowallpapers.com\/wp-content..."  
            }  
        },  
        . . .  
    ],  
    "categories" : [
        {  
            "title" : "Music videos",  
            "subcategories" : [
                {  
                    "tiles" : [
                        {  
                            "query" : {  
                                "text" : "Song Title - Artist Name",  
                                "displayText" : "Song Title - Artist Name",  
                                "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=3E8F5..."
                            },  
                            "image" : {  
                                "description" : "Image from: contoso.com",  
                                "thumbnailUrl" : "https:\/\/tse2.mm.bing.net\/th?id=...",  
                                "contentUrl" : "http:\/\/images6.contoso.com\/image..."  
                            }  
                        },  
                        . . .  
                    ],
                    "title" : "Top "  
                },
                {
                    "tiles" : [...],
                    "title" : "Trending"
                },
                . . .
            ],  
        },
        {
            "title" : "Viral videos",
            "subcategories" : [
                {
                    "tiles" : [...],
                    "title" : "Trending"
                },
                . . .
            ],  
        },
        . . .  
    ]  
}  
  
```  
Die Antwort enthält eine Liste der Videos geordnet nach Kategorie und Unterkategorie. Wenn die Liste der Kategorien beispielsweise eine Kategorie „Musikvideo“ enthielt und eine ihrer Unterkategorien „Top“ war, können Sie eine Kategorie „Top-Musikvideo“ in Ihrer Benutzererfahrung erstellen. Sie können dann die Felder `thumbnailUrl`, `displayText` und `webSearchUrl` verwenden, um eine klickbare Kachel unter jeder Kategorie zu erstellen (z.B. Top-Musikvideos). Wenn der Benutzer auf die Kachel klickt, wird er zu Bings Videobrowser weitergeleitet, wo das Video abgespielt wird.

Die Antwort enthält auch Bannervideos, die die beliebtesten Videos sind. Die Bannervideos können aus einer oder mehr Kategorien stammen.  
  
