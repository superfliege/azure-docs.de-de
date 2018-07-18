---
title: AudioInputStream – Konzepte | Microsoft-Dokumentation
description: Übersicht über die Funktionen der AudioInputStream-API.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: fmegen
manager: wolfma61
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 06/07/2018
ms.author: fmegen
ms.openlocfilehash: 528356473c4221a815fa68cbec3426866c4cbd23
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2018
ms.locfileid: "35378987"
---
# <a name="about-the-audio-input-stream-api"></a>Informationen zur API für Audioeingabedatenströme

Die API für **Audioeingabedatenströme** bietet eine Möglichkeit zum Streamen von Audiodatenströmen in die Erkennungen anstelle der Verwendung des Mikrofons oder der APIs für Eingabedateien.

## <a name="api-overview"></a>API-Übersicht

Die API verwendet zwei Komponenten: `AudioInputStream` (Audiorohdaten) und `AudioInputStreamFormat`.

`AudioInputStreamFormat` definiert das Format der Audiodaten. Es kann mit der standardmäßigen `WAVEFORMAT`-Struktur für Wavedateien unter Windows verglichen werden.

  - `FormatTag`

    Das Format des Audiomaterials. Das Speech SDK unterstützt derzeit nur `format 1` (PCM – Little-Endian).

  - `Channels`

    Die Anzahl der Kanäle. Der aktuelle Speech-Dienst unterstützt nur Audiomaterial mit einem Kanal (Mono).

  - `SamplesPerSec`

    Die Abtastrate. Bei einer Mikrofonaufzeichnung werden normalerweise 16.000 Abtastvorgänge pro Sekunde durchgeführt.

  - `AvgBytesPerSec`

    Durchschnittliche Anzahl von Bytes pro Sekunde, berechnet als `SamplesPerSec * Channels * ceil(BitsPerSample, 8)`. Die durchschnittliche Anzahl von Bytes pro Sekunde kann bei Audiodatenströmen, bei denen variable Bitraten verwendet werden, abweichen.

  - `BlockAlign`

    Die Größe eines einzelnen Frames, berechnet als `Channels * ceil(wBitsPerSample, 8)`. Aufgrund des Innenabstands kann der tatsächliche Wert höher als dieser Wert sein.

  - `BitsPerSample`

    Die Bits pro Abtastvorgang. In einem Audiodatenstrom werden normalerweise 16 Bit pro Abtastvorgang verwendet (CD-Qualität).

Die `AudioInputStream`-Basisklasse wird durch Ihren benutzerdefinierten Datenstromadapter überschrieben. Der Adapter muss folgende Funktionen implementieren:

   - `GetFormat()`

     Diese Funktion wird aufgerufen, um das Format des Audiodatenstroms abzurufen. Er ruft einen Zeiger auf den AudioInputStreamFormat-Puffer ab.

   - `Read()`

     Diese Funktion wird aufgerufen, um Daten aus dem Audiodatenstrom abzurufen. Ein Parameter gibt einen Zeiger auf den Puffer an, in den die Audiodaten kopiert werden. Der zweite Parameter gibt die Größe des Puffers an. Die Funktion gibt die Anzahl von Bytes zurück, die in den Puffer kopiert werden. Der Rückgabewert `0` gibt das Ende des Datenstroms an.

   - `Close()`

     Diese Funktion wird aufgerufen, um den Audiodatenstrom zu schließen.

## <a name="usage-examples"></a>Anwendungsbeispiele

Im Allgemeinen werden bei der Verwendung von Audioeingabedatenströmen die folgenden Schritte ausgeführt:

  - Ermitteln Sie das Format des Audiodatenstroms. Das Format muss vom SDK und dem Speech-Dienst unterstützt werden. Derzeit wird die folgende Konfiguration unterstützt:

    Ein Audioformat-Tag (PCM), ein Kanal, 16.000 Abtastvorgänge pro Sekunde, 32.000 Byte pro Sekunde, Ausrichtung zweier Blöcke (16 Bit mit Innenabstand für einen Abtastvorgang), 16 Bit pro Abtastvorgang

  - Stellen Sie sicher, dass der Code die Audiorohdaten entsprechend der oben aufgeführten Spezifikationen bereitstellen kann. Wenn die Audioquelldaten nicht mit den unterstützten Formaten übereinstimmen, muss das Audiomaterial in das erforderliche Format transcodiert werden.

  - Leiten Sie die Klasse des benutzerdefinierten Audioeingabedatenstroms von `AudioInputStream` ab. Implementieren Sie die Vorgänge `GetFormat()`, `Read()` und `Close()`. Die genaue Funktionssignatur ist sprachabhängig, der Code entspricht jedoch in etwa dem folgenden Codebeispiel:

    ```
     public class ContosoAudioStream : AudioInputStream {
        ContosoConfig config;

        public ContosoAudioStream(const ContosoConfig& config) {
            this.config = config;
        }

        public void GetFormat(AudioInputStreamFormat& format) {
            // returns format data to the caller.
            // e.g. format.FormatTag = config.XXX;
            // ...
        }

        public size_t Read(byte *buffer, size_t size) {
            // returns audio data to the caller.
            // e.g. return read(config.YYY, buffer, size);
        }

        public void Close() {
            // close and cleanup resources.
        }
     };
    ```

  - Verwenden Sie Ihren Audioeingabedatenstrom:

    ```
    var contosoStream = new ContosoAudioStream(contosoConfig);

    var factory = SpeechFactory.FromSubscription(...);
    var recognizer = CreateSpeechRecognizerWithStream(contosoStream);

    // run stream through recognizer
    var result = await recognizer.RecognizeAsync();

    var text = result.GetText();

    // In some languages you need to delete the stream explicitly.
    // delete contosoStream;
    ```

  - In einigen Sprachen muss `contosoStream` nach Abschluss der Erkennung ausdrücklich gelöscht werden. Sie können den Audiodatenstrom erst freigeben, nachdem die vollständige Eingabe gelesen wurde. In einem Szenario mit `StopContinuousRecognitionAsync` und `StopContinuousRecognitionAsync` ist ein Konzept erforderlich, das in folgendem Beispiel veranschaulicht wird:

    ```
    var contosoStream = new ContosoAudioStream(contosoConfig);

    var factory = SpeechFactory.FromSubscription(...);
    var recognizer = CreateSpeechRecognizerWithStream(contosoStream);

    // run stream through recognizer
    await recognizer.StartContinuousRecognitionAsync();

    // ERROR: do not delete the contosoStream before ending recognition!
    // delete contosoStream;

    await recognizer.StopContinuousRecognitionAsync();

    // OK: Safe to delete the contosoStream.
    // delete contosoStream;
    ```

## <a name="next-steps"></a>Nächste Schritte

* [Abrufen Ihres Testabonnements für Speech](https://azure.microsoft.com/try/cognitive-services/)
* [Erkennen von Sprache in C#](quickstart-csharp-windows.md)
