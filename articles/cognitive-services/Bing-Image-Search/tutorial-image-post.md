---
title: Bing-Bildupload für Einblicke | Microsoft-Dokumentation
description: Konsolenanwendung, die die Bing-Bildersuche-API zum Hochladen eines Bildes verwendet, um Bildeinblicke zu erhalten.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 12/07/2017
ms.author: v-gedod
ms.openlocfilehash: f0bf32a9951527a072fffe464f6b5f50d0f237a2
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35372842"
---
# <a name="tutorial-bing-image-upload-for-insights"></a>Tutorial: Bing-Bildupload für Einblicke

Die Bing-Bildersuche-API bietet eine `POST`-Option, um ein Bild hochzuladen und nach Informationen zu suchen, die für das Bild relevant sind. Diese C#-Konsolenanwendung lädt ein Bild mit dem Bildersucheendpunkt hoch, um Details über das Bild zu erhalten.
Die Ergebnisse sind JSON-Objekten, wie z.B. folgende:

![[JSON-Ergebnisse]](media/cognitive-services-bing-images-api/jsonResult.jpg)

In diesem Tutorial werden folgende Punkte erläutert:

> [!div class="checklist"]
> * Verwenden des `/details`-Endpunkts der Bildersuche in einer `POST`-Anforderung
> * Angeben von Headern für die Anforderung
> * Verwenden von URL-Parametern, um Ergebnisse anzugeben
> * Hochladen der Bilddaten und senden der `POST`-Anforderung
> * Ausgeben von JSON-Ergebnisse an die Konsole

## <a name="app-components"></a>App-Komponenten

Das Tutorial umfasst drei Teile:

> [!div class="checklist"]
> * Endpunktkonfiguration zum Angeben des Endpunkts für die Bing-Bildersuche und der erforderlichen Header
> * Bilddateiupload für `POST`-Anforderung an den Endpunkt
> * Analysieren der JSON-Ergebnisse, als Details von der `POST`-Anforderung zurückgegeben wurden

## <a name="scenario-overview"></a>Übersicht über das Szenario
Es gibt [drei Bildersucheendpunkte](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/image-search-endpoint). Der `/details`-Endpunkt kann eine `POST`-Anforderung mit Bilddaten im Text der Anforderung verwenden.
```
https://api.cognitive.microsoft.com/bing/v7.0/images/details
```
Der `modules`-URL-Parameter nach `/details?` gibt an, welche Art von Details die Ergebnisse enthalten:
* `modules=All`
* `modules=RecognizedEntities` (im Bild sichtbare Personen oder Orte)

Geben Sie `modules=All` in der `POST`-Anforderung an, um JSON-Text mit der folgenden Liste abzurufen:
* `bestRepresentativeQuery` -eine Bing-Abfrage, die ähnliche Bilder wie das hochgeladene Bild zurückgibt
* `detectedObjects` z.B.ein Begrenzungsrahmen oder Hotspots im Bild
* `image`-Metadata
* `imageInsightsToken` -Token für eine nachfolgende `GET` -Anforderung, die `RecognizedEntities` abruft (im Bild sichtbare Personen oder Orte) 
* `imageTags`
* `pagesIncluding` -Webseiten, die das Bild enthalten
* `relatedSearches`
* `visuallySimilarImages`

Geben Sie `modules=RecognizedEntities` in der `POST`-Anforderung an, um nur `imageInsightsToken` abzurufen, das in einer nachfolgenden `GET`-Anforderung verwendet wird. Damit werden im Bild sichtbare Personen oder Orte identifiziert.

## <a name="webclient-and-headers-for-the-post-request"></a>WebClient und Header für die POST-Anforderung
Erstellen Sie ein `WebClient`-Objekt, und legen Sie die Header fest. Für alle Anforderungen an die Bing-Suche-API ist ein `Ocp-Apim-Subscription-Key` erforderlich. Eine `POST`-Anforderung zum Hochladen eines Bildes muss außerdem `ContentType: multipart/form-data` angeben.

```
            WebClient client = new WebClient();
            client.Headers["Ocp-Apim-Subscription-Key"] = accessKey;
            client.Headers["ContentType"] = "multipart/form-data"; 
```

