---
title: 'Schnellstart: Erkennen von Sprache in C++ unter Linux mit dem Cognitive Services Speech SDK'
titleSuffix: Microsoft Cognitive Services
description: Erfahren Sie, wie Sie Sprache in C++ unter Linux mit dem Cognitive Services Speech SDK erkennen können.
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 07/16/2018
ms.author: wolfma
ms.openlocfilehash: 92bd5980ac2e6befbe352df6ddf8644f04d37d34
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2018
ms.locfileid: "43126864"
---
# <a name="quickstart-recognize-speech-in-c-on-linux-using-the-speech-sdk"></a>Schnellstart: Erkennen von Sprache in C++ unter Linux mit dem Speech SDK

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Dieser Artikel enthält Informationen zur Erstellung einer C++-Konsolenanwendung in Linux (Ubuntu 16.04) mithilfe des Cognitive Services Speech SDK zum Umwandeln von Sprache in Text.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Abonnementschlüssel für den Spracherkennungsdienst. Mehr erfahren Sie unter [Kostenloses Testen des Spracherkennungsdiensts](get-started.md).
* Ein Ubuntu 16.04-PC mit einem funktionierenden Mikrofon.
* Um die Pakete zu installieren, die zum Erstellen und Ausführen dieses Beispiels erforderlich sind, führen Sie Folgendes aus:

  ```sh
  sudo apt-get update
  sudo apt-get install build-essential libssl1.0.0 libcurl3 libasound2 wget
  ```

## <a name="get-the-speech-sdk"></a>Abrufen des Speech SDK

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

Die aktuelle Version des Cognitive Services Speech SDK ist `0.6.0`.

Das Cognitive Services Speech SDK für Linux ist zum Erstellen von 64-Bit- und 32-Bit-Anwendungen verfügbar.
Die erforderlichen Dateien können als TAR-Datei von https://aka.ms/csspeech/linuxbinary heruntergeladen werden.
Laden Sie das SDK wie folgt herunter, und installieren Sie es:

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

   [!INCLUDE [Linux Binary Archive Content](../../../includes/cognitive-services-speech-service-linuxbinary-content.md)]

## <a name="add-the-sample-code"></a>Hinzufügen des Beispielcodes

1. Fügen Sie den folgenden Code in einer Datei namens `helloworld.cpp` hinzu:

  [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/cpp-linux/helloworld.cpp#code)]

1. Ersetzen Sie die Zeichenfolge `YourSubscriptionKey` durch Ihren Abonnementschlüssel.

1. Ersetzen Sie die Zeichenfolge `YourServiceRegion` durch die [Region](regions.md), die mit Ihrem Abonnement verknüpft ist (z. B. `westus` für das kostenlose Testabonnement).

## <a name="building"></a>Erstellen

> [!NOTE]
> Fügen Sie die kopierten Buildbefehle unten unbedingt in einer _einzelnen Zeile_ ein.

* Führen Sie auf einem **x64**-Computer den folgenden Befehl aus, um die Anwendung zu erstellen:

  ```sh
  g++ helloworld.cpp -o helloworld -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/x64" -l:libssl.so.1.0.0 -l:libcurl.so.4 -l:libasound.so.2
  ```

* Führen Sie auf einem **x86**-Computer den folgenden Befehl aus, um die Anwendung zu erstellen:

  ```sh
  g++ helloworld.cpp -o helloworld -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/x86" -l:libssl.so.1.0.0 -l:libcurl.so.4 -l:libasound.so.2
  ```

## <a name="run-the-sample"></a>Ausführen des Beispiels

1. Konfigurieren Sie den Bibliothekspfad des Ladeprogramms so, dass er auf die Speech SDK-Bibliothek verweist.

   * Führen Sie auf einem **x64**-Computer Folgendes aus:

     ```sh
     export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/x64"
     ```

   * Führen Sie auf einem **x86**-Computer Folgendes aus:

     ```sh
     export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/x86"
     ```

1. Führen Sie die Anwendung wie folgt aus:

   ```sh
   ./helloworld
   ```

1. Eine ähnliche Ausgabe wie die folgende sollte angezeigt werden:

   ```text
   Say something...
   We recognized: What's the weather
   ```

[!INCLUDE [Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Suchen Sie nach diesem Beispiel im Ordner `quickstart/cpp-linux`.

## <a name="next-steps"></a>Nächste Schritte

* [Abrufen der Beispiele](speech-sdk.md#get-the-samples)
