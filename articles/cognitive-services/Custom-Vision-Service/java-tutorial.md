---
title: 'Tutorial: Erstellen eines Bildklassifizierungsprojekts – Custom Vision Service, Java'
titlesuffix: Azure Cognitive Services
description: Erstellen Sie ein Projekt, fügen Sie Kategorien hinzu, laden Sie Bilder hoch, trainieren Sie Ihr Projekt, und machen Sie eine Vorhersage, die den Standardendpunkt verwendet.
services: cognitive-services
author: areddish
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: tutorial
ms.date: 08/28/2018
ms.author: areddish
ms.openlocfilehash: 9a7f50e0eb33016d6a2d8f28be047b327135c51f
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2018
ms.locfileid: "46367354"
---
# <a name="tutorial-build-an-image-classification-project-with-java"></a>Tutorial: Erstellen eines Bildklassifizierungsprojekts mit Java

Erfahren Sie, wie Sie ein Projekt zur Bildklassifizierung mit dem Custom Vision Service und Java erstellen. Nach der Erstellung können Sie Tags hinzufügen, Bilder hochladen, das Projekt trainieren, die Standardendpunkt-URL des Projekts für Vorhersagen abrufen und das Projekt für das programmgesteuerte Testen eines Bilds verwenden. Verwenden Sie dieses Open-Source-Beispiel als Vorlage zum Erstellen Ihrer eigenen App mithilfe der Custom Vision-API.

## <a name="prerequisites"></a>Voraussetzungen

- JDK 7 oder 8 muss installiert sein.
- Maven muss installiert sein.

## <a name="get-the-training-and-prediction-keys"></a>Abrufen der Trainings- und Vorhersageschlüssel

