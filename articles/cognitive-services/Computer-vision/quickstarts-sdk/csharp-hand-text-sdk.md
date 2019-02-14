---
title: 'Schnellstart: Extrahieren von handschriftlichem Text – SDK, C#'
titleSuffix: Azure Cognitive Services
description: In diesem Schnellstart extrahieren Sie Text aus einem Bild, indem Sie die Windows-Clientbibliothek der Maschinelles Sehen-API verwenden.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 09/27/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: ca11c25f5e2dc793bc3c8a4ee10b4a89139ce1f3
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55879223"
---
# <a name="quickstart-extract-text-using-the-computer-vision-sdk-and-c"></a>Schnellstart: Extrahieren von Text mit dem SDK für maschinelles Sehen und C#

In dieser Schnellstartanleitung extrahieren Sie handschriftlichen oder gedruckten Text aus einem Bild, indem Sie die Windows-Clientbibliothek der Maschinelles Sehen-API verwenden.

## <a name="prerequisites"></a>Voraussetzungen

* Zum Verwenden der Maschinelles Sehen-API benötigen Sie einen Abonnementschlüssel. Siehe [Obtaining Subscription Keys](../Vision-API-How-to-Topics/HowToSubscribe.md) (Abrufen von Abonnementschlüsseln).
* Eine beliebige Edition von [Visual Studio 2015 oder 2017](https://www.visualstudio.com/downloads/).
* Das NuGet-Paket [Microsoft.Azure.CognitiveServices.Vision.ComputerVision](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.ComputerVision) mit der Clientbibliothek. Es ist nicht erforderlich, das Paket herunterzuladen. Unten ist die Installationsanleitung angegeben.

## <a name="recognizetextasync-method"></a>RecognizeTextAsync-Methode

> [!TIP]
> Laden Sie den aktuellen Code als Visual Studio-Projektmappe von [GitHub](https://github.com/Azure-Samples/cognitive-services-vision-csharp-sdk-quickstarts/tree/master/ComputerVision) herunter.

Die Methoden `RecognizeTextAsync` und `RecognizeTextInStreamAsync` umschließen die [Bildanalyse-API](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200) für lokale Bilder bzw. Remotebilder. Die `GetTextOperationResultAsync`-Methode umschließt die [Get Recognize Text Operation Result-API](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2cf1154055056008f201).  Sie können diese Methoden verwenden, um Text in einem Bild zu erkennen und erkannte Zeichen als computerlesbare Zeichenfolge zu extrahieren.

Führen Sie zum Ausführen des Beispiels die folgenden Schritte aus:

1. Erstellen Sie in Visual Studio eine neue Visual C#-Konsolen-App.
1. Installieren Sie das NuGet-Paket mit der Clientbibliothek der Maschinelles Sehen-API.
    1. Klicken Sie im Menü auf **Werkzeuge**, wählen Sie **NuGet-Paket-Manager** und dann **NuGet-Pakete für Projektmappe verwalten** aus.
    1. Klicken Sie auf die Registerkarte **Durchsuchen**, und geben Sie „Microsoft.Azure.CognitiveServices.Vision.ComputerVision“ in das Feld **Suchen** ein.
    1. Wählen Sie **Microsoft.Azure.CognitiveServices.Vision.ComputerVision** aus, wenn das Paket angezeigt wird, aktivieren Sie das Kontrollkästchen neben dem Projektnamen, und klicken Sie dann auf **Installieren**.
1. Ersetzen Sie `Program.cs` durch den folgenden Code.
1. Ersetzen Sie `<Subscription Key>` durch Ihren gültigen Abonnementschlüssel.
1. Ändern Sie `computerVision.Endpoint` bei Bedarf in die Azure-Region, die Ihren Abonnementschlüsseln zugeordnet ist, wenn erforderlich.
1. Legen Sie optional `textRecognitionMode` auf `TextRecognitionMode.Printed` fest.
1. Ersetzen Sie `<LocalImage>` durch den Pfad und Dateinamen eines lokalen Bilds.
1. Legen Sie optional `remoteImageUrl` auf ein anderes Bild fest.
1. Führen Sie das Programm aus.

```csharp
using Microsoft.Azure.CognitiveServices.Vision.ComputerVision;
using Microsoft.Azure.CognitiveServices.Vision.ComputerVision.Models;

using System;
using System.IO;
using System.Threading.Tasks;

namespace ExtractText
{
    class Program
    {
        // subscriptionKey = "0123456789abcdef0123456789ABCDEF"
        private const string subscriptionKey = "<SubscriptionKey>";

        // For printed text, change to TextRecognitionMode.Printed
        private const TextRecognitionMode textRecognitionMode =
            TextRecognitionMode.Handwritten;

        // localImagePath = @"C:\Documents\LocalImage.jpg"
        private const string localImagePath = @"<LocalImage>";

        private const string remoteImageUrl =
            "https://upload.wikimedia.org/wikipedia/commons/thumb/d/dd/" +
            "Cursive_Writing_on_Notebook_paper.jpg/" +
            "800px-Cursive_Writing_on_Notebook_paper.jpg";

        private const int numberOfCharsInOperationId = 36;

        static void Main(string[] args)
        {
            ComputerVisionClient computerVision = new ComputerVisionClient(
                new ApiKeyServiceClientCredentials(subscriptionKey),
                new System.Net.Http.DelegatingHandler[] { });

            // You must use the same region as you used to get your subscription
            // keys. For example, if you got your subscription keys from westus,
            // replace "westcentralus" with "westus".
            //
            // Free trial subscription keys are generated in the "westus"
            // region. If you use a free trial subscription key, you shouldn't
            // need to change the region.

            // Specify the Azure region
            computerVision.Endpoint = "https://westcentralus.api.cognitive.microsoft.com";

            Console.WriteLine("Images being analyzed ...");
            var t1 = ExtractRemoteTextAsync(computerVision, remoteImageUrl);
            var t2 = ExtractLocalTextAsync(computerVision, localImagePath);

            Task.WhenAll(t1, t2).Wait(5000);
            Console.WriteLine("Press ENTER to exit");
            Console.ReadLine();
        }

        // Recognize text from a remote image
        private static async Task ExtractRemoteTextAsync(
            ComputerVisionClient computerVision, string imageUrl)
        {
            if (!Uri.IsWellFormedUriString(imageUrl, UriKind.Absolute))
            {
                Console.WriteLine(
                    "\nInvalid remoteImageUrl:\n{0} \n", imageUrl);
                return;
            }

            // Start the async process to recognize the text
            RecognizeTextHeaders textHeaders =
                await computerVision.RecognizeTextAsync(
                    imageUrl, textRecognitionMode);

            await GetTextAsync(computerVision, textHeaders.OperationLocation);
        }

        // Recognize text from a local image
        private static async Task ExtractLocalTextAsync(
            ComputerVisionClient computerVision, string imagePath)
        {
            if (!File.Exists(imagePath))
            {
                Console.WriteLine(
                    "\nUnable to open or read localImagePath:\n{0} \n", imagePath);
                return;
            }

            using (Stream imageStream = File.OpenRead(imagePath))
            {
                // Start the async process to recognize the text
                RecognizeTextInStreamHeaders textHeaders =
                    await computerVision.RecognizeTextInStreamAsync(
                        imageStream, textRecognitionMode);

                await GetTextAsync(computerVision, textHeaders.OperationLocation);
            }
        }

        // Retrieve the recognized text
        private static async Task GetTextAsync(
            ComputerVisionClient computerVision, string operationLocation)
        {
            // Retrieve the URI where the recognized text will be
            // stored from the Operation-Location header
            string operationId = operationLocation.Substring(
                operationLocation.Length - numberOfCharsInOperationId);

            Console.WriteLine("\nCalling GetHandwritingRecognitionOperationResultAsync()");
            TextOperationResult result =
                await computerVision.GetTextOperationResultAsync(operationId);

            // Wait for the operation to complete
            int i = 0;
            int maxRetries = 10;
            while ((result.Status == TextOperationStatusCodes.Running ||
                    result.Status == TextOperationStatusCodes.NotStarted) && i++ < maxRetries)
            {
                Console.WriteLine(
                    "Server status: {0}, waiting {1} seconds...", result.Status, i);
                await Task.Delay(1000);

                result = await computerVision.GetTextOperationResultAsync(operationId);
            }

            // Display the results
            Console.WriteLine();
            var lines = result.RecognitionResult.Lines;
            foreach (Line line in lines)
            {
                Console.WriteLine(line.Text);
            }
            Console.WriteLine();
        }
    }
}
```

## <a name="recognizetextasync-response"></a>RecognizeTextAsync-Antwort

Bei einer erfolgreichen Antwort werden für jedes Bild die Zeilen mit dem erkannten Text angezeigt.

Unter [Schnellstart: Extrahieren von handschriftlichem Text – REST, C#](../QuickStarts/CSharp-hand-text.md#examine-the-response) finden Sie ein Beispiel für eine unformatierte JSON-Ausgabe.

```cmd
Calling GetHandwritingRecognitionOperationResultAsync()

Calling GetHandwritingRecognitionOperationResultAsync()
Server status: Running, waiting 1 seconds...
Server status: Running, waiting 1 seconds...

dog
The quick brown fox jumps over the lazy
Pack my box with five dozen liquor jugs
```

## <a name="next-steps"></a>Nächste Schritte

Erkunden Sie die Maschinelles Sehen-APIs, die zum Analysieren von Bildern, Erkennen von Prominenten und Sehenswürdigkeiten, Erstellen von Miniaturansichten und Extrahieren von gedrucktem sowie handschriftlichem Text verwendet werden.

> [!div class="nextstepaction"]
> [Maschinelles Sehen-APIs erkunden](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
