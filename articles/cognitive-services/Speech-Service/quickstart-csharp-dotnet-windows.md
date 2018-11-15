---
title: 'Schnellstart: Erkennen und Transkribieren von Sprache, .NET Framework (Windows) – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: In dieser Anleitung erfahren Sie, wie Sie unter Verwendung von .NET Framework für Windows und dem Speech SDK eine Konsolenanwendung zur Spracherkennung erstellen. Danach können Sie das Mikrofon Ihres Computers verwenden, um Sprache in Echtzeit zu transkribieren.
services: cognitive-services
author: wolfma61
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 11/05/2018
ms.author: wolfma
ms.openlocfilehash: 502f59c88808480f26e6ea5a6d2c5b362c78869b
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2018
ms.locfileid: "51683494"
---
# <a name="quickstart-recognize-and-transcribe-speech-using-the-speech-sdk-and-net-framework-windows"></a>Schnellstart: Erkennen und Transkribieren von Sprache unter Verwendung von Speech SDK und .NET Framework (Windows)

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

In dieser Anleitung erfahren Sie, wie Sie unter Verwendung von .NET Framework für Windows und dem Speech SDK eine Konsolenanwendung zur Spracherkennung erstellen. Danach können Sie das Mikrofon Ihres Computers verwenden, um Sprache in Echtzeit zu transkribieren.

Für diese Schnellstartanleitung wird ein [Azure Cognitive Services-Konto](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) mit aktiviertem Microsoft Speech-Dienst benötigt. Wenn Sie über kein Konto verfügen, können Sie über die [kostenlose Testversion](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started) einen Abonnementschlüssel abrufen.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Projekt benötigen Sie Folgendes:

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* Einen Abonnementschlüssel für den Speech-Dienst
* Zugriff auf das Mikrofon Ihres Computers

## <a name="create-a-visual-studio-project"></a>Erstellen eines Visual Studio-Projekts

[!INCLUDE [Create project ](../../../includes/cognitive-services-speech-service-create-speech-project-vs-csharp.md)]

## <a name="add-sample-code"></a>Hinzufügen von Beispielcode

1. Öffnen Sie `Program.cs`, und ersetzen Sie den automatisch generierten Code durch dieses Beispiel:

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-dotnet-windows/helloworld/Program.cs#code)]

1. Suchen Sie nach der Zeichenfolge `YourSubscriptionKey`, und ersetzen Sie sie durch Ihren Abonnementschlüssel für den Speech-Dienst.

1. Suchen Sie nach der Zeichenfolge `YourServiceRegion`, und ersetzen Sie sie durch die [Region](regions.md), die mit Ihrem Abonnement verknüpft ist. Bei Verwendung der kostenlosen Testversion ist die Region beispielsweise `westus`.

1. Speichern Sie die Änderungen am Projekt.

## <a name="build-and-run-the-app"></a>Erstellen und Ausführen der App

1. Wählen Sie in der Menüleiste **Build** > **Projektmappe erstellen** aus. Der Code sollte nun ohne Fehler kompiliert werden.

    ![Screenshot der Visual Studio-Anwendung mit hervorgehobener Option „Projektmappe erstellen](media/sdk/qs-csharp-dotnet-windows-08-build.png "Erfolgreicher Build“")

1. Wählen Sie auf der Menüleiste **Debuggen** > **Debuggen starten** aus, oder drücken Sie**F5**, um die Anwendung zu starten.

    ![Screenshot der Visual Studio-Anwendung mit hervorgehobener Option „Debuggen starten](media/sdk/qs-csharp-dotnet-windows-09-start-debugging.png "Debuggen der App starten“")

1. Ein Konsolenfenster mit einer Sprechaufforderung wird angezeigt. Sagen Sie etwas auf Englisch. Ihre Spracheingabe wird an den Speech-Dienst übermittelt und in Echtzeit in Text umgewandelt. Das Ergebnis wird in der Konsole ausgegeben.

    ![Screenshot der Konsolenausgabe nach erfolgreicher Erkennung](media/sdk/qs-csharp-dotnet-windows-10-console-output.png "Konsolenausgabe nach erfolgreicher Erkennung")

[!INCLUDE [Download this sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Der Code steht im Ordner `quickstart/csharp-dotnet-windows` zur Verfügung.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erkennen von Absichten anhand von gesprochener Sprache mit dem Speech SDK für C#](how-to-recognize-intents-from-speech-csharp.md)

## <a name="see-also"></a>Weitere Informationen

- [Übersetzen von Sprache](how-to-translate-speech-csharp.md)
- [Tutorial: Erstellen eines benutzerdefinierten Akustikmodells](how-to-customize-acoustic-models.md)
- [Tutorial: Erstellen eines benutzerdefinierten Sprachmodells](how-to-customize-language-model.md)
