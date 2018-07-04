---
title: Schnellstart zum Speech SDK für C++ und Windows | Microsoft-Dokumentation
titleSuffix: Microsoft Cognitive Services
description: Informationen und Codebeispiele für den schnellen Einstieg in die Verwendung des Speech SDK mit Windows und C++ in Cognitive Services.
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 06/07/2018
ms.author: wolfma
ms.openlocfilehash: 0bcdc3c4357cb8985fad16c607957bffad4a2b8c
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37049229"
---
# <a name="quickstart-for-c-and-windows"></a>Schnellstart für C++ und Windows

Die aktuelle Version des Cognitive Services Speech SDK ist `0.4.0`.

In diesem Artikel wird beschrieben, wie Sie eine C++-basierte Konsolenanwendung für Windows Desktop erstellen, in der das Speech SDK verwendet wird.
Die Anwendung basiert auf dem [NuGet-Paket für das Microsoft Cognitive Services Speech SDK](https://www.nuget.org/packages/Microsoft.CognitiveServices.Speech) und Microsoft Visual Studio 2017.

> [!NOTE]
> Informationen zum Schnellstart für C++ und Linux finden Sie [hier](quickstart-cpp-linux.md).<br>
> Informationen zum Schnellstart für C# und Windows finden Sie [hier](quickstart-csharp-windows.md).

> [!NOTE]
> Für diesen Schnellstart ist ein PC mit einem funktionierenden Mikrofon erforderlich.<br>
> Ein Beispiel zur Spracherkennung aus einer angegebenen Audioeingabedatei finden Sie [hier](speech-to-text-sample.md#speech-recognition-from-a-file).

> [!NOTE]
> Ihre Visual Studio-Installation muss die Workload **Desktopentwicklung mit C++** enthalten.
> Wenn Sie nicht sicher sind, gehen Sie wie folgt vor, um dies zu überprüfen und gegebenenfalls zu korrigieren: Wählen Sie in Visual Studio 2017 **Tools** \> **Tools und Features abrufen** aus, und bestätigen Sie die Aufforderung der Benutzerkontensteuerung durch Auswählen von **Ja**.
> Wenn auf der Registerkarte **Workloads** neben **Desktopentwicklung mit C++** kein Kontrollkästchen aktiviert ist, aktivieren Sie es, und klicken Sie auf **Ändern**, um die Änderungen zu speichern.

[!include[Get a Subscription Key](includes/get-subscription-key.md)]

## <a name="creating-an-empty-console-application-project"></a>Erstellen eines leeren Konsolenanwendungsprojekts

Erstellen Sie in Visual Studio 2017 in Visual C++ unter Windows Desktop eine neue Windows-Konsolenanwendung mit dem Namen CppHelloSpeech:

![Erstellen einer Windows-Konsolenanwendung in Visual C++ unter Windows Desktop](media/sdk/speechsdk-05-vs-cpp-new-console-app.png)

Wenn Sie eine 64-Bit-Windows-Installation ausführen, können Sie die Buildplattform optional in `x64` ändern:

![Ändern der Buildplattform in x64](media/sdk/speechsdk-07-vs-cpp-switch-to-x64.png)

## <a name="install-and-reference-the-speech-sdk-nuget-package"></a>Installieren des NuGet-Pakets für das Speech SDK und Verweisen auf dieses

> [!NOTE]
> Stellen Sie sicher, dass der NuGet-Paket-Manager für Ihre Visual Studio 2017-Installation aktiviert ist.
> Wählen Sie in Visual Studio 2017 **Tools** \> **Tools und Features abrufen** aus, und bestätigen Sie die Aufforderung der Benutzerkontensteuerung durch Auswählen von **Ja**. Wählen Sie dann die Registerkarte **Einzelne Komponenten** aus, und suchen Sie **NuGet-Paket-Manager** unter **Codetools**.
> Wenn das Kontrollkästchen links daneben nicht aktiviert ist, aktivieren Sie es, und klicken Sie auf **Ändern**, um die Änderungen zu speichern.
>
> ![Aktivieren von NuGet-Paket-Manager in Visual Studio ](media/sdk/speechsdk-05-vs-enable-nuget-package-manager.png)

Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf die Projektmappe, und klicken Sie dann auf **NuGet-Pakete für Projektmappe verwalten**.

![Rechtsklick auf „NuGet-Pakete für Projektmappe verwalten“](media/sdk/speechsdk-09-vs-cpp-manage-nuget-packages.png)

Wählen Sie rechts oben im Feld **Paketquelle** die Option „Nuget.org“ aus.
Suchen Sie auf der Registerkarte **Durchsuchen** nach dem Paket „Microsoft.CognitiveServices.Speech“, und wählen Sie es aus. Aktivieren Sie die Kontrollkästchen **Projekt** und **CppHelloSpeech** im rechten Bereich, und wählen Sie **Installieren** aus, um das Paket im Projekt CppHelloSpeech zu installieren.

![Installieren des NuGet-Pakets Microsoft.CognitiveServices.Speech](media/sdk/speechsdk-11-vs-cpp-manage-nuget-install.png)

Stimmen Sie im dann angezeigten Lizenzbildschirm den Lizenzbedingungen zu:

![Akzeptieren der Lizenzbedingungen](media/sdk/speechsdk-12-vs-cpp-manage-nuget-license.png)

## <a name="add-the-sample-code"></a>Hinzufügen des Beispielcodes

Ersetzen Sie den standardmäßigen Startercode durch folgenden Code:

[!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/Windows/quickstart-cpp/CppHelloSpeech.cpp#code)]

> [!IMPORTANT]
> Ersetzen Sie den Abonnementschlüssel durch den Abonnementschlüssel, den Sie erhalten haben. <br>
> Ersetzen Sie die Region durch Ihre Region aus der [Speech Service-REST-API](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/rest-apis), z.B. „westus“.

![Hinzufügen Ihres Abonnementschlüssels](media/sdk/sub-key-recognize-speech-cpp.png)

## <a name="build-and-run-the-sample"></a>Erstellen und Ausführen des Beispiels

Der Code sollte nun ohne Fehler kompiliert werden:

![Erfolgreicher Build](media/sdk/speechsdk-16-vs-cpp-build.png)

Starten Sie das Programm im Debugger über „Debuggen starten“ oder über die Taste F5:

![Starten der Anwendung im Debugger](media/sdk/speechsdk-17-vs-cpp-f5.png)

Ein Konsolenfenster sollte angezeigt werden, in dem Sie aufgefordert werden, (auf Englisch) zu sprechen.
Das Ergebnis der Erkennung wird auf dem Bildschirm angezeigt.

![Konsolenausgabe nach erfolgreicher Erkennung](media/sdk/speechsdk-18-vs-cpp-console-output-release.png)

## <a name="downloading-the-sample"></a>Herunterladen des Beispiels

Die neuesten Beispiele finden Sie im [GitHub-Beispielrepository für das Cognitive Services Speech SDK](https://aka.ms/csspeech/samples).

## <a name="next-steps"></a>Nächste Schritte

* Weitere Beispiele finden Sie auf [dieser Seite](samples.md).
