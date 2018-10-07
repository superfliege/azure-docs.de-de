---
title: 'Schnellstart: Erkennen von Sprache in C# in einer UWP-App mit dem Cognitive Services Speech SDK'
titleSuffix: Microsoft Cognitive Services
description: Erfahren Sie, wie Sie Sprache in einer UWP-App mit dem Cognitive Services Speech SDK erkennen können.
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 09/24/2018
ms.author: wolfma
ms.openlocfilehash: 2eb6bda7066a01e5532fe7c0e20b0ee13f4289b6
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/28/2018
ms.locfileid: "47432986"
---
# <a name="quickstart-recognize-speech-in-a-uwp-app-by-using-the-speech-sdk"></a>Schnellstart: Erkennen von Sprache in einer UWP-App mit dem Speech SDK

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

In diesem Artikel erstellen Sie eine C#-UWP-Anwendung (Universelle Windows-Plattform) mithilfe des Cognitive Services [Speech SDK](speech-sdk.md). Sie transkribieren über das Mikrofon des Geräts Sprache in Echtzeit in Text. Die Anwendung basiert auf dem [NuGet-Paket für das Speech SDK](https://aka.ms/csspeech/nuget) und auf Microsoft Visual Studio 2017 (beliebige Edition).

> [!NOTE]
> Mithilfe der universellen Windows-Plattform können Sie Apps entwickeln, die auf einem beliebigen Gerät mit Unterstützung für Windows 10 (z.B. PCs, Xbox, Surface Hub und andere Geräte) ausgeführt werden.

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen einen Abonnementschlüssel für den Spracherkennungsdienst, um diesen Schnellstart abzuschließen. Sie können einen solchen Schlüssel kostenlos abrufen. Details dazu erfahren Sie unter [Kostenloses Testen des Spracherkennungsdiensts](get-started.md).

## <a name="create-a-visual-studio-project"></a>Erstellen eines Visual Studio-Projekts

1. Starten Sie Visual Studio 2017.

1. Stellen Sie sicher, dass die Workload **Entwicklung für die universelle Windows-Plattform** verfügbar ist. Wählen Sie **Tools** > **Tools und Features abrufen** in der Visual Studio-Menüleiste aus, um den Visual Studio-Installer zu öffnen. Wenn diese Workload bereits aktiviert ist, schließen Sie das Dialogfeld. 

    ![Screenshot des Visual Studio-Installers mit der hervorgehobenen Registerkarte „Workloads“](media/sdk/vs-enable-uwp-workload.png)

    Aktivieren Sie andernfalls das Kontrollkästchen neben **Plattformübergreifende .NET-Entwicklung**, und wählen Sie **Ändern** in der unteren rechten Ecke des Dialogfelds aus. Die Installation des neuen Features nimmt etwas Zeit in Anspruch.

1. Erstellen Sie eine leere universelle Visual C#-Windows-App. Wählen Sie zunächst **Datei** > **Neu** > **Projekt** aus dem Menü aus. Erweitern Sie im Dialogfeld **Neues Projekt** die Option **Installiert** > **Visual C#** > **Windows universell** im linken Bereich. Wählen Sie dann **Leere App (universelles Windows)** aus. Geben Sie als Projektnamen *helloworld* ein.

    ![Screenshot: Dialogfeld „Neues Projekt“](media/sdk/qs-csharp-uwp-01-new-blank-app.png)

1. Die Speech SDK erfordert, dass Ihre Anwendung für das Windows 10 Fall Creators Update oder höher erstellt wird. Wählen Sie im Fenster **Neues UWP-Projekt (Universelle Windows-Plattform)**, das angezeigt wird, **Windows 10 Fall Creators Update (10.0, Build 16299)** als **Mindestversion** aus. Wählen Sie im Feld **Zielversion** diese oder eine spätere Version aus, und klicken Sie dann auf **OK**.

    ![Screenshot des Fensters „Neues UWP-Projekt (Universelle Windows-Plattform)“](media/sdk/qs-csharp-uwp-02-new-uwp-project.png)

1. Wenn Sie 64-Bit-Windows ausführen, können Sie Ihre Buildplattform in `x64` ändern, indem Sie das Dropdownmenü auf der Visual Studio-Symbolleiste verwenden. (Mit 64-Bit-Windows können 32-Bit-Anwendungen ausgeführt werden, sodass Sie die Einstellung `x86` beibehalten können, wenn Sie möchten.)

   ![Screenshot der Visual Studio-Symbolleiste mit hervorgehobener Einstellung „x64“](media/sdk/qs-csharp-uwp-03-switch-to-x64.png)

   > [!NOTE]
   > Das Speech SDK unterstützt nur Intel-kompatible Prozessoren. ARM wird zurzeit nicht unterstützt.

