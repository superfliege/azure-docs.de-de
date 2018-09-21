---
title: 'Tutorial: SDK für die visuelle Bing-Suche – Ergebnisse für den Zuschneidebereich | Microsoft-Dokumentation'
description: Es wird beschrieben, wie Sie das SDK für die visuelle Bing-Suche nutzen, um URLs von Bildern abzurufen, die Ähnlichkeit mit dem Zuschneidebereich des hochgeladenen Bilds aufweisen.
services: cognitive-services
author: mikedodaro
manager: ronakshah
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: article
ms.date: 06/20/2018
ms.author: rosh
ms.openlocfilehash: dd51ed7c710cc51a9fe0e63e55aa0d2c4ea24bee
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/14/2018
ms.locfileid: "45574488"
---
# <a name="tutorial-bing-visual-search-sdk-image-crop-area-and-results"></a>Tutorial: SDK für die visuelle Bing-Suche – Bildzuschneidebereich und Ergebnisse
Das SDK für die visuelle Suche enthält eine Option zum Auswählen eines Bildbereichs und für die Onlinesuche von Bildern, die Ähnlichkeit mit dem Zuschneidebereich des größeren Bilds aufweisen.  In diesem Beispiel ist der Zuschneidebereich mit einer Person aus einem Bild angegeben, auf dem mehrere Personen zu sehen sind.  Der Code sendet den Zuschneidebereich und die URL des größeren Bilds und gibt Ergebnisse zurück, die URLs der Bing-Suche und URLs von ähnlichen online ermittelten Bildern enthalten.

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen [Visual Studio 2017](https://www.visualstudio.com/downloads/), um diesen Code unter Windows ausführen zu können. (Die kostenlose Community Edition ist hierfür geeignet.)

Sie müssen über ein [Cognitive Services-API-Konto](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) mit Bing-Suche-APIs verfügen. Die [kostenlose Testversion](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) ist für diesen Schnellstart ausreichend. Sie benötigen den Zugriffsschlüssel, den Sie beim Aktivieren Ihrer kostenlosen Testversion erhalten, oder Sie können den Schlüssel eines kostenpflichtigen Abonnements von Ihrem Azure-Dashboard verwenden.

## <a name="application-dependencies"></a>Anwendungsabhängigkeiten
Navigieren Sie in Visual Studio über den Projektmappen-Explorer zur Option „NuGet-Pakete verwalten“, um mit dem SDK für die Bing-Websuche eine Konsolenanwendung einzurichten. Fügen Sie das Paket „Microsoft.Azure.CognitiveServices.Search.VisualSearch“ hinzu.

Beim Installieren des Pakets mit dem SDK für die NuGet-Websuche werden auch Abhängigkeiten installiert, z.B.:

* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json

## <a name="image-and-crop-area"></a>Bild und Zuschneidebereich
Das folgende Bild zeigt das Führungsteam von Microsoft.  Mit dem SDK für die visuelle Suche laden wir einen Zuschneidebereich des Bilds hoch und suchen nach anderen Bildern und Webseiten, die die Entität im ausgewählten Bereich des größeren Bilds enthalten.  In diesem Fall ist die Entität eine Person.

![Führungsteam von Microsoft](./media/MS_SrLeaders.jpg)

## <a name="specify-the-crop-area-as-imageinfo-in-visualsearchrequest"></a>Angeben des Zuschneidebereichs als ImageInfo in VisualSearchRequest
In diesem Beispiel wird ein Zuschneidebereich des vorherigen Bilds genutzt, bei dem die Koordinaten oben links und unten rechts basierend auf dem Prozentsatz des gesamten Bilds angegeben werden.  Mit dem folgenden Code wird ein `ImageInfo`-Objekt aus dem Zuschneidebereich erstellt und das `ImageInfo`-Objekt in ein `VisualSearchRequest`-Element geladen.  Das `ImageInfo`-Objekt enthält auch die URL des Onlinebilds.

```
CropArea CropArea = new CropArea(top: (float)0.01, bottom: (float)0.30, left: (float)0.01, right: (float)0.20);
string imageURL = "https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/media/ms_srleaders.jpg;
ImageInfo imageInfo = new ImageInfo(cropArea: CropArea, url: imageURL);

VisualSearchRequest visualSearchRequest = new VisualSearchRequest(imageInfo: imageInfo);
```
## <a name="search-for-images-similar-to-crop-area"></a>Suchen nach Bildern mit Ähnlichkeit zum Zuschneidebereich
Das `VisualSearchRequest`-Element enthält Zuschneidebereichsinformationen zum Bild und zur zugehörigen URL.  Mit der `VisualSearchMethodAsync`-Methode werden die Ergebnisse abgerufen.
```
Console.WriteLine("\r\nSending visual search request with knowledgeRequest that contains URL and crop area");
var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: visualSearchRequest).Result; 

```

## <a name="get-the-url-data-from-imagemoduleaction"></a>Abrufen der URL-Daten aus ImageModuleAction
Die Ergebnisse der thematischen Suche sind `ImageTag`-Objekte.  Jedes Tag enthält eine Liste mit `ImageAction`-Objekten.  Jedes `ImageAction`-Objekt enthält ein `Data`-Feld. Hierbei handelt es sich um eine Liste mit Werten, die vom Typ der Aktion abhängen:

Sie können die unterschiedlichen Typen mit dem folgenden Code abrufen:
```
Console.WriteLine("\r\n" + "ActionType: " + i.ActionType + " -> WebSearchUrl: " + i.WebSearchUrl);

```
Die vollständige Anwendung gibt Folgendes zurück:

* ActionType: PagesIncluding WebSearchURL:
* ActionType: MoreSizes WebSearchURL:
* ActionType: VisualSearch WebSearchURL:
* ActionType: ImageById WebSearchURL: 
* ActionType: RelatedSearches WebSearchURL:
* ActionType: Entity -> WebSearchUrl: https://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=BvvDoRtmZ35Xc_UZE4lZx6_eg7FHgcCkigU1D98NHQo&v=1&r=https%3a%2f%2fwww.bing.com%2fsearch%3fq%3dSatya%2bNadella&p=DevEx,5380.1
* ActionType: TopicResults -> WebSearchUrl: https://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=3QGtxPb3W9LemuHRxAlW4CW7XN4sPkUYCUynxAqI9zQ&v=1&r=https%3a%2f%2fwww.bing.com%2fdiscover%2fnadella%2bsatya&p=DevEx,5382.1
* ActionType: ImageResults -> WebSearchUrl: https://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=l-WNHO89Kkw69AmIGe2MhlUp6MxR6YsJszgOuM5sVLs&v=1&r=https%3a%2f%2fwww.bing.com%2fimages%2fsearch%3fq%3dSatya%2bNadella&p=DevEx,5384.1

Wie in der obigen Liste gezeigt, enthält das `ActionType`-Element von `Entity` eine Abfrage der Bing-Suche, mit der Informationen zu einer erkennbaren Person, einem Ort oder einem Gegenstand zurückgegeben werden.  Die Typen `TopicResults` und `ImageResults` enthalten Abfragen für verwandte Bilder. Die URLs in der Liste sind Verknüpfungen zu Bing-Suchergebnissen.


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

## <a name="complete-code"></a>Vollständiger Code

Mit dem folgenden Code werden die vorherigen Beispiele ausgeführt. Im Text der POST-Anforderung wird eine Bildbinärdatei gesendet (zusammen mit einem cropArea-Objekt), und die URLs der Bing-Suche für jeden ActionType werden ausgegeben. Wenn ActionType auf PagesIncluding festgelegt ist, ruft der Code die ImageObject-Elemente in den ImageObject-Daten ab.  Die Daten enthalten eine Liste mit Werten, bei denen es sich um die URLs von Bildern auf Webseiten handelt.  Fügen Sie die sich ergebenden URLs der visuellen Suche in den Browser ein, um die Ergebnisse anzuzeigen. Kopieren Sie die ContentUrl-Elemente, und fügen Sie sie in den Browser ein, um die Bilder anzuzeigen.

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

            VisualSearchImageBinaryWithCropArea(subscriptionKey);

            Console.WriteLine("Any key to quit...");
            Console.ReadKey();

        }

        public static void VisualSearchImageBinaryWithCropArea(string subscriptionKey)
        {
            var client = new VisualSearchAPI(new Microsoft.Azure.CognitiveServices.Search.VisualSearch.ApiKeyServiceClientCredentials(subscriptionKey));

            try
            {
                CropArea CropArea = new CropArea(top: (float)0.01, bottom: (float)0.30, left: (float)0.01, right: (float)0.20);
                
                // The ImageInfo struct specifies the crop area in the image and the URL of the larger image. 
                string imageURL = "https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/media/ms_srleaders.jpg";
                ImageInfo imageInfo = new ImageInfo(cropArea: CropArea, url: imageURL);
                
                VisualSearchRequest visualSearchRequest = new VisualSearchRequest(imageInfo: imageInfo);

                var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: visualSearchRequest).Result; 

                Console.WriteLine("\r\nVisual search request with image from URL and crop area combined in knowledgeRequest");

                if (visualSearchResults == null)
                {
                    Console.WriteLine("No visual search result data.");
                }
                else
                {
                    // List of tags
                    if (visualSearchResults.Tags.Count > 0)
                    {

                        foreach(ImageTag t in visualSearchResults.Tags)
                        {
                            foreach (ImageAction i in t.Actions)
                            { 
                                Console.WriteLine("\r\n" + "ActionType: " + i.ActionType + " -> WebSearchUrl: " + i.WebSearchUrl);

                                if (i.ActionType == "PagesIncluding")
                                {
                                    foreach(ImageObject o in (i as ImageModuleAction).Data.Value)
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