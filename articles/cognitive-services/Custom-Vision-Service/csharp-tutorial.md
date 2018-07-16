---
title: Verwenden des Custom Vision Service über eine C#-Anwendung – Azure Cognitive Services | Microsoft-Dokumentation
description: Lernen Sie eine einfache C#-App kennen, die die Custom Vision-API in Microsoft Cognitive Services verwendet. Erstellen Sie ein Projekt, fügen Sie Tags hinzu, laden Sie Bilder hoch, und erstellen Sie eine Vorhersage mit dem Standardendpunkt.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 05/03/2018
ms.author: anroth
ms.openlocfilehash: 80cb022808748ed2c60dff7c363d6020cb4043a8
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35374722"
---
# <a name="use-the-custom-vision-service-from-a-c35-application"></a>Verwenden des Custom Vision Service über eine C&#35;-Anwendung

Es wird beschrieben, wie Sie den Custom Vision Service über eine C#-Anwendung verwenden. Nach der Erstellung können Sie Tags hinzufügen, Bilder hochladen, das Projekt trainieren, die Standardendpunkt-URL für Vorhersagen des Projekts abrufen und den Endpunkt für die programmgesteuerte Überprüfung eines Bilds verwenden. Verwenden Sie dieses Open-Source-Beispiel als Vorlage zum Erstellen Ihrer eigenen App für Windows, indem Sie die Custom Vision Service-API verwenden.

## <a name="prerequisites"></a>Voraussetzungen

* Eine beliebige Edition von Visual Studio 2015 oder 2017 für Windows.

* [Custom Vision Service SDK](http://github.com/Microsoft/Cognitive-CustomVision-Windows/). Enthält das in diesem Dokument verwendete Beispiel und die entsprechenden Bilder.

## <a name="get-the-training-and-prediction-keys"></a>Abrufen der Trainings- und Vorhersageschlüssel

Besuchen Sie die [Custom Vision-Webseite](https://customvision.ai), und klicken Sie in der oberen rechten Ecke auf das __Zahnradsymbol__, um die in diesem Beispiel verwendeten Schlüssel abzurufen. Kopieren Sie aus dem Abschnitt __Accounts__ (Konten) die Werte der Felder __Training Key__ (Trainingsschlüssel) und __Prediction Key__ (Vorhersageschlüssel).

![Abbildung der Schlüsselbenutzeroberfläche](./media/csharp-tutorial/training-prediction-keys.png)

## <a name="understand-the-code"></a>Grundlegendes zum Code

Öffnen Sie in Visual Studio das Projekt, das sich im Verzeichnis `Samples/CustomVision.Sample/` des SDK-Projekts befindet.

Diese Anwendung nutzt den Trainingsschlüssel, den Sie zuvor abgerufen haben, um ein neues Projekt mit dem Namen __My New Project__ (Mein neues Projekt) zu erstellen. Anschließend werden Bilder zum Trainieren und Testen einer Klassifizierung hochgeladen. Mit der Klassifizierung wird identifiziert, ob es sich bei einem Baum um eine __Hemlocktanne__ oder eine __Japanische Zierkirsche__ handelt.

Mit den folgenden Codeausschnitten wird die primäre Funktionalität dieses Beispiels implementiert:

* __Erstellen eines neuen Custom Vision Service-Projekts__:

    ```csharp
     // Create a new project
    Console.WriteLine("Creating new project:");
    var project = trainingApi.CreateProject("My New Project");
    ```

* __Erstellen von Tags in einem Projekt__:

    ```csharp
    // Make two tags in the new project
    var hemlockTag = trainingApi.CreateTag(project.Id, "Hemlock");
    var japaneseCherryTag = trainingApi.CreateTag(project.Id, "Japanese Cherry");
    ```

* __Hochladen und Kennzeichnen von Bildern__:

    ```csharp
    // Add some images to the tags
    Console.WriteLine("\tUploading images");
    LoadImagesFromDisk();

    // Images can be uploaded one at a time
    foreach (var image in hemlockImages)
    {
        using (var stream = new MemoryStream(File.ReadAllBytes(image)))
        {
            trainingApi.CreateImagesFromData(project.Id, stream, new List<string>() { hemlockTag.Id.ToString() });
        }
    }

    // Or uploaded in a single batch 
    var imageFiles = japaneseCherryImages.Select(img => new ImageFileCreateEntry(Path.GetFileName(img), File.ReadAllBytes(img))).ToList();
    trainingApi.CreateImagesFromFiles(project.Id, new ImageFileCreateBatch(imageFiles, new List<Guid>() { japaneseCherryTag.Id }));
    ```

* __Trainieren der Klassifizierung__:

    ```csharp
    // Now there are images with tags start training the project
    Console.WriteLine("\tTraining");
    var iteration = trainingApi.TrainProject(project.Id);

    // The returned iteration will be in progress, and can be queried periodically to see when it has completed
    while (iteration.Status == "Completed")
    {
        Thread.Sleep(1000);

        // Re-query the iteration to get it's updated status
        iteration = trainingApi.GetIteration(project.Id, iteration.Id);
    }
    ```

* __Festlegen einer Standarditeration für den Vorhersageendpunkt__:

    ```csharp
    // The iteration is now trained. Make it the default project endpoint
    iteration.IsDefault = true;
    trainingApi.UpdateIteration(project.Id, iteration.Id, iteration);
    Console.WriteLine("Done!\n");
    ```

* __Erstellen eines Vorhersageendpunkts__:
 
    ```csharp
    // Create a prediction endpoint, passing in obtained prediction key
    PredictionEndpoint endpoint = new PredictionEndpoint() { ApiKey = predictionKey };
    ```
 
* __Senden eines Bilds an den Vorhersageendpunkt__:

    ```csharp
    // Make a prediction against the new project
    Console.WriteLine("Making a prediction:");
    var result = endpoint.PredictImage(project.Id, testImage);

    // Loop over each prediction and write out the results
    foreach (var c in result.Predictions)
    {
        Console.WriteLine($"\t{c.TagName}: {c.Probability:P1}");
    }
    ```

## <a name="run-the-application"></a>Ausführen der Anwendung

1. Nehmen Sie die folgenden Änderungen vor, um die Trainings- und Vorhersageschlüssel der Anwendung hinzuzufügen:

    * Fügen Sie der folgenden Zeile Ihren __Trainingsschlüssel__ hinzu:

        ```csharp
        string trainingKey = "<your key here>";
        ```

    * Fügen Sie der folgenden Zeile Ihren __Vorhersageschlüssel__ hinzu:

        ```csharp
        string predictionKey = "<your key here>";
        ```

2. Führen Sie die Anwendung aus. Wenn die Anwendung ausgeführt wird, wird die folgende Ausgabe in die Konsole geschrieben:

    ```
    Creating new project:
            Uploading images
            Training
    Done!

    Making a prediction:
            Hemlock: 95.0%
            Japanese Cherry: 0.0%
    ```

3. Drücken Sie eine beliebige Taste, um die Anwendung zu beenden.
