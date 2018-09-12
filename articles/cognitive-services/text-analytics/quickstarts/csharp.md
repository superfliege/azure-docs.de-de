---
title: C#-Schnellstart für Cognitive Services, Textanalyse-API | Microsoft-Dokumentation
description: Informationen und Codebeispiele für die ersten Schritte mit der Textanalyse-API in Microsoft Cognitive Services in Azure
services: cognitive-services
documentationcenter: ''
author: luiscabrer
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: article
ms.date: 08/30/2018
ms.author: ashmaka
ms.openlocfilehash: 42a682898303b742a17b0a6d4d98c2b9fedf9003
ms.sourcegitcommit: 3d0295a939c07bf9f0b38ebd37ac8461af8d461f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2018
ms.locfileid: "43841693"
---
# <a name="quickstart-for-the-text-analytics-api-with-c"></a>Schnellstart für die Textanalyse-API mit C# 
<a name="HOLTop"></a>

Dieser Artikel veranschaulicht, wie Sie mithilfe der [Textanalyse-APIs](//go.microsoft.com/fwlink/?LinkID=759711) mit C# Sprachen erkennen, Stimmungen analysieren und Schlüsselausdrücke extrahieren. Der Code wurde für eine .NET Core-Anwendung geschrieben und enthält minimale Verweise auf externe Bibliotheken, damit Sie ihn auch unter Linux oder macOS ausführen können.

Die technische Dokumentation für die APIs finden Sie in den [API-Definitionen](//go.microsoft.com/fwlink/?LinkID=759346).

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen ein [Cognitive Services-API-Konto](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) mit der Textanalyse-API. Sie können diesen Schnellstart mit dem *Free-Tarif für 5.000 Transaktionen/Monat* abschließen.

Außerdem benötigen Sie den [Endpunkt- und den Zugriffsschlüssel](../How-tos/text-analytics-how-to-access-key.md), die bei der Registrierung für Sie generiert wurden. 


## <a name="install-the-nuget-sdk-package"></a>Installieren des NuGet-SDK-Pakets
1. Erstellen Sie eine neue Konsolenprojektmappe in Visual Studio.
1. Klicken Sie mit der rechten Maustaste auf die Projektmappe, und wählen Sie **NuGet-Pakete für Projektmappe verwalten** aus.
1. Aktivieren Sie das Kontrollkästchen **Vorabversion einbeziehen**.
1. Wählen Sie die Registerkarte **Durchsuchen** aus, und suchen Sie nach **Microsoft.Azure.CognitiveServices.Language**.
1. Wählen Sie das NuGet-Paket **Microsoft.Azure.CognitiveServices.Language.TextAnalytics** aus, und installieren Sie es.

> [!Tip]
> Sie können die [HTTP-Endpunkte](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6) zwar direkt über C# aufrufen, das SDK Microsoft.Azure.CognitiveServices.Language erleichtert jedoch das Aufrufen des Diensts erheblich, und Sie müssen sich nicht um die Serialisierung und Deserialisierung von JSON kümmern.
>
> Einige nützliche Links:
> - [NuGet-Seite des SDK](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.TextAnalytics)
> - [SDK-Code](https://github.com/Azure/azure-sdk-for-net/tree/psSdkJson6/src/SDKs/CognitiveServices/dataPlane/Language/TextAnalytics)


## <a name="call-the-text-analytics-api-by-using-the-sdk"></a>Aufrufen der Textanalyse-API mit dem SDK
1. Ersetzen Sie „Program.cs“ durch den folgenden Code. Dieses Programm veranschaulicht die Funktionen der Textanalyse-API in drei Abschnitten (Sprachextraktion, Schlüsselausdrucksextraktion und Stimmungsanalyse).
1. Ersetzen Sie den Headerwert `Ocp-Apim-Subscription-Key` durch einen für Ihr Abonnement gültigen Zugriffsschlüssel.
1. Ersetzen Sie den Standort in `Endpoint` durch den Endpunkt, für den Sie sich registriert haben. Sie finden den Endpunkt in der Ressource im Azure-Portal. In der Regel beginnt der Endpunkt mit https://[Region].api.cognitive.microsoft.com. Fügen Sie nur das Protokoll und den Hostnamen ein.
1. Führen Sie das Programm aus.

```csharp
using System;
using Microsoft.Azure.CognitiveServices.Language.TextAnalytics;
using Microsoft.Azure.CognitiveServices.Language.TextAnalytics.Models;
using System.Collections.Generic;
using Microsoft.Rest;
using System.Linq;
using System.Net.Http;
using System.Threading;
using System.Threading.Tasks;

namespace ConsoleApp1
{
    class Program
    {
        /// <summary>
        /// Container for subscription credentials. Make sure to enter your valid key.
        /// </summary>
        class ApiKeyServiceClientCredentials : ServiceClientCredentials
        {
            public override Task ProcessHttpRequestAsync(HttpRequestMessage request, CancellationToken cancellationToken)
            {
                request.Headers.Add("Ocp-Apim-Subscription-Key", "ENTER KEY HERE");
                return base.ProcessHttpRequestAsync(request, cancellationToken);
            }
        }

        static void Main(string[] args)
        {

            // Create a client.
            ITextAnalyticsClient client = new TextAnalyticsClient(new ApiKeyServiceClientCredentials())
            {
                Endpoint = "https://westus.api.cognitive.microsoft.com"
            };

            Console.OutputEncoding = System.Text.Encoding.UTF8;
```

## <a name="detect-language"></a>Sprache erkennen

Die Sprachenerkennungs-API erfasst die Sprache eines Textdokuments mithilfe der [Sprachenerkennungsmethode](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7).

```csharp
            // Extracting language.
            Console.WriteLine("===== LANGUAGE EXTRACTION ======");

            var result =  client.DetectLanguageAsync(new BatchInput(
                    new List<Input>()
                        {
                          new Input("1", "This is a document written in English."),
                          new Input("2", "Este es un document escrito en Español."),
                          new Input("3", "这是一个用中文写的文件")
                    })).Result;

            // Printing language results.
            foreach (var document in result.Documents)
            {
                Console.WriteLine("Document ID: {0} , Language: {1}", document.Id, document.DetectedLanguages[0].Name);
            }
```

## <a name="extract-key-phrases"></a>Extrahieren von Schlüsselbegriffen

Die API zur Schlüsselbegriffserkennung extrahiert Schlüsselbegriffe aus einem Textdokument mithilfe der [Schlüsselbegriffsmethode](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6).

```csharp
            // Getting key phrases.
            Console.WriteLine("\n\n===== KEY-PHRASE EXTRACTION ======");

            KeyPhraseBatchResult result2 = client.KeyPhrasesAsync(new MultiLanguageBatchInput(
                        new List<MultiLanguageInput>()
                        {
                          new MultiLanguageInput("ja", "1", "猫は幸せ"),
                          new MultiLanguageInput("de", "2", "Fahrt nach Stuttgart und dann zum Hotel zu Fu."),
                          new MultiLanguageInput("en", "3", "My cat is stiff as a rock."),
                          new MultiLanguageInput("es", "4", "A mi me encanta el fútbol!")
                        })).Result;

            // Printing key phrases.
            foreach (var document in result2.Documents)
            {
                Console.WriteLine("Document ID: {0} ", document.Id);

                Console.WriteLine("\t Key phrases:");

                foreach (string keyphrase in document.KeyPhrases)
                {
                    Console.WriteLine("\t\t" + keyphrase);
                }
            }
```

## <a name="analyze-sentiment"></a>Analysieren von Stimmungen

Die Standpunktanalyse-API erkennt die Stimmung in einer Gruppe von Textdatensätzen mithilfe der [Stimmungsmethode](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c9).

```csharp
            // Analyzing sentiment.
            Console.WriteLine("\n\n===== SENTIMENT ANALYSIS ======");

            SentimentBatchResult result3 = client.SentimentAsync(
                    new MultiLanguageBatchInput(
                        new List<MultiLanguageInput>()
                        {
                          new MultiLanguageInput("en", "0", "I had the best day of my life."),
                          new MultiLanguageInput("en", "1", "This was a waste of my time. The speaker put me to sleep."),
                          new MultiLanguageInput("es", "2", "No tengo dinero ni nada que dar..."),
                          new MultiLanguageInput("it", "3", "L'hotel veneziano era meraviglioso. È un bellissimo pezzo di architettura."),
                        })).Result;


            // Printing sentiment results.
            foreach (var document in result3.Documents)
            {
                Console.WriteLine("Document ID: {0} , Sentiment Score: {1:0.00}", document.Id, document.Score);
            }
```

## <a name="identify-linked-entities"></a>Erkennen von verknüpften Entitäten

Die Entitätsverknüpfungs-API erkennt bekannte Entitäten in einem Textdokument mithilfe der [Entitätsverknüpfungsmethode](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634).

```csharp
            // Linking entities
            Console.WriteLine("\n\n===== ENTITY LINKING ======");

            EntitiesBatchResult result4 = client.EntitiesAsync(
                    new MultiLanguageBatchInput(
                        new List<MultiLanguageInput>()
                        {
                            new MultiLanguageInput("en", "0", "I really enjoy the new XBox One S. It has a clean look, it has 4K/HDR resolution and it is affordable."),
                            new MultiLanguageInput("en", "1", "The Seattle Seahawks won the Super Bowl in 2014."),
                        })).Result;

            // Printing entity results.
            foreach (var document in result4.Documents)
            {
                Console.WriteLine("Document ID: {0} , Entities: {1}", document.Id, String.Join(", ", document.Entities.Select(entity => entity.Name)));
            }
        }
    }
}
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Textanalyse mit Power BI](../tutorials/tutorial-power-bi-key-phrases.md)

## <a name="see-also"></a>Weitere Informationen 

 [Übersicht über die Textanalyse](../overview.md)  
 [Häufig gestellte Fragen (FAQ)](../text-analytics-resource-faq.md)
