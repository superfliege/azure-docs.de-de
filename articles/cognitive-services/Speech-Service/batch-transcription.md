---
title: Azure Batch-Transkriptions-API
description: Beispiele
services: cognitive-services
author: PanosPeriorellis
ms.service: cognitive-services
ms.component: Speech
ms.topic: article
ms.date: 04/26/2018
ms.author: panosper
ms.openlocfilehash: 8f9a033ebf9cdfdb96ae8511b14202e49ec0a85e
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2018
ms.locfileid: "48884458"
---
# <a name="batch-transcription"></a>Batch-Transkription

Die Batch-Transkription ist optimal geeignet, wenn Sie über große Audiodatenmengen verfügen. Sie können im asynchronen Modus über den URI auf Audiodateien verweisen und Transkriptionen zurückerhalten.

## <a name="batch-transcription-api"></a>Batch-Transkriptions-API

Die Batch-Transkriptions-API bietet asynchrone Transkription von Sprache in Text sowie zusätzliche Funktionen.

> [!NOTE]
> Die Batch-Transkriptions-API eignet sich ideal für Callcenter, bei denen sich üblicherweise Tausende von Stunden mit Audioaufzeichnungen ansammeln. Aufgrund der eigenständigen Funktionsweise dieser API können große Mengen von Audioaufnahmen problemlos transkribiert werden.

### <a name="supported-formats"></a>Unterstützte Formate

Die Batch-Transkriptions-API unterstützt die folgenden Formate:

NAME| Kanal  |
----|----------|
MP3 |   Mono   |   
MP3 |  Stereo  | 
WAV |   Mono   |
WAV |  Stereo  |

Bei Audiodatenströmen in Stereo wird der linke und rechte Kanal während der Transkription von der Batch-Transkriptions-API geteilt. Die beiden JSON-Dateien mit dem Ergebnis werden jeweils von einem einzigen Kanal erstellt. Die Zeitstempel pro Äußerung ermöglicht es dem Entwickler, eine geordnete endgültige Transkription zu erstellen. Das folgende JSON-Beispiel zeigt die Ausgabe eines Kanals.

```json
       {
        "recordingsUrl": "https://mystorage.blob.core.windows.net/cris-e2e-datasets/TranscriptionsDataset/small_sentence.wav?st=2018-04-19T15:56:00Z&se=2040-04-21T15:56:00Z&sp=rl&sv=2017-04-17&sr=b&sig=DtvXbMYquDWQ2OkhAenGuyZI%2BYgaa3cyvdQoHKIBGdQ%3D",
        "resultsUrls": {
            "channel_0": "https://mystorage.blob.core.windows.net/bestor-87a0286f-304c-4636-b6bd-b3a96166df28/TranscriptionData/24265e4c-e459-4384-b572-5e3e7795221f?sv=2017-04-17&sr=b&sig=IY2qd%2Fkgtz2PwRe2C88BphH4Hv%2F1VCb1UVJ33xsw%2BEY%3D&se=2018-04-23T14:48:24Z&sp=r"
        },
        "statusMessage": "None.",
        "id": "0bb95356-ff06-469d-acc7-81f9144a269a",
        "createdDateTime": "2018-04-20T14:11:57.167",
        "lastActionDateTime": "2018-04-20T14:12:54.643",
        "status": "Succeeded",
        "locale": "en-US"
    },
```

> [!NOTE]
> Die Batch-Transkriptions-API verwendet einen REST-Dienst zum Anfordern von Transkriptionen, deren Status und zugehörigen Ergebnissen. Sie können die API in jeder Sprache verwenden. Im nächsten Abschnitt wird ihre Verwendung beschrieben.

## <a name="authorization-token"></a>Autorisierungstoken

