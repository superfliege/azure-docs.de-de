---
title: 'Tutorial: Erkennen von Emotionen auf Gesichtern in einem Bild – Emotionen-API, C#'
titlesuffix: Azure Cognitive Services
description: Lernen Sie eine einfache Windows-App kennen, die Emotionen auf Gesichtern in einem Bild erkennt.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: tutorial
ms.date: 01/23/2017
ms.author: anroth
ROBOTS: NOINDEX
ms.openlocfilehash: da605ec4013fb11606f99f3d9a2dcfcfcab00d3b
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2018
ms.locfileid: "53163326"
---
# <a name="tutorial-recognize-emotions-on-a-face-in-an-image"></a>Tutorial: Erkennen von Emotionen auf Gesichtern in einem Bild

> [!IMPORTANT]
> Die Emotionen-API wird am 15. Februar 2019 eingestellt. Die Funktion zur Erkennung von Emotionen ist jetzt als Teil der [Gesichtserkennungs-API](https://docs.microsoft.com/azure/cognitive-services/face/) allgemein verfügbar. 

Untersuchen einer einfachen Windows-App, die die Emotionen-API zur Erkennung der Emotionen verwendet, die Gesichter in einem Bild ausdrücken. Im folgenden Beispiel können Sie eine Bild-URL oder eine lokal gespeicherte Datei übermitteln. Sie können dieses Open-Source-Beispiel als Vorlage verwenden, um Ihre eigene App für Windows mit der Emotionen-API und WPF (Windows Presentation Foundation; eine Komponente von .NET Framework) zu erstellen.

## <a name="Prerequisites">Voraussetzungen</a>
#### <a name="platform-requirements"></a>Plattformanforderungen  
Das folgende Beispiel wurde mit [Visual Studio 2015, Community Edition](https://www.visualstudio.com/products/visual-studio-community-vs) für .NET Framework entwickelt.  

#### <a name="subscribe-to-emotion-api-and-get-a-subscription-key"></a>Abonnieren der Emotionen-API und Anfordern eines Abonnementschlüssels  
Bevor Sie das Beispiel erstellen, müssen Sie die Emotionen-API abonnieren, die Teil der Microsoft Cognitive Services ist. Weitere Informationen finden Sie unter [Abonnements](https://azure.microsoft.com/try/cognitive-services/). In diesem Tutorial können sowohl der Primär- als auch der Sekundärschlüssel verwendet werden. Befolgen Sie die empfohlenen Vorgehensweisen, um Ihren API-Schlüssel sicher zu speichern.  

#### <a name="get-the-client-library-and-example"></a>Abrufen der Clientbibliothek und des Beispiels  
Sie können die Emotionen-API-Clientbibliothek über [SDK](https://www.github.com/microsoft/cognitive-emotion-windows) herunterladen. Die heruntergeladene ZIP-Datei muss in einen Ordner Ihrer Wahl extrahiert werden. Viele Benutzer wählen hierfür den Visual Studio 2015-Ordner.
## <a name="Step1">Schritt 1: Öffnen des Beispiels</a>
1.  Starten Sie Microsoft Visual Studio 2015, klicken Sie auf **Datei**, und wählen Sie **Öffnen** und dann **Projekt/Projektmappe** aus.
2.  Navigieren Sie zu dem Ordner, in dem Sie die heruntergeladenen Dateien für die Emotionen-API gespeichert haben. Klicken Sie auf **Emotionen**, **Windows** und den **WPF-Beispiel**-Ordner.
3.  Doppelklicken Sie auf die SLN-Datei **EmotionAPI-WPF-Samples.sln** (eine Visual Studio 2015-Projektmappe). Damit wird die Projektmappe in Visual Studio geöffnet.

## <a name="Step2">Schritt 2: Erstellen des Beispiels</a>
1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Verweise**, und wählen Sie **NuGet-Pakete verwalten** aus.

  ![Öffnen des NuGet-Paket-Managers](../Images/EmotionNuget.png)

2.  Das Fenster **NuGet-Paket-Manager** wird geöffnet. Wählen Sie zuerst **Durchsuchen** in der oberen linken Ecke aus, geben Sie dann „Newtonsoft.Json“ in das Suchfeld ein, wählen Sie das Paket **Newtonsoft.Json** aus, und klicken Sie auf **Installieren**.  

  ![Navigieren zum NuGet-Paket](../Images/EmotionNugetBrowse.png)  

3.  Drücken Sie STRG+UMSCHALT+B, oder klicken Sie auf dem Menüband auf **Erstellen** und anschließend auf **Projektmappe erstellen**.

## <a name="Step3">Schritt 3: Ausführen des Beispiels</a>
1.  Drücken Sie nach Abschluss des Buildvorgangs**F5**, oder klicken Sie auf dem Menüband auf **Starten**, um das Beispiel auszuführen.
2.  Navigieren Sie zum Fenster der Emotionen-API mit dem **Textfeld** **Abonnementschlüssel hier einfügen, um zu beginnen**. Fügen Sie Ihren Abonnementschlüssel in das Textfeld ein, wie im folgenden Screenshot gezeigt. Durch Klicken auf „Schlüssel speichern“ können Sie den Abonnementschlüssel auf Ihrem PC oder Laptop speichern. Wenn Sie den Abonnementschlüssel aus dem System löschen möchten, klicken Sie auf „Schlüssel löschen“, um ihn von Ihrem PC oder Laptop zu entfernen.

  ![Emotionen-Funktionalitätsschnittstelle](../Images/EmotionKey.png)

3.  Klicken Sie unter „**Szenario auswählen**“ auf eines der beiden Szenarien, um es zu verwenden – „**Emotionen mithilfe eines Datenstroms erkennen** „ oder „**Emotionen mithilfe einer URL erkennen**", und befolgen Sie die Anweisungen auf dem Bildschirm. Microsoft empfängt die Bilder, die Sie hochladen, und nutzt sie ggf. zur Verbesserung der Emotionen-API und der dazugehörigen Dienste. Wenn Sie ein Bild übermitteln, bestätigen Sie dadurch, dass Sie sich an unsere [Verhaltensregeln für Entwickler](https://azure.microsoft.com/support/legal/developer-code-of-conduct/) gehalten haben.
4.  Für diese Beispielanwendung stehen Beispielbilder zur Verfügung. Diese befinden sich im Ordner **Data** des [GitHub-Repositorys für die Gesichtserkennungs-API](https://github.com/Microsoft/Cognitive-Face-Windows/tree/master/Data). Bitte beachten Sie, dass die Verwendung dieser Bilder im Rahmen einer Fair Use-Vereinbarung lizenziert ist, was bedeutet, dass sie zum Testen dieses Beispiel verwendet werden können, aber nicht zum erneuten Veröffentlichen.

## <a name="Review">Ansehen und Lernen</a>
Nachdem Sie nun über eine aktive Anwendung verfügen, erfahren Sie als Nächstes, wie sich die Integration zwischen dieser Beispiel-App und Microsoft Cognitive Services gestaltet. Auf der Grundlage dieser Informationen können Sie entweder diese App weiterentwickeln oder Ihre eigene App mit der Emotionen-API von Microsoft entwickeln.

Diese Beispiel-App nutzt die Clientbibliothek der Emotionen-API. Dabei handelt es sich um einen C#-Thin Client-Wrapper für die Emotionen-API von Microsoft. Wenn Sie die Beispiel-App wie oben beschrieben erstellt haben, haben Sie die Clientbibliothek aus einem NuGet-Paket erhalten. Der Quellcode der Clientbibliothek befindet sich im Ordner „[Client Library](https://github.com/Microsoft/Cognitive-Emotion-Windows/tree/master/ClientLibrary)“ unter **Emotionen** > **Windows** > **Client Library**. Dieser Ordner ist Teil des heruntergeladenen Dateirepositorys, das unter [Voraussetzungen](#Prerequisites) erwähnt wurde.

Darüber hinaus können Sie sich mit der Verwendung des Clientbibliothekcodes im **Projektmappen-Explorer** vertraut machen: Erweitern Sie unter **EmotionAPI-WPF_Samples** die Option **DetectEmotionUsingStreamPage.xaml**, und suchen Sie nach der Datei **DetectEmotionUsingStreamPage.xaml.cs** (wird verwendet, um zu einer lokal gespeicherten Datei zu navigieren), oder erweitern Sie **DetectEmotionUsingURLPage.xaml**, und suchen Sie nach der Datei  **DetectEmotionUsingURLPage.xaml.cs** (wird beim Hochladen einer Bild-URL verwendet). Doppelklicken Sie auf die XAML.CS-Dateien, um sie in neuen Visual Studio-Fenstern zu öffnen.

Bei der Untersuchung, wie die Emotionen-Clientbibliothek in unserer Beispiel-App verwendet wird, betrachten wir zwei Codeausschnitte aus **DetectEmotionUsingStreamPage.xaml.cs** und **DetectEmotionUsingURLPage.xaml.cs**. Jede Datei enthält die Codekommentare „KEY SAMPLE CODE STARTS HERE“ und „KEY SAMPLE CODE ENDS HERE“, die den Beginn und das Ende des betreffenden Beispielcodes angeben, damit Sie die weiter unten reproduzierten Codeausschnitte leichter finden.

Die Emotionen-API kann als Eingabe eine Bild-URL oder binäre Bilddaten (in Form eines Oktett-Streams) verwenden. Im Folgenden werden die beiden Optionen untersucht. In beiden Fällen befindet sich am Anfang eine using-Direktive, die die Verwendung der Clientbibliothek für Emotionen ermöglicht.
```csharp

            // -----------------------------------------------------------------------
            // KEY SAMPLE CODE STARTS HERE
            // Use the following namespace for EmotionServiceClient
            // -----------------------------------------------------------------------
            using Microsoft.ProjectOxford.Emotion;
            using Microsoft.ProjectOxford.Emotion.Contract;
            // -----------------------------------------------------------------------
            // KEY SAMPLE CODE ENDS HERE
            // -----------------------------------------------------------------------
```
#### <a name="detectemotionusingurlpagexamlcs"></a>DetectEmotionUsingURLPage.xaml.cs

Dieser Codeausschnitt zeigt, wie Sie mithilfe der Clientbibliothek Ihren Abonnementschlüssel und eine Foto-URL an den Emotionen-API-Dienst übermitteln.

```csharp

            // -----------------------------------------------------------------------
            // KEY SAMPLE CODE STARTS HERE
            // -----------------------------------------------------------------------

            window.Log("EmotionServiceClient is created");

            //
            // Create Project Oxford Emotion API Service client
            //
            EmotionServiceClient emotionServiceClient = new EmotionServiceClient(subscriptionKey);

            window.Log("Calling EmotionServiceClient.RecognizeAsync()...");
            try
            {
                //
                // Detect the emotions in the URL
                //
                Emotion[] emotionResult = await emotionServiceClient.RecognizeAsync(url);
                return emotionResult;
            }
            catch (Exception exception)
            {
                window.Log("Detection failed. Please make sure that you have the right subscription key and proper URL to detect.");
                window.Log(exception.ToString());
                return null;
            }
            // -----------------------------------------------------------------------
            // KEY SAMPLE CODE ENDS HERE
            // -----------------------------------------------------------------------
```
#### <a name="detectemotionusingstreampagexamlcs"></a>DetectEmotionUsingStreamPage.xaml.cs

Unten wird gezeigt, wie Ihr Abonnementschlüssel und ein lokal gespeichertes Bild an die Emotionen-API übermittelt werden.


```csharp


            // -----------------------------------------------------------------------
            // KEY SAMPLE CODE STARTS HERE
            // -----------------------------------------------------------------------

            //
            // Create Project Oxford Emotion API Service client
            //
            EmotionServiceClient emotionServiceClient = new EmotionServiceClient(subscriptionKey);

            window.Log("Calling EmotionServiceClient.RecognizeAsync()...");
            try
            {
                Emotion[] emotionResult;
                using (Stream imageFileStream = File.OpenRead(imageFilePath))
                {
                    //
                    // Detect the emotions in the URL
                    //
                    emotionResult = await emotionServiceClient.RecognizeAsync(imageFileStream);
                    return emotionResult;
                }
            }
            catch (Exception exception)
            {
                window.Log(exception.ToString());
                return null;
            }
            // -----------------------------------------------------------------------
            // KEY SAMPLE CODE ENDS HERE
            // -----------------------------------------------------------------------
```
<!--
## <a name="Related">Related Topics</a>
[Emotion API Overview](.)
-->
