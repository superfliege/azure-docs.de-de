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
ms.openlocfilehash: 6f679536d69f700fd6678eb3bbbb869e42439cde
ms.sourcegitcommit: 7804131dbe9599f7f7afa59cacc2babd19e1e4b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/17/2018
ms.locfileid: "51853352"
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
    "code":403001,
    "message":"The operation is not allowed because the subscription has exceeded its free quota."
    }
}
```
Der Fehlercode ist eine 6-stellige Zahl, die aus dem 3-stelligen HTTP-Statuscode gefolgt von einer 3-stelligen Zahl zur Kategorisierung des Fehlers besteht. Häufige Fehlercodes sind:

| Code | BESCHREIBUNG |
|:----|:-----|
| 400000| Eine der Anforderungseingaben ist ungültig.|
| 400001| Der scope-Parameter ist ungültig.|
| 400002| Der category-Parameter ist ungültig.|
| 400003| Ein Sprachenspezifizierer fehlt oder ist ungültig.|
| 400004| Ein Zielskriptspezifizierer („To script“) fehlt oder ist ungültig.|
| 400005| Ein Eingabetext fehlt oder ist ungültig.|
| 400006| Die Kombination von Sprache und Skript ist ungültig.|
| 400018| Ein Quellskriptspezifizierer („From script“) fehlt oder ist ungültig.|
| 400019| Eine der angegebenen Sprachen wird nicht unterstützt.|
| 400020| Eines der Elemente im Array des Eingabetexts ist ungültig.|
| 400021| Der API-Versionsparameter fehlt oder ist ungültig.|
| 400023| Eines der angegebenen Sprachpaare wird nicht unterstützt.|
| 400035| Die Ausgangssprache (Feld „From“) ist ungültig.|
| 400036| Der Zielsprache (Feld „To“) fehlt oder ist ungültig.|
| 400042| Eine der angegebenen Optionen (Feld „Optionen“) ist ungültig.|
| 400043| Die Clientablaufverfolgungs-ID (Feld „ClientTraceId“ oder X-ClientTranceId-Header) fehlt oder ist ungültig.|
| 400050| Der Eingabetext ist zu lang.|
| 400064| Der translation-Parameter fehlt oder ist ungültig.|
| 400070| Die Anzahl der Zielskripts (ToScript-Parameter) entspricht nicht der Anzahl von Zielsprachen (To-Parameter).|
| 400071| Der Wert ist für TextType ungültig.|
| 400072| Das Array mit dem Eingabetext enthält zu viele Elemente.|
| 400073| Der script-Parameter ist ungültig.|
| 400074| Der Anforderungstext ist kein gültiger JSON-Code.|
| 400075| Die Kombination aus Sprachpaar und Kategorie ist ungültig.|
| 400077| Die maximale Anforderungsgröße wurde überschritten.|
| 400079| Das für die Übersetzung zwischen Ausgangs- und Zielsprache angeforderte benutzerdefinierte System ist nicht vorhanden.|
| 401000| Die Anforderung wurde nicht autorisiert, da die Anmeldeinformationen fehlen oder ungültig sind.|
| 401015| „Die angegebenen Anmeldeinformationen gelten für die SAPI. Diese Anforderung erfordert Anmeldeinformationen für die Text-API. Verwenden Sie ein Abonnement für die Textübersetzungs-API.“|
| 403000| Der Vorgang ist nicht zulässig.|
| 403001| Der Vorgang ist nicht zulässig, da das kostenlose Kontingent für das Abonnement überschritten wurde.|
| 405000| Die Anforderungsmethode wird für die angeforderte Ressource nicht unterstützt.|
| 408001| Das angeforderte benutzerdefinierte Übersetzungssystem ist noch nicht verfügbar. Versuchen Sie es in einigen Minuten erneut.|
| 415000| Der Content-Type-Header fehlt oder ist ungültig.|
| 429000, 429001, 429002| Der Server hat die Anforderung abgelehnt, da der Client zu viele Anforderungen sendet. Reduzieren Sie die Häufigkeit der Anforderungen, um eine Drosselung zu vermeiden.|
| 500000| Ein unerwarteter Fehler ist aufgetreten. Wenn der Fehler weiterhin besteht, melden Sie ihn, und geben Sie dabei Folgendes an: Datum und Zeitpunkt des Fehlers, Anforderungsbezeichner aus dem Antwortheader X-RequestId und Clientbezeichner aus dem Anforderungsheader X-ClientTraceId.|
| 503000| Service is temporarily unavailable. (Der Dienst ist vorübergehend nicht verfügbar.) Versuchen Sie es erneut. Wenn der Fehler weiterhin besteht, melden Sie ihn, und geben Sie dabei Folgendes an: Datum und Zeitpunkt des Fehlers, Anforderungsbezeichner aus dem Antwortheader X-RequestId und Clientbezeichner aus dem Anforderungsheader X-ClientTraceId.|

