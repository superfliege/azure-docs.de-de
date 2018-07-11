---
title: Problembehandlung für das Cognitive Services-Sprach-SDK | Microsoft-Dokumentation
description: Problembehandlung für das Cognitive Services-Sprach-SDK
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: wolfma
ms.openlocfilehash: 16eaebcf9494ab57521068a9418ccf2ac7f5a8fe
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35376962"
---
# <a name="troubleshooting-speech-services-sdk"></a>Problembehandlung für das Sprachdienste-SDK

Dieser Artikel enthält Informationen zur Lösung von Problemen, die bei Verwendung des Sprach-SDK auftreten können.

## <a name="error-websocket-upgrade-failed-with-an-authentication-error-403"></a>Fehler `WebSocket Upgrade failed with an authentication error (403).`

Sie verwenden möglicherweise den falschen Endpunkt für Ihre Region oder den Dienst. Prüfen Sie noch einmal den URI, um sicherzustellen, dass er korrekt ist. Lesen Sie außerdem den folgenden Abschnitt, da es sich auch um ein Problem mit dem Abonnementschlüssel oder dem Autorisierungstoken handeln kann.

## <a name="error-http-403-forbidden-or-error-http-401-unauthorized"></a>Fehler `HTTP 403 Forbidden` oder Fehler `HTTP 401 Unauthorized`

Dieser Fehler wird häufig durch Authentifizierungsprobleme verursacht. Verbindungsanforderungen ohne gültigen `Ocp-Apim-Subscription-Key` oder `Authorization`-Header werden mit dem Status 401 oder 403 abgelehnt.

* Wenn Sie einen Abonnementschlüssel für die Authentifizierung verwenden, kann Folgendes die Ursache sein:

    - Der Abonnementschlüssel fehlt oder ist ungültig.
    - Sie haben das Nutzungskontingent Ihres Abonnements überschritten.

* Wenn Sie ein Autorisierungstoken für die Authentifizierung verwenden, kann Folgendes die Ursache sein:

    - Das Autorisierungstoken ist ungültig.
    - Das Autorisierungstoken ist abgelaufen.

### <a name="validate-your-subscription-key"></a>Überprüfen des Abonnementschlüssels

Sie können überprüfen, ob Sie über einen gültigen Abonnementschlüssel verfügen, indem Sie einen der unten aufgeführten Befehle ausführen.

> [!NOTE]
> Ersetzen Sie `YOUR_SUBSCRIPTION_KEY` und `YOUR_REGION` durch den eigenen Abonnementschlüssel bzw. die zugehörige Region.

* PowerShell

    ```Powershell
    $FetchTokenHeader = @{
      'Content-type'='application/x-www-form-urlencoded'
      'Content-Length'= '0'
      'Ocp-Apim-Subscription-Key' = 'YOUR_SUBSCRIPTION_KEY'
    }
    $OAuthToken = Invoke-RestMethod -Method POST -Uri https://YOUR_REGION.api.cognitive.microsoft.com/sts/v1.0/issueToken -Headers $FetchTokenHeader
    $OAuthToken
    ```

* cURL

    ```
    curl -v -X POST "https://YOUR_REGION.api.cognitive.microsoft.com/sts/v1.0/issueToken" -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY" -H "Content-type: application/x-www-form-urlencoded" -H "Content-Length: 0"
    ```

### <a name="validate-an-authorization-token"></a>Überprüfen eines Autorisierungstokens

Wenn Sie ein Autorisierungstoken für die Authentifizierung verwenden, führen Sie einen der folgenden Befehle aus, um sicherzustellen, dass das Autorisierungstoken noch gültig ist. Token sind 10 Minuten lang gültig.

> [!NOTE]
> Ersetzen Sie `YOUR_AUDIO_FILE` durch den Pfad zu Ihrer aufgezeichneten Audiodatei, `YOUR_ACCESS_TOKEN` durch das im vorherigen Schritt zurückgegebene Autorisierungstoken und `YOUR_REGION` durch die richtige Region.

* PowerShell

    ```Powershell
    $SpeechServiceURI =
    'https://YOUR_REGION.stt.speech.microsoft.com/speech/recognition/interactive/cognitiveservices/v1?language=en-US'
    
    # $OAuthToken is the authorization token returned by the token service.
    $RecoRequestHeader = @{
      'Authorization' = 'Bearer '+ $OAuthToken
      'Transfer-Encoding' = 'chunked'
      'Content-type' = 'audio/wav; codec=audio/pcm; samplerate=16000'
    }
    
    # Read audio into byte array
    $audioBytes = [System.IO.File]::ReadAllBytes("YOUR_AUDIO_FILE")
    
    $RecoResponse = Invoke-RestMethod -Method POST -Uri $SpeechServiceURI -Headers $RecoRequestHeader -Body $audioBytes
    
    # Show the result
    $RecoResponse
    ```

* cURL

    ```
    curl -v -X POST "https://YOUR_REGION.stt.speech.microsoft.com/speech/recognition/interactive/cognitiveservices/v1?language=en-US" -H "Authorization: Bearer YOUR_ACCESS_TOKEN" -H "Transfer-Encoding: chunked" -H "Content-type: audio/wav; codec=audio/pcm; samplerate=16000" --data-binary @YOUR_AUDIO_FILE
    ```

---

## <a name="error-http-400-bad-request"></a>Fehler `HTTP 400 Bad Request`

Dieser Fehler tritt gewöhnlich auf, wenn der Anforderungstext ungültige Audiodaten enthält. Nur das `WAV`-Format wird unterstützt. Prüfen Sie auch die Anforderungsheader, um sicherzustellen, dass geeignete Werte für `Content-Type` und `Content-Length` angegeben sind.

## <a name="error-http-408-request-timeout"></a>Fehler `HTTP 408 Request Timeout`

Der Fehler tritt wahrscheinlich auf, weil keine Audiodaten an den Dienst gesendet werden. Dieser Fehler kann auch durch Netzwerkprobleme verursacht werden.

## <a name="the-recognitionstatus-in-the-response-is-initialsilencetimeout"></a>Der `RecognitionStatus` in der Antwort ist `InitialSilenceTimeout`

Normalerweise sind Audiodaten die Ursache des Problems. Beispiel: 

* Zu Beginn einer Audiodatei ist es über einen längeren Zeitraum still. Der Dienst beendet die Erkennung nach einigen Sekunden und gibt `InitialSilenceTimeout` zurück.
* Die Audiodatei verwendet ein nicht unterstütztes Codec-Format, wodurch die Audiodaten als Stille behandelt werden.

## <a name="next-steps"></a>Nächste Schritte

* [Anmerkungen zu dieser Version](releasenotes.md)

