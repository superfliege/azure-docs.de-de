---
title: 'Schnellstart: Erkennen von Sprache in C# mit .NET Core unter Windows mit dem Cognitive Services Speech SDK'
titleSuffix: Microsoft Cognitive Services
description: Erfahren Sie, wie die Spracherkennung in C# mit .NET Core unter Windows mit dem Cognitive Services Speech SDK funktioniert.
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 09/24/2018
ms.author: wolfma
ms.openlocfilehash: 2cb89606986645d567136655d5ab3f07223ba70d
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/28/2018
ms.locfileid: "47434771"
---
# <a name="quickstart-recognize-speech-in-c-under-net-core-on-windows-by-using-the-speech-sdk"></a>Schnellstart: Erkennen von Sprache in C# mit .NET Core unter Windows mit dem Speech SDK

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

In diesem Artikel erstellen Sie eine C#-Konsolenanwendung für .NET Core unter Windows mithilfe des Cognitive Services [Speech SDK](speech-sdk.md). Sie transkribieren über das Mikrofon Ihres Computers Sprache in Echtzeit in Text. Die Anwendung basiert auf dem [NuGet-Paket für das Speech SDK](https://aka.ms/csspeech/nuget) und auf Microsoft Visual Studio 2017 (beliebige Edition).

> [!NOTE]
> .NET Core ist eine Open-Source- und plattformübergreifende .NET-Plattform, die die [.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard)-Spezifikation implementiert.

Sie benötigen einen Abonnementschlüssel für den Spracherkennungsdienst, um diesen Schnellstart abzuschließen. Sie können einen solchen Schlüssel kostenlos abrufen. Details dazu erfahren Sie unter [Kostenloses Testen des Spracherkennungsdiensts](get-started.md).


## <a name="create-a-visual-studio-project"></a>Erstellen eines Visual Studio-Projekts

1. Starten Sie Visual Studio 2017.

1. Stellen Sie sicher, dass die Workload **Plattformübergreifende .NET-Entwicklung** verfügbar ist. Wählen Sie **Tools** > **Tools und Features abrufen** in der Visual Studio-Menüleiste aus, um den Visual Studio-Installer zu öffnen. Wenn diese Workload bereits aktiviert ist, schließen Sie das Dialogfeld.

    ![Screenshot des Visual Studio-Installers mit der hervorgehobenen Registerkarte „Workloads“](media/sdk/vs-enable-net-core-workload.png)

    Aktivieren Sie andernfalls das Kontrollkästchen neben **Plattformübergreifende .NET Core-Entwicklung**, und wählen Sie **Ändern** in der unteren rechten Ecke des Dialogfelds aus. Die Installation des neuen Features nimmt etwas Zeit in Anspruch.

1. Erstellen Sie eine neue Visual C# .NET Core-Konsolen-App. Erweitern Sie im Dialogfeld **Neues Projekt** im linken Bereich **Installiert** > **Visual C#** > **.NET Core**. Wählen Sie dann **Konsolen-App (.NET Core)** aus. Geben Sie als Projektnamen *helloworld* ein.

    ![Screenshot des Dialogfelds „Neues Projekt“](media/sdk/qs-csharp-dotnetcore-windows-01-new-console-app.png "Visual C#-Konsolen-App erstellen (.NET Core)")

1. Installieren Sie das [Speech-SDK-NuGet-Paket](https://aka.ms/csspeech/nuget). Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf die Projektmappe, und wählen Sie dann **NuGet-Pakete für Projektmappe verwalten** aus.

    ![Screenshot des Projektmappen-Explorers mit der hervorgehobenen Option „NuGet-Pakete für Projektmappe verwalten“](media/sdk/qs-csharp-dotnetcore-windows-02-manage-nuget-packages.png "NuGet-Pakete für Projektmappe verwalten")

1. Wählen Sie rechts oben im Feld **Paketquelle** die Option **Nuget.org** aus. Suchen Sie nach dem Paket `Microsoft.CognitiveServices.Speech`, und installieren Sie es im Projekt **helloworld**.

    ![Screenshot des Dialogfelds „Pakete für Projektmappe verwalten](media/sdk/qs-csharp-dotnetcore-windows-03-nuget-install-1.0.0.png "NuGet-Paket installieren“")

1. Akzeptieren Sie die angezeigten Lizenzbedingungen, um die Installation des NuGet-Pakets zu starten.

    ![Screenshot des Dialogfelds zur Zustimmung zu den Lizenzbedingungen](media/sdk/qs-csharp-dotnetcore-windows-04-nuget-license.png "Lizenzbedingungen annehmen")

Nachdem das Paket installiert wurde, wird eine Bestätigung in der Paket-Manager-Konsole angezeigt.


## <a name="add-sample-code"></a>Hinzufügen von Beispielcode

1. Öffnen Sie `Program.cs`, und ersetzen Sie den gesamten in der Datei enthaltenen Code wie folgt.

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-dotnetcore-windows/helloworld/Program.cs#code)]

1. Ersetzen Sie in der gleichen Datei die Zeichenfolge `YourSubscriptionKey` durch Ihren Abonnementschlüssel.

1. Ersetzen Sie außerdem die Zeichenfolge `YourServiceRegion` durch die [Region](regions.md), die mit Ihrem Abonnement verknüpft ist (z.B. `westus` für das kostenlose Testabonnement).

1. Speichern Sie die Änderungen am Projekt.

## <a name="build-and-run-the-app"></a>Erstellen und Ausführen der App

1. Erstellen Sie die Anwendung. Wählen Sie in der Menüleiste **Erstellen** > **Projektmappe erstellen** aus. Der Code sollte ohne Fehler kompiliert werden.

    ![Screenshot der Visual Studio-Anwendung mit hervorgehobener Option „Projektmappe erstellen](media/sdk/qs-csharp-dotnetcore-windows-05-build.png "Erfolgreicher Build“")

1. Starten Sie die Anwendung. Wählen Sie in der Menüleiste **Debuggen** > **Debuggen starten** aus, oder drücken Sie **F5**.

    ![Screenshot der Visual Studio-Anwendung mit hervorgehobener Option „Debuggen starten](media/sdk/qs-csharp-dotnetcore-windows-06-start-debugging.png "Debuggen der App starten“")

1. Ein Konsolenfenster wird angezeigt, in dem Sie aufgefordert werden, etwas zu sagen. Sprechen Sie einen englischen Ausdruck oder Satz. Ihre Spracheingabe wird an den Spracherkennungsdienst übermittelt und in Text transkribiert, der im gleichen Fenster angezeigt wird.

    ![Screenshot der Konsolenausgabe nach erfolgreicher Erkennung](media/sdk/qs-csharp-dotnetcore-windows-07-console-output.png "Konsolenausgabe nach erfolgreicher Erkennung")

[!INCLUDE [Download this sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Suchen Sie nach diesem Beispiel im Ordner `quickstart/csharp-dotnetcore-windows`.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erkennen von Absichten anhand von gesprochener Sprache mit dem Speech SDK für C#](how-to-recognize-intents-from-speech-csharp.md)

## <a name="see-also"></a>Weitere Informationen

- [Übersetzen von Sprache](how-to-translate-speech-csharp.md)
- [Tutorial: Erstellen eines benutzerdefinierten Akustikmodells](how-to-customize-acoustic-models.md)
- [Tutorial: Erstellen eines benutzerdefinierten Sprachmodells](how-to-customize-language-model.md)
