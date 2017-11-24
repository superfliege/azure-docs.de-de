---
title: Indizierung der JSON-Blobs mit Azure Search-Blobindexer
description: Indizierung der JSON-Blobs mit Azure Search-Blobindexer
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: 57e32e51-9286-46da-9d59-31884650ba99
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 09/07/2017
ms.author: eugenesh
ms.openlocfilehash: 2dac2c5980970946a6b9c26ee6ee8ac0f0344144
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2017
---
# <a name="indexing-json-blobs-with-azure-search-blob-indexer"></a>Indizierung der JSON-Blobs mit Azure Search-Blobindexer
In diesem Artikel wird beschrieben, wie Sie einen Azure Search-Blobindexer konfigurieren, um strukturierten Inhalt aus JSON-Blobs in Azure Blob Storage zu extrahieren.

JSON-Blobs in Azure Blob Storage bestehen normalerweise entweder aus einem einzelnen JSON-Dokument oder einem JSON-Array. Mit dem Blobindexer in Azure Search können beide Fälle analysiert werden. Dies richtet sich danach, wie Sie den Parameter **parsingMode** in der Anforderung festlegen.

| JSON-Dokument | parsingMode | Beschreibung | Verfügbarkeit |
|--------------|-------------|--------------|--------------|
| Ein Dokument pro Blob | `json` | JSON-Blobs werden als einzelne Textblöcke analysiert. Jedes JSON-Blob wird zu einem einzelnen Azure Search-Dokument. | Allgemein verfügbar in [REST](https://docs.microsoft.com/rest/api/searchservice/indexer-operations)- und [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer)-APIs. |
| Mehrere Dokumente pro Blob | `jsonArray` | Analysiert ein JSON-Array im Blob, und jedes Element des Arrays wird zu einem separaten Azure Search-Dokument.  | Befindet sich für [REST api-version=`2016-09-01-Preview`](search-api-2016-09-01-preview.md) und [.NET SDK Preview](https://aka.ms/search-sdk-preview) in der Vorschauphase. |

> [!Note]
> Vorschau-APIs sind für Tests und Evaluierungen bestimmt und sollten nicht in Produktionsumgebungen eingesetzt werden.
>

## <a name="setting-up-json-indexing"></a>Einrichten der JSON-Indizierung
Die Indizierung von JSON-Blobs ähnelt der regulären Dokumentextraktion in einem dreiteiligen Workflow, der für alle Indexer in Azure Search gleich ist.

### <a name="step-1-create-a-data-source"></a>Schritt 1: Erstellen einer Datenquelle

Der erste Schritt umfasst die Bereitstellung der Datenquellen-Verbindungsinformationen, die vom Indexer verwendet werden. Mit dem Datenquellentyp, der hier als `azureblob` angegeben ist, wird bestimmt, welches Datenextraktionsverhalten vom Indexer aufgerufen wird. Bei der JSON-Blobindizierung ist die Datenquellendefinition für JSON-Dokumente und -Arrays gleich. 

    POST https://[service name].search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "optional, my-folder" }
    }   

### <a name="step-2-create-a-target-search-index"></a>Schritt 2: Erstellen eines Zielsuchindex 

Indexer sind mit einem Indexschema gekoppelt. Bereiten Sie bei Verwendung der API (anstelle des Portals) vorher einen Index vor, damit Sie ihn für den Indexervorgang angeben können. 

> [!Note]
> Indexer werden im Portal verfügbar gemacht, indem die Aktion **Import** für eine begrenzte Anzahl von allgemein verfügbaren Indexern verwendet wird. Mit dem Importworkflow kann basierend auf den Metadaten der Quelle häufig ein vorläufiger Index erstellt werden. Weitere Informationen finden Sie unter [Importieren von Daten in Azure Search über das Portal](search-import-data-portal.md).

### <a name="step-3-configure-and-run-the-indexer"></a>Schritt 3: Konfigurieren und Ausführen des Indexers

Bisher waren Definitionen für die Datenquelle und den Index parsingMode-agnostisch. Aber in Schritt 3 der Indexerkonfiguration teilt sich die Vorgehensweise auf. Der weitere Weg hängt davon ab, wie der JSON-Blobinhalt analysiert und in einem Azure Search-Index strukturiert werden soll.

Gehen Sie beim Aufrufen des Indexers wie folgt vor:

+ Legen Sie den Parameter **parsingMode** auf `json` (zum Indizieren jedes Blob als einzelnes Dokument) oder `jsonArray` (wenn die Blobs JSON-Arrays enthalten und jedes Element eines Arrays als separates Dokument behandelt werden soll) fest.

+ Verwenden Sie optional **Feldzuordnungen**, um auszuwählen, welche Eigenschaften des JSON-Quelldokuments zum Auffüllen Ihres Zielsuchindex verwendet werden sollen. Für JSON-Arrays gilt Folgendes: Wenn das Array als Eigenschaft auf niedrigerer Ebene vorhanden ist, können Sie einen Dokumentstamm festlegen, mit dem angegeben wird, wo das Array im Blob angeordnet ist.

> [!IMPORTANT]
> Bei Verwendung der Modi `json` oder `jsonArray` geht Azure Search davon aus, dass alle Blobs in Ihrer Datenquelle JSON enthalten. Wenn Sie eine Kombination von JSON- und Nicht-JSON-Blobs in der gleichen Datenquelle unterstützen müssen, informieren Sie uns auf unserer [UserVoice-Website](https://feedback.azure.com/forums/263029-azure-search).


## <a name="how-to-parse-single-json-blobs"></a>Analysieren von einzelnen JSON-Blobs

In der Standardeinstellung analysiert der [Azure Search-Blob-Indexer](search-howto-indexing-azure-blob-storage.md) JSON-Blobs als einen einzelnen Textblock. Häufig möchten Sie die Struktur Ihrer JSON-Dokumente beibehalten. Angenommen, Sie verwenden in Azure Blob Storage das folgende JSON-Dokument:

    {
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13"
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

### <a name="indexer-definition-for-single-json-blobs"></a>Indexerdefinition für einzelne JSON-Blobs

Mit dem Azure Search-Blobindexer wird ein JSON-Dokument, das dem Dokument im obigen Beispiel ähnelt, in ein einzelnes Azure Search-Dokument analysiert. Der Indexer lädt einen Index, indem die Elemente „text“, „datePublished“ und „tags“ aus der Quelle mit Zielfeldern abgeglichen werden, die den gleichen Namen und Typ aufweisen.

Die Konfiguration wird im Text eines Indexervorgangs bereitgestellt. Wir wissen, dass mit dem zuvor definierten Datenquellenobjekt der Datenquellentyp und die Verbindungsinformationen angegeben werden. Außerdem muss der Zielindex als leerer Container in Ihrem Dienst vorhanden sein. Zeitplan und Parameter sind optional. Aber wenn Sie diese Angaben weglassen, wird der Indexer sofort ausgeführt, indem `json` als Analysemodus verwendet wird.

Eine vollständige Anforderung kann beispielsweise wie folgt aussehen:

    POST https://[service name].search.windows.net/indexers?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "json" } }
    }

Wie schon erwähnt, sind Feldzuordnungen nicht erforderlich. Bei einem Index mit den Feldern „text“, „datePublished“ und „tags“ kann der Blobindexer die richtige Zuordnung ableiten, ohne dass in der Anforderung eine Feldzuordnung enthalten ist.

## <a name="how-to-parse-json-arrays-preview"></a>Analysieren von JSON-Arrays (Vorschauversion)

Alternativ dazu können Sie auch das Vorschaufeature mit dem JSON-Array wählen. Diese Funktion ist nützlich, wenn Blobs ein *Array mit JSON-Objekten* enthalten und jedes Element zu einem separaten Azure Search-Dokument werden soll. Wenn beispielsweise das folgende JSON-Blob vorhanden ist, können Sie Ihren Azure Search-Index mit drei separaten Dokumenten auffüllen, die jeweils die Felder „id“ und „text“ aufweisen.  

    [
        { "id" : "1", "text" : "example 1" },
        { "id" : "2", "text" : "example 2" },
        { "id" : "3", "text" : "example 3" }
    ]

### <a name="indexer-definition-for-a-json-array"></a>Indexerdefinition für ein JSON-Array

Für ein JSON-Array nutzt die Indexeranforderung die Vorschau-API und den Parser `jsonArray`. Dies sind die beiden einzigen Array-spezifischen Anforderungen zum Indizieren von JSON-Blobs.

    POST https://[service name].search.windows.net/indexers?api-version=2016-09-01-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "jsonArray" } }
    }

Beachten Sie auch hierbei wieder, dass keine Feldzuordnungen erforderlich sind. Bei einem Index mit den Feldern „id“ und „text“ kann der Blobindexer die richtige Zuordnung ohne Feldzuordnungsliste ableiten.

### <a name="nested-json-arrays"></a>Geschachtelte JSON-Arrays
Was geschieht, wenn Sie ein Array aus JSON-Objekten indizieren möchten, dieses Array aber irgendwo im Dokument geschachtelt ist? Sie können mithilfe der Konfigurationseigenschaft `documentRoot` auswählen, welche Eigenschaft das Array enthält. Angenommen, Ihre Blobs sehen folgendermaßen aus:

    {
        "level1" : {
            "level2" : [
                { "id" : "1", "text" : "Use the documentRoot property" },
                { "id" : "2", "text" : "to pluck the array you want to index" },
                { "id" : "3", "text" : "even if it's nested inside the document" }  
            ]
        }
    }

Verwenden Sie diese Konfiguration, um das in der `level2`-Eigenschaft enthaltene Array zu indizieren:

    {
        "name" : "my-json-array-indexer",
        ... other indexer properties
        "parameters" : { "configuration" : { "parsingMode" : "jsonArray", "documentRoot" : "/level1/level2" } }
    }

## <a name="using-field-mappings-to-build-search-documents"></a>Verwenden von Feldzuordnungen zum Erstellen von Suchdokumenten

Wenn Quell- und Zielfelder nicht genau aneinander ausgerichtet sind, können Sie im Anforderungstext einen Abschnitt für die Feldzuordnung definieren, um explizite Feld-zu-Feld-Zuordnungen zu ermöglichen.

Derzeit kann Azure Search JSON-Dokumente nicht willkürlich direkt indizieren, da nur primitive Datentypen, Zeichenfolgenarrays und GeoJSON-Punkte unterstützt werden. Sie können jedoch mit **Feldzuordnungen** Teile des JSON-Dokuments auswählen und diese in die Felder der obersten Ebene des Suchdokuments „heben“. Informationen zu den Grundlagen von Feldzuordnungen finden Sie unter [Feldzuordnungen in Azure Search-Indexern](search-indexer-field-mappings.md).

Wir verwenden wieder unser JSON-Beispieldokument:

    {
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13"
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

Angenommen, Sie haben einen Suchindex mit den folgenden Feldern: `text` vom Typ `Edm.String`, `date` vom Typ `Edm.DateTimeOffset` und `tags` vom Typ `Collection(Edm.String)`. Beachten Sie die Abweichung zwischen „datePublished“ in der Quelle und dem Feld `date` im Index. Um das JSON-Objekt in der gewünschten Form zuzuordnen, verwenden Sie die folgenden Feldzuordnungen:

    "fieldMappings" : [
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
      ]

Die Quellenfeldnamen in den Zuordnungen werden mit der [JSON-Zeiger](http://tools.ietf.org/html/rfc6901) -Notation angegeben. Sie beginnen mit einem Schrägstrich, um auf das Stammverzeichnis des JSON-Dokuments zu verweisen, und wählen dann mittels eines schrägstrichgetrennten Weiterleitungspfads den Pfad zur gewünschten Eigenschaft (auf beliebiger Schachtelungsebene) aus.

Sie können auch mit einem nullbasierten Index auf einzelne Arrayelemente verweisen. Um z. B. das erste Element des Arrays „tags“ aus dem obigen Beispiel auszuwählen, verwenden Sie eine Feldzuordnung wie folgt:

    { "sourceFieldName" : "/article/tags/0", "targetFieldName" : "firstTag" }

> [!NOTE]
> Wenn ein Quellfeldname in einer Feldzuordnung auf eine Eigenschaft verweist, die nicht in JSON vorhanden ist, wird die Zuordnung ohne Fehler übersprungen. Dies geschieht, damit Dokumente mit einem anderen Schema unterstützt werden können (ein häufiger Anwendungsfall). Da keine Überprüfung erfolgt, müssen Sie darauf achten, Tippfehler in Ihrer Feldzuordnungspezifikation zu vermeiden.
>
>

## <a name="example-indexer-request-with-field-mappings"></a>Beispiel: Indexeranforderung mit Feldzuordnungen

Das folgende Beispiel enthält eine vollständig angegebene Indexernutzlast mit Feldzuordnungen:

    POST https://[service name].search.windows.net/indexers?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "json" } },
      "fieldMappings" : [
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
        ]
    }


## <a name="help-us-make-azure-search-better"></a>Helfen Sie uns bei der Verbesserung von Azure Search
Teilen Sie uns auf unserer [UserVoice-Website](https://feedback.azure.com/forums/263029-azure-search/) mit, wenn Sie sich Features wünschen oder Verbesserungsvorschläge haben.

## <a name="see-also"></a>Weitere Informationen

+ [Indexer in Azure Search](search-indexer-overview.md)
+ [Indizieren von Azure Blob Storage mit Azure Search](search-howto-index-json-blobs.md)
+ [Indizierung von CSV-Blobs mit Azure Search-Blobindexer](search-howto-index-csv-blobs.md)
+ [Tutorial: Durchsuchen von teilweise strukturierten Daten in Cloudspeicher](search-semi-structured-data.md)