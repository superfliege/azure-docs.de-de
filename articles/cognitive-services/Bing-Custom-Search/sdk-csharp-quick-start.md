---
title: 'Schnellstartanleitung für das SDK für die benutzerdefinierte Suche: C# | Microsoft-Dokumentation'
titleSuffix: Cognitive Services
description: Einrichten des SDK für die benutzerdefinierte Suche für die Konsolenanwendung in C#
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 09/06/2018
ms.author: scottwhi
ms.openlocfilehash: 6c9917e3a63515f36b386e444edcc53de07799fc
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46949926"
---
# <a name="c-sdk-quickstart"></a>C#-SDK-Schnellstart

Das SDK für die benutzerdefinierte Bing-Suche bietet ein einfacheres Programmiermodell als die REST-API für die benutzerdefinierte Bing-Suche. In diesem Abschnitt werden Sie durch das Erstellen Ihrer ersten Aufrufe für die benutzerdefinierte Suche mit dem C#-SDK geführt.

## <a name="prerequisites"></a>Voraussetzungen

Für die Durchführung dieses Schnellstarts benötigen Sie Folgendes:

- Eine einsatzbereite Instanz für die benutzerdefinierte Suche. Weitere Informationen finden Sie unter [Erstellen Ihrer ersten Instanz der benutzerdefinierten Bing-Suche](quick-start.md).  
  
- Einen Abonnementschlüssel. Sie erhalten einen Abonnementschlüssel, wenn Sie Ihre [kostenlose Testversion](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search) aktivieren, oder Sie können einen kostenpflichtigen Abonnementschlüssel über Ihr Azure-Dashboard beziehen (siehe [Erstellen eines Cognitive Services-APIs-Kontos im Azure-Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)).  
  
