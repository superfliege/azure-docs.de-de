---
title: Die Qualifikation „Benannte Entität erkennen“ der kognitiven Suche – Azure Search
description: Extrahieren Sie benannte Entitäten für Personen, Orte und Organisationen aus Text in einer Pipeline der kognitiven Suche in Azure Search.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 02/22/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: 28fae27b52ea150c1fa732715212e2f2c9534bc6
ms.sourcegitcommit: e88188bc015525d5bead239ed562067d3fae9822
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/24/2019
ms.locfileid: "56750428"
---
#    <a name="named-entity-recognition-cognitive-skill"></a>Der kognitive Skill „Benannte Entität erkennen“

Der Skill **Benannte Entitäten erkennen** extrahiert benannte Entitäten aus Text. Zu den verfügbaren Entitäten gehören die Typen `person`, `location` und `organization`.

> [!IMPORTANT]
> Die Qualifikation zur Erkennung benannter Entitäten gilt als veraltet und wird durch [Microsoft.Skills.Text.EntityRecognitionSkill](cognitive-search-skill-entity-recognition.md) ersetzt. Diese Qualifikation wird ab dem 15. Februar 2019 nicht mehr unterstützt. Führen Sie unter Berücksichtigung der Empfehlungen unter [Veraltete Qualifikationen für die kognitive Suche](cognitive-search-skill-deprecated.md) eine Migration zu einer unterstützten Qualifikation durch.

> [!NOTE]
> Seit dem 21. Dezember 2018 können Sie einer Azure Search-Qualifikationsgruppe eine [Cognitive Services-Ressource](cognitive-search-attach-cognitive-services.md) anfügen. Dies ermöglicht es uns, für die Ausführung von Qualifikationsgruppen mit der Gebührenberechnung zu beginnen. Ab diesem Datum haben wir außerdem damit begonnen, die Bildextraktion als Teil der Aufschlüsselung von Dokumenten zu berechnen. Die Textextraktion aus Dokumenten wird weiterhin ohne Zusatzkosten angeboten.
>
> Die Ausführung [integrierter kognitiver Qualifikationen](cognitive-search-predefined-skills.md) wird nach dem [nutzungsbasierten Preis für Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services) berechnet, und zwar zu demselben Tarif wie beim direkten Ausführen der Aufgabe. Für die Extraktion von Bildern fällt eine Azure Search-Gebühr an, die derzeit Vorschaupreisen entspricht. Ausführlichere Informationen finden Sie auf der [Seite „Azure Search – Preise“](https://go.microsoft.com/fwlink/?linkid=2042400) oder unter [Funktionsweise der Abrechnung](search-sku-tier.md#how-billing-works).

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.NamedEntityRecognitionSkill

## <a name="data-limits"></a>Datengrenzwerte
Die maximale Größe eines Datensatzes ist 50.000 Zeichen, gemessen durch `String.Length`. Wenn Sie Ihre Daten teilen müssen, bevor Sie sie an die Schlüsselbegriffserkennung senden, denken Sie daran, den [Skill „Text teilen“](cognitive-search-skill-textsplit.md) zu verwenden.

## <a name="skill-parameters"></a>Skillparameter

Bei den Parametern wird zwischen Groß- und Kleinschreibung unterschieden.

| Parametername     | BESCHREIBUNG |
|--------------------|-------------|
| categories    | Array von zu extrahierenden Kategorien.  Mögliche Kategorietypen: `"Person"`, `"Location"`, `"Organization"`. Wenn keine Kategorie angegeben ist, werden alle Typen zurückgegeben.|
|defaultLanguageCode |  Sprachcode des Eingabetexts. Die folgenden Sprachen werden unterstützt: `de, en, es, fr, it`|
| minimumPrecision  | Eine Zahl zwischen 0 und 1. Wenn die Genauigkeit unter diesem Wert liegt, wird die Entität nicht zurückgegeben. Der Standardwert ist 0.|

## <a name="skill-inputs"></a>Skilleingaben

| Eingabename      | BESCHREIBUNG                   |
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
    "categories": [ "Person", "Location", "Organization"],
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
             "text": "This is the loan application for Joe Romero, he is a Microsoft employee who was born in Chile and then moved to Australia… Ana Smith is provided as a reference.",
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
        "locations": ["Chile", "Australia"],
        "organizations":["Microsoft"],
        "entities":  
        [
          {
            "category":"person",
            "value": "Joe Romero",
            "offset": 33,
            "confidence": 0.87
          },
          {
            "category":"person",
            "value": "Ana Smith",
            "offset": 124,
            "confidence": 0.87
          },
          {
            "category":"location",
            "value": "Chile",
            "offset": 88,
            "confidence": 0.99
          },
          {
            "category":"location",
            "value": "Australia",
            "offset": 112,
            "confidence": 0.99
          },
          {
            "category":"organization",
            "value": "Microsoft",
            "offset": 54,
            "confidence": 0.99
          }
        ]
      }
    }
  ]
}
```


## <a name="error-cases"></a>Auftretende Fehler
Wird der Sprachcode für das Dokument nicht unterstützt, wird ein Fehler zurückgegeben, und es werden keine Entitäten extrahiert.

## <a name="see-also"></a>Weitere Informationen

+ [Vordefinierte Skills](cognitive-search-predefined-skills.md)
+ [Definieren eines Skillsets](cognitive-search-defining-skillset.md)
+ [Die kognitive Qualifikation „Entitätserkennung“](cognitive-search-skill-entity-recognition.md)
