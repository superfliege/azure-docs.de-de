---
title: 'Schnellstart: Durchführen einer Neuigkeitensuche – Bing-News-Suche-SDK für Node.js'
titleSuffix: Azure Cognitive Services
description: Verwenden Sie diese Schnellstartanleitung, um mit dem Bing-News-Suche-SDK für Node.js nach Nachrichten zu suchen und die Antwort zu verarbeiten.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: quickstart
ms.date: 02/12/2018
ms.author: v-gedod
ms.custom: seodec2018
ms.openlocfilehash: 0fa78474d6400f890626859c32915c8657f92d4e
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2018
ms.locfileid: "53249344"
---
# <a name="quickstart-perform-a-news-search-with-the-bing-news-search-sdk-for-nodejs"></a>Schnellstart: Durchführen einer Neuigkeitensuche mit dem Bing-News-Suche-SDK für Node.js

Das SDK für die Bing-News-Suche enthält die Funktionalität der REST-API für das Abfragen von News und das Analysieren von Ergebnissen. 

Der [Quellcode der Node-Beispiele für das SDK für die Bing-News-Suche](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/newsSearch.js) ist bei GitHub verfügbar.

## <a name="application-dependencies"></a>Anwendungsabhängigkeiten
Rufen Sie unter **Suchen** einen [Cognitive Services-Zugriffsschlüssel](https://azure.microsoft.com/try/cognitive-services/) ab.  Siehe auch [Cognitive Services-Preise – Bing-Suche-API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/) 

So richten Sie eine Konsolenanwendung mit dem SDK für die Bing-News-Suche ein:
* Führen Sie `npm install ms-rest-azure` in Ihrer Entwicklungsumgebung aus.
* Führen Sie `npm install azure-cognitiveservices-newssearch` in Ihrer Entwicklungsumgebung aus.

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
