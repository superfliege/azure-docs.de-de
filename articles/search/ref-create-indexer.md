---
title: Erstellen eines Indexers (Azure Search-Dienst-REST-API-Version=2017-11-11-Preview)
description: In der Vorschau-API wurden Indexer erweitert und umfassen jetzt outputFieldMapping-Parameter, um eine Anreicherungsausgabe einem Feld in einem Azure Search-Index zuzuordnen.
services: search
manager: pablocas
author: luiscabrer
ms.author: luisca
ms.date: 05/01/2018
ms.prod: azure
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: language-reference
ms.openlocfilehash: d47b14342caf0312e052584d20f1a9c86ca29cad
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
ms.locfileid: "33786919"
---
# <a name="create-indexer-azure-search-service-rest-api-version2017-11-11-preview"></a>Erstellen eines Indexers (Azure Search-Dienst-REST-API-Version=2017-11-11-Preview)

Diese API-Referenz ist eine vorschauspezifische Version der Dokumentation und umfasst Erweiterungen der Indizierung durch die kognitive Suche.

Ebenso wie bei der [allgemein verfügbaren](https://docs.microsoft.com/rest/api/searchservice/create-indexer) Version können Sie mithilfe einer HTTP POST-Anforderung einen neuen Indexer innerhalb eines Azure Search-Diensts erstellen. 

```http
POST https://[service name].search.windows.net/indexers?api-version=[api-version]  
    Content-Type: application/json  
    api-key: [admin key]  
```  
Der **api-key** muss ein Verwaltungsschlüssel sein (im Gegensatz zu einem Abfrageschlüssel). Weitere Informationen zu Schlüsseln finden Sie unter [Sicherheit in Azure Search](search-security-overview.md) im Abschnitt zur Authentifizierung. Unter [Erstellen eines Azure Search-Diensts im Portal](search-create-service-portal.md) wird erläutert, wie Sie die Dienst-URL und Schlüsseleigenschaften abrufen können, die in der Anforderung verwendet werden.

Alternativ können Sie PUT verwenden und den Namen der Datenquelle für den URI angeben. Wenn die Datenquelle nicht vorhanden ist, wird sie erstellt.  

```http
PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]  
```  
Die **api-version** ist erforderlich. Die aktuelle Version ist `2016-09-01`. Weitere Informationen finden Sie unter [API-Versionen in Azure Search](search-api-versions.md).

Um Informationen zum Erstellen von Indexern für verschiedene Datenplattformen zu erhalten, lesen Sie die Übersicht über [Indexer](search-indexer-overview.md), die eine vollständige Liste [verwandter Artikel](search-indexer-overview.md#next-steps) enthält.

> [!NOTE]  
>  Die maximal zulässige Anzahl von Indexern variiert je nach Preisstufe. Der kostenlose Dienst kann bis zu 3 Indexer enthalten. Der Standard-Dienst kann 50 Indexer enthalten. In den [Einschränkungen für Dienste](search-limits-quotas-capacity.md) finden Sie weitere Informationen.    

## <a name="request"></a>Anforderung  
 Der Anforderungstext enthält eine Indexer-Definition, in der die Datenquelle und der Zielindex für die Indizierung angegeben sind, sowie einen optionalen Indizierungszeitplan und -parameter.  

 Die Syntax für die Strukturierung der Anforderungsnutzlast ist wie folgt. Eine Beispielanforderung wird weiter unten in diesem Thema bereitgestellt.  

```json
{   
    "name" : "Required for POST, optional for PUT. The name of the indexer",  
    "description" : "Optional. Anything you want, or null",  
    "dataSourceName" : "Required. The name of an existing data source",  
    "targetIndexName" : "Required. The name of an existing index",  
    "schedule" : { Optional. See Indexing Schedule below. },  
    "parameters" : { Optional. See Indexing Parameters below. },  
    "fieldMappings" : { Optional. See fieldMappings below. },
    "outputFieldMappings" : { Required for enrichment pipelines. See outputFieldMappings below. },
    "disabled" : Optional boolean value indicating whether the indexer is disabled. False by default.
}  
```

### <a name="indexer-schedule"></a>Indexerzeitplan  
Ein Indexer kann optional einen Zeitplan angeben. Wenn ein Zeitplan vorliegt, wird der Indexer regelmäßig nach diesem Zeitplan ausgeführt. Der Planer ist integriert; Sie können keinen externen Planer verwenden. Der **Zeitplan** besitzt die folgenden Attribute: 

-   **Intervall**: Erforderlich. Ein Zeitdauerwert, der ein Intervall oder den Zeitraum für Indexer-Ausführungen angibt. Das kleinste zulässige Intervall beträgt 5 Minuten. Das längste ist ein Tag. Es muss als XSD-Wert „dayTimeDuration“ formatiert sein (eine eingeschränkte Teilmenge eines [ISO 8601-Zeitwerts](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration)). Das Muster sieht folgendermaßen aus: `"P[nD][T[nH][nM]]".` Beispiele: `PT15M` für ein Intervall von 15 Minuten, `PT2H` für ein Intervall von 2 Stunden.  

-   **startTime**: Optional. Ein UTC-datetime-Wert, der angibt, wann die Ausführung des Indexers beginnen soll.  

### <a name="indexer-parameters"></a>Indexerparameter  
 Optional kann ein Indexer mehrere Parameter angeben, die sein Verhalten beeinflussen. Alle unten aufgeführten Parameter sind optional.  

-   **maxFailedItems**: Die maximale Anzahl von Elementen, bei deren Indizierung ein Fehler auftreten darf. Wird die Anzahl überschritten, gilt die Ausführung des Indexers als fehlerhaft. Der Standardwert ist 0. Informationen zu Elementen mit Fehlern werden mit dem Vorgang [Indexerstatus abrufen &#40;Azure Search-Dienst-REST-API&#41;](https://docs.microsoft.com/rest/api/searchservice/get-indexer-status) zurückgegeben.  

-   **maxFailedItemsPerBatch**: Die maximale Anzahl von Elementen in jedem, bei deren Indizierung ein Fehler auftreten darf. Wird die Anzahl überschritten, gilt die Ausführung des Indexers als fehlerhaft. Der Standardwert ist 0.  

-   **batchSize**: Gibt die Anzahl von Elementen an, die aus einer Datenquelle gelesen und als einzelner Batch indiziert werden, um die Leistung zu verbessern. Der Standardwert richtet sich nach dem Datenquellentyp: 1000 für Azure SQL und Azure Cosmos DB und 10 für Azure Blob Storage.

### <a name="field-mapping-parameters"></a>Parameter für die Feldzuordnung

Indexerdefinitionen enthalten Feldzuordnungen, um in einem Azure Search-Index ein Quellfeld einem Zielfeld zuzuordnen. Es gibt zwei Arten von Zuordnungen, je nachdem, ob die Inhaltsübertragung einem direkten oder einem erweiterten Pfad folgt:

+ **fieldMappings** sind optional und werden angewendet, wenn die Feldnamen für Quelle und Ziel nicht übereinstimmen oder wenn Sie eine Funktion angeben möchten.
+ **outputFieldMappings** sind erforderlich, wenn Sie eine [Anreicherungspipeline](cognitive-search-concept-intro.md) erstellen. In einer Anreicherungspipeline ist das Ausgabefeld ein Konstrukt, das während des Anreicherungsprozesses definiert wird. Das Ausgabefeld kann beispielsweise eine zusammengesetzte Struktur sein, die während der Anreicherung aus zwei separaten Feldern im Quelldokument erstellt wird. 

#### <a name="fieldmappings"></a>fieldMappings

Im folgenden Beispiel wird eine Quelltabelle mit einem `_id`-Feld verwendet. Da Feldnamen in Azure Search nicht einem Unterstrich beginnen dürfen, muss das Feld umbenannt werden. Hierzu können Sie die `fieldMappings` -Eigenschaft des Indexers wie folgt verwenden:

```json
"fieldMappings" : [ { "sourceFieldName" : "_id", "targetFieldName" : "id" } ]
```

Sie können mehrere Feldzuordnungen angeben.

```json
"fieldMappings" : [
    { "sourceFieldName" : "_id", "targetFieldName" : "id" },
    { "sourceFieldName" : "_timestamp", "targetFieldName" : "timestamp" }
]
```

Beachten Sie bei Quell- und Zielfeldnamen die Groß-und Kleinschreibung.

Informationen zu Szenarien, in denen Feldzuordnungen nützlich sind, finden Sie unter [Feldzuordnungen in Search-Indexern](https://docs.microsoft.com/azure/search/search-indexer-field-mappings).

#### <a name="outputfieldmappings"></a>outputFieldMappings

In Szenarien mit kognitiver Suche, in denen ein Skillset an einen Indexer gebunden ist, müssen Sie `outputFieldMappings` hinzufügen, um jede Ausgabe eines Anreicherungsschritts zuzuordnen, der Inhalt für ein durchsuchbares Feld im Index bereitstellt.

```json
  "outputFieldMappings" : [
        {
          "sourceFieldName" : "/document/organizations", 
          "targetFieldName" : "organizations"
        },
        {
          "sourceFieldName" : "/document/pages/*/keyPhrases/*", 
          "targetFieldName" : "keyphrases"
        },
        {
            "sourceFieldName": "/document/languageCode",
            "targetFieldName": "language",
            "mappingFunction": null
        }      
   ],
```

<a name="FieldMappingFunctions"></a>

#### <a name="field-mapping-functions"></a>Feldzuordnungsfunktionen

Feldzuordnungen können auch verwendet werden, um Quellfeldwerte mithilfe von *Feldzuordnungsfunktionen*umzuwandeln. Ein beliebiger Zeichenfolgenwert kann beispielsweise Base64-codiert sein, damit er zum Auffüllen eines Dokumentschlüsselfelds verwendet werden kann.

Informationen dazu, wann und wie Sie Feldzuordnungsfunktionen verwenden, finden Sie unter [Feldzuordnungsfunktionen](https://docs.microsoft.com/azure/search/search-indexer-field-mappings#field-mapping-functions).

### <a name="request-body-examples"></a>Beispiele für einen Anforderungstext  
 Anhand des folgenden Beispiels wird ein Indexer erstellt, der Daten aus der Tabelle, die von der `ordersds`-Datenquelle referenziert wird, in den `orders`-Index kopiert. Der Vorgang erfolgt nach einem Zeitplan, der am 1. Januar 2015 UTC beginnt und stündlich ausgeführt wird. Die Aufrufe des Indexers sind erfolgreich, wenn nicht mehr als fünf zu indizierende Elemente in jedem Batch fehlschlagen und insgesamt nicht mehr als zehn zu indizierende Elemente fehlschlagen.  

```json
{
    "name" : "myindexer",  
    "description" : "a cool indexer",  
    "dataSourceName" : "ordersds",  
    "targetIndexName" : "orders",  
    "schedule" : { "interval" : "PT1H", "startTime" : "2015-01-01T00:00:00Z" },  
    "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 5 }  
}
```

## <a name="response"></a>response  
 Bei erfolgreicher Anforderung: "201 Erstellt".  

## <a name="see-also"></a>Weitere Informationen

+ [Übersicht über die kognitive Suche](cognitive-search-concept-intro.md)
+ [Schnellstart: Testen der kognitiven Suche](cognitive-search-quickstart-blob.md)
+ [Zuordnen von Feldern](cognitive-search-output-field-mapping.md)
