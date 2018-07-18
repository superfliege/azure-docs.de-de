---
title: Tutorial zur Maschinelles Sehen-API mit C# | Microsoft-Dokumentation
description: Hier lernen Sie eine einfache Windows-App kennen, die die Maschinelles Sehen-API in Microsoft Cognitive Services verwendet. Führen Sie eine optische Zeichenerkennung durch, erstellen Sie Miniaturansichten, und arbeiten Sie mit visuellen Merkmalen in einem Bild.
services: cognitive-services
author: KellyDF
manager: corncar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: article
ms.date: 05/22/2017
ms.author: kefre
ms.openlocfilehash: f2aeb1734f8858ed8b80e5cdf48ef7e558703919
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35374754"
---
# <a name="computer-vision-api-c35-tutorial"></a>Maschinelles Sehen-API: C&#35;-Tutorial

Lernen Sie eine einfache Windows-Anwendung kennen, die die Maschinelles Sehen-API verwendet, um eine optische Zeichenerkennung (Optical Character Recognition, OCR) durchzuführen, intelligent zugeschnittene Miniaturansichten zu erstellen sowie visuelle Merkmale (einschließlich Gesichter) in einem Bild zu erkennen, zu kategorisieren, zu markieren und zu beschreiben. Im folgenden Beispiel können Sie eine Bild-URL oder eine lokal gespeicherte Datei übermitteln. Sie können dieses Open-Source-Beispiel als Vorlage verwenden, um Ihre eigene App für Windows mit der Maschinelles Sehen-API und WPF (Windows Presentation Foundation; eine Komponente von .NET Framework) zu erstellen.

### <a name="prerequisites"></a>Voraussetzungen

#### <a name="platform-requirements"></a>Plattformanforderungen

