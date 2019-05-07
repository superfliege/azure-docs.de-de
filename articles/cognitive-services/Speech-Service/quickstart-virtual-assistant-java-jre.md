---
title: 'Schnellstart: Benutzerdefinierter Voice-First-Assistent (Vorschauversion), Java (Windows, Linux): Speech-Dienste'
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie das Cognitive Services Speech SDK (Software Development Kit) in einer Java-Konsolenanwendung verwenden. Sie lernen, wie Sie Ihre Clientanwendung mit einem zuvor erstellten Bot Framework-Bot verbinden, der für die Verwendung des Direct Line Speech-Kanals konfiguriert ist, und einen virtuellen Voice-First-Assistenten implementieren.
services: cognitive-services
author: bidishac
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 05/02/2019
ms.author: bidishac
ms.openlocfilehash: 4e9010bed54d0b2a7cb1a95b9e01e5ba02ea9fd5
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/02/2019
ms.locfileid: "65025365"
---
# <a name="quickstart-create-a-voice-first-virtual-assistant-with-the-speech-sdk-java"></a>Schnellstart: Erstellen eines virtuellen Voice-First-Assistenten mit dem Speech SDK, Java

In diesem Artikel erstellen Sie eine Java-Konsolenanwendung unter Verwendung des [Cognitive Services Speech SDK](speech-sdk.md). Die Anwendung stellt eine Verbindung mit einem zuvor erstellten Bot her, der für die Verwendung des Direct Line Speech-Kanals konfiguriert ist, sendet eine Sprachanforderung, und gibt eine Sprachantwortaktivität zurück (sofern konfiguriert). Die Anwendung wird mit dem Maven-Paket für das Speech SDK sowie mit der Eclipse Java-IDE unter Windows, Ubuntu Linux oder macOS erstellt. Sie wird in einer 64-Bit Java 8-JRE (Runtime Environment) ausgeführt.

## <a name="prerequisites"></a>Voraussetzungen

Für diese Schnellstartanleitung ist Folgendes erforderlich:

