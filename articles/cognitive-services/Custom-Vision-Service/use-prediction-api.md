---
title: 'Beispiel: Verwenden des Endpunkts der Vorhersage für das programmgesteuerte Testen von Bildern mit einer Klassifizierung – Custom Vision'
titlesuffix: Azure Cognitive Services
description: API-Nutzung für das programmgesteuerte Testen von Bildern mit Ihrer Custom Vision Service-Klassifizierung
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: article
ms.date: 03/26/2019
ms.author: anroth
ms.openlocfilehash: 715fa526c83608c9922315e3a0d89b67b31e0d16
ms.sourcegitcommit: fbfe56f6069cba027b749076926317b254df65e5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2019
ms.locfileid: "58472723"
---
#  <a name="use-your-model-with-the-prediction-api"></a>Verwenden des Modells mit der Vorhersage-API

Nachdem Sie Ihr Modell trainiert haben, können Sie Bilder programmgesteuert testen, indem Sie sie an die Vorhersage-API senden.

> [!NOTE]
> Dieser Artikel veranschaulicht, wie Sie mit C# ein Bild an die Vorhersage-API senden. Weitere Informationen und Beispiele für die Verwendung der API finden Sie in der [Vorhersage-API-Referenz](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Prediction_3.0/operations/5c82db60bf6a2b11a8247c15).

## <a name="publish-your-trained-iteration"></a>Veröffentlichen der trainierten Iteration

