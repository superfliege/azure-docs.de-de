---
title: 'Schnellstart: Erstellen eines Bildklassifizierungsprojekts mit dem Custom Vision SDK für C#'
titlesuffix: Azure Cognitive Services
description: Erstellen Sie ein Projekt, fügen Sie Tags hinzu, laden Sie Bilder hoch, trainieren Sie Ihr Projekt, und machen Sie eine Vorhersage unter Verwendung des .NET SDK mit C#.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: quickstart
ms.date: 10/31/2018
ms.author: anroth
ms.openlocfilehash: aaffd5e47c8465d44c2dbcdde3f40cf6e6995e55
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2019
ms.locfileid: "54079084"
---
# <a name="quickstart-create-an-image-classification-project-with-the-custom-vision-net-sdk"></a>Schnellstart: Erstellen eines Bildklassifizierungsprojekts mit dem Custom Vision SDK für .NET

Dieser Artikel enthält Informationen und Beispielcode für die ersten Schritte mit dem Custom Vision SDK mit C# und unterstützt Sie beim Erstellen eines Bildklassifizierungsmodells. Nach der Erstellung können Sie Tags hinzufügen, Bilder hochladen, das Projekt trainieren, die Standardendpunkt-URL für Vorhersagen des Projekts abrufen und den Endpunkt für die programmgesteuerte Überprüfung eines Bilds verwenden. Verwenden Sie dieses Beispiel als Vorlage für die Erstellung Ihrer eigenen .NET-Anwendung. Falls Sie den Prozess zur Erstellung und Verwendung eines Klassifizierungsmodells _ohne_ Code durchlaufen möchten, sehen Sie sich stattdessen die [browserbasierte Anleitung](getting-started-build-a-classifier.md) an.

## <a name="prerequisites"></a>Voraussetzungen
- Eine beliebige Edition von [Visual Studio 2015 oder 2017](https://www.visualstudio.com/downloads/)


## <a name="get-the-custom-vision-sdk-and-sample-code"></a>Abrufen von Custom Vision SDK und Beispielcode
Wenn Sie eine .NET-App schreiben möchten, die Custom Vision verwendet, benötigen Sie die NuGet-Pakete für Custom Vision. Diese sind in dem Beispielprojekt enthalten, das Sie herunterladen. Sie können hier aber auch einzeln auf sie zugreifen:

* [Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training/)
* [Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction/)

Klonen Sie das Projekt [Cognitive Services .NET Samples](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples) (Cognitive Services-Beispiele für .NET), oder laden Sie es herunter. Navigieren Sie zum Ordner **CustomVision/ImageClassification**, und öffnen Sie _ImageClassification.csproj_ in Visual Studio.

Dieses Visual Studio-Projekt erstellt ein neues Custom Vision-Projekt namens __My New Project__, auf das über die [Custom Vision-Website](https://customvision.ai/) zugegriffen werden kann. Anschließend werden Bilder zum Trainieren und Testen einer Klassifizierung hochgeladen. In diesem Projekt soll die Klassifizierung bestimmen, ob es sich bei einem Baum um eine __Hemlocktanne__ oder um eine __japanische Zierkirsche__ handelt.

[!INCLUDE [get-keys](includes/get-keys.md)]

## <a name="understand-the-code"></a>Grundlegendes zum Code

Öffnen Sie die Datei _Program.cs_, und sehen Sie sich den Code an. Fügen Sie Ihre Abonnementschlüssel in die entsprechenden Definitionen in der Methode **Main** ein.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ImageClassification/Program.cs?range=21-30)]

Der Endpunktparameter muss auf die Region verweisen, in der die Azure-Ressourcengruppe mit den Custom Vision-Ressourcen erstellt wurde. Für dieses Beispiel ist das die Region „USA, Süden-Mitte“, und wir verwenden Folgendes:

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ImageClassification/Program.cs?range=14-14)]

Die folgenden Codezeilen führen die primäre Funktionalität des Projekts aus.

### <a name="create-a-new-custom-vision-service-project"></a>Erstellen eines neuen Custom Vision Service-Projekts

Das erstellte Projekt wird auf der [Custom Vision-Website](https://customvision.ai/) angezeigt, die Sie zuvor besucht haben. 

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ImageClassification/Program.cs?range=32-34)]

### <a name="create-tags-in-the-project"></a>Erstellen von Tags im Projekt

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ImageClassification/Program.cs?range=36-38)]

### <a name="upload-and-tag-images"></a>Hochladen und Kennzeichnen von Bildern

Die Bilder für dieses Projekt sind bereits enthalten. Auf sie wird in _Program.cs_ in der Methode **LoadImagesFromDisk** verwiesen.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ImageClassification/Program.cs?range=40-55)]

### <a name="train-the-classifier"></a>Trainieren des Klassifizierers

Dieser Code erstellt die erste Iteration im Projekt und kennzeichnet sie als Standarditeration. Die Standarditeration spiegelt die Version des Modells wider, die auf Vorhersageanforderungen reagiert. Sie sollte jedes Mal aktualisiert werden, wenn Sie das Modell neu trainieren.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ImageClassification/Program.cs?range=57-73)]

### <a name="set-the-prediction-endpoint"></a>Festlegen des Vorhersageendpunkts

Der Vorhersageendpunkt ist die Referenz, die Sie verwenden, um ein Bild an das aktuelle Modell zu übermitteln und eine Klassifizierungsvorhersage zu erhalten.
 
[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ImageClassification/Program.cs?range=77-82)]
 
### <a name="submit-an-image-to-the-default-prediction-endpoint"></a>Übermitteln eines Bilds an den Standardendpunkt für Vorhersagen

In diesem Skript wird das Testbild in der Methode **LoadImagesFromDisk** geladen, und die Vorhersage des Modells soll in der Konsole ausgegeben werden.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ImageClassification/Program.cs?range=84-92)]

## <a name="run-the-application"></a>Ausführen der Anwendung

Während der Anwendungsausführung sollte ein Konsolenfenster mit folgender Ausgabe geöffnet werden:

```
Creating new project:
        Uploading images
        Training
Done!

Making a prediction:
        Hemlock: 95.0%
        Japanese Cherry: 0.0%
```

Daraufhin können Sie sich vergewissern, dass das Testbild (unter **Images/Test/**) ordnungsgemäß gekennzeichnet ist. Drücken Sie eine beliebige Taste, um die Anwendung zu beenden. Sie können auch zur [Custom Vision-Website](https://customvision.ai) zurückgehen und den aktuellen Status Ihres neu erstellten Projekts ansehen.

[!INCLUDE [clean-ic-project](includes/clean-ic-project.md)]

## <a name="next-steps"></a>Nächste Schritte

Sie wissen nun, wie die einzelnen Schritte des Bildklassifizierungsprozesses im Code ausgeführt werden. In diesem Beispiel wird eine einzelne Trainingsiteration ausgeführt. Zur Verbesserung der Genauigkeit muss ein Modell jedoch häufig mehrmals trainiert und getestet werden.

> [!div class="nextstepaction"]
> [Testen und erneutes Trainieren eines Modells mit Custom Vision Service](test-your-model.md)