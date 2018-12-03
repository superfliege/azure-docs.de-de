---
title: Migrieren von der Azure Video Indexer-API v1 zu v2
titlesuffix: Azure Media Services
description: In diesem Thema wird beschrieben, wie Sie die Migration von Azure Video Indexer-API v1 zu v2 durchführen.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.topic: article
ms.date: 11/19/2018
ms.author: juliako
ms.openlocfilehash: ad2e2dace7d94c196d46beed957b39f3953246b5
ms.sourcegitcommit: beb4fa5b36e1529408829603f3844e433bea46fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/22/2018
ms.locfileid: "52291842"
---
# <a name="migrate-from-the-video-indexer-api-v1-to-v2"></a>Migrieren von Video Indexer-API v1 zu v2

> [!Note]
> Die v1-API von Video Indexer gilt ab dem 1. August 2018 als veraltet. Es ist ratsam, jetzt die v2-API von Video Indexer zu verwenden. <br/>Eine Anleitung zur Entwicklung mit den v2-APIs für Video Indexer finden Sie [hier](https://api-portal.videoindexer.ai/). 

In diesem Artikel werden die Änderungen beschrieben, die in v2 eingeführt wurden.  

## <a name="api-changes"></a>API-Änderungen

### <a name="authorization-and-operations"></a>Authorization (Autorisierung) und Operations (Vorgänge)

In Version 2 wurde für Video Indexer das Authentifizierungs- und Autorisierungsmodell der API geändert. Es sind zwei API-Sätze vorhanden: 

* Autorisierung 
* Vorgänge

Die **Authorization**-API (Autorisierung) wird verwendet, um Zugriffstoken für den Aufruf der **Operations**-API (Vorgänge) zu erhalten. Die **Operations**-API enthält alle Video Indexer-APIs, z.B. für „Video hochladen“, „Erkenntnisse gewinnen“ und andere Vorgänge.

Nachdem Sie die **Authorization**-API [abonniert](video-indexer-get-started.md) haben, können Sie Zugriffstoken abrufen, indem Sie Ihren Abonnementschlüssel übergeben (wie bei Version 1).

## <a name="getting-and-using-the-access-token-for-operations-apis"></a>Abrufen und Verwenden des Zugriffstokens für Operations-APIs

Beim Aufrufen der **Operations**-APIs wird der Abonnementschlüssel nicht mehr verwendet. Stattdessen übergeben Sie die Zugriffstoken, die über die **Authorization**-API abgerufen werden. 

Jede Anforderung sollte über ein gültiges Token verfügen, das der Zugriffsebene der aufgerufenen API entspricht. Für Vorgänge für Ihren Benutzer, z.B. das Abrufen Ihrer Konten, ist ein Benutzerzugriffstoken erforderlich. Für Vorgänge auf Kontoebene, z.B. das Auflisten aller Videos, ist ein Kontozugriffstoken erforderlich. Für Vorgänge für Videos, z.B. das Neuindizieren eines Videos, ist entweder ein Kontozugriffstoken oder ein Videozugriffstoken erforderlich.

Zur Vereinfachung können Sie „**Authorization**-API“ > „**GetAccounts**“ verwenden, um Ihre Konten abzurufen, ohne dass Sie zuerst ein Benutzertoken beschaffen müssen. Sie können auch darum bitten, dass Sie die Konten mit gültigen Token erhalten, um einen zusätzlichen Aufruf zum Abrufen eines Kontotokens zu sparen.

Weitere Informationen zu den unterschiedlichen Zugriffstoken finden Sie unter [Verwenden der Azure Video Indexer-API](video-indexer-use-apis.md).

### <a name="locations"></a>Standorte

Jeder Aufruf der API sollte den Standort Ihres Video Indexer-Kontos enthalten. Für API-Aufrufe, die nicht über einen Standort oder einen falschen Standort verfügen, tritt ein Fehler auf.

Es gelten die in der folgenden Tabelle beschriebenen Werte. Der **Parameterwert** ist der Wert, den Sie beim Verwenden der API übergeben.

|**Name**|**Parameterwert**|**Beschreibung**|
|---|---|---|
|Testversion|Testversion|Wird für Testkonten verwendet. Beispiel: https://api.videoindexer.ai/trial/Accounts/{accountId}/Videos/{videoId}/Index?language=English.|
|USA (Westen)|westus2|Wird für die Azure-Region „West US 2“ (USA, Westen 2) verwendet.  Beispiel: https://api.videoindexer.ai/westus2/Accounts/{accountId}/Videos/{videoId}/Index?language=English.|
|Nordeuropa |northeurope|Wird für die Azure-Region „North Europe“ (Europa, Norden) verwendet. Beispiel: https://api.videoindexer.ai/northeurope/Accounts/{accountId}/Videos/{videoId}/Index?language=English. |
|Asien, Osten|eastasia|Wird für die Azure-Region „East Asia“ (Asien, Osten) verwendet. Beispiel: https://api.videoindexer.ai/eastasia/Accounts/{accountId}/Videos/{videoId}/Index?language=English.|

### <a name="data-model"></a>Datenmodell

Video Indexer verfügt jetzt über ein vereinfachtes Datenmodell, mit dem deutlich bessere Erkenntnisse geliefert werden können. Weitere Informationen zur Ausgabe, die mit der v2-API erstellt wird, finden Sie unter [Untersuchen der von der v2-API erstellten Azure Video Indexer-Ausgabe](video-indexer-output-json-v2.md).

### <a name="swagger"></a>Swagger

Die Definitionen der Video Indexer-API wurden entsprechend aktualisiert und stehen über das [Portal für Video Indexer-Entwickler](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token) zum Download zur Verfügung.


### <a name="v1-vs-v2-examples"></a>v1- und v2-Beispiele

Die neuen v2-APIs weisen drei Hauptparameter auf:

1. [LOCATION]: Wie oben beschrieben. Kann den Wert „trial“, „westus2“, „northeurope“ oder „eastasia“ haben.
2. [YOUR_ACCOUNT_ID]: Eine GUID für Ihr Konto. Wird beim Abrufen aller Konten abgerufen (unten beschrieben).
3. [YOUR_VIDEO_ID]: Die ID Ihres Videos (z.B. „d4fa369abc“). Wird zurückgegeben, wenn ein Video hochgeladen oder wenn nach Videos gesucht wird.

#### <a name="uploading-a-video-in-v1"></a>Hochladen eines Videos in v1:

```
https://videobreakdown.azure-api.net/Breakdowns/Api/Partner/Breakdowns?name=some_name&description=some_description&privacy=private&videoUrl=http://URL_TO_YOUR_VIDEO
```

#### <a name="uploading-a-video-in-v2"></a>Hochladen eines Videos in v2:

1. Abrufen eines Zugriffstokens für die Uploadanforderung:

  Sie können alle Konten und die zugehörigen Zugriffstoken abrufen:

    ```
    https://api.videoindexer.ai/auth/[LOCATION]/Accounts?generateAccessTokens=true&allowEdit=true
    ```

  Oder das spezifische Kontozugriffstoken:
  
  ```
  https://api.videoindexer.ai/auth/[LOCATION]/Accounts/[YOUR_ACCOUNT_ID]/AccessToken?allowEdit=true
  ```
2. Hochladen eines Videos:

  ```
  POST https://api.videoindexer.ai/[LOCATION]/Accounts/[YOUR_ACCOUNT_ID]/Videos?name=MySample&description=MySampleDescription&videoUrl=[URL_ENCODED_VIDEO_URL]&accessToken=eyJ0eXAiOiJ...
  ```

#### <a name="getting-insights-in-v1"></a>Gewinnen von Erkenntnissen in v1:

```
https://videobreakdown.azure-api.net/Breakdowns/Api/Partner/Breakdowns/[VIDEO_ID]
```
  
#### <a name="getting-insights-in-v2"></a>Gewinnen von Erkenntnissen in v2:

1. Verwenden Sie entweder das Kontozugriffstoken, oder rufen Sie ein Zugriffstoken für die Videoebene ab:

  ```
  https://api.videoindexer.ai/auth/[LOCATION]/Accounts/[YOUR_ACCOUNT_ID]/Videos/[VIDEO_ID]/AccessToken
  ```
  
2. Gewinnen von Erkenntnissen:

  ```
  https://api.videoindexer.ai/[LOCATION]/Accounts[YOUR_ACCOUNT_ID]/Videos/[VIDEO_ID]/Index?accessToken=eyJ0eXA...
  ```

#### <a name="getting-video-processing-state-in-v1"></a>Abrufen des Status der Videoverarbeitung in v1:

```
https://videobreakdown.azure-api.net/Breakdowns/Api/Partner/Breakdowns/[VIDEO_ID]/State
```
  
#### <a name="getting-video-processing-state-in-v2"></a>Abrufen des Status der Videoverarbeitung in v2:

In der v2-API wird der Verarbeitungsstatus als Teil der API zum Abrufen des Videoindex (Get Video Index) zurückgegeben.

1. Verwenden Sie entweder das Kontozugriffstoken, oder rufen Sie ein Zugriffstoken für die Videoebene ab:

  ```
  https://api.videoindexer.ai/trial/[LOCATION]/[YOUR_ACCOUNT_ID]/Videos/[VIDEO_ID]/Index?accessToken=eyJ0eXA...
  ```
  
2. Gewinnen von Erkenntnissen:

  ```
  https://api.videoindexer.ai/trial/[LOCATION]/[YOUR_ACCOUNT_ID]/Videos/[VIDEO_ID]/Index?accessToken=eyJ0eXA...
  ```

## <a name="next-steps"></a>Nächste Schritte

[Verwenden der Azure Video Indexer-API](video-indexer-use-apis.md)

