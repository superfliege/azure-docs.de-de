---
title: Der Skill „Spracherkennung“ der kognitiven Suche (Azure Search) | Microsoft-Dokumentation
description: Bewertet unstrukturierten Text und gibt für jeden Datensatz einen Sprachbezeichner mit einer Punktzahl zurück, die die Stärke der Analyse in einer Azure Search-Anreicherungspipeline angibt.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 338d89b47ea451efcf8300d4ac016a6946a95259
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
ms.locfileid: "33786839"
---
#   <a name="language-detection-cognitive-skill"></a>Der kognitive Skill „Spracherkennung“

Bis zu 120 Sprachen des eingegebenen Textes erkennt die **Spracherkennung** und meldet einen einzigen Sprachcode für jedes Dokument, das auf Anforderung gesendet wird. Der Sprachcode ist mit einem Wert kombiniert, der die Stärke der Analyse angibt.

Diese Funktion ist besonders nützlich, wenn Sie die Sprache des Texts als Eingabe für andere Skills (z.B. den [Skill „Stimmung“](cognitive-search-skill-sentiment.md) oder den [Skill „Text teilen“](cognitive-search-skill-textsplit.md)) benötigen.

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.LanguageDetectionSkill

## <a name="data-limits"></a>Datengrenzwerte
Die maximale Größe eines Datensatzes ist 50.000 Zeichen, gemessen durch `String.Length`. Wenn Sie Ihre Daten teilen müssen, bevor Sie sie an das Stimmungsanalysetool senden, verwenden Sie den [Skill „Text teilen“](cognitive-search-skill-textsplit.md).

## <a name="skill-inputs"></a>Skilleingaben

Bei den Parametern wird zwischen Groß- und Kleinschreibung unterschieden.

| Eingaben     | Beschreibung |
|--------------------|-------------|
| text | Der zu analysierende Text|

## <a name="skill-outputs"></a>Skillausgaben

| Ausgabename    | Beschreibung |
|--------------------|-------------|
| languageCode | Der für die Sprache identifizierte ISO-6391-Sprachcode. Beispiel: „en“. |
| languageName | Der Name der Sprache. Beispiel: „Englisch“. |
| Ergebnis Ihrer App | Ein Wert zwischen 0 und 1 ein. Die Wahrscheinlichkeit, dass die Sprache korrekt identifiziert ist. Der Wert kann kleiner als 1 sein, wenn im Satz verschiedene Sprachen kombiniert sind.  |

##  <a name="sample-definition"></a>Beispieldefinition

```json
 {
    "@odata.type": "#Microsoft.Skills.Text.LanguageDetectionSkill ",
    "inputs": [
      {
        "name": "text",
        "source": "/document/text"
      }
    ],
    "outputs": [
      {
        "name": "languageCode",
        "targetName": "myLanguageCode"
      },
      {
        "name": "languageName",
        "targetName": "myLanguageName"
      },
      {
        "name": "score",
        "targetName": "myLanguageScore"
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
        "data":
           {
             "text": "Glaciers are huge rivers of ice that ooze their way over land, powered by gravity and their own sheer weight. "
           }
      },
      {
        "recordId": "2",
        "data":
           {
             "text": "Estamos muy felices de estar con ustedes."
           }
      }
    ]
```


##  <a name="sample-output"></a>Beispielausgabe

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
            {
              "languageCode": "en",
              "languageName": "English",
              "score": 1,
            }
      },
      {
        "recordId": "2",
        "data":
            {
              "languageCode": "es",
              "languageName": "Spanish",
              "score": 1,
            }
      }
    ]
}
```


## <a name="error-cases"></a>Auftretende Fehler
Wenn der Text in einer nicht unterstützten Sprache ausgedrückt wird, tritt ein Fehler auf, und es wird keine Sprachen-ID zurückgegeben.

## <a name="see-also"></a>Weitere Informationen

+ [Vordefinierte Skills](cognitive-search-predefined-skills.md)
+ [Definieren eines Skillsets](cognitive-search-defining-skillset.md)