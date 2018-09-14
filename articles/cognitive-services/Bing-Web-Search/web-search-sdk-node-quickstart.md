---
title: 'Schnellstart: Verwenden des Bing-Websuche-SDK für Node.js'
description: Hier erfahren Sie, wie Sie das Bing-Websuche-SDK für Node.js verwenden.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: quickstart
ms.date: 08/16/2018
ms.author: erhopf
ms.openlocfilehash: 7c3003ab4ba40a9d0212e7c94b6dd3bfbc8f0ca2
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2018
ms.locfileid: "43186630"
---
# <a name="quickstart-use-the-bing-web-search-sdk-for-nodejs"></a>Schnellstart: Verwenden des Bing-Websuche-SDK für Node.js

Das Bing-Websuche-SDK erleichtert die Integration der Bing-Websuche in Ihre Node.js-Anwendung. In dieser Schnellstartanleitung erfahren Sie, wie Sie einen Client instanziieren, eine Anforderung senden und die Antwort ausgeben.

Möchten Sie den Code sofort sehen? Die [Beispiele zum Bing-Websuche-SDK für Node.js](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples) sind auf GitHub verfügbar.

[!INCLUDE [bing-web-search-quickstart-signup](../../../includes/bing-web-search-quickstart-signup.md)]

## <a name="prerequisites"></a>Voraussetzungen

Im Folgenden sind die Tools aufgeführt, die Sie zum Ausführen dieser Schnellstartanleitung benötigen:

* [Node.js 6](https://nodejs.org/en/download/) oder höher
* Abonnementschlüssel  

## <a name="set-up-your-development-environment"></a>Einrichten der Entwicklungsumgebung

Als Erstes richten wir die Entwicklungsumgebung für das Node.js-Projekt ein.

1. Erstellen Sie ein neues Verzeichnis für Ihr Projekt:

    ```console
    mkdir YOUR_PROJECT
    ```

2. Erstellen Sie eine neue Paketdatei:

    ```console
    cd YOUR_PROJECT
    npm init
    ```

3. Als Nächstes installieren wir einige Azure-Module und fügen sie `package.json` hinzu:

    ```console
    npm install --save azure-cognitiveservices-websearch
    npm install --save ms-rest-azure
    ```

## <a name="create-a-project-and-declare-required-modules"></a>Erstellen eines Projekts und Deklarieren der erforderlichen Module

Erstellen Sie in dem Verzeichnis, in dem sich auch `package.json` befindet, ein neues Node.js-Projekt. Verwenden Sie dazu Ihre bevorzugte IDE oder Ihren bevorzugten Editor. Beispiel: `sample.js`.

Kopieren Sie diesen Code in Ihr Projekt. Er lädt die Module, die im vorherigen Abschnitt installiert wurden.

```javascript
const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
const WebSearchAPIClient = require('azure-cognitiveservices-websearch');
```

## <a name="instantiate-the-client"></a>Instanziieren des Clients

Dieser Code instanziiert einen Client und verwendet das Modul `azure-cognitiveservices-websearch`. Geben Sie einen gültigen Abonnementschlüssel für Ihr Azure-Konto ein, bevor Sie fortfahren.

```javascript
let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
let webSearchApiClient = new WebSearchAPIClient(credentials);
```

## <a name="make-a-request-and-print-the-results"></a>Senden einer Anforderung und Ausgeben der Ergebnisse

Verwenden Sie den Client, um eine Suchabfrage an die Bing-Websuche zu senden. Wenn die Antwort Ergebnisse für eines der Elemente im Array `properties` enthält, wird `result.value` in der Konsole ausgegeben.

```javascript
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

## <a name="run-the-program"></a>Ausführen des Programms

Im letzten Schritt wird das Programm ausgeführt.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Denken Sie daran, nach Abschluss dieses Projekts Ihren Abonnementschlüssel aus dem Anwendungscode zu entfernen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Cognitive Services SDK-Beispiele für Node.js](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)

## <a name="see-also"></a>Weitere Informationen

* [Referenz zum Azure Node SDK](https://docs.microsoft.com/javascript/api/azure-cognitiveservices-websearch/)
