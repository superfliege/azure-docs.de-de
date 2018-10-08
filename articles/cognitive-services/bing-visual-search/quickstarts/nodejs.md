---
title: 'Schnellstart: Erstellen einer Abfrage für die visuelle Suche, Node.js – Visuelle Bing-Suche'
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie ein Bild in die API für die visuelle Bing-Suche hochladen und dadurch Informationen zu diesem Bild erhalten.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: quickstart
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: b13738c5bfd8fc75224bf934ae8be56e7c2edd69
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47225496"
---
# <a name="quickstart-your-first-bing-visual-search-query-in-javascript"></a>Schnellstart: Ihre erste Abfrage für die visuelle Bing-Suche in JavaScript

Die API für die visuelle Bing-Suche gibt Informationen zu einem von Ihnen bereitgestellten Bild zurück. Sie können ein Bild über die Bild-URL, ein Erkenntnistoken oder durch Hochladen des Bilds bereitstellen. Informationen zu diesen Optionen finden Sie unter [Was ist die API für die visuelle Bing-Suche?](../overview.md). In diesem Artikel wird gezeigt, wie Sie ein Bild hochladen. Das Hochladen eines Bilds ist besonders in Szenarien mit einem mobilen Gerät nützlich, wenn Sie eine bekannte Sehenswürdigkeit fotografiert haben und Informationen dazu erhalten möchten. Die Informationen können z.B. Wissenswertes zur Sehenswürdigkeit beinhalten. 

Wenn Sie ein lokales Bild hochladen, müssen Sie die folgenden Formulardaten in den Text der POST-Anforderung einfügen. Die Formulardaten müssen den Header „Content-Disposition“ enthalten. Der `name`-Parameter muss auf „image“ festgelegt werden. Der `filename`-Parameter kann auf eine beliebige Zeichenfolge festgelegt werden. Der Inhalt des Formulars sind die Binärdaten des Bilds. Sie können eine maximale Bildgröße von 1 MB hochladen. 

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

In diesem Artikel wird eine einfache Konsolenanwendung gezeigt, die eine Anforderung an die API für die visuelle Bing-Suche sendet und die Suchergebnisse im JSON-Format anzeigt. Die Anwendung ist zwar in JavaScript geschrieben, an sich ist die API aber ein RESTful-Webdienst, der mit jeder Programmiersprache kompatibel ist, die HTTP-Anforderung stellen und JSON analysieren kann. 

## <a name="prerequisites"></a>Voraussetzungen

Zum Ausführen dieses Code benötigen Sie [Node.js 6](https://nodejs.org/en/download/).

Für diese Schnellstartanleitung können Sie den Schlüssel eines [kostenlosen Testabonnements](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) oder eines kostenpflichtigen Abonnements verwenden.

## <a name="running-the-application"></a>Ausführen der Anwendung

Das folgende Beispiel zeigt, wie Sie eine Nachricht mit Formulardaten in Node.js senden.

Führen Sie die folgenden Schritte aus, um eine Anwendung auszuführen:

1. Erstellen Sie einen Ordner für Ihr Projekt (oder verwenden Sie Ihre bevorzugte IDE oder Ihren bevorzugten Editor).
2. Navigieren Sie an einer Eingabeaufforderung oder einem Terminal zu dem Ordner, den Sie gerade erstellt haben.
3. Installieren Sie die Anforderungsmodule:  
  ```  
  npm install request  
  ```  
3. Installieren Sie die Formulardatenmodule:  
  ```  
  npm install form-data  
  ```  
4. Erstellen Sie eine Datei mit dem Namen „GetVisualInsights.js“, und fügen Sie dieser den folgenden Code hinzu.
5. Ersetzen Sie den `subscriptionKey`-Wert durch Ihren Abonnementschlüssel.
6. Ersetzen Sie den `imagePath`-Wert durch den Pfad des Bilds, das Sie hochladen möchten.
7. Führen Sie das Programm aus.  
  ```
  node GetVisualInsights.js
  ```

```javascript
var request = require('request');
var FormData = require('form-data');
var fs = require('fs');

var baseUri = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch';
var subscriptionKey = '<yoursubscriptionkeygoeshere>';
var imagePath = "<pathtoyourimagegoeshere>";

var form = new FormData();
form.append("image", fs.createReadStream(imagePath));

form.getLength(function(err, length){
  if (err) {
    return requestCallback(err);
  }

  var r = request.post(baseUri, requestCallback);
  r._form = form; 
  r.setHeader('Ocp-Apim-Subscription-Key', subscriptionKey);
});

function requestCallback(err, res, body) {
    console.log(JSON.stringify(JSON.parse(body), null, '  '))
}
```


## <a name="next-steps"></a>Nächste Schritte

[Abrufen von Informationen zu einem Bild mithilfe eines Erkenntnistokens](../use-insights-token.md)  
[Tutorial zum Bildupload für die visuelle Bing-Suche](../tutorial-visual-search-image-upload.md)
[Tutorial zu einer Single-Page-App für die visuelle Bing-Suche](../tutorial-bing-visual-search-single-page-app.md)  
[Übersicht über die visuelle Bing-Suche](../overview.md)  
[Testen](https://aka.ms/bingvisualsearchtryforfree)  
[Abrufen eines Zugriffsschlüssels für eine kostenlose Testversion](https://azure.microsoft.com/try/cognitive-services/?api=bing-visual-search-api)  
[Referenz zur API für die visuelle Bing-Suche](https://aka.ms/bingvisualsearchreferencedoc)
