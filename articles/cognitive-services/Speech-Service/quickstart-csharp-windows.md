---
title: 'Schnellstart: Erkennen von Sprache mithilfe des Cognitive Services C#-Spracherkennungs-SDKs für Windows | Microsoft Docs'
description: Informationen zur Spracherkennung mithilfe des C#-SDKs für den Spracherkennungsdienst.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: wolfma
ms.openlocfilehash: 64281215d139731b61365936bc1b837798ad8fbf
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37016833"
---
# <a name="quickstart-recognize-speech-using-the-cognitive-services-speech-c-sdk"></a>Schnellstart: Erkennen von Sprache mithilfe des Cognitive Services C#-Spracherkennungs-SDKs

Dieser Artikel enthält Informationen zur Erstellung einer C#-Konsolenanwendung in Windows mithilfe des Cognitive Services-Spracherkennungs-SDKs zum Umwandeln von Sprache in Text.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Abonnementschlüssel für den Spracherkennungsdienst. Mehr erfahren Sie unter [Kostenloses Testen des Spracherkennungsdiensts](get-started.md).
* Visual Studio 2017 Community Edition oder höher
* Die **.NET-Desktopentwicklung**-Workload in Visual Studio. Sie können sie unter **Extras** \> **Tools und Features abrufen** aktivieren. 

## <a name="create-a-visual-studio-project"></a>Erstellen eines Visual Studio-Projekts

