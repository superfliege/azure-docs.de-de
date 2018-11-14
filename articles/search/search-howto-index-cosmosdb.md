---
title: Indizieren einer Azure Cosmos DB-Datenquelle für Azure Search | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie einen Azure Search-Indexer mit einer Azure Cosmos DB-Datenquelle erstellen.
ms.date: 10/17/2018
author: mgottein
manager: cgronlun
ms.author: magottei
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
robot: noindex
ms.openlocfilehash: 07768ee1590fa087a1eb1486cb59ab0f57d02b64
ms.sourcegitcommit: 6678e16c4b273acd3eaf45af310de77090137fa1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/01/2018
ms.locfileid: "50747540"
---
# <a name="connecting-cosmos-db-with-azure-search-using-indexers"></a>Verbinden von Cosmos DB mit Azure Search mithilfe von Indexern

In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
> * Konfigurieren Sie den [Azure Search-Indexer](search-indexer-overview.md), der eine Azure Cosmos DB-Sammlung als Datenquelle verwendet.
> * Erstellen Sie einen Suchindex mit JSON-kompatiblen Datentypen.
> * Konfigurieren Sie einen Indexer für eine bedarfsgesteuerte und wiederholte Indizierung.
> * Aktualisieren Sie den Index schrittweise basierend auf Änderungen an den zugrunde liegenden Daten.

> [!NOTE]
> Azure Cosmos DB ist die nächste Generation von DocumentDB. Obwohl der Produktname geändert wurde, ist die `documentdb`-Syntax in den Azure Search-Indexern immer noch aus Gründen der Abwärtskompatibilität sowohl in den Azure Search-APIs als auch auf den Portalseiten vorhanden. Wenn Sie Indexer konfigurieren, stellen Sie sicher, dass Sie die `documentdb`-Syntax wie in diesem Artikel beschrieben angeben.

Im folgenden Video veranschaulicht Azure Cosmos DB-Programm-Manager Andrew Liu das Hinzufügen eines Azure Search-Indexes zu einem Azure Cosmos DB-Container.