* Betriebssystem: Windows (64 Bit), Ubuntu Linux 16.04/18.04 (64 Bit) oder macOS (ab Version 10.13)
* [Eclipse Java IDE](https://www.eclipse.org/downloads/)
* [Java 8](https://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) oder [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* Ein Azure-Abonnementschlüssel für den Speech-Dienst. [Hier erhalten Sie einen kostenlosen Schlüssel.](get-started.md)
* Ein vorkonfigurierter Bot, der mit der Bot Framework-Version 4.2 oder einer höheren Version erstellt wurde. Der Bot muss den neuen Kanal „Direct Line Speech“ abonnieren, um Spracheingaben zu empfangen.

    > [!NOTE]
    > In der Vorschauversion unterstützt der Direct Line Speech-Kanal derzeit nur die Region **westus2**.

    > [!NOTE]
    > Die 30-tägige kostenlose Testversion für den Standard-Tarif, die unter [Kostenloses Testen von Speech Services](get-started.md) beschrieben ist, ist auf **westus** (nicht **westus2**) beschränkt und daher nicht mit Direct Line Speech kompatibel. Abonnements für **westus2** im Free- und Standard-Tarif sind kompatibel.

Vergewissern Sie sich bei Verwendung von Ubuntu 16.04/18.04, dass die folgenden Abhängigkeiten installiert sind, bevor Sie Eclipse starten:

```console
sudo apt-get update
sudo apt-get install build-essential libssl1.0.0 libasound2 wget
```

Vergewissern Sie sich bei Verwendung von Windows (64 Bit), dass Microsoft Visual C++ Redistributable für Ihre Plattform installiert ist:
* [Microsoft Visual C++ Redistributable für Visual Studio 2017 herunterladen](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads)

## <a name="optional-get-started-fast"></a>Optional: Schneller Einstieg

In dieser Schnellstartanleitung erfahren Sie Schritt für Schritt, wie Sie eine einfache Clientanwendung erstellen, die Sie mit Ihrem sprachaktivierten Bot verbinden können. Wenn Sie lieber sofort loslegen möchten, finden Sie den kompilierbereiten Quellcode, der in dieser Schnellstartanleitung verwendet wird, in den [Speech SDK-Beispielen](https://aka.ms/csspeech/samples) im Ordner `quickstart`.

## <a name="create-and-configure-project"></a>Erstellen und Konfigurieren des Projekts

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-java-create-proj.md)]

Fügen Sie außerdem der Datei **pom.xml** die folgende Abhängigkeit hinzu, um die Protokollierung zu aktivieren:

   ```xml
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-simple</artifactId>
        <version>1.7.5</version>
    </dependency>
   ```

## <a name="add-sample-code"></a>Hinzufügen von Beispielcode

1. Um Ihrem Java-Projekt eine neue leere Klasse hinzuzufügen, wählen Sie **Datei** > **Neu** > **Klasse** aus.

1. Geben Sie im Fenster **Neue Java-Klasse** **speechsdk.quickstart** in das Feld **Paket** und **Main** in das Feld **Name** ein.

   ![Screenshot des Fensters „Neue Java-Klasse“](media/sdk/qs-java-jre-06-create-main-java.png)

1. Öffnen Sie die neu erstellte Klasse **Main**, und ersetzen Sie den Inhalt der Datei `Main.java` durch den folgenden Startcode:

    ```java
    package speechsdk.quickstart;

    import java.io.IOException;
    import java.io.PipedOutputStream;
    import java.util.HashMap;

    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;

    import com.microsoft.cognitiveservices.speech.ResultReason;
    import com.microsoft.cognitiveservices.speech.audio.AudioConfig;
    import com.microsoft.cognitiveservices.speech.dialog.BotConnectorConfig;
    import com.microsoft.cognitiveservices.speech.dialog.SpeechBotConnector;

    public class Main {
        final Logger log = LoggerFactory.getLogger(Main.class);

        public static void main(String[] args) {
            // New code will go here
        }
    }
    ```

1. In der Methode **main** müssen Sie zunächst `BotConnectorConfig` konfigurieren und damit eine `SpeechBotConnector`-Instanz erstellen. Dadurch wird eine Verbindung mit dem Direct Line Speech-Kanal für die Interaktion mit Ihrem Bot hergestellt. Außerdem wird eine `AudioConfig`-Instanz verwendet, um die Quelle für die Audioeingabe anzugeben. In diesem Beispiel wird mit `AudioConfig.fromDefaultMicrophoneInput()` das Standardmikrofon verwendet.

    * Ersetzen Sie die Zeichenfolge `YourSubscriptionKey` durch Ihren Abonnementschlüssel ([hier](get-started.md) verfügbar).
    * Ersetzen Sie die Zeichenfolge `YourServiceRegion` durch die mit Ihrem Abonnement verknüpfte [Region](regions.md).
    * Ersetzen Sie die Zeichenfolge `YourChannelSecret` durch Ihr Geheimnis für den Direct Line Speech-Kanal.

    > [!NOTE]
    > In der Vorschauversion unterstützt der Direct Line Speech-Kanal derzeit nur die Region **westus2**.

    > [!NOTE]
    > Die 30-tägige kostenlose Testversion für den Standard-Tarif, die unter [Kostenloses Testen von Speech Services](get-started.md) beschrieben ist, ist auf **westus** (nicht **westus2**) beschränkt und daher nicht mit Direct Line Speech kompatibel. Abonnements für **westus2** im Free- und Standard-Tarif sind kompatibel.

    ```java
    final String channelSecret = "YourChannelSecret"; // Your channel secret
    final String subscriptionKey = "YourSubscriptionKey"; // your subscription key
    final String region = "YourServiceRegion"; // Your service region. Currently assumed to be westus2
    final BotConnectorConfig botConnectorConfig = BotConnectorConfig.fromSecretKey(channelSecret, subscriptionKey, region);

    // Configure audio input from microphone.
    final AudioConfig audioConfig = AudioConfig.fromDefaultMicrophoneInput();

    // Create a SpeechjBotConnector instance
    final SpeechBotConnector botConnector = new SpeechBotConnector(botConnectorConfig, audioConfig);
    ```

1. `SpeechBotConnector` nutzt mehrere Ereignisse, um Botaktivitäten, Spracherkennungsergebnisse und andere Informationen zu kommunizieren. Fügen Sie daher als Nächstes die folgenden Ereignislistener hinzu:

    ```java
    // Recognizing will provide the intermediate recognized text while an audio stream is being processed
    botConnector.recognizing.addEventListener((o, speechRecognitionResultEventArgs) -> {
        log.info("Recognizing speech event text: {}", speechRecognitionResultEventArgs.getResult().getText());
    });

    // Recognized will provide the final recognized text once audio capture is completed
    botConnector.recognized.addEventListener((o, speechRecognitionResultEventArgs) -> {
        log.info("Recognized speech event reason text: {}", speechRecognitionResultEventArgs.getResult().getText());
    });

    // SessionStarted will notify when audio begins flowing to the service for a turn
    botConnector.sessionStarted.addEventListener((o, sessionEventArgs) -> {
        log.info("Session Started event id: {} ", sessionEventArgs.getSessionId());
    });

    // SessionStopped will notify when a turn is complete and it's safe to begin listening again
    botConnector.sessionStopped.addEventListener((o, sessionEventArgs) -> {
        log.info("Session stopped event id: {}", sessionEventArgs.getSessionId());
    });

    // Canceled will be signaled when a turn is aborted or experiences an error condition
    botConnector.canceled.addEventListener((o, canceledEventArgs) -> {
        log.info("Canceled event details: {}", canceledEventArgs.getErrorDetails());
        botConnector.disconnectAsync();
    });

    // ActivityReceived is the main way your bot will communicate with the client and uses bot framework activities
    botConnector.activityReceived.addEventListener((o, activityEventArgs) -> {
        String act = activityEventArgs.getActivity().serialize();
        log.info("Received activity: {}", act);
    });
    ```

1. Rufen Sie die Methode `connectAsync()` auf, um `SpeechBotConnector` mit Direct Line Speech zu verbinden. Um Ihren Bot zu testen, können Sie die Methode `listenOnceAsync` aufrufen und eine Audioeingabe von Ihrem Mikrofon senden. Außerdem können Sie die Methode `sendActivityAsync` verwenden, um eine benutzerdefinierte Aktivität als serialisierte Zeichenfolge zu senden. Diese benutzerdefinierten Aktivitäten können zusätzliche Daten bereitstellen, die Ihr Bot in der Konversation verwendet.

    ```java
    botConnector.connectAsync();
    // Start listening.
    System.out.println("Say something ...");
    botConnector.listenOnceAsync();

    // botConnector.sendActivityAsync(...)
    ```

1. Speichern Sie die Änderungen an der Datei `Main`.

1. Zur Unterstützung der Antwortwiedergabe müssen Sie eine zusätzliche Klasse hinzufügen, die Hilfsmethoden für die Audiounterstützung enthält. Fügen Sie Ihrem Java-Projekt eine neue leere Klasse hinzu, um die Verwendung von Audio zu ermöglichen. Wählen Sie hierzu **Datei** > **Neu** > **Klasse** aus.

1. Geben Sie im Fenster **Neue Java-Klasse** die Zeichenfolge **speechsdk.quickstart** in das Feld **Paket** und **AudioPlayer** in das Feld **Name** ein.

   ![Screenshot des Fensters „Neue Java-Klasse“](media/sdk/qs-java-jre-06-create-main-java.png)

1. Öffnen Sie die neu erstellte Klasse **AudioPlayer**, und ersetzen Sie den Inhalt durch den folgenden Code:

    ```java
    import static javax.sound.sampled.AudioFormat.Encoding.PCM_SIGNED;

    import java.io.InputStream;
    import java.io.PipedInputStream;
    import java.io.PipedOutputStream;
    import java.util.concurrent.ExecutorService;
    import java.util.concurrent.Executors;
    import java.util.concurrent.atomic.AtomicBoolean;

    import javax.sound.sampled.AudioFormat;
    import javax.sound.sampled.AudioSystem;
    import javax.sound.sampled.DataLine;
    import javax.sound.sampled.SourceDataLine;

    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;


    public class AudioPlayer {

        public static final int SAMPLE_RATE = 16000; // 16Hz sampling rate
        public static final int SAMPLE_SIZE_IN_BITS = 16; // 16 bit PCM
        public static final int CHANNELS = 1; // Use Mono / Single channel

        public static final int FRAME_RATE = 16000;
        public static final int FRAME_SIZE = 2;

        private static final Logger log = LoggerFactory.getLogger(AudioPlayer.class);
        private AtomicBoolean isPlaying = new AtomicBoolean(false);
        private ExecutorService executorService = Executors.newSingleThreadExecutor();

        public boolean isPlaying() {
            return isPlaying.get();
        }

        public void stopPlaying() {
            isPlaying.set(false);
        }

        public void play(final PipedOutputStream pipedOutputStream) {
            // The current audio supported by the Microsoft Bot framework ~ 16-bit PCM encoding, 16KHz sampling rate.
            final AudioFormat defaultFormat = new AudioFormat(PCM_SIGNED, SAMPLE_RATE, SAMPLE_SIZE_IN_BITS, CHANNELS, FRAME_SIZE, FRAME_RATE, false);
            try {
                final PipedInputStream inputStream = new PipedInputStream(pipedOutputStream);

                executorService.submit(() -> {
                    try {
                        isPlaying.set(true);
                        play(inputStream, defaultFormat);
                        inputStream.close();
                    } catch (Exception e) {
                        log.error("Exception thrown during playback. Message: {}", e.getMessage(), e);
                    }
                });
            } catch (Exception e) {
                log.error("Exception thrown during playback. Message: {}", e.getMessage(), e);
            }
        }

        private void play(final InputStream inputStream, final AudioFormat targetFormat) throws Exception {
            final byte[] buffer = new byte[1024];
            final DataLine.Info info = new DataLine.Info(SourceDataLine.class, targetFormat);
            final SourceDataLine line = (SourceDataLine) AudioSystem.getLine(info);
            line.open();
            if (line != null) {
                line.start();
                int bytesRead = 0;
                while (bytesRead != -1) {
                    bytesRead = inputStream.read(buffer, 0, buffer.length);
                    if (bytesRead != -1) {
                        line.write(buffer, 0, bytesRead);
                    }
                }
                line.drain();
                line.stop();
                line.close();
            }
        }
    }
    ```

1. Speichern Sie die Änderungen an der Datei `AudioPlayer`.

## <a name="build-and-run-the-app"></a>Erstellen und Ausführen der App

Drücken Sie F11, oder wählen Sie **Ausführen** > **Debuggen** aus.
In der Konsole wird „Say something“ (Sagen Sie etwas) angezeigt. Nun können Sie einen englischen Ausdruck oder Satz sprechen, der von Ihrem Bot verstanden wird. Das Gesagte wird über den Direct Line Speech-Kanal an Ihren Bot übermittelt, der das Gesagte erkennt und verarbeitet, bevor die Antwort als Aktivität zurückgegeben wird. Wenn Ihr Bot eine Sprachantwort zurückgibt, erfolgt die Audiowiedergabe unter Verwendung der Klasse `AudioPlayer`.

![Screenshot der Konsolenausgabe nach erfolgreicher Erkennung](media/sdk/qs-java-jre-07-console-output.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Beispiele, z.B. das Auslesen von Sprache aus einer Audiodatei, sind auf GitHub verfügbar.

> [!div class="nextstepaction"]
> [Java-Beispiele auf GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Weitere Informationen

- [Schnellstart: Übersetzen von Sprache, Java (Windows, Linux)](quickstart-translate-speech-java-jre.md)
- [Tutorial: Erstellen eines benutzerdefinierten Akustikmodells](how-to-customize-acoustic-models.md)
- [Tutorial: Erstellen eines benutzerdefinierten Sprachmodells](how-to-customize-language-model.md)
