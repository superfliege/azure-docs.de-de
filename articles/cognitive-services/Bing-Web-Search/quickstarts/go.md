---
title: Go-Schnellstart für Azure Cognitive Services – Bing-Websuche-API | Microsoft-Dokumentation
description: Enthält Informationen zum schnellen Einstieg in die Nutzung der Sprache Go zum Abfragen der Bing-Websuche-API in Microsoft Cognitive Services unter Azure.
services: cognitive-services
author: Nhoya
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 03/09/2018
ms.author: rosh
ms.reviewer: nhoyadx@gmail.com, v-gedod
ms.openlocfilehash: 86cb67d46bca40c83c2f175ab7fdf6fbf52cb02f
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35374491"
---
# <a name="call-and-response-your-first-bing-web-search-query-in-go"></a>Aufruf und Antwort: Ihre erste Bing-Websuche-Abfrage in Go

Die Bing-Websuche-API ähnelt Bing.com, indem Suchergebnisse zurückgegeben werden, die für die Abfrage des Benutzers relevant sind. Die Ergebnisse können Webseiten, Bilder, Videos, Nachrichten und Entitäten sowie verwandte Suchabfragen, Rechtschreibkorrekturen, Zeitzonen, Einheitenkonvertierung, Übersetzungen und Berechnungen enthalten. Die Ergebnisse basieren auf ihrer Relevanz und der Ebene der von Ihnen abonnierten Bing-Suche-APIs.

Die [API-Referenz](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference) enthält ausführliche Informationen zu den APIs.

## <a name="prerequisites"></a>Voraussetzungen
Sie benötigen ein [Cognitive Services-API-Konto](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) mit **Bing-Suche-APIs**. Die [kostenlose Testversion](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) ist für diesen Schnellstart ausreichend. Sie benötigen den Zugriffsschlüssel, den Sie beim Aktivieren Ihrer kostenlosen Testversion erhalten, oder Sie können den Schlüssel eines kostenpflichtigen Abonnements aus Ihrem Azure-Dashboard verwenden.

Installieren Sie die [Go-Binärdateien](https://golang.org/dl/).
 
In diesem Tutorial werden nur **Kernbibliotheken** verwendet, sodass keine externen Abhängigkeiten vorhanden sind.

## <a name="core-libraries"></a>Kernbibliotheken

Verwenden Sie `http`, um die Anforderung an den Endpunkt zu senden, `ioutil`, um die Antwort zu lesen, `time` und `json` zum Verarbeiten des JSON-Codes und `fmt` zum Drucken der Ausgabe.

```
package main

import (
    "fmt"
    "net/http"
    "io/iutil"
    "time"
    "encoding/json"
)
```

## <a name="define-variables"></a>Definieren von Variablen
Legen Sie den API-Endpunkt und den Suchbegriff fest.

```
//This is the valid endpoint at the time of the writing
const endpoint = "https://api.cognitive.microsoft.com/bing/v7.0/search"
//API token
token := "YOUR-ACCESS-KEY"
searchTerm := "Microsoft Cognitive Services"
```

## <a name="building-and-sending-the-request"></a>Erstellen und Senden der Anforderung

```
//Declare a new GET request
req, err := http.NewRequest("GET", endpoint, nil)
if err != nil {
    panic(err)
}
//Add the payload to the request
param := req.URL.Query()
param.Add("q", searchTerm)
//Encoding the payload
req.URL.RawQuery = param.Encode()

//Insert the API token in the request header
req.Header.Add("Ocp-Apim-Subscription-Key", token)

//create new client
client := new(http.Client)
//Send the request
resp, err := client.Do(req)
if err != nil {
    panic(err)
}
//Close the response body at the function exit
defer resp.Body.Close() 
```

## <a name="printing-the-answer"></a>Drucken der Antwort
Das Suchergebnis befindet sich in der Variablen `resp`. Drucken Sie den Antworttext aus der Variablen aus.

```
body, err := ioutil.ReadAll(resp.Body)
if err != nil {
    panic(err)
}
//Convert body from byte to string.
fmt.Println(string(body))
```

## <a name="put-everything-together"></a>Zusammenfügen der Bestandteile

```
package main
import (
    "fmt"
    "net/http"
    "io/ioutil"
    "time"
    "encoding/json"
)

//The struct that will contain the answer
type BingAnswer struct {
        Type         string `json:"_type"`
        QueryContext struct {
                OriginalQuery string `json:"originalQuery"`
        } `json:"queryContext"`
        WebPages struct {
                WebSearchURL          string `json:"webSearchUrl"`
                TotalEstimatedMatches int    `json:"totalEstimatedMatches"`
                Value                 []struct {
                        ID               string    `json:"id"`
                        Name             string    `json:"name"`
                        URL              string    `json:"url"`
                        IsFamilyFriendly bool      `json:"isFamilyFriendly"`
                        DisplayURL       string    `json:"displayUrl"`
                        Snippet          string    `json:"snippet"`
                        DateLastCrawled  time.Time `json:"dateLastCrawled"`
                        SearchTags       []struct {
                                Name    string `json:"name"`
                                Content string `json:"content"`
                        } `json:"searchTags,omitempty"`
                        About []struct {
                                Name string `json:"name"`
                        } `json:"about,omitempty"`
                } `json:"value"`
        } `json:"webPages"`
        RelatedSearches struct {
                ID    string `json:"id"`
                Value []struct {
                        Text         string `json:"text"`
                        DisplayText  string `json:"displayText"`
                        WebSearchURL string `json:"webSearchUrl"`
                } `json:"value"`
        } `json:"relatedSearches"`
        RankingResponse struct {
                Mainline struct {
                        Items []struct {
                                AnswerType  string `json:"answerType"`
                                ResultIndex int    `json:"resultIndex"`
                                Value       struct {
                                        ID string `json:"id"`
                                } `json:"value"`
                        } `json:"items"`
                } `json:"mainline"`
                Sidebar struct {
                        Items []struct {
                                AnswerType string `json:"answerType"`
                                Value      struct {
                                        ID string `json:"id"`
                                } `json:"value"`
                        } `json:"items"`
                } `json:"sidebar"`
        } `json:"rankingResponse"`
}

func main() {
    const endpoint = "https://api.cognitive.microsoft.com/bing/v7.0/search"
    token := "YOUR-ACCESS-KEY"
    searchTerm := "Microsoft Cognitive Services"
    
    req, err := http.NewRequest("GET", endpoint, nil)
    if err != nil {
        panic(err)
    }
    
    param := req.URL.Query()
    param.Add("q", searchTerm)
    req.URL.RawQuery = param.Encode()

    req.Header.Add("Ocp-Apim-Subscription-Key", token)
    
    client := new(http.Client)
    resp, err := client.Do(req)
    if err != nil {
        panic(err)
    }
    defer resp.Body.Close() 
    body, err := ioutil.ReadAll(resp.Body)
    if err != nil {
        panic(err)
    }
    //creating a new answer struct
    ans := new(BingAnswer)
    err = json.Unmarshal(body, &ans)
    if err != nil {
         fmt.Println(err)
    }
    //Iterate over search results
    for _, result := range ans.WebPages.Value {
         //Printing result name and URL
         fmt.Println(result.Name, "||", result.URL)
    }

}
```

## <a name="next-steps"></a>Nächste Schritte

[Übersicht zur Bing-Websuche](../overview.md)  
[Testen](https://azure.microsoft.com/services/cognitive-services/bing-web-search-api/)  
[Zugriffsschlüssel für eine kostenlose Testversion](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api)
[Referenz zur Bing-Websuche-API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference)