Wählen Sie auf der [Custom Vision-Webseite](https://customvision.ai) Ihr Projekt und dann die Registerkarte __Leistung__ aus.

Um Bilder an die Vorhersage-API zu senden, müssen Sie zuerst Ihre Iteration für die Vorhersage veröffentlichen. Dazu wählen Sie __Veröffentlichen__ aus und geben einen Namen für die veröffentlichte Iteration an. Auf diese Weise wird Ihr Modell für die Vorhersage-API Ihrer Azure-Ressource von Custom Vision zugänglich. 

![Die Registerkarte „Leistung“ wird mit einem roten Rechteck um die Schaltfläche „Veröffentlichen“ angezeigt.](./media/use-prediction-api/unpublished-iteration.png)

Sobald das Modell erfolgreich veröffentlicht wurde, wird neben der Iteration in der linken Seitenleiste die Bezeichnung „Veröffentlicht“ angezeigt, ebenso wie der Name der veröffentlichten Iteration in der Beschreibung der Iteration.

![Die Registerkarte „Leistung“ wird mit einem roten Rechteck um die Bezeichnung „Veröffentlicht“ und den Namen der veröffentlichten Iteration angezeigt.](./media/use-prediction-api/published-iteration.png)

## <a name="get-the-url-and-prediction-key"></a>Abrufen der Vorhersage-URL und des Vorhersage-Schlüssels

Nachdem das Modell veröffentlicht wurde, können Sie Informationen zur Verwendung der Vorhersage-API abrufen, indem Sie __Vorhersage-URL__ auswählen. Dadurch wird ein Dialogfeld wie das unten angezeigte mit Informationen zur Verwendung der Vorhersage-API geöffnet, einschließlich __Vorhersage-URL__ und __Vorhersage-Schlüssel__.

![Die Registerkarte „Leistung“ wird mit einem roten Rechteck um die Schaltfläche „Vorhersage-URL“ angezeigt.](./media/use-prediction-api/published-iteration-prediction-url.png)

![Die Registerkarte „Leistung“ wird mit einem roten Rechteck um den Wert „Vorhersage-URL“ für die Verwendung einer Bilddatei und um den Wert „Vorhersage-Schlüssel“ angezeigt.](./media/use-prediction-api/prediction-api-info.png)

> [!TIP]
> Ihr __Vorhersage-Schlüssel__ befindet sich auch auf der Seite [Azure-Portal](https://portal.azure.com) für die Ihrem Projekt zugeordnete Azure-Ressource von Custom Vision unter __Schlüssel__. 

Kopieren Sie aus dem Dialogfeld die folgenden Angaben, um sie in der Anwendung einzufügen:

* Die __Vorhersage-URL__ für die Verwendung einer __Bilddatei__.
* Der Wert für den __Vorhersage-Schlüssel__.

## <a name="create-the-application"></a>Erstellen der Anwendung

1. Erstellen Sie in Visual Studio eine neue C#-Konsolenanwendung.

1. Verwenden Sie den folgenden Code als Textkörper der Datei __Program.cs__.

    > [!IMPORTANT]
    > Ändern Sie die folgenden Angaben:
    >
    > * Legen Sie als __Namespace__ den Namen Ihres Projekts fest.
    > * Legen Sie den Wert für den __Vorhersage-Schlüssel__ fest, den Sie zuvor in der Zeile erhalten haben, die mit `client.DefaultRequestHeaders.Add("Prediction-Key",` beginnt.
    > * Legen Sie den Wert für die __Vorhersage-URL__ fest, den Sie zuvor in der Zeile erhalten haben, die mit `string url =` beginnt.

    ```csharp
    using System;
    using System.IO;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Threading.Tasks;

    namespace CVSPredictionSample
    {
        public static class Program
        {
            public static void Main()
            {
                Console.Write("Enter image file path: ");
                string imageFilePath = Console.ReadLine();

                MakePredictionRequest(imageFilePath).Wait();

                Console.WriteLine("\n\nHit ENTER to exit...");
                Console.ReadLine();
            }

            public static async Task MakePredictionRequest(string imageFilePath)
            {
                var client = new HttpClient();

                // Request headers - replace this example key with your valid Prediction-Key.
                client.DefaultRequestHeaders.Add("Prediction-Key", "3b9dde6d1ae1453a86bfeb1d945300f2");

                // Prediction URL - replace this example URL with your valid Prediction URL.
                string url = "https://southcentralus.api.cognitive.microsoft.com/customvision/v3.0/Prediction/8622c779-471c-4b6e-842c-67a11deffd7b/classify/iterations/Cats%20vs.%20Dogs%20-%20Published%20Iteration%203/image";

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

            private static byte[] GetImageAsByteArray(string imageFilePath)
            {
                FileStream fileStream = new FileStream(imageFilePath, FileMode.Open, FileAccess.Read);
                BinaryReader binaryReader = new BinaryReader(fileStream);
                return binaryReader.ReadBytes((int)fileStream.Length);
            }
        }
    }
    ```

## <a name="use-the-application"></a>Verwenden der Anwendung

Wenn die Anwendung ausgeführt wird, geben Sie den Pfad zu einer Bilddatei in die Konsole ein. Das Bild wird an die Vorhersage-API übergeben und die Ergebnisse der Vorhersage werden als JSON-Dokument zurückgegeben. Der folgende JSON-Code ist ein Beispiel für eine Antwort.

```json
{
    "Id":"7796df8e-acbc-45fc-90b4-1b0c81b73639",
    "Project":"8622c779-471c-4b6e-842c-67a11deffd7b",
    "Iteration":"59ec199d-f3fb-443a-b708-4bca79e1b7f7",
    "Created":"2019-03-20T16:47:31.322Z",
    "Predictions":[
        {"TagId":"d9cb3fa5-1ff3-4e98-8d47-2ef42d7fb373","TagName":"cat", "Probability":1.0},
        {"TagId":"9a8d63fb-b6ed-4462-bcff-77ff72084d99","TagName":"dog", "Probability":0.1087869}
    ]
}
```

## <a name="next-steps"></a>Nächste Schritte

[Exportieren Ihres Modells für die Verwendung mit Mobilgeräten](export-your-model.md)

[Erste Schritte mit .NET SDKs](csharp-tutorial.md)

[Erste Schritte mit Python SDKs](python-tutorial.md)

[Erste Schritte mit Java SDKs](java-tutorial.md)

[Erste Schritte mit Node SDKs](node-tutorial.md)

[Erste Schritte mit Go SDKs](go-tutorial.md)
