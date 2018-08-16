---
title: 'Schnellstart: Erkennen von Sprache in C# in einer UWP-App mit dem Cognitive Services Speech SDK'
titleSuffix: Microsoft Cognitive Services
description: Erfahren Sie, wie Sie Sprache in einer UWP-App mit dem Cognitive Services Speech SDK erkennen können.
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/16/2018
ms.author: wolfma
ms.openlocfilehash: d1245b07ac0de680c13542b9af86b25bdf517c21
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2018
ms.locfileid: "39576133"
---
# <a name="quickstart-recognize-speech-in-a-uwp-app-using-the-speech-sdk"></a>Schnellstart: Erkennen von Sprache in einer UWP-App mit dem Speech SDK

[!include[Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Dieser Artikel enthält Informationen zur Erstellung einer UWP-Anwendung (Universelle Windows-Plattform) mithilfe des Cognitive Services Speech SDK zum Umwandeln von Sprache in Text.
Die Anwendung basiert auf dem [NuGet-Paket für das Microsoft Cognitive Services Speech SDK](https://aka.ms/csspeech/nuget) und auf Microsoft Visual Studio 2017.

> [!NOTE]
> Mithilfe der universellen Windows-Plattform können Sie Apps entwickeln, die auf einem beliebigen Gerät mit Unterstützung für Windows 10 (z.B. PCs, Xbox, Surface Hub und andere Geräte) ausgeführt werden. Apps, bei denen das Speech SDK verwendet wird, erfüllen noch nicht die Anforderungen des Zertifizierungskits für Windows-Apps (Windows App Certification Kit, WACK). Ihre App kann quergeladen werden, zurzeit jedoch nicht an den Microsoft Store übermittelt werden.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Abonnementschlüssel für den Spracherkennungsdienst. Mehr erfahren Sie unter [Kostenloses Testen des Spracherkennungsdiensts](get-started.md).
* Ein Windows-PC (Windows 10 Fall Creators Update oder höher) mit kompatiblem Mikrofon
* [Microsoft Visual Studio 2017](https://www.visualstudio.com/) Community Edition oder höher
* Die Workload zur **Entwicklung für die universelle Windows-Plattform** in Visual Studio. Diese können Sie unter **Tools** \> **Tools und Features abrufen** aktivieren.

  ![Aktivieren der Entwicklung für die universelle Windows-Plattform](media/sdk/vs-enable-uwp-workload.png)

## <a name="create-a-visual-studio-project"></a>Erstellen eines Visual Studio-Projekts

1. Erstellen Sie in Visual Studio 2017 eine neue leere Visual C#-App (Universelle Windows-App). Erweitern Sie im Dialogfeld **Neues Projekt** im linken Bereich **Installiert** \> **Visual C#** \> **Windows/universell**, und wählen Sie dann **Leere App (Universelle Windows-App)** aus. Geben Sie als Projektnamen *helloworld* ein.

    ![](media/sdk/qs-csharp-uwp-01-new-blank-app.png)

1. Wählen Sie im Fenster **Neues UWP-Projekt (Universelle Windows-Plattform)**, das angezeigt wird, **Windows 10 Fall Creators Update (10.0, Build 16299)** als **Mindestversion** und diese oder eine höhere Version als **Zielversion** aus, und klicken Sie dann auf **OK**.

    ![](media/sdk/qs-csharp-uwp-02-new-uwp-project.png)

1. Wenn Sie eine 64-Bit-Windows-Installation ausführen, können Sie die Buildplattform in `x64` ändern.

   ![Ändern der Buildplattform in x64](media/sdk/qs-csharp-uwp-03-switch-to-x64.png)

   > [!NOTE]
   > Gegenwärtig unterstützt das Speech SDK Intel-kompatible Prozessoren, jedoch keine ARM-Prozessoren.

1. Installieren Sie das [Speech-SDK-NuGet-Paket](https://aka.ms/csspeech/nuget). Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf die Projektmappe, und wählen Sie dann **NuGet-Pakete für Projektmappe verwalten** aus.

    ![Rechtsklick auf „NuGet-Pakete für Projektmappe verwalten“](media/sdk/qs-csharp-uwp-04-manage-nuget-packages.png)

1. Wählen Sie rechts oben im Feld **Paketquelle** die Option **Nuget.org** aus. Suchen Sie nach dem Paket `Microsoft.CognitiveServices.Speech`, und installieren Sie es im Projekt **helloworld**.

    ![Installieren Sie das Microsoft.CognitiveServices.Speech-NuGet-Paket](media/sdk/qs-csharp-uwp-05-nuget-install-0.5.0.png "Nuget-Paket installieren")

1. Akzeptieren Sie die angezeigte Lizenz.

    ![Akzeptieren der Lizenzbedingungen](media/sdk/qs-csharp-uwp-06-nuget-license.png "Akzeptieren der Lizenzbedingungen")

1. Die folgende Ausgabezeile wird in der Paket-Manager-Konsole angezeigt.

   ```text
   Successfully installed 'Microsoft.CognitiveServices.Speech 0.5.0' to helloworld
   ```

## <a name="add-the-sample-code"></a>Hinzufügen des Beispielcodes

1. Doppelklicken Sie im Projektmappen-Explorer auf **Package.appxmanifest**, um Ihr Anwendungsmanifest zu bearbeiten.
   Wählen Sie die Registerkarte **Funktionen** aus, aktivieren Sie das Kontrollkästchen für die **Mikrofon**-Funktion, und speichern Sie Ihre Änderungen.

   ![](media/sdk/qs-csharp-uwp-07-capabilities.png)

1. Bearbeiten Sie die Benutzeroberfläche Ihrer Anwendung durch Doppelklicken auf `MainPage.xaml` im Projektmappen-Explorer. 

    Fügen Sie in der XAML-Ansicht des Designers den folgenden XAML-Ausschnitt in das Grid-Tag (zwischen `<Grid>` und `</Grid>`) ein.

   [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/csharp-uwp/helloworld/MainPage.xaml#StackPanel)]

1. Bearbeiten Sie das XAML-CodeBehind durch Doppelklicken auf `MainPage.xaml.cs` im Projektmappen-Explorer (gruppiert unter dem Element `MainPage.xaml`).
   Ersetzen Sie den Code in dieser Datei durch die folgenden Codezeilen:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-uwp/helloworld/MainPage.xaml.cs#code)]

1. Ersetzen Sie im Handler `SpeechRecognitionFromMicrophone_ButtonClicked` die Zeichenfolge `YourSubscriptionKey` durch Ihren Abonnementschlüssel.

1. Ersetzen Sie im Handler `SpeechRecognitionFromMicrophone_ButtonClicked` die Zeichenfolge `YourServiceRegion` durch die [Region](regions.md), die mit Ihrem Abonnement verknüpft ist (z.B. `westus` für das kostenlose Testabonnement).

1. Speichern Sie alle Änderungen am Projekt.

## <a name="build-and-run-the-sample"></a>Erstellen und Ausführen des Beispiels

1. Erstellen Sie die Anwendung. Wählen Sie in der Menüleiste **Build** > **Projektmappe erstellen** aus. Der Code sollte nun ohne Fehler kompiliert werden.

    ![Erfolgreicher Build](media/sdk/qs-csharp-uwp-08-build.png "Erfolgreicher Build")

1. Starten Sie die Anwendung. Wählen Sie in der Menüleiste **Debuggen** > **Debuggen starten** aus, oder drücken Sie **F5**.

    ![Starten der Anwendung im Debuggen](media/sdk/qs-csharp-uwp-09-start-debugging.png "Starten der Anwendung im Debuggen")

1. Ein Fenster der grafischen Benutzeroberfläche wird angezeigt. Klicken Sie zuerst auf die Schaltfläche **Mikrofon aktivieren**, und bestätigen Sie die Berechtigungsanforderung, die angezeigt wird.

    ![Starten der Anwendung im Debuggen](media/sdk/qs-csharp-uwp-10-access-prompt.png "Starten der Anwendung im Debuggen")

1. Klicken Sie auf **Spracherkennung durch Mikrofoneingabe**, und sprechen Sie einen kurzen Ausdruck in das Mikrofon Ihres Geräts. Der erkannte Text wird im Fenster angezeigt.

    ![](media/sdk/qs-csharp-uwp-11-ui-result.png)

[!include[Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Suchen Sie nach diesem Beispiel im Ordner `quickstart/csharp-uwp`.

## <a name="next-steps"></a>Nächste Schritte

- [Übersetzen von Sprache](how-to-translate-speech-csharp.md)
- [Tutorial: Erstellen eines benutzerdefinierten Akustikmodells](how-to-customize-acoustic-models.md)
- [Tutorial: Erstellen eines benutzerdefinierten Sprachmodells](how-to-customize-language-model.md)
