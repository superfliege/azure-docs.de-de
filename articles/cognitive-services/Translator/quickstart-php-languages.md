---
title: 'Schnellstart: Abrufen von unterstützten Sprachen, PHP – Textübersetzungs-API'
titleSuffix: Azure Cognitive Services
description: In diesem Schnellstart rufen Sie eine Liste der für Übersetzung, Transliteration und Wörterbuchsuche unterstützten Sprachen sowie Beispiele ab. Dazu verwenden Sie die Textübersetzungs-API mit PHP.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 02/08/2019
ms.author: erhopf
ms.openlocfilehash: 1b91c5801a64581098250468c2cd1df448a7a7b1
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/09/2019
ms.locfileid: "55976588"
---
# <a name="quickstart-get-supported-languages-with-the-translator-text-rest-api-php"></a>Schnellstart: Abrufen von unterstützten Sprachen mit der Textübersetzungs-REST-API (PHP)

In dieser Schnellstartanleitung rufen Sie mit der Textübersetzungs-API eine Liste der für Übersetzung, Transliteration und Wörterbuchsuche unterstützten Sprachen sowie Beispiele ab.

## <a name="prerequisites"></a>Voraussetzungen

Zum Ausführen des Codes benötigen Sie [PHP 5.6.x](http://php.net/downloads.php).

## <a name="languages-request"></a>Sprachenanforderung (Languages)

Mit dem folgenden Code werden mithilfe der Methode für [Sprachen](./reference/v3-0-languages.md) (Languages) eine Liste der unterstützten Sprachen für Übersetzung, Transliteration und Wörterbuchsuche sowie Beispiele abgerufen.

1. Erstellen Sie ein neues PHP-Projekt in Ihrem bevorzugten Code-Editor.
2. Fügen Sie den unten stehenden Code hinzu.
3. Führen Sie das Programm aus.

```php
<?php
// NOTE: Be sure to uncomment the following line in your php.ini file.
// ;extension=php_openssl.dll
$host = "https://api.cognitive.microsofttranslator.com";
$path = "/languages?api-version=3.0";
$output_path = "output.txt";
function GetLanguages ($host, $path) {
    $headers = "Content-type: text/xml\r\n";
    // NOTE: Use the key 'http' even if you are making an HTTPS request. See:
    // http://php.net/manual/en/function.stream-context-create.php
    $options = array (
        'http' => array (
            'header' => $headers,
            'method' => 'GET'
        )
    );
    $context  = stream_context_create ($options);
    $result = file_get_contents ($host . $path, false, $context);
    return $result;
}
$result = GetLanguages ($host, $path);
// Note: We convert result, which is JSON, to and from an object so we can pretty-print it.
// We want to avoid escaping any Unicode characters that result contains. See:
// http://php.net/manual/en/function.json-encode.php
$json = json_encode(json_decode($result), JSON_UNESCAPED_UNICODE | JSON_PRETTY_PRINT);
// Write the output to file.
$out = fopen($output_path, 'w');
fwrite($out, $json);
fclose($out);
?>
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
> [PHP-Beispiele auf GitHub](https://aka.ms/TranslatorGitHub?type=&language=php)
