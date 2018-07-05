---
title: Custom Vision-ONNX-Modell mit Windows ML – Cognitive Services | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie eine Windows-UWP-App erstellen, für die ein aus Cognitive Services exportiertes ONNX-Modell verwendet wird.
services: cognitive-services
author: larryfr
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: conceptual
ms.date: 06/19/2018
ms.author: larryfr
ms.openlocfilehash: 0b128ba1800e74c20c09a9c5711c8473f1dd00d0
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/25/2018
ms.locfileid: "36939432"
---
# <a name="tutorial-use-an-onnx-model-from-custom-vision-with-windows-ml-preview"></a>Tutorial: Verwenden eines ONNX-Modells von Custom Vision mit Windows ML (Vorschauversion)

Es wird beschrieben, wie Sie ein aus Custom Vision exportiertes ONNX-Modell mit Windows ML (Vorschauversion) verwenden.

Anhand der Informationen in diesem Dokument wird veranschaulicht, wie Sie eine ONNX-Datei, die aus Custom Vision Service exportiert wurde, mit Windows ML nutzen. Es wird auch eine Windows-UWP-Beispielanwendung verwendet. Das Beispiel enthält ein trainiertes Modell, mit dem Hunde und Katzen erkannt werden können. Außerdem sind die Schritte angegeben, wie Sie Ihr eigenes Modell mit diesem Beispiel verwenden können.

> [!div class="checklist"]
> * Informationen zur Beispiel-App
> * Abrufen des Beispielcodes
> * Ausführen des Beispiels
> * Verwenden eines eigenen Modells

## <a name="prerequisites"></a>Voraussetzungen

* Windows 10-Gerät mit:

    * Kamera

    * Visual Studio 2017 Version 15.7 oder höher mit aktivierter Workload __Entwicklung für die universelle Windows-Plattform__.

    * Aktivierter Entwicklermodus. Weitere Informationen finden Sie im Dokument [Aktivieren Ihres Geräts für die Entwicklung](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development).

