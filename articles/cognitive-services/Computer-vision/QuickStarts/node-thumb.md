---
title: 'Schnellstart: Generieren einer Miniaturansicht – REST, Node.js– Maschinelles Sehen-API'
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung generieren Sie eine Miniaturansicht aus einem Bild, indem Sie die Maschinelles Sehen-API mit Node.js in Cognitive Services verwenden.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: v-deken
ms.openlocfilehash: ebc9c89969641ace679b3d6677efd67138424712
ms.sourcegitcommit: 3d0295a939c07bf9f0b38ebd37ac8461af8d461f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2018
ms.locfileid: "43842594"
---
# <a name="quickstart-generate-a-thumbnail---rest-nodejs---computer-vision"></a>Schnellstart: Generieren einer Miniaturansicht – REST, Node.js– Maschinelles Sehen-API

In dieser Schnellstartanleitung generieren Sie aus einem Bild eine Miniaturansicht, indem Sie die Maschinelles Sehen-API verwenden.

## <a name="prerequisites"></a>Voraussetzungen

Zum Verwenden der Maschinelles Sehen-API benötigen Sie einen Abonnementschlüssel. Siehe [Obtaining Subscription Keys](../Vision-API-How-to-Topics/HowToSubscribe.md) (Abrufen von Abonnementschlüsseln).

## <a name="get-thumbnail-request"></a>Get Thumbnail-Anforderung

Mit der [Get Thumbnail-Methode](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb) können Sie aus einem Bild eine Miniaturansicht generieren. Sie geben die Höhe und Breite an. Diese kann sich vom Seitenverhältnis des Eingabebilds unterscheiden. Für das maschinelle Sehen wird die intelligente Zuschneidefunktion verwendet, um die gewünschte Region zu identifizieren und basierend auf dieser Region Zuschneidekoordinaten zu generieren.

Führen Sie zum Ausführen des Beispiels die folgenden Schritte aus:

1. Kopieren Sie den folgenden Code in einen Editor.
1. Ersetzen Sie `<Subscription Key>` durch Ihren gültigen Abonnementschlüssel.
1. Ändern Sie den Wert von `uriBase` ggf. in die Region, in der Sie Ihre Abonnementschlüssel erhalten haben.
1. Ändern Sie optional den Wert von `imageUrl` in das Bild, das Sie analysieren möchten.
1. Speichern Sie die Datei mit der Erweiterung `.js`.
1. Öffnen Sie die Node.js-Eingabeaufforderung, und führen Sie die Datei aus, z.B. `node myfile.js`.

In diesem Beispiel wird das npm-[request](https://www.npmjs.com/package/request)-Paket verwendet.

```nodejs
'use strict';

const request = require('request');

// Replace <Subscription Key> with your valid subscription key.
const subscriptionKey = '<Subscription Key>';

// You must use the same location in your REST call as you used to get your
// subscription keys. For example, if you got your subscription keys from
// westus, replace "westcentralus" in the URL below with "westus".
const uriBase =
    'https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail';

const imageUrl =
    'https://upload.wikimedia.org/wikipedia/commons/9/94/Bloodhound_Puppy.jpg';

// Request parameters.
const params = {
    'width': '100',
    'height': '100',
    'smartCropping': 'true'
};

const options = {
    uri: uriBase,
    qs: params,
    body: '{"url": ' + '"' + imageUrl + '"}',
    headers: {
        'Content-Type': 'application/json',
        'Ocp-Apim-Subscription-Key' : subscriptionKey
    }
};

request.post(options, (error, response, body) => {
  if (error) {
    console.log('Error: ', error);
    return;
  }
});
```

## <a name="get-thumbnail-response"></a>Get Thumbnail-Antwort

Eine erfolgreiche Antwort enthält die Binärdaten zur Miniaturansicht. Wenn die Anforderung nicht erfolgreich ist, enthält die Antwort einen Fehlercode und eine Meldung, damit Sie ermitteln können, wo der Fehler liegt.

## <a name="next-steps"></a>Nächste Schritte

Erkunden Sie die Maschinelles Sehen-APIs, die zum Analysieren von Bildern, Erkennen von Prominenten und Sehenswürdigkeiten, Erstellen von Miniaturansichten und Extrahieren von gedrucktem sowie handschriftlichem Text verwendet werden. Wenn Sie schnell mit Ihren Experimenten mit den Maschinelles Sehen-APIs beginnen möchten, können Sie die [Open API-Testkonsole](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console) verwenden.

> [!div class="nextstepaction"]
> [Maschinelles Sehen-APIs erkunden](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
