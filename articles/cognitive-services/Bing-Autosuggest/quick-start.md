---
title: 'Schnellstart: Bing-Vorschlagssuche-API'
titlesuffix: Azure Cognitive Services
description: In diesem Artikel lernen Sie die ersten Schritte zur Verwendung der Bing-Vorschlagssuche-API.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-autosuggest
ms.topic: quickstart
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: 446d271854a4e45bcea8c261a0dc078e549f8229
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/06/2018
ms.locfileid: "48830752"
---
# <a name="quickstart-making-your-first-autosuggest-query"></a>Schnellstart: Erstellen Ihrer ersten Abfrage für die Vorschlagssuche

Bevor Sie Ihren ersten Aufruf vornehmen können, benötigen Sie einen Cognitive Services-Abonnementschlüssel. Unter [Cognitive Services ausprobieren](https://azure.microsoft.com/try/cognitive-services/?api=autosuggest-api) erfahren Sie, wie Sie einen Schlüssel erhalten.

Um nur Websuchergebnisse zu erhalten, senden Sie eine GET-Anforderung an den folgenden Endpunkt:

```http
https://api.cognitive.microsoft.com/bing/v5.0/Suggestions
```

> [!NOTE]
> Endpunkt für V7-Vorschau:
>
> ```http
> https://api.cognitive.microsoft.com/bing/v7.0/Suggestions
> ```

Die Anforderung muss das HTTPS-Protokoll verwenden.

Alle Anforderungen sollten von einem Server stammen. Das Verteilen eines Schlüssels als Teil einer Clientanwendung birgt ein größeres Risiko, dass eine böswillige dritte Partei darauf zugreift. Aufrufe von einem Server stellen ebenso einen einzelnen Upgradepunkt für zukünftige Versionen der API dar.

Die Anforderung muss den Abfrageparameter [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#query) angeben, der den partiellen Suchbegriff des Benutzers enthält. Obwohl es optional ist, sollte die Anforderung auch den Abfrageparameter [mkt](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#mkt) angeben, wodurch der Markt identifiziert wird, von dem die Ergebnisse stammen sollen. Unter [Abfrageparameter](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#query-parameters) finden Sie eine Liste optionaler Abfrageparameter. Alle Abfrageparameter müssen URL-codiert sein.

Die Anforderung muss den [Ocp-Apim-Subscription-Key](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#subscriptionkey)-Header angeben. Auch wenn dies optional ist, sollten Sie die folgenden Header ebenfalls angeben:

- [User-Agent](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#useragent)
- [X-MSEdge-ClientID](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#clientid)
- [X-Search-ClientIP](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#clientip)
- [X-Search-Location](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#location)

Die Client-IP- und Adressheader sind für die Rückgabe von standortorientiertem Inhalt wichtig.

Eine Liste aller Anforderungs- und Antwortheader finden Sie unter [Header](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#headers).

## <a name="the-request"></a>Die Anforderung

Die Anforderung sollte alle vorgeschlagenen Abfrageparameter und Header enthalten. Diese API wird jedes Mal aufgerufen, wenn ein Benutzer ein neues Zeichen in das Suchfeld eingibt. Die Vollständigkeit der Abfragezeichenfolge wirkt sich auf die Relevanz der vorgeschlagenen Abfragebegriffe aus, die von der API zurückgegeben werden. Wenn die Abfragezeichenfolge vollständig ist, steigt auch die Relevanz der vorgeschlagenen Abfragebegriffe in der Liste. Beispielsweise sind die Vorschläge der API für die Eingabe *s* vermutlich weniger relevant als die Abfragen, die bei der Eingabe *sailing dinghies* zurückgegeben werden. 

Im folgenden Beispiel wird eine Anforderung gezeigt, die die vorgeschlagene Abfragezeichenfolgen für *sail* zurückgibt.

```http
GET https://api.cognitive.microsoft.com/bing/v5.0/suggestions?q=sail&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```

> [!NOTE]
> Vorschau der V7-Anforderung:

> ```http
> GET https://api.cognitive.microsoft.com/bing/v7.0/suggestions?q=sail&mkt=en-us HTTP/1.1
> Ocp-Apim-Subscription-Key: 123456789ABCDE
> X-MSEdge-ClientIP: 999.999.999.999
> X-Search-Location: lat:47.60357;long:-122.3295;re:100
> X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
> Host: api.cognitive.microsoft.com
> ```

Wenn Sie zuvor noch keine der Bing-APIs aufgerufen haben, lassen Sie den Client-ID-Header weg. Schließen Sie den Client-ID-Header nur ein, wenn Sie bereits eine Bing-API aufgerufen haben und Bing eine Client-ID für die Kombination aus Benutzer und Gerät zurückgegeben hat.

Das folgende Beispiel zeigt die Antwort auf die vorherige Anforderung. Die Antwort enthält eine Web-Vorschlagsgruppe, die eine Liste von Abfragevorschlägen enthält. Jeder Vorschlag enthält die Felder `displayText`, `query` und `url`.

Das `displayText`-Feld enthält die vorgeschlagene Abfrage, die Sie zum Auffüllen der Dropdownliste des Suchfelds verwenden. Sie müssen alle in der Antwort enthaltenen Vorschläge in der angegebenen Reihenfolge anzeigen.  

Wenn der Benutzer eine Abfrage aus der Dropdownliste auswählt, können Sie die Abfragezeichenfolge im `query`-Feld verwenden, um die [Bing-Suche-API](../bing-web-search/search-the-web.md) aufzurufen und die Ergebnisse selbst anzuzeigen. Alternativ könnten Sie die URL im `url`-Feld verwenden, um den Benutzer zur Seite mit den Bing-Suchergebnissen weiterzuleiten.

```  
BingAPIs-TraceId: 76DD2C2549B94F9FB55B4BD6FEB6AC
X-MSEdge-ClientID: 1C3352B306E669780D58D607B96869
BingAPIs-Market: en-US

{
    "_type" : "Suggestions",
    "queryContext" : {
        "originalQuery" : "sail"
    },
    "suggestionGroups" : [{
        "name" : "Web",
        "searchSuggestions" : [{
            "url" : "https:\/\/www.bing.com\/search?q=sailing+lessons+seattle&FORM=USBAPI",
            "displayText" : "sailing lessons seattle",
            "query" : "sailing lessons seattle",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailor+moon+news&FORM=USBAPI",
            "displayText" : "sailor moon news",
            "query" : "sailor moon news",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailor+jack%27s+lincoln+city&FORM=USBAPI",
            "displayText" : "sailor jack's lincoln city",
            "query" : "sailor jack's lincoln city",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailing+anarchy&FORM=USBAPI",
            "displayText" : "sailing anarchy",
            "query" : "sailing anarchy",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailboats+for+sale&FORM=USBAPI",
            "displayText" : "sailboats for sale",
            "query" : "sailboats for sale",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailstn.mylabsplus.com&FORM=USBAPI",
            "displayText" : "sailstn.mylabsplus.com",
            "query" : "sailstn.mylabsplus.com",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailusfood&FORM=USBAPI",
            "displayText" : "sailusfood",
            "query" : "sailusfood",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailboats+for+sale+seattle&FORM=USBAPI",
            "displayText" : "sailboats for sale seattle",
            "query" : "sailboats for sale seattle",
            "searchKind" : "WebSearch"
        }]
    }]
}
```

## <a name="next-steps"></a>Nächste Schritte

Testen Sie die API. Navigieren Sie dazu zu [Autosuggest API Testing Console (Testkonsole für die Vorschlagssuche-API)](https://dev.cognitive.microsoft.com/docs/services/56c7694ecf5ff801a090fbd1/operations/56c769a2cf5ff801a090fbd2).

Ausführliche Details zur Nutzung der Antwortobjekte finden Sie unter [Getting Suggested Search Terms (Abrufen vorgeschlagener Suchbegriffe)](./get-suggested-search-terms.md).
