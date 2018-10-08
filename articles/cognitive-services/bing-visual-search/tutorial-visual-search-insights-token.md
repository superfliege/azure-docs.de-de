---
title: 'Tutorial: ImageInsightsToken – Visuelle Bing-Suche'
titlesuffix: Azure Cognitive Services
description: Es wird beschrieben, wie Sie das SDK für die visuelle Bing-Suche nutzen, um URLs von Bildern abzurufen, die durch das ImageInsightsToken angegeben werden.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: tutorial
ms.date: 06/21/2018
ms.author: rosh
ms.openlocfilehash: bda4bdeea019d8cf3ae677d5eaf81e631ca38d16
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47222572"
---
# <a name="tutorial-bing-visual-search-sdk-imageinsightstoken-and-results"></a>Tutorial: SDK für die visuelle Bing-Suche: ImageInsightsToken und Ergebnisse
Das SDK für die visuelle Suche enthält eine Option, Bilder aus einer vorherigen Suche online zu finden, die ein `ImageInsightsToken` zurückgibt.  In diesem Beispiel wird ein `ImageInsightsToken` abgerufen und das Token in einer nachfolgenden Suche verwendet.  Der Code sendet das `ImageInsightsToken` an Bing und gibt Ergebnisse zurück, die URLs der Bing-Suche und URLs von ähnlichen online ermittelten Bildern enthalten.

## <a name="prerequisites"></a>Voraussetzungen
Visual Studio 2017. Laden Sie bei Bedarf die kostenlose Communityversion herunter: https://www.visualstudio.com/vs/community/.
Für die Authentifizierung von SDK-Aufrufen ist ein Cognitive Services-API-Schlüssel erforderlich. Registrieren Sie sich für einen kostenlosen Testschlüssel. Der Testschlüssel ist sieben Tage lang gültig und auf einen Aufruf pro Sekunde beschränkt. Für Produktionsszenarien erwerben Sie einen Zugriffsschlüssel. Weitere Details finden Sie in den Preisinformationen.
Möglichkeit zum Ausführen des .NET Core SDK sowie von .NET Core 1.1-Apps. CORE, Framework und Runtime finden Sie hier: https://www.microsoft.com/net/download/.

##<a name="application-dependencies"></a>Anwendungsabhängigkeiten
Navigieren Sie in Visual Studio über den Projektmappen-Explorer zur Option „NuGet-Pakete verwalten“, um mit dem SDK für die Bing-Websuche eine Konsolenanwendung einzurichten. Hinzufügen:
* Microsoft.Azure.CognitiveServices.Search.VisualSearch
* Microsoft.Azure.CognitiveServices.Search.ImageSearchpackage packages.

Beim Installieren des Pakets mit dem SDK für die NuGet-Websuche werden auch Abhängigkeiten installiert, z.B.:

* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json

