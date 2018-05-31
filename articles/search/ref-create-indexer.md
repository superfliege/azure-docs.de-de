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
ms.openlocfilehash: 595502ecf0a78491c73800e8077de65707c7a486
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
ms.locfileid: "34365184"
---
# <a name="create-indexer-azure-search-service-rest-api-version2017-11-11-preview"></a>Erstellen eines Indexers (Azure Search-Dienst-REST-API-Version=2017-11-11-Preview)

Diese API-Referenz ist eine vorschauspezifische Version der Dokumentation und erweitert die API zum Erstellen eines Indexers um Elemente der [kognitiven Suche](cognitive-search-concept-intro.md). Ebenso wie bei der [allgemein verfügbaren](https://docs.microsoft.com/rest/api/searchservice/create-indexer) Version können Sie mithilfe einer HTTP POST-Anforderung einen neuen Indexer innerhalb eines Azure Search-Diensts erstellen. 

```http
POST https://[service name].search.windows.net/indexers?api-version=2017-11-11-Preview
    Content-Type: application/json  
    api-key: [admin key]  
```  
Der **api-key** muss ein Verwaltungsschlüssel sein (im Gegensatz zu einem Abfrageschlüssel). Weitere Informationen zu Schlüsseln finden Sie unter [Sicherheit in Azure Search](search-security-overview.md) im Abschnitt zur Authentifizierung. Unter [Erstellen eines Azure Search-Diensts im Portal](search-create-service-portal.md) wird erläutert, wie Sie die Dienst-URL und Schlüsseleigenschaften abrufen können, die in der Anforderung verwendet werden.

Alternativ können Sie PUT verwenden und den Namen der Datenquelle für den URI angeben. Wenn die Datenquelle nicht vorhanden ist, wird sie erstellt.  

```http
PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]  
```  
Die **api-version** ist erforderlich. Die aktuelle allgemein verfügbare Version ist `api-version=2017-11-11`, aber Sie benötigen die Vorschauversion für die kognitive Suche: `api-version=2017-11-11-Preview`.  Weitere Informationen finden Sie unter [API-Versionen in Azure Search](search-api-versions.md).

Um Informationen zum Erstellen von Indexern für verschiedene Datenplattformen zu erhalten, lesen Sie die Übersicht über [Indexer](search-indexer-overview.md), die eine vollständige Liste [verwandter Artikel](search-indexer-overview.md#next-steps) enthält.

> [!NOTE]  
>  Die maximal zulässige Anzahl von Indexern variiert je nach Preisstufe. Der kostenlose Dienst kann bis zu 3 Indexer enthalten. Der Standard-Dienst kann 50 Indexer enthalten. High-Definition-Standarddienste verfügen nicht über Unterstützung von Indexern. In den [Einschränkungen für Dienste](search-limits-quotas-capacity.md) finden Sie weitere Informationen.    

## <a name="request"></a>Anforderung  

Eine [Datenquelle](https://docs.microsoft.com/rest/api/searchservice/create-data-source), ein [Index](https://docs.microsoft.com/rest/api/searchservice/create-index) und ein [Skillset](ref-create-skillset.md) sind Teil der Definition eines [Indexers](search-indexer-overview.md). Es handelt sich aber jeweils um eine unabhängige Komponente, die in unterschiedlichen Kombinationen verwendet werden kann. Beispielsweise können Sie dieselbe Datenquelle mit mehreren Indexern, denselben Index mit mehreren Indexern oder mehrere Indexer verwenden, die in einen gemeinsamen Index schreiben.

 Der Text einer Anforderung enthält eine Indexerdefinition mit den folgenden Bestandteilen:

+ [dataSourceName](#dataSourceName)
+ [targetIndexName](#targetIndexName)
+ [skillsetName](#skillset)
+ [schedule](#indexer-schedule)
+ [parameters](#indexer-parameters)
+ [fieldMappings](#field-mappings)
+ [outputFieldMappings](#output-fieldmappings)

## <a name="request-syntax"></a>Anforderungssyntax

Die Syntax für die Strukturierung der Anforderungsnutzlast lautet wie hier angegeben. Eine Beispielanforderung wird weiter unten in diesem Artikel bereitgestellt.  

```json
{   
    "name" : "Required for POST, optional for PUT. The name of the indexer",  
    "description" : "Optional. Anything you want, or null",  
    "dataSourceName" : "Required. The name of an existing data source",  
    "targetIndexName" : "Required. The name of an existing index",  
    "skillsetName" : "Required for cognitive search enrichment",
    "schedule" : { Optional, but immediately runs once if unspecified. See Indexing Schedule below. },  
    "parameters" : { Optional. See Indexing Parameters below. },  
    "fieldMappings" : { Optional. See fieldMappings below. },
    "outputFieldMappings" : { Required for enrichment pipelines. See outputFieldMappings below. },
    "disabled" : Optional boolean value indicating whether the indexer is disabled. False by default.
}  
```
<a name="dataSourceName"></a>

### <a name="datasourcename"></a>"dataSourceName"

Eine [Datenquellendefinition](https://docs.microsoft.com/rest/api/searchservice/create-data-source) enthält häufig Eigenschaften, die ein Indexer verwenden kann, um Merkmale der Quellplattform auszunutzen. Die Datenquelle, die Sie an den Indexer übergeben, bestimmt die Verfügbarkeit bestimmter Eigenschaften und Parameter, z.B. die Filterung des Inhaltstyps in Azure-Blobs oder den Abfragetimeout für Azure SQL-Datenbank. 

<a name="targetIndexName"></a>

### <a name="targetindexname"></a>"targetIndexName"

Ein [Indexschema](https://docs.microsoft.com/rest/api/searchservice/create-index) definiert die Feldsammlung mit durchsuchbaren, filterbaren, abrufbaren und anderen Attributzuordnungen, über die festgelegt wird, wie das Feld verwendet wird. Während der Indizierung führt der Indexer einen Crawlvorgang für die Datenquelle durch, kann optional Dokumente aufbrechen und Informationen extrahieren, serialisiert die Ergebnisse im JSON-Format und indiziert die Nutzlast basierend auf dem für Ihren Index definierten Schema.

<a name="skillset"></a>

### <a name="skillsetname"></a>"skillsetName"

Die [kognitive Suche (Vorschauversion)](cognitive-search-concept-intro.md) bezieht sich auf Funktionen für natürliche Sprache und Bildverarbeitung in Azure Search, die bei der Datenerfassung zum Extrahieren von Entitäten, Schlüsselwörtern, Sprache, Informationen aus Bildern usw. angewendet wird. Transformationen, die auf Inhalt angewendet werden, werden über *Skills* durchgeführt, die Sie zu einem [*Skillset*](ref-create-skillset.md) kombinieren (eins pro Indexer). Wie bei Datenquellen und Indizes auch, ist ein Skillset eine unabhängige Komponente, die Sie an einen Indexer anfügen. Sie können ein Skillset auch für einen anderen Indexer anpassen, aber für jeden Indexer kann jeweils nur ein Skillset verwendet werden.
 
<a name="indexer-schedule"></a>

### <a name="schedule"></a>"schedule"  
Ein Indexer kann optional einen Zeitplan angeben. Ohne Zeitplan wird der Indexer sofort ausgeführt, wenn Sie die Anforderung senden: Es wird eine Verbindung hergestellt, der Crawlvorgang durchgeführt und die Datenquelle indiziert. Für einige Szenarien, z.B. Indizierungsaufträge mit langer Ausführungsdauer, werden Zeitpläne genutzt, um [das Verarbeitungsfenster zu erweitern](search-howto-reindex.md#scale-out-indexing) (über das Maximum von 24 Stunden hinaus). Wenn ein Zeitplan vorliegt, wird der Indexer regelmäßig nach diesem Zeitplan ausgeführt. Der Planer ist integriert. Sie können keinen externen Planer verwenden. Ein **Zeitplan** besitzt die folgenden Attribute: 

-   **Intervall**: Erforderlich. Ein Zeitdauerwert, der ein Intervall oder den Zeitraum für Indexer-Ausführungen angibt. Das kleinste zulässige Intervall beträgt fünf Minuten. Die längste Dauer ist ein Tag. Es muss als XSD-Wert „dayTimeDuration“ formatiert sein (eine eingeschränkte Teilmenge eines [ISO 8601-Zeitwerts](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration)). Das Muster sieht folgendermaßen aus: `"P[nD][T[nH][nM]]".` Beispiele: `PT15M` für ein Intervall von 15 Minuten, `PT2H` für ein Intervall von 2 Stunden.  

-   **startTime**: Optional. Ein UTC-datetime-Wert, der angibt, wann die Ausführung des Indexers beginnen soll.  

<a name="indexer-parameters"></a>

### <a name="parameters"></a>"parameters"

Ein Indexer kann optional Konfigurationsparameter verwenden, mit denen das Laufzeitverhalten geändert wird. Konfigurationsparameter werden in der Indexeranforderung durch Kommas getrennt. 

```json
    {
      "name" : "my-blob-indexer-for-cognitive-search",
      ... other indexer properties
      "parameters" : { "maxFailedItems" : "15", "batchSize" : "100", "configuration" : { "parsingMode" : "json", "indexedFileNameExtensions" : ".json, .jpg, .png", "imageAction" : "generateNormalizedImages", "dataToExtract" : "contentAndMetadata" } }
    }
```

#### <a name="general-parameters-for-all-indexers"></a>Allgemeine Parameter für alle Indexer

| Parameter | Typ und zulässige Werte   | Verwendung  |
|-----------|------------|--------------------------|--------|
| `"batchSize"` | Ganze Zahl <br/>Ist standardmäßig quellenspezifisch (1.000 für Azure SQL-Datenbank und Azure Cosmos DB, 10 für Azure Blob Storage) | Gibt die Anzahl von Elementen an, die aus einer Datenquelle gelesen und als einzelner Batch indiziert werden, um die Leistung zu verbessern. |
| `"maxFailedItems"` | Ganze Zahl <br/>Der Standardwert ist 0. | Anzahl von Fehlern, die toleriert werden, bevor eine Indexerausführung als Fehler angesehen wird. Legen Sie diese Option auf -1 fest, wenn Sie nicht möchten, dass der Indizierungsprozess durch Fehler beendet wird. Sie können Informationen zu fehlgeschlagenen Elementen abrufen, indem Sie den Vorgang zum [Abrufen des Indexerstatus](https://docs.microsoft.com/rest/api/searchservice/get-indexer-status) verwenden.  |
| `"maxFailedItemsPerBatch"` | Ganze Zahl <br/>Der Standardwert ist 0. | Anzahl von Fehlern, die pro Batch toleriert werden, bevor eine Indexerausführung als Fehler angesehen wird. Legen Sie diese Option auf -1 fest, wenn Sie nicht möchten, dass der Indizierungsprozess durch Fehler beendet wird. |

#### <a name="blob-configuration-parameters"></a>Blobkonfigurationsparameter

Mehrere Parameter gelten exklusiv für einen bestimmten Indexer, z.B. [Azure-Blobindizierung](search-howto-indexing-azure-blob-storage.md).

| Parameter | Typ und zulässige Werte   | Verwendung  |
|-----------|---------------------------|--------|
| `"parsingMode"` | Zeichenfolge<br/>`"text"`<br/>`"delimitedText"`<br/>`"json"`<br/>`"jsonArray"`  | Legen Sie diesen Parameter für [Azure-Blobs](search-howto-indexing-azure-blob-storage.md) auf `text` fest, um die Indizierungsleistung für Klartextdateien in Blobspeicher zu verbessern. <br/>Legen Sie diesen Parameter für [CSV-Blobs](search-howto-index-csv-blobs.md) auf `delimitedText` fest, wenn es sich bei Blobs um reine CSV-Dateien handelt. <br/>Legen Sie diesen Parameter für [JSON-Blobs](search-howto-index-json-blobs.md) auf `json` fest, um strukturierten Inhalt zu extrahieren, oder auf `jsonArray` (Vorschauversion), um einzelne Elemente eines Arrays in Azure Search als separate Dokumente zu extrahieren. |
| `"excludedFileNameExtensions"` | Zeichenfolge<br/>Liste mit Kommas als Trennzeichen | Ignorieren Sie für [Azure-Blobs](search-howto-indexing-azure-blob-storage.md) alle Dateitypen in der Liste. Beispielsweise können Sie „.png, .png, .mp4“ ausschließen, damit diese Dateien bei der Indizierung übersprungen werden. | 
| `"indexedFileNameExtensions"` | Zeichenfolge<br/>Liste mit Kommas als Trennzeichen | Wählen Sie für [Azure-Blobs](search-howto-indexing-azure-blob-storage.md) Blobs aus, wenn die Dateierweiterung in der Liste enthalten ist. Beispielsweise können Sie die Indizierung auf spezifische Anwendungsdateien wie „.docx, .pptx, .msg“ beschränken, um diese Dateitypen explizit einzubeziehen. | 
| `"failOnUnsupportedContentType"` | true <br/>false (Standard) | Legen Sie diesen Parameter für [Azure-Blobs](search-howto-indexing-azure-blob-storage.md) auf `false` fest, falls Sie die Indizierung fortsetzen möchten, wenn ein nicht unterstützter Inhaltstyp erkannt wird und Ihnen nicht alle Inhaltstypen (Dateierweiterungen) vorher bekannt sind. |
| `"failOnUnprocessableDocument"` | true <br/>false (Standard)| Legen Sie diesen Parameter für [Azure-Blobs](search-howto-indexing-azure-blob-storage.md) auf `false` fest, falls Sie die Indizierung fortsetzen möchten, wenn die Indizierung für ein Dokument fehlschlägt. |
| `"indexStorageMetadataOnlyForOversizedDocuments"` | true <br/>false (Standard)| Legen Sie diese Eigenschaft für [Azure-Blobs](search-howto-indexing-azure-blob-storage.md) auf `true` fest, um Speichermetadaten für Blobinhalt, der zu groß zum Verarbeiten ist, trotzdem zu indizieren.  Zu große Blobs werden standardmäßig als Fehler behandelt. Die Grenzwerte für die Blobgröße finden Sie unter [Grenzwerte für den Azure Search-Dienst](search-limits-quotas-capacity.md). |
| `"delimitedTextHeaders"` | Zeichenfolge<br/>Liste mit Kommas als Trennzeichen| Für [CSV-Blobs (Vorschauversion)](search-howto-index-csv-blobs.md) wird eine durch Kommas getrennte Liste mit Spaltenheadern angegeben, die hilfreich ist, um Quellfelder in einem Index Zielfeldern zuzuordnen. |
| `"delimitedTextDelimiter"` | Zeichenfolge<br/>Benutzerdefiniert | Für [CSV-Blobs (Vorschauversion)](search-howto-index-csv-blobs.md) wird das Trennzeichen für das Zeilenende für CSV-Dateien angegeben, wobei mit jeder Zeile ein neues Dokument gestartet wird (z.B. `"|"`).  |
| `"firstLineContainsHeaders"` | true (default) <br/>false | Für [CSV-Blobs (Vorschauversion)](search-howto-index-csv-blobs.md) wird angegeben, dass die erste (nicht leere) Zeile der einzelnen Blobs Header enthält.|
| `"documentRoot"`  | Zeichenfolge<br/>Benutzerdefiniert | Für [JSON-Arrays (Vorschauversion)](search-howto-index-json-blobs.md#nested-json-arrays) können Sie bei einem strukturierten oder halbstrukturierten Dokument einen Pfad zum Array angeben, indem Sie diese Eigenschaft verwenden. |
| `"dataToExtract"` | Zeichenfolge<br/>`"storageMetadata"`<br/>`"allMetadata"`<br/>`"contentAndMetadata"` (Standard) | Für [Azure-Blobs](search-howto-indexing-azure-blob-storage.md):<br/>Legen Sie diesen Parameter auf `"storageMetadata"` fest, um nur die [Standardblobeigenschaften und vom Benutzer angegebenen Metadaten](../storage/blobs/storage-properties-metadata.md) zu indizieren. <br/>Legen Sie diesen Parameter auf `"allMetadata"` fest, um Metadaten zu extrahieren, die vom Azure-Blobspeichersystem bereitgestellt werden, und die [für den Inhaltstyp spezifischen Metadaten](search-howto-indexing-azure-blob-storage.md#ContentSpecificMetadata) (z.B. Metadaten, die nur für PNG-Dateien eindeutig sind) zu indizieren. <br/>Legen Sie diesen Parameter auf `"contentAndMetadata"` fest, um alle Metadaten und Textinhalte aus jedem Blob zu extrahieren. <br/><br/>Wenn `"imageAction"` für die [Bildanalyse der kognitiven Suche (Vorschauversion)](cognitive-search-concept-image-scenarios.md) auf `"generateNormalizedImages"` festgelegt ist, weist die Einstellung `"dataToExtract"` den Indexer an, welche Daten aus dem Bildinhalt extrahiert werden sollen. Dies gilt für eingebetteten Bildinhalt in einer PDF-Datei oder eine andere Anwendung oder für Bilddateien, z.B. .jpg und .png, in Azure-Blobs.  |
| `"imageAction"` |  Zeichenfolge<br/>`"none"`<br/>`"generateNormalizedImages"` | Legen Sie diesen Parameter für [Azure-Blobs](search-howto-indexing-azure-blob-storage.md) auf `"none"` fest, um eingebettete Bilder oder Bilddateien im Dataset zu ignorieren. Dies ist die Standardoption. <br/><br/>Legen Sie diesen Parameter für die [Bildanalyse der kognitiven Suche](cognitive-search-concept-image-scenarios.md) auf `"generateNormalizedImages"` fest, um Text aus Bildern zu extrahieren (z.B. das Wort „Stop“ aus einem Stoppschild) und ihn als Teil des Inhaltsfelds einzubetten. Während der Bildanalyse erstellt der Indexer ein Array mit normalisierten Bildern als Teil des Vorgangs, mit dem Dokumente aufgebrochen werden, und bettet die generierten Informationen in das Inhaltsfeld ein. Für diese Aktion ist es erforderlich, `"dataToExtract"` auf `"contentAndMetadata"` festzulegen. Ein normalisiertes Bild bezieht sich auf eine zusätzliche Verarbeitung, die zu einer einheitlichen Bildausgabe führt. Für die Ausgabe wird die Größe angepasst, und sie wird gedreht, um das einheitliche Rendern zu fördern, wenn Sie Bilder in visuelle Suchergebnisse einbinden (z.B. Fotos gleicher Größe für ein Graphsteuerelement wie in der [JFK-Demo](https://github.com/Microsoft/AzureSearch_JFK_Files)). Diese Informationen werden für jedes Bild generiert. |


#### <a name="other-configuration-parameters"></a>Andere Konfigurationsparameter

Die folgenden Parameter gelten spezifisch für Azure SQL-Datenbank.

| Parameter | Typ und zulässige Werte   | Verwendung  |
|-----------|---------------------------|--------|
| `"queryTimeout"` | Zeichenfolge<br/>"hh:mm:ss"<br/>"00:05:00"| Legen Sie diesen Parameter für [Azure SQL-Datenbank](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md) fest, um für das Zeitlimit einen höheren Wert als den Standardwert von fünf Minuten anzugeben.|

<a name="field-mappings"></a>

### <a name="fieldmappings"></a>"fieldMappings"

Indexerdefinitionen enthalten Feldzuordnungen, um in einem Azure Search-Index ein Quellfeld einem Zielfeld zuzuordnen. Es gibt zwei Arten von Zuordnungen, je nachdem, ob die Inhaltsübertragung einem direkten oder einem erweiterten Pfad folgt:

+ **fieldMappings** sind optional und werden angewendet, wenn die Feldnamen für Quelle und Ziel nicht übereinstimmen oder wenn Sie eine Funktion angeben möchten.
+ **outputFieldMappings** sind erforderlich, wenn Sie eine [Anreicherungspipeline](cognitive-search-concept-intro.md) erstellen. In einer Anreicherungspipeline ist das Ausgabefeld ein Konstrukt, das während des Anreicherungsprozesses definiert wird. Das Ausgabefeld kann beispielsweise eine zusammengesetzte Struktur sein, die während der Anreicherung aus zwei separaten Feldern im Quelldokument erstellt wird. 

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

<a name="output-fieldmappings"></a>

### <a name="outputfieldmappings"></a>"outputFieldMappings"

Bei Szenarien mit [kognitiver Suche](cognitive-search-concept-intro.md), bei denen ein Skillset an einen Indexer gebunden ist, müssen Sie `outputFieldMappings` hinzufügen, um jede Ausgabe eines Anreicherungsschritts zuzuordnen, mit dem Inhalt für ein durchsuchbares Feld im Index bereitgestellt wird.

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

### <a name="field-mapping-functions"></a>Feldzuordnungsfunktionen

Feldzuordnungen können auch verwendet werden, um Quellfeldwerte mithilfe von *Feldzuordnungsfunktionen*umzuwandeln. Ein beliebiger Zeichenfolgenwert kann beispielsweise Base64-codiert sein, damit er zum Auffüllen eines Dokumentschlüsselfelds verwendet werden kann.

Informationen dazu, wann und wie Sie Feldzuordnungsfunktionen verwenden, finden Sie unter [Feldzuordnungsfunktionen](https://docs.microsoft.com/azure/search/search-indexer-field-mappings#field-mapping-functions).

## <a name="request-examples"></a>Beispiele für Anforderungen  
 Im ersten Beispiel wird ein Indexer erstellt, der Daten aus der Tabelle, die von der `ordersds`-Datenquelle referenziert wird, in den `orders`-Index kopiert. Der Vorgang erfolgt nach einem Zeitplan, der am 1. Januar 2015 UTC beginnt und stündlich ausgeführt wird. Die Aufrufe des Indexers sind erfolgreich, wenn nicht mehr als fünf zu indizierende Elemente in jedem Batch fehlschlagen und insgesamt nicht mehr als zehn zu indizierende Elemente fehlschlagen.  

```json
{
    "name" : "myindexer",  
    "description" : "a cool indexer",  
    "dataSourceName" : "ordersds",  
    "targetIndexName" : "orders",  
    "schedule" : { "interval" : "PT1H", "startTime" : "2018-01-01T00:00:00Z" },  
    "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 5 }  
}
```

Im zweiten Beispiel wird ein Vorgang der kognitiven Suche veranschaulicht, der durch den Verweis auf ein Skillset und [outputFieldMappings](#output-fieldmappings) angegeben ist. [Skillsets](ref-create-skillset.md) sind allgemeine Ressourcen, die separat definiert werden. Dieses Beispiel ist eine Abkürzung der Indexerdefinition, die im [Tutorial zur kognitiven Suche](cognitive-search-tutorial-blob.md) enthalten ist.

```json
{
  "name":"demoindexer", 
  "dataSourceName" : "demodata",
  "targetIndexName" : "demoindex",
  "skillsetName" : "demoskillset",
  "fieldMappings" : [
    {
        "sourceFieldName" : "content",
        "targetFieldName" : "content"
    }
   ],
  "outputFieldMappings" : 
  [
    {
        "sourceFieldName" : "/document/organizations", 
        "targetFieldName" : "organizations"
    },
  ],
  "parameters":
  {
    "maxFailedItems":-1,
    "configuration": 
    {
    "dataToExtract": "contentAndMetadata",
    "imageAction": "generateNormalizedImages"
    }
  }
}
```

## <a name="response"></a>response  
 Bei erfolgreicher Anforderung: "201 Erstellt".  

## <a name="see-also"></a>Weitere Informationen

+ [Indexer (Übersicht)](search-indexer-overview.md)
+ [Übersicht über die kognitive Suche](cognitive-search-concept-intro.md)
+ [Schnellstart: Testen der kognitiven Suche](cognitive-search-quickstart-blob.md)
+ [Zuordnen von Feldern](cognitive-search-output-field-mapping.md)
