---
title: Was ist die Bing-Bildersuche? | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie das Web mithilfe der Bing-Bildersuche-API nach Bildern durchsuchen.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 1446AD8B-A685-4F5F-B4AA-74C8E9A40BE9
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: overview
ms.date: 10/11/2017
ms.author: scottwhi
ms.openlocfilehash: 457707065059b5cb83c9d2b81f5d073cf1c89b84
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "36336518"
---
# <a name="what-is-bing-image-search"></a>Was ist die Bing-Bildersuche?

Die Bing-Bildersuche-API bietet eine ähnliche Erfahrung wie [Bing-Bilder](https://www.bing.com/images): Sie können eine Suchabfrage eines Benutzers an Bing senden und eine Liste mit relevanten Bildern erhalten.

Wenn Sie eine rein bildorientierte Suchergebnisseite erstellen, um nach relevanten Bildern für die Suchabfrage des Benutzers zu suchen, rufen Sie anstelle der allgemeineren [Websuche-API](../bing-web-search/search-the-web.md) diese API auf. Möchten Sie dagegen neben Bildern auch andere Arten von Inhalten (etwa Webseiten, Nachrichten und Videos) anzeigen, rufen Sie die Websuche-API auf.

## <a name="suggesting--using-search-terms"></a>Vorschlagen und Verwenden von Suchbegriffen

Wenn Sie ein Suchfeld bereitstellen, in das der Benutzer den gewünschten Suchbegriff eingibt, verwenden Sie die [Bing-Vorschlagssuche-API](../bing-autosuggest/get-suggested-search-terms.md), um die Benutzerfreundlichkeit zu verbessern. Die API gibt auf der Grundlage unvollständiger Suchbegriffe vorgeschlagene Abfragezeichenfolgen zurück, während der Benutzer seinen Suchbegriff eingibt.

Codieren Sie den vom Benutzer eingegebenen Suchbegriff als URL, bevor Sie den Abfrageparameter [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#query) festlegen. Wenn der Benutzer also beispielsweise *sailing dinghies* eingibt, legen Sie `q` auf `sailing+dinghies` oder `sailing%20dinghies` fest.

## <a name="getting-images"></a>Abrufen von Bildern

Senden Sie die folgende GET-Anforderung, um Bilder aus dem Web abzurufen, die mit dem Suchbegriff des Benutzers zusammenhängen:

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies&mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE
X-MSEdge-ClientIP: 999.999.999.999
X-Search-Location: lat:47.60357;long:-122.3295;re:100
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
Host: api.cognitive.microsoft.com
```

Alle Anforderungen müssen über einen Server erfolgen. Clientbasierte Aufrufe sind nicht möglich.

Wenn Sie zuvor noch keine der Bing-APIs aufgerufen haben, lassen Sie den Client-ID-Header weg. Schließen Sie die Client-ID nur ein, wenn Sie bereits eine Bing-API aufgerufen haben und Bing eine Client-ID für die Kombination aus Benutzer und Gerät zurückgegeben hat.

Wenn Sie Bilder aus einer bestimmten Domäne abrufen möchten, verwenden Sie den Abfrageoperator [site:](http://msdn.microsoft.com/library/ff795613.aspx).

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies+site:contososailing.com&mkt=en-us HTTP/1.1
```

Die zurückgegebene Antwort vom Typ [Images](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images) enthält eine Liste mit Bildern, die nach Einschätzung von Bing für die Abfrage relevant sind. Jedes Objekt vom Typ [Image](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image) in der Liste enthält die URL, die Größe, die Abmessungen und das Codierungsformat des jeweiligen Bilds. Darüber hinaus enthält das Bildobjekt die URL einer Miniaturansicht des Bilds sowie die Abmessungen der Miniaturansicht.

```json
{
    "name": "Rich Passage Sailing Dinghy",
    "webSearchUrl": "https:\/\/www.bing.com\/cr?IG=73118C8B4E3...",
    "thumbnailUrl": "https:\/\/tse1.mm.bing.net\/th?id=OIP.GNarK7m...",
    "datePublished": "2011-10-29T11:26:00",
    "contentUrl": "http:\/\/www.bing.com\/cr?IG=73118C8B4E3D4C3...",
    "hostPageUrl": "http:\/\/www.bing.com\/cr?IG=73118C8B4E3D4C3687...",
    "contentSize": "79239 B",
    "encodingFormat": "jpeg",
    "hostPageDisplayUrl": "en.contoso.org\/wiki\/File:Rich_Passage...",
    "width": 526,
    "height": 688,
    "thumbnail": {
        "width": 229,
        "height": 300
    },
    "imageInsightsToken": "ccid_GNarK7ma*mid_CCF85447ADA6...",
    "insightsSourcesSummary": {
        "shoppingSourcesCount": 0,
        "recipeSourcesCount": 0
    },
    "imageId": "CCF85447ADA6FFF9E96E7DF0B796F7A86E34593",
    "accentColor": "376094"
},
```

Sie können eine Collage aller Miniaturansichten oder eine Teilmenge der Miniaturansichten anzeigen. Wenn Sie eine Teilmenge anzeigen, geben Sie dem Benutzer die Möglichkeit, die restlichen Bilder anzuzeigen. Die Bilder müssen in der Reihenfolge angezeigt werden, in der sie in der Antwort bereitgestellt werden.

Die Miniaturansicht kann erweitert werden, wenn der Benutzer mit dem Cursor darauf zeigt. Wenn das Bild erweitert wird, muss die Quelle angegeben werden. Hierzu können Sie beispielsweise den Host aus [hostPageDisplayUrl](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image-hostpagedisplayurl) extrahieren und unter dem Bild anzeigen. Informationen zum Ändern der Größe der Miniaturansicht finden Sie unter [Resizing and cropping thumbnail images](./resize-and-crop-thumbnails.md) (Ändern der Größe und Zuschneiden von Miniaturansichten).

<!-- Removing image until we can replace it with a sanatized version.
![Expanded view of thumbnail image](../bing-web-search/media/cognitive-services-bing-web-api/bing-web-image-thumbnail-expansion.PNG)
-->

Wenn der Benutzer auf die Miniaturansicht klickt, können Sie die Inhalts-URL ([contentUrl](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image-contenturl)) verwenden, um dem Benutzer das Bild in voller Größe anzuzeigen. Vergessen Sie nicht, die Quelle anzugeben.

Wenn `shoppingSourcesCount` oder `recipeSourcesCount` größer Null ist, fügen Sie der Miniaturansicht Badging (beispielsweise einen Einkaufswagen) hinzu, um anzugeben, dass für den Artikel auf dem Bild Einkaufsmöglichkeiten oder Rezepte verfügbar sind.

<!-- this is a sanitized version but we're removing all images for now until everything is sanitized.
![Shopping sources badge](./media/cognitive-services-bing-images-api/bing-images-shopping-source.PNG)
-->

Verwenden Sie [imageInsightsToken](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image-imageinsightstoken), um Insights für das Bild abzurufen (etwa Webseiten, die das Bild enthalten, oder Personen, die auf dem Bild erkannt wurden). Ausführliche Informationen finden Sie unter [Get insights about an image](./image-insights.md) (Abrufen von Insights zu einem Bild).

## <a name="filtering-images"></a>Filtern von Bildern

 Die Bildersuche-API gibt standardmäßig alle Bilder zurück, die für die Abfrage relevant sind. Wenn Sie allerdings nur Bilder mit einem transparenten Hintergrund oder nur Bilder mit einer bestimmten Größe erhalten möchten, können Sie die von Bing zurückgegebenen Bilder mithilfe der folgenden Abfrageparameter filtern:  

- [aspect](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#aspect): Filtert Bilder nach Seitenverhältnis (beispielsweise Standard oder Breitbild).
- [color](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#color): Filtert Bilder nach dominanter Farbe oder Schwarzweiß.
- [freshness](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#freshness): Filtert Bilder nach Alter (beispielsweise Bilder, die von Bing in der letzten Woche gefunden wurden).
- [height](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#height), [width](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#width): Filtert Bilder nach Breite und Höhe.
- [imageContent](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagecontent): Filtert Bilder nach Inhalt (beispielsweise Bilder, die nur das Gesicht einer Person zeigen).
- [imageType](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagetype): Filtert Bilder nach Typ (beispielsweise ClipArts, animierte GIFs oder transparente Hintergründe).
- [license](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#license): Filtert Bilder nach Art der Websitelizenz.
- [size](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#size): Filtert Bilder nach Größe (beispielsweise kleine Bilder bis zu einer Größe von 200 x 200 Pixel).

Wenn Sie Bilder aus einer bestimmten Domäne abrufen möchten, verwenden Sie den Abfrageoperator [site:](http://msdn.microsoft.com/library/ff795613.aspx).

> [!NOTE]
> Bei Verwendung des Operators `site:` kann es je nach Abfrage vorkommen, dass die Antwort nicht jugendfreie Inhalte enthält (unabhängig von der Einstellung [safeSearch](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#safesearch)). Verwenden Sie `site:` nur, wenn Sie wissen, welche Inhalte die Website enthält, und wenn in Ihrem Szenario ggf. auch nicht jugendfreie Inhalte zulässig sind.

Das folgende Beispiel zeigt, wie Sie von ContosoSailing.com kleine Bilder abrufen, die Bing in der letzten Woche gefunden hat.  

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies+site:contososailing.com&size=small&freshness=week&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```

## <a name="pivoting-the-query"></a>Pivotieren der Abfrage

Wenn Bing die ursprüngliche Suchabfrage segmentieren kann, enthält das Objekt [Images](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images) das Feld `pivotSuggestions`. Lautet die ursprüngliche Abfrage also beispielsweise *Microsoft Surface*, kann Bing die Abfrage in *Microsoft* und *Surface* segmentieren.  

Das folgende Beispiel zeigt die Pivotvorschläge für *Microsoft Surface*:  

```json
{
    "_type": "Images",
    "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=microsoft%20surface&FORM=OIIARP",
    "totalEstimatedMatches": 1000,
    "value": [...],
    "queryExpansions": [...],
    "pivotSuggestions": [{
        "pivot": "microsoft",
        "suggestions": [{
            "text": "Contoso Surface",
            "displayText": "Contoso",
            "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=OtterBox+Surface&FORM=IRQBPS",
            "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=Contoso...",
                    "searchLink": "https:\/\/api.cognitive.microsoft.com\/api...",
            "thumbnail": {
                "thumbnailUrl": "https:\/\/tse3.mm.bing.net\/th?q=Contoso+Surface..."
            }
        },
        {
            "text": "Adatum Surface",
            "displayText": "Adatum",
            "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Adatum+Surface&FORM=IRQBPS",
            "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=...",
            "thumbnail": {
                "thumbnailUrl": "https:\/\/tse3.mm.bing.net\/th?q=Adatum+Surface&pid=Ap..."
            }
        },
        ...
        ]
    },
    {
        "pivot": "surface",
        "suggestions": [{
            "text": "Microsoft Surface4",
            "displayText": "Surface4",
            "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Microsoft+Surface...",
            "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?...",
            "thumbnail": {
                "thumbnailUrl": "https:\/\/tse4.mm.bing.net\/th?q=Microsoft..."
            }
        },
        {
            "text": "Microsoft Tablet",
            "displayText": "Tablet",
            "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Microsoft+Tablet&FORM=IRQBPS",
            "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?...",
            "thumbnail": {
                "thumbnailUrl": "https:\/\/tse3.mm.bing.net\/th?q=Microsoft+Tablet..."
            }
        },
        ...
    ],
    "nextOffsetAddCount": 0
}
```

Daraufhin können Sie dem Benutzer die optionalen Abfrageausdrücke anzeigen, falls diese für den Benutzer interessant sind.

Das Feld `pivotSuggestions` enthält die Liste mit den Segmenten (Pivotwerte), in die die ursprüngliche Abfrage aufgeteilt wurde. Die Antwort enthält für jeden Pivotwert eine Liste mit Objekten vom Typ [Query](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#query_obj), die wiederum vorgeschlagene Abfragen enthalten. Das Feld `text` enthält die vorgeschlagene Abfrage. Das Feld `displayText` enthält den Ausdruck, der den Pivotwert in der ursprünglichen Abfrage ersetzt. Beispiel: Veröffentlichungsdatum von Surface.

Mit den Feldern `text` und `thumbnail` können Sie dem Benutzer die Pivotabfragezeichenfolgen anzeigen, falls dieser eigentlich nach der Pivotabfragezeichenfolge suchen wollte. Verwenden Sie die URL `webSearchUrl` oder `searchLink`, um die Miniaturansicht bzw. den Text klickbar zu machen. Verwenden Sie `webSearchUrl`, um den Benutzer zu den Bing-Suchergebnissen weiterzuleiten, oder `searchLink`, falls Sie eine eigene Ergebnisseite verwenden.

<!-- Need a sanitized version of the image
The following shows an example of the pivot queries.

![Pivot suggestions](./media/cognitive-services-bing-images-api/bing-image-pivotsuggestion.GIF)
-->

## <a name="expanding-the-query"></a>Erweitern der Abfrage

Wenn Bing die Abfrage erweitern kann, um die ursprüngliche Suche einzugrenzen, enthält das Objekt [Images](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images) das Feld `queryExpansions`. Für die Abfrage *Microsoft Surface* sind beispielsweise folgende erweiterte Abfragen denkbar: „Microsoft Surface **Pro 3**“, „Microsoft Surface **RT**“, „Microsoft Surface **Phone**“ und „Microsoft Surface **Hub**“.

Das folgende Beispiel zeigt die erweiterten Abfragen für *Microsoft Surface*:

```json
{
    "_type": "Images",
    "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=microsoft%20surface...",
    "totalEstimatedMatches": 1000,
    "value": [...],
    "queryExpansions":  [{
        "text": "Microsoft Surface Pro 3",
        "displayText": "Pro 3",
        "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Microsoft+Surface+Pro+3...",
        "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=Microsoft...",
        "thumbnail": {
            "thumbnailUrl": "https:\/\/tse4.mm.bing.net\/th?q=Microsoft+Surface+Pro+3..."
        }
    },
    {
        "text": "Microsoft Surface RT",
        "displayText": "RT",
        "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Microsoft+Surface+RT...",
        "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=...",
        "thumbnail": {
            "thumbnailUrl": "https:\/\/tse4.mm.bing.net\/th?q=Microsoft+Surface+RT..."
        }
    },
    {
        "text": "Microsoft Surface Phone",
        "displayText": "Phone",
        "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Microsoft+Surface+Phone",
        "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=...",
        "thumbnail": {
            "thumbnailUrl": "https:\/\/tse4.mm.bing.net\/th?q=Microsoft+Surface+Phone..."
        }
    }],
    "pivotSuggestions": [...],
    "nextOffsetAddCount": 0
}
```

Das Feld `queryExpansions` enthält eine Liste mit Objekten vom Typ [Query](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#query_obj). Das Feld `text` enthält die erweiterte Abfrage und das Feld `displayText` den Erweiterungsausdruck. Mit den Feldern `text` und `thumbnail` können Sie dem Benutzer die erweiterten Abfragezeichenfolgen anzeigen, falls dieser eigentlich nach der erweiterten Abfragezeichenfolge suchen wollte. Verwenden Sie die URL `webSearchUrl` oder `searchLink`, um die Miniaturansicht bzw. den Text klickbar zu machen. Verwenden Sie `webSearchUrl`, um den Benutzer zu den Bing-Suchergebnissen weiterzuleiten, oder `searchLink`, falls Sie eine eigene Ergebnisseite verwenden.

<!-- Removing until we can replace with a sanitized image.
The following shows an example Bing implementation that uses expanded queries. If the user clicks the Microsoft Surface Pro 3 link, they're taken to the Bing search results page, which shows them images of the Pro 3.

![Query expansion suggestions](./media/cognitive-services-bing-images-api/bing-image-queryexpansion.GIF)
-->

## <a name="throttling-requests"></a>Drosselungsanforderungen

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Nächste Schritte

Die [Schnellstartanleitung für C#](quickstarts/csharp.md) ermöglicht einen schnellen Einstieg in die Verwendung Ihrer ersten Anforderung.

Machen Sie sich mit der Referenz für die [Bing-Bildersuche-API v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference) vertraut. Die Referenz enthält die Liste der Endpunkte, Header und Abfrageparameter, die Sie zum Anfordern von Suchergebnissen verwenden können. Darüber hinaus finden Sie dort Definitionen der Antwortobjekte.

Informationen zur Verbesserung der Benutzerfreundlichkeit für das Suchfeld finden Sie unter [What is Bing Autosuggest?](../bing-autosuggest/get-suggested-search-terms.md) (Was ist die Bing-Vorschlagssuche?). Während der Benutzer den gesuchten Ausdruck eingibt, können Sie diese API aufrufen, um relevante Abfrageausdrücke zu erhalten, die von anderen Benutzern verwendet wurden.

Machen Sie sich vorher unbedingt mit den [Verwendungs- und Anzeigeanforderungen von Bing](./useanddisplayrequirements.md) vertraut, um nicht gegen die Regeln für die Verwendung der Suchergebnisse zu verstoßen.

Wenn Sie die Bing-Bildersuche-API aufrufen, gibt Bing eine Liste mit Ergebnissen zurück. Bei der Liste handelt es sich um eine Teilmenge der gesamten Ergebnisse, die für die Abfrage relevant sind. Das Feld `totalEstimatedMatches` der Antwort enthält die geschätzte Anzahl von Bildern, die angezeigt werden können. Ausführliche Informationen zum Blättern durch die restlichen Bilder finden Sie [hier](./paging-images.md).