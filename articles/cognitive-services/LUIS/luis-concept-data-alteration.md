---
title: Grundlegende Konzepte der Datenänderung in LUIS – Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Daten vor der Vorhersage in Language Understanding Intelligent Service (LUIS) geändert werden können.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/26/2018
ms.author: diberry
ms.openlocfilehash: d8421114bb5a7416ad2523fe9b0353f03f672619
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2018
ms.locfileid: "39223982"
---
# <a name="data-alterations"></a>Datenänderungen
LUIS bietet Möglichkeiten zum Bearbeiten einer Äußerung vor oder während der Vorhersage. 

## <a name="correct-spelling-errors-in-utterance"></a>Korrigieren von Rechtschreibfehlern in Äußerungen
LUIS verwendet die [Bing-Rechtschreibprüfungs-API V7](https://azure.microsoft.com/services/cognitive-services/spell-check/) um Rechtschreibfehler in Äußerungen zu korrigieren. LUIS benötigt den diesem Dienst zugewiesenen Schlüssel. Erstellen Sie den Schlüssel, und fügen Sie den Schlüssel dann als Parameter der Abfragezeichenfolge an den [Endpunkt](https://aka.ms/luis-endpoint-apis) hinzu. 

Sie können Rechtschreibfehler auch im **Testbereich** korrigieren, indem Sie den [Schlüssel eingeben](luis-interactive-test.md#view-bing-spell-check-corrections-in-test-panel). Der Schlüssel wird als Sitzungsvariable im Browser des Testbereichs beibehalten. Fügen Sie den Schlüssel dem Testbereich in jeder Browsersitzung hinzu, in der Sie Rechtschreibfehler korrigieren möchten. 

Die Verwendungen des Schlüssels im Testbereich und am Endpunkt werden zum Kontingent für die [Schlüsselverwendung](https://azure.microsoft.com/pricing/details/cognitive-services/spellcheck-api/) hinzugerechnet. LUIS implementiert die Limits für die Bing-Rechtschreibprüfung bezüglich der Textlänge. 

Der Endpunkt benötigt zwei Parameter, damit Rechtschreibkorrekturen funktionieren:

|Parameter|Wert|
|--|--|
|`spellCheck`|boolean|
|`bing-spell-check-subscription-key`|[Bing-Rechtschreibprüfungs-API V7](https://azure.microsoft.com/services/cognitive-services/spell-check/) – Endpunktschlüssel|

Wenn die [Bing-Rechtschreibprüfungs-API V7](https://azure.microsoft.com/services/cognitive-services/spell-check/) einen Fehler erkennt, werden die ursprüngliche Äußerung und die korrigierte Äußerung zusammen mit Vorhersagen vom Endpunkt zurückgegeben.

```JSON
{
  "query": "Book a flite to London?",
  "alteredQuery": "Book a flight to London?",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 0.780123
  },
  "entities": []
}
```
 
## <a name="change-time-zone-of-prebuilt-datetimev2-entity"></a>Ändern der Zeitzone von vordefinierten datetimeV2-Entitäten
Wenn eine LUIS-App eine vordefinierte datetimeV2-Entität verwendet, kann ein datetime-Wert in der Vorhersageantwort zurückgegeben werden. Der richtige datetime-Wert für die Rückgaben wird anhand der Zeitzone der Anforderung bestimmt. Wenn die Anforderung von einem Bot oder einer anderen zentralen Anwendung vor dem Empfang durch LUIS stammt, korrigieren Sie die von LUIS verwendete Zeitzone. 

### <a name="endpoint-querystring-parameter"></a>QueryString-Parameter für den Endpunkt
Sie korrigieren die Zeitzone, indem Sie am [Endpunkt](https://aka.ms/luis-endpoint-apis) mithilfe des `timezoneOffset`-Parameters die Zeitzone des Benutzers hinzufügen. Der Wert von `timezoneOffset` sollte die positive oder negative Zahl in Minuten sein, um die die Zeit geändert werden soll.  

|Parameter|Wert|
|--|--|
|`timezoneOffset`|Positive oder negative Zahl, in Minuten|

### <a name="daylight-savings-example"></a>Sommerzeitbeispiel
Wenn Sie den zurückgegebenen vordefinierten datetimeV2-Wert für die Anpassung an die Sommerzeit benötigen, sollten Sie den QueryString-Parameter `timezoneOffset` mit einem +/-Wert in Minuten für die Abfrage an den [Endpunkt](https://aka.ms/luis-endpoint-apis) verwenden.

Hinzufügen von 60 Minuten: 

https://{Region}.api.cognitive.microsoft.com/luis/v2.0/apps/{App-ID}?q=Turn the lights on?**timezoneOffset=60**&verbose={Boolean}&spellCheck={Boolean}&staging={Boolean}&bing-spell-check-subscription-key={Zeichenfolge}&log={Boolean}

Entfernen von 60 Minuten: 

https://{Region}.api.cognitive.microsoft.com/luis/v2.0/apps/{App-ID}?q=Turn the lights on?**timezoneOffset=-60**&verbose={Boolean}&spellCheck={Boolean}&staging={Boolean}&bing-spell-check-subscription-key={Zeichenfolge}&log={Boolean}

## <a name="c-code-determines-correct-value-of-timezoneoffset"></a>C#-Code zur Bestimmung des richtigen Werts für timezoneOffset
Der folgende C#-Code verwendet die [FindSystemTimeZoneById](https://docs.microsoft.com/dotnet/api/system.timezoneinfo.findsystemtimezonebyid?view=netframework-4.7.1#examples)-Methode der [TimeZoneInfo](https://docs.microsoft.com/dotnet/api/system.timezoneinfo?view=netframework-4.7.1)-Klasse, um das richtige `timezoneOffset` basierend auf der Systemzeit zu bestimmen:

```CSharp
// Get CST zone id
TimeZoneInfo targetZone = TimeZoneInfo.FindSystemTimeZoneById("Central Standard Time");

// Get local machine's value of Now
DateTime utcDatetime = DateTime.UtcNow;

// Get Central Standard Time value of Now
DateTime cstDatetime = TimeZoneInfo.ConvertTimeFromUtc(utcDatetime, targetZone);

// Find timezoneOffset
int timezoneOffset = (int)((cstDatetime - utcDatetime).TotalMinutes);
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Korrigieren Sie Rechtschreibfehler mithilfe dieses Tutorials.](luis-tutorial-bing-spellcheck.md)