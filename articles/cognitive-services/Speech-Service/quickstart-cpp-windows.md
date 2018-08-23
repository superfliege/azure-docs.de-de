---
title: 'Schnellstart: Erkennen von Sprache in C++ unter Windows Desktop mit dem Cognitive Services Speech SDK'
titleSuffix: Microsoft Cognitive Services
description: Erfahren Sie, wie Sie Sprache in C++ unter Windows Desktop mit dem Cognitive Services Speech SDK erkennen können.
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 07/16/2018
ms.author: wolfma
ms.openlocfilehash: 59c70d33d537742a5918dc1ba55ab8ac1f84e12c
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2018
ms.locfileid: "41929798"
---
# <a name="quickstart-recognize-speech-in-c-on-windows-desktop-using-the-speech-sdk"></a>Schnellstart: Erkennen von Sprache in C++ unter Windows Desktop mit dem Speech SDK

[!include[Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

In diesem Artikel wird beschrieben, wie Sie eine C++-basierte Konsolenanwendung für Windows Desktop erstellen, in der das Speech SDK verwendet wird.
Die Anwendung basiert auf dem [NuGet-Paket für das Microsoft Cognitive Services Speech SDK](https://aka.ms/csspeech/nuget) und Microsoft Visual Studio 2017.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Abonnementschlüssel für den Spracherkennungsdienst. Mehr erfahren Sie unter [Kostenloses Testen des Spracherkennungsdiensts](get-started.md).
* Ein Windows-PC mit einem funktionierenden Mikrofon.
* [Microsoft Visual Studio 2017](https://www.visualstudio.com/) Community Edition oder höher.
* Die Workload **Desktopentwicklung mit C++** in Visual Studio und die Komponente **NuGet-Paket-Manager** in Visual Studio.
  Sie können beides über **Tools** \> **Tools und Features abrufen** auf den Registerkarten **Workloads** bzw. **Einzelkomponenten** aktivieren:

  ![Aktivieren der Workload „Desktopentwicklung mit C++“](media/sdk/vs-enable-cpp-workload.png)

  ![Aktivieren von NuGet-Paket-Manager in Visual Studio ](media/sdk/vs-enable-nuget-package-manager.png)

## <a name="create-a-visual-studio-project"></a>Erstellen eines Visual Studio-Projekts

Erstellen Sie in Visual Studio 2017 in Visual C++ unter Windows Desktop eine neue Windows-Konsolenanwendung. Erweitern Sie im Dialogfeld **Neues Projekt** im linken Bereich **Installiert** \> **Visual C++** \> **Windows Desktop**, und wählen Sie dann **Windows-Konsolenanwendung** aus. Geben Sie als Projektnamen *helloworld* ein.

![Erstellen einer Windows-Konsolenanwendung in Visual C++ unter Windows Desktop](media/sdk/qs-cpp-windows-01-new-console-app.png)

Wenn Sie eine 64-Bit-Windows-Installation ausführen, können Sie die Buildplattform optional in `x64` ändern:

![Ändern der Buildplattform in x64](media/sdk/qs-cpp-windows-02-switch-to-x64.png)

## <a name="install-and-reference-the-speech-sdk-nuget-package"></a>Installieren des NuGet-Pakets für das Speech SDK und Verweisen auf dieses

Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf die Projektmappe, und klicken Sie dann auf **NuGet-Pakete für Projektmappe verwalten**.

![Rechtsklick auf „NuGet-Pakete für Projektmappe verwalten“](media/sdk/qs-cpp-windows-03-manage-nuget-packages.png)

Wählen Sie rechts oben im Feld **Paketquelle** die Option „Nuget.org“ aus.
Suchen Sie auf der Registerkarte **Durchsuchen** nach dem Paket „Microsoft.CognitiveServices.Speech“, und wählen Sie es aus. Aktivieren Sie die Kontrollkästchen **Projekt** und **helloworld** im rechten Bereich, und wählen Sie **Installieren** aus, um das Paket im Projekt „helloworld“ zu installieren.

> [!NOTE]
> Die aktuelle Version des Cognitive Services Speech SDK ist `0.6.0`.

![Installieren des NuGet-Pakets Microsoft.CognitiveServices.Speech](media/sdk/qs-cpp-windows-04-nuget-install-0.5.0.png)

Stimmen Sie im dann angezeigten Lizenzbildschirm den Lizenzbedingungen zu:

![Akzeptieren der Lizenzbedingungen](media/sdk/qs-cpp-windows-05-nuget-license.png)

## <a name="add-the-sample-code"></a>Hinzufügen des Beispielcodes

1. Ersetzen Sie den standardmäßigen Startercode durch folgenden Code:

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/cpp-windows/helloworld/helloworld.cpp#code)]

1. Ersetzen Sie die Zeichenfolge `YourSubscriptionKey` durch Ihren Abonnementschlüssel.

1. Ersetzen Sie die Zeichenfolge `YourServiceRegion` durch die [Region](regions.md), die mit Ihrem Abonnement verknüpft ist (z. B. `westus` für das kostenlose Testabonnement).

1. Speichern Sie die Änderungen am Projekt.

## <a name="build-and-run-the-sample"></a>Erstellen und Ausführen des Beispiels

1. Erstellen Sie die Anwendung. Wählen Sie in der Menüleiste **Build** > **Projektmappe erstellen** aus. Der Code sollte nun ohne Fehler kompiliert werden:

   ![Erfolgreicher Build](media/sdk/qs-cpp-windows-06-build.png)

1. Starten Sie die Anwendung. Wählen Sie in der Menüleiste **Debuggen** > **Debuggen starten** aus, oder drücken Sie **F5**.

   ![Starten der Anwendung im Debugger](media/sdk/qs-cpp-windows-07-start-debugging.png)

1. Ein Konsolenfenster wird angezeigt, in dem Sie aufgefordert werden, (auf Englisch) zu sprechen.
   Das Ergebnis der Erkennung wird auf dem Bildschirm angezeigt.

   ![Konsolenausgabe nach erfolgreicher Erkennung](media/sdk/qs-cpp-windows-08-console-output-release.png)

[!include[Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Suchen Sie nach diesem Beispiel im Ordner `quickstart/cpp-windows`.

## <a name="next-steps"></a>Nächste Schritte

* [Abrufen der Beispiele](speech-sdk.md#get-the-samples)
