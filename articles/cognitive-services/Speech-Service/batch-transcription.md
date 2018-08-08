---
title: Azure Batch-Transkriptions-API
description: Beispiele
services: cognitive-services
author: PanosPeriorellis
ms.service: cognitive-services
ms.technology: Speech to Text
ms.topic: article
ms.date: 04/26/2018
ms.author: panosper
ms.openlocfilehash: f21973855ceb3a257627c147490ac50465c54020
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/27/2018
ms.locfileid: "39281938"
---
# <a name="batch-transcription"></a>Batch-Transkription

Die Batch-Transkription ist optimal für Anwendungsfälle mit großen Audiodatenmengen geeignet. Sie ermöglicht es dem Entwickler, im asynchronen Modus auf Audiodateien zu verweisen und Transkriptionen zurückzuerhalten.

## <a name="batch-transcription-api"></a>Batch-Transkriptions-API

Die Batch-Transkriptions-API ermöglicht das oben beschriebene Szenario. Sie bietet asynchrone Transkription von Sprache in Text sowie zusätzliche Funktionen.

> [!NOTE]
> Die Batch-Transkriptions-API eignet sich ideal für Callcenter, bei denen sich üblicherweise Tausende von Stunden mit Audioaufzeichnungen ansammeln. Aufgrund der eigenständigen Funktionsweise dieser API können große Mengen von Audioaufnahmen problemlos transkribiert werden.

### <a name="supported-formats"></a>Unterstützte Formate

Die Batch-Transkriptions-API ist darauf ausgelegt, der De-facto-Standard für alle Callcenter-bezogenen Offline-Szenarien zu werden, und bietet Unterstützung für alle zugehörigen Formate. Derzeit unterstützte Formate:

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
> Die Batch-Transkriptions-API verwendet einen REST-Dienst zum Anfordern von Transkriptionen, deren Status und zugehörigen Ergebnissen. Die API kann in jeder Sprache verwendet werden. Im nächsten Abschnitt wird ihre Verwendung beschrieben.

## <a name="authorization-token"></a>Autorisierungstoken