Besuchen Sie die [Custom Vision-Webseite](https://customvision.ai), und klicken Sie in der oberen rechten Ecke auf das __Zahnradsymbol__, um die in diesem Beispiel verwendeten Schlüssel abzurufen. Kopieren Sie aus dem Abschnitt __Accounts__ (Konten) die Werte der Felder __Training Key__ (Trainingsschlüssel) und __Prediction Key__ (Vorhersageschlüssel).

![Abbildung der Schlüsselbenutzeroberfläche](./media/python-tutorial/training-prediction-keys.png)

## <a name="install-the-custom-vision-service-sdk"></a>Installieren des Custom Vision Service-SDKs

Sie können das Custom Vision SDK aus dem zentralen Maven-Repository installieren:
* [Trainings-SDK](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customvision-training)
* [Vorhersage-SDK](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customvision-prediction)

## <a name="understand-the-code"></a>Grundlegendes zum Code

Das vollständige Projekt einschließlich Bildern ist im [Repository mit Azure-Beispielen für Custom Vision mit Java](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master) verfügbar. 

Verwenden Sie Ihre bevorzugte Java-IDE, um das `Vision/CustomVision`-Projekt zu öffnen. 

Diese Anwendung verwendet den Trainingsschlüssel, den Sie zuvor abgerufen haben, um ein neues Projekt mit dem Namen __Sample Java Project__ zu erstellen. Anschließend werden Bilder zum Trainieren und Testen einer Klassifizierung hochgeladen. Mit der Klassifizierung wird identifiziert, ob es sich bei einem Baum um eine __Hemlocktanne__ oder eine __Japanische Zierkirsche__ handelt.

Mit den folgenden Codeausschnitten wird die primäre Funktionalität dieses Beispiels implementiert:

## <a name="create-a-custom-vision-service-project"></a>Erstellen eines Custom Vision Service-Projekts

> [!IMPORTANT]
> Legen Sie den `trainingApiKey` auf den Wert des Trainingsschlüssels fest, den Sie zuvor abgerufen hatten.

```java
final String trainingApiKey = "insert your training key here";
TrainingApi trainClient = CustomVisionTrainingManager.authenticate(trainingApiKey);

Trainings trainer = trainClient.trainings();

System.out.println("Creating project...");
Project project = trainer.createProject()
            .withName("Sample Java Project")
            .execute();
```

## <a name="add-tags-to-your-project"></a>Hinzufügen von Kategorien zu Ihrem Projekt

```java
// create hemlock tag
Tag hemlockTag = trainer.createTag()
    .withProjectId(project.id())
    .withName("Hemlock")
    .execute();

// create cherry tag
Tag cherryTag = trainer.createTag()
    .withProjectId(project.id())
    .withName("Japanese Cherry")
    .execute();
```

## <a name="upload-images-to-the-project"></a>Hochladen von Bildern ins Projekt

Dieses Beispiel ist so eingerichtet, dass die Bilder im endgültigen Paket enthalten sind. Bilder werden aus dem Ressourcenabschnitt der JAR-Datei gelesen und in den Dienst hochgeladen.

```java
System.out.println("Adding images...");
for (int i = 1; i <= 10; i++) {
    String fileName = "hemlock_" + i + ".jpg";
    byte[] contents = GetImage("/Hemlock", fileName);
    AddImageToProject(trainer, project, fileName, contents, hemlockTag.id(), null);
}

for (int i = 1; i <= 10; i++) {
    String fileName = "japanese_cherry_" + i + ".jpg";
    byte[] contents = GetImage("/Japanese Cherry", fileName);
    AddImageToProject(trainer, project, fileName, contents, cherryTag.id(), null);
}
```

Der vorherige Codeausschnitt nutzt zwei Hilfsfunktionen, die die Bilder als Ressourcenstreams abrufen und in den Dienst hochladen.

```java
private static void AddImageToProject(Trainings trainer, Project project, String fileName, byte[] contents, UUID tag)
{
    System.out.println("Adding image: " + fileName);

    ImageFileCreateEntry file = new ImageFileCreateEntry()
        .withName(fileName)
        .withContents(contents);

    ImageFileCreateBatch batch = new ImageFileCreateBatch()
        .withImages(Collections.singletonList(file));

    batch = batch.withTagIds(Collections.singletonList(tag));

    trainer.createImagesFromFiles(project.id(), batch);
}

private static byte[] GetImage(String folder, String fileName)
{
    try {
        return ByteStreams.toByteArray(CustomVisionSamples.class.getResourceAsStream(folder + "/" + fileName));
    } catch (Exception e) {
        System.out.println(e.getMessage());
        e.printStackTrace();
    }

    return null;
}
```

## <a name="train-the-project"></a>Trainieren des Projekts

Hiermit wird die erste Iteration im Projekt erstellt und als Standarditeration gekennzeichnet. 

```java
System.out.println("Training...");
Iteration iteration = trainer.trainProject(project.id());

while (iteration.status().equals("Training"))
{
    System.out.println("Training Status: "+ iteration.status());
    Thread.sleep(1000);
    iteration = trainer.getIteration(project.id(), iteration.id());
}

System.out.println("Training Status: "+ iteration.status());
trainer.updateIteration(project.id(), iteration.id(), iteration.withIsDefault(true));
```

## <a name="get-and-use-the-default-prediction-endpoint"></a>Abrufen und Verwenden des Standardendpunkts für Vorhersagen

> [!IMPORTANT]
> Legen Sie den `predictionApiKey` auf den Wert des Vorhersageschlüssels fest, den Sie zuvor abgerufen hatten.

```java
final String predictionApiKey = "insert your prediction key here";
PredictionEndpoint predictClient = CustomVisionPredictionManager.authenticate(predictionApiKey);

// Use below for predictions from a url
// String url = "some url";
// ImagePrediction results = predictor.predictions().predictImage()
//                         .withProjectId(project.id())
//                         .withUrl(url)
//                         .execute();

// load test image
byte[] testImage = GetImage("/Test", "test_image.jpg");

// predict
ImagePrediction results = predictor.predictions().predictImage()
    .withProjectId(project.id())
    .withImageData(testImage)
    .execute();

for (Prediction prediction: results.predictions())
{
    System.out.println(String.format("\t%s: %.2f%%", prediction.tagName(), prediction.probability() * 100.0f));
}
```

## <a name="run-the-example"></a>Ausführen des Beispiels

So kompilieren Sie die Lösung mithilfe von Maven und führen sie aus:

```
mvn compile exec:java
```

Die Ausgabe der Anwendung ähnelt dem folgenden Text:

```
Creating project...
Adding images...
Adding image: hemlock_1.jpg
Adding image: hemlock_2.jpg
Adding image: hemlock_3.jpg
Adding image: hemlock_4.jpg
Adding image: hemlock_5.jpg
Adding image: hemlock_6.jpg
Adding image: hemlock_7.jpg
Adding image: hemlock_8.jpg
Adding image: hemlock_9.jpg
Adding image: hemlock_10.jpg
Adding image: japanese_cherry_1.jpg
Adding image: japanese_cherry_2.jpg
Adding image: japanese_cherry_3.jpg
Adding image: japanese_cherry_4.jpg
Adding image: japanese_cherry_5.jpg
Adding image: japanese_cherry_6.jpg
Adding image: japanese_cherry_7.jpg
Adding image: japanese_cherry_8.jpg
Adding image: japanese_cherry_9.jpg
Adding image: japanese_cherry_10.jpg
Training...
Training status: Training
Training status: Training
Training status: Completed
Done!
        Hemlock: 93.53%
        Japanese Cherry: 0.01%
```