>[!VIDEO https://www.youtube.com/embed/OyoYu1Wzk4w]

<a name="supportedAPIs"></a>
## <a name="supported-api-types"></a>Unterstützte API-Typen

Obwohl Azure Cosmos DB eine Vielzahl von Datenmodellen und APIs unterstützt, erstreckt sich die Produktionsunterstützung des Azure Search-Indexers nur auf die SQL-API. Die Unterstützung der MongoDB-API ist derzeit als öffentliche Vorschau verfügbar.  

Unterstützung für zusätzliche APIs ist in Kürze verfügbar. Um uns zu helfen, Prioritäten zu setzen, welche zuerst unterstützt werden sollen, stimmen Sie bitte auf der User Voice-Website ab:

* [Unterstützung für Tabellen-API-Datenquellen](https://feedback.azure.com/forums/263029-azure-search/suggestions/32759746-azure-search-should-be-able-to-index-cosmos-db-tab)
* [Unterstützung für Graph-API-Datenquellen](https://feedback.azure.com/forums/263029-azure-search/suggestions/13285011-add-graph-databases-to-your-data-sources-eg-neo4)
* [Unterstützung für Apache Cassandra-API-Datenquellen](https://feedback.azure.com/forums/263029-azure-search/suggestions/32857525-indexer-crawler-for-apache-cassandra-api-in-azu)

## <a name="prerequisites"></a>Voraussetzungen

Zusätzlich zu einem Cosmos DB-Konto benötigen Sie einen [Azure Search-Dienst](search-create-service-portal.md). 

In Ihrem Cosmos DB-Konto können Sie auswählen, ob die Sammlung automatisch alle Dokumente indizieren soll. Standardmäßig werden alle Dokumente automatisch indiziert, aber Sie können die automatische Indizierung deaktivieren. Wenn die Indizierung deaktiviert ist, kann auf die Dokumente nur über ihre Self-Links oder über Abfragen mithilfe der Dokument-ID zugegriffen werden. Für Azure Search muss die automatische Cosmos DB-Indizierung für die Sammlung aktiviert sein, die von Azure Search indiziert wird. 

<a name="Concepts"></a>
## <a name="azure-search-indexer-concepts"></a>Azure Search Indexer-Konzepte

Eine **Datenquelle** gibt die zu indizierenden Daten, Anmeldeinformationen und Richtlinien für das Bestimmen von Änderungen in den Daten an (z.B. geänderte oder gelöschte Dokumente in Ihrer Sammlung). Die Datenquelle wird als unabhängige Ressource definiert, sodass sie von mehreren Indexern verwendet werden kann.

Ein **Indexer** beschreibt, wie die Daten von der Datenquelle in einen Zielsuchindex fließen. Ein Indexer ermöglicht folgende Vorgänge:

* Eine einmalige Kopie der Daten zum Auffüllen eines Indexes ausführen.
* Einen Index mit Änderungen an der Datenquelle nach einem Zeitplan synchronisieren.
* Bedarfs-Updates für einen Index je nach Notwendigkeit abrufen.

Zum Einrichten eines Azure Cosmos DB-Indexers benötigen Sie einen Index, eine Datenquelle und schließlich den Indexer. Sie können diese Objekte mit dem [Portal](search-import-data-portal.md), dem [.NET SDK](/dotnet/api/microsoft.azure.search) oder der [REST-API](/rest/api/searchservice/) erstellen. 

Dieser Artikel beschreibt die Verwendung der REST-API. Wenn Sie sich für das Portal entscheiden, führt Sie der [Datenimport-Assistent](search-import-data-portal.md) durch die Erstellung aller dieser Ressourcen samt Index.

> [!TIP]
> Starten Sie über das Azure Cosmos DB-Dashboard den **Datenimport-Assistenten**, um die Indizierung für diese Datenquelle zu vereinfachen. Navigieren Sie im linken Navigationsbereich zu **Sammlungen** > **Azure Search hinzufügen**, um mit dem Vorgang zu beginnen.

> [!NOTE] 
> Derzeit können Sie die **MongoDB**-Datenquellen mit dem Azure-Portal oder der .NET SDK erstellen oder bearbeiten. Allerdings **können** Sie den Ausführungsverlauf des MongoDB-Indexers im Portal überwachen.  

<a name="CreateDataSource"></a>
## <a name="step-1-create-a-data-source"></a>Schritt 1: Erstellen einer Datenquelle
Führen Sie einen POST aus, um eine Datenquelle zu erstellen:

    POST https://[service name].search.windows.net/datasources?api-version=2017-11-11
    Content-Type: application/json
    api-key: [Search service admin key]

    {
        "name": "mydocdbdatasource",
        "type": "documentdb",
        "credentials": {
            "connectionString": "AccountEndpoint=https://myCosmosDbEndpoint.documents.azure.com;AccountKey=myCosmosDbAuthKey;Database=myCosmosDbDatabaseId"
        },
        "container": { "name": "myCollection", "query": null },
        "dataChangeDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName": "_ts"
        }
    }

Der Anforderungstext umfasst die Datenquellendefinition, welche die folgenden Felder enthalten sollte:

* **Name**: Wählen Sie einen beliebigen Namen für Ihre Datenbank.
* **type**: Muss `documentdb` lauten.
* **Anmeldeinformationen**:
  
  * **ConnectionString**: Erforderlich. Geben Sie die Verbindungsinformationen zu Ihrer Azure Cosmos DB-Datenbank im folgenden Format an: `AccountEndpoint=<Cosmos DB endpoint url>;AccountKey=<Cosmos DB auth key>;Database=<Cosmos DB database id>`. Fügen Sie für MongoDB-Sammlungen **ApiKind=MongoDb** der Verbindungszeichenfolge hinzu: `AccountEndpoint=<Cosmos DB endpoint url>;AccountKey=<Cosmos DB auth key>;Database=<Cosmos DB database id>;ApiKind=MongoDb`.
  Vermeiden Sie Portnummern in der Endpunkt-URL. Wenn Sie die Portnummer einfügen, kann Azure Search Ihre Azure Cosmos DB-Datenbank nicht indizieren.
* **Container**:
  
  * **Name**: Erforderlich. Geben Sie die ID der zu indizierenden Datenbanksammlung an.
  * **Abfrage**: Optional. Sie können eine Abfrage spezifizieren, um ein beliebiges JSON-Dokument in ein Flatfile-Schema zu reduzieren, welches Azure Search indizieren kann. Abfragen werden für MongoDB-Sammlungen nicht unterstützt. 
* **dataChangeDetectionPolicy**: Empfohlen. Weitere Informationen finden Sie im Abschnitt [Indizieren von geänderten Dokumenten](#DataChangeDetectionPolicy).
* **dataDeletionDetectionPolicy**: Optional. Weitere Informationen finden Sie im Abschnitt [Indizieren von gelöschten Dokumenten](#DataDeletionDetectionPolicy).

### <a name="using-queries-to-shape-indexed-data"></a>Verwenden von Abfragen zum Formen indizierter Daten
Sie können eine SQL-Abfrage angeben, um geschachtelte Eigenschaften oder Arrays zu vereinfachen, JSON-Eigenschaften zu projizieren und die zu indizierenden Daten zu filtern. 

> [!WARNING]
> Benutzerdefinierte Abfragen werden für **MongoDB**-Sammlungen nicht unterstützt: Der `container.query`-Parameter muss auf NULL festgelegt oder nicht angegeben werden. Wenn Sie eine benutzerdefinierte Abfrage verwenden möchten, informieren Sie uns auf [User Voice](https://feedback.azure.com/forums/263029-azure-search).

Beispieldokument:

    {
        "userId": 10001,
        "contact": {
            "firstName": "andy",
            "lastName": "hoh"
        },
        "company": "microsoft",
        "tags": ["azure", "documentdb", "search"]
    }

Abfrage zur Filterung:

    SELECT * FROM c WHERE c.company = "microsoft" and c._ts >= @HighWaterMark ORDER BY c._ts

Vereinfachen der Abfrage:

    SELECT c.id, c.userId, c.contact.firstName, c.contact.lastName, c.company, c._ts FROM c WHERE c._ts >= @HighWaterMark ORDER BY c._ts
    
    
Abfrage zur Projektion:

    SELECT VALUE { "id":c.id, "Name":c.contact.firstName, "Company":c.company, "_ts":c._ts } FROM c WHERE c._ts >= @HighWaterMark ORDER BY c._ts


Abfrage zum Vereinfachen eines Arrays:

    SELECT c.id, c.userId, tag, c._ts FROM c JOIN tag IN c.tags WHERE c._ts >= @HighWaterMark ORDER BY c._ts

<a name="CreateIndex"></a>
## <a name="step-2-create-an-index"></a>Schritt 2: Erstellen eines Index
Erstellen Sie einen Azure Search-Zielindex, wenn Sie bislang noch über keinen verfügen. Sie können einen Index über die [Benutzeroberfläche des Azure-Portals](search-create-index-portal.md), die [REST-API zum Erstellen von Indizes](/rest/api/searchservice/create-index) und die [Index-Klasse](/dotnet/api/microsoft.azure.search.models.index) erstellen.

Anhand des folgenden Beispiels wird ein Index mit einer ID und einem Beschreibungsfeld erstellt:

    POST https://[service name].search.windows.net/indexes?api-version=2017-11-11
    Content-Type: application/json
    api-key: [Search service admin key]

    {
       "name": "mysearchindex",
       "fields": [{
         "name": "id",
         "type": "Edm.String",
         "key": true,
         "searchable": false
       }, {
         "name": "description",
         "type": "Edm.String",
         "filterable": false,
         "sortable": false,
         "facetable": false,
         "suggestions": true
       }]
     }

Stellen Sie sicher, dass das Schema des Ziel-Indexes mit dem Schema der JSON-Quelldokumente oder mit der Ausgabe Ihrer benutzerdefinierten Abfrageprojektion kompatibel ist.

> [!NOTE]
> Für partitionierte Sammlungen ist der Standarddokumentschlüssel die Eigenschaft `_rid` von Azure Cosmos DB, die Azure Search automatisch in `rid` umbenennt, da Feldnamen nicht mit einem Unterstrich beginnen dürfen. Darüber hinaus enthalten die `_rid`-Werte von Azure Cosmos DB Zeichen, die in Azure Search-Schlüsseln ungültig sind. Deshalb sind die `_rid`-Werte Base64-codiert.
> 
> Für MongoDB-Sammlungen benennt Azure Search die `_id`-Eigenschaft automatisch in `doc_id` um.  

### <a name="mapping-between-json-data-types-and-azure-search-data-types"></a>Zuordnung zwischen JSON-Datentypen und Azure Search-Datentypen
| JSON-Datentyp | Kompatible Feldtypen im Zielindex |
| --- | --- |
| Bool |Edm.Boolean, Edm.String |
| Zahlen, die wie Ganzzahlen aussehen |Edm.Int32, Edm.Int64, Edm.String |
| Zahlen, die wie Gleitkommas aussehen |Edm.Double, Edm.String |
| Zeichenfolge |Edm.String |
| Arrays primitiver Typen, z.B. ["a", "b", "c"] |Collection(Edm.String) |
| Zeichenfolgen, die wie Datumsangaben aussehen |Edm.DateTimeOffset, Edm.String |
| GeoJSON-Objekte, z.B. { "type": "Point", "coordinates": [long, lat] } |Edm.GeographyPoint |
| Andere JSON-Objekte |N/V |

<a name="CreateIndexer"></a>

## <a name="step-3-create-an-indexer"></a>Schritt 3: Erstellen eines Indexers

Nach der Erstellung von Index und Datenquelle können Sie den Indexer erstellen:

    POST https://[service name].search.windows.net/indexers?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "mydocdbindexer",
      "dataSourceName" : "mydocdbdatasource",
      "targetIndexName" : "mysearchindex",
      "schedule" : { "interval" : "PT2H" }
    }

Dieser Indexer wird alle zwei Stunden ausgeführt (das Planungsintervall ist auf „PT2H“ festgelegt). Um einen Indexer alle 30 Minuten auszuführen, legen Sie das Intervall auf „PT30M“ fest. Das kürzeste unterstützte Intervall beträgt fünf Minuten. Der Zeitplan ist optional. Ohne Zeitplan wird ein Indexer nur einmal bei seiner Erstellung ausgeführt. Allerdings können Sie ein Indexer bei Bedarf jederzeit ausführen.   

Weitere Informationen zur API zum Erstellen eines Indexers finden Sie unter [Erstellen eines Indexers](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

<a id="RunIndexer"></a>
### <a name="running-indexer-on-demand"></a>Ausführen des Indexers nach Bedarf
Zusätzlich zur Ausführung in regelmäßigen Abständen nach einem Zeitplan kann ein Indexer auch nach Bedarf ausgeführt werden:

    POST https://[service name].search.windows.net/indexers/[indexer name]/run?api-version=2017-11-11
    api-key: [Search service admin key]

> [!NOTE]
> Wenn die Ausführungs-API erfolgreich ausgeführt wurde, wurde der Indexeraufruf geplant, die eigentliche Verarbeitung erfolgt jedoch asynchron. 

Sie können den indexerstatus im Portal oder mithilfe der im Folgenden beschriebenen API zum Abrufen des Indexer-Status überwachen. 

<a name="GetIndexerStatus"></a>
### <a name="getting-indexer-status"></a>Abrufen des Indexer-Status
Sie können den Status und den Ausführungsverlauf eines Indexers abrufen:

    GET https://[service name].search.windows.net/indexers/[indexer name]/status?api-version=2017-11-11
    api-key: [Search service admin key]

Die Antwort enthält den Gesamtstatus des Indexers, den letzten (oder laufenden) Aufruf des Indexers sowie den Verlauf der letzten Indexeraufrufe.

    {
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
         },
        "executionHistory":[ {
            "status":"success",
             "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
        }]
    }

Der Ausführungsverlauf enthält bis zu 50 der jüngsten abgeschlossenen Ausführungen. Diese sind in umgekehrter chronologischer Reihenfolge sortiert (somit ist die neueste Ausführung als Erstes in der Antwort aufgelistet).

<a name="DataChangeDetectionPolicy"></a>
## <a name="indexing-changed-documents"></a>Indizieren von geänderten Dokumenten
Die Richtlinie zum Erkennen von Datenänderungen dient einer effizienten Identifizierung geänderter Datenelemente. Derzeit ist die einzige unterstützte Richtlinie die `High Water Mark`-Richtlinie, die die `_ts`-Eigenschaft (Zeitstempel) verwendet, die von Azure Cosmos DB bereitgestellt wird. Diese wird wie folgt angegeben:

    {
        "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
        "highWaterMarkColumnName" : "_ts"
    }

Wenn Sie diese Richtlinie verwenden, wird dringend empfohlen sicherzustellen, dass der Indexer eine hohe Leistung erzielt. 

Wenn Sie eine benutzerdefinierte Abfrage verwenden, stellen sicher, dass die `_ts`Eigenschaft von der Abfrage projiziert wird.

<a name="IncrementalProgress"></a>
### <a name="incremental-progress-and-custom-queries"></a>Inkrementeller Status und benutzerdefinierte Abfragen
Der inkrementelle Status während der Indizierung stellt sicher, dass der Indexer bei der nächsten Ausführung an der gleichen Stelle fortgesetzt werden kann und nicht die gesamte Sammlung von vorn indizieren muss, wenn er durch vorübergehende Fehler oder das Erreichen des Ausführungszeitlimits unterbrochen wird. Dies ist besonders beim Indizieren umfangreicher Sammlungen wichtig. 

Stellen Sie zum Aktivieren des inkrementellen Status bei Verwendung einer benutzerdefinierten Abfrage sicher, dass die Abfrage die Ergebnisse nach der Spalte `_ts` sortiert. Dadurch können regelmäßige-Prüfpunkte erstellt werden, die von Azure Search im Falle eines Fehlers zur Bereitstellung des inkrementellen Status verwendet werden.   

In bestimmten Fällen kann Azure Search unter Umständen nicht erkennen, dass die Abfrage nach `_ts` sortiert ist. (Dies kann sogar vorkommen, wenn die Abfrage eine `ORDER BY [collection alias]._ts`-Klausel enthält.) Mithilfe der Konfigurationseigenschaft `assumeOrderByHighWaterMarkColumn` können Sie Azure Search mitteilen, dass die Ergebnisse sortiert sind. Erstellen oder aktualisieren Sie Ihren Indexer wie folgt, um diesen Hinweis anzugeben: 

    {
     ... other indexer definition properties
     "parameters" : {
            "configuration" : { "assumeOrderByHighWaterMarkColumn" : true } }
    } 

<a name="DataDeletionDetectionPolicy"></a>
## <a name="indexing-deleted-documents"></a>Indizieren von gelöschten Dokumenten
Wenn Zeilen aus der Quelltabelle gelöscht werden, möchten Sie diese Zeilen in der Regel auch aus dem Suchindex löschen. Die Richtlinie zum Erkennen von Datenlöschungen dient einer effizienten Identifizierung gelöschter Datenelemente. Zurzeit ist `Soft Delete` die einzige unterstützte Richtlinie (die Löschung wird durch ein bestimmtes Kennzeichen markiert). Diese wird folgendermaßen festgelegt:

    {
        "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
        "softDeleteColumnName" : "the property that specifies whether a document was deleted",
        "softDeleteMarkerValue" : "the value that identifies a document as deleted"
    }

Wenn Sie eine benutzerdefinierte Abfrage verwenden, stellen Sie sicher, dass die Eigenschaft, auf die `softDeleteColumnName` verweist, von der Abfrage projiziert wird.

Im folgenden Beispiel wird eine Datenquelle mit einer Richtlinie zum vorläufigen Löschen erstellt:

    POST https://[service name].search.windows.net/datasources?api-version=2017-11-11
    Content-Type: application/json
    api-key: [Search service admin key]

    {
        "name": "mydocdbdatasource",
        "type": "documentdb",
        "credentials": {
            "connectionString": "AccountEndpoint=https://myDocDbEndpoint.documents.azure.com;AccountKey=myDocDbAuthKey;Database=myDocDbDatabaseId"
        },
        "container": { "name": "myDocDbCollectionId" },
        "dataChangeDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName": "_ts"
        },
        "dataDeletionDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
            "softDeleteColumnName": "isDeleted",
            "softDeleteMarkerValue": "true"
        }
    }

## <a name="NextSteps"></a>Nächste Schritte
Glückwunsch! Sie wissen nun, wie Azure Cosmos DB mit dem Indexer in Azure Search integriert wird.

* Weitere Informationen zu Azure Cosmos DB finden Sie auf der [Seite über den Azure Cosmos DB-Dienst](https://azure.microsoft.com/services/cosmos-db/).
* Weitere Informationen zu Azure Search finden Sie auf der [Seite des Search-Diensts](https://azure.microsoft.com/services/search/).
