---
title: 'Schnellstart: Suchen nach Videos mithilfe des Bing-Videosuche-SDK für Node.js'
titleSuffix: Azure Cognitive Services
description: Verwenden Sie diese Schnellstartanleitung, um Videosuchanforderungen mit dem Bing-Videosuche-SDK für Node.js zu senden.
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-video-search
ms.topic: quickstart
ms.date: 01/31/2019
ms.author: rosh
ms.openlocfilehash: ffd03a900ec304a930dfc64e7860e28e0374da55
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55863754"
---
# <a name="quickstart-perform-a-video-search-with-the-bing-video-search-sdk-for-nodejs"></a>Schnellstart: Ausführen einer Videosuche mit dem Bing-Videosuche-SDK für Node.js

Verwenden Sie diese Schnellstartanleitung, um unter Verwendung des Bing-Videosuche-SDK für Node.js mit der Suche nach Nachrichten zu beginnen. Die Bing-Videosuche verfügt zwar über eine REST-API, die mit den meisten Programmiersprachen kompatibel ist, aber das SDK ist eine einfache Möglichkeit, den Dienst in Ihre Anwendungen zu integrieren. Den Quellcode für dieses Beispiel finden Sie auf [GitHub](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/videoSearch.js). Er enthält weitere Anmerkungen und Features.

## <a name="prerequisites"></a>Voraussetzungen

- [Node.js](https://www.nodejs.org/)

So richten Sie eine Konsolenanwendung mit dem SDK für die Bing-Videosuche ein:
* Führen Sie `npm install ms-rest-azure` in Ihrer Entwicklungsumgebung aus.
* Führen Sie `npm install azure-cognitiveservices-videosearch` in Ihrer Entwicklungsumgebung aus.

[!INCLUDE [cognitive-services-bing-video-search-signup-requirements](../../../../includes/cognitive-services-bing-video-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Erstellen und Initialisieren der Anwendung

1. Erstellen Sie eine neue JavaScript-Datei in Ihrer bevorzugten IDE oder einem Editor, und fügen Sie eine `require()`-Anweisung für das Bing-Videosuche-SDK und das Modul `CognitiveServicesCredentials` hinzu. Erstellen Sie eine Variable für Ihren Abonnementschlüssel. 
    
    ```javascript
    const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
    const VideoSearchAPIClient = require('azure-cognitiveservices-videosearch');
    ```

2. Erstellen Sie eine Instanz der `CognitiveServicesCredentials`-Klasse mit Ihrem Schlüssel. Verwenden Sie sie anschließend zum Erstellen einer Instanz des Clients für die Videosuche.

    ```javascript
    let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
    let client = new VideoSearchAPIClient(credentials);
    ```

## <a name="send-the-search-request"></a>Senden der Suchanforderung

1. Senden Sie mithilfe von `client.videosOperations.search()` eine Suchanforderung an die Bing-Videosuche-API. Wenn die Suchergebnisse zurückgegeben werden, verwenden Sie `.then()`, um das Ergebnis zu protokollieren.
    
    ```javascript
    client.videosOperations.search('Interstellar Trailer').then((result) => {
        console.log(result.value);
    }).catch((err) => {
        throw err;
    });
    ```

<!-- Remove until the response can be replace with a sanitized version.
The code prints `result.value` items to the console without parsing any text. The results will be:
- _type: 'VideoObjectElementType'

![Video results](media/video-search-sdk-node-results.png)
-->

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen einer Single-Page-Webanwendung](../tutorial-bing-video-search-single-page-app.md)

## <a name="see-also"></a>Weitere Informationen 

* [Worum handelt es sich bei der Bing-Videosuche-API?](../overview.md)
* [Cognitive Services SDK-Beispiele für .NET](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)