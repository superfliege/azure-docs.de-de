---
title: Der Skill „Benannte Entität erkennen“ der kognitiven Suche (Azure Search) | Microsoft-Dokumentation
description: Extrahieren Sie benannte Entitäten für Personen, Orte und Organisationen aus Text in einer Pipeline der kognitiven Suche in Azure Search.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 73ffcf5e2ced63fddaf0f5ef2ca7e72a7d94b966
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
ms.locfileid: "33786819"
---
#    <a name="named-entity-recognition-cognitive-skill"></a>Der kognitive Skill „Benannte Entität erkennen“

Der Skill **Benannte Entitäten erkennen** extrahiert benannte Entitäten aus Text. Zu den verfügbaren Entitäten gehören die Typen `person`, `location` und `organization`.

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.NamedEntityRecognitionSkill

## <a name="skill-parameters"></a>Skillparameter

Bei den Parametern wird zwischen Groß- und Kleinschreibung unterschieden.

| Parametername     | Beschreibung |
|--------------------|-------------|
| categories    | Array von zu extrahierenden Kategorien.  Mögliche Kategorietypen: `"Person"`, `"Location"`, `"Organization"`. Wenn keine Kategorie angegeben ist, werden alle Typen zurückgegeben.|
|defaultLanguageCode |  Sprachcode des Eingabetexts. Die folgenden Sprachen werden unterstützt: `ar, cs, da, de, en, es, fi, fr, he, hu, it, ko, pt-br, pt`|
| minimumPrecision  | Eine Zahl zwischen 0 und 1. Wenn die Genauigkeit unter diesem Wert liegt, wird die Entität nicht zurückgegeben. Der Standardwert ist 0.|

## <a name="skill-inputs"></a>Skilleingaben

| Eingabename      | Beschreibung                   |
|---------------|-------------------------------|
| languageCode  | Optional. Der Standardwert ist `"en"`.  |
| text          | Der zu analysierende Text          |

## <a name="skill-outputs"></a>Skillausgaben

| Ausgabename     | BESCHREIBUNG                   |
|---------------|-------------------------------|
| persons      | Ein Array von Zeichenfolgen, wobei jede Zeichenfolge den Namen einer Person darstellt. |
| locations  | Ein Array von Zeichenfolgen, wobei jede Zeichenfolge einen Ort darstellt. |
| organizations  | Ein Array von Zeichenfolgen, wobei jede Zeichenfolge eine Organisation darstellt. |
| entities | Ein Array komplexer Typen. Jeder komplexe Typ umfasst die folgenden Felder: <ul><li>category (`"person"`, `"organization"` oder `"location"`)</li> <li>value (der tatsächliche Entitätsname)</li><li>offset (die Fundstelle im Text)</li><li>confidence (Ein Wert zwischen 0 und 1, der das Vertrauen darstellt, dass der Wert eine tatsächliche Entität ist.)</li></ul> |

##  <a name="sample-definition"></a>Beispieldefinition

```json
  {
    "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
    "categories": [ "Person"],
    "defaultLanguageCode": "en",
    "inputs": [
      {
        "name": "text",
        "source": "/document/content"
      }
    ],
    "outputs": [
      {
        "name": "persons",
        "targetName": "people"
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
             "text": "This is a the loan application for Joe Romero, he is a Microsoft employee who was born in Chile and then moved to Australia… Ana Smith is provided as a reference.",
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
      "data" : 
      {
        "persons": [ "Joe Romero", "Ana Smith"],
        "locations": ["Seattle"],
        "organizations":["Microsoft Corporation"],
        "entities":  
        [
          {
            "category":"person",
            "value": "Joe Romero",
            "offset": 45,
            "confidence": 0.87
          },
          {
            "category":"person",
            "value": "Ana Smith",
            "offset": 59,
            "confidence": 0.87
          },
          {
            "category":"location",
            "value": "Seattle",
            "offset": 5,
            "confidence": 0.99
          },
          {
            "category":"organization",
            "value": "Microsoft Corporation",
            "offset": 120,
            "confidence": 0.99
          }
        ]
      }
    }
  ]
}
```


## <a name="error-cases"></a>Auftretende Fehler
Wenn Sie einen nicht unterstützten Sprachcode angeben oder Inhalt nicht mit der angegebenen Sprache übereinstimmt, wird ein Fehler zurückgegeben, und es werden keine Entitäten extrahiert.

## <a name="see-also"></a>Weitere Informationen

+ [Vordefinierte Skills](cognitive-search-predefined-skills.md)
+ [Definieren eines Skillsets](cognitive-search-defining-skillset.md)