---
title: BreakSentence-Methode der Textübersetzungs-API
titlesuffix: Azure Cognitive Services
description: Verwenden Sie die BreakSentence-Methode der Textübersetzungs-API.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: reference
ms.date: 03/29/2018
ms.author: v-jansko
ms.openlocfilehash: 1202d49688bfd6aee50d1fa21c10423c071c6d92
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "46124982"
---
# <a name="translator-text-api-30-breaksentence"></a>Textübersetzungs-API 3.0: BreakSentence

Erkennt die Positionierung von Satzgrenzen in einem Textabschnitt.

## <a name="request-url"></a>Anfrage-URL

Sendet eine `POST`-Anforderung an:

```HTTP
https://api.cognitive.microsofttranslator.com/breaksentence?api-version=3.0
```

## <a name="request-parameters"></a>Anforderungsparameter

Die folgenden Anforderungsparameter werden in der Abfragezeichenfolge übergeben:

<table width="100%">
  <th width="20%">Query parameter (Abfrageparameter)</th>
  <th>BESCHREIBUNG</th>
  <tr>
    <td>api-version</td>
    <td>*Erforderlicher Abfrageparameter*<br/>Die vom Client angeforderte Version der API. Der Wert muss `3.0` sein.</td>
  </tr>
  <tr>
    <td>Language</td>
    <td>*Optionaler Abfrageparameter*<br/>Sprachtag, das die Sprache des Eingabetexts angibt. Wenn kein Code angegeben ist, wird die automatische Spracherkennung angewendet.</td>
  </tr>
  <tr>
    <td>script</td>
    <td>*Optionaler Abfrageparameter*<br/>Skripttag, das das Skript erkennt, das vom Eingabetext verwendet wird. Wenn kein Skript angegeben ist, wird vom Standardskript der Sprache ausgegangen.</td>
  </tr>
</table> 

Anforderungsheader enthalten Folgendes:

