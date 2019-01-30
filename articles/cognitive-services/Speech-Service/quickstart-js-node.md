---
title: 'Schnellstart: Erkennen von Sprache, Node.js – Spracherkennungsdienste'
titleSuffix: Azure Cognitive Services
description: In dieser Anleitung erfahren Sie, wie Sie unter Verwendung des Speech SDK für Node.js eine Konsolenanwendung zur Spracherkennung erstellen. Danach können Sie das Mikrofon Ihres Computers verwenden, um Sprache in Echtzeit zu transkribieren.
services: cognitive-services
author: fmegen
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 1/16/2019
ms.author: fmegen
ms.openlocfilehash: e0ae916687ca32835dd8daf6e5059b8f6eea0ff6
ms.sourcegitcommit: ba9f95cf821c5af8e24425fd8ce6985b998c2982
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/17/2019
ms.locfileid: "54382162"
---
# <a name="quickstart-recognize-speech-with-the-speech-sdk-for-nodejs"></a>Schnellstart: Erkennen von Sprache mit dem Speech SDK für Node.js

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Dieser Artikel enthält Informationen zur Erstellung eines Node.js-Projekts mithilfe der JavaScript-Bindung des Cognitive Services Speech SDK zum Umwandeln von Sprache in Text.
Die Anwendung basiert auf dem [Cognitive Services Speech SDK](https://aka.ms/csspeech/npmpackage) von Microsoft.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnementschlüssel für den Speech-Dienst. [Hier können Sie den Schlüssel kostenlos erhalten](get-started.md).
* Eine aktuelle Version von [Node.js](https://nodejs.org).

## <a name="create-a-new-project"></a>Erstellen eines neuen Projekts

Erstellen Sie einen neuen Ordner, und initialisieren Sie das Projekt.

```sh
npm init -f
```

Dadurch wird die Datei „package.json“ mit Standardwerten initiiert. Diese Datei möchten Sie später wahrscheinlich noch bearbeiten.

## <a name="install-the-speech-sdk"></a>Installieren des Speech SDK

Fügen Sie das Speech SDK Ihrem Node.js-Projekt hinzu.

```
npm install microsoft-cognitiveservices-speech-sdk
```

Dadurch werden die neueste Version des Speech SDK sowie alle ggf. erforderlichen Komponenten von npmjs herunterladen und installiert. Das SDK wird im Verzeichnis `node_modules` in Ihrem Projektordner installiert.

## <a name="use-the-speech-sdk"></a>Verwenden des Speech SDK

Erstellen Sie eine neue Datei im Ordner `index.js`, und öffnen Sie diese Datei mit einem Text-Editor.

> [!NOTE]
> In Node.js unterstützt das Speech SDK weder das Mikrofon noch den Datentyp „Datei“. Beides wird nur in Browsern unterstützt. Verwenden Sie stattdessen die Streamschnittstelle für das Speech SDK (entweder über `AudioInputStream.createPushStream()` oder über `AudioInputStream.createPullStream()`).

In diesem Beispiel verwenden wir die Schnittstelle `PushAudioInputStream`.

Fügen Sie den folgenden JavaScript-Code hinzu:

[!code-javascript[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/js-node/index.js#code)]

## <a name="run-the-sample"></a>Ausführen des Beispiels

Passen Sie zum Starten der App `YourSubscriptionKey`, `YourServiceRegion` und `YourAudioFile.wav` an Ihre Konfiguration an. Anschließend können Sie sie durch Aufrufen des folgenden Befehls ausführen:

```sh
node index.js
```

Daraufhin wird eine Erkennung unter Verwendung des angegebenen Dateinamens ausgelöst und die Ausgabe in der Konsole angezeigt.

Hier sehen Sie eine Beispielausgabe der Ausführung von `index.js` nach Aktualisierung des Abonnementschlüssels und bei Verwendung der Datei `whatstheweatherlike.wav`.

```json
SpeechRecognitionResult {
  "privResultId": "9E30EEBD41AC4571BB77CF9164441F46",
  "privReason": 3,
  "privText": "What's the weather like?",
  "privDuration": 15900000,
  "privOffset": 300000,
  "privErrorDetails": null,
  "privJson": {
    "RecognitionStatus": "Success",
    "DisplayText": "What's the weather like?",
    "Offset": 300000,
    "Duration": 15900000
  },
  "privProperties": null
}
```

## <a name="install-and-use-the-speech-sdk-with-visual-studio-code"></a>Installieren und Verwenden des Speech SDK mit Visual Studio Code

Das Beispiel kann auch über Visual Studio Code ausgeführt werden. Gehen Sie wie folgt vor, um den Schnellstart zu installieren, zu öffnen und auszuführen:

1. Starten Sie Visual Studio Code, klicken Sie auf „Ordner öffnen“, und navigieren Sie zum Ordner für den Schnellstart.

   ![Screenshot von „Ordner öffnen“](media/sdk/qs-js-node-01-open_project.png)

1. Öffnen Sie in Visual Studio Code ein Terminalfenster.

   ![Screenshot des Terminalfensters](media/sdk/qs-js-node-02_open_terminal.png)

1. Führen Sie npm aus, um die Abhängigkeiten zu installieren.

   ![Screenshot von „npm install“](media/sdk/qs-js-node-03-npm_install.png)

1. Nun können Sie `index.js` öffnen und einen Breakpoint festlegen.

   ![Screenshot von „index.js“ mit einem Breakpoint in Zeile 16](media/sdk/qs-js-node-04-setup_breakpoint.png)

1. Drücken Sie zum Starten des Debuggens entweder F5, oder wählen Sie im Menü „Debuggen“ die Option „Debuggen starten“ aus.

   ![Screenshot des Menüs „Debuggen“](media/sdk/qs-js-node-05-start_debugging.png)

1. Wenn ein Breakpoint erreicht wird, können Sie die Aufrufliste und die Variablen untersuchen.

   ![Screenshot des Debuggers](media/sdk/qs-js-node-06-hit_breakpoint.png)

1. Ausgaben werden im Fenster der Debugging-Konsole angezeigt.

   ![Screenshot der Debugging-Konsole](media/sdk/qs-js-node-07-debug_output.png)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Node.js-Beispiele auf GitHub](https://aka.ms/csspeech/samples)
