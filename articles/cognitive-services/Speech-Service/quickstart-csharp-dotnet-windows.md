---
title: 'Schnellstart: Erkennen von Sprache in C# unter .NET Framework für Windows mit dem Cognitive Services Speech SDK'
titleSuffix: Microsoft Cognitive Services
description: Hier erfahren Sie, wie die Spracherkennung in C# unter .NET Framework für Windows mit dem Cognitive Services Speech SDK funktioniert.
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/16/2018
ms.author: wolfma
ms.openlocfilehash: 20d0f4736d3aa5f23d9061b4482da7a623862344
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2018
ms.locfileid: "43128220"
---
# <a name="quickstart-recognize-speech-in-c-under-net-framework-on-windows-using-the-speech-sdk"></a>Schnellstart: Erkennen von Sprache in C# unter .NET Framework für Windows mit dem Speech SDK

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Dieser Artikel beschreibt, wie Sie eine C#-Konsolen-App für .NET Framework unter Windows mit dem Cognitive Services Speech SDK erstellen, um Spracherkennung zu transkribieren.
Die Anwendung basiert auf dem [NuGet-Paket für das Microsoft Cognitive Services Speech SDK](https://aka.ms/csspeech/nuget) und auf Microsoft Visual Studio 2017.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Abonnementschlüssel für den Spracherkennungsdienst. Mehr erfahren Sie unter [Kostenloses Testen des Spracherkennungsdiensts](get-started.md).
* Ein Windows-PC mit einem funktionierenden Mikrofon.
* Visual Studio 2017 Community Edition oder höher
* Die **.NET-Desktopentwicklung**-Workload in Visual Studio. Sie können sie unter **Extras** \> **Tools und Features abrufen** aktivieren.

  ![Aktivieren der .NET-Desktopentwicklung](media/sdk/vs-enable-net-desktop-workload.png)

  ![Aktivieren der plattformübergreifenden .NET Core-Entwicklung](media/sdk/vs-enable-net-desktop-workload.png)

## <a name="create-a-visual-studio-project"></a>Erstellen eines Visual Studio-Projekts

1. Erstellen Sie in Visual Studio 2017 eine neue Visual C#-Konsolenanwendung. Erweitern Sie im Dialogfeld links **Neues Projekt** **Installiert** \> **Visual C#** \> **Windows Desktop**, und wählen Sie dann **Konsolen-App (.NET Framework)** aus. Geben Sie als Projektnamen *helloworld* ein.

    ![Erstellen einer Visual C#-Konsolen-App (.NET Framework)](media/sdk/qs-csharp-dotnet-windows-01-new-console-app.png "Erstellen einer Visual C#-Konsolen-App (.NET Framework)")

1. Installieren Sie das [Speech-SDK-NuGet-Paket](https://aka.ms/csspeech/nuget). Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf die Projektmappe, und wählen Sie dann **NuGet-Pakete für Projektmappe verwalten** aus.

    ![Klicken Sie mit der rechten Maustaste auf „NuGet-Pakete für Projektmappen verwalten“](media/sdk/qs-csharp-dotnet-windows-02-manage-nuget-packages.png "NuGet-Pakete für Projektmappe verwalten")

1. Wählen Sie rechts oben im Feld **Paketquelle** die Option **Nuget.org** aus. Suchen Sie nach dem Paket `Microsoft.CognitiveServices.Speech`, und installieren Sie es im Projekt **helloworld**.

    ![Installieren Sie das Microsoft.CognitiveServices.Speech-NuGet-Paket](media/sdk/qs-csharp-dotnet-windows-03-nuget-install-0.5.0.png "Nuget-Paket installieren")

1. Akzeptieren Sie die angezeigte Lizenz.

    ![Akzeptieren der Lizenzbedingungen](media/sdk/qs-csharp-dotnet-windows-04-nuget-license.png "Akzeptieren der Lizenzbedingungen")

1. Die folgende Ausgabezeile wird in der Paket-Manager-Konsole angezeigt.

   ```text
   Successfully installed 'Microsoft.CognitiveServices.Speech 0.6.0' to helloworld
   ```

## <a name="create-a-platform-configuration-matching-your-pc-architecture"></a>Erstellen Sie eine Plattformkonfiguration, die mit Ihrer PC-Architektur übereinstimmt

In diesem Abschnitt fügen Sie der Konfiguration eine neue Plattform hinzu, die mit Ihrer Prozessorarchitektur übereinstimmt.

1. Starten Sie den Konfigurations-Manager. Wählen Sie **Build** > **Konfigurations-Manager** aus.

    ![Starten des Konfigurations-Managers](media/sdk/qs-csharp-dotnet-windows-05-cfg-manager-click.png "Starten des Konfigurations-Managers")

1. Fügen Sie im Dialogfeld **Konfigurations-Manager** eine neue Plattform hinzu. Wählen Sie in der Dropdownliste **Aktive Projektmappenplattform** den Eintrag **Neu** aus.

    ![Hinzufügen einer neuen Plattform im Fenster des Konfigurations-Managers](media/sdk/qs-csharp-dotnet-windows-06-cfg-manager-new.png "Hinzufügen einer neuen Plattform im Fenster des Konfigurations-Managers")

1. Wenn Sie Windows 64-Bit ausführen, erstellen Sie eine neue Plattformkonfiguration mit dem Namen `x64`. Wenn Sie Windows 32-Bit ausführen, erstellen Sie eine neue Plattformkonfiguration mit dem Namen `x86`. In diesem Artikel erstellen Sie eine `x64`-Plattformkonfiguration.

    ![Fügen Sie unter Windows 64-Bit eine neue Plattform mit dem Namen „x64“ hinzu](media/sdk/qs-csharp-dotnet-windows-07-cfg-manager-add-x64.png "x64-Plattform hinzufügen")

## <a name="add-the-sample-code"></a>Hinzufügen des Beispielcodes

1. Öffnen Sie `Program.cs`, und ersetzen Sie den gesamten darin enthaltenen Code wie folgt.

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-dotnet-windows/helloworld/Program.cs#code)]

1. Ersetzen Sie die Zeichenfolge `YourSubscriptionKey` durch Ihren Abonnementschlüssel.

1. Ersetzen Sie die Zeichenfolge `YourServiceRegion` durch die [Region](regions.md), die mit Ihrem Abonnement verknüpft ist (z. B. `westus` für das kostenlose Testabonnement).

1. Speichern Sie die Änderungen am Projekt.

## <a name="build-and-run-the-sample"></a>Erstellen und Ausführen des Beispiels

1. Erstellen Sie die Anwendung. Wählen Sie in der Menüleiste **Build** > **Projektmappe erstellen** aus. Der Code sollte nun ohne Fehler kompiliert werden.

    ![Erfolgreicher Build](media/sdk/qs-csharp-dotnet-windows-08-build.png "Erfolgreicher Build")

1. Starten Sie die Anwendung. Wählen Sie in der Menüleiste **Debuggen** > **Debuggen starten** aus, oder drücken Sie **F5**.

    ![Starten der Anwendung im Debuggen](media/sdk/qs-csharp-dotnet-windows-09-start-debugging.png "Starten der Anwendung im Debuggen")

1. Ein Konsolenfenster wird angezeigt, in dem Sie aufgefordert werden, (auf Englisch) zu sprechen. Der erkannte Text erscheint dann im gleichen Fenster.

    ![Konsolenausgabe nach erfolgreicher Erkennung](media/sdk/qs-csharp-dotnet-windows-10-console-output.png "Konsolenausgabe nach erfolgreicher Erkennung")

[!INCLUDE [Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Suchen Sie nach diesem Beispiel im Ordner `quickstart/csharp-dotnet-windows`.

## <a name="next-steps"></a>Nächste Schritte

- [Übersetzen von Sprache](how-to-translate-speech-csharp.md)
- [Tutorial: Erstellen eines benutzerdefinierten Akustikmodells](how-to-customize-acoustic-models.md)
- [Tutorial: Erstellen eines benutzerdefinierten Sprachmodells](how-to-customize-language-model.md)
