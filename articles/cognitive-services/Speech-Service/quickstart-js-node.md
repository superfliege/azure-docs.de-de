---
title: 'Schnellstart: Erkennen von Sprache in JavaScript in Node.js mit dem Speech Service SDK'
titleSuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie das Speech Service SDK verwenden, um Sprache in JavaScript in Node.js zu erkennen.
services: cognitive-services
author: fmegen
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 12/18/2018
ms.author: fmegen
ms.openlocfilehash: 35652b169067bc545fa0d1fcc977bbaee79ec3aa
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/21/2018
ms.locfileid: "53724432"
---
# <a name="quickstart-recognize-speech-in-javascript-in-nodejs-using-the-speech-service-sdk"></a>Schnellstart: Erkennen von Sprache in JavaScript in Node.js mit dem Speech Service SDK

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Dieser Artikel enthält Informationen zur Erstellung eines Node.js-Projekts mithilfe der JavaScript-Bindung des Cognitive Services Speech SDK zum Umwandeln von Sprache in Text.
Die Anwendung basiert auf dem [Cognitive Services Speech SDK](https://aka.ms/csspeech/npmpackage) von Microsoft.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnementschlüssel für den Speech-Dienst. [Hier können Sie den Schlüssel kostenlos erhalten](get-started.md).
* Eine aktuelle Version von [Node.js](https://nodejs.org).

## <a name="create-a-new-project-folder"></a>Erstellen eines neuen Projektordners

Erstellen Sie einen neuen, leeren Ordner, und initialisieren Sie ihn als neues JavaScript- und Node.js-Projekt.

```sh
npm init -f
```

Dadurch wird die Datei „package.json“ mit Standardwerten initiiert. Diese Datei möchten Sie später wahrscheinlich noch bearbeiten.

## <a name="install-the-speech-sdk-for-javascript-into-that-folder"></a>Installieren des Speech SDK für JavaScript in diesem Ordner

Fügen Sie das Speech SDK mithilfe von `npm install microsoft-cognitiveservices-speech-sdk` Ihrem Node.js-Projekt hinzu.

Dadurch werden die neueste Version des Speech SDK sowie alle ggf. erforderlichen Komponenten von npmjs herunterladen und installiert. Das SDK wird im Verzeichnis `node_modules` in Ihrem Projektordner installiert.

## <a name="using-the-speech-sdk"></a>Verwenden des Speech SDK

Erstellen Sie eine neue Datei im Ordner `index.js`, und öffnen Sie diese Datei mit einem Text-Editor.

> [!NOTE]
> In Node.js unterstützt das Speech SDK weder das Mikrofon noch den Datentyp „Datei“. Beides wird nur in Browsern unterstützt. Verwenden Sie stattdessen die Streamschnittstelle für das Speech SDK (entweder über `AudioInputStream.createPushStream()` oder über `AudioInputStream.createPullStream()`).

In diesem Beispiel verwenden wir die Schnittstelle `PushAudioInputStream`.

Fügen Sie den folgenden JavaScript-Code hinzu:

[!code-javascript[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/js-node/index.js#code)]

## <a name="running-the-sample-from-command-line"></a>Ausführen des Beispiels über die Befehlszeile

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

## <a name="running-the-sample-from-visual-studio-code"></a>Ausführen des Beispiels über Visual Studio Code

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
