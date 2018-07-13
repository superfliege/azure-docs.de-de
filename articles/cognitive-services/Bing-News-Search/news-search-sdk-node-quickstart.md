---
title: Node-Schnellstart für das SDK für die News-Suche | Microsoft-Dokumentation
description: Einrichten der Konsolenanwendung für das SDK für die News-Suche
titleSuffix: Azure cognitive services
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: article
ms.date: 02/12/2018
ms.author: v-gedod
ms.openlocfilehash: 4ae99aa100b697a0dd75863c6f0c3c556dfa3d21
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35377939"
---
# <a name="news-search-sdk-node-quickstart"></a>Node-Schnellstart für das SDK für die News-Suche

Das SDK für die Bing-News-Suche enthält die Funktionalität der REST-API für das Abfragen von News und das Analysieren von Ergebnissen. 

Der [Quellcode der Node-Beispiele für das SDK für die Bing-News-Suche](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/newsSearch.js) ist bei GitHub verfügbar.

## <a name="application-dependencies"></a>Anwendungsabhängigkeiten

Führen Sie `npm install azure-cognitiveservices-newssearch` in Ihrer Entwicklungsumgebung aus, um eine Konsolenanwendung für das SDK für die Bing-News-Suche einzurichten.

## <a name="news-search-client"></a>Client für die News-Suche
Rufen Sie unter *Suchen* einen [Cognitive Services-Zugriffsschlüssel](https://azure.microsoft.com/try/cognitive-services/) ab. Erstellen Sie eine Instanz der `CognitiveServicesCredentials`:
```
const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
```
Instanziieren Sie dann den Client:
```
const NewsSearchAPIClient = require('azure-cognitiveservices-newssearch');
let client = new NewsSearchAPIClient(credentials);
```
Verwenden Sie den Client zur Suche mit einem Abfragetext, in diesem Fall „Winter Olympics“:
```
client.newsOperations.search('Winter Olympics').then((result) => {
    console.log(result.value);
}).catch((err) => {
    throw err;
});

```
Der Code gibt `result.value` Elemente an der Konsole aus, ohne Text zu analysieren. Die Ergebnisse pro Kategorie (sofern vorhanden) schließen Folgendes ein:
- _type: 'NewsArticle'
- _type: 'WebPage'
- _type: 'VideoObject'
- _type: 'ImageObject'

<!-- Remove until we can replace with santized version
![News results](media/node-sdk-quickstart-results.png)
-->

## <a name="next-steps"></a>Nächste Schritte

[Cognitive Services SDK-Beispiele für Node.js](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)
