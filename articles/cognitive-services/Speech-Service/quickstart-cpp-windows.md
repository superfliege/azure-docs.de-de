---
title: 'Schnellstart: Erkennen von Sprache in C++ unter Windows mit dem Cognitive Services Speech SDK'
titleSuffix: Microsoft Cognitive Services
description: Erfahren Sie, wie Sie Sprache in C++ unter Windows Desktop mit dem Cognitive Services Speech SDK erkennen können.
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.component: Speech
ms.topic: quickstart
ms.date: 10/12/2018
ms.author: wolfma
ms.openlocfilehash: 2adad78760c1d12da688106c45e86c91a8b13f91
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2018
ms.locfileid: "49339354"
---
# <a name="quickstart-recognize-speech-in-c-on-windows-by-using-the-speech-sdk"></a>Schnellstart: Erkennen von Sprache in C++ unter Windows mit dem Speech SDK

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

In diesem Artikel erstellen Sie eine C++-Konsolenanwendung für Windows. Sie verwenden das Cognitive Services [Speech SDK](speech-sdk.md), um Sprache über das Mikrofon Ihres Computers in Echtzeit in Text zu transkribieren. Die Anwendung basiert auf dem [NuGet-Paket für das Speech SDK](https://aka.ms/csspeech/nuget) und auf Microsoft Visual Studio 2017 (beliebige Edition).

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen einen Abonnementschlüssel für den Spracherkennungsdienst, um diesen Schnellstart abzuschließen. Sie können einen solchen Schlüssel kostenlos abrufen. Details dazu erfahren Sie unter [Kostenloses Testen des Spracherkennungsdiensts](get-started.md).

## <a name="create-a-visual-studio-project"></a>Erstellen eines Visual Studio-Projekts

1. Starten Sie Visual Studio 2017.

1. Stellen Sie sicher, dass die Workload **Desktopentwicklung mit C++** verfügbar ist. Wählen Sie **Tools** > **Tools und Features abrufen** in der Visual Studio-Menüleiste aus, um den Visual Studio-Installer zu öffnen. Wenn diese Workload bereits aktiviert ist, fahren Sie mit dem nächsten Schritt fort. 

    ![Screenshot der Registerkarte „Visual Studio-Workloads“](media/sdk/vs-enable-cpp-workload.png)

    Aktivieren Sie andernfalls das Kontrollkästchen neben **Desktopentwicklung mit C++**. 

1. Stellen Sie sicher, dass die Komponente **NuGet-Paket-Manager** verfügbar ist. Wechseln Sie zur Registerkarte **Einzelne Komponenten** des Dialogfelds „Visual Studio-Installer“, und wählen Sie **NuGet-Paket-Manager** aus, wenn diese Option nicht bereits aktiviert ist.

      ![Screenshot der Registerkarte „Einzelne Visual Studio-Komponenten“](media/sdk/vs-enable-nuget-package-manager.png)

1. Wenn Sie entweder die C++-Workload oder NuGet aktivieren mussten, wählen Sie **Ändern** (in der unteren rechten Ecke des Dialogfelds) aus. Die Installation der neuen Features nimmt etwas Zeit in Anspruch. Wenn beide Features bereits aktiviert wurden, schließen Sie stattdessen das Dialogfeld.

1. Erstellen Sie eine neue Windows-Konsolenanwendung in Visual C++ unter Windows Desktop. Wählen Sie zunächst **Datei** > **Neu** > **Projekt** aus dem Menü aus. Erweitern Sie im Dialogfeld **Neues Projekt** die Option **Installiert** > **Visual C++** > **Windows Desktop** im linken Bereich. Wählen Sie dann **Windows-Konsolenanwendung** aus. Geben Sie als Projektnamen *helloworld* ein.

    ![Screenshot: Dialogfeld „Neues Projekt“](media/sdk/qs-cpp-windows-01-new-console-app.png)

1. Wenn Sie 64-Bit-Windows ausführen, können Sie Ihre Buildplattform ggf. in `x64` ändern, indem Sie das Dropdownmenü auf der Visual Studio-Symbolleiste verwenden. (64-Bit-Versionen von Windows können 32-Bit-Anwendungen ausführen, daher ist dies nicht erforderlich.)

    ![Screenshot der Visual Studio-Symbolleiste mit hervorgehobener Option „x64“](media/sdk/qs-cpp-windows-02-switch-to-x64.png)

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf die Projektmappe, und wählen Sie dann **NuGet-Pakete für Projektmappe verwalten** aus.

    ![Screenshot des Projektmappen-Explorers mit der hervorgehobenen Option „NuGet-Pakete für Projektmappe verwalten“](media/sdk/qs-cpp-windows-03-manage-nuget-packages.png)

1. Wählen Sie rechts oben im Feld **Paketquelle** die Option **Nuget.org** aus. Suchen Sie nach dem Paket `Microsoft.CognitiveServices.Speech`, und installieren Sie es im Projekt **helloworld**.

    ![Screenshot des Dialogfelds „Pakete für Projektmappe verwalten“](media/sdk/qs-cpp-windows-04-nuget-install-1.0.0.png)

    > [!NOTE]
    > Die aktuelle Version des Cognitive Services Speech SDK ist `1.0.1`.

1. Akzeptieren Sie die angezeigten Lizenzbedingungen, um die Installation des NuGet-Pakets zu starten.

    ![Screenshot des Dialogfelds zur Zustimmung zu den Lizenzbedingungen](media/sdk/qs-cpp-windows-05-nuget-license.png)

Nachdem das Paket installiert wurde, wird eine Bestätigung in der Paket-Manager-Konsole angezeigt.

## <a name="add-sample-code"></a>Hinzufügen von Beispielcode

1. Öffnen Sie die Quelldatei *helloworld.cpp*. Ersetzen Sie den gesamten Code unter der ersten include-Anweisung (`#include "stdafx.h"` oder `#include "pch.h"`) durch Folgendes:

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/cpp-windows/helloworld/helloworld.cpp#code)]

1. Ersetzen Sie in der gleichen Datei die Zeichenfolge `YourSubscriptionKey` durch Ihren Abonnementschlüssel.

1. Ersetzen Sie die Zeichenfolge `YourServiceRegion` durch die [Region](regions.md), die mit Ihrem Abonnement verknüpft ist (z. B. `westus` für das kostenlose Testabonnement).

1. Speichern Sie die Änderungen am Projekt.

## <a name="build-and-run-the-app"></a>Erstellen und Ausführen der App

1. Erstellen Sie die Anwendung. Wählen Sie in der Menüleiste **Erstellen** > **Projektmappe erstellen** aus. Der Code sollte ohne Fehler kompiliert werden.

   ![Screenshot der Visual Studio-Anwendung mit hervorgehobener Option „Projektmappe erstellen“](media/sdk/qs-cpp-windows-06-build.png)

1. Starten Sie die Anwendung. Wählen Sie in der Menüleiste **Debuggen** > **Debuggen starten** aus, oder drücken Sie **F5**.

   ![Screenshot der Visual Studio-Anwendung mit hervorgehobener Option „Debuggen starten“](media/sdk/qs-cpp-windows-07-start-debugging.png)

1. Ein Konsolenfenster wird angezeigt, in dem Sie aufgefordert werden, etwas zu sagen. Sprechen Sie einen englischen Ausdruck oder Satz. Ihre Spracheingabe wird an den Spracherkennungsdienst übermittelt und in Text transkribiert, der im gleichen Fenster angezeigt wird.

   ![Screenshot der Konsolenausgabe nach erfolgreicher Erkennung](media/sdk/qs-cpp-windows-08-console-output-release.png)

[!INCLUDE [Download this sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Suchen Sie nach diesem Beispiel im Ordner `quickstart/cpp-windows`.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erkennen von Absichten anhand von gesprochener Sprache mit dem Speech SDK für C++](how-to-recognize-intents-from-speech-cpp.md)

## <a name="see-also"></a>Weitere Informationen

- [Übersetzen von Sprache](how-to-translate-speech-csharp.md)
- [Tutorial: Erstellen eines benutzerdefinierten Akustikmodells](how-to-customize-acoustic-models.md)
- [Tutorial: Erstellen eines benutzerdefinierten Sprachmodells](how-to-customize-language-model.md)