Wie bei allen Features des Spracherkennungsdiensts erstellen Sie einen Abonnementschlüssel im [Azure-Portal](https://portal.azure.com) anhand der Anleitung unter [Erste Schritte](get-started.md). Wenn Sie das Abrufen von Transkriptionen aus unseren Baselinemodellen planen, müssen Sie nichts weiter tun. 

Wenn Sie ein benutzerdefiniertes Modell anpassen und verwenden möchten, müssen Sie diesen Abonnementschlüssel wie folgt im Custom Speech-Portal hinzufügen:

1. Melden Sie sich bei [Custom Speech](https://customspeech.ai) an.

2. Wählen Sie **Abonnements**.

3. Wählen Sie **Verbindung mit vorhandenem Abonnement herstellen** aus.

4. Fügen Sie den Abonnementschlüssel und einen Alias in der nun eingeblendeten Ansicht hinzu.

    ![Screenshot der Seite „Abonnements“ von Custom Speech](media/stt/Subscriptions.jpg)

5. Kopieren Sie diesen Schlüssel, und fügen Sie ihn in den Clientcode im folgenden Beispiel ein.

> [!NOTE]
> Wenn Sie ein benutzerdefiniertes Modell verwenden möchten, benötigen Sie auch die ID dieses Modells. Beachten Sie, dass es sich hier nicht um die Endpunkt-ID handelt, die in der Ansicht „Endpunktdetails“ angegeben ist. Es ist die Modell-ID, die Sie durch Klicken auf die Details dieses Modells abrufen können.

## <a name="sample-code"></a>Beispielcode

Passen Sie den folgenden Beispielcode mit einem Abonnementschlüssel und einem API-Schlüssel an. Dadurch können Sie ein Bearertoken abrufen.

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

Nachdem Sie das Token abgerufen haben, müssen Sie den SAS-URI angeben, der auf die Audiodatei zeigt, für die eine Transkription erforderlich ist. Der Rest des Codes durchläuft den Status und zeigt Ergebnisse an.

```cs
   static async Task TranscribeAsync()
        { 
            private const string SubscriptionKey = "<your Speech subscription key>";
            private const string HostName = "westus.cris.ai";
            private const int Port = 443;
    
            // Creating a Batch transcription API Client
            var client = CrisClient.CreateApiV2Client(SubscriptionKey, HostName, Port);
            
            var transcriptions = await client.GetTranscriptionAsync().ConfigureAwait(false);

            var transcriptionLocation = await client.PostTranscriptionAsync(Name, Description, Locale, new Uri(RecordingsBlobUri), new[] { AdaptedAcousticId, AdaptedLanguageId }).ConfigureAwait(false);

            // get the transcription Id from the location URI
            var createdTranscriptions = new List<Guid>();
            createdTranscriptions.Add(new Guid(transcriptionLocation.ToString().Split('/').LastOrDefault()))

            while (true)
            {
                // get all transcriptions for the user
                transcriptions = await client.GetTranscriptionAsync().ConfigureAwait(false);
                completed = 0; running = 0; notStarted = 0;

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
                                // not creted form here, continue
                                continue;
                            }
                            
                            completed++;
                            
                            // if the transcription was successfull, check the results
                            if (transcription.Status == "Succeeded")
                            {
                                var resultsUri = transcription.ResultsUrls["channel_0"];
                                WebClient webClient = new WebClient();
                                var filename = Path.GetTempFileName();
                                webClient.DownloadFile(resultsUri, filename);
                                var results = File.ReadAllText(filename);
                                Console.WriteLine("Transcription succedded. Results: ");
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

                Console.WriteLine(string.Format("Transcriptions status: {0} completed, {1} running, {2} not started yet", completed, running, notStarted));

                await Task.Delay(TimeSpan.FromSeconds(5)).ConfigureAwait(false);
            }

            Console.WriteLine("Press any key...");
        }
```

> [!NOTE]
> Im oben angegebenen Code stammt der Abonnementschlüssel von der Speech-Ressource, die Sie im Azure-Portal erstellen. Schlüssel, die von der Custom Speech Service-Ressource abgerufen werden, funktionieren nicht.

Beachten Sie das asynchrone Setup für das Senden von Audiodaten und das Empfangen des Transkriptionsstatus. Der erstellte Client ist ein .NET-HTTP-Client. Es gibt eine `PostTranscriptions`-Methode für das Senden der Audiodateidetails und eine `GetTranscriptions`-Methode zum Empfangen der Ergebnisse. `PostTranscriptions` gibt ein Handle zurück, und `GetTranscriptions` verwendet dieses Handle, um ein Handle zum Abrufen des Transkriptionsstatus zu erstellen.

Im aktuellen Beispielcode sind keine benutzerdefinierten Modelle angegeben. Der Dienst verwendet die Basismodelle zum Transkribieren der Datei bzw. Dateien. Zum Angeben der Modelle können Sie dieselben Modell-IDs für das Akustikmodell und das Sprachmodell übergeben. 

Wenn Sie die Basismodelle nicht verwenden möchten, müssen Sie Modell-IDs sowohl für das Akustikmodell als auch das Sprachmodell übergeben.

> [!NOTE]
> Für eine Basistranskription müssen Sie die Endpunkte der Basismodelle nicht deklarieren. Wenn Sie benutzerdefinierte Modelle verwenden möchten, geben Sie deren Endpunkte-IDs als [Beispiel](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI) an. Wenn Sie ein Basisakustikmodell mit einem Basissprachmodell verwenden möchten, müssen Sie nur die Endpunkt-ID des benutzerdefinierten Modells deklarieren. Microsoft erkennt das Partnerbasismodell (ganz gleich, ob Akustik- oder Sprachmodell) und verwendet dieses zur Erfüllung der Transkriptionsanforderung.

### <a name="supported-storage"></a>Unterstützter Speicher

Derzeit wird als Speicher nur Azure Blob Storage unterstützt.

## <a name="downloading-the-sample"></a>Herunterladen des Beispiels

Das hier gezeigte Beispiel befindet sich auf [GitHub](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI).

> [!NOTE]
> Für eine Audiotranskription wird in der Regel eine Zeitspanne benötigt, die der Dauer der Audiodatei plus einem Mehraufwand von 2 bis 3 Minute entspricht.

## <a name="next-steps"></a>Nächste Schritte

* [Abrufen Ihres Testabonnements für Speech](https://azure.microsoft.com/try/cognitive-services/)
