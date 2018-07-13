---
title: Node-Schnellstart für das Websuche-SDK | Microsoft-Dokumentation
description: Einrichten des Websuche-SDK für die Konsolenanwendung
titleSuffix: Azure cognitive services
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 02/12/2018
ms.author: v-gedod
ms.openlocfilehash: 44f7f97f6c442df3fbb1e5e08189b8db7d4b9db0
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35377962"
---
# <a name="web-search-sdk-node-quickstart"></a>Node-Schnellstart für das Websuche-SDK

Das Bing-Websuche-SDK enthält die Funktionalität der REST-API für Webabfragen und das Analysieren von Ergebnissen.

Der [Beispielquellcode für das Bing-Websuche-SDK für Node](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/webSearch.js) ist auf GitHub verfügbar.

## <a name="application-dependencies"></a>Anwendungsabhängigkeiten

Führen Sie zum Einrichten einer Konsolenanwendung mit dem Bing-Websuche-SDK in Ihrer Entwicklungsumgebung `npm install azure-cognitiveservices-websearch` aus.

## <a name="web-search-client"></a>Websuche-Client
Rufen Sie unter *Suchen* einen [Cognitive Services-Zugriffsschlüssel](https://azure.microsoft.com/try/cognitive-services/) ab. Erstellen Sie eine Instanz der `CognitiveServicesCredentials`:
```
const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
```
Instanziieren Sie dann den Client:
```
const WebSearchAPIClient = require('azure-cognitiveservices-websearch');
let webSearchApiClient = new WebSearchAPIClient(credentials);
```
Suchen Sie nach Ergebnissen:
```
webSearchApiClient.web.search('seahawks').then((result) => {
    let properties = ["images", "webPages", "news", "videos"];
    for (let i = 0; i < properties.length; i++) {
        if (result[properties[i]]) {
            console.log(result[properties[i]].value);
        } else {
            console.log(`No ${properties[i]} data`);
        }
    }
}).catch((err) => {
    throw err;
})

```
Der Code gibt `result.value` Elemente an der Konsole aus, ohne Text zu analysieren.  Die Ergebnisse pro Kategorie (sofern vorhanden) schließen Folgendes ein:
- _type: 'ImageObject'
- _type: 'NewsArticle'
- _type: 'WebPage'
- _type: 'VideoObjectElementType'

<!-- Remove until this can be replaced with a sanitized version.
![Video results](media/web-search-sdk-node-results.png)
-->

## <a name="next-steps"></a>Nächste Schritte

[Cognitive Services SDK-Beispiele für Node.js](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)
