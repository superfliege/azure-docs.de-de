---
title: 'Schnellstart: Analysieren von Text auf anstößige Inhalte in C#'
titlesuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie Text unter Verwendung des Content Moderator SDK für .NET auf verschiedene anstößige Inhalte analysieren.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: quickstart
ms.date: 10/31/2018
ms.author: sajagtap
ms.openlocfilehash: 0540a81db93570928dd33b66a69b6883b2df0cd9
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2018
ms.locfileid: "51007687"
---
# <a name="quickstart-analyze-text-content-for-objectionable-material-in-c"></a>Schnellstart: Analysieren von Text auf anstößige Inhalte in C# 

Dieser Artikel enthält Informationen und Codebeispiele, die Ihnen den Einstieg in die Verwendung des [Content Moderator SDK für .NET](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) erleichtern. Sie erfahren, wie Sie eine begriffsbasierte Filterung und Klassifizierung von Textinhalten ausführen, um potenziell anstößige Inhalte zu moderieren.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen. 

## <a name="prerequisites"></a>Voraussetzungen
- Ein Content Moderator-Abonnementschlüssel. Gehen Sie wie unter [Schnellstart: Erstellen eines Cognitive Services-Kontos im Azure-Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) beschrieben vor, um Content Moderator zu abonnieren und Ihren Schlüssel zu erhalten.
- Eine beliebige Edition von [Visual Studio 2015 oder 2017](https://www.visualstudio.com/downloads/).

> [!NOTE]
> In dieser Anleitung wird ein kostenloses Content Moderator-Abonnement verwendet. Weitere Informationen zum Umfang der einzelnen Abonnementtarife finden Sie auf der Seite [Preise und Einschränkungen](https://azure.microsoft.com/pricing/details/cognitive-services/content-moderator/).

## <a name="create-the-visual-studio-project"></a>Erstellen des Visual Studio-Projekts

1. Erstellen Sie in Visual Studio ein neues Projekt vom Typ **Konsolen-App (.NET Framework)**, und nennen Sie es **TextModeration**. 
1. Sollte Ihre Projektmappe noch andere Projekte enthalten, wählen Sie dieses Projekt als das alleinige Startprojekt aus.
1. Rufen Sie die erforderlichen NuGet-Pakete ab. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, wählen Sie **NuGet-Pakete verwalten** aus, suchen Sie nach den folgenden Paketen, und installieren Sie sie:
    - Microsoft.Azure.CognitiveServices.ContentModerator
    - Microsoft.Rest.ClientRuntime
    - Newtonsoft.Json

## <a name="add-text-moderation-code"></a>Hinzufügen von Code für die Textmoderation

Kopieren Sie als nächstes den Code aus dieser Anleitung, und fügen Sie ihn in Ihr Projekt ein, um ein einfaches Inhaltsmoderationsszenario zu implementieren.

### <a name="include-namespaces"></a>Einschließen von Namespaces

Fügen Sie am Anfang der Datei *Program.cs* die folgenden `using`-Anweisungen hinzu:

```csharp
using Microsoft.Azure.CognitiveServices.ContentModerator;
using Microsoft.CognitiveServices.ContentModerator;
using Microsoft.CognitiveServices.ContentModerator.Models;
using Newtonsoft.Json;
using System;
using System.Collections.Generic;
using System.IO;
using System.Threading;
```

### <a name="create-the-content-moderator-client"></a>Erstellen des Content Moderator-Clients

Fügen Sie der Datei *Program.cs* den folgenden Code hinzu, um einen Content Moderator-Clientanbieter für Ihr Abonnement zu erstellen. Fügen Sie den Code parallel zur Klasse **Program** im gleichen Namespace hinzu. Aktualisieren Sie die Felder **AzureRegion** und **CMSubscriptionKey** mit den Werten Ihres Regionsbezeichners und des Abonnementschlüssels.

```csharp
// Wraps the creation and configuration of a Content Moderator client.
public static class Clients
{
    // The region/location for your Content Moderator account, 
    // for example, westus.
    private static readonly string AzureRegion = "YOUR API REGION";

    // The base URL fragment for Content Moderator calls.
    private static readonly string AzureBaseURL =
        $"https://{AzureRegion}.api.cognitive.microsoft.com";

    // Your Content Moderator subscription key.
    private static readonly string CMSubscriptionKey = "YOUR API KEY";

    // Returns a new Content Moderator client for your subscription.
    public static ContentModeratorClient NewClient()
    {
        // Create and initialize an instance of the Content Moderator API wrapper.
        ContentModeratorClient client = new ContentModeratorClient(new ApiKeyServiceClientCredentials(CMSubscriptionKey));

        client.Endpoint = AzureBaseURL;
        return client;
    }
}
```

### <a name="set-up-input-and-output-targets"></a>Einrichten von Ein- und Ausgabezielen

Fügen Sie der Klasse **Program** in _Program.cs_ die folgenden statischen Felder hinzu. Diese geben die Dateien für die Eingabe (Textinhalte) und die Ausgabe (JSON-Inhalte) an.

```csharp
// The name of the file that contains the text to evaluate.
private static string TextFile = "TextFile.txt";

// The name of the file to contain the output from the evaluation.
private static string OutputFile = "TextModerationOutput.txt";
```

Sie müssen die Eingabedatei *TextFile.txt* erstellen und ihren Pfad entsprechend aktualisieren. (Relative Pfade sind relativ zum Ausführungsverzeichnis.) Öffnen Sie _TextFile.txt_, und fügen Sie den zu moderierenden Text hinzu. In dieser Schnellstartanleitung wird folgender Beispieltext verwendet:

```
Is this a grabage or crap email abcdef@abcd.com, phone: 6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, WA 98052.
These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 
0800 820 3300. Also, 999-99-9999 looks like a social security number (SSN).
```

### <a name="load-the-input-text"></a>Laden des Eingabetexts

Fügen Sie der Methode **Main** den folgenden Code hinzu. Der entscheidende Vorgang ist die Methode **ScreenText**. Die dazugehörigen Parameter geben die auszuführenden Inhaltsmoderationsvorgänge an. In diesem Beispiel wird die Methode für Folgendes konfiguriert:
- Erkennen potenziell anstößiger Ausdrücke im Text
- Normalisieren des Texts und automatisches Korrigieren von Tippfehlern
- Erkennen personenbezogener Informationen (Personally Identifiable Information, PII) wie etwa US- und UK-Telefonnummern, E-Mail-Adressen und US-Postadressen.
- Klassifizieren von Text in drei Kategorien mithilfe Machine Learning-basierter Modelle

Weitere Informationen zur Funktionsweise dieser Vorgänge finden Sie unter dem Link im Abschnitt [Nächste Schritte](#next-steps).

```csharp
// Load the input text.
string text = File.ReadAllText(TextFile);
Console.WriteLine("Screening {0}", TextFile);

text = text.Replace(System.Environment.NewLine, " ");
byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(text);
MemoryStream stream = new MemoryStream(byteArray);

// Save the moderation results to a file.
using (StreamWriter outputWriter = new StreamWriter(OutputFile, false))
{
    // Create a Content Moderator client and evaluate the text.
    using (var client = Clients.NewClient())
    {
        // Screen the input text: check for profanity,
        // autocorrect text, check for personally identifying
        // information (PII), and classify the text into three categories
        outputWriter.WriteLine("Autocorrect typos, check for matching terms, PII, and classify.");
        var screenResult =
        client.TextModeration.ScreenText("text/plain", stream, "eng", true, true, null, true);
        outputWriter.WriteLine(
                JsonConvert.SerializeObject(screenResult, Formatting.Indented));
    }
    outputWriter.Flush();
    outputWriter.Close();
}
```

## <a name="run-the-program"></a>Ausführen des Programms

Das Programm schreibt JSON-Zeichenfolgendaten in die Datei _TextModerationOutput.txt_. Der in dieser Schnellstartanleitung verwendete Beispieltext führt zu folgender Ausgabe:

```json
Autocorrect typos, check for matching terms, PII, and classify.
{
"OriginalText": "\"Is this a grabage or crap email abcdef@abcd.com, phone: 6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, WA 98052. These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300. Also, 999-99-9999 looks like a social security number (SSN).\"",
"NormalizedText": "\" Is this a garbage or crap email abide@ abed. com, phone: 6657789887, IP: 255. 255. 255. 255, 1 Microsoft Way, Redmond, WA 98052. These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300. Also, 999-99-9999 looks like a social security number ( SSN) . \"",
"AutoCorrectedText": "\" Is this a garbage or crap email abide@ abed. com, phone: 6657789887, IP: 255. 255. 255. 255, 1 Microsoft Way, Redmond, WA 98052. These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300. Also, 999-99-9999 looks like a social security number ( SSN) . \"",
"Misrepresentation": null,
    
"Classification": {
        "Category1": {
        "Score": 1.5113095059859916E-06
        },
        "Category2": {
        "Score": 0.12747249007225037
        },
        "Category3": {
        "Score": 0.98799997568130493
        },
        "ReviewRecommended": true
  },
  "Status": {
    "Code": 3000,
    "Description": "OK",
    "Exception": null
  },
  "PII": {
        "Email": [
            {
            "Detected": "abcdef@abcd.com",
            "SubType": "Regular",
            "Text": "abcdef@abcd.com",
            "Index": 33
            }
            ],
        "IPA": [
            {
            "SubType": "IPV4",
            "Text": "255.255.255.255",
            "Index": 73
            }
            ],
        "Phone": [
            {
            "CountryCode": "US",
            "Text": "6657789887",
            "Index": 57
            },
            {
            "CountryCode": "US",
            "Text": "870 608 4000",
            "Index": 211
            },
            {
            "CountryCode": "UK",
            "Text": "+44 870 608 4000",
            "Index": 207
            },
            {
            "CountryCode": "UK",
            "Text": "0344 800 2400",
            "Index": 227
            },
            {
        "CountryCode": "UK",
            "Text": "0800 820 3300",
            "Index": 244
            }
            ],
         "Address": [{
             "Text": "1 Microsoft Way, Redmond, WA 98052",
             "Index": 89
                }]
    },
  "Language": "eng",
  "Terms": [
    {
        "Index": 22,
        "OriginalIndex": 22,
        "ListId": 0,
        "Term": "crap"
    }
  ],
  "TrackingId": "9392c53c-d11a-441d-a874-eb2b93d978d3"
}
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie eine einfache .NET-Anwendung entwickelt, die unter Verwendung des Content Moderator-Diensts relevante Informationen zu einem bestimmten Textbeispiel zurückgibt. Informieren Sie sich als Nächstes über die Bedeutung der verschiedenen Flags und Klassifizierungen, um entscheiden zu können, welche Daten Sie benötigen und wie Ihre App mit ihnen umgehen soll.

> [!div class="nextstepaction"]
> [Textmoderation](text-moderation-api.md)
