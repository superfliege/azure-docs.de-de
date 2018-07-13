---
title: Verwenden des Endpunkts der Custom Vision Service-Vorhersage – Azure Cognitive Services | Microsoft-Dokumentation
description: API-Nutzung für das programmgesteuerte Testen von Bildern mit Ihrer Custom Vision Service-Klassifizierung
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 05/03/2018
ms.author: anroth
ms.openlocfilehash: 54f9d9fec1f40c167341dec6a8699b6a558419da
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35375299"
---
# <a name="use-the-prediction-endpoint-to-test-images-programmatically-with-a-custom-vision-service-classifier"></a>Verwenden des Endpunkts der Vorhersage für das programmgesteuerte Testen von Bildern mit einer Custom Vision Service-Klassifizierung

Nachdem Sie Ihr Modell trainiert haben, können Sie Bilder programmgesteuert testen, indem Sie sie an die Vorhersage-API senden. 

> [!NOTE]
> Dieser Artikel veranschaulicht, wie Sie mit C# ein Bild an die Vorhersage-API senden. Weitere Informationen und Beispiele für die Verwendung der API finden Sie in der [Vorhersage-API-Referenz](https://go.microsoft.com/fwlink/?linkid=865445).

## <a name="get-the-url-and-prediction-key"></a>Abrufen der Vorhersage-URL und des Vorhersage-Schlüssels

Wählen Sie auf der [Custom Vision-Webseite](https://customvision.ai) Ihr Projekt und dann die Registerkarte __Leistung__ aus. Wählen Sie __Vorhersage-URL__ aus, um Informationen zur Nutzung der Vorhersage-API anzuzeigen. Kopieren Sie die folgenden Angaben, um sie in der Anwendung einzufügen:

* Die __URL__ für die Verwendung einer __Bilddatei__
* Den Wert für den __Vorhersageschlüssel__

> [!TIP]
> Wenn Sie mehrere Iterationen haben, können Sie definieren, welche verwendet wird, indem Sie sie als Standard festlegen. Wählen Sie im Abschnitt __Iterationen__ die Iteration und dann oben auf der Seite __Als Standard festlegen__ aus.

![Die Registerkarte „Leistung“ wird mit einem roten Rechteck um die Vorhersage-URL angezeigt.](./media/use-prediction-api/prediction-url.png)

## <a name="create-the-application"></a>Erstellen der Anwendung

1. Erstellen Sie in Visual Studio eine neue C#-Konsolenanwendung.

2. Verwenden Sie den folgenden Code als Textkörper der Datei __Program.cs__.

    > [!IMPORTANT]
    > Ändern Sie die folgenden Angaben:
    >
    > * Legen Sie als __Namespace__ den Namen Ihres Projekts fest.
    > * Legen Sie den Wert für den __Vorhersageschlüssel__ fest, den Sie zuvor erhalten haben. Er beginnt mit `client.DefaultRequestHeaders.Add("Prediction-Key",`.
    > * Legen Sie den Wert für die __URL__ fest, den Sie zuvor erhalten haben. Er beginnt mit `string url =`.

    ```csharp
    using System;
    using System.IO;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Threading.Tasks;

    namespace CSPredictionSample
    {
        static class Program
        {
            static void Main()
            {
                Console.Write("Enter image file path: ");
                string imageFilePath = Console.ReadLine();

                MakePredictionRequest(imageFilePath).Wait();

                Console.WriteLine("\n\n\nHit ENTER to exit...");
                Console.ReadLine();
            }

            static byte[] GetImageAsByteArray(string imageFilePath)
            {
                FileStream fileStream = new FileStream(imageFilePath, FileMode.Open, FileAccess.Read);
                BinaryReader binaryReader = new BinaryReader(fileStream);
                return binaryReader.ReadBytes((int)fileStream.Length);
            }

            static async Task MakePredictionRequest(string imageFilePath)
            {
                var client = new HttpClient();

                // Request headers - replace this example key with your valid subscription key.
                client.DefaultRequestHeaders.Add("Prediction-Key", "13hc77781f7e4b19b5fcdd72a8df7156");

                // Prediction URL - replace this example URL with your valid prediction URL.
                string url = "http://southcentralus.api.cognitive.microsoft.com/customvision/v1.0/prediction/d16e136c-5b0b-4b84-9341-6a3fff8fa7fe/image?iterationId=f4e573f6-9843-46db-8018-b01d034fd0f2";

                HttpResponseMessage response;

                // Request body. Try this sample with a locally stored image.
                byte[] byteData = GetImageAsByteArray(imageFilePath);

                using (var content = new ByteArrayContent(byteData))
                {
                    content.Headers.ContentType = new MediaTypeHeaderValue("application/octet-stream");
                    response = await client.PostAsync(url, content);
                    Console.WriteLine(await response.Content.ReadAsStringAsync());
                }
            }
        }
    }
    ```

## <a name="use-the-application"></a>Verwenden der Anwendung

Wenn die Anwendung ausgeführt wird, geben Sie den Pfad einer Bilddatei ein. Das Bild wird an die API gesendet, und die Ergebnisse werden als JSON-Dokument zurückgegeben. Der folgende JSON-Code ist ein Beispiel für eine Antwort:

```json
{
    "Id":"3f76364c-b8ae-4818-a2b2-2794cfbe377a",
    "Project":"2277aca4-7aff-4742-8afb-3682e251c913",
    "Iteration":"84105bfe-73b5-4fcc-addb-756c0de17df2",
    "Created":"2018-05-03T14:15:22.5659829Z",
    "Predictions":[
        {"TagId":"35ac2ad0-e3ef-4e60-b81f-052a1057a1ca","Tag":"dog","Probability":0.102716163},
        {"TagId":"28e1a872-3776-434c-8cf0-b612dd1a953c","Tag":"cat","Probability":0.02037274}
    ]
}
```

## <a name="next-steps"></a>Nächste Schritte

[Exportieren Ihres Modells für die Verwendung mit Mobilgeräten](export-your-model.md)