---
title: 'C# SDK-Schnellstart: Analysieren von Bildern mit der Maschinelles Sehen-API | Microsoft-Dokumentation'
titleSuffix: Microsoft Cognitive Services
description: In dieser Schnellstartanleitung analysieren Sie ein Bild mithilfe der C#-Clientbibliothek der Maschinelles Sehen-API in Cognitive Services.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: v-deken
ms.openlocfilehash: 3ff3a4702ab0b1fb663ee896f268065caf043809
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2018
ms.locfileid: "43770332"
---
# <a name="quickstart-analyze-an-image---sdk-c35"></a>Schnellstart: Analysieren eines Bilds – SDK und C&#35;

In dieser Schnellstartanleitung analysieren Sie mithilfe der Windows-Clientbibliothek der Maschinelles Sehen-API ein lokales Bild und ein Remotebild, um visuelle Merkmale zu extrahieren.

## <a name="prerequisites"></a>Voraussetzungen

* Zur Verwendung der Maschinelles Sehen-API benötigen Sie einen Abonnementschlüssel. Siehe [Obtaining Subscription Keys](../Vision-API-How-to-Topics/HowToSubscribe.md) (Abrufen von Abonnementschlüsseln).
* Eine beliebige Edition von [Visual Studio 2015 oder 2017](https://www.visualstudio.com/downloads/).
* Das NuGet-Paket [Microsoft.Azure.CognitiveServices.Vision.ComputerVision](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.ComputerVision) mit der Clientbibliothek. Es ist nicht erforderlich, das Paket herunterzuladen. Unten ist die Installationsanleitung angegeben.

## <a name="analyzeimageasync-method"></a>AnalyzeImageAsync-Methode

Die Methoden `AnalyzeImageAsync` und `AnalyzeImageInStreamAsync` umschließen die [Bildanalyse-API](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) für lokale Bilder bzw. Remotebilder. Sie können diese Methoden verwenden, um visuelle Merkmale basierend auf dem Bildinhalt zu extrahieren und die Merkmale auszuwählen, die zurückgegeben werden sollen. Zu den verfügbaren Merkmalen zählen beispielsweise:

* Eine detaillierte Liste der Tags im Zusammenhang mit dem Bildinhalt
* Eine Beschreibung des Bildinhalts in einem vollständigen Satz
* Die Koordinaten, das Geschlecht und das Alter aller im Bild enthaltenen Gesichter
* Der Bildtyp (ClipArt oder Strichzeichnung)
* Die dominante Farbe, die Akzentfarbe und, ob ein Bild schwarzweiß ist
* Die in dieser [Taxonomie](../Category-Taxonomy.md) definierte Kategorie
* Ob das Bild nicht jugendfreie oder freizügige Inhalte enthält

Führen Sie zum Ausführen des Beispiels die folgenden Schritte aus:

1. Erstellen Sie in Visual Studio eine neue Visual C#-Konsolen-App.
1. Installieren Sie das NuGet-Paket mit der Clientbibliothek der Maschinelles Sehen-API.
    1. Klicken Sie im Menü auf **Werkzeuge**, wählen Sie **NuGet-Paket-Manager** und dann **NuGet-Pakete für Projektmappe verwalten** aus.
    1. Klicken Sie auf die Registerkarte **Durchsuchen**, und geben Sie „Microsoft.Azure.CognitiveServices.Vision.ComputerVision“ in das Feld **Suchen** ein.
    1. Wählen Sie **Microsoft.Azure.CognitiveServices.Vision.ComputerVision** aus, wenn das Paket angezeigt wird, aktivieren Sie das Kontrollkästchen neben dem Projektnamen, und klicken Sie dann auf **Installieren**.
1. Ersetzen Sie `Program.cs` durch den folgenden Code.
1. Ersetzen Sie `<Subscription Key>` durch Ihren gültigen Abonnementschlüssel.
1. Ändern Sie `computerVision.AzureRegion = AzureRegions.Westcentralus` ggf. in die Region, von der Sie Ihre Abonnementschlüssel erhalten haben.
1. Ersetzen Sie `<LocalImage>` durch den Pfad und Dateinamen eines lokalen Bilds.
1. Legen Sie optional `remoteImageUrl` auf ein anderes Bild fest.
1. Führen Sie das Programm aus.

```csharp
using Microsoft.Azure.CognitiveServices.Vision.ComputerVision;
using Microsoft.Azure.CognitiveServices.Vision.ComputerVision.Models;

using System;
using System.Collections.Generic;
using System.IO;
using System.Threading.Tasks;

namespace ImageAnalyze
{
    class Program
    {
        // subscriptionKey = "0123456789abcdef0123456789ABCDEF"
        private const string subscriptionKey = "<SubscriptionKey>";

        // localImagePath = @"C:\Documents\LocalImage.jpg"
        private const string localImagePath = @"<LocalImage>";

        private const string remoteImageUrl =
            "http://upload.wikimedia.org/wikipedia/commons/3/3c/Shaki_waterfall.jpg";

        // Specify the features to return
        private static readonly List<VisualFeatureTypes> features =
            new List<VisualFeatureTypes>()
        {
            VisualFeatureTypes.Categories, VisualFeatureTypes.Description,
            VisualFeatureTypes.Faces, VisualFeatureTypes.ImageType,
            VisualFeatureTypes.Tags
        };

        static void Main(string[] args)
        {
            ComputerVisionAPI computerVision = new ComputerVisionAPI(
                new ApiKeyServiceClientCredentials(subscriptionKey), 
                new System.Net.Http.DelegatingHandler[] { });

            // You must use the same region as you used to get your subscription
            // keys. For example, if you got your subscription keys from westus,
            // replace "Westcentralus" with "Westus".
            //
            // Free trial subscription keys are generated in the westcentralus
            // region. If you use a free trial subscription key, you shouldn't
            // need to change the region.

            // Specify the Azure region
            computerVision.AzureRegion = AzureRegions.Westcentralus;

            Console.WriteLine("Images being analyzed ...");
            var t1 = AnalyzeRemoteAsync(computerVision, remoteImageUrl);
            var t2 = AnalyzeLocalAsync(computerVision, localImagePath);

            Task.WhenAll(t1, t2).Wait(5000);
            Console.WriteLine("Press any key to exit");
            Console.ReadLine();
        }

        // Analyze a remote image
        private static async Task AnalyzeRemoteAsync(
            ComputerVisionAPI computerVision, string imageUrl)
        {
            if (!Uri.IsWellFormedUriString(imageUrl, UriKind.Absolute))
            {
                Console.WriteLine(
                    "\nInvalid remoteImageUrl:\n{0} \n", imageUrl);
                return;
            }

            ImageAnalysis analysis =
                await computerVision.AnalyzeImageAsync(imageUrl, features);
            DisplayResults(analysis, imageUrl);
        }

        // Analyze a local image
        private static async Task AnalyzeLocalAsync(
            ComputerVisionAPI computerVision, string imagePath)
        {
            if (!File.Exists(imagePath))
            {
                Console.WriteLine(
                    "\nUnable to open or read localImagePath:\n{0} \n", imagePath);
                return;
            }

            using (Stream imageStream = File.OpenRead(imagePath))
            {
                ImageAnalysis analysis = await computerVision.AnalyzeImageInStreamAsync(
                    imageStream, features);
                DisplayResults(analysis, imagePath);
            }
        }

        // Display the most relevant caption for the image
        private static void DisplayResults(ImageAnalysis analysis, string imageUri)
        {
            Console.WriteLine(imageUri);
            Console.WriteLine(analysis.Description.Captions[0].Text + "\n");
        }
    }
}
```

## <a name="analyzeimageasync-response"></a>AnalyzeImageAsync-Antwort

In einer erfolgreichen Antwort werden die relevantesten Titel für jedes Bild angezeigt.

Ein Beispiel für die unformatierte JSON-Ausgabe finden Sie unter [API Quickstarts: Analyze a local image with C#](../QuickStarts/CSharp-analyze.md#analyze-image-response) (API-Schnellstarts: Analysieren von lokalen Bildern mit C#).

```cmd
http://upload.wikimedia.org/wikipedia/commons/3/3c/Shaki_waterfall.jpg
a large waterfall over a rocky cliff
```

## <a name="next-steps"></a>Nächste Schritte

Erkunden Sie die Maschinelles Sehen-APIs, die zum Analysieren von Bildern, Erkennen von Prominenten und Sehenswürdigkeiten, Erstellen von Miniaturansichten und Extrahieren von gedrucktem sowie handschriftlichem Text verwendet werden.

> [!div class="nextstepaction"]
> [Maschinelles Sehen-APIs erkunden](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
