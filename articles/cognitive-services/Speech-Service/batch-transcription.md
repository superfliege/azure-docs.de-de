---
title: Verwenden von Batch-Transkriptionen – Speech Services
titlesuffix: Azure Cognitive Services
description: Batch-Transkriptionen eignen sich besonders, wenn Sie eine große Menge von Audiodaten in einen Speicher wie z.B. Azure-Blobs transkribieren möchten. Mithilfe der spezifischen Rest-API können Sie per SAS-URI (Shared Access Signature) auf Audiodateien verweisen und Transkriptionen asynchron empfangen.
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 2/20/2019
ms.author: panosper
ms.custom: seodec18
ms.openlocfilehash: 3b403eb80bae01efe730b69b7e6a5ddaea81355a
ms.sourcegitcommit: 6cab3c44aaccbcc86ed5a2011761fa52aa5ee5fa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2019
ms.locfileid: "56447649"
---
# <a name="why-use-batch-transcription"></a>Gründe für die Verwendung von Batch-Transkriptionen

Batch-Transkriptionen eignen sich besonders, wenn Sie eine große Menge von Audiodaten in einen Speicher wie z.B. Azure-Blobs transkribieren möchten. Mithilfe der spezifischen Rest-API können Sie per SAS-URI (Shared Access Signature) auf Audiodateien verweisen und Transkriptionen asynchron empfangen.

## <a name="prerequisites"></a>Voraussetzungen

### <a name="subscription-key"></a>Abonnementschlüssel