Das folgende Beispiel wurde mit [Visual Studio 2015, Community Edition](https://www.visualstudio.com/downloads/) für .NET Framework entwickelt.

#### <a name="subscribe-to-computer-vision-api-and-get-a-subscription-key"></a>Abonnieren der Maschinelles Sehen-API und Abrufen eines Abonnementschlüssels 

Bevor Sie das Beispiel erstellen, müssen Sie die Maschinelles Sehen-API aus Microsoft Cognitive Services (ehemals Project Oxford) abonnieren. Informationen zu Abonnements und zur Schlüsselverwaltung finden Sie unter [Abonnements](https://azure.microsoft.com/try/cognitive-services/). In diesem Tutorial kann sowohl der Primär- als auch der Sekundärschlüssel verwendet werden. 

> [!NOTE]
> Das Tutorial ist für die Verwendung von Abonnementschlüsseln in der Region **westcentralus** konzipiert. Die in der kostenlosen Testversion für maschinelles Sehen generierten Abonnementschlüssel verwenden die Region **westcentralus** und funktionieren daher ordnungsgemäß. Wenn Sie Ihre Abonnementschlüssel unter Verwendung Ihres Azure-Kontos über [https://azure.microsoft.com/](https://azure.microsoft.com/) generiert haben, müssen Sie die Region **westcentralus** angeben. Außerhalb der Region **westcentralus** generierte Schlüssel können nicht verwendet werden.

#### <a name="get-the-client-library-and-example"></a>Abrufen der Clientbibliothek und des Beispiels

Sie können die Clientbibliothek für die Maschinelles Sehen-API und die entsprechende Beispielanwendung über das [SDK](https://www.github.com/microsoft/cognitive-vision-windows) auf Ihrem Computer klonen. Laden Sie sie nicht als ZIP-Datei herunter.

### <a name="Step1">Schritt 1: Installieren des Beispiels</a>

Öffnen Sie „Sample-WPF\VisionAPI-WPF-Samples.sln“ in Ihrer GitHub Desktop-Instanz.

### <a name="Step2">Schritt 2: Erstellen des Beispiels</a>

* Drücken Sie STRG+UMSCHALT+B, oder klicken Sie auf dem Menüband auf „Erstellen“ und anschließend auf „Projektmappe erstellen“.

### <a name="Step3">Schritt 3: Ausführen des Beispiels</a>

1. Drücken Sie nach Abschluss des Buildvorgangs**F5**, oder klicken Sie auf dem Menüband auf **Starten**, um das Beispiel auszuführen.
2. Navigieren Sie zum Fenster mit der Benutzeroberfläche der Maschinelles Sehen-API und dem Textfeld „Paste your subscription key here to start“ (Abonnementschlüssel hier einfügen, um zu beginnen).
Durch Klicken auf „Save Key“ (Schlüssel speichern) können Sie den Abonnementschlüssel auf Ihrem PC oder Laptop speichern. Wenn Sie den Abonnementschlüssel aus dem System löschen möchten, klicken Sie auf „Delete Key“ (Schlüssel löschen), um ihn von Ihrem PC oder Laptop zu entfernen.

    ![Abonnementschlüssel für maschinelles Sehen](../Images/Vision_UI_Subscription.PNG)

3. Klicken Sie unter „Select Scenario“ (Szenario auswählen) auf eines der sechs Szenarien, und folgen Sie dann den Anweisungen auf dem Bildschirm. Microsoft empfängt die Bilder, die Sie hochladen, und nutzt sie ggf. zur Verbesserung der Maschinelles Sehen-API und der dazugehörigen Dienste. Wenn Sie ein Bild übermitteln, bestätigen Sie dadurch, dass Sie sich an unsere [Verhaltensregeln für Entwickler](https://azure.microsoft.com/support/legal/developer-code-of-conduct/) gehalten haben.

    ![Schnittstelle für die Bildanalyse](../Images/Analyze_Image_Example.PNG)

4. Für diese Beispielanwendung stehen Beispielbilder zur Verfügung. Diese befinden sich im Ordner [Data](https://github.com/Microsoft/Cognitive-Face-Windows/tree/master/Data) des Windows-GitHub-Repositorys für die Gesichtserkennungs-API. Die Verwendung dieser Bilder ist unter der Vereinbarung [LICENSE-IMAGE](https://github.com/Microsoft/Cognitive-Face-Windows/blob/master/LICENSE-IMAGE.md) lizenziert.

### <a name="Review">Ansehen und Lernen</a>

Nachdem Sie nun über eine aktive Anwendung verfügen, erfahren Sie als Nächstes, wie sich die Integration zwischen dieser Beispiel-App und der Cognitive Services-Technologie gestaltet. Auf der Grundlage dieser Informationen können Sie entweder diese App weiterentwickeln oder Ihre eigene App mit der Maschinelles Sehen-API von Microsoft entwickeln.

Diese Beispiel-App nutzt die Clientbibliothek der Maschinelles Sehen-API. Dabei handelt es sich um einen C#-Thin Client-Wrapper für die Maschinelles Sehen-API von Microsoft. Wenn Sie die Beispiel-App wie oben beschrieben erstellt haben, haben Sie die Clientbibliothek aus einem NuGet-Paket erhalten. Der Quellcode der Clientbibliothek befindet sich im Ordner „**Client Library**“ unter **Vision** > **Windows** > **Client Library**. Dieser Ordner ist Teil des heruntergeladenen Dateirepositorys, das unter „Voraussetzungen“ erwähnt wurde.

Sie können sich auch im Projektmappen-Explorer mit der Verwendung des Clientbibliothekcodes vertraut machen: Erweitern Sie unter **VisionAPI-WPF_Samples** die Option **AnalyzePage.xaml**, und suchen Sie nach **AnalyzePage.xaml.cs** (dient zum Übermitteln eines Bilds an den Endpunkt für die Bildanalyse). Doppelklicken Sie auf die XAML.CS-Dateien, um sie in neuen Visual Studio-Fenstern zu öffnen.

Zur Erläuterung der Verwendung der Clientbibliothek für maschinelles Sehen in unserer Beispiel-App betrachten wir zwei Codeausschnitte aus **AnalyzePage.xaml.cs**. Die Datei enthält die Codekommentare „KEY SAMPLE CODE STARTS HERE“ und „KEY SAMPLE CODE ENDS HERE“, die den Beginn und das Ende des betreffenden Beispielcodes angeben, damit Sie die weiter unten reproduzierten Codeausschnitte leichter finden.

Der Analyseendpunkt kann als Eingabe eine Bild-URL oder binäre Bilddaten (in Form eines Oktett-Streams) verwenden. Am Anfang befindet sich eine using-Direktive, die die Verwendung der Clientbibliothek für maschinelles Sehen ermöglicht.

```
                // ----------------------------------------------------------------------
                // KEY SAMPLE CODE STARTS HERE
                // Use the following namespace for VisionServiceClient 
                // ---------------------------------------------------------------------- 
                using Microsoft.ProjectOxford.Vision; 
                using Microsoft.ProjectOxford.Vision.Contract; 
                // ----------------------------------------------------------------------
                // KEY SAMPLE CODE ENDS HERE 
                // ----------------------------------------------------------------------

```
**UploadAndAnalyzeImage(...)** – Dieser Codeausschnitt zeigt, wie Sie mithilfe der Clientbibliothek Ihren Abonnementschlüssel und ein lokal gespeichertes Bild an den Analyseendpunkt des Maschinelles Sehen-API-Diensts übermitteln.

```
    private async Task<AnalysisResult> UploadAndAnalyzeImage(string imageFilePath)
    {
        // -----------------------------------------------------------------------
        // KEY SAMPLE CODE STARTS HERE
        // -----------------------------------------------------------------------  
        //
        // Create Project Oxford Computer Vision API Service client
        //
        VisionServiceClient VisionServiceClient = new VisionServiceClient(SubscriptionKey);
        Log("VisionServiceClient is created");
    
        using (Stream imageFileStream = File.OpenRead(imageFilePath))
        {
            //
            // Analyze the image for all visual features
            //
            Log("Calling VisionServiceClient.AnalyzeImageAsync()...");
         VisualFeature[] visualFeatures = new VisualFeature[] { VisualFeature.Adult, VisualFeature.Categories, VisualFeature.Color, VisualFeature.Description, VisualFeature.Faces, VisualFeature.ImageType, VisualFeature.Tags };
            AnalysisResult analysisResult = await VisionServiceClient.AnalyzeImageAsync(imageFileStream, visualFeatures);
            return analysisResult;
        }
    
        // -----------------------------------------------------------------------
        // KEY SAMPLE CODE ENDS HERE
        // -----------------------------------------------------------------------
        }
```
**AnalyzeUrl(...)** – Dieser Codeausschnitt zeigt, wie Sie mithilfe der Clientbibliothek Ihren Abonnementschlüssel und eine Foto-URL an den Analyseendpunkt des Maschinelles Sehen-API-Diensts übermitteln.

```
    private async Task<AnalysisResult> AnalyzeUrl(string imageUrl)
    {
        // -----------------------------------------------------------------------
        // KEY SAMPLE CODE STARTS HERE
        // -----------------------------------------------------------------------
    
        //
        // Create Project Oxford Computer Vision API Service client
        //
     VisionServiceClient VisionServiceClient = new VisionServiceClient(SubscriptionKey);
        Log("VisionServiceClient is created");
    
        //
        // Analyze the url for all visual features
        //
        Log("Calling VisionServiceClient.AnalyzeImageAsync()...");
        VisualFeature[] visualFeatures = new VisualFeature[] { VisualFeature.Adult, VisualFeature.Categories, VisualFeature.Color, VisualFeature.Description, VisualFeature.Faces, VisualFeature.ImageType, VisualFeature.Tags };
        AnalysisResult analysisResult = await VisionServiceClient.AnalyzeImageAsync(imageUrl, visualFeatures);
     return analysisResult;
    }
        // -----------------------------------------------------------------------
        // KEY SAMPLE CODE ENDS HERE
        // -----------------------------------------------------------------------
```
**Andere Seiten und Endpunkte** – Informationen zur Interaktion mit den anderen Endpunkten, die vom Maschinelles Sehen-API-Dienst verfügbar gemacht werden, können Sie den anderen Seiten des Beispiels entnehmen. So wird beispielsweise der OCR-Endpunkt im Code in „OCRPage.xaml.cs“ angezeigt. 

### <a name="Related">Verwandte Themen</a>
 * [Erste Schritte mit der Gesichtserkennungs-API](../../Face/Tutorials/FaceAPIinCSharpTutorial.md)
 
 


