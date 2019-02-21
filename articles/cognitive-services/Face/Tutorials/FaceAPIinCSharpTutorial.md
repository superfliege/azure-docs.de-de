---
title: 'Tutorial: Erkennen und Anzeigen von Gesichtserkennungsdaten in einem Bild mit dem .NET SDK'
titleSuffix: Azure Cognitive Services
description: In diesem Tutorial erstellen Sie eine Windows-App, die die Gesichtserkennungs-API verwendet, um Gesichter in einem Bild zu erkennen und zu umranden.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: tutorial
ms.date: 02/06/2019
ms.author: pafarley
ms.openlocfilehash: 6a60afc45894518f92115976876ddd50efa1e410
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/15/2019
ms.locfileid: "56310184"
---
# <a name="tutorial-create-a-wpf-app-to-display-face-data-in-an-image"></a>Tutorial: Erstellen einer WPF-App zum Anzeigen von Gesichtserkennungsdaten in einem Bild

In diesem Tutorial erfahren Sie, wie sie die Azure-Gesichtserkennungs-API über das .NET Client SDK verwenden, um Gesichter in einem Bild zu erkennen und diese Daten anschließend auf der Benutzeroberfläche darzustellen. Sie erstellen eine einfache WPF-Anwendung (Windows Presentation Foundation), die Gesichter erkennt, einen Rahmen um jedes Gesicht zeichnet und eine Beschreibung des Gesichts auf der Statusleiste anzeigt. 

Dieses Tutorial veranschaulicht folgende Vorgehensweisen:

> [!div class="checklist"]
> - Erstellen einer WPF-Anwendung
> - Installieren der Clientbibliothek für die Gesichtserkennungs-API
> - Verwenden der Clientbibliothek zum Erkennen von Gesichtern in einem Bild
> - Zeichnen eines Rahmens um jedes erkannte Gesicht
> - Anzeigen einer Beschreibung des hervorgehobenen Gesichts auf der Statusleiste

![Screenshot: Erkannte Gesichter mit Rechtecken als Umrandung](../Images/getting-started-cs-detected.png)

Der vollständige Beispielcode ist im Repository [Cognitive Face CSharp sample](https://github.com/Azure-Samples/Cognitive-Face-CSharp-sample) (CSharp-Beispiel für die Gesichtserkennung) auf GitHub verfügbar.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen. 


## <a name="prerequisites"></a>Voraussetzungen

- Ein Abonnementschlüssel für die Gesichtserkennungs-API. Über die Seite [Cognitive Services ausprobieren](https://azure.microsoft.com/try/cognitive-services/?api=face-api) können Sie einen Abonnementschlüssel für eine kostenlose Testversion abrufen. Gehen Sie andernfalls wie unter [Schnellstart: Erstellen eines Cognitive Services-Kontos im Azure-Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) beschrieben vor, um den Gesichtserkennungs-API-Dienst zu abonnieren und Ihren Schlüssel zu erhalten.
- Eine beliebige Edition von [Visual Studio 2015 oder 2017](https://www.visualstudio.com/downloads/).

## <a name="create-the-visual-studio-project"></a>Erstellen des Visual Studio-Projekts

Führen Sie diese Schritte aus, um ein neues WPF-Anwendungsprojekt zu erstellen.

1. Öffnen Sie in Visual Studio das Dialogfeld „Neues Projekt“. Erweitern Sie **Installiert** und dann **Visual C#**, und wählen Sie **WPF-App (.NET Framework)** aus.
1. Geben Sie der Anwendung den Namen **FaceTutorial**, und klicken Sie anschließend auf **OK**.
1. Rufen Sie die erforderlichen NuGet-Pakete ab. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, wählen Sie **NuGet-Pakete verwalten** aus, suchen Sie nach den folgenden Paketen, und installieren Sie sie:
    - [Microsoft.Azure.CognitiveServices.Vision.Face 2.2.0-preview](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.Face/2.2.0-preview)

## <a name="add-the-initial-code"></a>Hinzufügen des anfänglichen Codes

In diesem Abschnitt fügen Sie das einfache Framework der App ohne spezifische Features für die Gesichtserkennung hinzu.

### <a name="create-the-ui"></a>Erstellen der Benutzeroberfläche

Öffnen Sie *MainWindow.xaml*, und ersetzen Sie den Inhalt durch den folgenden Code. Dadurch wird das Fenster für die Benutzeroberfläche erstellt. Beachten Sie, dass `FacePhoto_MouseMove` und `BrowseButton_Click` Ereignishandler sind, die Sie später definieren.

[!code-xaml[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml?range=1-18)]

### <a name="create-the-main-class"></a>Erstellen der Hauptklasse

Öffnen Sie *MainWindow.xaml.cs*, und fügen Sie die Namespaces für die Clientbibliothek sowie weitere erforderliche Namespaces hinzu. 

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?range=1-12)]

Fügen Sie als Nächstes den folgenden Code in die **MainWindow**-Klasse ein. Dadurch wird eine **FaceClient**-Instanz erstellt, die den Abonnementschlüssel verwendet. Diesen müssen Sie selbst eingeben. Darüber hinaus müssen Sie die Regionszeichenfolge in `faceEndpoint` auf die korrekte Region für Ihr Abonnement festlegen. (Eine Liste aller Regionsendpunkte finden Sie in den [Dokumenten zur Gesichtserkennungs-API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).)

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?range=18-46)]

