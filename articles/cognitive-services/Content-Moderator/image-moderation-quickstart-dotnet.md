---
title: 'Schnellstart: Analysieren von Bildern auf anstößige Inhalte in C#'
titlesuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie Bilder unter Verwendung des Content Moderator SDK für .NET auf verschiedene anstößige Inhalte analysieren.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: quickstart
ms.date: 10/26/2018
ms.author: sajagtap
ms.openlocfilehash: 8f407a42ab2e1538193206dec1955257a5f9940a
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2018
ms.locfileid: "51005494"
---
# <a name="quickstart-analyze-image-content-for-objectionable-material-in-c"></a>Schnellstart: Analysieren von Bildern auf anstößige Inhalte in C#

Dieser Artikel enthält Informationen und Codebeispiele, die Ihnen den Einstieg in die Verwendung des [Content Moderator SDK für .NET](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) erleichtern. Sie erfahren, wie Sie nach nicht jugendfreien oder freizügigen Inhalten, nach extrahierbarem Text und nach menschlichen Gesichtern suchen, um potenziell anstößige Inhalte zu moderieren.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen. 

## <a name="prerequisites"></a>Voraussetzungen

- Ein Content Moderator-Abonnementschlüssel. Gehen Sie wie unter [Schnellstart: Erstellen eines Cognitive Services-Kontos im Azure-Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) beschrieben vor, um Content Moderator zu abonnieren und Ihren Schlüssel zu erhalten.
- Eine beliebige Edition von [Visual Studio 2015 oder 2017](https://www.visualstudio.com/downloads/).


> [!NOTE]
> In dieser Anleitung wird ein kostenloses Content Moderator-Abonnement verwendet. Weitere Informationen zum Umfang der einzelnen Abonnementtarife finden Sie auf der Seite [Preise und Einschränkungen](https://azure.microsoft.com/pricing/details/cognitive-services/content-moderator/).

## <a name="create-the-visual-studio-project"></a>Erstellen des Visual Studio-Projekts

1. Erstellen Sie in Visual Studio ein neues Projekt vom Typ **Konsolen-App (.NET Framework)**, und nennen Sie es **ImageModeration**. 
1. Sollte Ihre Projektmappe noch andere Projekte enthalten, wählen Sie dieses Projekt als das alleinige Startprojekt aus.
1. Rufen Sie die erforderlichen NuGet-Pakete ab. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, wählen Sie **NuGet-Pakete verwalten** aus, suchen Sie nach den folgenden Paketen, und installieren Sie sie:
    - Microsoft.Azure.CognitiveServices.ContentModerator
    - Microsoft.Rest.ClientRuntime
    - Newtonsoft.Json

## <a name="add-image-moderation-code"></a>Hinzufügen von Code für die Bildmoderation

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

Fügen Sie der Klasse **Program** in _Program.cs_ die folgenden statischen Felder hinzu. Diese geben die Dateien für die Eingabe (Bildinhalte) und die Ausgabe (JSON-Inhalte) an.

```csharp
//The name of the file that contains the image URLs to evaluate.
private static string ImageUrlFile = "ImageFiles.txt";

///The name of the file to contain the output from the evaluation.
private static string OutputFile = "ModerationOutput.json";
```

Sie müssen die Eingabedatei *_ImageFiles.txt* erstellen und ihren Pfad entsprechend aktualisieren. (Relative Pfade sind relativ zum Ausführungsverzeichnis.) Öffnen Sie _ImageFiles.txt_, und fügen Sie die URLs von zu moderierenden Bildern hinzu. In dieser Schnellstartanleitung werden als Beispieleingabe die folgenden URLs verwendet:
```
https://moderatorsampleimages.blob.core.windows.net/samples/sample2.jpg
https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png
```

### <a name="create-a-class-to-handle-results"></a>Erstellen einer Klasse zum Behandeln der Ergebnisse

Fügen Sie *Program.cs* den folgenden Code parallel zur Klasse **Program** im gleichen Namespace hinzu. Sie verwenden eine Instanz dieser Klasse, um die Moderationsergebnisse für jedes der überprüften Bilder zu erfassen.

```csharp
// Contains the image moderation results for an image, 
// including text and face detection results.
public class EvaluationData
{
    // The URL of the evaluated image.
    public string ImageUrl;

    // The image moderation results.
    public Evaluate ImageModeration;

    // The text detection results.
    public OCR TextDetection;

    // The face detection results;
    public FoundFaces FaceDetection;
}
```

### <a name="define-the-image-evaluation-method"></a>Definieren der Bildauswertungsmethode

Fügen Sie der **Program**-Klasse die folgende Methode hinzu. Diese Methode wertet ein einzelnes Bild auf drei Arten aus und gibt die Auswertungsergebnisse zurück. Weitere Informationen zur Funktionsweise der einzelnen Vorgänge finden Sie unter dem Link im Abschnitt [Nächste Schritte](#next-steps).

```csharp
// Evaluates an image using the Image Moderation APIs.
private static EvaluationData EvaluateImage(
  ContentModeratorClient client, string imageUrl)
{
    var url = new BodyModel("URL", imageUrl.Trim());

    var imageData = new EvaluationData();

    imageData.ImageUrl = url.Value;

    // Evaluate for adult and racy content.
    imageData.ImageModeration =
        client.ImageModeration.EvaluateUrlInput("application/json", url, true);
    Thread.Sleep(1000);

    // Detect and extract text.
    imageData.TextDetection =
        client.ImageModeration.OCRUrlInput("eng", "application/json", url, true);
    Thread.Sleep(1000);

    // Detect faces.
    imageData.FaceDetection =
        client.ImageModeration.FindFacesUrlInput("application/json", url, true);
    Thread.Sleep(1000);

    return imageData;
}
```

### <a name="load-the-input-images"></a>Laden der Eingabebilder

Fügen Sie der Methode **Main** in der Klasse **Program** den folgenden Code hinzu. Dadurch wird das Programm für den Abruf von Auswertungsdaten für jede Bild-URL in der Eingabedatei eingerichtet. Die Daten werden anschließend in eine einzelne Ausgabedatei geschrieben.

```csharp
// Create an object to store the image moderation results.
List<EvaluationData> evaluationData = new List<EvaluationData>();

// Create an instance of the Content Moderator API wrapper.
using (var client = Clients.NewClient())
{
    // Read image URLs from the input file and evaluate each one.
    using (StreamReader inputReader = new StreamReader(ImageUrlFile))
    {
        while (!inputReader.EndOfStream)
        {
            string line = inputReader.ReadLine().Trim();
            if (line != String.Empty)
            {
                EvaluationData imageData = EvaluateImage(client, line);
                evaluationData.Add(imageData);
            }
        }
    }
}

// Save the moderation results to a file.
using (StreamWriter outputWriter = new StreamWriter(OutputFile, false))
{
    outputWriter.WriteLine(JsonConvert.SerializeObject(
        evaluationData, Formatting.Indented));

    outputWriter.Flush();
    outputWriter.Close();
}
```

## <a name="run-the-program"></a>Ausführen des Programms

Das Programm schreibt JSON-Zeichenfolgendaten in die Datei _ModerationOutput.json_. Die in dieser Schnellstartanleitung verwendeten Beispielbilder führen zu folgender Ausgabe. Beachten Sie, dass jedes Bild über unterschiedliche Abschnitte für `ImageModeration`, `FaceDetection` und `TextDetection` verfügt. Diese entsprechen den drei API-Aufrufen aus der Methode **EvaluateImage**.

```json
[{
  "ImageUrl": "https://moderatorsampleimages.blob.core.windows.net/samples/sample2.jpg",
  "ImageModeration": {
    "cacheID": "7733c303-3b95-4710-a41e-7a322ae81a15_636488005858745661",
    "result": false,
    "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_687c356d-0f00-4aeb-ae5f-c7555af80247",
    "adultClassificationScore": 0.019196987152099609,
    "isImageAdultClassified": false,
    "racyClassificationScore": 0.032390203326940536,
    "isImageRacyClassified": false,
    "advancedInfo": [
      {
        "key": "ImageDownloadTimeInMs",
        "value": "116"
      },
      {
        "key": "ImageSizeInBytes",
        "value": "273405"
      }
    ],
    "status": {
      "code": 3000.0,
      "description": "OK",
      "exception": null
    }
  },
  "TextDetection": {
    "status": {
      "code": 3000.0,
      "description": "OK",
      "exception": null
    },
    "metadata": [
      {
        "key": "ImageDownloadTimeInMs",
        "value": "12"
      },
      {
        "key": "ImageSizeInBytes",
        "value": "273405"
      }
    ],
    "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_814fa162-c5d6-4ca6-997b-30ed0686ca83",
    "cacheId": "3fb69496-c64b-4de9-affd-6dd6d23f3e78_636488005876558920",
    "language": "eng",
    "text": "IF WE DID \r\nALL \r\nTHE THINGS \r\nWE ARE \r\nCAPABLE \r\nOF DOING, \r\nWE WOULD \r\nLITERALLY \r\nASTOUND \r\nOURSELVE \r\n",
    "candidates": []
  },
  "FaceDetection": {
    "status": {
      "code": 3000.0,
      "description": "OK",
      "exception": null
    },
    "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_a2c40dbe-609d-4eb8-b01c-9988388804ea",
    "cacheId": "e4c0b500-ea8e-4a31-8fb3-35f98c4fbd65_636488005889528303",
    "result": false,
    "count": 0,
    "advancedInfo": [
      {
        "key": "ImageDownloadTimeInMs",
        "value": "11"
      },
      {
        "key": "ImageSizeInBytes",
        "value": "273405"
      }
    ],
    "faces": []
  }
},
{
  "ImageUrl": "https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png",
  "ImageModeration": {
    "cacheID": "b4866aa2-5e69-44ed-806a-f9a5d618c8ae_636488005930693926",
    "result": false,
    "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_fdce5510-f689-4791-b081-c2ad54dcfe78",
    "adultClassificationScore": 0.0035635426174849272,
    "isImageAdultClassified": false,
    "racyClassificationScore": 0.021369094029068947,
    "isImageRacyClassified": false,
    "advancedInfo": [
      {
        "key": "ImageDownloadTimeInMs",
        "value": "109"
      },
      {
        "key": "ImageSizeInBytes",
        "value": "2278902"
      }
    ],
    "status": {
      "code": 3000.0,
      "description": "OK",
      "exception": null
    }
  },
  "TextDetection": {
    "status": {
      "code": 3000.0,
      "description": "OK",
      "exception": null
    },
    "metadata": [
      {
        "key": "ImageDownloadTimeInMs",
        "value": "46"
      },
      {
        "key": "ImageSizeInBytes",
        "value": "2278902"
      }
    ],
    "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_08a4bc19-6010-41bb-a440-a77278e167d8",
    "cacheId": "28b37471-41b3-4f79-bd23-965498bcff51_636488005950851288",
    "language": "eng",
    "text": "",
    "candidates": []
  },
  "FaceDetection": {
    "status": {
      "code": 3000.0,
      "description": "OK",
      "exception": null
    },
    "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_40f2ce07-14ba-47cd-ba09-58b557a89854",
    "cacheId": "ec9c1be3-99b7-4bd9-8bc4-dc958c74459f_636488005964914299",
    "result": true,
    "count": 6,
    "advancedInfo": [
      {
        "key": "ImageDownloadTimeInMs",
        "value": "60"
      },
      {
        "key": "ImageSizeInBytes",
        "value": "2278902"
      }
    ],
    "faces": [
      {
        "bottom": 598,
        "left": 44,
        "right": 268,
        "top": 374
      },
      {
        "bottom": 620,
        "left": 308,
        "right": 532,
        "top": 396
      },
      {
        "bottom": 575,
        "left": 594,
        "right": 773,
        "top": 396
      },
      {
        "bottom": 563,
        "left": 812,
        "right": 955,
        "top": 420
      },
      {
        "bottom": 611,
        "left": 972,
        "right": 1151,
        "top": 432
      },
      {
        "bottom": 510,
        "left": 1232,
        "right": 1456,
        "top": 286
      }
    ]
  }
}]
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie eine einfache .NET-Anwendung entwickelt, die unter Verwendung des Content Moderator-Diensts relevante Informationen zu einem bestimmten Bildbeispiel zurückgibt. Informieren Sie sich als Nächstes über die Bedeutung der verschiedenen Flags und Klassifizierungen, um entscheiden zu können, welche Daten Sie benötigen und wie Ihre App mit ihnen umgehen soll.

> [!div class="nextstepaction"]
> [Bildmoderation](image-moderation-api.md)