Wie bei allen Features des vereinheitlichten Spracherkennungsdiensts muss der Benutzer einen Abonnementschlüssel aus dem [Azure-Portal](https://portal.azure.com) erstellen. Darüber hinaus muss ein API-Schlüssel aus dem Speech-Portal abgerufen werden. Zum Generieren eines API-Schlüssels müssen die folgenden Schritte ausgeführt werden:

1. Melden Sie sich bei https://customspeech.ai an.

2. Klicken Sie auf „Abonnements“.

3. Klicken Sie auf die Option `Generate API Key`.

    ![Die Uploadansicht](media/stt/Subscriptions.jpg)

4. Kopieren Sie diesen Schlüssel, und fügen Sie ihn im Clientcode des nachfolgenden Beispiels ein.

> [!NOTE]
> Wenn Sie ein benutzerdefiniertes Modell verwenden möchten, benötigen Sie auch die ID dieses Modells. Beachten Sie, dass es sich hier nicht um die Bereitstellungs- oder Endpunkt-ID handelt, die in der Ansicht „Endpunktdetails“ angegeben ist, sondern um die Modell-ID, die Sie durch Klicken auf die Details dieses Modells abrufen können.

## <a name="sample-code"></a>Beispielcode

Die Verwendung der API ist recht unkompliziert. Der Beispielcode unten muss mit einem Abonnementschlüssel und einem API-Schlüssel angepasst werden, wodurch der Entwickler ein Bearertoken abrufen kann, wie der folgende Codeausschnitt zeigt:

```cs
    public static async Task<CrisClient> CreateApiV1ClientAsync(string username, string key, string hostName, int port)
        {
            var client = new HttpClient();
            client.Timeout = TimeSpan.FromMinutes(25);
            client.BaseAddress = new UriBuilder(Uri.UriSchemeHttps, hostName, port).Uri;

            var tokenProviderPath = "/oauth/ctoken";
            var clientToken = await CreateClientTokenAsync(client, hostName, port, tokenProviderPath, username, key).ConfigureAwait(false);
            client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("bearer", clientToken.AccessToken);

            return new CrisClient(client);
        }
```

Nachdem das Token abgerufen wurde, muss der Entwickler den SAS-URI angeben, der auf die Audiodatei zeigt, für die eine Transkription erforderlich ist. Der Rest des Codes durchläuft einfach den Status und zeigt Ergebnisse an.

```cs
   static async Task TranscribeAsync()
        { 
            // Creating a Batch transcription API Client
            var client = 
                await CrisClient.CreateApiV1ClientAsync(
                    "<your msa>", // MSA email
                    "<your api key>", // API key
                    "stt.speech.microsoft.com",
                    443).ConfigureAwait(false);
            
            var newLocation = 
                await client.PostTranscriptionAsync(
                    "<selected locale i.e. en-us>", // Locale 
                    "<your subscription key>", // Subscription Key
                    new Uri("<SAS URI to your file>")).ConfigureAwait(false);

            var transcription = await client.GetTranscriptionAsync(newLocation).ConfigureAwait(false);

            while (true)
            {
                transcription = await client.GetTranscriptionAsync(transcription.Id).ConfigureAwait(false);

                if (transcription.Status == "Failed" || transcription.Status == "Succeeded")
                {
                    Console.WriteLine("Transcription complete!");

                    if (transcription.Status == "Succeeded")
                    {
                        var resultsUri = transcription.ResultsUrls["channel_0"];

                        WebClient webClient = new WebClient();

                        var filename = Path.GetTempFileName();
                        webClient.DownloadFile(resultsUri, filename);

                        var results = File.ReadAllText(filename);
                        Console.WriteLine(results);
                    }

                    await client.DeleteTranscriptionAsync(transcription.Id).ConfigureAwait(false);

                    break;
                }
                else
                {
                    Console.WriteLine("Transcription status: " + transcription.Status);
                }

                await Task.Delay(TimeSpan.FromSeconds(5)).ConfigureAwait(false);
            }

            Console.ReadLine();
        }
```

> [!NOTE]
> Der im Codeausschnitt oben angegebene Abonnementschlüssel ist der Schlüssel von der Speech (Vorschau)-Ressource, die Sie im Azure-Portal erstellen. Schlüssel, die von der Custom Speech Service-Ressource abgerufen werden, funktionieren nicht.


Beachten Sie das asynchrone Setup für das Senden von Audiodaten und das Empfangen des Transkriptionsstatus. Der erstellte Client ist ein .NET-HTTP-Client. Es gibt eine `PostTranscriptions`-Methode für das Senden der Audiodateidetails und eine `GetTranscriptions`-Methode zum Empfangen der Ergebnisse. `PostTranscriptions` gibt ein Handle zurück, und die `GetTranscriptions`-Methode verwendet dieses Handle, um ein Handle zum Abrufen des Transkriptionsstatus zu erstellen.

Im aktuellen Beispielcode sind keine benutzerdefinierten Modelle angegeben. Der Dienst verwendet die Basismodelle zum Transkribieren der Datei(en). Wenn der Benutzer die Modelle angeben möchte, können dieselben Modell-IDs für das Akustikmodell und das Sprachmodell übergeben werden. 

Sollen die Basismodelle nicht verwendet werden, müssen Modell-IDs sowohl für das Akustikmodell als auch das Sprachmodell übergeben werden.

> [!NOTE]
> Für eine Basistranskription muss der Benutzer die Endpunkte der Basismodelle nicht deklarieren. Sollen benutzerdefinierte Modelle verwendet werden, muss der Benutzer deren Endpunkte-IDs als [Beispiel](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI) angeben. Wenn der Benutzer ein Basisakustikmodell mit einem Basissprachmodell verwenden möchte, muss er nur die Endpunkt-ID des benutzerdefinierten Modells deklarieren. Das System ermittelt intern das Partnerbasismodell (ganz gleich, ob Akustik- oder Sprachmodell) und verwendet dieses zur Erfüllung der Transkriptionsanforderung.

### <a name="supported-storage"></a>Unterstützter Speicher

Derzeit wird als Speicher nur Azure Blob unterstützt.

## <a name="downloading-the-sample"></a>Herunterladen des Beispiels

Das hier gezeigte Beispiel befindet sich auf [GitHub](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI).

> [!NOTE]
> Für eine Audiotranskription wird in der Regel eine Zeitspanne benötigt, die der Dauer der Audiodatei plus einem Mehraufwand von 2 bis 3 Minute entspricht.

## <a name="next-steps"></a>Nächste Schritte

* [Abrufen Ihres Testabonnements für Speech](https://azure.microsoft.com/try/cognitive-services/)
