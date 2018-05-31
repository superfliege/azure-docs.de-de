---
title: Der Skill „Stimmung“ der kognitiven Suche (Azure Search) | Microsoft-Dokumentation
description: Extrahieren Sie die Stimmung aus Text in einer Azure Search-Anreicherungspipeline.
services: search
manager: pablocas
author: luiscabrer
documentationcenter: ''
ms.assetid: ''
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 1ddbba5b881cd05a997cd24a9396d5b722376e6f
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
ms.locfileid: "33786759"
---
#   <a name="sentiment-cognitive-skill"></a>Der kognitive Skill „Stimmung“

Der Skill **Stimmung** bewertet unstrukturierten Text anhand eines Positiv-Negativ-Kontinuums und gibt für jeden Datensatz einen numerischen Wert zwischen 0 und 1 zurück. Dabei weisen Werte nahe 1 auf eine positive Stimmung und Werte nahe 0 auf eine negative Stimmung hin.

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.SentimentSkill

## <a name="data-limits"></a>Datengrenzwerte
Die maximale Größe eines Datensatzes sollte 5000 Zeichen betragen, wie mit `String.Length` gemessen. Wenn Sie Ihre Daten teilen müssen, bevor Sie sie an das Stimmungsanalysetool senden, verwenden Sie den [Skill „Text teilen“](cognitive-search-skill-textsplit.md).


## <a name="skill-parameters"></a>Skillparameter

Bei den Parametern wird zwischen Groß- und Kleinschreibung unterschieden.

| Parametername |                      |
|----------------|----------------------|
| defaultLanguageCode | (Optional) Der Sprachcode, der für Dokumente gilt, die nicht explizit eine Sprache angeben. <br/> Siehe die [vollständige Liste der unterstützten Sprachen](../cognitive-services/text-analytics/text-analytics-supported-languages.md). |

## <a name="skill-inputs"></a>Skilleingaben 

| Eingabename | Beschreibung |
|--------------------|-------------|
| text | Der zu analysierende Text.|
| languageCode  |  (Optional) Eine Zeichenfolge, die die Sprache der Datensätze angibt. Wenn dieser Parameter nicht festgelegt ist, wird der Standardwert „en“ verwendet. <br/>Siehe die [vollständige Liste der unterstützten Sprachen](../cognitive-services/text-analytics/text-analytics-supported-languages.md).|

## <a name="skill-outputs"></a>Skillausgaben

| Ausgabename | Beschreibung |
|--------------------|-------------|
| score | Ein Wert zwischen 0 und 1, der die Stimmung des analysierten Textes repräsentiert. Werte nahe 0 stehen für eine negative, nahe 0,5 für eine neutrale und nahe 1 für eine positive Stimmung.|


##  <a name="sample-definition"></a>Beispieldefinition

```json
{
    "@odata.type": "#Microsoft.Skills.Text.SentimentSkill",
    "inputs": [
        {
            "name": "text",
            "source": "/document/content"
        },
        {
            "name": "languageCode",
            "source": "/document/languagecode"
        }
    ],
    "outputs": [
        {
            "name": "score",
            "targetName": "mySentiment"
        }
    ]
}
```

##  <a name="sample-input"></a>Beispieleingabe

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "text": "I had a terrible time at the hotel. The staff was rude and the food was awful.",
                "languageCode": "en"
            }
        }
    ]
}
```


##  <a name="sample-output"></a>Beispielausgabe

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "score": 0.01
            }
        }
    ]
}
```

## <a name="notes"></a>Notizen
Wenn der Wert leer ist, wird für diese Datensätze kein Stimmungswert zurückgegeben.

## <a name="error-cases"></a>Auftretende Fehler
Wenn eine Sprache nicht unterstützt wird, wird eine Fehlermeldung generiert und kein Stimmungswert zurückgegeben.

## <a name="see-also"></a>Weitere Informationen

+ [Vordefinierte Skills](cognitive-search-predefined-skills.md)
+ [Definieren eines Skillsets](cognitive-search-defining-skillset.md)