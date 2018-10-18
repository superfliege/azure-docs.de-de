---
title: Erkennen und Umranden von Gesichtern in einem Bild – Gesichtserkennungs-API, C#
titleSuffix: Azure Cognitive Services
description: In diesem Tutorial erstellen Sie eine Windows-App, die die Gesichtserkennungs-API verwendet, um Gesichter in einem Bild zu erkennen und zu umranden.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: tutorial
ms.date: 09/24/2018
ms.author: pafarley
ms.openlocfilehash: ba850c1d4f9d8018f333662eead56f8e91b3fbd7
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2018
ms.locfileid: "49340952"
---
# <a name="tutorial-create-a-wpf-app-to-detect-and-frame-faces-in-an-image"></a>Tutorial: Erstellen einer WPF-App zum Erkennen und Umranden von Gesichtern in einem Bild

In diesem Tutorial erstellen Sie eine WPF-Anwendung (Windows Presentation Foundation), für die der Gesichtserkennungsdienst über die zugehörige .NET-Clientbibliothek genutzt wird. Die App erkennt Gesichter in einem Bild, zeichnet einen Rahmen um jedes Gesicht und zeigt eine Beschreibung des Gesichts in der Statusleiste an. Der vollständige Beispielcode ist auf GitHub unter [Detect and frame faces in an image on Windows](https://github.com/Azure-Samples/Cognitive-Face-CSharp-sample) (Erkennen und Umranden von Gesichtern in einem Bild unter Windows) verfügbar.

![Screenshot: Erkannte Gesichter mit Rechtecken als Umrandung](../Images/getting-started-cs-detected.png)

Dieses Tutorial veranschaulicht folgende Vorgehensweisen:

> [!div class="checklist"]
> - Erstellen einer WPF-Anwendung
> - Installieren der Clientbibliothek für den Gesichtserkennungsdienst
> - Verwenden der Clientbibliothek zum Erkennen von Gesichtern in einem Bild
> - Zeichnen eines Rahmens um jedes erkannte Gesicht
> - Anzeigen einer Beschreibung des Gesichts in der Statusleiste

## <a name="prerequisites"></a>Voraussetzungen

- Zum Ausführen des Beispiels benötigen Sie einen Abonnementschlüssel. Über die Seite [Cognitive Services ausprobieren](https://azure.microsoft.com/try/cognitive-services/?api=face-api) können Sie Abonnementschlüssel für eine kostenlose Testversion abrufen.
- Eine beliebige Edition von [Visual Studio 2015 oder 2017](https://www.visualstudio.com/downloads/). Für Visual Studio 2017 ist die Workload für die .NET-Desktopanwendungsentwicklung erforderlich. In diesem Tutorial wird die Visual Studio 2017 Community Edition verwendet.
- Das NuGet-Paket [Microsoft.Azure.CognitiveServices.Vision.Face 2.2.0-preview](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.Face/2.2.0-preview) mit der Clientbibliothek. Es ist nicht erforderlich, das Paket herunterzuladen. Unten ist die Installationsanleitung angegeben.

## <a name="create-the-visual-studio-solution"></a>Erstellen der Visual Studio-Projektmappe

Führen Sie diese Schritte aus, um ein unter Windows ein WPF-Anwendungsprojekt zu erstellen.

1. Öffnen Sie Visual Studio, und klicken Sie im Menü **Datei** auf **Neu** und dann auf **Projekt**.
   - Erweitern Sie in Visual Studio 2017 die Option **Installiert** und dann **Andere Sprachen**. Wählen Sie **Visual C#** und dann **WPF-App (.NET Framework)**.
   - Erweitern Sie in Visual Studio 2015 die Option **Installiert** und dann **Vorlagen**. Wählen Sie **Visual C#** und dann **WPF-Anwendung**.
1. Geben Sie der Anwendung den Namen **FaceTutorial**, und klicken Sie anschließend auf **OK**.

## <a name="install-the-face-service-client-library"></a>Installieren der Clientbibliothek für den Gesichtserkennungsdienst

Verwenden Sie die folgende Anleitung, um die Clientbibliothek zu installieren:

1. Wählen Sie im Menü **Extras** den Eintrag **NuGet-Paket-Manager** und danach **Paket-Manager-Konsole**.
1. Fügen Sie in der **Paket-Manager-Konsole** Folgendes ein, und drücken Sie die **EINGABETASTE**.

    `Install-Package Microsoft.Azure.CognitiveServices.Vision.Face -Version 2.2.0-preview`

## <a name="add-the-initial-code"></a>Hinzufügen des anfänglichen Codes

### <a name="mainwindowxaml"></a>MainWindow.xaml

Öffnen Sie *MainWindow.xaml* (Tipp: Bereichswechsel über das **Symbol mit dem Pfeil nach oben/unten**), und ersetzen Sie den Inhalt durch den folgenden Code. Dieser XAML-Code wird verwendet, um das Fenster für die Benutzeroberfläche zu erstellen. Beachten Sie die Ereignishandler `FacePhoto_MouseMove` und `BrowseButton_Click`.

```xml
<Window x:Class="FaceTutorial.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        Title="MainWindow" Height="700" Width="960">
    <Grid x:Name="BackPanel">
        <Image x:Name="FacePhoto" Stretch="Uniform" Margin="0,0,0,50" MouseMove="FacePhoto_MouseMove" />
        <DockPanel DockPanel.Dock="Bottom">
            <Button x:Name="BrowseButton" Width="72" Height="20" VerticalAlignment="Bottom" HorizontalAlignment="Left"
                    Content="Browse..."
                    Click="BrowseButton_Click" />
            <StatusBar VerticalAlignment="Bottom">
                <StatusBarItem>
                    <TextBlock Name="faceDescriptionStatusBar" />
                </StatusBarItem>
            </StatusBar>
        </DockPanel>
    </Grid>
</Window>
```

### <a name="mainwindowxamlcs"></a>MainWindow.xaml.cs

Erweitern Sie *MainWindow.xaml*, öffnen Sie *MainWindow.xaml.cs*, und ersetzen Sie den Inhalt durch den folgenden Code. Ignorieren Sie die Unterstreichungen mit den roten Wellenlinien. Sie sind nach dem ersten Buildvorgang nicht mehr vorhanden.

Mit den ersten beiden Zeilen werden die Namespaces für die Clientbibliothek importiert. Als Nächstes wird `FaceClient` erstellt, und der Abonnementschlüssel wird übergeben, während die Azure-Region im Konstruktor `MainWindow` festgelegt wird. Die beiden Methoden `BrowseButton_Click` und `FacePhoto_MouseMove` entsprechen den Ereignishandlern, die in *MainWindow.xaml* deklariert wurden.

Mit `BrowseButton_Click` wird das `OpenFileDialog`-Element erstellt, mit dem der Benutzer ein JPG-Bild auswählen kann. Das Bild wird gelesen und im Hauptfenster angezeigt. Der restliche Code für `BrowseButton_Click` und der Code für `FacePhoto_MouseMove` werden in den nachfolgenden Schritten eingefügt.

```csharp
using Microsoft.Azure.CognitiveServices.Vision.Face;
using Microsoft.Azure.CognitiveServices.Vision.Face.Models;

using System;
using System.Collections.Generic;
using System.IO;
using System.Text;
using System.Threading.Tasks;
using System.Windows;
using System.Windows.Input;
using System.Windows.Media;
using System.Windows.Media.Imaging;

namespace FaceTutorial
{
    public partial class MainWindow : Window
    {
        // Replace <SubscriptionKey> with your valid subscription key.
        // For example, subscriptionKey = "0123456789abcdef0123456789ABCDEF"
        private const string subscriptionKey = "<SubscriptionKey>";

        // Replace or verify the region.
        //
        // You must use the same region as you used to obtain your subscription
        // keys. For example, if you obtained your subscription keys from the
        // westus region, replace "westcentralus" with "westus".
        //
        // NOTE: Free trial subscription keys are generated in the westcentralus
        // region, so if you are using a free trial subscription key, you should
        // not need to change this region.
        private const string faceEndpoint =
            "https://westcentralus.api.cognitive.microsoft.com";

        private readonly IFaceClient faceClient = new FaceClient(
            new ApiKeyServiceClientCredentials(subscriptionKey),
            new System.Net.Http.DelegatingHandler[] { });

        IList<DetectedFace> faceList;   // The list of detected faces.
        String[] faceDescriptions;      // The list of descriptions for the detected faces.
        double resizeFactor;            // The resize factor for the displayed image.

        public MainWindow()
        {
            InitializeComponent();

            if (Uri.IsWellFormedUriString(faceEndpoint, UriKind.Absolute))
            {
                faceClient.Endpoint = faceEndpoint;
            }
            else
            {
                MessageBox.Show(faceEndpoint,
                    "Invalid URI", MessageBoxButton.OK, MessageBoxImage.Error);
                Environment.Exit(0);
            }
        }

        // Displays the image and calls UploadAndDetectFaces.
        private async void BrowseButton_Click(object sender, RoutedEventArgs e)
        {
            // Get the image file to scan from the user.
            var openDlg = new Microsoft.Win32.OpenFileDialog();

            openDlg.Filter = "JPEG Image(*.jpg)|*.jpg";
            bool? result = openDlg.ShowDialog(this);

            // Return if canceled.
            if (!(bool)result)
            {
                return;
            }

            // Display the image file.
            string filePath = openDlg.FileName;

            Uri fileUri = new Uri(filePath);
            BitmapImage bitmapSource = new BitmapImage();

            bitmapSource.BeginInit();
            bitmapSource.CacheOption = BitmapCacheOption.None;
            bitmapSource.UriSource = fileUri;
            bitmapSource.EndInit();

            FacePhoto.Source = bitmapSource;
        }

        // Displays the face description when the mouse is over a face rectangle.
        private void FacePhoto_MouseMove(object sender, MouseEventArgs e)
        {
        }
    }
}
```

### <a name="insert-your-subscription-key-and-verify-or-change-the-region"></a>Einfügen Ihres Abonnementschlüssels und Überprüfen bzw. Ändern der Region

- Suchen Sie in *MainWindow.xaml.cs* nach der folgenden Zeile, und ersetzen Sie `<Subscription Key>` durch Ihren Abonnementschlüssel für die Gesichtserkennungs-API:

    ```csharp
    private const string subscriptionKey = "<SubscriptionKey>";
    ```

- Suchen Sie in *MainWindow.xaml.cs* nach der folgenden Zeile, und ersetzen Sie die Azure-Region, die Ihrem Abonnementschlüssel zugeordnet ist, bzw. verifizieren Sie sie:

    ```csharp
    private const string Endpoint =
        "https://westcentralus.api.cognitive.microsoft.com";
    ```

    Stellen Sie sicher, dass dies derselbe Standort ist, von dem Sie Ihre Abonnementschlüssel erhalten haben. Wenn Sie Ihre Abonnementschlüssel in der Region **westus** erhalten haben, ersetzen Sie beispielsweise `Westcentralus` durch `Westus`.

    Wenn Sie Ihre Abonnementschlüssel über die Nutzung der kostenlosen Testversion erhalten haben, lautet die Region für Ihre Schlüssel **westcentralus**, sodass keine Änderung erforderlich ist.

### <a name="test-the-app"></a>Testen der App

Wählen Sie im Menü die Option **Start**, um Ihre App zu testen. Klicken Sie nach dem Öffnen des Fensters unten links auf **Durchsuchen**. Das Dialogfeld **Datei öffnen** wird angezeigt. Darin können Sie ein Foto suchen und auswählen, das im Fenster angezeigt werden soll.

![Screenshot: Anzeige eines unbearbeiteten Bilds mit Gesichtern](../Images/getting-started-cs-ui.png)

## <a name="upload-an-image-to-detect-faces"></a>Hochladen eines Bilds zur Erkennung von Gesichtern

Der einfachste Weg zur Erkennung von Gesichtern ist das Aufrufen der `FaceClient.Face.DetectWithStreamAsync`-Methode, die die [Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)-API-Methode zum Hochladen des lokalen Bilds umschließt.

Fügen Sie die folgende Methode in der `MainWindow`-Klasse unterhalb der `FacePhoto_MouseMove`-Methode ein.

Eine Liste mit Gesichtsattributen für die Analyse wird erstellt, und die übermittelte Bilddatei wird in einen `Stream` eingelesen. Beide Elemente werden an den `DetectWithStreamAsync`-Aufruf übergeben.

```csharp
// Uploads the image file and calls DetectWithStreamAsync.
private async Task<IList<DetectedFace>> UploadAndDetectFaces(string imageFilePath)
{
    // The list of Face attributes to return.
    IList<FaceAttributeType> faceAttributes =
        new FaceAttributeType[]
        {
            FaceAttributeType.Gender, FaceAttributeType.Age,
            FaceAttributeType.Smile, FaceAttributeType.Emotion,
            FaceAttributeType.Glasses, FaceAttributeType.Hair
        };

    // Call the Face API.
    try
    {
        using (Stream imageFileStream = File.OpenRead(imageFilePath))
        {
            // The second argument specifies to return the faceId, while
            // the third argument specifies not to return face landmarks.
            IList<DetectedFace> faceList =
                await faceClient.Face.DetectWithStreamAsync(
                    imageFileStream, true, false, faceAttributes);
            return faceList;
        }
    }
    // Catch and display Face API errors.
    catch (APIErrorException f)
    {
        MessageBox.Show(f.Message);
        return new List<DetectedFace>();
    }
    // Catch and display all other errors.
    catch (Exception e)
    {
        MessageBox.Show(e.Message, "Error");
        return new List<DetectedFace>();
    }
}
```

## <a name="draw-rectangles-around-each-face"></a>Zeichnen von Rechtecken um jedes Gesicht

Fügen Sie den Code hinzu, mit dem um jedes erkannte Gesicht im Bild ein Rechteck gezeichnet wird.

Fügen Sie in *MainWindow.xaml.cs* den Modifizierer `async` der `BrowseButton_Click`-Methode hinzu.

```csharp
private async void BrowseButton_Click(object sender, RoutedEventArgs e)
```

Fügen Sie den folgenden Code am Ende der `BrowseButton_Click`-Methode nach der Zeile `FacePhoto.Source = bitmapSource` ein.

Die Liste mit den erkannten Gesichtern wird über den Aufruf von `UploadAndDetectFaces` aufgefüllt. Als Nächstes wird um jedes Gesicht ein Rechteck gezeichnet, und das geänderte Bild wird im Hauptfenster angezeigt.

```csharp
// Detect any faces in the image.
Title = "Detecting...";
faceList = await UploadAndDetectFaces(filePath);
Title = String.Format(
    "Detection Finished. {0} face(s) detected", faceList.Count);

if (faceList.Count > 0)
{
    // Prepare to draw rectangles around the faces.
    DrawingVisual visual = new DrawingVisual();
    DrawingContext drawingContext = visual.RenderOpen();
    drawingContext.DrawImage(bitmapSource,
        new Rect(0, 0, bitmapSource.Width, bitmapSource.Height));
    double dpi = bitmapSource.DpiX;
    resizeFactor = (dpi > 0) ? 96 / dpi : 1;
    faceDescriptions = new String[faceList.Count];

    for (int i = 0; i < faceList.Count; ++i)
    {
        DetectedFace face = faceList[i];

        // Draw a rectangle on the face.
        drawingContext.DrawRectangle(
            Brushes.Transparent,
            new Pen(Brushes.Red, 2),
            new Rect(
                face.FaceRectangle.Left * resizeFactor,
                face.FaceRectangle.Top * resizeFactor,
                face.FaceRectangle.Width * resizeFactor,
                face.FaceRectangle.Height * resizeFactor
                )
        );

        // Store the face description.
        faceDescriptions[i] = FaceDescription(face);
    }

    drawingContext.Close();

    // Display the image with the rectangle around the face.
    RenderTargetBitmap faceWithRectBitmap = new RenderTargetBitmap(
        (int)(bitmapSource.PixelWidth * resizeFactor),
        (int)(bitmapSource.PixelHeight * resizeFactor),
        96,
        96,
        PixelFormats.Pbgra32);

    faceWithRectBitmap.Render(visual);
    FacePhoto.Source = faceWithRectBitmap;

    // Set the status bar text.
    faceDescriptionStatusBar.Text =
        "Place the mouse pointer over a face to see the face description.";
}
```

## <a name="describe-the-faces-in-the-image"></a>Beschreiben der Gesichter im Bild

Fügen Sie die folgende Methode an die `MainWindow`-Klasse unterhalb der `UploadAndDetectFaces`-Methode an.

Die Methode konvertiert die Gesichtsattribute in eine Zeichenfolge zur Beschreibung des Gesichts. Die Zeichenfolge wird angezeigt, wenn der Mauszeiger auf das Rechteck für das Gesicht bewegt wird.

```csharp
// Creates a string out of the attributes describing the face.
private string FaceDescription(DetectedFace face)
{
    StringBuilder sb = new StringBuilder();

    sb.Append("Face: ");

    // Add the gender, age, and smile.
    sb.Append(face.FaceAttributes.Gender);
    sb.Append(", ");
    sb.Append(face.FaceAttributes.Age);
    sb.Append(", ");
    sb.Append(String.Format("smile {0:F1}%, ", face.FaceAttributes.Smile * 100));

    // Add the emotions. Display all emotions over 10%.
    sb.Append("Emotion: ");
    Emotion emotionScores = face.FaceAttributes.Emotion;
    if (emotionScores.Anger >= 0.1f)
        sb.Append(String.Format("anger {0:F1}%, ", emotionScores.Anger * 100));
    if (emotionScores.Contempt >= 0.1f)
        sb.Append(String.Format("contempt {0:F1}%, ", emotionScores.Contempt * 100));
    if (emotionScores.Disgust >= 0.1f)
        sb.Append(String.Format("disgust {0:F1}%, ", emotionScores.Disgust * 100));
    if (emotionScores.Fear >= 0.1f)
        sb.Append(String.Format("fear {0:F1}%, ", emotionScores.Fear * 100));
    if (emotionScores.Happiness >= 0.1f)
        sb.Append(String.Format("happiness {0:F1}%, ", emotionScores.Happiness * 100));
    if (emotionScores.Neutral >= 0.1f)
        sb.Append(String.Format("neutral {0:F1}%, ", emotionScores.Neutral * 100));
    if (emotionScores.Sadness >= 0.1f)
        sb.Append(String.Format("sadness {0:F1}%, ", emotionScores.Sadness * 100));
    if (emotionScores.Surprise >= 0.1f)
        sb.Append(String.Format("surprise {0:F1}%, ", emotionScores.Surprise * 100));

    // Add glasses.
    sb.Append(face.FaceAttributes.Glasses);
    sb.Append(", ");

    // Add hair.
    sb.Append("Hair: ");

    // Display baldness confidence if over 1%.
    if (face.FaceAttributes.Hair.Bald >= 0.01f)
        sb.Append(String.Format("bald {0:F1}% ", face.FaceAttributes.Hair.Bald * 100));

    // Display all hair color attributes over 10%.
    IList<HairColor> hairColors = face.FaceAttributes.Hair.HairColor;
    foreach (HairColor hairColor in hairColors)
    {
        if (hairColor.Confidence >= 0.1f)
        {
            sb.Append(hairColor.Color.ToString());
            sb.Append(String.Format(" {0:F1}% ", hairColor.Confidence * 100));
        }
    }

    // Return the built string.
    return sb.ToString();
}
```

## <a name="display-the-face-description"></a>Anzeigen der Gesichtsbeschreibung

Ersetzen Sie die `FacePhoto_MouseMove`-Methode durch den folgenden Code.

Dieser Ereignishandler zeigt die Zeichenfolge für die Gesichtsbeschreibung an, wenn der Mauszeiger auf das Rechteck für das Gesicht bewegt wird.

```csharp
private void FacePhoto_MouseMove(object sender, MouseEventArgs e)
{
    // If the REST call has not completed, return.
    if (faceList == null)
        return;

    // Find the mouse position relative to the image.
    Point mouseXY = e.GetPosition(FacePhoto);

    ImageSource imageSource = FacePhoto.Source;
    BitmapSource bitmapSource = (BitmapSource)imageSource;

    // Scale adjustment between the actual size and displayed size.
    var scale = FacePhoto.ActualWidth / (bitmapSource.PixelWidth / resizeFactor);

    // Check if this mouse position is over a face rectangle.
    bool mouseOverFace = false;

    for (int i = 0; i < faceList.Count; ++i)
    {
        FaceRectangle fr = faceList[i].FaceRectangle;
        double left = fr.Left * scale;
        double top = fr.Top * scale;
        double width = fr.Width * scale;
        double height = fr.Height * scale;

        // Display the face description if the mouse is over this face rectangle.
        if (mouseXY.X >= left && mouseXY.X <= left + width &&
            mouseXY.Y >= top  && mouseXY.Y <= top + height)
        {
            faceDescriptionStatusBar.Text = faceDescriptions[i];
            mouseOverFace = true;
            break;
        }
    }

    // String to display when the mouse is not over a face rectangle.
    if (!mouseOverFace)
        faceDescriptionStatusBar.Text =
            "Place the mouse pointer over a face to see the face description.";
}
```

## <a name="run-the-app"></a>Ausführen der App

Führen Sie die Anwendung aus, und suchen Sie nach einem Bild mit einem Gesicht. Warten Sie einige Sekunden, damit der Gesichtserkennungsdienst reagieren kann. Anschließend wird für die Gesichter im Bild ein rotes Rechteck angezeigt. Die Beschreibung des Gesichts wird in der Statusleiste angezeigt, wenn der Mauszeiger auf das Rechteck für ein Gesicht bewegt wird.

![Screenshot: Erkannte Gesichter mit Rechtecken als Umrandung](../Images/getting-started-cs-detected.png)

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial wurde der grundlegende Prozess für die Verwendung der Clientbibliothek des Gesichtserkennungsdiensts beschrieben und eine Anwendung zum Anzeigen und Umranden von Gesichtern in einem Bild erstellt.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über das Erkennen und Verwenden von Gesichtszügen.

> [!div class="nextstepaction"]
> [Gewusst wie: Gesichtserkennung in Bildern](../Face-API-How-to-Topics/HowtoDetectFacesinImage.md)
