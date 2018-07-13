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
ms.date: 06/07/2018
ms.author: wolfma
ms.openlocfilehash: 0b1559d288380cf3d0c180a225278cc13d22a5d0
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2018
ms.locfileid: "35378995"
---
# <a name="release-notes"></a>Versionshinweise

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
