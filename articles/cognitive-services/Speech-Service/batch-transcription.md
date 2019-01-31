---
title: Verwenden von Batch-Transkriptionen – Speech Services
titlesuffix: Azure Cognitive Services
description: Batch-Transkriptionen eignen sich besonders, wenn Sie eine große Menge von Audiodaten in einen Speicher wie z.B. Azure-Blobs transkribieren möchten. Mithilfe der spezifischen Rest-API können Sie per SAS-URI (Shared Access Signature) auf Audiodateien verweisen und Transkriptionen asynchron empfangen.
services: cognitive-services
author: PanosPeriorellis
manager: cgronlun
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: panosper
ms.custom: seodec18
ms.openlocfilehash: bf89180ea98473d2da3495286396a12c6f25288f
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55228660"
---
# <a name="why-use-batch-transcription"></a>Gründe für die Verwendung von Batch-Transkriptionen

Batch-Transkriptionen eignen sich besonders, wenn Sie eine große Menge von Audiodaten in einen Speicher wie z.B. Azure-Blobs transkribieren möchten. Mithilfe der spezifischen Rest-API können Sie per SAS-URI (Shared Access Signature) auf Audiodateien verweisen und Transkriptionen asynchron empfangen.

>[!NOTE]
> Für die Verwendung von Batch-Transkriptionen ist ein Standardabonnement (S0) für Speech Services erforderlich. Kostenlose Abonnementschlüssel (F0) funktionieren nicht. Weitere Informationen finden Sie unter [Preise und Einschränkungen](https://azure.microsoft.com/en-us/pricing/details/cognitive-services/speech-services/).

## <a name="the-batch-transcription-api"></a>Die Batch-Transkriptions-API

Die Batch-Transkriptions-API bietet asynchrone Transkription von Sprache in Text sowie zusätzliche Funktionen. Es handelt sich um eine REST-API, die Methoden für Folgendes verfügbar macht:

1. Erstellen von Anforderungen für Batch-Verarbeitungen
1. Abfragestatus
1. Herunterladen von Transkriptionen

> [!NOTE]
> Die Batch-Transkriptions-API eignet sich ideal für Callcenter, bei denen sich üblicherweise Tausende von Stunden mit Audioaufzeichnungen ansammeln. Aufgrund der eigenständigen Funktionsweise dieser API können große Mengen von Audioaufnahmen problemlos transkribiert werden.

### <a name="supported-formats"></a>Unterstützte Formate

Die Batch-Transkriptions-API unterstützt die folgenden Formate:

| Format | Codec | Bitrate | Samplingrate |
|--------|-------|---------|-------------|
| WAV | PCM | 16 Bit | 8 oder 16 kHz, Mono, Stereo |
| MP3 | PCM | 16 Bit | 8 oder 16 kHz, Mono, Stereo |
| OGG | OPUS | 16 Bit | 8 oder 16 kHz, Mono, Stereo |

> [!NOTE]
> Die Batch-Transkriptions-API erfordert einen S0-Schlüssel (Bezahltarif). Sie funktioniert nicht mit einem kostenlosen Schlüssel (f0).

Bei Audiostreams in Stereo teilt die Batch-Transkriptions-API den linken und rechten Kanal während des Transkriptionsvorgangs. Die beiden JSON-Dateien mit dem Ergebnis werden jeweils von einem einzigen Kanal erstellt. Die Zeitstempel pro Äußerung ermöglicht es dem Entwickler, eine geordnete endgültige Transkription zu erstellen. Das folgende JSON-Beispiel zeigt die Ausgabe eines Kanals, einschließlich der Eigenschaften für die Einrichtung eines Filters für anstößige Ausdrücke und des Modells für die Zeichensetzung.

```json
{
  "recordingsUrl": "https://contoso.com/mystoragelocation",
  "models": [],
  "locale": "en-US",
  "name": "Transcription using locale en-US",
  "description": "An optional description of the transcription.",
  "properties": {
    "ProfanityFilterMode": "Masked",
    "PunctuationMode": "DictatedAndAutomatic"
  },
```

> [!NOTE]
> Die Batch-Transkriptions-API verwendet einen REST-Dienst zum Anfordern von Transkriptionen, deren Status und zugehörigen Ergebnissen. Sie können die API in jeder Sprache verwenden. Im nächsten Abschnitt wird die Verwendung der API beschrieben.

### <a name="query-parameters"></a>Abfrageparameter

Diese Parameter können in der Abfragezeichenfolge der REST-Anforderung enthalten sein.

| Parameter | BESCHREIBUNG | Erforderlich/optional |
|-----------|-------------|---------------------|
| `ProfanityFilterMode` | Gibt den Umgang mit Obszönitäten in Erkennungsergebnissen an. Zulässige Werte sind `none` (deaktiviert den Obszönitätenfilter), `masked` (Obszönitäten werden durch Sternchen ersetzt), `removed` (Obszönitäten werden aus dem Ergebnis entfernt) und `tags` (fügt „Obszönität“-Tags ein). Die Standardeinstellung ist `masked`. | Optional |
| `PunctuationMode` | Gibt den Umgang mit Satzzeichen in Erkennungsergebnissen an. Gültige Werte sind `none` (deaktiviert Satzzeichen), `dictated` (explizite Satzzeichen), `automatic` (der Decoder verwaltet die Satzzeichen), `dictatedandautomatic` (vorgeschriebene Satzzeichen) oder „automatic“. | Optional |


## <a name="authorization-token"></a>Autorisierungstoken

Wie bei allen Features des Speech-Diensts erstellen Sie mithilfe der Anleitung unter [Erste Schritte](get-started.md) einen Abonnementschlüssel im [Azure-Portal](https://portal.azure.com). Wenn Sie das Abrufen von Transkriptionen aus unseren Basismodellen planen, müssen Sie außer der Schlüsselerstellung nichts weiter tun.

Wenn Sie planen, ein eigenes Modell zu erstellen und zu verwenden, fügen Sie den Abonnementschlüssel mithilfe der folgenden Schritte zum Custom Speech-Portal hinzu:

1. Melden Sie sich bei [Custom Speech](https://customspeech.ai) an.

2. Wählen Sie oben rechts **Abonnements** aus.

3. Wählen Sie **Verbindung mit vorhandenem Abonnement herstellen** aus.

4. Fügen Sie im Popupfenster den Abonnementschlüssel und einen Alias hinzu.

    ![Fenster „Abonnement hinzufügen“](media/stt/Subscriptions.jpg)

5. Kopieren Sie diesen Schlüssel, und fügen Sie ihn in den Clientcode im folgenden Beispiel ein.

> [!NOTE]
> Wenn Sie ein benutzerdefiniertes Modell verwenden möchten, benötigen Sie auch die ID dieses Modells. Diese ID ist nicht die Endpunkt-ID, die in der Ansicht „Endpunktdetails“ angegeben ist. Es ist die Modell-ID, die Sie durch Klicken auf die Details dieses Modells abrufen können.

## <a name="sample-code"></a>Beispielcode

Passen Sie den folgenden Beispielcode mit einem Abonnementschlüssel und einem API-Schlüssel an. Mit dieser Aktion können Sie ein Bearertoken abrufen.

```cs
     public static CrisClient CreateApiV2Client(string key, string hostName, int port)

        {
            var client = new HttpClient();
            client.Timeout = TimeSpan.FromMinutes(25);
            client.BaseAddress = new UriBuilder(Uri.UriSchemeHttps, hostName, port).Uri;
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", key);

            return new CrisClient(client);
        }
```

Nachdem Sie das Token abgerufen haben, geben Sie die SAS-URI an, die auf die Audiodatei zeigt, für die eine Transkription erforderlich ist. Der Rest des Codes durchläuft den Status und zeigt die Ergebnisse an. Zuerst richten Sie den Schlüssel, die Region, die zu verwendenden Modelle und die Sicherheitszuordnung ein, wie im folgenden Codeausschnitt gezeigt. Als Nächstes instanziieren Sie den Client und die POST-Anforderung.

```cs
            private const string SubscriptionKey = "<your Speech subscription key>";
            private const string HostName = "westus.cris.ai";
            private const int Port = 443;

            // SAS URI
            private const string RecordingsBlobUri = "SAS URI pointing to the file in Azure Blob Storage";

            // adapted model Ids
            private static Guid AdaptedAcousticId = new Guid("guid of the acoustic adaptation model");
            private static Guid AdaptedLanguageId = new Guid("guid of the language model");

            // Creating a Batch Transcription API Client
            var client = CrisClient.CreateApiV2Client(SubscriptionKey, HostName, Port);

            var transcriptionLocation = await client.PostTranscriptionAsync(Name, Description, Locale, new Uri(RecordingsBlobUri), new[] { AdaptedAcousticId, AdaptedLanguageId }).ConfigureAwait(false);
```

Nachdem Sie die Anforderung gesendet haben, können Sie nun die Transkriptionsergebnisse abfragen und herunterladen, wie im folgenden Codeausschnitt gezeigt:

```cs

            // get all transcriptions for the user
            transcriptions = await client.GetTranscriptionAsync().ConfigureAwait(false);

            // for each transcription in the list we check the status
            foreach (var transcription in transcriptions)
            {
                switch(transcription.Status)
                {
                    case "Failed":
                    case "Succeeded":

                            // we check to see if it was one of the transcriptions we created from this client.
                        if (!createdTranscriptions.Contains(transcription.Id))
                        {
                            // not created from here, continue
                            continue;
                        }

                        completed++;

                        // if the transcription was successful, check the results
                        if (transcription.Status == "Succeeded")
                        {
                            var resultsUri = transcription.ResultsUrls["channel_0"];
                            WebClient webClient = new WebClient();
                            var filename = Path.GetTempFileName();
                            webClient.DownloadFile(resultsUri, filename);
                            var results = File.ReadAllText(filename);
                            Console.WriteLine("Transcription succeeded. Results: ");
                            Console.WriteLine(results);
                        }

                    break;
                    case "Running":
                    running++;
                     break;
                    case "NotStarted":
                    notStarted++;
                    break;

                    }
                }
            }
        }
```

Vollständige Informationen zu den vorherigen Aufrufen finden Sie in unserer [Swagger-Dokumentation](https://westus.cris.ai/swagger/ui/index) (in englischer Sprache). Das vollständige hier gezeigte Beispiel finden Sie auf [GitHub](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI).

> [!NOTE]
> Im oben angegebenen Code stammt der Abonnementschlüssel aus der Speech-Ressource, die Sie im Azure-Portal erstellen. Schlüssel, die Sie aus der Custom Speech Service-Ressource abrufen, funktionieren nicht.

Beachten Sie das asynchrone Setup für das Senden von Audiodaten und das Empfangen des Transkriptionsstatus. Der von Ihnen erstellte Client ist ein .NET-HTTP-Client. Es gibt eine `PostTranscriptions`-Methode für das Senden der Audiodateidetails und eine `GetTranscriptions`-Methode zum Empfangen der Ergebnisse. `PostTranscriptions` gibt ein Handle zurück, und `GetTranscriptions` verwendet dieses Handle, um ein Handle zum Abrufen des Transkriptionsstatus zu erstellen.

Im aktuellen Beispielcode ist kein benutzerdefiniertes Modell angegeben. Der Dienst verwendet die Basismodelle zum Transkribieren der Datei bzw. Dateien. Zum Angeben der Modelle können Sie dieselbe Methode wie bei den Modell-IDs für das Akustikmodell und das Sprachmodell übergeben.

Wenn Sie die Basismodelle nicht verwenden möchten, übergeben Sie Modell-IDs sowohl für das Akustikmodell als auch für das Sprachmodell.

> [!NOTE]
> Für Basistranskriptionen müssen Sie die Endpunkte der Basismodelle nicht deklarieren. Wenn Sie benutzerdefinierte Modelle verwenden möchten, geben Sie deren Endpunkte-IDs als [Beispiel](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI) an. Wenn Sie ein Basisakustikmodell mit einem Basissprachmodell verwenden möchten, müssen Sie nur die Endpunkt-ID des benutzerdefinierten Modells deklarieren. Microsoft erkennt das Partnerbasismodell – ganz gleich, ob Akustik- oder Sprachmodell – und verwendet dieses zum Erfüllen der Transkriptionsanforderung.

### <a name="supported-storage"></a>Unterstützter Speicher

Derzeit wird als Speicher nur Azure Blob Storage unterstützt.

## <a name="download-the-sample"></a>Herunterladen des Beispiels

Sie finden das Beispiel dieses Artikels in [GitHub](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI).

> [!NOTE]
> Für eine Audiotranskription wird normalerweise eine Zeitspanne benötigt, die der Dauer der Audiodatei plus einem Mehraufwand von 2 bis 3 Minuten entspricht.

## <a name="next-steps"></a>Nächste Schritte

* [Abrufen Ihres Testabonnements für Speech](https://azure.microsoft.com/try/cognitive-services/)