Fügen Sie anschließend den folgenden Code in die **MainWindow**-Methode ein.

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?range=50-61)]

Fügen Sie abschließend die Methoden **BrowseButton_Click** und **FacePhoto_MouseMove** zur Klasse hinzu. Die beiden Methoden entsprechen den Ereignishandlern, die in *MainWindow.xaml* deklariert wurden. Die Methode **BrowseButton_Click** erstellt ein **OpenFileDialog**-Element, über das der Benutzer ein JPG-Bild auswählen kann. Anschließend wird das Bild im Hauptfenster angezeigt. Sie fügen den übrigen Code für **BrowseButton_Click** und **FacePhoto_MouseMove** später ein. Beachten Sie außerdem den Verweis `faceList`: eine Liste der **DetectedFace**-Objekte. Hier werden die tatsächlichen Gesichtserkennungsdaten von Ihrer App gespeichert und aufgerufen.

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?range=64-90,146)]

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?range=148-150,187)]

### <a name="try-the-app"></a>Testen der App

Wählen Sie im Menü die Option **Start**, um Ihre App zu testen. Klicken Sie nach dem Öffnen des App-Fensters unten links auf **Durchsuchen**. Das Dialogfeld **File Open** (Datei öffnen) sollte angezeigt werden. Wählen Sie ein Bild in Ihrem Dateisystem aus, und vergewissern Sie sich, dass es im Fenster angezeigt wird. Schließen Sie dann die App, und fahren Sie mit dem nächsten Schritt fort.

![Screenshot: Anzeige eines unbearbeiteten Bilds mit Gesichtern](../Images/getting-started-cs-ui.png)

## <a name="upload-image-and-detect-faces"></a>Hochladen eines Bilds und Erkennen von Gesichtern

Ihre App erkennt Gesichter durch Aufrufen der Methode **FaceClient.Face.DetectWithStreamAsync**, die die [Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)-REST-API zum Hochladen eines lokalen Bilds umschließt.

Fügen Sie die folgende Methode in die **MainWindow**-Klasse unterhalb der Methode **FacePhoto_MouseMove** ein. Dadurch wird eine Liste mit abzurufenden Gesichtsattributen definiert und die übermittelte Bilddatei in einen **Datenstrom** eingelesen. Anschließend werden diese beiden Objekte an den **DetectWithStreamAsync**-Methodenaufruf übergeben.

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?range=189-226)]

## <a name="draw-rectangles-around-faces"></a>Zeichnen von Rechtecken um Gesichter

Im nächsten Schritt fügen Sie den Code hinzu, mit dem um jedes erkannte Gesicht im Bild ein Rechteck gezeichnet wird. Fügen Sie in der **MainWindow**-Klasse den folgenden Code am Ende der Methode **BrowseButton_Click** nach der Zeile `FacePhoto.Source = bitmapSource` ein. Hierdurch wird über den Aufruf **UploadAndDetectFaces** eine Liste der erkannten Gesichter aufgefüllt. Als Nächstes wird um jedes Gesicht ein Rechteck gezeichnet, und das geänderte Bild wird im Hauptfenster angezeigt.

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?range=92-145)]

## <a name="describe-the-faces"></a>Beschreibung der Gesichter

Fügen Sie die folgende Methode zur **MainWindow**-Klasse unterhalb der **UploadAndDetectFaces**-Methode ein. Die Methode konvertiert die abgerufenen Gesichtsattribute in eine Zeichenfolge zur Beschreibung des Gesichts.

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?range=228-286)]

## <a name="display-the-face-description"></a>Anzeigen der Gesichtsbeschreibung

Fügen Sie den folgenden Code zur Methode **FacePhoto_MouseMove** hinzu. Dieser Ereignishandler zeigt die Zeichenfolge für die Gesichtsbeschreibung in `faceDescriptionStatusBar` an, wenn Sie mit dem Cursor auf das Rechteck für das erkannte Gesicht zeigen.

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?range=151-186)]


## <a name="run-the-app"></a>Ausführen der App

Führen Sie die Anwendung aus, und suchen Sie nach einem Bild mit einem Gesicht. Warten Sie einige Sekunden, damit der Gesichtserkennungsdienst reagieren kann. Um jedes Gesicht im Bild sollte ein rotes Rechteck angezeigt werden. Wenn Sie den Mauszeiger auf das Rechteck für ein Gesicht bewegen, sollte die Beschreibung dieses Gesichts auf der Statusleiste angezeigt werden.

![Screenshot: Erkannte Gesichter mit Rechtecken als Umrandung](../Images/getting-started-cs-detected.png)


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde der grundlegende Prozess für die Verwendung des .NET SDK des Gesichtserkennungsdiensts beschrieben und eine Anwendung zum Erkennen und Umranden von Gesichtern in einem Bild erstellt. Machen Sie sich als Nächstes mit den Details zur Gesichtserkennung vertraut.

> [!div class="nextstepaction"]
> [Gewusst wie: Gesichtserkennung in Bildern](../Face-API-How-to-Topics/HowtoDetectFacesinImage.md)