- Eine installierte Version von Visual Studio 2017. Sollten Sie nicht bereits über Visual Studio verfügen, können Sie die **kostenlose** [Visual Studio 2017 Community-Edition](https://www.visualstudio.com/downloads/) herunterladen.  
  
- Ein installiertes [NuGet-Paket für die benutzerdefinierte Suche](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.CustomSearch/1.2.0). Klicken Sie im Projektmappen-Explorer in Visual Studio mit der rechten Maustaste auf Ihr Projekt, und wählen Sie im Menü `Manage NuGet Packages` (NuGet-Pakete verwalten) aus. Installieren Sie das `Microsoft.Azure.CognitiveServices.Search.CustomSearch`-Paket.

Bei der Installation des NuGet-Pakets für die benutzerdefinierte Suche werden gleichzeitig die folgenden Assemblys installiert:

* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json



## <a name="run-the-code"></a>Ausführen des Codes

Führen Sie diese Schritte aus, um das Beispiel auszuführen:

1. Öffnen Sie Visual Studio 2017.
  
2. Klicken Sie auf das Menü **Datei**, klicken Sie auf **Neu**, auf **Projekt** und dann auf die Vorlage **Visual C# Console Application** (Visual-C#-Konsolenanwendung).
  
3. Benennen Sie Ihre Projektmappe „CustomSearchSolution“, und navigieren Sie zu dem Ordner, in dem sie abgelegt werden soll.
  
4. Klicken Sie auf „OK“, um die Projektmappe zu erstellen.  
  
4. Klicken Sie im Projektmappen-Explorer mit der Maustaste auf das Projekt (es hat den gleichen Namen wie die Projektmappe), und wählen Sie `Manage NuGet Packages` aus. Klicken Sie im NuGet-Paket-Manager auf **Durchsuchen**. Geben Sie in das Suchfeld „Microsoft.Azure.CognitiveServices.Search.CustomSearch“ ein, und drücken Sie die EINGABETASTE. Wählen Sie das Paket aus, und klicken Sie auf „Installieren“.  
  
4. Kopieren Sie den folgenden Code in die Datei „Program.cs“. Ersetzen Sie **YOUR-SUBSCRIPTION-KEY** und **YOUR-CUSTOM-CONFIG-ID** durch Ihren Abonnementschlüssel und Ihre Konfigurations-ID.  
  
    ```csharp
    using System;
    using System.Linq;
    using Microsoft.Azure.CognitiveServices.Search.CustomSearch;

    namespace CustomSrchSDK
    {
        class Program
        {
            static void Main(string[] args)
            {

                var client = new CustomSearchAPI(new ApiKeyServiceClientCredentials("YOUR-SUBSCRIPTION-KEY"));

                try
                {
                    // This will look up a single query (Xbox).
                    var webData = client.CustomInstance.SearchAsync(query: "Xbox", customConfig: Int32.Parse("YOUR-CUSTOM-CONFIG-ID")).Result;
                    Console.WriteLine("Searched for Query# \" Xbox \"");

                    //WebPages
                    if (webData?.WebPages?.Value?.Count > 0)
                    {
                        // find the first web page
                        var firstWebPagesResult = webData.WebPages.Value.FirstOrDefault();

                        if (firstWebPagesResult != null)
                        {
                            Console.WriteLine("Number of webpage results {0}", webData.WebPages.Value.Count);
                            Console.WriteLine("First web page name: {0} ", firstWebPagesResult.Name);
                            Console.WriteLine("First web page URL: {0} ", firstWebPagesResult.Url);
                        }
                        else
                        {
                            Console.WriteLine("Couldn't find web results!");
                        }
                    }
                    else
                    {
                        Console.WriteLine("Didn't see any Web data..");
                    }
                }
                catch (Exception ex)
                {
                    Console.WriteLine("Encountered exception. " + ex.Message);
                }

                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }

        }
    }
    ```  
  
5. Erstellen Sie die Projektmappe, und führen Sie sie aus (debuggen Sie sie). 




## <a name="breaking-it-down"></a>Aufschlüsselung

Nach der Installation des NuGet-Pakets für die benutzerdefinierte Suche müssen Sie eine using-Anweisung hinzufügen.

```csharp
using Microsoft.Azure.CognitiveServices.Search.CustomSearch;
```

Als Nächstes instanziieren Sie den Client für die benutzerdefinierte Suche, mit dem Sie Suchanfragen stellen. Achten Sie darauf, `YOUR-SUBSCRIPTION-KEY` durch Ihren Schlüssel zu ersetzen.

```csharp
var client = new CustomSearchAPI(new ApiKeyServiceClientCredentials("YOUR-CUSTOM-SEARCH-KEY"));
```

Jetzt können Sie mit dem Client eine Suchanfrage senden. Ersetzen Sie `YOUR-CUSTOM-CONFIG-ID` durch die Konfigurations-ID Ihrer Instanz (Sie finden die ID im [Portal für die benutzerdefinierte Suche](https://www.customsearch.ai/)). In diesem Beispiel wird nach „Xbox“ gesucht. Aktualisieren Sie es bei Bedarf.

```csharp
var webData = client.CustomInstance.SearchAsync(query: "Xbox", customConfig: Int32.Parse("YOUR-CUSTOM-CONFIG-ID")).Result;
```

Die Methode `SearchAsync` gibt ein `WebData`-Objekt zurück. Verwenden Sie `WebData`, um alle gefundenen Webseiten (`WebPages`) zu durchlaufen. Dieser Code findet das erste Ergebnis der Webseite und gibt `Name` und `URL` der Webseite aus.

```csharp
//WebPages
if (webData?.WebPages?.Value?.Count > 0)
{
    // find the first web page
    var firstWebPagesResult = webData.WebPages.Value.FirstOrDefault();

    if (firstWebPagesResult != null)
    {
        Console.WriteLine("Webpage Results#{0}", webData.WebPages.Value.Count);
        Console.WriteLine("First web page name: {0} ", firstWebPagesResult.Name);
        Console.WriteLine("First web page URL: {0} ", firstWebPagesResult.Url);
    }
    else
    {
        Console.WriteLine("Couldn't find web results!");
    }
}
else
{
    Console.WriteLine("Didn't see any Web data..");
}

```


## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die SDK-Beispiele an. Jedes Beispiel enthält eine ReadMe-Datei mit Details zu den Voraussetzungen sowie zur Installation und Ausführung der Beispiele.

* Erste Schritte mit [.NET-Beispielen](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7) 
    * [NuGet-Paket](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.CustomSearch/1.2.0)
    * In den [.NET-Bibliotheken](https://github.com/Azure/azure-sdk-for-net/tree/psSdkJson6/src/SDKs/CognitiveServices/dataPlane/Search/BingCustomSearch) finden Sie Definitionen und Informationen zu Abhängigkeiten.
* Erste Schritte mit [NodeJS-Beispielen](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples) 
    * In den [NodeJS-Bibliotheken](https://github.com/Azure/azure-sdk-for-node/tree/master/lib/services/customSearch) finden Sie Definitionen und Informationen zu Abhängigkeiten.
* Erste Schritte mit [Java-Beispielen](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples) 
    * In den [Java-Bibliotheken](https://github.com/Azure/azure-sdk-for-java/tree/master/cognitiveservices/azure-customsearch) finden Sie Definitionen und Informationen zu Abhängigkeiten.
* Erste Schritte mit [Python-Beispielen](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples) 
    * In den [Python-Bibliotheken](https://github.com/Azure/azure-sdk-for-python/tree/master/azure-cognitiveservices-search-customsearch) finden Sie Definitionen und Informationen zu Abhängigkeiten.

