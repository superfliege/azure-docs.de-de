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
ms.date: 07/17/2018
ms.author: wolfma
ms.openlocfilehash: 50a8c183bd7f2711847ce6d0acade4cb498ef2fc
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2018
ms.locfileid: "39116094"
---
# <a name="release-notes"></a>Versionshinweise

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
