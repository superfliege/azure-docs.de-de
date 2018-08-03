---
title: 'Azure Content Moderator: Moderieren von Text mithilfe von .NET | Microsoft-Dokumentation'
description: Moderieren von Text mithilfe des Azure Content Moderator SDK für .NET
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/04/2018
ms.author: sajagtap
ms.openlocfilehash: 7320286e186d7e6ba4041d3ed52f19e573b4d7e3
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/14/2018
ms.locfileid: "39049880"
---
# <a name="moderate-text-using-net"></a>Moderieren von Text mithilfe von .NET

Dieser Artikel enthält Informationen und Codebeispiele, die Ihnen den Einstieg in die Verwendung des Content Moderator SDK für .NET erleichtern, um Folgendes durchzuführen:
- Erkennen potenziell anstößiger Ausdrücke in Text mit begriffsbasierter Filterung
- [Klassifizieren von Text](text-moderation-api.md#classification) in drei Kategorien mithilfe Machine Learning-basierter Modelle
- Erkennen personenbezogener Informationen (Personally Identifiable Information, PII) wie etwa US- und UK-Telefonnummern, E-Mail-Adressen und US-Postadressen.
- Normalisieren von Text und automatisches Korrigieren von Tippfehlern

In diesem Artikel wird davon ausgegangen, dass Sie bereits mit Visual Studio und C# vertraut sind.

## <a name="sign-up-for-content-moderator-services"></a>Registrieren für Content Moderator-Dienste

Um Content Moderator-Dienste über die REST-API oder über das SDK verwenden zu können, benötigen Sie einen Abonnementschlüssel.
Informationen zum Abrufen des Schlüssels finden Sie in [diesem Schnellstart](quick-start.md).

## <a name="create-your-visual-studio-project"></a>Erstellen Ihres Visual Studio-Projekts

1. Fügen Sie Ihrer Projektmappe ein neues Projekt vom Typ **Konsolen-App (.NET Framework)** hinzu.

   Versehen Sie das Projekt im Beispielcode mit dem Namen **TextModeration**.

1. Wählen Sie dieses Projekt als einzelnes Startprojekt für die Projektmappe aus.

1. Fügen Sie einen Verweis auf die Projektassembly **ModeratorHelper** hinzu, die Sie im Schnellstart [Hilfscode zum Zurückgeben eines Content Moderator-Clients](content-moderator-helper-quickstart-dotnet.md) erstellt haben.

### <a name="install-required-packages"></a>Installieren erforderlicher Pakete

Installieren Sie die folgenden NuGet-Pakete:

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Newtonsoft.Json

### <a name="update-the-programs-using-statements"></a>Aktualisieren der using-Anweisungen des Programms

Ändern Sie die using-Anweisungen des Programms.

    using Microsoft.CognitiveServices.ContentModerator;
    using Microsoft.CognitiveServices.ContentModerator.Models;
    using ModeratorHelper;
    using Newtonsoft.Json;
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Threading;


### <a name="initialize-application-specific-settings"></a>Initialisieren anwendungsspezifischer Einstellungen

Fügen Sie der Klasse **Program** in „Program.cs“ die folgenden statischen Felder hinzu.

    /// <summary>
    /// The name of the file that contains the text to evaluate.
    /// </summary>
    /// <remarks>You will need to create an input file and update this path
    /// accordingly. Relative paths are ralative the execution directory.</remarks>
    private static string TextFile = "TextFile.txt";

    /// <summary>
    /// The name of the file to contain the output from the evaluation.
    /// </summary>
    /// <remarks>Relative paths are ralative the execution directory.</remarks>
    private static string OutputFile = "TextModerationOutput.txt";

Für diese Schnellstartanleitung wurde folgender Text verwendet, um die Ausgabe zu generieren:

> [!NOTE]
> Die ungültige US-Sozialversicherungsnummer im folgenden Beispieltext ist beabsichtigt. Hier soll das Format der Beispieleingabe und -ausgabe vermittelt werden.

    Is this a grabage or crap email abcdef@abcd.com, phone: 6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, WA 98052.
    These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 
    0800 820 3300. Also, 999-99-9999 looks like a social security number (SSN).

## <a name="add-code-to-load-and-evaluate-the-input-text"></a>Hinzufügen von Code zum Laden und Auswerten des Eingabetexts

Fügen Sie der Methode **Main** den folgenden Code hinzu:

    // Load the input text.
    string text = File.ReadAllText(TextFile);
    text = text.Replace(System.Environment.NewLine, " ");

    // Save the moderation results to a file.
    using (StreamWriter outputWriter = new StreamWriter(OutputFile, false))
    {
        // Create a Content Moderator client and evaluate the text.
        using (var client = Clients.NewClient())
        {
            // Screen the input text: check for profanity, classify the text into three categories
                // do autocorrect text, and check for personally identifying 
                // information (PII)
                outputWriter.WriteLine("Autocorrect typos, check for matching terms, PII, and classify.");
                var screenResult =
                    client.TextModeration.ScreenText("eng", "text/plain", text, true, true, null, true);
                outputWriter.WriteLine(
                        JsonConvert.SerializeObject(screenResult, Formatting.Indented));
        }
        outputWriter.Flush();
        outputWriter.Close();
    }

> [!NOTE]
> Die RPS-Rate (Requests Per Second, Anforderungen pro Sekunde) Ihres Content Moderator-Dienstschlüssels ist begrenzt. Bei Überschreitung des Limits löst das SDK eine Ausnahme mit dem Fehlercode 429 aus.
>
> Wenn Sie einen Schlüssel des Free-Tarifs verwenden, ist die Rate der Anforderungen auf eine Anforderung pro Sekunde beschränkt.

## <a name="run-the-program-and-review-the-output"></a>Ausführen des Programms und Überprüfen der Ausgabe

Die Beispielausgabe für das Programm sieht wie folgt aus. (Hierbei handelt es sich um die Ausgabe, die in die Protokolldatei geschrieben wird.)

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
            "   CountryCode": "UK",
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

## <a name="next-steps"></a>Nächste Schritte

[Laden Sie die entsprechende Visual Studio-Projektmappe sowie andere Content Moderator-Schnellstarts für .NET herunter](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator), und beginnen Sie mit der Integration.
