---
title: Worum handelt es sich bei der Bing-Vorschlagssuche?
titlesuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie4 Sie die Bing-Vorschlagssuche-API verwenden.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-autosuggest
ms.topic: overview
ms.date: 09/12/2017
ms.author: scottwhi
ms.openlocfilehash: b5959e014b7e531b8f52fcbe6f6492576eedd61a
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55875670"
---
# <a name="what-is-bing-autosuggest"></a>Worum handelt es sich bei der Bing-Vorschlagssuche?

Wenn Sie Abfragen an eine der Bing-Suche-APIs senden, können Sie die Bing-Vorschlagssuche-API verwenden, um die Suchfeldumgebung zu verbessern. Die Bing-Vorschlagssuche-API gibt eine Liste vorgeschlagener Abfragen basierend auf der unvollständigen Abfragezeichenfolge zurück, die der Benutzer in das Suchfeld eingibt. Die Vorschläge werden in der Dropdownliste des Suchfelds angezeigt. Die vorgeschlagenen Begriffe basieren auf vorgeschlagenen Abfragen, nach denen andere Benutzer gesucht haben, und auf der Absicht des Benutzers.

Diese API wird normalerweise jedes Mal aufgerufen, wenn ein Benutzer ein neues Zeichen in das Suchfeld eingibt. Die Vollständigkeit der Abfragezeichenfolge wirkt sich auf die Relevanz der vorgeschlagenen Abfragebegriffe aus, die von der API zurückgegeben werden. Wenn die Abfragezeichenfolge vollständig ist, steigt auch die Relevanz der vorgeschlagenen Abfragebegriffe in der Liste. Beispielsweise sind die Vorschläge der API für die Eingabe *s* vermutlich weniger relevant als die Abfragen, die bei der Eingabe *sailing dinghies* zurückgegeben werden.

## <a name="getting-suggested-search-terms"></a>Abrufen vorgeschlagener Suchbegriffe

Im folgenden Beispiel wird eine Anforderung gezeigt, die die vorgeschlagene Abfragezeichenfolgen für *sail* zurückgibt. Denken Sie daran, für den Teilabfragebegriff des Benutzers eine URL-Codierung durchzuführen, wenn Sie den Abfrageparameter [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v7-reference#query) festlegen. Wenn der Benutzer also beispielsweise *sailing les* eingibt, legen Sie `q` auf `sailing+les` oder `sailing%20les` fest.

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/suggestions?q=sail&mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE
X-MSEdge-ClientIP: 999.999.999.999
X-Search-Location: lat:47.60357;long:-122.3295;re:100
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
Host: api.cognitive.microsoft.com
```

Die folgende Antwort enthält eine Liste mit [SearchAction](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v7-reference#searchaction)-Objekten, in denen die vorgeschlagenen Abfragebegriffe enthalten sind.

```json
{
    "url" : "https:\/\/www.bing.com\/search?q=sailing+lessons+seattle&FORM=USBAPI",
    "displayText" : "sailing lessons seattle",
    "query" : "sailing lessons seattle",
    "searchKind" : "WebSearch"
}, ...
```

Jeder Vorschlag enthält die Felder `displayText`, `query` und `url`. Das Feld `displayText` enthält die vorgeschlagene Abfrage, die Sie zum Auffüllen der Dropdownliste des Suchfelds verwenden. Sie müssen alle in der Antwort enthaltenen Vorschläge in der angegebenen Reihenfolge anzeigen.

Im Folgenden sehen Sie ein Beispiel für ein Dropdownsuchfeld mit vorgeschlagenen Suchbegriffen.

![Dropdownliste des Suchfelds der Vorschlagssuche](./media/cognitive-services-bing-autosuggest-api/bing-autosuggest-drop-down-list.PNG)

Wenn der Benutzer eine vorgeschlagene Abfrage aus der Dropdownliste auswählt, können Sie den Abfragebegriff im `query`-Feld verwenden, um die [Bing-Websuche-API](../bing-web-search/search-the-web.md) aufzurufen und die Ergebnisse selbst anzuzeigen. Alternativ könnten Sie die URL im `url`-Feld verwenden, um den Benutzer zur Seite mit den Bing-Suchergebnissen weiterzuleiten.

## <a name="throttling-requests"></a>Drosselungsanforderungen

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Nächste Schritte

Der Artikel [Erstellen Ihrer ersten Anforderung](quickstarts/csharp.md) ermöglicht einen schnellen Einstieg in die Verwendung Ihrer ersten Abfrage.

Machen Sie sich mit der Referenz für die [Bing-Vorschlagssuche-API v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v7-reference) vertraut. Die Referenz enthält die Liste mit den Endpunkten, Headern und Abfrageparametern, die Sie zum Anfordern von vorgeschlagenen Abfragebegriffen verwenden können, sowie die Definitionen der Antwortobjekte.

Hier erfahren Sie, wie Sie das Internet unter Verwendung der [Bing-Websuche-API](../bing-web-search/search-the-web.md) durchsuchen:

Machen Sie sich vorher unbedingt mit den [Verwendungs- und Anzeigeanforderungen von Bing](./useanddisplayrequirements.md) vertraut, um nicht gegen die Regeln für die Verwendung der Suchergebnisse zu verstoßen.
