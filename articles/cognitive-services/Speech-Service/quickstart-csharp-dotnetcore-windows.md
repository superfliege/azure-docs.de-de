---
title: 'Schnellstart: Erkennen von Sprache in C# unter .NET Core für Windows mit dem Cognitive Services Speech SDK'
titleSuffix: Microsoft Cognitive Services
description: Hier erfahren Sie, wie die Spracherkennung in C# unter .NET Core für Windows mit dem Cognitive Services Speech SDK funktioniert.
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/16/2018
ms.author: wolfma
ms.openlocfilehash: 7790920b6553ba0e2738d693710bfc3a1d3b4f89
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/27/2018
ms.locfileid: "39325100"
---
# <a name="quickstart-recognize-speech-in-c-under-net-core-on-windows-using-the-speech-sdk"></a>Schnellstart: Erkennen von Sprache in C# unter .NET Core für Windows mit dem Speech SDK

[!include[Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Dieser Artikel beschreibt, wie Sie eine C#-Konsolen-App für .NET Core unter Windows mit dem Cognitive Services Speech SDK erstellen, um Spracherkennung zu transkribieren.
Die Anwendung umfasst das [NuGet-Paket für das Microsoft Cognitive Services Speech SDK](https://aka.ms/csspeech/nuget) und Microsoft Visual Studio 2017.

> [!NOTE]
> .NET Core ist eine Open Source- und plattformübergreifende .NET-Plattform, die die [.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard)-Spezifikation implementiert.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Abonnementschlüssel für den Spracherkennungsdienst. Mehr erfahren Sie unter [Kostenloses Testen des Spracherkennungsdiensts](get-started.md).
* Ein Windows-PC mit einem funktionierenden Mikrofon.
* [Microsoft Visual Studio 2017](https://www.visualstudio.com/) Community Edition oder höher.
* Die Workload **Plattformübergreifende .NET Core-Entwicklung** in Visual Studio. Sie können sie unter **Extras** \> **Tools und Features abrufen** aktivieren.

  ![Aktivieren der plattformübergreifenden .NET Core-Entwicklung](media/sdk/vs-enable-net-core-workload.png)

## <a name="create-a-visual-studio-project"></a>Erstellen eines Visual Studio-Projekts

1. Erstellen Sie in Visual Studio 2017 eine neue .NET Core-Konsolen-App in Visual C#. Erweitern Sie im Dialogfeld links **Neues Projekt** **Installiert** \> **Visual C#** \> **.NET Core**, und wählen Sie **Konsolen-App (.NET Core)** aus. Geben Sie als Projektnamen *helloworld* ein.

    ![Erstellen einer Visual C#-Konsolen-App (.NET Core)](media/sdk/qs-csharp-dotnetcore-windows-01-new-console-app.png "Erstellen einer Visual C#-Konsolen-App (.NET Core)")

1. Installieren Sie das [Speech-SDK-NuGet-Paket](https://aka.ms/csspeech/nuget). Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf die Projektmappe, und wählen Sie dann **NuGet-Pakete für Projektmappe verwalten** aus.

    ![Klicken Sie mit der rechten Maustaste auf „NuGet-Pakete für Projektmappen verwalten“](media/sdk/qs-csharp-dotnetcore-windows-02-manage-nuget-packages.png "NuGet-Pakete für Projektmappe verwalten")

1. Wählen Sie rechts oben im Feld **Paketquelle** die Option **Nuget.org** aus. Suchen Sie nach dem Paket `Microsoft.CognitiveServices.Speech`, und installieren Sie es im Projekt **helloworld**.

    ![Installieren Sie das Microsoft.CognitiveServices.Speech-NuGet-Paket](media/sdk/qs-csharp-dotnetcore-windows-03-nuget-install-0.5.0.png "Nuget-Paket installieren")

1. Akzeptieren Sie die Lizenzbedingungen im angezeigten Dialogfeld.

    ![Akzeptieren der Lizenzbedingungen](media/sdk/qs-csharp-dotnetcore-windows-04-nuget-license.png "Akzeptieren der Lizenzbedingungen")

## <a name="add-the-sample-code"></a>Hinzufügen des Beispielcodes

1. Öffnen Sie `Program.cs` in Ihrem Visual Studio-Projekt und ersetzen Sie den gesamten Code in der Datei durch den folgenden.

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-dotnetcore-windows/helloworld/Program.cs#code)]

1. Ersetzen Sie die Zeichenfolge `YourSubscriptionKey` durch Ihren Abonnementschlüssel.

1. Ersetzen Sie die Zeichenfolge `YourServiceRegion` durch die [Region](regions.md), die mit Ihrem Abonnement verknüpft ist (z. B. `westus` für das kostenlose Testabonnement).

1. Speichern Sie die Änderungen am Projekt.

## <a name="build-and-run-the-sample"></a>Erstellen und Ausführen des Beispiels

1. Erstellen Sie die Anwendung. Wählen Sie in der Menüleiste **Erstellen** > **Projektmappe erstellen** aus. Der Code sollte nun ohne Fehler kompiliert werden.

    ![Erfolgreicher Build](media/sdk/qs-csharp-dotnetcore-windows-05-build.png "Erfolgreicher Build")

1. Starten Sie die Anwendung. Wählen Sie in der Menüleiste **Debuggen** > **Debuggen starten** aus, oder drücken Sie **F5**.

    ![Starten der Anwendung im Debuggen](media/sdk/qs-csharp-dotnetcore-windows-06-start-debugging.png "Starten der Anwendung im Debuggen")

1. Ein Konsolenfenster wird angezeigt, in dem Sie aufgefordert werden, (auf Englisch) zu sprechen. Der erkannte Text erscheint dann im gleichen Fenster.

    ![Konsolenausgabe nach erfolgreicher Erkennung](media/sdk/qs-csharp-dotnetcore-windows-07-console-output.png "Konsolenausgabe nach erfolgreicher Erkennung")

[!include[Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Suchen Sie nach diesem Beispiel im Ordner `quickstart/csharp-dotnetcore-windows`.

## <a name="next-steps"></a>Nächste Schritte

- [Übersetzen von Sprache](how-to-translate-speech-csharp.md)
- [Tutorial: Erstellen eines benutzerdefinierten Akustikmodells](how-to-customize-acoustic-models.md)
- [Tutorial: Erstellen eines benutzerdefinierten Sprachmodells](how-to-customize-language-model.md)
