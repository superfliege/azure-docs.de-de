---
title: 'Schnellstart: Generieren einer Miniaturansicht – REST, Go'
titleSuffix: Azure Cognitive Services
description: In diesem Schnellstart generieren Sie eine Miniaturansicht von einem Bild, indem Sie die Maschinelles Sehen-API mit Go verwenden.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 9abd34266c5ee109d836484a9e7eb9d95c082296
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/18/2018
ms.locfileid: "53580908"
---
# <a name="quickstart-generate-a-thumbnail-using-the-rest-api-and-go-in-computer-vision"></a>Schnellstart: Generieren einer Miniaturansicht mit der REST-API und Go in Maschinelles Sehen

In diesem Schnellstart generieren Sie eine Miniaturansicht von einem Bild, indem Sie die REST-API von Maschinelles Sehen verwenden. Mit der [Get Thumbnail](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb)-Methode können Sie eine Miniaturansicht von einem Bild generieren. Sie geben die Höhe und Breite an. Diese kann sich vom Seitenverhältnis des Eingabebilds unterscheiden. Für das maschinelle Sehen wird die intelligente Zuschneidefunktion verwendet, um den gewünschten Bereich zu identifizieren und basierend auf dieser Region Zuschneidekoordinaten zu generieren.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/ai/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=cognitive-services) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

- [Go](https://golang.org/dl/) muss installiert sein.
- Sie benötigen einen Abonnementschlüssel für Maschinelles Sehen. Informationen zum Beziehen eines Abonnementschlüssels finden Sie unter [Gewusst wie: Beziehen von Abonnementschlüsseln](../Vision-API-How-to-Topics/HowToSubscribe.md).

## <a name="create-and-run-the-sample"></a>Erstellen und Ausführen des Beispiels

Führen Sie zum Erstellen und Ausführen des Beispiels die folgenden Schritte aus:

1. Kopieren Sie den folgenden Code in einen Text-Editor.
1. Nehmen Sie bei Bedarf die folgenden Änderungen im Code vor:
    1. Ersetzen Sie den `subscriptionKey`-Wert durch Ihren Abonnementschlüssel.
    1. Ersetzen Sie den Wert von `uriBase` durch die Endpunkt-URL für die Methode [Get Thumbnail](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb) in der Azure-Region, in der Sie Ihre Abonnementschlüssel bezogen haben, falls erforderlich.
    1. Ersetzen Sie den Wert von `imageUrl` optional durch die URL eines anderen Bilds, von dem Sie eine Miniaturansicht generieren möchten.
1. Speichern Sie den Code als Datei mit der Erweiterung `.go`. Beispiel: `get-thumbnail.go`.
1. Öffnen Sie ein Eingabeaufforderungsfenster.
1. Führen Sie an der Eingabeaufforderung den Befehl `go build` aus, um zur Paketerstellung die Datei zu kompilieren. Beispiel: `go build get-thumbnail.go`.
1. Führen Sie an der Eingabeaufforderung das kompilierte Paket aus. Beispiel: `get-thumbnail`.

```go
package main

import (
    "encoding/json"
    "fmt"
    "io/ioutil"
    "net/http"
    "strings"
    "time"
)

func main() {
    // Replace <Subscription Key> with your valid subscription key.
    const subscriptionKey = "<Subscription Key>"

    // You must use the same Azure region in your REST API method as you used to
    // get your subscription keys. For example, if you got your subscription keys
    // from the West US region, replace "westcentralus" in the URL
    // below with "westus".
    //
    // Free trial subscription keys are generated in the "westus" region.
    // If you use a free trial subscription key, you shouldn't need to change
    // this region.
    const uriBase =
        "https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail"
    const imageUrl =
        "https://upload.wikimedia.org/wikipedia/commons/9/94/Bloodhound_Puppy.jpg"

    const params = "?width=100&height=100&smartCropping=true"
    const uri = uriBase + params
    const imageUrlEnc = "{\"url\":\"" + imageUrl + "\"}"

    reader := strings.NewReader(imageUrlEnc)

    // Create the HTTP client
    client := &http.Client{
        Timeout: time.Second * 2,
    }

    // Create the POST request, passing the image URL in the request body
    req, err := http.NewRequest("POST", uri, reader)
    if err != nil {
        panic(err)
    }

    // Add headers
    req.Header.Add("Content-Type", "application/json")
    req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)

    // Send the request and retrieve the response
    resp, err := client.Do(req)
    if err != nil {
        panic(err)
    }

    defer resp.Body.Close()

    // Read the response body.
    // Note, data is a byte array
    data, err := ioutil.ReadAll(resp.Body)
    if err != nil {
        panic(err)
    }

    // Parse the JSON data
    var f interface{}
    json.Unmarshal(data, &f)

    // Format and display the JSON result
    jsonFormatted, _ := json.MarshalIndent(f, "", "  ")
    fmt.Println(string(jsonFormatted))
}
```

## <a name="examine-the-response"></a>Untersuchen der Antwort

Eine erfolgreiche Antwort enthält die Binärdaten zur Miniaturansicht. Wenn die Anforderung nicht erfolgreich ist, enthält die Antwort einen Fehlercode und eine Meldung, damit Sie ermitteln können, wo der Fehler liegt.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie das kompilierte Paket nicht mehr benötigen, löschen Sie das Paket sowie die Datei, die zur Paketerstellung kompiliert wurde, und schließen Sie dann das Eingabeaufforderungsfenster und den Text-Editor.

## <a name="next-steps"></a>Nächste Schritte

Erkunden Sie die Maschinelles Sehen-API, die zum Analysieren von Bildern, Erkennen von Prominenten und Sehenswürdigkeiten, Erstellen von Miniaturansichten und Extrahieren von gedrucktem sowie handschriftlichem Text verwendet wird. Wenn Sie schnell mit Ihren Experimenten mit der Maschinelles Sehen-API beginnen möchten, können Sie die [Open API-Testkonsole](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console) verwenden.

> [!div class="nextstepaction"]
> [Erkunden der Maschinelles Sehen-API](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
