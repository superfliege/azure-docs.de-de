---
title: Erstellen eines Skillsets in einer Pipeline der kognitiven Suche (Azure Search) | Microsoft-Dokumentation
description: Definieren Sie Datenextrahierung, Verarbeitung natürlicher Sprache oder Bildanalyseschritte, um strukturierte Informationen aus Ihren Daten für die Verwendung in Azure Search anzureichern und zu extrahieren.
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 3ab35cfd8ce5cf54a68473736fe05b78d26850de
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
ms.locfileid: "33786869"
---
# <a name="how-to-create-a-skillset-in-an-enrichment-pipeline"></a>Erstellen eines Skillsets in einer Anreicherungspipeline

Die kognitive Suche extrahiert Daten und reichert diese an, um sie in Azure Search durchsuchbar zu machen. Extrahierungs- und Anreicherungsschritte werden als *kognitive Skills* bezeichnet, die in einem *Skillset* kombiniert sind, auf das während der Indizierung verwiesen wird. Ein Skillset kann [vordefinierte Skills](cognitive-search-predefined-skills.md) oder benutzerdefinierte Skills verwenden (Weitere Informationen finden Sie unter [Beispiel: Erstellen eines benutzerdefinierten Skills](cognitive-search-create-custom-skill-example.md)).

In diesem Artikel erfahren Sie, wie Sie eine Anreicherungspipeline für die Skills erstellen, die Sie verwenden möchten. Ein Skillset wird an einen [Azure Search-Indexer](search-indexer-overview.md) angehängt. Ein Teil des Pipelineentwurfs, der in diesem Artikel behandelt wird, ist die Konstruktion des Skillsets selbst. 

