---
title: Qualifikation „Entitätserkennung“ für die kognitive Suche (Azure Search) | Microsoft-Dokumentation
description: Extrahieren Sie in Azure Search in einer Pipeline für die kognitive Suche verschiedene Entitätstypen aus Text.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: luisca
ms.openlocfilehash: 7599ab7eb7a6ff247548d988c57bdc6c501a5a6b
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2018
ms.locfileid: "52447586"
---
#    <a name="entity-recognition-cognitive-skill"></a>Die kognitive Qualifikation „Entitätserkennung“

Mit der Qualifikation **Entitätserkennung** (EntityRecognitionSkill) können Sie Entitäten aus verschiedenen Arten von Text extrahieren. 

> [!NOTE]
> Die kognitive Suche befindet sich derzeit in der öffentlichen Vorschauphase. Die Ausführung von Qualifikationsgruppen, das Extrahieren von Bildern und die Normalisierung werden derzeit kostenlos angeboten. Die Preise für diese Funktionen werden zu einem späteren Zeitpunkt bekannt gegeben. 

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.EntityRecognitionSkill

## <a name="data-limits"></a>Datengrenzwerte
Die maximale Größe eines Datensatzes ist 50.000 Zeichen, gemessen durch `String.Length`. Wenn Sie Ihre Daten teilen müssen, bevor Sie sie an die Schlüsselbegriffserkennung senden, denken Sie daran, die [Qualifikation „Text teilen“](cognitive-search-skill-textsplit.md) zu verwenden.

## <a name="skill-parameters"></a>Qualifikationsparameter

Bei den Parametern, die alle optional sind, wird die Groß-/Kleinschreibung beachtet.

| Parametername     | BESCHREIBUNG |
|--------------------|-------------|
| categories    | Array von zu extrahierenden Kategorien.  Mögliche Kategorietypen: `"Person"`, `"Location"`, `"Organization"`, `"Quantity"`, `"Datetime"`, `"URL"` und `"Email"`. Wenn keine Kategorie angegeben ist, werden alle Typen zurückgegeben.|
|defaultLanguageCode |  Sprachcode des Eingabetexts. Die folgenden Sprachen werden unterstützt: `de, en, es, fr, it`|
|minimumPrecision | Nicht verwendet. Für die zukünftige Verwendung reserviert. |
|includeTypelessEntites | Wenn dieser Parameter auf „True“ festgelegt ist und der Text eine bekannte Entität enthält, die aber keiner der unterstützten Kategorien zugeordnet werden kann, wird sie als Teil des komplexen Ausgabefelds `"entities"` zurückgegeben. Der Standardwert ist `false`. |


## <a name="skill-inputs"></a>Eingaben für die Qualifikation

| Eingabename      | BESCHREIBUNG                   |
|---------------|-------------------------------|
| languageCode  | Optional. Der Standardwert ist `"en"`.  |
| text          | Der zu analysierende Text          |

## <a name="skill-outputs"></a>Ausgaben der Qualifikation

**Hinweis**: Nicht alle Entitätskategorien werden für alle Sprachen unterstützt.
Die Extraktion der Typen `"Quantity"`, `"Datetime"`, `"URL"`, `"Email"` wird nur für _en_ und _es_ unterstützt.

| Ausgabename     | BESCHREIBUNG                   |
|---------------|-------------------------------|
| persons      | Ein Array von Zeichenfolgen, wobei jede Zeichenfolge den Namen einer Person darstellt. |
| locations  | Ein Array von Zeichenfolgen, wobei jede Zeichenfolge einen Ort darstellt. |
| organizations  | Ein Array von Zeichenfolgen, wobei jede Zeichenfolge eine Organisation darstellt. |
| quantities  | Ein Array von Zeichenfolgen, wobei jede Zeichenfolge eine Menge darstellt. |
| dateTimes  | Ein Array von Zeichenfolgen, wobei jede Zeichenfolge einen DateTime-Wert darstellt (wie im Text gezeigt). |
| urls | Ein Array von Zeichenfolgen, wobei jede Zeichenfolge eine URL darstellt. |
| emails | Ein Array von Zeichenfolgen, wobei jede Zeichenfolge eine E-Mail-Adresse darstellt. |
| namedEntities | Ein Array von komplexen Typen mit den folgenden Feldern: <ul><li>category</li> <li>value (der tatsächliche Entitätsname)</li><li>offset (die Fundstelle im Text)</li><li>confidence (derzeit nicht verwendet; wird auf einen Wert von -1 festgelegt)</li></ul> |
| entities | Ein Array von komplexen Typen, die umfangreiche Informationen zu den aus dem Text extrahierten Entitäten enthalten, mit den folgenden Feldern: <ul><li> name (der tatsächliche Entitätsname; stellt eine „normalisierte“ Form dar)</li><li> wikipediaId</li><li>wikipediaLanguage</li><li>wikipediaUrl (Link zur Wikipedia-Seite für die Entität)</li><li>bingId</li><li>type (Kategorie der erkannten Entität)</li><li>subtype (nur für bestimmte Kategorien verfügbar; ermöglicht eine präzisere Ansicht des Entitätstyps)</li><li> matches (eine komplexe Sammlung mit:)<ul><li>text (unformatierter Text für die Entität)</li><li>offset (Fundstelle)</li><li>length (Länge des unformatierten Texts für die Entität)</li></ul></li></ul> |

##  <a name="sample-definition"></a>Beispieldefinition

```json
  {
    "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
    "categories": [ "Person", "Email"],
    "defaultLanguageCode": "en",
    "includeTypelessEntities": true,
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
      },
      {
        "name": "emails",
        "targetName": "contact"
      },
      {
        "name": "entities"
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
             "text": "Contoso corporation was founded by John Smith. They can be reached at contact@contoso.com",
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
        "persons": [ "John Smith"],
        "emails":["contact@contoso.com"],
        "namedEntities": 
        [
          {
            "category":"Person",
            "value": "John Smith",
            "offset": 35,
            "confidence": -1
          }
        ],
        "entities":  
        [
          {
            "name":"John Smith",
            "wikipediaId": null,
            "wikipediaLanguage": null,
            "wikipediaUrl": null,
            "bingId": null,
            "type": "Person",
            "subType": null,
            "matches": [{
                "text": "John Smith",
                "offset": 35,
                "length": 10
            }]
          },
          {
            "name": "contact@contoso.com",
            "wikipediaId": null,
            "wikipediaLanguage": null,
            "wikipediaUrl": null,
            "bingId": null,
            "type": "Email",
            "subType": null,
            "matches": [
            {
                "text": "contact@contoso.com",
                "offset": 70,
                "length": 19
            }]
          },
          {
            "name": "Contoso",
            "wikipediaId": "Contoso",
            "wikipediaLanguage": "en",
            "wikipediaUrl": "https://en.wikipedia.org/wiki/Contoso",
            "bingId": "349f014e-7a37-e619-0374-787ebb288113",
            "type": null,
            "subType": null,
            "matches": [
            {
                "text": "Contoso",
                "offset": 0,
                "length": 7
            }]
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

+ [Vordefinierte Qualifikationen](cognitive-search-predefined-skills.md)
+ [Definieren einer Qualifikationsgruppe](cognitive-search-defining-skillset.md)