---
title: Senden von Abfragen an die Bing-Bildersuche-API | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie Suchabfragen, die für die Bing-Bildersuche-API bestimmt sind, senden und anpassen.
services: cognitive-services
author: aahill
manager: cgronlun
ms.assetid: C2862E98-8BCC-423B-9C4A-AC79A287BE38
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: conceptual
ms.date: 8/8/2018
ms.author: aahi
ms.openlocfilehash: d74f59ffcf095e639686a3ada3b09dac988fc544
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2018
ms.locfileid: "40082576"
---
# <a name="sending-queries-to-the-bing-image-search-api"></a>Senden von Abfragen an die Bing-Bildersuche-API

Die Bing-Bildersuche-API bietet eine ähnliche Erfahrung wie „Bing.com/Images“: Sie können eine Suchabfrage eines Benutzers an Bing senden und eine Liste mit relevanten Bildern erhalten.

## <a name="using-and-suggesting-search-terms"></a>Verwenden und Vorschlagen von Suchbegriffen

Nachdem ein Suchbegriff eingegeben wurde, sollten Sie den Begriff mit einer URL-Codierung versehen, bevor Sie den Abfrageparameter [**q**](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#query) festlegen. Wenn ein Benutzer also beispielsweise *sailing dinghies* eingibt, legen Sie `q` auf `sailing+dinghies` oder `sailing%20dinghies` fest.

Wenn Ihre App über ein Suchfeld verfügt, in das Suchbegriffe eingegeben werden, können Sie die [Bing-Vorschlagssuche-API](../../bing-autosuggest/get-suggested-search-terms.md) zum Verbessern der Benutzerfreundlichkeit verwenden, indem in Echtzeit vorgeschlagene Suchbegriffe angezeigt werden. Die API gibt vorgeschlagene Abfragezeichenfolgen zurück, die auf Teilen von Suchbegriffen und Azure Cognitive Services basieren.

## <a name="pivoting-the-query"></a>Pivotieren der Abfrage

Wenn die ursprüngliche Suchabfrage von Bing segmentiert werden kann, enthält das zurückgegebene [Images](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images)-Objekt `pivotSuggestions`, die für Benutzer als optionale Suchbegriffe angezeigt werden können. Lautet die ursprüngliche Abfrage also beispielsweise *Microsoft Surface*, kann Bing die Abfrage in *Microsoft* und *Surface* segmentieren und dafür jeweils Pivotvorschläge anzeigen. Diese können für Benutzer als optionale Abfragebegriffe angezeigt werden.

Das folgende Beispiel veranschaulicht die Pivotvorschläge für *Microsoft Surface*:  

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

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Nächste Schritte

Verwenden Sie die [Schnellstartanleitung](../quickstarts/csharp.md), falls Sie die Bing-Bildersuche-API noch nicht ausprobiert haben. Falls Sie nach einem komplexeren Ansatz suchen, können Sie das Tutorial zur Erstellung einer [Einzelseiten-Web-App](../tutorial-bing-image-search-single-page-app.md) ausprobieren.