<table width="100%">
  <th width="20%">Header</th>
  <th>BESCHREIBUNG</th>
  <tr>
    <td>_Eine Autorisierung_<br/>_Header_</td>
    <td>*Erforderlicher Anforderungsheader*.<br/>Weitere Informationen finden Sie in den [verfügbaren Optionen für die Authentifizierung](./v3-0-reference.md#authentication).</td>
  </tr>
  <tr>
    <td>Content-Typ</td>
    <td>*Erforderlicher Anforderungsheader*.<br/>Gibt den Inhaltstyp der Nutzlast an. Mögliche Werte: `application/json`.</td>
  </tr>
  <tr>
    <td>Content-Length</td>
    <td>*Erforderlicher Anforderungsheader*.<br/>Die Länge des Anforderungstexts.</td>
  </tr>
  <tr>
    <td>X-ClientTraceId</td>
    <td>*Optional*.<br/>Eine vom Client erstellte GUID zur eindeutigen Identifizierung der Anforderung. Beachten Sie, dass Sie diesen Header weglassen können, wenn Sie die Ablaufverfolgungs-ID mithilfe eines Abfrageparameters namens `ClientTraceId` in die Abfragezeichenfolge einschließen.</td>
  </tr>
</table> 

## <a name="request-body"></a>Anforderungstext

Der Anforderungstext ist ein JSON-Array. Jedes Arrayelement ist ein JSON-Objekt mit einer Zeichenfolgeneigenschaft namens `Text`. Satzgrenzen werden für den Wert der `Text`-Eigenschaft berechnet. Nachfolgend finden Sie ein Beispiel für einen Anforderungstext, der aus einem Abschnitt besteht:

```json
[
    { "Text": "How are you? I am fine. What did you do today?" }
]
```

Es gelten die folgenden Einschränkungen:

* Das Array kann höchstens über 100 Elemente verfügen.
* Der Textwert eines Arrayelements kann 10.000 Zeichen (einschließlich Leerzeichen) nicht überschreiten.
* Der gesamte Text, der in einer Anforderung enthalten ist, kann 50.000 Zeichen (einschließlich Leerzeichen) nicht überschreiten.
* Wenn der Abfrageparameter `language` angegeben ist, müssen alle Arrayelemente in derselben Sprache sein. Wenn dies nicht der Fall ist, wird die automatische Spracherkennung auf jedes Arrayelement einzeln angewendet.

## <a name="response-body"></a>Antworttext

Eine erfolgreiche Antwort ist ein JSON-Array mit einem Ergebnis für jede Zeichenfolge im Eingabearray. Ein Ergebnisobjekt enthält die folgenden Eigenschaften:

  * `sentLen`: ein Integerarray stellt die Länge der Sätze im Textelement dar. Die Länge des Arrays stellt die Anzahl von Sätzen dar, und die Werte stehen jeweils für die Länge der einzelnen Sätze. 

  * `detectedLanguage`: ein Objekt, das über die folgenden Eigenschaften die erkannte Sprache beschreibt:

     * `language`: Code der erkannten Sprache.

     * `score`: ein float-Wert, der die Zuverlässigkeit des Ergebnisses angibt. Die Bewertung bewegt sich zwischen 0 (null) und 1, und eine niedrige Bewertung gibt an, dass die Zuverlässigkeit zweifelhaft ist.
     
    Beachten Sie, dass die `detectedLanguage`-Eigenschaft nur im Ergebnisobjekt enthalten ist, wenn die automatische Spracherkennung angefordert wird.

Eine JSON-Beispielantwort lautet wie folgt:

```json
[
  {
    "sentenceLengths": [ 13, 11, 22 ]
    "detectedLanguage": {
      "language": "en",
      "score": 401
    },
  }
]
```

## <a name="response-headers"></a>Antwortheader

<table width="100%">
  <th width="20%">Header</th>
  <th>BESCHREIBUNG</th>
  <tr>
    <td>X-RequestId</td>
    <td>Der Wert, der vom Dienst für die Identifizierung der Anforderung generiert wird. Er wird zu Problembehandlungszwecken verwendet.</td>
  </tr>
</table> 

## <a name="response-status-codes"></a>Antwortstatuscodes

Im Folgenden finden Sie die möglichen HTTP-Statuscodes, die eine Anforderung zurückgeben kann. 

<table width="100%">
  <th width="20%">Statuscode</th>
  <th>BESCHREIBUNG</th>
  <tr>
    <td>200</td>
    <td>Erfolgreich.</td>
  </tr>
  <tr>
    <td>400</td>
    <td>Einer der Abfrageparameter fehlt oder ist ungültig. Korrigieren Sie die Anforderungsparameter, bevor Sie es erneut versuchen.</td>
  </tr>
  <tr>
    <td>401</td>
    <td>Die Anforderung konnte nicht authentifiziert werden. Überprüfen Sie, ob die Anmeldeinformationen angegeben und gültig sind.</td>
  </tr>
  <tr>
    <td>403</td>
    <td>Die Anforderung ist nicht autorisiert. Weitere Informationen finden Sie in der Fehlermeldung. Diese weist oft darauf hin, dass alle kostenlosen Übersetzungen, die mit einer Testversion bereitgestellt wurden, aufgebraucht sind.</td>
  </tr>
  <tr>
    <td>429</td>
    <td>Der Aufrufer sendet zu viele Anforderungen.</td>
  </tr>
  <tr>
    <td>500</td>
    <td>Ein unerwarteter Fehler ist aufgetreten. Wenn der Fehler weiterhin besteht, melden Sie ihn, und gebe Sie Folgendes an: Datum und Zeitpunkt des Fehlers, Anforderungsbezeichner aus dem Anforderungsheader `X-RequestId` und Clientbezeichner aus dem Anforderungsheader `X-ClientTraceId`.</td>
  </tr>
  <tr>
    <td>503</td>
    <td>Der Server ist vorübergehend nicht verfügbar. Wiederholen Sie die Anforderung. Wenn der Fehler weiterhin besteht, melden Sie ihn, und gebe Sie Folgendes an: Datum und Zeitpunkt des Fehlers, Anforderungsbezeichner aus dem Anforderungsheader `X-RequestId` und Clientbezeichner aus dem Anforderungsheader `X-ClientTraceId`.</td>
  </tr>
</table> 

## <a name="examples"></a>Beispiele

Das folgende Beispiel zeigt, wie Sie Satzgrenzen für einen einzelnen Satz abrufen können. Die Sprache des Satzes wird automatisch vom Dienst erkannt.

# <a name="curltabcurl"></a>[curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/breaksentence?api-version=3.0" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'How are you? I am fine. What did you do today?'}]"
```

---

