---
title: 'PHP-Schnellstart: Analysieren von Bildern mit der Maschinelles Sehen-API | Microsoft-Dokumentation'
titleSuffix: Microsoft Cognitive Services
description: In dieser Schnellstartanleitung analysieren Sie ein Bild mithilfe der Maschinelles Sehen-API mit PHP in Cognitive Services.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: v-deken
ms.openlocfilehash: 114674f47ee945717e866d97ffed747ae45decc8
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2018
ms.locfileid: "43770116"
---
# <a name="quickstart-analyze-a-remote-image---rest-php"></a>Schnellstart: Analysieren eines Remotebilds – REST und PHP

In dieser Schnellstartanleitung analysieren Sie mithilfe der Maschinelles Sehen-API ein Bild, um visuelle Merkmale zu extrahieren.

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung der Maschinelles Sehen-API benötigen Sie einen Abonnementschlüssel. Siehe [Obtaining Subscription Keys](../Vision-API-How-to-Topics/HowToSubscribe.md) (Abrufen von Abonnementschlüsseln).

## <a name="analyze-image-request"></a>Bildanalyseanforderung

Mit der [Methode zum Analysieren von Bildern](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) können Sie basierend auf dem Inhalt des Bilds visuelle Merkmale extrahieren. Sie können ein Bild hochladen oder eine Bild-URL angeben und auswählen, welche Merkmale zurückgegeben werden sollen. Zu den verfügbaren Merkmalen zählen beispielsweise:

* Eine detaillierte Liste der Tags im Zusammenhang mit dem Bildinhalt
* Eine Beschreibung des Bildinhalts in einem vollständigen Satz
* Die Koordinaten, das Geschlecht und das Alter aller im Bild enthaltenen Gesichter
* Der Bildtyp (ClipArt oder Strichzeichnung)
* Die dominante Farbe, die Akzentfarbe und, ob ein Bild schwarzweiß ist
* Die in dieser [Taxonomie](../Category-Taxonomy.md) definierte Kategorie
* Ob das Bild nicht jugendfreie oder freizügige Inhalte enthält

Führen Sie zum Ausführen des Beispiels die folgenden Schritte aus:

1. Kopieren Sie den folgenden Code in einen Editor.
1. Ersetzen Sie `<Subscription Key>` durch Ihren gültigen Abonnementschlüssel.
1. Ändern Sie `uriBase` ggf. in den Speicherort, vom dem Sie Ihren Abonnementschlüssel abgerufen haben.
1. Legen Sie optional `imageUrl` auf das Bild fest, das Sie analysieren möchten.
1. Ändern Sie optional die Sprache der Antwort (`'language' => 'en'`).
1. Speichern Sie die Datei mit der Erweiterung `.php`.
1. Öffnen Sie die Datei in einem Browserfenster mit PHP-Unterstützung.

In diesem Beispiel wird das PHP5-Paket [HTTP_Request2](http://pear.php.net/package/HTTP_Request2) verwendet.

```php
<html>
<head>
    <title>Analyze Image Sample</title>
</head>
<body>
<?php
// Replace <Subscription Key> with a valid subscription key.
$ocpApimSubscriptionKey = '<Subscription Key>';

// You must use the same location in your REST call as you used to obtain
// your subscription keys. For example, if you obtained your subscription keys
// from westus, replace "westcentralus" in the URL below with "westus".
$uriBase = 'https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/';

$imageUrl = 'http://upload.wikimedia.org/wikipedia/commons/3/3c/Shaki_waterfall.jpg';

require_once 'HTTP/Request2.php';

$request = new Http_Request2($uriBase . '/analyze');
$url = $request->getUrl();

$headers = array(
    // Request headers
    'Content-Type' => 'application/json',
    'Ocp-Apim-Subscription-Key' => $ocpApimSubscriptionKey
);
$request->setHeader($headers);

$parameters = array(
    // Request parameters
    'visualFeatures' => 'Categories,Description',
    'details' => '',
    'language' => 'en'
);
$url->setQueryVariables($parameters);

$request->setMethod(HTTP_Request2::METHOD_POST);

// Request body parameters
$body = json_encode(array('url' => $imageUrl));

// Request body
$request->setBody($body);

try
{
    $response = $request->send();
    echo "<pre>" .
        json_encode(json_decode($response->getBody()), JSON_PRETTY_PRINT) . "</pre>";
}
catch (HttpException $ex)
{
    echo "<pre>" . $ex . "</pre>";
}
?>
</body>
</html>
```

## <a name="analyze-image-response"></a>Bildanalyseantwort

Eine erfolgreiche Antwort wird im JSON-Format zurückgegeben. Hier sehen Sie ein Beispiel:

```json
{
  "categories": [
    {
      "name": "outdoor_water",
      "score": 0.9921875,
      "detail": {
        "landmarks": []
      }
    }
  ],
  "description": {
    "tags": [
      "nature",
      "water",
      "waterfall",
      "outdoor",
      "rock",
      "mountain",
      "rocky",
      "grass",
      "hill",
      "covered",
      "hillside",
      "standing",
      "side",
      "group",
      "walking",
      "white",
      "man",
      "large",
      "snow",
      "grazing",
      "forest",
      "slope",
      "herd",
      "river",
      "giraffe",
      "field"
    ],
    "captions": [
      {
        "text": "a large waterfall over a rocky cliff",
        "confidence": 0.916458423253597
      }
    ]
  },
  "requestId": "ebf5a1bc-3ba2-4c56-99b4-bbd20ba28705",
  "metadata": {
    "height": 959,
    "width": 1280,
    "format": "Jpeg"
  }
}
```

## <a name="next-steps"></a>Nächste Schritte

Erkunden Sie die Maschinelles Sehen-APIs, die zum Analysieren von Bildern, Erkennen von Prominenten und Sehenswürdigkeiten, Erstellen von Miniaturansichten und Extrahieren von gedrucktem sowie handschriftlichem Text verwendet werden. Wenn Sie schnell mit Ihren Experimenten mit den Maschinelles Sehen-APIs beginnen möchten, können Sie die [Open API-Testkonsole](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console) verwenden.

> [!div class="nextstepaction"]
> [Maschinelles Sehen-APIs erkunden](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
