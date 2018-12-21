---
title: 'Schnellstart: Abrufen von unterstützten Sprachen mit Go – Textübersetzungs-API'
titleSuffix: Azure Cognitive Services
description: In diesem Schnellstart rufen Sie eine Liste der für Übersetzung, Transliteration und Wörterbuchsuche unterstützten Sprachen sowie Beispiele ab. Dazu verwenden Sie die Textübersetzungs-API mit Go.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 12/05/2018
ms.author: erhopf
ms.openlocfilehash: 1e630d4dee3629fc256fdc97eefad259aff909e2
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/06/2018
ms.locfileid: "53000281"
---
# <a name="quickstart-use-the-translator-text-api-to-get-a-list-of-supported-languages-using-go"></a>Schnellstart: Verwenden der Textübersetzungs-API zum Abrufen einer Liste unterstützter Sprachen mit Go

In dieser Schnellstartanleitung wird beschrieben, wie Sie Go und die Textübersetzungs-REST-API nutzen, um eine GET-Anforderung zu senden, die eine Liste mit unterstützten Sprachen zurückgibt.

Für diese Schnellstartanleitung wird ein [Azure Cognitive Services-Konto](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) mit einer Textübersetzungsressource benötigt. Wenn Sie über kein Konto verfügen, können Sie über die [kostenlose Testversion](https://azure.microsoft.com/try/cognitive-services/) einen Abonnementschlüssel abrufen.

## <a name="prerequisites"></a>Voraussetzungen

Für diese Schnellstartanleitung ist Folgendes erforderlich:

* [Go](https://golang.org/doc/install)
* Ein Azure-Abonnementschlüssel für die Textübersetzung

## <a name="create-a-project-and-import-required-modules"></a>Erstellen eines Projekts und Importieren der erforderlichen Module

Erstellen Sie in Ihrer bevorzugten IDE oder Ihrem bevorzugten Editor ein neues Go-Projekt. Kopieren Sie anschließend den folgenden Codeausschnitt in Ihr Projekt in eine Datei namens `get-languages.go`.

```go
package main

import (
    "encoding/json"
    "fmt"
    "log"
    "net/http"
    "net/url"
    "os"
)
```

## <a name="create-the-main-function"></a>Erstellen der main-Funktion

Dieses Beispiel liest den Textübersetzungs-Abonnementschlüssel aus der Umgebungsvariablen `TRANSLATOR_TEXT_KEY`. Wenn Sie mit Umgebungsvariablen nicht vertraut sind, können Sie `subscriptionKey` als Zeichenfolge festlegen und die Bedingungsanweisung auskommentieren.

Kopieren Sie diesen Code in Ihr Projekt:

```go
func main() {
    /*
     * Read your subscription key from an env variable.
     * Please note: You can replace this code block with
     * var subscriptionKey = "YOUR_SUBSCRIPTION_KEY" if you don't
     * want to use env variables.
     */
    subscriptionKey := os.Getenv("TRANSLATOR_TEXT_KEY")
    if subscriptionKey == "" {
       log.Fatal("Environment variable TRANSLATOR_TEXT_KEY is not set.")
    }
    /*
     * This calls our getLanguages function, which we'll
     * create in the next section. It takes a single argument,
     * the subscription key.
     */
    getLanguages(subscriptionKey)
}
```

## <a name="create-a-function-to-get-a-list-of-supported-languages"></a>Erstellen einer Funktion zum Abrufen einer Liste mit unterstützten Sprachen

In diesem Schritt erstellen wir eine Funktion zum Abrufen einer Liste mit unterstützten Sprachen. Diese Funktion verwendet ein einzelnes Argument: Ihren Textübersetzungs-Abonnementschlüssel.

```go
func getLanguages(subscriptionKey string) {
    /*  
     * In the next few sections, we'll add code to this
     * function to make a request and handle the response.
     */
}
```

Als Nächstes erstellen wir die URL. Die URL wird mit den Methoden `Parse()` und `Query()` erstellt.

Kopieren Sie diesen Code in die Funktion `getLanguages`.

```go
// Build the request URL. See: https://golang.org/pkg/net/url/#example_URL_Parse
u, _ := url.Parse("https://api.cognitive.microsofttranslator.com/languages?api-version=3.0")
q := u.Query()
u.RawQuery = q.Encode()
```

>[!NOTE]
> Weitere Informationen zu Endpunkten, Routen und Anforderungsparametern finden Sie unter [Textübersetzungs-API 3.0: Languages](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-languages).

## <a name="build-the-request"></a>Erstellen der Anforderung

Nachdem Sie nun den Anforderungstext im JSON-Format codiert haben, können Sie Ihre POST-Anforderung erstellen und die Textübersetzungs-API aufrufen.

```go
// Build the HTTP GET request
req, err := http.NewRequest("GET", u.String(), nil)
if err != nil {
    log.Fatal(err)
}
// Add required headers
req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)
req.Header.Add("Content-Type", "application/json")

// Call the Translator Text API
res, err := http.DefaultClient.Do(req)
if err != nil {
    log.Fatal(err)
}
```

## <a name="handle-and-print-the-response"></a>Verarbeiten und Ausgeben der Antwort

Fügen Sie diesen Code der Funktion `getLanguages` hinzu, um die JSON-Antwort zu decodieren, zu formatieren und das Ergebnis auszugeben.

```go
// Decode the JSON response
var result interface{}
if err := json.NewDecoder(res.Body).Decode(&result); err != nil {
    log.Fatal(err)
}
// Format and print the response to terminal
prettyJSON, _ := json.MarshalIndent(result, "", "  ")
fmt.Printf("%s\n", prettyJSON)
```

## <a name="put-it-all-together"></a>Korrektes Zusammenfügen

Das war's: Sie haben ein einfaches Programm erstellt, das die Textübersetzungs-API aufruft und eine JSON-Antwort zurückgibt. Führen Sie das Programm jetzt aus:

```console
go run get-languages.go
```

[Auf GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Go) finden Sie das vollständige Beispiel, falls Sie Ihren Code mit unserem Code vergleichen möchten.

## <a name="sample-response"></a>Beispiel für eine Antwort

Es wird eine erfolgreiche Antwort im JSON-Format zurückgegeben, wie im folgenden Beispiel gezeigt:

```json
{
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
  },
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
  }
}
```

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich Go-Pakete für Cognitive Services-APIs aus dem [Azure SDK für Go](https://github.com/Azure/azure-sdk-for-go) auf GitHub an.

> [!div class="nextstepaction"]
> [Go-Pakete auf GitHub](https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices)

## <a name="see-also"></a>Weitere Informationen

Informieren Sie sich, wie Sie die Textübersetzungs-API für folgende Zwecke verwenden:

* [Übersetzen von Text](quickstart-go-translate.md)
* [Transliteration von Text](quickstart-go-transliterate.md)
* [Identifizieren der Sprache anhand der Eingabe](quickstart-go-detect.md)
* [Ermitteln alternativer Übersetzungen](quickstart-go-dictionary.md)
* [Bestimmen der Satzlänge aus einer Eingabe](quickstart-go-sentences.md)