## <a name="upload-the-image-and-get-results"></a>Hochladen des Bilds und Abrufen der Ergebnisse

Die `WebClient`-Klasse enthält die `UpLoadFile`-Methode, die Daten für die `POST`-Anforderung formatiert. Sie formatiert den `RequestStream` und ruft `HttpWebRequest` ab, um zu viel Komplexität zu vermeiden.
Rufen Sie `WebClient.UpLoadFile` mit dem `/details`-Endpunkt und der hochzuladende Bilddatei ab. Die Antwort sind binäre Daten, die problemlos in JSON konvertiert werden können. 

Verwenden Sie den JSON-Text, um eine Instanz der `SearchResult`-Struktur zu initialisieren (siehe den [Anwendungsquelltext](tutorial-image-post-source.md) für den Kontext).
```        
         const string uriBase = "https://api.cognitive.microsoft.com/bing/v7.0/images/details";

        // The image to upload. Replace with your file and path.
        const string imageFile = "ansel-adams-tetons-snake-river.jpg";
            
        byte[] resp = client.UploadFile(uriBase + "?modules=All", imageFile);
        var json = System.Text.Encoding.Default.GetString(resp);

        // Create result object for return
        var searchResult = new SearchResult()
        {
            jsonResult = json,
            relevantHeaders = new Dictionary<String, String>()
        };
```

## <a name="print-the-results"></a>Ausgeben der Ergebnisse
Der Rest des Codes analysiert das JSON-Ergebnis und gibt es an die Konsole aus.

```
        /// <summary>
        /// Formats the given JSON string by adding line breaks and indents.
        /// </summary>
        /// <param name="json">The raw JSON string to format.</param>
        /// <returns>The formatted JSON string.</returns>
        static string JsonPrettyPrint(string json)
        {
            if (string.IsNullOrEmpty(json))
                return string.Empty;

            json = json.Replace(Environment.NewLine, "").Replace("\t", "");

            StringBuilder sb = new StringBuilder();
            bool quote = false;
            bool ignore = false;
            char last = ' ';
            int offset = 0;
            int indentLength = 2;

            foreach (char ch in json)
            {
                switch (ch)
                {
                    case '"':
                        if (!ignore) quote = !quote;
                        break;
                    case '\\':
                        if (quote && last != '\\') ignore = true;
                        break;
                }

                if (quote)
                {
                    sb.Append(ch);
                    if (last == '\\' && ignore) ignore = false;
                }
                else
                {
                    switch (ch)
                    {
                        case '{':
                        case '[':
                            sb.Append(ch);
                            sb.Append(Environment.NewLine);
                            sb.Append(new string(' ', ++offset * indentLength));
                            break;
                        case '}':
                        case ']':
                            sb.Append(Environment.NewLine);
                            sb.Append(new string(' ', --offset * indentLength));
                            sb.Append(ch);
                            break;
                        case ',':
                            sb.Append(ch);
                            sb.Append(Environment.NewLine);
                            sb.Append(new string(' ', offset * indentLength));
                            break;
                        case ':':
                            sb.Append(ch);
                            sb.Append(' ');
                            break;
                        default:
                            if (quote || ch != ' ') sb.Append(ch);
                            break;
                    }
                }
                last = ch;
            }

            return sb.ToString().Trim();
        }
```
## <a name="get-request-using-the-imageinsightstoken"></a>GET-Anforderung mithilfe von ImageInsightsToken
Um das mit den Ergebnissen von `POST` zurückgegebene `ImageInsightsToken` zu verwenden, erstellen Sie eine `GET`-Anforderung wie folgende:
```
https://api.cognitive.microsoft.com/bing/v7.0/images/details?InsightsToken="bcid_A2C4BB81AA2C9EF8E049C5933C546449*ccid_osS7gaos*mid_BF7CC4FC4A882A3C3D56E644685BFF7B8BACEAF2
```
Wenn es identifizierbare Personen oder Orte im Bild gibt, wird diese Anforderung Informationen über sie zurückgeben.
Die [Schnellstarts](https://docs.microsoft.com/azure/cognitive-services/bing-image-search) enthalten zahlreiche Codebeispiele.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Referenz zur Bing-Bildersuche-API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)

