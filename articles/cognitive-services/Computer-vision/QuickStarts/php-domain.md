---
title: 'Schnellstart: Verwenden eines Domänenmodells – REST, PHP – Maschinelles Sehen'
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung verwenden Sie ein Domänenmodell, um in einem Bild Sehenswürdigkeiten zu identifizieren, indem Sie die Maschinelles Sehen-API und PHP verwenden.
services: cognitive-services
author: noellelacharite
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: v-deken
ms.openlocfilehash: 40a4fa90442a5771f6b221a25ddca46d3ebc4f5c
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/15/2018
ms.locfileid: "45629490"
---
# <a name="quickstart-use-a-domain-model-using-the-rest-api-and-php-in-computer-vision"></a>Schnellstart: Verwenden eines Domänenmodells mit der REST-API und PHP in der Maschinelles Sehen-API

In dieser Schnellstartanleitung verwenden Sie ein Domänenmodell, um in einem Bild Sehenswürdigkeiten oder wahlweise Prominente zu identifizieren, indem Sie die REST-API von Maschinelles Sehen verwenden. Mit der Methode [Recognize Domain Specific Content](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e200) können Sie ein domänenspezifisches Modell anwenden, um Inhalte in einem Bild zu erkennen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/ai/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=cognitive-services) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

- [PHP](https://secure.php.net/downloads.php) muss installiert sein.
- [PEAR](https://pear.php.net) muss installiert sein.
- Sie benötigen einen Abonnementschlüssel für Maschinelles Sehen. Informationen zum Beziehen eines Abonnementschlüssels finden Sie unter [Gewusst wie: Beziehen von Abonnementschlüsseln](../Vision-API-How-to-Topics/HowToSubscribe.md).

## <a name="create-and-run-the-sample"></a>Erstellen und Ausführen des Beispiels

Führen Sie zum Erstellen und Ausführen des Beispiels die folgenden Schritte aus:

1. Installieren Sie das PHP5-Paket [`HTTP_Request2`](http://pear.php.net/package/HTTP_Request2).
   1. Öffnen Sie ein Eingabeaufforderungsfenster als Administrator.
   1. Führen Sie den folgenden Befehl aus:

      ```console
      pear install HTTP_Request2
      ```

   1. Wenn das Paket erfolgreich installiert wurde, schließen Sie das Eingabeaufforderungsfenster.

1. Kopieren Sie den folgenden Code, und fügen Sie ihn in einen Text-Editor ein.
1. Nehmen Sie bei Bedarf die folgenden Änderungen im Code vor:
    1. Ersetzen Sie den `subscriptionKey`-Wert durch Ihren Abonnementschlüssel.
    1. Ersetzen Sie den Wert von `uriBase` durch die Endpunkt-URL für die Methode [Recognize Domain Specific Content](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e200) in der Azure-Region, in der Sie Ihre Abonnementschlüssel bezogen haben, falls erforderlich.
    1. Ersetzen Sie optional den Wert von `imageUrl` durch die URL eines anderen Bilds, das analysiert werden soll.
    1. Ersetzen Sie optional den Wert des Anforderungsparameters `domain` durch `celebrites`, wenn Sie anstelle des Domänenmodells `landmarks` das Domänenmodell `celebrities` verwenden möchten.
1. Speichern Sie den Code als Datei mit der Erweiterung `.php`. Beispiel: `use-domain-model.php`.
1. Öffnen Sie ein Browserfenster mit PHP-Unterstützung.
1. Ziehen Sie die Datei in das Browserfenster, und legen Sie sie dort ab.

```php
<html>
<head>
    <title>Analyze Domain Model Sample</title>
</head>
<body>
<?php
// Replace <Subscription Key> with a valid subscription key.
$ocpApimSubscriptionKey = '<Subscription Key>';

// You must use the same location in your REST call as you used to obtain
// your subscription keys. For example, if you obtained your subscription keys
// from westus, replace "westcentralus" in the URL below with "westus".
$uriBase = 'https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/';

// Change 'landmarks' to 'celebrities' to use the Celebrities model.
$domain = 'landmarks';

$imageUrl =
    'https://upload.wikimedia.org/wikipedia/commons/2/23/Space_Needle_2011-07-04.jpg';

require_once 'HTTP/Request2.php';

$request = new Http_Request2($uriBase . 'models/' . $domain . '/analyze');
$url = $request->getUrl();

$headers = array(
    // Request headers
    'Content-Type' => 'application/json',
    'Ocp-Apim-Subscription-Key' => $ocpApimSubscriptionKey
);
$request->setHeader($headers);

$parameters = array(
    // Request parameters
    'model' => $domain
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

## <a name="examine-the-response"></a>Untersuchen der Antwort

Eine erfolgreiche Antwort wird im JSON-Format zurückgegeben. Die Beispielwebsite analysiert eine Antwort und zeigt diese bei erfolgreicher Ausführung im Eingabeaufforderungsfenster an. Im Folgenden finden Sie ein Beispiel dafür:

```json
{
    "result": {
        "landmarks": [
            {
                "name": "Space Needle",
                "confidence": 0.9998177886009216
            }
        ]
    },
    "requestId": "4d26587b-b2b9-408d-a70c-1f8121d84b0d",
    "metadata": {
        "height": 4132,
        "width": 2096,
        "format": "Jpeg"
    }
}
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Datei nicht mehr benötigen, löschen Sie sie, und deinstallieren Sie dann das PHP5-Paket `HTTP_Request2`. Führen Sie die folgenden Schritte durch, um das Paket zu deinstallieren:

1. Öffnen Sie ein Eingabeaufforderungsfenster als Administrator.
2. Führen Sie den folgenden Befehl aus:

   ```console
   pear uninstall HTTP_Request2
   ```

3. Wenn das Paket erfolgreich deinstalliert wurde, schließen Sie das Eingabeaufforderungsfenster.

## <a name="next-steps"></a>Nächste Schritte

Erkunden Sie die Maschinelles Sehen-API, die zum Analysieren von Bildern, Erkennen von Prominenten und Sehenswürdigkeiten, Erstellen von Miniaturansichten und Extrahieren von gedrucktem sowie handschriftlichem Text verwendet wird. Wenn Sie schnell mit Ihren Experimenten mit der Maschinelles Sehen-API beginnen möchten, können Sie die [Open API-Testkonsole](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console) verwenden.

> [!div class="nextstepaction"]
> [Erkunden der Maschinelles Sehen-API](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
