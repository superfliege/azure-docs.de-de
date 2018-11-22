---
title: 'Schnellstart: Ausführen einer Suche mit Node.js – Bing-Websuche-API'
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung wird beschrieben, wie Sie die Bing-Websuche-API zum ersten Mal aufrufen, indem Sie Node.js verwenden, und wie Sie eine JSON-Antwort erhalten.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: quickstart
ms.date: 9/26/2018
ms.author: aahi
ms.openlocfilehash: 80d527baa5d919290bc80c65745bcaaf6ef5639d
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2018
ms.locfileid: "52164353"
---
# <a name="quickstart-use-nodejs-to-call-the-bing-web-search-api"></a>Schnellstart: Verwenden von Node.js zum Aufrufen der Bing-Websuche-API  

Verwenden Sie diese Schnellstartanleitung, um die Bing-Websuche-API zum ersten Mal aufzurufen und innerhalb von weniger als zehn Minuten eine JSON-Antwort zu erhalten.

[!INCLUDE [bing-web-search-quickstart-signup](../../../../includes/bing-web-search-quickstart-signup.md)]

## <a name="prerequisites"></a>Voraussetzungen

Im Folgenden sind die Tools aufgeführt, die Sie zum Ausführen dieser Schnellstartanleitung benötigen:

* [Node.js 6](https://nodejs.org/en/download/) oder höher
* Abonnementschlüssel

## <a name="create-a-project-and-declare-required-modules"></a>Erstellen eines Projekts und Deklarieren der erforderlichen Module

Erstellen Sie in Ihrer bevorzugten IDE oder Ihrem bevorzugten Editor ein neues Node.js-Projekt.
Kopieren Sie den unten angegebenen Codeausschnitt dann in Ihr Projekt in eine Datei namens `search.js`.

```javascript
// Use this simple app to query the Bing Web Search API and get a JSON response.
// Usage: node search.js "your query".
const https = require('https')
```

## <a name="set-the-subscription-key"></a>Festlegen des Abonnementschlüssels

Dieser Codeausschnitt verwendet die Umgebungsvariable `AZURE_SUBSCRIPTION_KEY` zum Speichern Ihres Abonnementschlüssels, eine gute Vorgehensweise, um die versehentliche Offenlegung Ihrer Schlüssel beim Bereitstellen von Code zu verhindern. [Klicken Sie hier](https://azure.microsoft.com/try/cognitive-services/my-apis/?apiSlug=search-api-v7), um Ihren Abonnementschlüssel zu suchen.

Wenn Sie mit der Verwendung von Umgebungsvariablen nicht vertraut sind oder diese App so schnell wie möglich ausführen möchten, können Sie `process.env['AZURE_SUBSCRIPTION_KEY']` durch Ihren Abonnementschlüssel in Form einer Zeichenfolge ersetzen.

```javascript
const SUBSCRIPTION_KEY = process.env['AZURE_SUBSCRIPTION_KEY']
if (!SUBSCRIPTION_KEY) {
  throw new Error('AZURE_SUBSCRIPTION_KEY is not set.')
}
```

## <a name="create-a-function-to-make-the-request"></a>Erstellen einer Funktion, um die Anforderung auszuführen

Diese Funktion erstellt eine sichere GET-Anforderung und speichert die Suchabfrage als Abfrageparameter im Pfad. `encodeURIComponent` wird verwendet, um ungültige Zeichen mit Escapezeichen zu versehen, und der Abonnementschlüssel wird in einem Header übergeben. Der Rückruf erhält eine [Antwort](https://nodejs.org/dist/latest-v10.x/docs/api/http.html#http_class_http_serverresponse), die folgende Ereignisse abonniert: `data` zum Aggregieren des JSON-Texts, `error` zum Protokollieren von Problemen und `end`, um zu erkennen, wann die Nachricht als abgeschlossen anzusehen ist. Wenn die App fertig ist, druckt sie die interessanten Header und den Nachrichtentext. Sie können die Farben variieren und die Tiefe nach Ihren Wünschen einstellen, eine Tiefe von `1` ergibt eine schöne Zusammenfassung der Antwort.

```javascript
function bingWebSearch(query) {
  https.get({
    hostname: 'api.cognitive.microsoft.com',
    path:     '/bing/v7.0/search?q=' + encodeURIComponent(query),
    headers:  { 'Ocp-Apim-Subscription-Key': SUBSCRIPTION_KEY },
  }, res => {
    let body = ''
    res.on('data', part => body += part)
    res.on('end', () => {
      for (var header in res.headers) {
        if (header.startsWith("bingapis-") || header.startsWith("x-msedge-")) {
          console.log(header + ": " + res.headers[header])
        }
      }
      console.log('\nJSON Response:\n')
      console.dir(JSON.parse(body), { colors: false, depth: null })
    })
    res.on('error', e => {
      console.log('Error: ' + e.message)
      throw e
    })
  })
}
```

## <a name="get-the-query"></a>Abrufen der Abfrage

Schauen wir uns die Argumente des Programms an, um die Abfrage zu finden. Das erste Argument ist der Pfad zum Knoten, das zweite der Dateiname und das dritte die Abfrage. Ist keine Abfrage vorhanden, wird die Standardabfrage von „Microsoft Cognitive Services“ verwendet.

```javascript
const query = process.argv[2] || 'Microsoft Cognitive Services'
```

## <a name="make-a-request-and-print-the-response"></a>Senden einer Anforderung und Ausgeben der Antwort

Und nachdem jetzt alles definiert ist, rufen wir unsere Funktion auf!

```javascript
bingWebSearch(query)
```

## <a name="put-it-all-together"></a>Korrektes Zusammenfügen

Der letzte Schritt ist das Ausführen Ihres Codes: `node search.js "<your query>"`.

Hier ist das vollständige Programm angegeben, falls Sie Ihren Code mit unserem Code vergleichen möchten:

```javascript
const https = require('https')
const SUBSCRIPTION_KEY = process.env['AZURE_SUBSCRIPTION_KEY']
if (!SUBSCRIPTION_KEY) {
  throw new Error('Missing the AZURE_SUBSCRIPTION_KEY environment varable')
}
function bingWebSearch(query) {
  https.get({
    hostname: 'api.cognitive.microsoft.com',
    path:     '/bing/v7.0/search?q=' + encodeURIComponent(query),
    headers:  { 'Ocp-Apim-Subscription-Key': SUBSCRIPTION_KEY },
  }, res => {
    let body = ''
    res.on('data', part => body += part)
    res.on('end', () => {
      for (var header in res.headers) {
        if (header.startsWith("bingapis-") || header.startsWith("x-msedge-")) {
          console.log(header + ": " + res.headers[header])
        }
      }
      console.log('\nJSON Response:\n')
      console.dir(JSON.parse(body), { colors: false, depth: null })
    })
    res.on('error', e => {
      console.log('Error: ' + e.message)
      throw e
    })
  })
}
const query = process.argv[2] || 'Microsoft Cognitive Services'
bingWebSearch(query)
```

## <a name="sample-response"></a>Beispiel für eine Antwort

Antworten der Bing-Websuche-API werden im JSON-Format zurückgegeben. Diese Beispielantwort wurde gekürzt, damit nur ein Ergebnis angezeigt wird.

```json
{
  "_type": "SearchResponse",
  "queryContext": {
    "originalQuery": "Microsoft Cognitive Services"
  },
  "webPages": {
    "webSearchUrl": "https://www.bing.com/search?q=Microsoft+cognitive+services",
    "totalEstimatedMatches": 22300000,
    "value": [
      {
        "id": "https://api.cognitive.microsoft.com/api/v7/#WebPages.0",
        "name": "Microsoft Cognitive Services",
        "url": "https://www.microsoft.com/cognitive-services",
        "displayUrl": "https://www.microsoft.com/cognitive-services",
        "snippet": "Knock down barriers between you and your ideas. Enable natural and contextual interaction with tools that augment users' experiences via the power of machine-based AI. Plug them in and bring your ideas to life.",
        "deepLinks": [
          {
            "name": "Face API",
            "url": "https://azure.microsoft.com/services/cognitive-services/face/",
            "snippet": "Add facial recognition to your applications to detect, identify, and verify faces using a Face API from Microsoft Azure. ... Cognitive Services; Face API;"
          },
          {
            "name": "Text Analytics",
            "url": "https://azure.microsoft.com/services/cognitive-services/text-analytics/",
            "snippet": "Cognitive Services; Text Analytics API; Text Analytics API . Detect sentiment, ... you agree that Microsoft may store it and use it to improve Microsoft services, ..."
          },
          {
            "name": "Computer Vision API",
            "url": "https://azure.microsoft.com/services/cognitive-services/computer-vision/",
            "snippet": "Extract the data you need from images using optical character recognition and image analytics with Computer Vision APIs from Microsoft Azure."
          },
          {
            "name": "Emotion",
            "url": "https://www.microsoft.com/cognitive-services/emotion-api",
            "snippet": "Cognitive Services Emotion API - microsoft.com"
          },
          {
            "name": "Bing Speech API",
            "url": "https://azure.microsoft.com/services/cognitive-services/speech/",
            "snippet": "Add speech recognition to your applications, including text to speech, with a speech API from Microsoft Azure. ... Cognitive Services; Bing Speech API;"
          },
          {
            "name": "Get Started for Free",
            "url": "https://azure.microsoft.com/services/cognitive-services/",
            "snippet": "Add vision, speech, language, and knowledge capabilities to your applications using intelligence APIs and SDKs from Cognitive Services."
          }
        ]
      }
    ]
  },
  "relatedSearches": {
    "id": "https://api.cognitive.microsoft.com/api/v7/#RelatedSearches",
    "value": [
      {
        "text": "microsoft bot framework",
        "displayText": "microsoft bot framework",
        "webSearchUrl": "https://www.bing.com/search?q=microsoft+bot+framework"
      },
      {
        "text": "microsoft cognitive services youtube",
        "displayText": "microsoft cognitive services youtube",
        "webSearchUrl": "https://www.bing.com/search?q=microsoft+cognitive+services+youtube"
      },
      {
        "text": "microsoft cognitive services search api",
        "displayText": "microsoft cognitive services search api",
        "webSearchUrl": "https://www.bing.com/search?q=microsoft+cognitive+services+search+api"
      },
      {
        "text": "microsoft cognitive services news",
        "displayText": "microsoft cognitive services news",
        "webSearchUrl": "https://www.bing.com/search?q=microsoft+cognitive+services+news"
      },
      {
        "text": "ms cognitive service",
        "displayText": "ms cognitive service",
        "webSearchUrl": "https://www.bing.com/search?q=ms+cognitive+service"
      },
      {
        "text": "microsoft cognitive services text analytics",
        "displayText": "microsoft cognitive services text analytics",
        "webSearchUrl": "https://www.bing.com/search?q=microsoft+cognitive+services+text+analytics"
      },
      {
        "text": "microsoft cognitive services toolkit",
        "displayText": "microsoft cognitive services toolkit",
        "webSearchUrl": "https://www.bing.com/search?q=microsoft+cognitive+services+toolkit"
      },
      {
        "text": "microsoft cognitive services api",
        "displayText": "microsoft cognitive services api",
        "webSearchUrl": "https://www.bing.com/search?q=microsoft+cognitive+services+api"
      }
    ]
  },
  "rankingResponse": {
    "mainline": {
      "items": [
        {
          "answerType": "WebPages",
          "resultIndex": 0,
          "value": {
            "id": "https://api.cognitive.microsoft.com/api/v7/#WebPages.0"
          }
        }
      ]
    },
    "sidebar": {
      "items": [
        {
          "answerType": "RelatedSearches",
          "value": {
            "id": "https://api.cognitive.microsoft.com/api/v7/#RelatedSearches"
          }
        }
      ]
    }
  }
}
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Tutorial: Einseitige Web-App für die Bing-Websuche](../tutorial-bing-web-search-single-page-app.md)

[!INCLUDE [bing-web-search-quickstart-see-also](../../../../includes/bing-web-search-quickstart-see-also.md)]