* (Optional) Eine aus Custom Vision Service exportierte ONNX-Datei. Weitere Informationen finden Sie im Dokument [Exportieren Ihres Modells für die Verwendung mit Mobilgeräten](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/export-your-model).

    > [!NOTE]
    > Führen Sie die Schritte im Abschnitt [Verwenden eines eigenen Modells](#use-your-own-model) aus, wenn Sie ein eigenes Modell verwenden möchten.

## <a name="about-the-example-app"></a>Informationen zur Beispiel-App

Die Anwendung ist eine generische Windows-UWP-Anwendung. Sie nutzt die Kamera Ihres Windows 10-Geräts, um die Bilder für das Modell zu liefern. Die vom Modell zurückgegebenen Tags und Bewertungen werden unter dem Vorschaubild des Videos angezeigt.

* Wenn die Daten über die Kamera erfasst werden, wird [MediaFrameReader](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader) genutzt, um einzelne Frames zu extrahieren. Die Frames werden zur Bewertung an das Modell gesendet.

* Das Modell gibt die Tags, die zum Trainieren verwendet wurden, und einen Gleitkommawert zurück. Mit diesem Wert wird angegeben, wie hoch die Wahrscheinlichkeit ist, dass das Element im Bild enthalten ist.

### <a name="the-ui"></a>Benutzeroberfläche

Die Benutzeroberfläche für die Beispielanwendung wird mit den Steuerelementen __CaptureElement__ und __TextBlock__ erstellt. Mit CaptureElement wird für die Videodaten der Kamera eine Vorschau angezeigt, und mit TextBlock werden die Ergebnisse angezeigt, die vom Modell zurückgegeben werden. 

### <a name="the-model"></a>Modell

Das für das Beispiel bereitgestellte Modell (`cat-or-dog.onnx`) wurde per Custom Vision Service von Cognitive Services erstellt und trainiert. Anschließend wurde das trainierte Modell als ONNX-Modell exportiert. Weitere Informationen zur Verwendung dieses Diensts finden Sie in den Dokumenten [How to build a classifier with Custom Vision](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/getting-started-build-a-classifier) (Erstellen eines Klassifizierers mit Custom Vision) und [Exportieren Ihres Modells für die Verwendung mit Mobilgeräten](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/export-your-model).

> [!IMPORTANT]
> Das für das Beispiel bereitgestellte Modell wurde nur mit einer kleinen Menge an Hunde- und Katzenbildern trainiert. Es ist also nicht das weltweit beste Modell zur Erkennung von Hunden und Katzen.

### <a name="the-model-class-file"></a>Modellklassendatei

Wenn Sie eine ONNX-Datei einer Windows-UWP-Anwendung hinzufügen, wird eine CS-Datei erstellt. Diese Datei hat den gleichen Namen wie die `.onnx`-Datei (hier: `cat-or-dog`) und enthält die Klassen, die für die Nutzung des Modells per C# verwendet werden. Die Entitäten in der generierten Klasse können aber Namen wie `_x0033_04aa07b_x002D_6c8c_x002D_4641_x002D_93a6_x002D_f3152f8740a1_028da4e3_x002D_9c6e_x002D_480b_x002D_b53c_x002D_c1db13d24d70ModelInput` haben. Sie können diese Einträge ohne Weiteres in einen benutzerfreundlichen Anzeigenamen umbenennen (Rechte Maustaste > Umbenennen).

> [!NOTE]
> Im Beispielcode wurden die generierten Klassen- und Methodennamen wie folgt umgestaltet:
>
> * `ModelInput`
> * `ModelOutput`
> * `Model`
> * `CreateModel`

### <a name="camera-access"></a>Kamerazugriff

Die Registerkarte __Capabilities__ (Funktionen) in der Datei `Package.appxmanifest` ist so konfiguriert, dass der Zugriff auf die Webcam und das Mikrofon ermöglicht werden kann.

> [!NOTE]
> In diesem Beispiel werden zwar keine Audiodaten verwendet, aber das Mikrofon musste aktiviert werden, bevor auf dem Gerät die Kamera aktiviert werden konnte.

Die Anwendung versucht, die Kamera auf der Rückseite Ihres Geräts zu nutzen, falls dort eine verfügbar ist. Sie verwendet die [MediaCapture](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture)-Klasse, um mit dem Erfassen der Videodaten der Kamera zu beginnen. [MediaFrameReader](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader) wird verwendet, um Videoframes zu erfassen und an das Modell zu senden.

## <a name="get-the-example-code"></a>Abrufen des Beispielcodes

Die Beispielanwendung finden Sie unter [https://github.com/Azure-Samples/Custom-Vision-ONNX-UWP](https://github.com/Azure-Samples/Custom-Vision-ONNX-UWP).

## <a name="run-the-example"></a>Ausführen des Beispiels

1. Verwenden Sie die Taste `F5`, um die Anwendung in Visual Studio zu starten. Unter Umständen werden Sie aufgefordert, den Entwicklermodus zu aktivieren. Weitere Informationen finden Sie im Dokument [Aktivieren Ihres Geräts für die Entwicklung](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development).

2. Lassen Sie für die Anwendung den Zugriff auf die Kamera und das Mikrofon Ihres Geräts zu, wenn die entsprechende Aufforderung angezeigt wird.

3. Richten Sie die Kamera auf einen Hund oder eine Katze aus. Die Bewertung dafür, ob das Bild einen Hund oder eine Katze enthält, wird unter dem Vorschaubild in der Anwendung angezeigt.

    > [!TIP]
    > Falls Sie keinen Hund bzw. keine Katze besitzen, können Sie ein Foto eines Hunds oder einer Katze verwenden.

## <a name="use-your-own-model"></a>Verwenden eines eigenen Modells

Führen Sie die folgenden Schritte aus, um Ihr eigenes Modell zu verwenden:

> [!IMPORTANT]
> Mit den Schritten in diesem Abschnitt wird das aktuelle Modell (cat-or-dog.cs) umbenannt, und die Klassen- und Methodennamen werden für das neue Modell umgestaltet. Auf diese Weise sollen Namenskonflikte mit dem Beispielmodell vermieden werden.

1. Trainieren Sie ein Modell per Custom Vision Service. Informationen zum Trainieren eines Modells finden Sie unter [How to build a classifier with Custom Vision](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/getting-started-build-a-classifier) (Erstellen eines Klassifizierers mit Custom Vision).

2. Exportieren Sie das trainierte Modell als ONNX-Modell. Informationen zum Exportieren eines Modells finden Sie im Dokument [Exportieren Ihres Modells für die Verwendung mit Mobilgeräten](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/export-your-model).

3. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf die Datei __cat-or-dog.cs__, und benennen Sie sie in __cat-or-dog.txt__ um. Durch das Umbenennen werden Namenskonflikte mit dem neuen Modell verhindert.

    > [!TIP]
    > Sie können auch andere Namen für die Klassennamen im neuen Modell nutzen, aber die Wiederverwendung der vorhandenen Namen ist einfacher.

4. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Eintrag __VisionApp__, und wählen Sie dann __Hinzufügen__ > __Vorhandenes Element...__.

5. Wählen Sie zum Generieren einer Klasse für das Modell die zu importierende ONNX-Datei aus, und wählen Sie anschließend die Schaltfläche __Hinzufügen__. Eine neue Klasse mit dem gleichen Namen wie die ONNX-Datei (aber mit der Erweiterung `.cs`) wird im Projektmappen-Explorer hinzugefügt.

6. Öffnen Sie die generierte CS-Datei, und suchen Sie nach den Namen der folgenden Elemente:

    > [!IMPORTANT]
    > Verwenden Sie die Beispieldatei `cat-or-dog.txt` als Anleitung zum Erkennen der Klassen und Funktionen.

    * Die Klasse, mit der die Modelleingabe definiert wird. Der generierte Name kann beispielsweise `_x0033_04aa07b_x002D_6c8c_x002D_4641_x002D_93a6_x002D_f3152f8740a1_028da4e3_x002D_9c6e_x002D_480b_x002D_b53c_x002D_c1db13d24d70ModelInput` lauten. Benennen Sie diese Klasse in __ModelInput__ um.
    * Die Klasse, mit der die Modellausgabe definiert wird. Der generierte Name kann beispielsweise `_x0033_04aa07b_x002D_6c8c_x002D_4641_x002D_93a6_x002D_f3152f8740a1_028da4e3_x002D_9c6e_x002D_480b_x002D_b53c_x002D_c1db13d24d70ModelOutput` lauten. Benennen Sie diese Klasse in __ModelOutput__ um.
    * Die Klasse, mit der das Modell definiert wird. Der generierte Name kann beispielsweise `_x0033_04aa07b_x002D_6c8c_x002D_4641_x002D_93a6_x002D_f3152f8740a1_028da4e3_x002D_9c6e_x002D_480b_x002D_b53c_x002D_c1db13d24d70Model` lauten. Benennen Sie diese Klasse in __Model__ um.
    * Die Methode, mit der das Modell erstellt wird. Der generierte Name kann beispielsweise `Create_x0033_04aa07b_x002D_6c8c_x002D_4641_x002D_93a6_x002D_f3152f8740a1_028da4e3_x002D_9c6e_x002D_480b_x002D_b53c_x002D_c1db13d24d70Model` lauten. Benennen Sie diese Methode in __CreateModel__ um.

7. Verschieben Sie die `.onnx`-Datei im Projektmappen-Explorer in den Ordner __Assets__. 

8. Wählen Sie die `.onnx`-Datei aus, und legen Sie in den Eigenschaften __Build Action__ auf __Content__ fest, um die ONNX-Datei in das Anwendungspaket einzubinden.

9. Öffnen Sie die Datei __MainPage.xaml.cs__. Suchen Sie nach der folgenden Zeile, und ändern Sie den Dateinamen in die neue `.onnx`-Datei:

    ```csharp
    var file = await StorageFile.GetFileFromApplicationUriAsync(new Uri($"ms-appx:///Assets/cat-or-dog.onnx"));
    ```

    Aufgrund dieser Änderung wird das neue Modell zur Laufzeit geladen.

10. Erstellen Sie die App, und führen Sie sie aus. Sie nutzt jetzt das neue Modell zum Bewerten von Bildern.

## <a name="next-steps"></a>Nächste Schritte

Weitere Möglichkeiten zum Exportieren und Verwenden eines Custom Vision-Modells finden Sie in den folgenden Dokumenten:

* [Exportieren Ihres Modells](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/export-your-model)
* [Use exported Tensorflow model in an Android application](https://github.com/Azure-Samples/cognitive-services-android-customvision-sample) (Verwenden des exportierten Tensorflow-Modells in einer Android-Anwendung)
* [Use exported CoreML model in a Swift iOS application](https://go.microsoft.com/fwlink/?linkid=857726) (Verwenden des exportierten CoreML-Modells in einer Swift iOS-Anwendung)
* [Use exported CoreML model in an iOS application with Xamarin](https://github.com/xamarin/ios-samples/tree/master/ios11/CoreMLAzureModel) (Verwenden des exportierten CoreML-Modells in einer iOS-Anwendung mit Xamarin)

Weitere Informationen zur Verwendung von ONNX-Modellen mit Windows ML finden Sie im Dokument [Integrieren eines Modells in Ihre App mit Windows ML](https://docs.microsoft.com/windows/uwp/machine-learning/integrate-model).
