---
title: Dokumentation zum Cognitive Services Speech SDK | Microsoft-Dokumentation
description: Versionshinweise – Änderungen in den neuesten Versionen
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 08/16/2018
ms.author: wolfma
ms.openlocfilehash: 0900588f818855d72e415678338c96fb6505318d
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2018
ms.locfileid: "41929755"
---
# <a name="release-notes"></a>Versionshinweise

## <a name="cognitive-services-speech-sdk-060-2018-august-release"></a>Cognitive Services Speech SDK 0.6.0: 2018-August-Release

**Neue Features**

* Mit dem Speech SDK erstellte UWP-Apps erfüllen nun die Anforderungen des Windows App Certification Kit (WACK).
  Sehen Sie sich unseren [UWP-Schnellstart](quickstart-csharp-uwp.md) an.
* Unterstützung für .NET Standard 2.0 unter Linux (Ubuntu 16.04 x 64)
* Experimentell: Unterstützung für Java 8 unter Windows (64 Bit) und Linux (Ubuntu 16.04 x 64).
  Sehen Sie sich den [Schnellstart zur Java Run-Time Environment](quickstart-java-jre.md) an.

**Funktionale Änderungen**

* Es werden weitere Detailinformationen zu Verbindungsfehlern verfügbar gemacht.

**Wichtige Änderungen**

* In Java (Android) erfordert die `SpeechFactory.configureNativePlatformBindingWithDefaultCertificate`-Funktion keinen Path-Parameter mehr. Der Pfad wird nun auf allen unterstützten Plattformen automatisch erkannt.
* Der get-Accessor der `EndpointUrl`-Eigenschaft in Java und C# wurde entfernt.

**Fehlerbehebungen**

* In Java werden die Ergebnisse der Audiosynthese in der Übersetzungserkennung jetzt implementiert.
* Ein Problem wurde behoben, das inaktive Threads und eine erhöhte Anzahl von offenen und nicht verwendeten Sockets verursachen konnte.
* Ein Problem wurde behoben, das dazu führen konnte, dass lange ausgeführte Erkennungen während der Übertragung beendet wurden.
* Eine Racebedingung beim Herunterfahren der Erkennung wurde behoben.

## <a name="cognitive-services-speech-sdk-050-2018-july-release"></a>Cognitive Services Speech SDK 0.5.0: 2018-July-Release

**Neue Features**

* Unterstützung für die Android-Plattform (API 23: Android 6.0 Marshmallow oder höher).
  Sehen Sie sich den [Android-Schnellstart](quickstart-java-android.md) an.
* Unterstützung für .NET Standard 2.0 unter Windows.
  Sehen Sie sich den [.NET Core-Schnellstart](quickstart-csharp-dotnetcore-windows.md) an.
* Experimentell: Unterstützung für UWP unter Windows (Version 1709 oder höher)
  * Sehen Sie sich unseren [UWP-Schnellstart](quickstart-csharp-uwp.md) an.
  * Hinweis: Mit dem Speech SDK erstellte UWP-Apps erfüllen die Anforderungen des Windows App Certification Kit (WACK) noch nicht.
* Unterstützung einer lang andauernden Erkennung mit automatischer erneuter Verbindungsherstellung.

**Funktionale Änderungen**

* `StartContinuousRecognitionAsync()` unterstützt eine lang andauernde Erkennung
* Das Erkennungsergebnis enthält mehr Felder: Versatz vom Audiobeginn und Dauer (beides in Teilstrichen) des erkannten Texts, weitere Werte, die den Erkennungsstatus darstellen, z.B. `InitialSilenceTimeout`, `InitialBabbleTimeout`.
* Unterstützung für AuthorizationToken zum Erstellen von Factoryinstanzen.

**Wichtige Änderungen**

* Erkennungsereignisse: NoMatch-Ereignistyp wird mit dem Fehlerereignis zusammengeführt.
* „SpeechOutputFormat“ in C# wird in „OutputFormat“ umbenannt, um mit C++ konsistent zu bleiben.
* Der Rückgabetyp einiger Methoden der `AudioInputStream`-Schnittstelle wurde geringfügig geändert:
   * In Java gibt die `read`-Methode jetzt `long` anstelle von `int` zurück.
   * In C# gibt die `Read`-Methode jetzt `uint` anstelle von `int` zurück.
   * In C++ geben die `Read`- und die `GetFormat`-Methoden jetzt `size_t` anstelle von `int` zurück.
* C++: Instanzen von Audio-Eingabedatenströmen können jetzt nur als `shared_ptr` übergeben werden.

**Fehlerbehebungen**

* Korrektur falscher Rückgabewerte im Ergebnis, wenn bei `RecognizeAsync()` ein Timeout auftritt.
* Die Abhängigkeit von Media Foundation-Bibliotheken für Windows wurde entfernt. Das SDK verwendet jetzt Core Audio-APIs.
* Korrektur der Dokumentation: Eine Regionsseite wurde hinzugefügt, um die unterstützten Regionen zu beschreiben.

**Bekannte Probleme**

* Das Speech SDK für Android meldet die Ergebnisse der Sprachsynthese für Übersetzungen nicht.
  Dies wird im nächsten Release behoben.

## <a name="cognitive-services-speech-sdk-040-2018-june-release"></a>Cognitive Services Speech SDK 0.4.0: 2018-June-Release

**Funktionale Änderungen**

- AudioInputStream

  Eine Erkennung kann jetzt einen Stream als Audioquelle nutzen. Ausführliche Informationen finden Sie in der zugehörigen [Anleitung](how-to-use-audio-input-streams.md).

- Detailliertes Ausgabeformat

  Beim Erstellen von `SpeechRecognizer` können Sie das Ausgabeformat `Detailed` oder `Simple` anfordern. `DetailedSpeechRecognitionResult` enthält eine Zuverlässigkeitsbewertung, erkannten Text, eine lexikalische Rohform, eine normalisierte Form und eine normalisierte Form mit maskierten anstößigen Ausdrücken.

**Wichtige Änderung**

- Änderung von `SpeechRecognitionResult.RecognizedText` in `SpeechRecognitionResult.Text` in C#.

**Fehlerbehebungen**

- Ein mögliches Rückrufproblem auf USP-Ebene beim Herunterfahren wurde behoben.

- Wenn eine Audioeingabedatei von einer Erkennung genutzt wurde, wurde das Dateihandle länger als erforderlich gespeichert.

- Mehrere Deadlocks zwischen dem Nachrichtensystem und der Erkennung wurden entfernt.

- Ein `NoMatch`-Ergebnis wird ausgelöst, wenn bei der Antwort vom Dienst ein Timeout auftritt.

- Die Media Foundation-Bibliotheken unter Windows werden verzögert geladen. Diese Bibliothek ist nur für die Mikrofoneingabe erforderlich.

- Die Uploadgeschwindigkeit für Audiodaten ist auf das Doppelte der ursprünglichen Audiogeschwindigkeit beschränkt.

- C# .NET-Assemblys haben jetzt unter Windows einen starken Namen.

- Korrektur der Dokumentation: `Region` ist eine erforderliche Information zum Erstellen einer Erkennung.

Weitere Beispiele wurden hinzugefügt und werden regelmäßig aktualisiert. Die Sammlung der aktuellsten Beispiele finden Sie im [GitHub-Repository mit Beispielen für das Speech SDK](https://aka.ms/csspeech/samples).

## <a name="cognitive-services-speech-sdk-0212733-2018-may-release"></a>Cognitive Services Speech SDK 0.2.12733: 2018-May-Release

Die erste öffentliche Vorschauversion des Cognitive Services Speech SDK.