Wie bei allen Features des Speech-Diensts erstellen Sie mithilfe der Anleitung unter [Erste Schritte](get-started.md) einen Abonnementschlüssel im [Azure-Portal](https://portal.azure.com). Wenn Sie das Abrufen von Transkriptionen aus unseren Basismodellen planen, müssen Sie außer der Schlüsselerstellung nichts weiter tun.

>[!NOTE]
> Für die Verwendung von Batch-Transkriptionen ist ein Standardabonnement (S0) für Speech Services erforderlich. Kostenlose Abonnementschlüssel (F0) funktionieren nicht. Weitere Informationen finden Sie unter [Preise und Einschränkungen](https://azure.microsoft.com/en-us/pricing/details/cognitive-services/speech-services/).

### <a name="custom-models"></a>Benutzerdefinierte Modelle

Wenn Sie die Anpassung von Audio- oder Sprachmodellen planen, befolgen Sie die Schritte unter [Anpassen von Akustikmodellen](how-to-customize-acoustic-models.md) und [Erstellen eines benutzerdefinierten Sprachmodells](how-to-customize-language-model.md). Um die erstellten Modelle in der Batch-Transkription zu verwenden, benötigen Sie ihre Modell-IDs. Bei dieser ID handelt es sich nicht um die Endpunkt-ID, die in der Ansicht „Endpunktdetails“ angegeben ist, sondern um die Modell-ID. Diese können Sie abrufen, indem Sie die Details der Modelle auswählen.

## <a name="the-batch-transcription-api"></a>Die Batch-Transkriptions-API

Die Batch-Transkriptions-API bietet asynchrone Transkription von Sprache in Text sowie zusätzliche Funktionen. Es handelt sich um eine REST-API, die Methoden für Folgendes verfügbar macht:

1. Erstellen von Anforderungen für Batch-Verarbeitungen
1. Abfragestatus
1. Herunterladen von Transkriptionen

> [!NOTE]
> Die Batch-Transkriptions-API eignet sich ideal für Callcenter, bei denen sich üblicherweise Tausende von Stunden mit Audioaufzeichnungen ansammeln. Sie vereinfacht die Transkription großer Mengen von Audioaufnahmen.

### <a name="supported-formats"></a>Unterstützte Formate

Die Batch-Transkriptions-API unterstützt die folgenden Formate:

| Format | Codec | Bitrate | Samplingrate |
|--------|-------|---------|-------------|
| WAV | PCM | 16 Bit | 8 oder 16 kHz, Mono, Stereo |
| MP3 | PCM | 16 Bit | 8 oder 16 kHz, Mono, Stereo |
| OGG | OPUS | 16 Bit | 8 oder 16 kHz, Mono, Stereo |

Bei Audiostreams in Stereo teilt die Batch-Transkriptions-API den linken und rechten Kanal während des Transkriptionsvorgangs. Die beiden JSON-Dateien mit dem Ergebnis werden jeweils von einem einzigen Kanal erstellt. Die Zeitstempel pro Äußerung ermöglicht es dem Entwickler, eine geordnete endgültige Transkription zu erstellen. Diese Beispielanforderung umfasst Eigenschaften für die Filterung anstößiger Ausdrücke, Satzzeichen und Zeitstempel auf Wortebene. 

### <a name="configuration"></a>Konfiguration

Die Konfigurationsparameter werden als JSON angegeben:

```json
{
  "recordingsUrl": "<URL to the Azure blob to transcribe>",
 The correct way for specifying models is:
  "models": [{"Id":"<optional acoustic model ID>"},{"Id":"<optional language model ID>"}],
  "locale": "<local to us, for example en-US>",
  "name": "<user define name of the transcription batch>",
  "description": "<optional description of the transcription>",
  "properties": {
    "ProfanityFilterMode": "Masked",
    "PunctuationMode": "DictatedAndAutomatic",
    "AddWordLevelTimestamps" : "True"
  }
}
```

> [!NOTE]
> Die Batch-Transkriptions-API verwendet einen REST-Dienst zum Anfordern von Transkriptionen, deren Status und zugehörigen Ergebnissen. Sie können die API in jeder Sprache verwenden. Im nächsten Abschnitt wird die Verwendung der API beschrieben.

### <a name="configuration-properties"></a>Konfigurationseigenschaften

| Parameter | BESCHREIBUNG | Erforderlich/optional |
|-----------|-------------|---------------------|
| `ProfanityFilterMode` | Gibt den Umgang mit Obszönitäten in Erkennungsergebnissen an. Zulässige Werte sind `none` (deaktiviert den Obszönitätenfilter), `masked` (Obszönitäten werden durch Sternchen ersetzt), `removed` (Obszönitäten werden aus dem Ergebnis entfernt) und `tags` (fügt „Obszönität“-Tags ein). Die Standardeinstellung ist `masked`. | Optional |
| `PunctuationMode` | Gibt den Umgang mit Satzzeichen in Erkennungsergebnissen an. Gültige Werte sind `none` (deaktiviert Satzzeichen), `dictated` (explizite Satzzeichen), `automatic` (der Decoder verwaltet die Satzzeichen), `dictatedandautomatic` (vorgeschriebene Satzzeichen) oder „automatic“. | Optional |
 | `AddWordLevelTimestamps` | Gibt an, ob der Ausgabe Zeitstempel auf Wortebene hinzugefügt werden sollen. Gültige Werte sind `true` zum Aktivieren und `false` zum Deaktivieren von Zeitstempeln auf Wortebene. | Optional |

## <a name="sample-code"></a>Beispielcode

Das vollständige Beispiel steht im [GitHub-Beispielrepository](https://aka.ms/csspeech/samples) innerhalb des Unterverzeichnisses `samples/batch` zur Verfügung.

Sie müssen den Beispielcode mit Ihren Abonnementinformationen, der Dienstregion, dem SAS-URI mit Verweis auf die zu transkribierende Audiodatei und die Modell-IDs anpassen, falls Sie ein benutzerdefiniertes Audio- oder Sprachmodell verwenden möchten. 

[!code-csharp[Configuration variables for batch transcription](~/samples-cognitive-services-speech-sdk/samples/batch/csharp/program.cs#batchdefinition)]

Der Beispielcode richtet den Client ein und sendet die Transkriptionsanforderung. Anschließend fragt er die Statusinformationen ab und druckt Details über den Fortschritt der Transkription.

[!code-csharp[Code to check batch transcription status](~/samples-cognitive-services-speech-sdk/samples/batch/csharp/program.cs#batchstatus)]

Vollständige Informationen zu den vorherigen Aufrufen finden Sie in unserer [Swagger-Dokumentation](https://westus.cris.ai/swagger/ui/index) (in englischer Sprache). Das vollständige hier gezeigte Beispiel finden Sie auf [GitHub](https://aka.ms/csspeech/samples) im Unterverzeichnis `samples/batch`.

Beachten Sie das asynchrone Setup für das Senden von Audiodaten und das Empfangen des Transkriptionsstatus. Der von Ihnen erstellte Client ist ein .NET-HTTP-Client. Es gibt eine `PostTranscriptions`-Methode für das Senden der Audiodateidetails und eine `GetTranscriptions`-Methode zum Empfangen der Ergebnisse. `PostTranscriptions` gibt ein Handle zurück, und `GetTranscriptions` verwendet dieses Handle, um ein Handle zum Abrufen des Transkriptionsstatus zu erstellen.

Im aktuellen Beispielcode ist kein benutzerdefiniertes Modell angegeben. Der Dienst verwendet die Basismodelle zum Transkribieren der Datei bzw. Dateien. Zum Angeben der Modelle können Sie dieselbe Methode wie bei den Modell-IDs für das Akustikmodell und das Sprachmodell übergeben.

> [!NOTE]
> Für Basistranskriptionen müssen Sie die ID der Basismodelle nicht deklarieren. Wenn Sie nur eine Sprachmodell-ID (und keine Akustikmodell-ID) angeben, wird automatisch ein entsprechendes Akustikmodell ausgewählt. Wenn Sie nur eine Akustikmodell-ID angeben, wird automatisch ein entsprechendes Sprachmodell ausgewählt.

### <a name="supported-storage"></a>Unterstützter Speicher

Derzeit wird nur Azure-Blobspeicher unterstützt.

## <a name="download-the-sample"></a>Herunterladen des Beispiels

Sie finden das Beispiel im [GitHub-Beispielrepository](https://aka.ms/csspeech/samples) im Verzeichnis `samples/batch`.

> [!NOTE]
> Aufträge der Batch-Transkription werden auf bestmögliche Weise geplant. Es gibt keine geschätzte Zeit dafür, wann ein Auftrag in den Ausführungszustand wechselt. Sobald er ausgeführt wird, wird die aktuelle Transkription schneller als in Audioechtzeit verarbeitet.

## <a name="next-steps"></a>Nächste Schritte

* [Abrufen Ihres Testabonnements für Speech](https://azure.microsoft.com/try/cognitive-services/)