> [!NOTE]
> Ein weiterer Teil des Pipelineentwurfs ist die Festlegung eines Indexers im [nächsten Schritt](#next-step). Eine Indexerdefinition enthält einen Verweis auf das Skillset sowie Feldzuordnungen, die für die Verbindung von Eingaben zu Ausgaben im Zielindex verwendet werden.

Wichtige Punkte, die Sie beachten sollten:

+ Pro Indexer kann nur ein Skillset vorhanden sein.
+ Ein Skillset muss mindestens einen Skill umfassen.
+ Sie können mehrere Skills des gleichen Typs erstellen (z.B. Varianten eines Bildanalyseskills), aber jeder Skill kann nur einmal innerhalb eines Skillsets verwendet werden.

## <a name="begin-with-the-end-in-mind"></a>Denken Sie bereits zu Beginn an das Ende

Ein empfohlener erster Schritt ist die Entscheidung, welche Daten Sie aus Ihren Rohdaten extrahieren und wie Sie diese Daten in einer Suchanwendung verwenden wollen. Das Erstellen einer Abbildung der gesamten Anreicherungspipeline kann Ihnen helfen, die notwendigen Schritte zu identifizieren.

Angenommen, Sie möchten eine Reihe von Kommentaren von Finanzexperten verarbeiten. Für jede Datei möchten Sie Firmennamen und die allgemeine Stimmung der Kommentare extrahieren. Sie können auch eine benutzerdefinierte Anreicherungsfunktion schreiben, die den Bing-Entitätssuche-Dienst verwendet, um zusätzliche Informationen über das Unternehmen zu finden, z.B. welche Art von Geschäft das Unternehmen betreibt. Im Wesentlichen möchten Sie Informationen wie die folgenden extrahieren, die für jedes Dokument indiziert sind:

| Datensatztext | Unternehmen | Stimmung | Unternehmensbeschreibung |
|--------|-----|-----|-----|
|Beispiel-Datensatz| [„Microsoft“, „LinkedIn“] | 0,99 | [„Microsoft Corporation ist ein amerikanisches multinationales Technologieunternehmen....“ „LinkedIn ist ein wirtschafts- und beschäftigungsorientiertes soziales Netzwerk....“]

Das folgende Diagramm veranschaulicht eine hypothetische Anreicherungspipeline:

![Eine hypothetische Anreicherungspipeline](media/cognitive-search-defining-skillset/sample-skillset.png "Eine hypothetische Anreicherungspipeline")


Sobald Sie eine ungefähre Vorstellung davon haben, wie die Pipeline aussehen soll, können Sie das Skillset für die erforderlichen Schritte erstellen. Funktionell wird das Skillset ausgedrückt, sobald Sie Ihre Indexerdefinition in Azure Search hochladen. Weitere Informationen darüber, wie Sie Ihren Indexer hochladen können, finden Sie in der [Indexerdokumentation](https://docs.microsoft.com/rest/api/searchservice/create-indexer).


Im Diagramm erfolgt der Schritt zur *Dokumententschlüsselung* automatisch. Bekannte Dateien werden in Azure Search problemlos geöffnet, und es wird ein *content*-Feld erstellt, das den aus jedem Dokument extrahierten Text enthält. Die weißen Felder sind integrierte Anreicherungsfunktionen, und das gepunktete Feld „Bing Entity Search“ (Bing-Entitätssuche) stellt eine benutzerdefinierte Anreicherungsfunktion dar, die Sie erstellen. Wie dargestellt, enthält das Skillset drei Skills.

## <a name="skillset-definition-in-rest"></a>Skillsetdefinition in REST

Ein Skillset wird als ein Array von Skills definiert. Jeder Skill definiert die Quelle der Eingaben und den Namen der erzeugten Ausgaben. Mit der [REST-API zum Erstellen von Skillsets](ref-create-skillset.md) können Sie einen Skillset definieren, der dem vorherigen Diagramm entspricht: 

```http
PUT https://[servicename].search.windows.net/skillsets/[skillset name]?api-version=2017-11-11-Preview
api-key: [admin key]
Content-Type: application/json
```

```json
{
  "description": 
  "Extract sentiment from financial records, extract company names, and then find additional information about each company mentioned.",
  "skills":
  [
    {
      "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
      "context": "/document",
      "categories": [ "Organization" ],
      "defaultLanguageCode": "en",
      "inputs": [
        {
          "name": "text",
          "source": "/document/content"
        }
      ],
      "outputs": [
        {
          "name": "organizations",
          "targetName": "organizations"
        }
      ]
    },
    {
      "@odata.type": "#Microsoft.Skills.Text.SentimentSkill",
      "inputs": [
        {
          "name": "text",
          "source": "/document/content"
        }
      ],
      "outputs": [
        {
          "name": "score",
          "targetName": "mySentiment"
        }
      ]
    },
    {
      "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
     "description": "Calls an Azure function, which in turn calls Bing Entity Search",
      "uri": "https://indexer-e2e-webskill.azurewebsites.net/api/InvokeTextAnalyticsV3?code=foo",
      "httpHeaders": {
          "Ocp-Apim-Subscription-Key": "foobar",
      },
      "context": "/document/content/organizations/*",
      "inputs": [
        {
          "name": "query",
          "source": "/document/content/organizations/*"
        }
      ],
      "outputs": [
        {
          "name": "description",
          "targetName": "companyDescription"
        }
      ]
    }
  ]
}
```

## <a name="create-a-skillset"></a>Erstellen eines Skillsets

Beim Erstellen eines Skillsets können Sie eine Beschreibung angeben, um das Skillset selbstdokumentierend zu gestalten. Eine Beschreibung ist optional, aber zum Nachverfolgen der Wirkungsweise eines Skillsets durchaus nützlich. Da das Skillset ein JSON-Dokument ist, das keine Kommentare zulässt, müssen Sie dafür ein `description`-Element verwenden.

```json
{
  "description": 
  "This is our first skill set, it extracts sentiment from financial records, extract company names, and then finds additional information about each company mentioned.",
  ...
}
```

Das nächste Segment im Skillset ist ein Array von Skills. Man kann sich jeden Skill als einen Grundtyp für die Anreicherung vorstellen. In dieser Anreicherungspipeline führt jeder Skill eine kleine Aufgabe aus. Dies ist eine Eingabe (oder eine Reihe von Eingaben), durch die einige Ausgaben zurückgegeben werden. In den nächsten Abschnitten geht es darum, vordefinierte und benutzerdefinierte Skills festzulegen und diese durch Eingabe- und Ausgabeverweise miteinander zu verketten. Eingaben können aus Quelldaten oder aus einem anderen Skill stammen. Ausgaben können einem Feld in einem Suchindex zugeordnet oder als Eingabe für einen nachfolgenden Skill verwendet werden.

## <a name="add-predefined-skills"></a>Hinzufügen vordefinierter Skills

Betrachten Sie den ersten Skill, bei dem es sich um den vordefinierten [Skill zur Erkennung von Entitäten](cognitive-search-skill-named-entity-recognition.md) handelt:

```json
    {
      "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
      "context": "/document",
      "categories": [ "Organization" ],
      "defaultLanguageCode": "en",
      "inputs": [
        {
          "name": "text",
          "source": "/document/content"
        }
      ],
      "outputs": [
        {
          "name": "organizations",
          "targetName": "organizations"
        }
      ]
    }
```

* Jeder vordefinierte Skill verfügt über die Eigenschaften `odata.type`, `input` und `output`. Skillspezifische Eigenschaften bieten zusätzliche Informationen, die auf diesen Skill anwendbar sind. Bei der Entitätserkennung ist `categories` eine Entität aus einem festen Satz von Entitätstypen, die das vortrainierte Modell erkennen kann.

* Jeder Skill muss über ein ```"context"```-Element verfügen. Der Kontext stellt die Ebene dar, auf der Vorgänge ausgeführt werden. Im oben gezeigten Skill ist der Kontext das gesamte Dokument. Das bedeutet, dass die der Skill zur Erkennung benannter Entitäten einmal pro Dokument aufgerufen wird. Auf dieser Ebene werden auch Ausgaben erzeugt. Genauer gesagt werden ```"organizations"``` als Element von ```"/document"``` generiert. In den nachfolgenden Skills wird auf diese neu erstellten Informationen mit ```"/document/organizations"``` verwiesen.  Wenn das Feld ```"context"``` nicht explizit festgelegt ist, wird das Dokument als Standardkontext genommen.

* Der Skill hat eine Eingabe namens „text“, wobei die Eingabe der Quelle auf ```"/document/content"``` gesetzt ist. Der Skill (Erkennung benannter Entitäten) wird für das *content*-Feld jedes Dokuments ausgeführt. Hierbei handelt es sich um ein Standardfeld, das vom Azure Blob-Indexer erstellt wird. 

* Der Skill verfügt über eine Ausgabe mit der Bezeichnung ```"organizations"```. Ausgaben gibt es nur während der Verarbeitung. Um diese Ausgabe mit der Eingabe eines nachfolgenden Skills zu verketten, verweisen Sie auf die Ausgabe mit ```"/document/organizations"```.

* Für ein bestimmtes Dokument ist der Wert von ```"/document/organizations"``` ein Array von Organisationen, die aus dem Text extrahiert werden. Beispiel: 

  ```json
  ["Microsoft", "LinkedIn"]
  ```

In manchen Situationen ist es notwendig, auf jedes Element eines Arrays einzeln zu verweisen. Angenommen, Sie möchten jedes Element von ```"/document/organizations"``` separat an einen anderen Skill übergeben (wie z.B. die benutzerdefinierte Anreicherungsfunktion der Bing-Entitätssuche). Sie können auf jedes Element des Arrays verweisen, indem Sie dem Pfad ein Sternchen hinzufügen: ```"/document/organizations/*"``` 

Der zweite Skill zum Extrahieren von Stimmungen folgt dem gleichen Muster wie die erste Anreicherungsfunktion. Er verwendet ```"/document/content"``` als Eingabe und gibt einen Wert für die Stimmung für jede Inhaltsinstanz zurück. Da Sie das Feld ```"context"``` nicht explizit festgelegt haben, ist die Ausgabe (mySentiment) nun ein untergeordnetes Element von ```"/document"```.

```json
    {
      "@odata.type": "#Microsoft.Skills.Text.SentimentSkill",
      "inputs": [
        {
          "name": "text",
          "source": "/document/content"
        }
      ],
      "outputs": [
        {
          "name": "score",
          "targetName": "mySentiment"
        }
      ]
    },
```

## <a name="add-a-custom-skill"></a>Hinzufügen eines benutzerdefinierten Skills

Erinnern Sie sich an die Struktur der benutzerdefinierten Anreicherungsfunktion für die Bing-Entitätssuche:

```json
    {
      "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
     "description": "This skill calls an Azure function, which in turn calls Bing Entity Search",
      "uri": "https://indexer-e2e-webskill.azurewebsites.net/api/InvokeTextAnalyticsV3?code=foo",
      "httpHeaders": {
          "Ocp-Apim-Subscription-Key": "foobar",
      }
      "context": "/document/content/organizations/*",
      "inputs": [
        {
          "name": "query",
          "source": "/document/content/organizations/*"
        }
      ],
      "outputs": [
        {
          "name": "description",
          "targetName": "companyDescription"
        }
      ]
    }
```

Diese Definition ist ein benutzerdefinierter Skill, der eine Web-API als Teil des Anreicherungsprozesses aufruft. Für jede Organisation, die durch die Erkennung von benannten Entitäten identifiziert wird, ruft dieser Skill eine Web-API auf, um die Beschreibung dieser Organisation zu suchen. Eine interne Anreicherungs-Engine regelt die Orchestrierung, wann die Web-API aufgerufen werden soll und wie die empfangenen Informationen weitergeleitet werden sollen. Die für den Aufruf dieser benutzerdefinierten API erforderliche Initialisierung muss jedoch über das JSON-Dokument bereitgestellt werden (z.B. URI, HTTP-Header und die erwarteten Eingaben). Informationen zum Erstellen einer benutzerdefinierten Web-API für die Anreicherungspipeline finden Sie unter [Definieren einer benutzerdefinierten Schnittstelle](cognitive-search-custom-skill-interface.md).

Beachten Sie, dass das Feld „context“ mit einem Sternchen auf ```"/document/content/organizations/*"``` gesetzt ist, d.h. der Anreicherungsschritt wird *für jede* Organisation unter ```"/document/content/organizations"``` aufgerufen. 

Die Ausgabe, in diesem Fall eine Firmenbeschreibung, wird für jede identifizierte Organisation generiert. Bei Bezugnahme auf die Beschreibung in einem nachfolgenden Schritt (z.B. bei der Schlüsselbegriffserkennung) würden Sie dazu den Pfad ```"/document/content/organizations/*/description"``` verwenden. 

## <a name="enrichments-create-structure-out-of-unstructured-information"></a>Strukturieren von unstrukturierten Informationen mithilfe von Anreicherungen

Das Skillset generiert strukturierte Informationen aus unstrukturierten Daten. Betrachten Sie das folgende Beispiel:

*„In seinem vierten Quartal verzeichnete Microsoft 1,1 Milliarden Dollar Umsatz mit LinkedIn, dem im letzten Jahr erworbenen Unternehmen für soziale Netzwerke. Die Übernahme ermöglicht es Microsoft, LinkedIn-Funktionen mit seinen CRM- und Office-Funktionen zu kombinieren. Die Aktionäre sind von den bisherigen Fortschritten begeistert.“*

Ein wahrscheinliches Ergebnis wäre eine generierte Struktur ähnlich der folgenden Abbildung:

![Beispiel für Ausgabestruktur](media/cognitive-search-defining-skillset/enriched-doc.png "Beispiel für Ausgabestruktur")

Beachten Sie, dass dies eine interne Struktur ist. Dieses Diagramm können Sie nicht tatsächlich aus dem Code abrufen.

<a name="next-step"></a>
## <a name="next-steps"></a>Nächste Schritte

Nun, da Sie mit der Anreicherungspipeline und den Skillsets vertraut sind, können Sie mit den Artikeln [Verweisen auf Anmerkungen in einem Skillset](cognitive-search-concept-annotations-syntax.md) oder [Zuordnen von Ausgaben zu Feldern in einem Index](cognitive-search-output-field-mapping.md) fortfahren. 