1. Installieren Sie das [Speech-SDK-NuGet-Paket](https://aka.ms/csspeech/nuget). Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf die Projektmappe, und wählen Sie dann **NuGet-Pakete für Projektmappe verwalten** aus.

    ![Screenshot des Projektmappen-Explorers mit der hervorgehobenen Option „NuGet-Pakete für Projektmappe verwalten“](media/sdk/qs-csharp-uwp-04-manage-nuget-packages.png)

1. Wählen Sie rechts oben im Feld **Paketquelle** die Option **Nuget.org** aus. Suchen Sie nach dem Paket `Microsoft.CognitiveServices.Speech`, und installieren Sie es im Projekt **helloworld**.

    ![Screenshot des Dialogfelds „Pakete für Projektmappe verwalten](media/sdk/qs-csharp-uwp-05-nuget-install-1.0.0.png "NuGet-Paket installieren“")

1. Akzeptieren Sie die angezeigten Lizenzbedingungen, um die Installation des NuGet-Pakets zu starten.

    ![Screenshot des Dialogfelds zur Zustimmung zu den Lizenzbedingungen](media/sdk/qs-csharp-uwp-06-nuget-license.png "Lizenzbedingungen annehmen")

1. Die folgende Ausgabezeile wird in der Paket-Manager-Konsole angezeigt.

   ```text
   Successfully installed 'Microsoft.CognitiveServices.Speech 1.0.0' to helloworld
   ```

1. Da die Anwendung das Mikrofon für die Spracheingabe verwendet, fügen Sie dem Projekt die Funktion **Mikrofon** hinzu. Doppelklicken Sie im Projektmappen-Explorer auf **Package.appxmanifest**, um Ihr Anwendungsmanifest zu bearbeiten. Wechseln Sie dann auf die Registerkarte **Funktionen**, aktivieren Sie das Kontrollkästchen für die Funktion **Mikrofon**, und speichern Sie Ihre Änderungen.

   ![Screenshot des Visual Studio-Anwendungsmanifests mit hervorgehobenen Funktionen und Mikrofon](media/sdk/qs-csharp-uwp-07-capabilities.png)


## <a name="add-sample-code"></a>Hinzufügen von Beispielcode

1. Die Benutzeroberfläche der Anwendung wird mithilfe von XAML definiert. Öffnen Sie `MainPage.xaml` im Projektmappen-Explorer. Fügen Sie in der XAML-Ansicht des Designers den folgenden XAML-Ausschnitt in das Grid-Tag (zwischen `<Grid>` und `</Grid>`) ein.

   [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/csharp-uwp/helloworld/MainPage.xaml#StackPanel)]

1. Öffnen Sie die CodeBehind-Quelldatei `MainPage.xaml.cs` (sie ist unter `MainPage.xaml` gruppiert). Ersetzen Sie den gesamten darin enthaltenen Code wie folgt.

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-uwp/helloworld/MainPage.xaml.cs#code)]

1. Ersetzen Sie im `SpeechRecognitionFromMicrophone_ButtonClicked`-Handler in dieser Datei die Zeichenfolge `YourSubscriptionKey` durch Ihren Abonnementschlüssel.

1. Ersetzen Sie im Handler `SpeechRecognitionFromMicrophone_ButtonClicked` die Zeichenfolge `YourServiceRegion` durch die [Region](regions.md), die mit Ihrem Abonnement verknüpft ist (z.B. `westus` für das kostenlose Testabonnement).

1. Speichern Sie alle Änderungen am Projekt.

## <a name="build-and-run-the-app"></a>Erstellen und Ausführen der App

1. Erstellen Sie die Anwendung. Wählen Sie in der Menüleiste **Build** > **Projektmappe erstellen** aus. Der Code sollte nun ohne Fehler kompiliert werden.

    ![Screenshot der Visual Studio-Anwendung mit hervorgehobener Option „Projektmappe erstellen](media/sdk/qs-csharp-uwp-08-build.png "Erfolgreicher Build“")

1. Starten Sie die Anwendung. Wählen Sie in der Menüleiste **Debuggen** > **Debuggen starten** aus, oder drücken Sie **F5**.

    ![Screenshot der Visual Studio-Anwendung mit hervorgehobener Option „Debuggen starten](media/sdk/qs-csharp-uwp-09-start-debugging.png "Debuggen der App starten“")

1. Ein Fenster wird angezeigt. Wählen Sie **Mikrofon aktivieren** aus, und bestätigen Sie die Berechtigungsanforderung, die angezeigt wird.

    ![Screenshot der Berechtigungsanforderung](media/sdk/qs-csharp-uwp-10-access-prompt.png "Debuggen der App starten")

1. Wählen Sie **Spracherkennung durch Mikrofoneingabe** aus, und sprechen Sie einen englischen Ausdruck oder Satz in das Mikrofon Ihres Geräts. Ihre Spracheingabe wird an den Spracherkennungsdienst übermittelt und in Text transkribiert, der im Fenster angezeigt wird.

    ![Screenshot der Spracherkennungsbenutzeroberfläche](media/sdk/qs-csharp-uwp-11-ui-result.png)

[!INCLUDE [Download this sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Suchen Sie nach diesem Beispiel im Ordner `quickstart/csharp-uwp`.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erkennen von Absichten anhand von gesprochener Sprache mit dem Speech SDK für C#](how-to-recognize-intents-from-speech-csharp.md)

## <a name="see-also"></a>Weitere Informationen

- [Übersetzen von Sprache](how-to-translate-speech-csharp.md)
- [Tutorial: Erstellen eines benutzerdefinierten Akustikmodells](how-to-customize-acoustic-models.md)
- [Tutorial: Erstellen eines benutzerdefinierten Sprachmodells](how-to-customize-language-model.md)
