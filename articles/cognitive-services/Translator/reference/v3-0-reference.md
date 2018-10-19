---
title: Referenz für die Textübersetzungs-API Version 3.0
titlesuffix: Azure Cognitive Services
description: Referenzdokumentation für die Textübersetzungs-API Version 3.0
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: reference
ms.date: 03/29/2018
ms.author: v-jansko
ms.openlocfilehash: 9282d8af30cbfb3346394bcd71510faf8d8c8a21
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "46129385"
---
# <a name="translator-text-api-v30"></a>Textübersetzungs-API Version 3.0

## <a name="whats-new"></a>Neuerungen

Version 3 der Textübersetzungs-API umfasst eine moderne JSON-basierte Web-API. Sie ermöglicht eine bessere Nutzung und Leistung, indem vorhandene Features zu einer geringeren Zahl von Vorgängen zusammengefasst und neue Features bereitgestellt werden.

 * Transliteration zur Konvertierung von Text in einer Sprache aus einem Skript in ein anderes Skript.
 * Übersetzung in mehrere Sprachen innerhalb einer Anforderung.
 * Spracherkennung, Übersetzung und Transliteration innerhalb einer Anforderung.
 * Wörterbuch zum Nachschlagen von Übersetzungsalternativen für einen Begriff und zum Ermitteln von Rückübersetzungen und Kontextbeispielen für die Begriffe.
 * Informativere Ergebnisse für die Spracherkennung.

## <a name="base-urls"></a>Basis-URLs

Die Textübersetzungs-API Version 3.0 ist in der folgenden Cloud verfügbar:

| BESCHREIBUNG | Region | Basis-URL                                        |
|-------------|--------|-------------------------------------------------|
| Azure       | Global | api.cognitive.microsofttranslator.com           |


## <a name="authentication"></a>Authentifizierung

Abonnieren Sie die Textübersetzungs-API in Microsoft Cognitive Services, und verwenden Sie Ihren Abonnementschlüssel (im Azure-Portal) für die Authentifizierung. 

Die einfachste Möglichkeit ist das Übergeben Ihres geheimen Azure-Schlüssels an den Translator-Dienst mit dem Anforderungsheader `Ocp-Apim-Subscription-Key`.

Eine Alternative ist die Verwendung Ihres geheimen Schlüssels, um ein Autorisierungstoken aus dem Tokendienst abzurufen. Anschließend übergeben Sie das Autorisierungstoken an den Translator-Dienst, indem Sie den Anforderungsheader `Authorization` verwenden. Senden Sie eine `POST`-Anforderung an die folgende URL, um ein Autorisierungstoken zu erhalten:

| Environment     | Authentifizierungsdienst-URL                                |
|-----------------|-----------------------------------------------------------|
| Azure           | `https://api.cognitive.microsoft.com/sts/v1.0/issueToken` |

Hier sind Beispielanforderungen zum Abrufen eines Tokens über einen geheimen Schlüssel angegeben:

```
// Pass secret key using header
curl --header 'Ocp-Apim-Subscription-Key: <your-key>' --data "" 'https://api.cognitive.microsoft.com/sts/v1.0/issueToken'
// Pass secret key using query string parameter
curl --data "" 'https://api.cognitive.microsoft.com/sts/v1.0/issueToken?Subscription-Key=<your-key>'
```

Eine erfolgreiche Anforderung gibt das codierte Zugriffstoken im Antworttext im Nur-Text-Format zurück. Das gültige Token wird während der Autorisierung als Bearertoken an den Translator-Dienst übergeben.

```
Authorization: Bearer <Base64-access_token>
```

Ein Authentifizierungstoken ist zehn Minuten lang gültig. Das Token sollte wiederverwendet werden, wenn die Translator-APIs mehrfach aufgerufen werden. Wenn Ihr Programm aber über längere Zeit Anforderungen an die Translator-API sendet, muss das Programm regelmäßig (z.B. alle acht Minuten) ein neues Zugriffstoken anfordern.

Zusammenfassung: Eine Clientanforderung für die Translator-API enthält einen Autorisierungsheader aus der folgenden Tabelle:

<table width="100%">
  <th width="30%">Header</th>
  <th>BESCHREIBUNG</th>
  <tr>
    <td>Ocp-Apim-Subscription-Key</td>
    <td>*Verwendung mit Cognitive Services-Abonnement, wenn Sie Ihren geheimen Schlüssel übergeben*.<br/>Der Wert ist der geheime Azure-Schlüssel für Ihr Abonnement für die Textübersetzungs-API.</td>
  </tr>
  <tr>
    <td>Autorisierung</td>
    <td>*Verwendung mit dem Cognitive Services-Abonnement, wenn Sie ein Authentifizierungstoken übergeben.*<br/>Der Wert ist das Bearertoken: „Bearer <token>“.</td>
  </tr>
</table> 

## <a name="errors"></a>Errors

Eine Standardfehlerantwort ist ein JSON-Objekt mit einem Name-Wert-Paar mit dem Namen `error`. Der Wert ist außerdem ein JSON-Objekt mit Eigenschaften:

  * `code`: Ein vom Server definierter Fehlercode.

  * `message`: Eine Zeichenfolge als für Menschen lesbare Darstellung des Fehlers.

Ein Kunde mit einer kostenlose Testversion des Abonnements erhält beispielsweise den folgenden Fehler, nachdem das Kontingent für den Free-Tarif erschöpft ist:

```
{
  "error": {
    "code":403000,
    "message":"The subscription has exceeded its free quota."
    }
}
```