1. Erstellen Sie in Visual Studio 2017 eine neue Visual C#-Konsolenanwendung. Klappen Sie im Dialogfeld **Neues Projekt** auf der linken Seite **Installiert** auf, und wählen Sie dann **Konsolen-App (.NET Framework)** aus. Geben Sie als Projektnamen *CsharpHelloSpeech* ein.

    ![Erstellen einer Visual C#-Konsolen-App (.NET Framework)](media/sdk/speechsdk-05-vs-cs-new-console-app.png "Erstellen einer Visual C#-Konsolenanwendung")

2. Installieren Sie das [Speech-SDK-NuGet-Paket](https://aka.ms/csspeech/nuget). Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf die Projektmappe, und wählen Sie dann **NuGet-Pakete für Projektmappe verwalten** aus.

    ![Klicken Sie mit der rechten Maustaste auf „NuGet-Pakete für Projektmappen verwalten“](media/sdk/speechsdk-06-vs-cs-manage-nuget-packages.png "NuGet-Pakete für Projektmappe verwalten")

3. Wählen Sie rechts oben im Feld **Paketquelle** die Option **Nuget.org** aus. Suchen Sie nach dem Paket `Microsoft.CognitiveServices.Speech`, und installieren Sie es im **CsharpHelloSpeech**-Projekt.

    ![Installieren Sie das Microsoft.CognitiveServices.Speech-NuGet-Paket](media/sdk/speechsdk-08-vs-cs-nuget-install.png "Nuget-Paket installieren")

4. Stimmen Sie auf dem dann angezeigten Lizenzbildschirm den Lizenzbedingungen zu:

    ![Akzeptieren der Lizenzbedingungen](media/sdk/speechsdk-09-vs-cs-nuget-license.png "Akzeptieren der Lizenzbedingungen")

## <a name="create-a-platform-configuration-matching-your-pc-architecture"></a>Erstellen Sie eine Plattformkonfiguration, die mit Ihrer PC-Architektur übereinstimmt

In diesem Abschnitt fügen Sie der Konfiguration eine neue Plattform hinzu, die mit Ihrer Prozessorarchitektur übereinstimmt.

1. Starten Sie den Konfigurations-Manager. Wählen Sie **Build** > **Konfigurations-Manager** aus.

    ![Starten des Konfigurations-Managers](media/sdk/speechsdk-12-vs-cs-cfg-manager-click.png "Starten des Konfigurations-Managers")

2. Fügen Sie im Dialogfeld **Konfigurations-Manager** eine neue Plattform hinzu. Wählen Sie in der Dropdownliste **Aktive Projektmappenplattform** den Eintrag **Neu** aus.

    ![Hinzufügen einer neuen Plattform im Fenster des Konfigurations-Managers](media/sdk/speechsdk-14-vs-cs-cfg-manager-new.png "Hinzufügen einer neuen Plattform im Fenster des Konfigurations-Managers")

3. Wenn Sie Windows 64-Bit ausführen, erstellen Sie eine neue Plattformkonfiguration mit dem Namen `x64`. Wenn Sie Windows 32-Bit ausführen, erstellen Sie eine neue Plattformkonfiguration mit dem Namen `x86`. In diesem Artikel erstellen Sie eine `x64`-Plattformkonfiguration. 

    ![Fügen Sie unter Windows 64-Bit eine neue Plattform mit dem Namen „x64“ hinzu](media/sdk/speechsdk-15-vs-cs-cfg-manager-add-x64.png "x64-Plattform hinzufügen")

## <a name="add-the-sample-code"></a>Hinzufügen des Beispielcodes

1. Ersetzen Sie in der `Program.cs` für Ihr Visual Studio-Projekt den Textkörper der `Program`-Klasse durch den folgenden Code. Achten Sie darauf, den Abonnementschlüssel und die Region durch die Werte zu ersetzen, die Sie für den Dienst erhalten haben.

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/Windows/quickstart-csharp/Program.cs#code)]

2. Nach dem Einfügen des Codes muss die `Main()`-Methode ähnlich wie die Darstellung im folgenden Screenshot sein:

    ![Main-Methode nach dem Einfügen des Codes](media/sdk/speechsdk-17-vs-cs-paste-code.png "Endgültige Main-Methode")

3. Das IntelliSense-Feature von Visual Studio hebt die Verweise auf die Klassen des Spracherkennungs-SDKs hervor, die nicht aufgelöst werden konnten. Um dieses Problem zu beheben, fügen Sie am Anfang des Codes die folgende `using`-Anweisung hinzu (entweder manuell oder mithilfe der [schnellen Aktionen](https://docs.microsoft.com/visualstudio/ide/quick-actions) von Visual Studio).

    [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/Windows/quickstart-csharp/Program.cs#usingstatement)]

    ![Verwenden der schnellen Aktion zum Hinzufügen der fehlenden using-Anweisung](media/sdk/speechsdk-18-vs-cs-add-using.png "Beheben von IntelliSense-Problemen")

4. Vergewissern Sie sich, dass die IntelliSense-Hervorhebung behoben wurde, und speichern Sie die Änderungen am Projekt.

## <a name="build-and-run-the-sample"></a>Erstellen und Ausführen des Beispiels

1. Erstellen Sie die Anwendung. Wählen Sie in der Menüleiste **Build** > **Projektmappe erstellen** aus. Der Code sollte nun ohne Fehler kompiliert werden:

    ![Erfolgreicher Build](media/sdk/speechsdk-20-vs-cs-build.png "Erfolgreicher Build")

2. Starten Sie die Anwendung. Wählen Sie in der Menüleiste **Debuggen** > **Debuggen starten** aus, oder drücken Sie **F5**. 

    ![Starten der Anwendung im Debuggen](media/sdk/speechsdk-21-vs-cs-f5.png "Starten der Anwendung im Debuggen")

3. Ein Konsolenfenster wird angezeigt, in dem Sie aufgefordert werden, (auf Englisch) zu sprechen.
Das Ergebnis der Erkennung wird auf dem Bildschirm angezeigt.

    ![Konsolenausgabe nach erfolgreicher Erkennung](media/sdk/speechsdk-22-cs-vs-console-output.png "Konsolenausgabe nach erfolgreicher Erkennung")

## <a name="download-code"></a>Code herunterladen

Die Sammlung der aktuellsten Beispiele finden Sie im [Cognitive Services Speech SDK Sample-GitHub-Repository](https://aka.ms/csspeech/samples).

## <a name="next-steps"></a>Nächste Schritte

- [Übersetzen von Sprache](how-to-translate-speech.md)
- [Anpassen von Sprachmodellen](how-to-customize-speech-models.md)
