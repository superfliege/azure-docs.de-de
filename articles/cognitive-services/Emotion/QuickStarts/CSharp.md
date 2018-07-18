---
title: Emotionen-API-C#-Schnellstart | Microsoft Docs
description: Informationen und Codebeispiele für den schnellen Einstieg in die Verwendung der Emotionen-API mit C# in Cognitive Services.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 11/02/2017
ms.author: anroth
ms.openlocfilehash: 89735ae54395447e3cb421f45db3d6b99001ecd6
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37016564"
---
# <a name="emotion-api-c-quick-start"></a>Emotionen-API-C#-Schnellstart

> [!IMPORTANT]
> Die Vorschauversion der Video-API ist am 30. Oktober 2017 abgelaufen. Um auf einfache Weise Erkenntnisse aus Videos zu extrahieren, testen Sie die neue [Video-Indexer-API (Vorschau)](https://azure.microsoft.com/services/cognitive-services/video-indexer/). Sie können sie darüber hinaus durch Erkennen gesprochener Wörter, Gesichter, Charaktere und Emotionen zum Verbessern der Auffindbarkeit von Inhalten verwenden. Weitere Informationen finden Sie in der Übersicht zum [Video-Indexer (Vorschau)](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview).

Dieser Artikel enthält Informationen und Codebeispiele für den schnellen Einstieg in die Verwendung der [Recognize-Methode der Emotionen-API](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) mit C#. Sie können sie verwenden, um die von Personen auf einem Bild ausgedrückten Emotionen zu erkennen. 

## <a name="prerequisites"></a>Voraussetzungen
* Rufen Sie das Cognitive Services [Emotionen-API Windows-SDK](https://www.nuget.org/packages/Microsoft.ProjectOxford.Emotion/) ab.
* Rufen Sie Ihren kostenlosen [Abonnementschlüssel ab](https://azure.microsoft.com/try/cognitive-services/).

## <a name="emotion-recognition-c-example-request"></a>C#-Beispielanforderung zur Emotionserkennung

Erstellen Sie in Visual Studio eine neue Konsolenanwendung, und ersetzen Sie dann „Program.cs“ durch den folgenden Code. Ändern Sie die `string uri` so, dass die Region verwendet wird, in der Sie Ihre Abonnementschlüssel erhalten haben. Ersetzen Sie den Wert von **Ocp-Apim-Subscription-Key** durch Ihren gültigen Abonnementschlüssel. Um den Abonnementschlüssel zu suchen, navigieren Sie zum Azure-Portal. Navigieren Sie im Navigationsbereich auf der linken Seite im Abschnitt **Schlüssel** zu Ihrer Emotionen-API-Ressource. In ähnlicher Weise können Sie den richtigen Verbindungs-URI im Panel **Übersicht** für Ihre Ressource unter **Endpunkt** abrufen.

![Ihre API-Ressourcenschlüssel](../../media/emotion-api/keys.png)

Um die Antwort auf Ihre Anforderung zu verarbeiten, verwenden Sie eine Bibliothek wie `Newtonsoft.Json`. Auf diese Weise können Sie eine JSON-Zeichenfolge als Abfolge von verwaltbaren Objekten behandeln, die als Token bezeichnet werden. Um Ihrem Paket diese Bibliothek hinzuzufügen, klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf Ihr Projekt, und wählen Sie **Nuget-Pakete verwalten** aus. Suchen Sie dann nach **Newtonsoft**. Das erste Ergebnis sollte **Newtonsoft.Json** sein. Wählen Sie **Installieren** aus. Sie können jetzt in Ihrer Anwendung auf diese Bibliothek verweisen.

![Newtonsoft.Json installieren](../../media/emotion-api/newtonsoft-nuget.png)

```csharp
using System;
using System.IO;
using System.Net.Http.Headers;
using System.Net.Http;
using Newtonsoft.Json.Linq;

namespace CSHttpClientSample
{
    static class Program
    {
        static void Main()
        {
            Console.Write("Enter the path to a JPEG image file:");
            string imageFilePath = Console.ReadLine();

            MakeRequest(imageFilePath);

            Console.WriteLine("\n\n\nWait for the result below, then hit ENTER to exit...\n\n\n");
            Console.ReadLine(); // wait for ENTER to exit program
        }

        static byte[] GetImageAsByteArray(string imageFilePath)
        {
            FileStream fileStream = new FileStream(imageFilePath, FileMode.Open, FileAccess.Read);
            BinaryReader binaryReader = new BinaryReader(fileStream);
            return binaryReader.ReadBytes((int)fileStream.Length);
        }

        static async void MakeRequest(string imageFilePath)
        {
            var client = new HttpClient();

            // Request headers - replace this example key with your valid key.
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", "<your-subscription-key>"); // 

            // NOTE: You must use the same region in your REST call as you used to obtain your subscription keys.
            //   For example, if you obtained your subscription keys from westcentralus, replace "westus" in the 
            //   URI below with "westcentralus".
            string uri = "https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize?";
            HttpResponseMessage response;
            string responseContent;

            // Request body. Try this sample with a locally stored JPEG image.
            byte[] byteData = GetImageAsByteArray(imageFilePath);

            using (var content = new ByteArrayContent(byteData))
            {
                // This example uses content type "application/octet-stream".
                // The other content types you can use are "application/json" and "multipart/form-data".
                content.Headers.ContentType = new MediaTypeHeaderValue("application/octet-stream");
                response = await client.PostAsync(uri, content);
                responseContent = response.Content.ReadAsStringAsync().Result;
            }

            // A peek at the raw JSON response.
            Console.WriteLine(responseContent);

            // Processing the JSON into manageable objects.
            JToken rootToken = JArray.Parse(responseContent).First;

            // First token is always the faceRectangle identified by the API.
            JToken faceRectangleToken = rootToken.First;

            // Second token is all emotion scores.
            JToken scoresToken = rootToken.Last;

            // Show all face rectangle dimensions
            JEnumerable<JToken> faceRectangleSizeList = faceRectangleToken.First.Children();
            foreach (var size in faceRectangleSizeList) {
                Console.WriteLine(size);
            }

            // Show all scores
            JEnumerable<JToken> scoreList = scoresToken.First.Children();
            foreach (var score in scoreList) {
                Console.WriteLine(score);
            }
        }
    }
}
```

## <a name="recognize-emotions-sample-response"></a>Erkennen der Emotionen-Beispielantwort
Ein erfolgreicher Aufruf gibt ein Array von Gesichtseinträgen zusammen mit den ihnen zugeordneten Emotionsbewertungen zurück. Sie sind in der Rangfolge der Rechteckgröße des Gesichts in absteigender Reihenfolge aufgelistet. Eine leere Antwort zeigt an, dass keine Gesichter erkannt wurden. Ein Emotionseintrag enthält die folgenden Felder:

* faceRectangle: Rechteckposition des Gesichts auf dem Bild
* scores: Emotionsbewertungen für jedes Gesicht auf dem Bild 

```json
application/json 
[
  {
    "faceRectangle": {
      "left": 68,
      "top": 97,
      "width": 64,
      "height": 97
    },
    "scores": {
      "anger": 0.00300731952,
      "contempt": 5.14648448E-08,
      "disgust": 9.180124E-06,
      "fear": 0.0001912825,
      "happiness": 0.9875571,
      "neutral": 0.0009861537,
      "sadness": 1.889955E-05,
      "surprise": 0.008229999
    }
  }
]
