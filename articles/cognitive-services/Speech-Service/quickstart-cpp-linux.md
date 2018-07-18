---
title: Schnellstart zum Speech SDK für C++ und Linux | Microsoft-Dokumentation
titleSuffix: Microsoft Cognitive Services
description: Informationen und Codebeispiele für den schnellen Einstieg in die Verwendung des Speech SDK mit Linux und C++ in Cognitive Services.
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 06/07/2018
ms.author: wolfma
ms.openlocfilehash: cee70ba585f93dda3249fc5b39f25fb613b57a45
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/25/2018
ms.locfileid: "36753604"
---
# <a name="quickstart-for-c-and-linux"></a>Schnellstart für C++ und Linux

Die aktuelle Version des Cognitive Services Speech SDK ist `0.4.0`.

Das Cognitive Services Speech SDK für Linux ist zum Erstellen von 64-Bit- und 32-Bit-Anwendungen verfügbar. Die erforderlichen Dateien können als TAR-Datei von https://aka.ms/csspeech/linuxbinary heruntergeladen werden.

> [!NOTE]
> Informationen zum Schnellstart für C++ und Windows finden Sie [hier](quickstart-cpp-windows.md).
> Informationen zum Schnellstart für C# und Windows finden Sie [hier](quickstart-csharp-windows.md).

[!include[Get a Subscription Key](includes/get-subscription-key.md)]

> [!NOTE]
> Diese Anweisungen setzen voraus, dass Sie Ubuntu 16.04 auf einem PC (x86 oder x64) ausführen.
> Für eine andere Ubuntu-Version oder eine andere Linux-Distribution müssen Sie die erforderlichen Schritte anpassen.

## <a name="prerequisites"></a>Voraussetzungen

[!include[Ubuntu Prerequisites](includes/ubuntu1604-prerequisites.md)]

## <a name="getting-the-binary-package"></a>Abrufen des binären Pakets

[!include[License Notice](includes/license-notice.md)]

1. Wählen Sie ein Verzeichnis aus (absoluter Pfad), in dem Sie die Speech SDK-Binärdateien und Header ablegen möchten.
   Wählen Sie z.B. den Pfad `speechsdk` unter Ihrem Basisverzeichnis:

   ```sh
   export SPEECHSDK_ROOT="$HOME/speechsdk"
   ```

1. Erstellen Sie das Verzeichnis, wenn es noch nicht vorhanden ist:

   ```sh
   mkdir -p "$SPEECHSDK_ROOT"
   ```

1. Laden Sie das `.tar.gz`-Archiv mit den Speech SDK-Binärdateien herunter, und extrahieren Sie es:

   ```sh
   wget -O SpeechSDK-Linux.tar.gz https://aka.ms/csspeech/linuxbinary
   tar --strip 1 -xzf SpeechSDK-Linux.tar.gz -C "$SPEECHSDK_ROOT"
   ```

1. Überprüfen Sie den Inhalt des Verzeichnisses auf oberster Ebene des extrahierten Pakets:

   ```sh
   ls -l "$SPEECHSDK_ROOT"
   ```

   Es sollte einen Drittanbieterhinweis und Lizenzdateien sowie ein `include`-Verzeichnis für Header und ein `lib`-Verzeichnis für Bibliotheken enthalten.

   [!include[Linux Binary Archive Content](includes/linuxbinary-content.md)]

## <a name="sample-code"></a>Beispielcode

Der folgende Code erkennt, wenn Ihr Mikrofon englische Sprache aufnimmt.
Legen Sie ihn in einer Datei mit dem Namen `quickstart-linux.cpp` ab:

[!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/Linux/quickstart-linux/quickstart-linux.cpp#code)]

Ersetzen Sie den Abonnementschlüssel im Code mit dem, den Sie erhalten haben.

## <a name="building"></a>Erstellen

> [!NOTE]
> Fügen Sie die kopierten Buildbefehle unten unbedingt in einer _einzelnen Zeile_ ein.

* Führen Sie den folgenden Befehl aus, um die Anwendung auf einem x64-Computer zu erstellen:

  ```sh
  g++ quickstart-linux.cpp -o quickstart-linux -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/x64" -l:libssl.so.1.0.0 -l:libcurl.so.4 -l:libasound.so.2
  ```

* Führen Sie den folgenden Befehl aus, um die Anwendung auf einem x86-Computer zu erstellen:

  ```sh
  g++ quickstart-linux.cpp -o quickstart-linux -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/x86" -l:libssl.so.1.0.0 -l:libcurl.so.4 -l:libasound.so.2
  ```

## <a name="running"></a>Wird ausgeführt

Um die Anwendung auszuführen, müssen Sie den Bibliothekspfad des Laders so konfigurieren, dass er auf die Speech SDK-Bibliothek weist.

* Führen Sie auf einem x64-Computer Folgendes aus:

  ```sh
  export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/x64"
  ```

* Führen Sie auf einem x86-Computer Folgendes aus:

  ```sh
  export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/x86"
  ```

Führen Sie die Anwendung wie folgt aus:

```sh
./quickstart-linux
```

Bei erfolgreichem Verlauf sollte eine ähnliche Ausgabe wie die folgende angezeigt werden:

```text
Say something...
We recognized: What's the weather
```

## <a name="downloading-the-sample"></a>Herunterladen des Beispiels

Die Sammlung der aktuellsten Beispiele finden Sie im [Cognitive Services Speech SDK Sample-GitHub-Repository](https://aka.ms/csspeech/samples).

## <a name="next-steps"></a>Nächste Schritte

* Weitere Beispiele finden Sie auf [dieser Seite](samples.md).
