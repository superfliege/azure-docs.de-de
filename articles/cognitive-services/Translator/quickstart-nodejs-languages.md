---
title: 'Schnellstart: Abrufen von unterstützten Sprachen, Node.js: Textübersetzungs-API'
titleSuffix: Azure Cognitive Services
description: In diesem Schnellstart rufen Sie eine Liste der für Übersetzung, Transliteration und Wörterbuchsuche unterstützten Sprachen sowie Beispiele ab. Dazu verwenden Sie die Textübersetzungs-API mit Node.js.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 06/21/2018
ms.author: erhopf
ms.openlocfilehash: ff921243df078010bdc825d28b55d07333016774
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2018
ms.locfileid: "49646411"
---
# <a name="quickstart-get-supported-languages-with-the-translator-text-rest-api-nodejs"></a>Schnellstart: Abrufen von unterstützten Sprachen mit der Textübersetzungs-REST-API (Node.js)

In dieser Schnellstartanleitung rufen Sie mit der Textübersetzungs-API eine Liste der für Übersetzung, Transliteration und Wörterbuchsuche unterstützten Sprachen sowie Beispiele ab.

## <a name="prerequisites"></a>Voraussetzungen

Zum Ausführen dieses Code benötigen Sie [Node.js 6](https://nodejs.org/en/download/).

Damit Sie die Textübersetzungs-API verwenden können, benötigen Sie darüber hinaus einen Abonnementschlüssel. Informationen hierzu finden Sie unter [Registrieren für die Textübersetzungs-API](translator-text-how-to-signup.md).

## <a name="languages-request"></a>Sprachenanforderung (Languages)

Mit dem folgenden Code werden mithilfe der Methode für [Sprachen](./reference/v3-0-languages.md) (Languages) eine Liste der unterstützten Sprachen für Übersetzung, Transliteration und Wörterbuchsuche sowie Beispiele abgerufen.

1. Erstellen Sie ein neues Node.js-Projekt in Ihrem bevorzugten Code-Editor.
2. Fügen Sie den unten stehenden Code hinzu.
3. Ersetzen Sie den `subscriptionKey`-Wert durch einen für Ihr Abonnement gültigen Zugriffsschlüssel.
4. Führen Sie das Programm aus.

```javascript
'use strict';

let fs = require ('fs');
let https = require ('https');

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace the subscriptionKey string value with your valid subscription key.
let subscriptionKey = 'ENTER KEY HERE';

let host = 'api.cognitive.microsofttranslator.com';
let path = '/languages?api-version=3.0';

let output_path = 'output.txt';
let ws = fs.createWriteStream(output_path);

let response_handler = function (response) {
    let body = '';
    response.on ('data', function (d) {
        body += d;
    });
    response.on ('end', function () {
        let json = JSON.stringify(JSON.parse(body), null, 4);
        ws.write (json);
        ws.close ();
        console.log("File written.");

    });
    response.on ('error', function (e) {
        console.log ('Error: ' + e.message);
    });
};

let GetLanguages = function () {
    let request_params = {
        method : 'GET',
        hostname : host,
        path : path,
        headers : {
            'Ocp-Apim-Subscription-Key' : subscriptionKey,
        }
    };

    let req = https.request (request_params, response_handler);
    req.end ();
}

GetLanguages ();
```

## <a name="languages-response"></a>Sprachenantwort (Languages)

Es wird eine erfolgreiche Antwort im JSON-Format zurückgegeben, wie im folgenden Beispiel gezeigt:

```json
{
  "translation": {
    "af": {
      "name": "Afrikaans",
      "nativeName": "Afrikaans",
      "dir": "ltr"
    },
    "ar": {
      "name": "Arabic",
      "nativeName": "العربية",
      "dir": "rtl"
    },
...
  },
  "transliteration": {
    "ar": {
      "name": "Arabic",
      "nativeName": "العربية",
      "scripts": [
        {
          "code": "Arab",
          "name": "Arabic",
          "nativeName": "العربية",
          "dir": "rtl",
          "toScripts": [
            {
              "code": "Latn",
              "name": "Latin",
              "nativeName": "اللاتينية",
              "dir": "ltr"
            }
          ]
        },
        {
          "code": "Latn",
          "name": "Latin",
          "nativeName": "اللاتينية",
          "dir": "ltr",
          "toScripts": [
            {
              "code": "Arab",
              "name": "Arabic",
              "nativeName": "العربية",
              "dir": "rtl"
            }
          ]
        }
      ]
    },
...
  },
  "dictionary": {
    "af": {
      "name": "Afrikaans",
      "nativeName": "Afrikaans",
      "dir": "ltr",
      "translations": [
        {
          "name": "English",
          "nativeName": "English",
          "dir": "ltr",
          "code": "en"
        }
      ]
    },
    "ar": {
      "name": "Arabic",
      "nativeName": "العربية",
      "dir": "rtl",
      "translations": [
        {
          "name": "English",
          "nativeName": "English",
          "dir": "ltr",
          "code": "en"
        }
      ]
    },
...
  }
}
```

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich den Beispielcode für diese und andere Schnellstartanleitungen (einschließlich Übersetzung und Transliteration) sowie weitere Beispielprojekte für die Textübersetzung auf GitHub an.

> [!div class="nextstepaction"]
> [Node.js-Beispiele auf GitHub](https://aka.ms/TranslatorGitHub?type=&language=javascript)
