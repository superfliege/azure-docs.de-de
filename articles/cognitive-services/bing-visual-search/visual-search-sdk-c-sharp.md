---
title: 'Schnellstart: SDK für die visuelle Suche – C# | Microsoft-Dokumentation'
description: Setup für die C#-Konsolenanwendung des SDK für die visuelle Suche.
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 05/16/2018
ms.author: v-gedod
ms.openlocfilehash: 65102862caf3ff2af0d4d7bfdb26d720d17c50ea
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37026300"
---
# <a name="visual-search-sdk-c-quickstart"></a>Schnellstart: SDK für die visuelle Suche – C#

Das Bing-SDK für die visuelle Suche nutzt die Funktionen der REST-API für Webanforderungen und Ergebnisanalysen.
[C#-Beispielquellcode für das Bing-SDK für die visuelle Suche](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingVisualSearch) ist bei GitHub verfügbar.

Codeszenarien sind unter den folgenden Themen dokumentiert:
* [Client für die visuelle Suche](#client)
* [Vollständige Konsolenanwendung](#complete)
* [post-Anforderung für Bildbinärdatei mit „cropArea“](#binary-crop)
* [KnowledgeRequest-Parameter](#knowledge-req)
* [Tags, Aktionen und Aktionstyp](#tags-actions)
* [Anzahl von Tags, Anzahl von Aktionen und erster Aktionstyp](#num-tags-actions)

## <a name="prerequisites"></a>Voraussetzungen
* Visual Studio 2017 Laden Sie bei Bedarf die kostenlose Communityversion herunter: https://www.visualstudio.com/vs/community/.
* Für die Authentifizierung von SDK-Aufrufen ist ein Cognitive Services-API-Schlüssel erforderlich. Registrieren Sie sich für einen [kostenlosen Testschlüssel](https://azure.microsoft.com/try/cognitive-services/?api=search-api-v7). Der Testschlüssel ist sieben Tage lang gültig und auf einen Aufruf pro Sekunde beschränkt. Für ein Produktionsszenario können Sie [hier](https://portal.azure.com/#create/Microsoft.CognitiveServicesBingSearch-v7) einen Zugriffsschlüssel kaufen. Preisinformationen finden Sie [hier](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/visual/).
* Möglichkeit zum Ausführen des .NET Core SDK sowie von .NET Core 1.1-Apps. CORE, Framework- und Runtime finden Sie hier: https://www.microsoft.com/net/download/.

## <a name="application-dependencies"></a>Anwendungsabhängigkeiten

Navigieren Sie in Visual Studio über den Projektmappen-Explorer zur Option `Manage NuGet Packages`, um mit dem SDK für die Bing-Websuche eine Konsolenanwendung einzurichten.  Fügen Sie das Paket `Microsoft.Azure.CognitiveServices.Search.VisualSearch` hinzu.

Beim Installieren des [Pakets mit dem SDK für die NuGet-Websuche](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.VisualSearch/1.0) werden auch Abhängigkeiten installiert. Hierzu zählen unter anderem folgende:
* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json

<a name="client"></a> 
## <a name="visual-search-client"></a>Client für die visuelle Suche
Fügen Sie using-Direktiven hinzu, um eine Instanz des `VisualSearchAPI`-Clients zu erstellen:
```
using Microsoft.Azure.CognitiveServices.Search.VisualSearch;
using Microsoft.Azure.CognitiveServices.Search.VisualSearch.Models;

```
Instanziieren Sie dann den Client:
```
var client = new WebSearchAPI(new ApiKeyServiceClientCredentials("YOUR-ACCESS-KEY"));


```
Verwenden Sie den Client, um nach Bildern zu suchen:
```
 System.IO.FileStream stream = new FileStream(Path.Combine("TestImages", "image.jpg"), FileMode.Open;
 // The knowledgeRequest parameter is not required if an image binary is passed in the request body
 var visualSearchResults = client.Images.VisualSearchMethodAsync(image: stream, knowledgeRequest: (string)null).Result;

```
Analysieren Sie die Ergebnisse der vorherigen Abfrage:
```
    // Visual Search results
    if (visualSearchResults.Image?.ImageInsightsToken != null)
    {
        Console.WriteLine($"Uploaded image insights token: {visualSearchResults.Image.ImageInsightsToken}");
    }
    else
    {
        Console.WriteLine("Couldn't find image insights token!");
    }

    // List of tags
    if (visualSearchResults.Tags.Count > 0)
    {
        var firstTagResult = visualSearchResults.Tags.First();
        Console.WriteLine($"Visual search tag count: {visualSearchResults.Tags.Count}");

        // List of actions in first tag
        if (firstTagResult.Actions.Count > 0)
        {
            var firstActionResult = firstTagResult.Actions.First();
            Console.WriteLine($"First tag action count: {firstTagResult.Actions.Count}");
            Console.WriteLine($"First tag action type: {firstActionResult.ActionType}");
        }
        else
        {
            Console.WriteLine("Couldn't find tag actions!");
        }

```

<a name="complete"></a> 
## <a name="complete-console-application"></a>Vollständige Konsolenanwendung

Die folgende Konsolenanwendung führt die zuvor definierte Abfrage aus und analysiert die Ergebnisse:
```
using Microsoft.Azure.CognitiveServices.Search.VisualSearch;
using Microsoft.Azure.CognitiveServices.Search.VisualSearch.Models;
using System;
using System.IO;
using System.Linq;

namespace VisualSrchSDK
{
    class Program
    {
        static void Main(string[] args)
        {
            String subKey = "YOUR-SUBSCRIPTION-KEY";

            VisualSearchImageBinary(subKey);
            //VisualSearchImageBinaryWithCropArea(subKey);
            //VisualSearchUrlWithFilters(subKey);
            //VisualSearchInsightsTokenWithCropArea(subKey);
            //VisualSearchUrlWithJson(subKey);

            Console.WriteLine("\r\nAny key to quit...");
            Console.ReadKey();
        }


        // This will send an image binary in the body of the post request and print out the imageInsightsToken, 
        //  the number of tags, the number of actions, and the first actionType.

        public static void VisualSearchImageBinary(string subscriptionKey)
        {
            var client = new VisualSearchAPI(new ApiKeyServiceClientCredentials(subscriptionKey));

            try
            {
                using (System.IO.FileStream stream = new FileStream(Path.Combine("TestImages", "image.jpg"), FileMode.Open))
                {
                    // The knowledgeRequest parameter is not required if an image binary is passed in the request body
                    var visualSearchResults = client.Images.VisualSearchMethodAsync(image: stream, knowledgeRequest: (string)null).Result;
                    Console.WriteLine("\r\nVisual search request with binary of image");

                    if (visualSearchResults == null)
                    {
                        Console.WriteLine("No visual search result data.");
                    }
                    else
                    {
                        // Visual Search results
                        if (visualSearchResults.Image?.ImageInsightsToken != null)
                        {
                            Console.WriteLine($"Uploaded image insights token: {visualSearchResults.Image.ImageInsightsToken}");
                        }
                        else
                        {
                            Console.WriteLine("Couldn't find image insights token!");
                        }

                        // List of tags
                        if (visualSearchResults.Tags.Count > 0)
                        {
                            var firstTagResult = visualSearchResults.Tags.First();
                            Console.WriteLine($"Visual search tag count: {visualSearchResults.Tags.Count}");

                            // List of actions in first tag
                            if (firstTagResult.Actions.Count > 0)
                            {
                                var firstActionResult = firstTagResult.Actions.First();
                                Console.WriteLine($"First tag action count: {firstTagResult.Actions.Count}");
                                Console.WriteLine($"First tag action type: {firstActionResult.ActionType}");
                            }
                            else
                            {
                                Console.WriteLine("Couldn't find tag actions!");
                            }

                        }
                        else
                        {
                            Console.WriteLine("Couldn't find image tags!");
                        }
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

Die Beispiele für die Bing-Suche veranschaulichen verschiedene Features des SDK.  Fügen Sie der zuvor definierten Klasse `VisualSrchSDK` die folgenden Funktionen hinzu.

<a name="binary-crop"></a>
## <a name="image-binary-post-with-croparea"></a>post-Anforderung für Bildbinärdatei mit „cropArea“

Der folgende Code sendet eine Bildbinärdatei im Hauptteil der post-Anforderung sowie ein cropArea-Objekt.  Anschließend werden „imageInsightsToken“, die Anzahl von Tags, die Anzahl von Aktionen und der erste Aktionstyp ausgegeben.

```
        public static void VisualSearchImageBinaryWithCropArea(string subscriptionKey)
        {
            var client = new VisualSearchAPI(new ApiKeyServiceClientCredentials(subscriptionKey));

            try
            {
                using (FileStream stream = new FileStream(Path.Combine("TestImages", "image.jpg"), FileMode.Open))
                {
                    // The ImageInfo struct contains a crop area specifying a region to crop in the uploaded image
                    CropArea CropArea = new CropArea(top: (float)0.1, bottom: (float)0.5, left: (float)0.1, right: (float)0.9);
                    ImageInfo ImageInfo = new ImageInfo(cropArea: CropArea);
                    VisualSearchRequest VisualSearchRequest = new VisualSearchRequest(imageInfo: ImageInfo);

                    // The knowledgeRequest here holds additional information about the image, which is passed in in binary form
                    var visualSearchResults = client.Images.VisualSearchMethodAsync(image: stream, knowledgeRequest: VisualSearchRequest).Result;
                    Console.WriteLine("\r\nVisual search request with binary of image with knowledgeRequest of CropArea");

                    if (visualSearchResults == null)
                    {
                        Console.WriteLine("No visual search result data.");
                    }
                    else
                    {
                        // Visual Search results
                        if (visualSearchResults.Image?.ImageInsightsToken != null)
                        {
                            Console.WriteLine($"Uploaded image insights token: {visualSearchResults.Image.ImageInsightsToken}");
                        }
                        else
                        {
                            Console.WriteLine("Couldn't find image insights token!");
                        }

                        // List of tags
                        if (visualSearchResults.Tags.Count > 0)
                        {
                            var firstTagResult = visualSearchResults.Tags.First();
                            Console.WriteLine($"Visual search tag count: {visualSearchResults.Tags.Count}");

                            // List of actions in first tag
                            if (firstTagResult.Actions.Count > 0)
                            {
                                var firstActionResult = firstTagResult.Actions.First();
                                Console.WriteLine($"First tag action count: {firstTagResult.Actions.Count}");
                                Console.WriteLine($"First tag action type: {firstActionResult.ActionType}");
                            }
                            else
                            {
                                Console.WriteLine("Couldn't find tag actions!");
                            }

                        }
                        else
                        {
                            Console.WriteLine("Couldn't find image tags!");
                        }
                    }
                }
            }

            catch (Exception ex)
            {
                Console.WriteLine("Encountered exception. " + ex.Message);
            }
        }

```
<a name="knowledge-req"></a>
## <a name="knowledgerequest-parameter"></a>KnowledgeRequest-Parameter

Der folgende Code sendet eine Bild-URL im `knowledgeRequest`-Parameter sowie einen Filter vom Typ \"site:www.bing.com\".  Anschließend werden `imageInsightsToken`, die Anzahl von Tags, die Anzahl von Aktionen und der erste Aktionstyp ausgegeben.
```
        public static void VisualSearchUrlWithFilters(string subscriptionKey)
        {
            var client = new VisualSearchAPI(new ApiKeyServiceClientCredentials(subscriptionKey));

            try
            {
                // The image can be specified via URL, in the ImageInfo object
                var ImageUrl = "https://images.unsplash.com/photo-1512546148165-e50d714a565a?w=600&q=80";
                ImageInfo ImageInfo = new ImageInfo(url: ImageUrl);

                // Optional filters inside the knowledgeRequest will restrict similar products and images to certain domains
                Filters Filters = new Filters(site: "www.bing.com");
                KnowledgeRequest KnowledgeRequest = new KnowledgeRequest(filters: Filters);

                // An image binary is not necessary here, as the image is specified via URL
                VisualSearchRequest VisualSearchRequest = new VisualSearchRequest(imageInfo: ImageInfo, knowledgeRequest: KnowledgeRequest);
                var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: VisualSearchRequest).Result;
                Console.WriteLine("\r\nVisual search request with url of image and knowledgeRequest based on filters");

                if (visualSearchResults == null)
                {
                    Console.WriteLine("No visual search result data.");
                }
                else
                {
                    // Visual Search results
                    if (visualSearchResults.Image?.ImageInsightsToken != null)
                    {
                        Console.WriteLine($"Uploaded image insights token: {visualSearchResults.Image.ImageInsightsToken}");
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find image insights token!");
                    }

                    // List of tags
                    if (visualSearchResults.Tags.Count > 0)
                    {
                        var firstTagResult = visualSearchResults.Tags.First();
                        Console.WriteLine($"Visual search tag count: {visualSearchResults.Tags.Count}");

                        // List of actions in first tag
                        if (firstTagResult.Actions.Count > 0)
                        {
                            var firstActionResult = firstTagResult.Actions.First();
                            Console.WriteLine($"First tag action count: {firstTagResult.Actions.Count}");
                            Console.WriteLine($"First tag action type: {firstActionResult.ActionType}");
                        }
                        else
                        {
                            Console.WriteLine("Couldn't find tag actions!");
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

```
<a name="tags-actions"></a>
## <a name="tags-actions-and-actiontype"></a>Tags, Aktionen und Aktionstyp

Der folgende Code sendet ein Bildauswertungstoken im knowledgeRequest-Parameter sowie ein cropArea-Objekt.  Anschließend werden „imageInsightsToken“, die Anzahl von Tags, die Anzahl von Aktionen und der erste Aktionstyp ausgegeben.

```
        public static void VisualSearchInsightsTokenWithCropArea(string subscriptionKey)
        {
            var client = new VisualSearchAPI(new ApiKeyServiceClientCredentials(subscriptionKey));

            try
            {
                // The image can be specified via an insights token, in the ImageInfo object
                var ImageInsightsToken = "bcid_113F29C079F18F385732D8046EC80145*ccid_oV/QcH95*mid_687689FAFA449B35BC11A1AE6CEAB6F9A9B53708*thid_R.113F29C079F18F385732D8046EC80145";

                // An optional crop area can be passed in to define a region of interest in the image
                CropArea CropArea = new CropArea(top: (float)0.1, bottom: (float)0.5, left: (float)0.1, right: (float)0.9);
                ImageInfo ImageInfo = new ImageInfo(imageInsightsToken: ImageInsightsToken, cropArea: CropArea);

                // An image binary is not necessary here, as the image is specified via insights token
                VisualSearchRequest VisualSearchRequest = new VisualSearchRequest(imageInfo: ImageInfo);
                var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: VisualSearchRequest).Result;
                Console.WriteLine("\r\nVisual search request with ImageInsightsToken and knowledgeRequest based on imageInfo of cropArea");

                if (visualSearchResults == null)
                {
                    Console.WriteLine("No visual search result data.");
                }
                else
                {
                    // Visual Search results
                    if (visualSearchResults.Image?.ImageInsightsToken != null)
                    {
                        Console.WriteLine($"Uploaded image insights token: {visualSearchResults.Image.ImageInsightsToken}");
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find image insights token!");
                    }

                    // List of tags
                    if (visualSearchResults.Tags.Count > 0)
                    {
                        var firstTagResult = visualSearchResults.Tags.First();
                        Console.WriteLine($"Visual search tag count: {visualSearchResults.Tags.Count}");

                        // List of actions in first tag
                        if (firstTagResult.Actions.Count > 0)
                        {
                            var firstActionResult = firstTagResult.Actions.First();
                            Console.WriteLine($"First tag action count: {firstTagResult.Actions.Count}");
                            Console.WriteLine($"First tag action type: {firstActionResult.ActionType}");
                        }
                        else
                        {
                            Console.WriteLine("Couldn't find tag actions!");
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
```
<a name="num-tags-actions"></a>
## <a name="number-of-tags-number-of-actions-and-first-actiontype"></a>Anzahl von Tags, Anzahl von Aktionen und erster Aktionstyp
Der folgende Code sendet eine Bild-URL im knowledgeRequest-Parameter sowie einen Zuschneidebereich.  Anschließend werden „imageInsightsToken“, die Anzahl von Tags, die Anzahl von Aktionen und der erste Aktionstyp ausgegeben.
```
        public static void VisualSearchUrlWithJson(string subscriptionKey)
        {
            var client = new VisualSearchAPI(new ApiKeyServiceClientCredentials(subscriptionKey));

            try
            {

                //The visual search request can be passed in as a JSON string
                //The image is specified via URL in the ImageInfo object, along with a crop area as shown below:
                
                //     "imageInfo": {
                //         "url": "https://images.unsplash.com/photo-1512546148165-e50d714a565a?w=600&q=80",
                //     "cropArea": {
                //           "top": 0.1,
                //           "bottom": 0.5,
                //           "left": 0.1,
                //           "right": 0.9
                //         }
                //     },
                //     "knowledgeRequest": {
                //        "filters": {
                //            "site": "www.bing.com"
                //        }              
                //     }

                var VisualSearchRequestJSON = "{\"imageInfo\":{\"url\":\"https://images.unsplash.com/photo-1512546148165-e50d714a565a?w=600&q=80\",\"cropArea\":{\"top\":0.1,\"bottom\":0.5,\"left\":0.1,\"right\":0.9}},\"knowledgeRequest\":{\"filters\":{\"site\":\"www.bing.com\"}}}";

                // An image binary is not necessary here, as the image is specified by URL in JSON text
                var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: VisualSearchRequestJSON).Result;
                Console.WriteLine("\r\nVisual search request with image url specified by JSON text");

                if (visualSearchResults == null)
                {
                    Console.WriteLine("No visual search result data.");
                }
                else
                {
                    // Visual Search results
                    if (visualSearchResults.Image?.ImageInsightsToken != null)
                    {
                        Console.WriteLine($"Uploaded image insights token: {visualSearchResults.Image.ImageInsightsToken}");
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find image insights token!");
                    }

                    // List of tags
                    if (visualSearchResults.Tags.Count > 0)
                    {
                        var firstTagResult = visualSearchResults.Tags.First();
                        Console.WriteLine($"Visual search tag count: {visualSearchResults.Tags.Count}");

                        // List of actions in first tag
                        if (firstTagResult.Actions.Count > 0)
                        {
                            var firstActionResult = firstTagResult.Actions.First();
                            Console.WriteLine($"First tag action count: {firstTagResult.Actions.Count}");
                            Console.WriteLine($"First tag action type: {firstActionResult.ActionType}");
                        }
                        else
                        {
                            Console.WriteLine("Couldn't find tag actions!");
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
```

## <a name="next-steps"></a>Nächste Schritte

[Cognitive Services SDK-Beispiele für .NET](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)