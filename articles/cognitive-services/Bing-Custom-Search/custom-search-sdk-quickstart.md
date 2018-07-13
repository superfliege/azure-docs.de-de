---
title: 'Schnellstartanleitung für das SDK für die benutzerdefinierte Suche: C# | Microsoft-Dokumentation'
titleSuffix: Cognitive Services
description: Einrichten des SDK für die benutzerdefinierte Suche für die Konsolenanwendung in C#
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 01/31/2018
ms.author: rosh
ms.openlocfilehash: 59b208b53bec974433c50c0e2304dc96bd9bd09e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35372794"
---
# <a name="custom-search-sdk-c-quickstart"></a>Schnellstarts für das SDK für die benutzerdefinierte Suche: C#

Das SDK für die benutzerdefinierte Bing-Suche enthält die Funktionalität der REST-API für Entitätssuchen und das Analysieren von Ergebnissen.

## <a name="application-dependencies"></a>Anwendungsabhängigkeiten

Navigieren Sie in Visual Studio über den Projektmappen-Explorer zur Option `Manage NuGet Packages`, um mit dem SDK für die benutzerdefinierte Bing-Suche eine Konsolenanwendung einzurichten. Fügen Sie das Paket `Microsoft.Azure.CognitiveServices.Search.CustomSearch` hinzu.

Bei der Installation des [NuGet-Pakets für die benutzerdefinierte Suche](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.CustomSearch/1.2.0) werden gleichzeitig auch Abhängigkeiten einschließlich der folgenden Assemblys installiert:
* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json

## <a name="entity-search-client"></a>Entitätssuche-Client

Um eine Instanz des CustomSearchAPI-Clients zu erstellen, fügen Sie using-Direktiven hinzu:
```
using Microsoft.Azure.CognitiveServices.Search.CustomSearch;

```

Instanziieren Sie den Client für die benutzerdefinierte Suche: Ersetzen Sie `YOUR-CUSTOM-SEARCH-KEY` und `YOUR-CUSTOM-CONFIG-ID` durch Ihren Zugriffsschlüssel und die unter [Meine Instanzen](https://www.customsearch.ai/) zugewiesene ID der API-Endpunktkonfiguration.
```
var client = new CustomSearchAPI(new ApiKeyServiceClientCredentials("YOUR-CUSTOM-SEARCH-KEY"));

```
Verwenden Sie den Client, um mit einem Abfragetext eine Suche durchzuführen:
```
var webData = client.CustomInstance.SearchAsync(query: "Xbox", customConfig: Int32.Parse("YOUR-CUSTOM-CONFIG-ID")).Result;

```
## <a name="parse-the-results"></a>Analysieren der Ergebnisse

Die `SearchAsync`-Methode gibt ein `WebData`-Objekt mit `WebPages` zurück, wenn diese mit der Abfrage gefunden werden. Dieser Code sucht das erste Ergebnis und ruft `Name` und `URL` ab.
```
var webData = client.CustomInstance.SearchAsync(query: "Xbox", customConfig: Int32.Parse("YOUR-CUSTOM-CONFIG-ID")).Result;
 
Console.WriteLine("Searched for Query# \" Xbox \"");

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
## <a name="complete-console-application"></a>Vollständige Konsolenanwendung

Der folgende Code sucht mit der Abfrage „Xbox“ und druckt `Name` und `URL` für das erste Webergebnis aus.
```
using System;
using System.Linq;
using Microsoft.Azure.CognitiveServices.Search.CustomSearch;

namespace CustomSrchSDK
{
    class Program
    {
        static void Main(string[] args)
        {

            var client = new CustomSearchAPI(new ApiKeyServiceClientCredentials("YOUR-CUSTOM-SEARCH-KEY"));

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
            }
            catch (Exception ex)
            {
                Console.WriteLine("Encountered exception. " + ex.Message);
            }

            Console.WriteLine("Any key to exit...");
            Console.ReadKey();
        }

    }
}


```

## <a name="next-steps"></a>Nächste Schritte

[Cognitive Services SDK-Beispiele für .NET](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)
