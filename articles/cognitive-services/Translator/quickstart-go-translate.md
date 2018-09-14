---
title: 'Textübersetzung: Übersetzen von Text mit Go | Microsoft-Dokumentation'
titleSuffix: Microsoft Cognitive Services
description: In dieser Schnellstartanleitung übersetzen Sie Text von einer Sprache in eine andere. Dazu verwenden Sie die Textübersetzungs-API mit Go in Cognitive Services.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 06/29/2018
ms.author: nolachar
ms.openlocfilehash: 72d4a3bc7563d6427335f1536f787eab6d7a0de4
ms.sourcegitcommit: 4597964eba08b7e0584d2b275cc33a370c25e027
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2018
ms.locfileid: "43769791"
---
# <a name="quickstart-translate-text-with-go"></a>Schnellstart: Übersetzen von Text mit Go

In dieser Schnellstartanleitung übersetzen Sie mithilfe der Textübersetzungs-API Text von einer Sprache in eine andere.

## <a name="prerequisites"></a>Voraussetzungen

Zum Ausführen dieses Codes müssen Sie die [Go-Distribution](https://golang.org/doc/install) installieren. In diesem Beispielcode werden nur **Kernbibliotheken** verwendet, sodass keine externen Abhängigkeiten vorhanden sind.

Damit Sie die Textübersetzungs-API verwenden können, benötigen Sie darüber hinaus einen Abonnementschlüssel. Informationen hierzu finden Sie unter [Registrieren für die Textübersetzungs-API](translator-text-how-to-signup.md).

## <a name="translate-request"></a>Übersetzungsanforderung (Translate)

Mit dem folgenden Code wird mithilfe der Methode zum [Übersetzen](./reference/v3-0-translate.md) (Translate) Quelltext von einer Sprache in eine andere übersetzt.

1. Erstellen Sie ein neues Go-Projekt in Ihrem bevorzugten Code-Editor.
2. Fügen Sie den unten stehenden Code hinzu.
3. Ersetzen Sie den `subscriptionKey`-Wert durch einen für Ihr Abonnement gültigen Zugriffsschlüssel.
4. Speichern Sie die Datei mit der Erweiterung „.go“.
5. Öffnen Sie auf einem Computer, auf dem Go installiert ist, eine Eingabeaufforderung.
6. Erstellen Sie die Datei, beispielsweise „go build quickstart-translate.go“.
7. Führen Sie die Datei aus, etwa „quickstart-translate“.

```golang
package translate

import (
    "encoding/json"
    "fmt"
    "io/ioutil"
    "net/http"
    "strconv"
    "strings"
    "time"
)

func main() {
    // Replace the subscriptionKey string value with your valid subscription key
    const subscriptionKey = "<Subscription Key>"

    const uriBase = "https://api.cognitive.microsofttranslator.com"
    const uriPath = "/translate?api-version=3.0"

    // Translate to German and Italian
    const params = "&to=de&to=it"

    const uri = uriBase + uriPath + params

    const text = "Hello, world!"

    r := strings.NewReader("[{\"Text\" : \"" + text + "\"}]")

    client := &http.Client{
        Timeout: time.Second * 2,
    }

    req, err := http.NewRequest("POST", uri, r)
    if err != nil {
        fmt.Printf("Error creating request: %v\n", err)
        return
    }

    req.Header.Add("Content-Type", "application/json")
    req.Header.Add("Content-Length", strconv.FormatInt(req.ContentLength, 10))
    req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)

    resp, err := client.Do(req)
    if err != nil {
        fmt.Printf("Error on request: %v\n", err)
        return
    }
    defer resp.Body.Close()

    body, err := ioutil.ReadAll(resp.Body)
    if err != nil {
        fmt.Printf("Error reading response body: %v\n", err)
        return
    }

    var f interface{}
    json.Unmarshal(body, &f)

    jsonFormatted, err := json.MarshalIndent(f, "", "  ")
    if err != nil {
        fmt.Printf("Error producing JSON: %v\n", err)
        return
    }
    fmt.Println(string(jsonFormatted))
}
```

## <a name="translate-response"></a>Übersetzungsantwort (Translate)

Es wird eine erfolgreiche Antwort im JSON-Format zurückgegeben, wie im folgenden Beispiel gezeigt:

```json
[
  {
    "detectedLanguage": {
      "language": "en",
      "score": 1.0
    },
    "translations": [
      {
        "text": "Hallo Welt!",
        "to": "de"
      },
      {
        "text": "Salve, mondo!",
        "to": "it"
      }
    ]
  }
]
```

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich Go-Pakete für Cognitive Services-APIs aus dem [Azure SDK für Go](https://github.com/Azure/azure-sdk-for-go) auf GitHub an.

> [!div class="nextstepaction"]
> [Go-Pakete auf GitHub](https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices)
