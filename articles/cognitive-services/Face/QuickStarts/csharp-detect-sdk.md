---
title: 'Schnellstart: Erkennen von Gesichtern in einem Bild – SDK, C#'
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung führen Sie die Gesichtserkennung für ein Bild durch, indem Sie die Windows-C#-Clientbibliothek für die Gesichtserkennung in Cognitive Services verwenden.
services: cognitive-services
author: noellelacharite
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: quickstart
ms.date: 09/14/2018
ms.author: nolachar
ms.openlocfilehash: a9a7efd89f8e7462812064615d07acf12acbc3a3
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2018
ms.locfileid: "46364109"
---
# <a name="quickstart-detect-faces-in-an-image-using-c35---face"></a>Schnellstart: Erkennen von Gesichtern in einem Bild mit C&#35; – Gesichtserkennung

In dieser Schnellstartanleitung führen Sie eine Erkennung menschlicher Gesichter in einem Bild mithilfe der Windows-Clientbibliothek für die Gesichtserkennung durch.

Den Quellcode des Beispiels finden Sie auf [GitHub](https://github.com/Azure-Samples/cognitive-services-vision-csharp-sdk-quickstarts/tree/master/Face).

## <a name="prerequisites"></a>Voraussetzungen

* Zum Ausführen des Beispiels benötigen Sie einen Abonnementschlüssel. Über die Seite [Cognitive Services ausprobieren](https://azure.microsoft.com/try/cognitive-services/?api=face-api) können Sie Abonnementschlüssel für eine kostenlose Testversion abrufen.
* Eine beliebige [Visual Studio 2017](https://www.visualstudio.com/downloads/)-Edition.
* Das NuGet-Paket [Microsoft.Azure.CognitiveServices.Vision.Face 2.2.0-preview](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.Face/2.2.0-preview) mit der Clientbibliothek. Es ist nicht erforderlich, das Paket herunterzuladen. Unten ist die Installationsanleitung angegeben.

## <a name="detectwithurlasync-method"></a>DetectWithUrlAsync-Methode

Die Methoden `DetectWithUrlAsync` und `DetectWithStreamAsync` umschließen die [Face - Detect-API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) für lokale Bilder bzw. Remotebilder. Sie können diese Methoden verwenden, um Gesichter in einem Bild zu erkennen und Gesichtsattribute zurückzugeben, z.B.:

* Gesichtserkennungs-ID: Eindeutige ID, die in verschiedenen Gesichtserkennungs-API-Szenarien verwendet wird.
* Gesichtsrechteck: Die Position des linken und oberen Rands sowie die Breite und Höhe, um die Position des Gesichts im Bild anzugeben.
* Besondere Merkmale: Ein Array mit 27 Punkten zu Gesichtszügen, die auf die wichtigen Positionen von Gesichtskomponenten hinweisen.
* Dies können Gesichtsattribute wie Alter, Geschlecht, Intensität des Lächelns, Kopfhaltung und Gesichtsbehaarung sein.

Führen Sie zum Ausführen des Beispiels die folgenden Schritte aus:

1. Erstellen Sie in Visual Studio eine neue Visual C#-Konsolen-App.
1. Installieren Sie das NuGet-Paket mit der Clientbibliothek für die Gesichtserkennung.
    1. Klicken Sie im oberen Menü auf **Tools**, wählen Sie **NuGet-Paket-Manager** und dann **NuGet-Pakete für Projektmappe verwalten** aus.
    1. Klicken Sie auf die Registerkarte **Durchsuchen**, und wählen Sie **Vorabversion einbeziehen** aus.
    1. Geben Sie im Feld **Suchen** die Zeichenfolge „Microsoft.Azure.CognitiveServices.Vision.Face“ ein.
    1. Wählen Sie **Microsoft.Azure.CognitiveServices.Vision.Face** aus, wenn das Paket angezeigt wird. Aktivieren Sie das Kontrollkästchen neben dem Namen Ihres Projekts, und klicken Sie dann auf **Installieren**.
1. Ersetzen Sie *Program.cs* durch den folgenden Code.
1. Ersetzen Sie `<Subscription Key>` durch Ihren gültigen Abonnementschlüssel.
1. Ändern Sie `faceEndpoint` bei Bedarf in die Azure-Region, die Ihren Abonnementschlüsseln zugeordnet ist, wenn erforderlich.
1. Ersetzen Sie optional <`LocalImage>` durch den Pfad und Dateinamen eines lokalen Bilds (wird ignoriert, wenn nichts festgelegt wird).
1. Legen Sie optional `remoteImageUrl` auf ein anderes Bild fest.
1. Führen Sie das Programm aus.

```csharp
using Microsoft.Azure.CognitiveServices.Vision.Face;
using Microsoft.Azure.CognitiveServices.Vision.Face.Models;

using System;
using System.Collections.Generic;
using System.IO;
using System.Threading.Tasks;

namespace DetectFace
{
    class Program
    {
        // subscriptionKey = "0123456789abcdef0123456789ABCDEF"
        private const string subscriptionKey = "<SubscriptionKey>";

        // You must use the same region as you used to get your subscription
        // keys. For example, if you got your subscription keys from westus,
        // replace "westcentralus" with "westus".
        //
        // Free trial subscription keys are generated in the westcentralus
        // region. If you use a free trial subscription key, you shouldn't
        // need to change the region.
        // Specify the Azure region
        private const string faceEndpoint =
            "https://westcentralus.api.cognitive.microsoft.com";

        // localImagePath = @"C:\Documents\LocalImage.jpg"
        private const string localImagePath = @"<LocalImage>";

        private const string remoteImageUrl =
            "https://upload.wikimedia.org/wikipedia/commons/3/37/Dagestani_man_and_woman.jpg";

        private static readonly FaceAttributeType[] faceAttributes =
            { FaceAttributeType.Age, FaceAttributeType.Gender };

        static void Main(string[] args)
        {
            FaceClient faceClient = new FaceClient(
                new ApiKeyServiceClientCredentials(subscriptionKey),
                new System.Net.Http.DelegatingHandler[] { });
            faceClient.Endpoint = faceEndpoint;

            Console.WriteLine("Faces being detected ...");
            var t1 = DetectRemoteAsync(faceClient, remoteImageUrl);
            var t2 = DetectLocalAsync(faceClient, localImagePath);

            Task.WhenAll(t1, t2).Wait(5000);
            Console.WriteLine("Press any key to exit");
            Console.ReadLine();
        }

        // Detect faces in a remote image
        private static async Task DetectRemoteAsync(
            FaceClient faceClient, string imageUrl)
        {
            if (!Uri.IsWellFormedUriString(imageUrl, UriKind.Absolute))
            {
                Console.WriteLine("\nInvalid remoteImageUrl:\n{0} \n", imageUrl);
                return;
            }

            try
            {
                IList<DetectedFace> faceList =
                    await faceClient.Face.DetectWithUrlAsync(
                        imageUrl, true, false, faceAttributes);

                DisplayAttributes(GetFaceAttributes(faceList, imageUrl), imageUrl);
            }
            catch (APIErrorException e)
            {
                Console.WriteLine(imageUrl + ": " + e.Message);
            }
        }

        // Detect faces in a local image
        private static async Task DetectLocalAsync(FaceClient faceClient, string imagePath)
        {
            if (!File.Exists(imagePath))
            {
                Console.WriteLine(
                    "\nUnable to open or read localImagePath:\n{0} \n", imagePath);
                return;
            }

            try
            {
                using (Stream imageStream = File.OpenRead(imagePath))
                {
                    IList<DetectedFace> faceList =
                            await faceClient.Face.DetectWithStreamAsync(
                                imageStream, true, false, faceAttributes);
                    DisplayAttributes(
                        GetFaceAttributes(faceList, imagePath), imagePath);
                }
            }
            catch (APIErrorException e)
            {
                Console.WriteLine(imagePath + ": " + e.Message);
            }
        }

        private static string GetFaceAttributes(
            IList<DetectedFace> faceList, string imagePath)
        {
            string attributes = string.Empty;

            foreach (DetectedFace face in faceList)
            {
                double? age = face.FaceAttributes.Age;
                string gender = face.FaceAttributes.Gender.ToString();
                attributes += gender + " " + age + "   ";
            }

            return attributes;
        }

        // Display the face attributes
        private static void DisplayAttributes(string attributes, string imageUri)
        {
            Console.WriteLine(imageUri);
            Console.WriteLine(attributes + "\n");
        }
    }
}
```

### <a name="detectwithurlasync-response"></a>DetectWithUrlAsync-Antwort

Eine erfolgreiche Antwort zeigt das Geschlecht und Alter für jedes Gesicht im Bild an.

Ein Beispiel für die unformatierte JSON-Ausgabe finden Sie unter [API-Schnellstarts: Analysieren von lokalen Bildern mit C#](CSharp.md).

```
https://upload.wikimedia.org/wikipedia/commons/3/37/Dagestani_man_and_woman.jpg
Male 37   Female 56
```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie eine WPF-Anwendung erstellen, die mithilfe des Gesichtserkennungsdiensts Gesichter in Bildern erkennt. Die Anwendung zeichnet einen Rahmen um jedes Gesicht und zeigt eine Beschreibung des Gesichts in der Statusleiste an.

> [!div class="nextstepaction"]
> [Tutorial: Erstellen einer WPF-App zum Erkennen und Umranden von Gesichtern in einem Bild](../Tutorials/FaceAPIinCSharpTutorial.md)
