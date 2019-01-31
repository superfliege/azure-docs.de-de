---
title: Versionshinweise – Speech Services
titlesuffix: Azure Cognitive Services
description: Hier finden Sie eine fortlaufende Liste von veröffentlichten Features, Verbesserungen, Fehlerbehebungen und bekannten Problemen bei Azure Speech Services.
services: cognitive-services
author: wolfma61
manager: cgronlun
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/18/2018
ms.author: wolfma
ms.custom: seodec18
ms.openlocfilehash: 80cf0d9970eb7052eeb19fb297176453864202cf
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55205694"
---
# <a name="release-notes"></a>Versionshinweise

## <a name="speech-sdk-120-2018-december-release"></a>Speech SDK 1.2.0: Release von Dezember 2018

**Neue Features**

* Python
  * Die Betaversion der Python-Unterstützung (ab 3.5) ist mit diesem Release verfügbar. Weitere Informationen finden Sie [hier](quickstart-python.md).
* JavaScript
  * Das Speech SDK für JavaScript wird jetzt als Open-Source-Code bereitgestellt. Der Quellcode steht auf [GitHub](https://github.com/Microsoft/cognitive-services-speech-sdk-js)zur Verfügung.
  * Node.js wird jetzt unterstützt. Weitere Informationen finden Sie [hier](quickstart-js-node.md).
  * Die Längenbeschränkung für Audiositzungen wurde entfernt. Die Verbindungswiederherstellung erfolgt automatisch im Hintergrund.
* Verbindungsobjekt
  * Über die Erkennung können Sie auf ein Verbindungsobjekt zugreifen. Mit diesem Objekt können Sie die Dienstverbindung explizit initiieren und Verbindungsherstellungs- und Verbindungstrennungsereignisse abonnieren.
    (Für JavaScript und Python noch nicht verfügbar.)
* Unterstützung von Ubuntu 18.04
* Android
  * ProGuard-Unterstützung während der APK-Generierung aktiviert

**Verbesserungen**

* Verbesserungen bei der internen Threadverwendung (weniger Threads, Sperren, Mutexe)
* Verbesserte Fehlerberichterstellung/-informationen. In einigen Fällen wurden Fehlermeldungen nicht ordnungsgemäß verteilt.
* Entwicklungsabhängigkeiten in JavaScript wurden für die Verwendung aktueller Module aktualisiert.

**Fehlerbehebungen**

* Arbeitsspeicherverluste aufgrund eines Typenkonflikts in „RecognizeAsync“ behoben
* In einigen Fällen sind Ausnahmen verloren gegangen.
* Behebung des Arbeitsspeicherverlusts in Übersetzungsereignisargumenten
* Sperrproblem bei der Verbindungswiederherstellung in langen Sitzungen behoben
* Problem behoben, dass dazu führen konnte, dass das Endergebnis für fehlerhafte Übersetzungen verpasst wird
* C#: Wenn im Hauptthread nicht auf einen asynchronen Vorgang gewartet wurde, konnte es vorkommen, dass die Erkennung vor Abschluss der asynchronen Aufgabe entfernt wird.
* Java: Problem behoben, das zum Absturz des virtuellen Java-Computers geführt hat
* Objective-C: Enumerationszuordnung korrigiert (anstelle von „RecognizingIntent“ wurde „RecognizedIntent“ zurückgegeben)
* JavaScript: Standardausgabeformat in „SpeechConfig“ auf „einfach“ festgelegt
* JavaScript: Beseitigung der Inkonsistenz zwischen Eigenschaften des Konfigurationsobjekts in JavaScript und anderen Sprachen

**Beispiele**

* Mehrere Beispiele aktualisiert und korrigiert (unter anderem die Ausgabestimmen für die Übersetzung)
* Node.js-Beispiele zum [Beispielrepository](https://aka.ms/csspeech/samples) hinzugefügt

## <a name="speech-sdk-110"></a>Speech SDK 1.1.0

**Neue Features**

* Unterstützung für Android x86/x64.
* Proxyunterstützung: Im SpeechConfig-Objekt können Sie jetzt eine Funktion aufrufen, um die Proxyinformationen (Hostname, Port, Benutzername und Kennwort) festzulegen. Dieses Feature ist in iOS noch nicht verfügbar.
* Verbesserte Fehlercodes und Meldungen. Wenn eine Erkennung einen Fehler zurückgab, wurde dadurch bereits `Reason` (im abgebrochenen Ereignis) oder `CancellationDetails` (im Erkennungsergebnis) auf `Error` festgelegt. Das abgebrochene Ereignis enthält jetzt zwei zusätzliche Member: `ErrorCode` und `ErrorDetails`. Wenn der Server zusätzliche Fehlerinformationen mit dem Fehler zurückgibt, sind diese jetzt in den neuen Membern verfügbar.

**Verbesserungen**

* In der Konfiguration der Erkennung wurde eine zusätzliche Überprüfung hinzugefügt, und es wurde eine zusätzliche Fehlermeldung hinzugefügt.
* Die Verarbeitung von langen Pausen mitten in einer Audiodatei wurde verbessert.
* NuGet-Paket: Für .NET Framework-Projekte wird die Erstellung mit AnyCPU-Konfiguration verhindert.

**Fehlerbehebungen**

* In Erkennungen wurden verschiedene Ausnahmen behoben. Darüber hinaus werden Ausnahmen abgefangen und in abgebrochene Ereignisse konvertiert.
* Ein Arbeitsspeicherverlust in der Eigenschaftenverwaltung wurde behoben.
* Es wurde ein Fehler behoben, bei dem eine Audioeingabedatei zum Absturz der Erkennung führen konnte.
* Es wurde ein Fehler behoben, bei dem nach dem Ereignis zum Beenden einer Sitzung weiter Ereignisse empfangen werden konnten.
* Einige Racebedingungen im Threading wurden korrigiert.
* Ein iOS-Kompatibilitätsproblem wurde behoben, das zu einem Absturz führen konnte.
* Verbesserungen bei der Stabilität für die Android-Mikrofonunterstützung.
* Es wurde ein Fehler behoben, bei dem eine Erkennung in JavaScript die Erkennungssprache ignorierte.
* Es wurde ein Fehler behoben, der (in einigen Fällen) das Festlegen der EndpointId in JavaScript verhinderte.
* Die Parameterreihenfolge in AddIntent in JavaScript wurde geändert, und es wurde eine fehlende JavaScript-Signatur für AddIntent hinzugefügt.

**Beispiele**

* Dem [Beispielrepository](https://aka.ms/csspeech/samples) wurden C++- und C#-Beispiele für die Verwendung von Pull- und Pushstreams hinzugefügt.

## <a name="speech-sdk-101"></a>Speech SDK 1.0.1

Verbesserte Zuverlässigkeit und Fehlerbehebungen:

* Ein potenziell schwerwiegender Fehler aufgrund einer Racebedingung bei der Löscherkennung wurde behoben.
* Ein potenziell schwerwiegender Fehler bei einer nicht festgelegten Eigenschaft wurde behoben.
* Zusätzliche Fehler- und Parameterüberprüfungen wurden hinzugefügt.
* Objective-C: Ein potenziell schwerwiegender Fehler durch Namensüberschreibungen in NSString wurde behoben.
* Objective-C: Sichtbarkeit der API wurde angepasst
* JavaScript: Korrektur in Bezug auf Ereignisse und deren Nutzlasten.
* Verbesserungen bei der Dokumentation.

Im [Beispielrepository](https://aka.ms/csspeech/samples) wurde ein neues Beispiel für JavaScript hinzugefügt.

## <a name="cognitive-services-speech-sdk-100-2018-september-release"></a>Cognitive Services Speech SDK 1.0.0: Release von September 2018

**Neue Features**

* Unterstützung für Objective-C unter iOS. Sehen Sie sich unseren [Objective-C-Schnellstart für iOS](quickstart-objectivec-ios.md) an.
* Unterstützung für JavaScript im Browser. Sehen Sich unseren [JavaScript-Schnellstart](quickstart-js-browser.md) an.

**Wichtige Änderungen**

* Mit diesem Release werden einige Breaking Changes eingeführt.
  Ausführliche Informationen finden Sie auf [dieser Seite](https://aka.ms/csspeech/breakingchanges_1_0_0).

## <a name="cognitive-services-speech-sdk-060-2018-august-release"></a>Cognitive Services Speech SDK 0.6.0: Release von August 2018

**Neue Features**

* Mit dem Speech SDK erstellte UWP-Apps erfüllen nun die Anforderungen des Windows App Certification Kit (WACK).
  Sehen Sie sich den [UWP-Schnellstart](quickstart-csharp-uwp.md) an.
* Unterstützung für .NET Standard 2.0 unter Linux (Ubuntu 16.04 x 64)
* Experimentell: Unterstützung für Java 8 unter Windows (64 Bit) und Linux (Ubuntu 16.04 x64).
  Sehen Sie sich den [Schnellstart zur Java Runtime Environment](quickstart-java-jre.md) an.

**Funktionale Änderung**

* Es werden weitere Detailinformationen zu Verbindungsfehlern verfügbar gemacht.

**Wichtige Änderungen**

* In Java (Android) erfordert die `SpeechFactory.configureNativePlatformBindingWithDefaultCertificate`-Funktion keinen Path-Parameter mehr. Der Pfad wird nun auf allen unterstützten Plattformen automatisch erkannt.
* Der get-Accessor der `EndpointUrl`-Eigenschaft in Java und C# wurde entfernt.

**Fehlerbehebungen**

* In Java werden die Ergebnisse der Audiosynthese in der Übersetzungserkennung jetzt implementiert.
* Ein Problem wurde behoben, das inaktive Threads und eine erhöhte Anzahl von offenen und nicht verwendeten Sockets verursachen konnte.
* Ein Problem wurde behoben, das dazu führen konnte, dass lange ausgeführte Erkennungen während der Übertragung beendet wurden.
* Eine Racebedingung beim Herunterfahren der Erkennung wurde behoben.

## <a name="cognitive-services-speech-sdk-050-2018-july-release"></a>Cognitive Services Speech SDK 0.5.0: Release von Juli 2018

**Neue Features**

* Unterstützung für Android-Plattform (API 23: Android 6.0 Marshmallow oder höher). Sehen Sie sich den [Android-Schnellstart](quickstart-java-android.md) an.
* Unterstützung für .NET Standard 2.0 unter Windows. Sehen Sie sich den [.NET Core-Schnellstart](quickstart-csharp-dotnetcore-windows.md) an.
* Experimentell: Unterstützung für UWP unter Windows (Version 1709 oder höher).
  * Sehen Sie sich den [UWP-Schnellstart](quickstart-csharp-uwp.md) an.
  * Hinweis: Mit dem Speech SDK erstellte UWP-Apps erfüllen die Anforderungen des Windows App Certification Kit (WACK) noch nicht.
* Unterstützung einer lang andauernden Erkennung mit automatischer erneuter Verbindungsherstellung.

**Funktionale Änderungen**

* `StartContinuousRecognitionAsync()` unterstützt eine lang andauernde Erkennung.
* Das Erkennungsergebnis enthält mehr Felder. Versatz vom Audiobeginn und Dauer (beides in Takten) des erkannten Texts und weitere Werte, die den Erkennungsstatus darstellen, z.B. `InitialSilenceTimeout` und `InitialBabbleTimeout`.
* Unterstützung für AuthorizationToken zum Erstellen von Factoryinstanzen.

**Wichtige Änderungen**

* Erkennungsereignisse: Der NoMatch-Ereignistyp wurde mit dem Error-Ereignis zusammengeführt.
* SpeechOutputFormat in C# wurde in OutputFormat umbenannt, um mit C++ konsistent zu bleiben.
* Der Rückgabetyp einiger Methoden der `AudioInputStream`-Schnittstelle wurde geringfügig geändert:
   * In Java gibt die `read`-Methode jetzt `long` anstelle von `int` zurück.
   * In C# gibt die `Read`-Methode jetzt `uint` anstelle von `int` zurück.
   * In C++ geben die `Read`- und die `GetFormat`-Methoden jetzt `size_t` anstelle von `int` zurück.
* C++: Instanzen von Audioeingabestreams können jetzt nur als `shared_ptr` übergeben werden.

**Fehlerbehebungen**

* Korrektur falscher Rückgabewerte im Ergebnis, wenn bei `RecognizeAsync()` ein Timeout auftritt.
* Die Abhängigkeit von Media Foundation-Bibliotheken für Windows wurde entfernt. Das SDK verwendet jetzt die Core Audio-APIs.
* Korrektur der Dokumentation: Eine Seite [Regionen](regions.md) wurde hinzugefügt, um die unterstützten Regionen zu beschreiben.

**Bekanntes Problem**

* Das Speech SDK für Android meldet die Ergebnisse der Sprachsynthese für Übersetzungen nicht. Dieses Problem wird im nächsten Release behoben.

## <a name="cognitive-services-speech-sdk-040-2018-june-release"></a>Cognitive Services Speech SDK 0.4.0: Release von Juni 2018

**Funktionale Änderungen**

- AudioInputStream

  Eine Erkennung kann jetzt einen Stream als Audioquelle nutzen. Weitere Informationen finden Sie in der zugehörigen [Schrittanleitung](how-to-use-audio-input-streams.md).

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

- C# .NET-Assemblys haben unter Windows nun einen starken Namen.

- Korrektur der Dokumentation: `Region` ist eine erforderliche Information zum Erstellen einer Erkennung.

Weitere Beispiele wurden hinzugefügt und werden regelmäßig aktualisiert. Die Sammlung der aktuellsten Beispiele finden Sie im [GitHub-Repository mit Beispielen für das Speech SDK](https://aka.ms/csspeech/samples).

## <a name="cognitive-services-speech-sdk-0212733-2018-may-release"></a>Cognitive Services Speech SDK 0.2.12733: Release von Mai 2018

Dieses Release ist erste öffentliche Vorschauversion des Cognitive Services Speech SDK.
