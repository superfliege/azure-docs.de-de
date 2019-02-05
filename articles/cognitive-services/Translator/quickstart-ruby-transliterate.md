---
title: 'Schnellstart: Konvertieren eines Textskripts, Ruby: Textübersetzungs-API'
titleSuffix: Azure Cognitive Services
description: In diesem Schnellstart konvertieren Sie Text in einer Sprache aus einem Skript in ein anderes. Dazu verwenden Sie die Textübersetzungs-API mit Ruby.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 06/22/2018
ms.author: erhopf
ms.openlocfilehash: 10f74e04349984885f9c69ac17ac1aa9b41e710f
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55461184"
---
# <a name="quickstart-transliterate-text-with-the-translator-text-rest-api-ruby"></a>Schnellstart: Transliteration von Text mit der Textübersetzungs-REST-API (Ruby)

In dieser Schnellstartanleitung konvertieren Sie mithilfe der Textübersetzungs-API Text in einer Sprache aus einem Skript in ein anderes.

## <a name="prerequisites"></a>Voraussetzungen

Zum Ausführen dieses Codes benötigen Sie [Ruby 2.4](https://www.ruby-lang.org/en/downloads/) oder höher.

Damit Sie die Textübersetzungs-API verwenden können, benötigen Sie darüber hinaus einen Abonnementschlüssel. Informationen hierzu finden Sie unter [Registrieren für die Textübersetzungs-API](translator-text-how-to-signup.md).

## <a name="transliterate-request"></a>Transliterationsanforderung (Transliterate)

Der folgende Code konvertiert mithilfe der Methode für die [Transliteration](./reference/v3-0-transliterate.md) (Transliterate) Text in einer Sprache aus einem Skript in ein anderes Skript.

1. Erstellen Sie ein neues Ruby-Projekt in Ihrem bevorzugten Code-Editor.
2. Fügen Sie den unten stehenden Code hinzu.
3. Ersetzen Sie den `key`-Wert durch einen für Ihr Abonnement gültigen Zugriffsschlüssel.
4. Führen Sie das Programm aus.

```ruby
require 'net/https'
require 'uri'
require 'cgi'
require 'json'
require 'securerandom'

# **********************************************
# *** Update or verify the following values. ***
# **********************************************

# Replace the key string value with your valid subscription key.
key = 'ENTER KEY HERE'

host = 'https://api.cognitive.microsofttranslator.com'
path = '/transliterate?api-version=3.0'

# Transliterate text in Japanese from Japanese script (i.e. Hiragana/Katakana/Kanji) to Latin script.
params = "&language=ja&fromScript=jpan&toScript=latn";

uri = URI (host + path + params)

# Transliterate "good afternoon".
text = 'こんにちは'

content = '[{"Text" : "' + text + '"}]'

request = Net::HTTP::Post.new(uri)
request['Content-type'] = 'application/json'
request['Content-length'] = content.length
request['Ocp-Apim-Subscription-Key'] = key
request['X-ClientTraceId'] = SecureRandom.uuid
request.body = content

response = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https') do |http|
    http.request (request)
end

result = response.body.force_encoding("utf-8")

json = JSON.pretty_generate(JSON.parse(result))
puts json
```

## <a name="transliterate-response"></a>Transliterationsantwort (Transliterate)

Es wird eine erfolgreiche Antwort im JSON-Format zurückgegeben, wie im folgenden Beispiel gezeigt:

```json
[
  {
    "text": "konnnichiha",
    "script": "latn"
  }
]
```

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich den Beispielcode für diese und andere Schnellstartanleitungen (einschließlich Übersetzung und Sprachermittlung) sowie weitere Beispielprojekte für die Textübersetzung auf GitHub an.

> [!div class="nextstepaction"]
> [Ruby-Beispiele auf GitHub](https://aka.ms/TranslatorGitHub?type=&language=ruby)
