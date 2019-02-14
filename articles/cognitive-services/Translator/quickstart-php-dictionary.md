---
title: Nachschlagen von Wörtern im bilingualen Wörterbuch, PHP – Textübersetzungs-API
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung ermitteln Sie kontextbezogene alternative Übersetzungen und Beispiele für Begriffe. Dazu verwenden Sie die Textübersetzungs-API mit PHP.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 02/08/2019
ms.author: erhopf
ms.openlocfilehash: d58a85ac8160d3e7c413f58bb0095ede8ffb30b3
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/09/2019
ms.locfileid: "55981501"
---
# <a name="quickstart-look-up-words-with-bilingual-dictionary-using-php"></a>Schnellstart: Nachschlagen von Wörtern im bilingualen Wörterbuch mithilfe von PHP

In dieser Schnellstartanleitung suchen Sie mithilfe der Textübersetzungs-API Details zu möglichen alternativen Übersetzungen für einen Begriff sowie Verwendungsbeispiele für diese alternativen Übersetzungen.

## <a name="prerequisites"></a>Voraussetzungen

Zum Ausführen des Codes benötigen Sie [PHP 5.6.x](http://php.net/downloads.php).

Damit Sie die Textübersetzungs-API verwenden können, benötigen Sie darüber hinaus einen Abonnementschlüssel. Informationen hierzu finden Sie unter [Registrieren für die Textübersetzungs-API](translator-text-how-to-signup.md).

## <a name="dictionary-lookup-request"></a>Anforderung zur Wörterbuchsuche (Dictionary Lookup)

Mit dem folgenden Code werden mithilfe der Methode zur [Wörterbuchsuche](./reference/v3-0-dictionary-lookup.md) (Dictionary Lookup) alternative Übersetzungen für ein Wort ermittelt.

1. Erstellen Sie ein neues PHP-Projekt in Ihrem bevorzugten Code-Editor.
2. Fügen Sie den unten stehenden Code hinzu.
3. Ersetzen Sie den `key`-Wert durch einen für Ihr Abonnement gültigen Zugriffsschlüssel.
4. Führen Sie das Programm aus.

```php
<?php

// NOTE: Be sure to uncomment the following line in your php.ini file.
// ;extension=php_openssl.dll

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace the subscriptionKey string value with your valid subscription key.
$key = 'ENTER KEY HERE';

$host = "https://api.cognitive.microsofttranslator.com";
$path = "/dictionary/lookup?api-version=3.0";

// Translate from English to French.
$params = "&from=en&to=fr";

$text = "great";

if (!function_exists('com_create_guid')) {
  function com_create_guid() {
    return sprintf( '%04x%04x-%04x-%04x-%04x-%04x%04x%04x',
        mt_rand( 0, 0xffff ), mt_rand( 0, 0xffff ),
        mt_rand( 0, 0xffff ),
        mt_rand( 0, 0x0fff ) | 0x4000,
        mt_rand( 0, 0x3fff ) | 0x8000,
        mt_rand( 0, 0xffff ), mt_rand( 0, 0xffff ), mt_rand( 0, 0xffff )
    );
  }
}

function DictionaryLookup ($host, $path, $key, $params, $content) {

    $headers = "Content-type: application/json\r\n" .
        "Content-length: " . strlen($content) . "\r\n" .
        "Ocp-Apim-Subscription-Key: $key\r\n" .
        "X-ClientTraceId: " . com_create_guid() . "\r\n";

    // NOTE: Use the key 'http' even if you are making an HTTPS request. See:
    // http://php.net/manual/en/function.stream-context-create.php
    $options = array (
        'http' => array (
            'header' => $headers,
            'method' => 'POST',
            'content' => $content
        )
    );
    $context  = stream_context_create ($options);
    $result = file_get_contents ($host . $path . $params, false, $context);
    return $result;
}

$requestBody = array (
    array (
        'Text' => $text,
    ),
);
$content = json_encode($requestBody);

$result = DictionaryLookup ($host, $path, $key, $params, $content);

// Note: We convert result, which is JSON, to and from an object so we can pretty-print it.
// We want to avoid escaping any Unicode characters that result contains. See:
// http://php.net/manual/en/function.json-encode.php
$json = json_encode(json_decode($result), JSON_UNESCAPED_UNICODE | JSON_PRETTY_PRINT);
echo $json;
?>
```

## <a name="dictionary-lookup-response"></a>Antwort zur Wörterbuchsuche (Dictionary Lookup)

Es wird eine erfolgreiche Antwort im JSON-Format zurückgegeben, wie im folgenden Beispiel gezeigt:

```json
[
  {
    "normalizedSource": "great",
    "displaySource": "great",
    "translations": [
      {
        "normalizedTarget": "grand",
        "displayTarget": "grand",
        "posTag": "ADJ",
        "confidence": 0.2783,
        "prefixWord": "",
        "backTranslations": [
          {
            "normalizedText": "great",
            "displayText": "great",
            "numExamples": 15,
            "frequencyCount": 34358
          },
          {
            "normalizedText": "big",
            "displayText": "big",
            "numExamples": 15,
            "frequencyCount": 21770
          },
...
        ]
      },
      {
        "normalizedTarget": "super",
        "displayTarget": "super",
        "posTag": "ADJ",
        "confidence": 0.1514,
        "prefixWord": "",
        "backTranslations": [
          {
            "normalizedText": "super",
            "displayText": "super",
            "numExamples": 15,
            "frequencyCount": 12023
          },
          {
            "normalizedText": "great",
            "displayText": "great",
            "numExamples": 15,
            "frequencyCount": 10931
          },
...
        ]
      },
...
    ]
  }
]
```

## <a name="dictionary-examples-request"></a>Anforderung zu Wörterbuchbeispielen (Dictionary Examples)

Mit den folgenden Schritten rufen Sie mithilfe der Methode für [Wörterbuchbeispiele](./reference/v3-0-dictionary-examples.md) (Dictionary Examples) kontextbezogene Beispiele zur Verwendung eines Begriffs im Wörterbuch ab.

1. Erstellen Sie ein neues PHP-Projekt in Ihrem bevorzugten Code-Editor.
2. Fügen Sie den unten stehenden Code hinzu.
3. Ersetzen Sie den `key`-Wert durch einen für Ihr Abonnement gültigen Zugriffsschlüssel.
4. Führen Sie das Programm aus.

```php
<?php

// NOTE: Be sure to uncomment the following line in your php.ini file.
// ;extension=php_openssl.dll

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace the subscriptionKey string value with your valid subscription key.
$key = 'ENTER KEY HERE';

$host = "https://api.cognitive.microsofttranslator.com";
$path = "/dictionary/examples?api-version=3.0";

// Translate from English to French.
$params = "&from=en&to=fr";

$text = "great";
$translation = "formidable";

if (!function_exists('com_create_guid')) {
  function com_create_guid() {
    return sprintf( '%04x%04x-%04x-%04x-%04x-%04x%04x%04x',
        mt_rand( 0, 0xffff ), mt_rand( 0, 0xffff ),
        mt_rand( 0, 0xffff ),
        mt_rand( 0, 0x0fff ) | 0x4000,
        mt_rand( 0, 0x3fff ) | 0x8000,
        mt_rand( 0, 0xffff ), mt_rand( 0, 0xffff ), mt_rand( 0, 0xffff )
    );
  }
}

function DictionaryExamples ($host, $path, $key, $params, $content) {

    $headers = "Content-type: application/json\r\n" .
        "Content-length: " . strlen($content) . "\r\n" .
        "Ocp-Apim-Subscription-Key: $key\r\n" .
        "X-ClientTraceId: " . com_create_guid() . "\r\n";

    // NOTE: Use the key 'http' even if you are making an HTTPS request. See:
    // http://php.net/manual/en/function.stream-context-create.php
    $options = array (
        'http' => array (
            'header' => $headers,
            'method' => 'POST',
            'content' => $content
        )
    );
    $context  = stream_context_create ($options);
    $result = file_get_contents ($host . $path . $params, false, $context);
    return $result;
}

$requestBody = array (
    array (
        'Text' => $text,
        'Translation' => $translation,
    ),
);
$content = json_encode($requestBody);

$result = DictionaryExamples ($host, $path, $key, $params, $content);

// Note: We convert result, which is JSON, to and from an object so we can pretty-print it.
// We want to avoid escaping any Unicode characters that result contains. See:
// http://php.net/manual/en/function.json-encode.php
$json = json_encode(json_decode($result), JSON_UNESCAPED_UNICODE | JSON_PRETTY_PRINT);
echo $json;
?>
```

## <a name="dictionary-examples-response"></a>Antwort zu Wörterbuchbeispielen (Dictionary Examples)

Es wird eine erfolgreiche Antwort im JSON-Format zurückgegeben, wie im folgenden Beispiel gezeigt:

```json
[
  {
    "normalizedSource": "great",
    "normalizedTarget": "formidable",
    "examples": [
      {
        "sourcePrefix": "You have a ",
        "sourceTerm": "great",
        "sourceSuffix": " expression there.",
        "targetPrefix": "Vous avez une expression ",
        "targetTerm": "formidable",
        "targetSuffix": "."
      },
      {
        "sourcePrefix": "You played a ",
        "sourceTerm": "great",
        "sourceSuffix": " game today.",
        "targetPrefix": "Vous avez été ",
        "targetTerm": "formidable",
        "targetSuffix": "."
      },
...
    ]
  }
]
```

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich den Beispielcode für diese und andere Schnellstartanleitungen (einschließlich Übersetzung und Transliteration) sowie weitere Beispielprojekte für die Textübersetzung auf GitHub an.

> [!div class="nextstepaction"]
> [PHP-Beispiele auf GitHub](https://aka.ms/TranslatorGitHub?type=&language=php)