## <a name="get-the-imageinsightstoken-from-image-search"></a>Abrufen des ImageInsightsToken aus Bildersuche
In diesem Beispiel wird ein `ImageInsightsToken` verwendet, das mit der folgenden Methode gewonnen wurde.  Weitere Informationen zu diesem Aufruf finden Sie unter [Schnellstartanleitung für das SDK für die Bildersuche: C#](https://docs.microsoft.com/en-us/azure/cognitive-services/bing-image-search/image-search-sdk-quickstart).

Der Code sucht nach den Ergebnissen einer Abfrage für „Canadian Rockies“ und ruft ein ImageInsightsToken ab. Er druckt Insightstoken, Miniaturansicht-URL und Bildinhalt-URL für das erste Bild aus.  Die Methode gibt das `ImageInsightsToken` zur Verwendung in einer darauffolgenden Anforderung der thematischen Suche zurück.

```
        private static String ImageResults(String subKey)
        {
            String insightTok = "None";
            try
            {
                var client = new ImageSearchAPI(new Microsoft.Azure.CognitiveServices.Search.ImageSearch.ApiKeyServiceClientCredentials(subKey)); //
                var imageResults = client.Images.SearchAsync(query: "canadian rockies").Result;
                Console.WriteLine("Search images for query \"canadian rockies\"");

                if (imageResults == null)
                {
                    Console.WriteLine("No image result data.");
                }
                else
                {
                    // Image results
                    if (imageResults.Value.Count > 0)
                    {
                        var firstImageResult = imageResults.Value.First();

                        Console.WriteLine($"\r\nImage result count: {imageResults.Value.Count}");
                        insightTok = firstImageResult.ImageInsightsToken;
                        Console.WriteLine($"First image insights token: {firstImageResult.ImageInsightsToken}");  
                        Console.WriteLine($"First image thumbnail url: {firstImageResult.ThumbnailUrl}");
                        Console.WriteLine($"First image content url: {firstImageResult.ContentUrl}");
                    }
                    else
                    {
                        insightTok = "None found";
                        Console.WriteLine("Couldn't find image results!");
                    }
                }
            }

            catch (Exception ex)
            {
                Console.WriteLine("\r\nEncountered exception. " + ex.Message);
            }

            return insightTok;
        }
```

## <a name="specify-the-imageinsightstoken-for-visual-search-request"></a>Angeben des ImageInsightsTokens für eine Anforderung der thematischen Suche
In diesem Beispiel wird das von der vorherigen Methode zurückgegebene Insightstoken verwendet. Mit dem folgenden Code wird ein `ImageInfo`-Objekt aus dem `ImageInsightsToken` erstellt und das ImageInfo-Objekt in eine `VisualSearchRequest` geladen. Angeben des `ImageInsightsToken` in einer `ImageInfo` für die `VisualSearchRequest`

```
ImageInfo ImageInfo = new ImageInfo(imageInsightsToken: insightsTok);
```
## <a name="use-visual-search-to-find-images-from-an-imageinsightstoken"></a>Verwenden der thematischen Suche, um Bilder aus einem ImageInsightsToken zu finden
Die `VisualSearchRequest` enthält im `ImageInfo`-Objekt Informationen zu dem zu suchenden Bild.  Mit der `VisualSearchMethodAsync`-Methode werden die Ergebnisse abgerufen.
```
// An image binary is not necessary here, as the image is specified by insights token.
VisualSearchRequest VisualSearchRequest = new VisualSearchRequest(ImageInfo);

var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: VisualSearchRequest).Result;
Console.WriteLine("\r\nVisual search request with knowledgeRequest");

```

## <a name="get-the-url-data-from-imagemoduleaction"></a>Abrufen der URL-Daten aus ImageModuleAction
Die Ergebnisse der thematischen Suche sind `ImageTag`-Objekte.  Jedes Tag enthält eine Liste mit `ImageAction`-Objekten.  Jedes `ImageAction`-Objekt enthält ein `Data`-Feld. Hierbei handelt es sich um eine Liste mit Werten, die vom Typ der Aktion abhängen:

Sie können die unterschiedlichen Typen mit dem folgenden Code abrufen:
```
Console.WriteLine("\r\n" + "ActionType: " + i.ActionType + " -> WebSearchUrl: " + i.WebSearchUrl);

```
Die vollständige Anwendung gibt Folgendes zurück:

* ActionType: MoreSizes -> WebSearchUrl:
* ActionType: VisualSearch -> WebSearchUrl:
* ActionType: ImageById -> WebSearchUrl:
* ActionType: RelatedSearches -> WebSearchUrl:
* ActionType: DocumentLevelSuggestions -> WebSearchUrl:
* ActionType: TopicResults -> WebSearchUrl: https://www.bing.com/cr?IG=3E32CC6CA5934FBBA14ABC3B2E4651F9&CID=1BA795A21EAF6A63175699B71FC36B7C&rd=1&h=BcQifmzdKFyyBusjLxxgO42kzq1Geh7RucVVqvH-900&v=1&r=https%3a%2f%2fwww.bing.com%2fdiscover%2fcanadian%2brocky&p=DevEx,5823.1
* ActionType: ImageResults -> WebSearchUrl: https://www.bing.com/cr?IG=3E32CC6CA5934FBBA14ABC3B2E4651F9&CID=1BA795A21EAF6A63175699B71FC36B7C&rd=1&h=PV9GzMFOI0AHZp2gKeWJ8DcveSDRE3fP2jHDKMpJSU8&v=1&r=https%3a%2f%2fwww.bing.com%2fimages%2fsearch%3fq%3doutdoor&p=DevEx,5831.1

Wie in der vorherigen Liste gezeigt, enthalten die Typen `TopicResults` und `ImageResults` Abfragen nach verwandten Bilder. Die URLs in der Liste sind Verknüpfungen zu Bing-Suchergebnissen.


## <a name="pagesincluding-actiontype-urls-of-images-found-by-visual-search"></a>PagesIncluding ActionType-URLs von Bildern, die mittels thematischer Suche gefunden wurden

Zum Abrufen der eigentlichen Bild-URLs ist eine Umwandlung erforderlich, bei der ein `ActionType`-Element als `ImageModuleAction` gelesen wird, das ein `Data`-Element mit einer Werteliste enthält.  Jeder Wert ist die URL eines Bilds.  Im Folgenden wird der Aktionstyp `PagesIncluding` in `ImageModuleAction` umgewandelt, und die Werte werden gelesen.
```
    if (i.ActionType == "PagesIncluding")
    {
        foreach(ImageObject o in (i as ImageModuleAction).Data.Value)
        {
            Console.WriteLine("ContentURL: " + o.ContentUrl);
        }
    }
```
Weitere Informationen zu diesen Datentypen finden Sie unter [Bilder – thematische Suche](https://docs.microsoft.com/en-us/rest/api/cognitiveservices/bingvisualsearch/images/visualsearch).
## <a name="complete-code"></a>Vollständiger Code

Mit dem folgenden Code werden die vorherigen Beispiele ausgeführt. Er sendet das `ImageInsightsToken` in einer POST-Anforderung. Dann druckt er die Bing-Such-URLs für jeden ActionType aus. Wenn der ActionType `PagesIncluding` ist, ruft der Code die `ImageObject`-Elemente in `Data` ab.  Die `Data` enthalten eine Liste mit Werten, bei denen es sich um die URLs von Bildern auf Webseiten handelt.  Fügen Sie die sich ergebenden URLs der thematischen Suche in den Browser ein, um die Ergebnisse anzuzeigen. Kopieren Sie die ContentUrl-Elemente, und fügen Sie sie in den Browser ein, um die Bilder anzuzeigen.

```
using System;
using System.IO;
using System.Linq;
using Microsoft.Azure.CognitiveServices.Search.VisualSearch;
using Microsoft.Azure.CognitiveServices.Search.VisualSearch.Models;
using Microsoft.Azure.CognitiveServices.Search.ImageSearch;

namespace VisualSearchFeatures
{
    class Program
    {
        static void Main(string[] args)
        {
            String subscriptionKey = "YOUR-ACCESS-KEY";

            insightsToken = ImageResults(subscriptionKey);

            VisualSearchInsightsToken(subscriptionKey, insightsToken);

            Console.WriteLine("Any key to quit...");
            Console.ReadKey();

        }

        // Searches for results on query (Canadian Rockies) and gets an ImageInsightsToken.
        // Also prints first image insights token, thumbnail url, and image content url.
        private static String ImageResults(String subKey)
        {
            String insightTok = "None";
            try
            {
                var client = new ImageSearchAPI(new Microsoft.Azure.CognitiveServices.Search.ImageSearch.ApiKeyServiceClientCredentials(subKey)); //
                var imageResults = client.Images.SearchAsync(query: "canadian rockies").Result;
                Console.WriteLine("Search images for query \"canadian rockies\"");

                if (imageResults == null)
                {
                    Console.WriteLine("No image result data.");
                }
                else
                {
                    // Image results
                    if (imageResults.Value.Count > 0)
                    {
                        var firstImageResult = imageResults.Value.First();

                        Console.WriteLine($"\r\nImage result count: {imageResults.Value.Count}");
                        insightTok = firstImageResult.ImageInsightsToken;
                        Console.WriteLine($"First image insights token: {firstImageResult.ImageInsightsToken}");  
                        Console.WriteLine($"First image thumbnail url: {firstImageResult.ThumbnailUrl}");
                        Console.WriteLine($"First image content url: {firstImageResult.ContentUrl}");
                    }
                    else
                    {
                        insightTok = "None found";
                        Console.WriteLine("Couldn't find image results!");
                    }
                }
            }

            catch (Exception ex)
            {
                Console.WriteLine("\r\nEncountered exception. " + ex.Message);
            }

            return insightTok;
        }


        // This method will get Visual Search results from an imageInsightsToken obtained from the return value of the ImageResults method.
        // The method includes imageInsightsToken the in a knowledgeRequest parameter, along with a cropArea object. 
        // It prints out the imageInsightsToken uploaded in the request.
        // Finally the example prints URLs of images found using the imageInsightsToken.

        public static void VisualSearchInsightsToken(string subscriptionKey, string insightsTok)
        {
            var client = new VisualSearchAPI(new Microsoft.Azure.CognitiveServices.Search.VisualSearch.ApiKeyServiceClientCredentials(subscriptionKey));

            try
            {
                // The image can be specified via an insights token, in the ImageInfo object.
                ImageInfo ImageInfo = new ImageInfo(imageInsightsToken: insightsTok);

                // An image binary is not necessary here, as the image is specified by insights token.
                VisualSearchRequest VisualSearchRequest = new VisualSearchRequest(ImageInfo);

                var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: VisualSearchRequest).Result;
                Console.WriteLine("\r\nVisual search request with imageInsightsToken and knowledgeRequest");

                if (visualSearchResults == null)
                {
                    Console.WriteLine("No visual search result data.");
                }
                else
                {
                    // Visual Search results
                    if (visualSearchResults.Image?.ImageInsightsToken != null)
                    {
                        Console.WriteLine($"Uploaded image insights token: {insightsTok}");
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find image insights token!");
                    }

                    if (visualSearchResults.Tags.Count > 0)
                    {
                        // List of tags
                        foreach (ImageTag t in visualSearchResults.Tags)
                        {
                            foreach (ImageAction i in t.Actions)
                            {
                                Console.WriteLine("\r\n" + "ActionType: " + i.ActionType + " WebSearchURL: " + i.WebSearchUrl);

                                if (i.ActionType == "PagesIncluding")
                                {
                                    foreach (ImageObject o in (i as ImageModuleAction).Data.Value)
                                    {
                                        Console.WriteLine("ContentURL: " + o.ContentUrl);
                                    }
                                }
                            }
                        }
                    }

                    else
                    {
                        Console.WriteLine("Couldn't find image tags!");
                    }

                }
            }

            catch (Exception ex)
            {
                Console.WriteLine("Encountered exception. " + ex.Message);
            }
        }
    }
}

```
## <a name="next-steps"></a>Nächste Schritte
[Antwort der thematischen Suche](https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/overview#the-response)
