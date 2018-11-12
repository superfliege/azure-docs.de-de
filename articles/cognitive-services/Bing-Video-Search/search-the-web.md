---
title: Worum handelt es sich bei der Bing-Videosuche?
titlesuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie das Internet mithilfe der Bing-Videosuche-API nach Videos durchsuchen.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: overview
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: e48a0a056628e0c863330de792f8edfaa48aae34
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51261858"
---
# <a name="what-is-bing-video-search"></a>Worum handelt es sich bei der Bing-Videosuche?

Die Bing Videosuche-API bietet eine Umgebung ähnlich (aber nicht identisch mit) [Bing Videos](https://www.bing.com/videos). Mit der Bing Videosuche-API können Sie eine Suchabfrage an Bing senden und eine Liste relevanter Videos abrufen.

Wenn Sie eine rein videoorientierte Suchergebnisseite erstellen, um nach relevanten Videos für die Suchabfrage des Benutzers zu suchen, rufen Sie anstelle der allgemeineren [Bing-Websuche-API](../bing-web-search/search-the-web.md) diese API auf. Möchten Sie dagegen neben Videos auch andere Arten von Inhalten (etwa Webseiten, Videos und Bilder) anzeigen, rufen Sie die Bing-Websuche-API auf.

## <a name="suggesting--using-search-terms"></a>Vorschlagen und Verwenden von Suchbegriffen

Wenn Sie ein Suchfeld bereitstellen, in das Benutzer ihre Suchbegriffe eingeben, verwenden Sie die [Bing-Vorschlagssuche-API](../bing-autosuggest/get-suggested-search-terms.md), um die Benutzerfreundlichkeit zu verbessern. Die API gibt vorgeschlagene Abfragezeichenfolgen zurück, während der Benutzer einen Suchbegriff eingibt.

Codieren Sie den vom Benutzer eingegebenen Suchbegriff als URL, bevor Sie den Abfrageparameter [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#query) festlegen. Wenn der Benutzer also beispielsweise *sailing dinghies* eingibt, legen Sie `q` auf `sailing+dinghies` oder `sailing%20dinghies` fest.

## <a name="getting-videos"></a>Abrufen von Videos

Senden Sie die folgende GET-Anforderung, um Videos aus dem Web abzurufen, die mit dem Suchbegriff des Benutzers zusammenhängen:

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search?q=sailing+dinghies&mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)
X-Search-ClientIP: 999.999.999.999
X-Search-Location: lat:47.60357;long:-122.3295;re:100
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
Host: api.cognitive.microsoft.com
```

Alle Anforderungen müssen über einen Server erfolgen.

Wenn Sie zuvor noch keine der Bing-APIs aufgerufen haben, lassen Sie den Client-ID-Header weg. Schließen Sie die Client-ID nur ein, wenn Sie bereits eine Bing-API aufgerufen haben und Bing eine Client-ID für die Kombination aus Benutzer und Gerät zurückgegeben hat.

Wenn Sie Videos aus einer bestimmten Domäne abrufen möchten, verwenden Sie den Abfrageoperator [site:](https://msdn.microsoft.com/library/ff795613.aspx).

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search?q=sailing+dinghies+site:contososailing.com&mkt=en-us HTTP/1.1
```

Die zurückgegebene Antwort vom Typ [Videos](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#videos) enthält eine Liste mit Videos, die nach Einschätzung von Bing für die Abfrage relevant sind. Jedes [Video](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#video)-Objekt in der Liste enthält neben anderen Attributen die URL, die Dauer, die Abmessungen und das Codierungsformat des Videos. Darüber hinaus enthält das Videoobjekt die URL einer Miniaturansicht des Videos sowie die Abmessungen der Miniaturansicht.

```json
{
    "_type" : "Videos",
    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=81EF7545...",
    "totalEstimatedMatches" : 1000,
    "value" : [
        {
            "name" : "How to sail - What to Wear for Dinghy Sailing",
            "description" : "An informative video on what to wear when...",
            "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=81EF7...",
            "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?id=OVP.DYW...",
            "datePublished" : "2014-03-04T11:51:53",
            "publisher" : [
                {
                    "name" : "Fabrikam"
                }
            ],
            "creator" : 
            {
                "name" : "Marcus Appel"
            },
            "contentUrl" : "https:\/\/www.fabrikam.com\/watch?v=vzmPjZ--g",
            "hostPageUrl" : "https:\/\/www.bing.com\/cr?IG=81EF7545D569...",
            "encodingFormat" : "h264",
            "hostPageDisplayUrl" : "https:\/\/www.fabrikam.com\/watch?v=vzmPjZ--g",
            "width" : 1280,
            "height" : 720,
            "duration" : "PT2M47S",
            "motionThumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?id=OM.Y62...",
            "embedHtml" : "<iframe width=\"1280\" height=\"720\" src=\"https:...><\/iframe>",
            "allowHttpsEmbed" : true,
            "viewCount" : 8743,
            "thumbnail" : 
            {
                "width" : 300,
                "height" : 168
            },
            "videoId" : "6DB795E11A6E3CBAAD636DB795E113CBAAD63",
            "allowMobileEmbed" : true,
            "isSuperfresh" : false
        },
        ...
    ],
    "queryExpansions" : [...],
    "nextOffsetAddCount" : 0,
    "pivotSuggestions" : [...]
}
```

Sie können eine Collage aller Videominiaturansichten oder eine Teilmenge der Miniaturansichten anzeigen. Wenn Sie eine Teilmenge anzeigen, geben Sie dem Benutzer die Möglichkeit, die restlichen Videos anzuzeigen. Die Videos müssen in der Reihenfolge angezeigt werden, in der sie in der Antwort bereitgestellt werden. Informationen zum Ändern der Größe der Miniaturansicht finden Sie unter [Resizing and cropping thumbnail images](./resize-and-crop-thumbnails.md) (Ändern der Größe und Zuschneiden von Miniaturansichten). 

Wenn der Benutzer auf die Miniaturansicht zeigt, können Sie [motionThumbnailUrl](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#video-motionthumbnailurl) verwenden, um eine Miniaturansichtsversion des Videos wiederzugeben. Vergessen Sie bei der Anzeige der bewegten Miniaturansicht nicht, die Quelle anzugeben.

<!-- Removing until the images can be sanitized.
![Motion thumbnail of a video](../bing-web-search/media/cognitive-services-bing-web-api/bing-web-video-motion-thumbnail.PNG)
-->

Wenn der Benutzer auf die Miniaturansicht klickt, stehen folgende Optionen zum Anzeigen des Videos zur Verfügung:

- Verwenden Sie [hostPageUrl](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#video-hostpageurl), um das Video auf der Website des Hosts anzuzeigen (z. B. YouTube).
- Verwenden Sie [webSearchUrl](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#video-websearchurl), um das Video im Bing-Videobrowser anzuzeigen.
- Verwenden Sie [embdedHtml](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#video-embedhtml), um das Video in Ihrer Benutzeroberfläche einzubetten. 

Achten Sie darauf, dass bei der Wiedergabe des Videos der Herausgeber und der Ersteller als Attribute angegeben werden.

Ausführliche Informationen zur Verwendung von [videoId](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#video-videoid), um Einblicke in das Video zu erhalten, finden Sie unter [Videoeinblicke](./video-insights.md).

## <a name="filtering-videos"></a>Filtern von Videos

Die Videosuche-API gibt standardmäßig alle Videos zurück, die für die Abfrage relevant sind. Wenn Sie nur kostenlose Videos oder Videos von weniger als fünf Minuten Länge wünschen, verwenden Sie die folgenden Filterabfrageparameter:

- [pricing](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#pricing): Filtert Videos nach Preis (z. B. kostenlose oder kostenpflichtige Videos).
- [resolution](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#resolution): Filtert Videos nach Auflösung (z. B. Videos mit einer Auflösung von mindestens 720p).
- [videoLength](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#videolength): Filtert Videos nach Länge des Videos (z. B. Videos, die weniger als fünf Minuten lang sind).
- [freshness](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#freshness): Filtert Videos nach Alter (beispielsweise Videos, die von Bing in der letzten Woche gefunden wurden).

Um Videos von einer bestimmten Domäne zu erhalten, schließen Sie den Abfrageoperator [site:](https://msdn.microsoft.com/library/ff795613.aspx) in die Abfragezeichenfolge ein.

> [!NOTE]
> Bei Verwendung des Abfrageoperators `site:` kann es je nach Abfrage vorkommen, dass die Antwort nicht jugendfreie Inhalte enthält (unabhängig von der Einstellung [safeSearch](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#safesearch)). Verwenden Sie `site:` nur, wenn Sie wissen, welche Inhalte die Website enthält, und wenn in Ihrem Szenario ggf. auch nicht jugendfreie Inhalte zulässig sind.

Das folgende Beispiel zeigt, wie Sie kostenlose Videos von ContosoSailing.com erhalten, die eine Auflösung von mindestens 720p aufweisen und von Bing im letzten Monat gefunden wurden.

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search?q=sailing+dinghies+site:contososailing.com&pricing=free&freshness=month&resolution=720p&mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)
X-MSEdge-ClientIP: 999.999.999.999
X-Search-Location: lat:47.60357;long:-122.3295;re:100
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
Host: api.cognitive.microsoft.com
```

## <a name="expanding-the-query"></a>Erweitern der Abfrage

Wenn Bing die Abfrage erweitern kann, um die ursprüngliche Suche einzugrenzen, enthält das Objekt [Videos](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#videos) das Feld `queryExpansions`. Wenn die Abfrage beispielsweise *Dachrinne reinigen* war, können die erweiterten Abfragen Folgendes sein: Dachrinnenreinigung **Werkzeuge**, Dachrinne reinigen **vom Boden aus**, Dachrinnenreinigung **Maschine** und **einfache** Dachrinnenreinigung.

Das folgende Beispiel zeigt die erweiterten Abfragen für *Dachrinne reinigen*:

```json
{
    "_type" : "Videos",
    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=B52FBC5...",
    "totalEstimatedMatches" : 1000,
    "value" : [...],
    "nextOffsetAddCount" : 4,
    "queryExpansions" : [
        {
            "text" : "Gutter Cleaning Tools",
            "displayText" : "Tools",
            "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=B52FB....",
            "searchLink" : "https:\/\/api.cognitive.microsoft.com\/api\/v5...",
            "thumbnail" : {
                "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?q=Gutter..."
            }
        },
        ...
    ]
    "pivotSuggestions" : [...],
}
```

Das Feld `queryExpansions` enthält eine Liste mit Objekten vom Typ [Query](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#query_obj). Das Feld `text` enthält die erweiterte Abfrage und das Feld `displayText` den Erweiterungsausdruck. Mit den Feldern „Text“ und „Miniaturansicht“ können Sie dem Benutzer die erweiterten Abfragezeichenfolgen anzeigen, falls dieser eigentlich nach der erweiterten Abfragezeichenfolge suchen wollte. Verwenden Sie die URL `webSearchUrl` oder `searchLink`, um die Miniaturansicht bzw. den Text klickbar zu machen. Verwenden Sie `webSearchUrl`, um den Benutzer zu den Bing-Suchergebnissen weiterzuleiten, oder `searchLink`, falls Sie eine eigene Ergebnisseite verwenden.

## <a name="pivoting-the-query"></a>Pivotieren der Abfrage

Wenn Bing die ursprüngliche Suchabfrage segmentieren kann, enthält das Objekt [Videos](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#videos) das Feld `pivotSuggestions`. Lautet die ursprüngliche Abfrage also beispielsweise *Dachrinne reinigen*, kann Bing die Abfrage in *Reinigen* und *Dachrinne* segmentieren.

Das folgende Beispiel veranschaulicht die Pivotvorschläge für *Dachrinne reinigen*:

```json
{
    "_type" : "Videos",
    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=B52FBC...",
    "totalEstimatedMatches" : 1000,
    "value" : [...],
    "nextOffsetAddCount" : 0,
    "queryExpansions" : [...],
    "pivotSuggestions" : [
        {
            "pivot" : "cleaning",
            "suggestions" : [
                {
                    "text" : "Gutter Repair",
                    "displayText" : "Repair",
                    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=B52...",
                    "searchLink" : "https:\/\/api.cognitive.microsoft.com\/api\/v5\/videos...",
                    "thumbnail" : {
                        "thumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?q=Gutter..."
                    }
                },
                ...
            ]
        },
        {
            "pivot" : "gutters",
            "suggestions" : [
                {
                    "text" : "Window Cleaning",
                    "displayText" : "Window",
                    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=B52FBC59...",
                    "searchLink" : "https:\/\/api.cognitive.microsoft.com\/api\/v5...",
                    "thumbnail" : {
                        "thumbnailUrl" : "https:\/\/tse2.mm.bing.net\/th?q=Window..."
                    }
                },
                ...
            ]
        }
    ]
}
```

Die Antwort enthält für jeden Pivotwert eine Liste mit Objekten vom Typ [Query](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#query_obj), die wiederum vorgeschlagene Abfragen enthalten. Das Feld `text` enthält die vorgeschlagene Abfrage. Das Feld `displayText` enthält den Ausdruck, der den Pivotwert in der ursprünglichen Abfrage ersetzt. Z. B. Fensterreinigung.

Mit den Feldern `text` und `thumbnail` können Sie dem Benutzer die erweiterten Abfragezeichenfolgen anzeigen, falls dieser eigentlich nach der erweiterten Abfragezeichenfolge suchen wollte. Verwenden Sie die URL `webSearchUrl` oder `searchLink`, um die Miniaturansicht bzw. den Text klickbar zu machen. Verwenden Sie `webSearchUrl`, um den Benutzer zu den Bing-Suchergebnissen weiterzuleiten, oder `searchLink`, falls Sie eine eigene Ergebnisseite verwenden.

## <a name="throttling-requests"></a>Drosselungsanforderungen

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Nächste Schritte

Der Artikel [Erstellen Ihrer ersten Anforderung](./quick-start.md) ermöglicht einen schnellen Einstieg in die Verwendung Ihrer ersten Anforderung.

Informationen zum Abrufen Ihres Abonnementschlüssels finden Sie unter [Abonnementschlüssel](https://azure.microsoft.com/try/cognitive-services/?api=bing-video-search-api).

Machen Sie sich mit der Referenz für die [Bing-Videosuche-API v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference) vertraut. Die Referenz enthält die Liste mit den Endpunkten, Headern und Abfrageparametern, die Sie zum Anfordern von Suchergebnissen verwenden können. Darüber hinaus finden Sie dort Definitionen der Antwortobjekte. 

Informationen zur Verbesserung der Benutzerfreundlichkeit für das Suchfeld finden Sie unter [Bing-Vorschlagssuche-API](../bing-autosuggest/get-suggested-search-terms.md). Während ein Benutzer den Abfragebegriff eingibt, können Sie diese API aufrufen, um relevante Abfragebegriffe zu erhalten, die von anderen Benutzern verwendet wurden.

Machen Sie sich vorher unbedingt mit den [Verwendungs- und Anzeigeanforderungen von Bing](./useanddisplayrequirements.md) vertraut, um nicht gegen die Regeln für die Verwendung der Suchergebnisse zu verstoßen.

Wenn Sie die Videosuche-API aufrufen, gibt Bing eine Liste mit Ergebnissen zurück. Bei der Liste handelt es sich um eine Teilmenge der gesamten Ergebnisse, die für die Abfrage relevant sind. Das Feld `totalEstimatedMatches` der Antwort enthält die geschätzte Anzahl von Videos, die angezeigt werden können. Ausführliche Informationen zum Blättern durch die restlichen Videos finden Sie [Durchblättern von Videos](./paging-videos.md).

Details zum Abrufen von Auswertungen zu einem Video finden Sie unter [Videoauswertungen](./video-insights.md).

Ausführliche Informationen zum Abrufen von beliebten Videos finden Sie unter [Beliebte Videos](./trending-videos.md).