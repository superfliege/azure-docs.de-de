---
title: 'Schnellstart: SDK für Bing-Rechtschreibprüfung, C#'
titlesuffix: Azure Cognitive Services
description: Einrichten einer Konsolenanwendung mit dem SDK für Rechtschreibprüfung
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-spell-check
ms.topic: quickstart
ms.date: 01/30/2018
ms.author: v-gedod
ms.openlocfilehash: 406936200a39e21d7377e7b2dba19a7ee745de57
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2018
ms.locfileid: "51568834"
---
# <a name="quickstart-bing-spell-check-sdk-with-c"></a>Schnellstart: SDK für Bing-Rechtschreibprüfung mit C#

Das SDK für die Bing-Rechtschreibprüfung enthält die Funktionen der REST-API für die Rechtschreibprüfung.

## <a name="application-dependencies"></a>Anwendungsabhängigkeiten

Navigieren Sie in Visual Studio über den Projektmappen-Explorer zur Option `Manage NuGet Packages`, um mit dem SDK für die Bing-Rechtschreibprüfung eine Konsolenanwendung einzurichten. Fügen Sie das Paket `Microsoft.Azure.CognitiveServices.SpellCheck` hinzu.

Beim Installieren des [SpellCheck-SDK-Pakets](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.SpellCheck/1.2.0) werden auch Abhängigkeiten installiert. Hierzu zählen unter anderem Folgende:

* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.AZure
* Newtonsoft.Json

## <a name="spell-check-client"></a>Rechtschreibprüfungsclient

Um eine Instanz des `SpellCheckAPI`-Clients zu erstellen, fügen Sie using-Direktiven hinzu:

```cs
using Microsoft.Azure.CognitiveServices.SpellCheck;
```

Instanziieren Sie dann den Client:

```cs
var client = new SpellCheckAPI(new ApiKeyServiceClientCredentials("YOUR-ACCESS-KEY"));
```

Überprüfen Sie die Rechtschreibung mithilfe des Clients:

```cs
var result = client.SpellCheckerWithHttpMessagesAsync(text: "Bill Gatas", mode: "proof", acceptLanguage: "en-US").Result;
Console.WriteLine("Correction for Query# \"bill gatas\"");
```

Analysieren Sie die Ergebnisse:

```cs
// SpellCheck Results
if (result?.Body.FlaggedTokens?.Count > 0)
{
    // find the first spellcheck result
    var firstspellCheckResult = result.Body.FlaggedTokens.FirstOrDefault();

    if (firstspellCheckResult != null)
    {
        Console.WriteLine("SpellCheck Results#{0}", result.Body.FlaggedTokens.Count);
        Console.WriteLine("First SpellCheck Result token: {0} ", firstspellCheckResult.Token);
        Console.WriteLine("First SpellCheck Result Type: {0} ", firstspellCheckResult.Type);
        Console.WriteLine("First SpellCheck Result Suggestion Count: {0} ", firstspellCheckResult.Suggestions.Count);

        var suggestions = firstspellCheckResult.Suggestions;
        if (suggestions?.Count > 0)
        {
            var firstSuggestion = suggestions.FirstOrDefault();
            Console.WriteLine("First SpellCheck Suggestion Score: {0} ", firstSuggestion.Score);
            Console.WriteLine("First SpellCheck Suggestion : {0} ", firstSuggestion.Suggestion);
        }
        }
        else
        {
            Console.WriteLine("Couldn't get any Spell check results!");
        }
    }
    else
    {
        Console.WriteLine("Didn't see any SpellCheck results..");
    }
```

## <a name="complete-console-application"></a>Vollständige Konsolenanwendung

Die folgende Konsolenanwendung führt den vorherigen Code aus:

```cs
using System;
using System.Linq;
using Microsoft.Azure.CognitiveServices.SpellCheck;

namespace SpellCheckSDK
{
    class Program
    {
        static void Main(string[] args)
        {
            var client = new SpellCheckAPI(new ApiKeyServiceClientCredentials("YOUR-ACCESS-KEY"));

            try
            {
                var result = client.SpellCheckerWithHttpMessagesAsync(text: "Bill Gatas", mode: "proof", acceptLanguage: "en-US").Result;
                Console.WriteLine("Correction for Query# \"bill gatas\"");

                // SpellCheck Results
                if (result?.Body.FlaggedTokens?.Count > 0)
                {
                    // find the first spellcheck result
                    var firstspellCheckResult = result.Body.FlaggedTokens.FirstOrDefault();

                    if (firstspellCheckResult != null)
                    {
                        Console.WriteLine("SpellCheck Results#{0}", result.Body.FlaggedTokens.Count);
                        Console.WriteLine("First SpellCheck Result token: {0} ", firstspellCheckResult.Token);
                        Console.WriteLine("First SpellCheck Result Type: {0} ", firstspellCheckResult.Type);
                        Console.WriteLine("First SpellCheck Result Suggestion Count: {0} ", firstspellCheckResult.Suggestions.Count);

                        var suggestions = firstspellCheckResult.Suggestions;
                        if (suggestions?.Count > 0)
                        {
                            var firstSuggestion = suggestions.FirstOrDefault();
                            Console.WriteLine("First SpellCheck Suggestion Score: {0} ", firstSuggestion.Score);
                            Console.WriteLine("First SpellCheck Suggestion : {0} ", firstSuggestion.Suggestion);
                        }
                    }
                    else
                    {
                        Console.WriteLine("Couldn't get any Spell check results!");
                    }
                }
                else
                {
                    Console.WriteLine("Didn't see any SpellCheck results..");
                }
            }

            catch (Exception ex)
            {
                Console.WriteLine("Encountered exception. " + ex.Message);
            }

            Console.WriteLine("Any key to exit...");
            Console.ReadKey();
        }

        // This will trigger an error response from the API.
        public static void SpellCheckError(string subscriptionKey)
        {
            var client = new SpellCheckAPI(new ApiKeyServiceClientCredentials(subscriptionKey));

            try
            {
                var result = client.SpellCheckerAsync(mode: "proof").Result;
                Console.WriteLine("Correction for Query# \"empty text field\"");
            }
            catch (Exception ex)
            {
                if (ex.GetBaseException().GetType() == typeof(Exception) )
                {
                    Console.WriteLine("Encountered exception. " + ex.Message);
                }
            }
        }
    }
}

```

## <a name="next-steps"></a>Nächste Schritte

[Cognitive Services SDK-Beispiele für .NET](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)
