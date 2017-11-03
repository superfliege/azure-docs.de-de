---
title: Durchsuchen von teilweise strukturierten Daten in Azure-Cloudspeicher
description: Es wird beschrieben, wie Sie teilweise strukturierte Blobdaten mit Azure Search durchsuchen.
author: roygara
manager: timlt
ms.service: search
ms.topic: tutorial
ms.date: 10/12/2017
ms.author: v-rogara
ms.custom: mvc
ms.openlocfilehash: ea57fa35f09299f95cdfd3c11b44657d35972295
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2017
---
# <a name="search-semi-structured-data-in-cloud-storage"></a>Durchsuchen von teilweise strukturierten Daten in Cloudspeicher

In dieser zweiteiligen Tutorialreihe erfahren Sie, wie Sie teilweise strukturierte und unstrukturierte Daten mit Azure Search durchsuchen. In diesem Tutorial wird veranschaulicht, wie Sie teilweise strukturierte Daten, z.B. im JSON-Format, durchsuchen, die in Azure-Blobs gespeichert sind. Teilweise strukturierte Daten enthalten Tags oder Markierungen, mit denen Inhalte in den Daten voneinander getrennt werden. Sie unterscheiden sich von strukturierten Daten darin, dass keine formale Strukturierung gemäß einem Datenmodell durchgeführt wird, z.B. einem relationalen Datenbankschema.

In diesem Teil wird Folgendes beschrieben:

> [!div class="checklist"]
> * Erstellen und Auffüllen eines Index in einem Azure Search-Dienst
> * Verwenden des Azure Search-Diensts zum Durchsuchen des Index

> [!NOTE]
> Die Unterstützung von JSON-Arrays ist ein Vorschaufeature in Azure Search. Es ist im Portal derzeit nicht verfügbar. Aus diesem Grund verwenden wir die Vorschauversion der REST-API, in der das Feature enthalten ist, und ein REST-Clienttool zum Aufrufen der API.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:
* Arbeiten Sie das [vorherige Tutorial](../storage/blobs/storage-unstructured-search.md) durch.
    * In diesem Tutorial werden das Speicherkonto und der Suchdienst aus dem vorherigen Tutorial verwendet.
* Installieren Sie einen REST-Client, und machen Sie sich mit dem Erstellen einer HTTP-Anforderung vertraut.


## <a name="set-up-the-rest-client"></a>Einrichten des REST-Clients

Sie benötigen einen REST-Client, um dieses Tutorial durcharbeiten zu können. Für dieses Tutorial nutzen wir [Postman](https://www.getpostman.com/). Sie können auch einen anderen REST-Client verwenden, wenn Sie damit bereits gut vertraut sind.

Starten Sie Postman, nachdem Sie die Anwendung installiert haben.

Falls Sie zum ersten Mal REST-Aufrufe für Azure durchführen, hilft Ihnen diese kurze Einführung in die wichtigsten Komponenten für dieses Tutorial weiter: Die Anforderungsmethode für jeden Aufruf in diesem Tutorial ist „POST“. Die Headerschlüssel sind „Content-type“ und „api-key“. Die Werte der Headerschlüssel sind „application/json“ bzw. Ihr „Administratorschlüssel“ (der Administratorschlüssel ist ein Platzhalter für Ihren Primärschlüssel für die Suche). Im Text ordnen Sie den eigentlichen Inhalt Ihres Aufrufs an. Je nach verwendetem Client kann der Aufbau Ihrer Abfrage leicht abweichen, aber hier erhalten Sie die grundlegenden Informationen.

  ![Teilweise strukturierte Suche](media/search-semi-structured-data/postmanoverview.png)

Für die in diesem Tutorial beschriebenen REST-Aufrufe ist Ihr API-Schlüssel für die Suche erforderlich. Sie finden Ihren API-Schlüssel unter **Schlüssel** in Ihrem Suchdienst. Dieser API-Schlüssel muss im Header jedes API-Aufrufs enthalten sein (ersetzen Sie entsprechend „admin key“ im obigen Screenshot), den Sie für dieses Tutorial durchführen. Halten Sie den Schlüssel immer bereit, da Sie ihn für jeden Aufruf benötigen.

  ![Teilweise strukturierte Suche](media/search-semi-structured-data/keys.png)

## <a name="download-the-sample-data"></a>Herunterladen der Beispieldaten

Es wurde ein Beispieldataset für Sie vorbereitet. **Laden Sie [clinical-trials-json.zip](https://github.com/Azure-Samples/storage-blob-integration-with-cdn-search-hdi/raw/master/clinical-trials-json.zip)** herunter, und entzippen Sie die Datei in einem eigenen Ordner.

Im Beispiel sind JSON-Beispieldateien enthalten, bei denen es sich ursprünglich um Textdateien von [clinicaltrials.gov](https://clinicaltrials.gov/ct2/results) handelt. Wir haben sie der Einfachheit halber in JSON-Code konvertiert.

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich beim [Azure-Portal](http://portal.azure.com)an.

## <a name="upload-the-sample-data"></a>Hochladen der Beispieldaten

Navigieren Sie im Azure-Portal zurück zu dem Speicherkonto, das im [vorherigen Tutorial](../storage/blobs/storage-unstructured-search.md) erstellt wurde. Öffnen Sie dann den Container **data**, und klicken Sie auf **Hochladen**.

Klicken Sie auf **Erweitert**, geben Sie „clinical-trials-json“ ein, und laden Sie dann alle JSON-Dateien hoch, die Sie heruntergeladen haben.

  ![Teilweise strukturierte Suche](media/search-semi-structured-data/clinicalupload.png)

Nach Abschluss des Uploads sollten die Dateien im Datencontainer in einem eigenen Unterordner angezeigt werden.

## <a name="connect-your-search-service-to-your-container"></a>Verbinden des Azure Search-Diensts mit Ihrem Container

Wir nutzen Postman zum Senden von drei API-Aufrufen an Ihren Suchdienst, um eine Datenquelle, einen Index und einen Indexer zu erstellen. Die Datenquelle enthält einen Zeiger auf Ihr Speicherkonto und Ihre JSON-Daten. Ihr Suchdienst stellt die Verbindung beim Laden der Daten her.

Die Abfragezeichenfolge muss **api-version=2016-09-01-Preview** enthalten, und für jeden Aufruf sollte **201 Created** zurückgegeben werden. Die allgemein verfügbare API-Version verfügt noch nicht über die Funktion zum Behandeln von json als jsonArray. Dies ist derzeit nur für die API-Vorschauversion möglich.

Führen Sie die folgenden drei API-Aufrufe über Ihren REST-Client aus.

### <a name="create-a-datasource"></a>Erstellen einer Datenquelle

Eine Datenquelle gibt an, welche Daten indiziert werden sollen.

Der Endpunkt dieses Aufrufs lautet `https://[service name].search.windows.net/datasources?api-version=2016-09-01-Preview`. Ersetzen Sie `[service name]` durch den Namen Ihres Suchdiensts.

Für diesen Aufruf benötigen Sie den Namen Ihres Speicherkontos und Ihren Speicherkontoschlüssel. Sie finden den Speicherkontoschlüssel im Azure-Portal unter den **Zugriffsschlüsseln** Ihres Speicherkontos. Der Speicherort ist in der folgenden Abbildung angegeben:

  ![Teilweise strukturierte Suche](media/search-semi-structured-data/storagekeys.png)

Stellen Sie sicher, dass Sie vor dem Ausführen des Aufrufs im Text `[storage account name]` und `[storage account key]` ersetzen.

```json
{
    "name" : "clinical-trials-json",
    "type" : "azureblob",
    "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=[storage account name];AccountKey=[storage account key];" },
    "container" : { "name" : "data", "query" : "clinical-trials-json" }
}
```

Das Ergebnis sollte in etwa wie folgt aussehen:

```json
{
    "@odata.context": "https://exampleurl.search.windows.net/$metadata#datasources/$entity",
    "@odata.etag": "\"0x8D505FBC3856C9E\"",
    "name": "clinical-trials-json",
    "description": null,
    "type": "azureblob",
    "subtype": null,
    "credentials": {
        "connectionString": "DefaultEndpointsProtocol=https;AccountName=[mystorageaccounthere];AccountKey=[[myaccountkeyhere]]];"
    },
    "container": {
        "name": "data",
        "query": "clinical-trials-json"
    },
    "dataChangeDetectionPolicy": null,
    "dataDeletionDetectionPolicy": null
}
```

### <a name="create-an-index"></a>Erstellen eines Index
    
Mit dem zweiten API-Aufruf wird ein Index erstellt. Ein Index gibt alle Parameter und die dazugehörigen Attribute an.

Die URL für diesen Aufruf lautet `https://[service name].search.windows.net/indexes?api-version=2016-09-01-Preview`. Ersetzen Sie `[service name]` durch den Namen Ihres Suchdiensts.

Ersetzen Sie zunächst die URL. Fügen Sie den folgenden Code in den Text ein, und führen Sie die Abfrage aus.

```json
{
  "name": "clinical-trials-json-index",  
  "fields": [
  {"name": "FileName", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": false, "filterable": false, "sortable": true},
  {"name": "Description", "type": "Edm.String", "searchable": true, "retrievable": false, "facetable": false, "filterable": false, "sortable": false},
  {"name": "MinimumAge", "type": "Edm.Int32", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": true},
  {"name": "Title", "type": "Edm.String", "searchable": true, "retrievable": true, "facetable": false, "filterable": true, "sortable": true},
  {"name": "URL", "type": "Edm.String", "searchable": false, "retrievable": false, "facetable": false, "filterable": false, "sortable": false},
  {"name": "MyURL", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": false, "filterable": false, "sortable": false},
  {"name": "Gender", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": false},
  {"name": "MaximumAge", "type": "Edm.Int32", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": true},
  {"name": "Summary", "type": "Edm.String", "searchable": true, "retrievable": true, "facetable": false, "filterable": false, "sortable": false},
  {"name": "NCTID", "type": "Edm.String", "key": true, "searchable": true, "retrievable": true, "facetable": false, "filterable": true, "sortable": true},
  {"name": "Phase", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": false},
  {"name": "Date", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": false, "filterable": false, "sortable": true},
  {"name": "OverallStatus", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": false},
  {"name": "OrgStudyId", "type": "Edm.String", "searchable": true, "retrievable": true, "facetable": false, "filterable": true, "sortable": false},
  {"name": "HealthyVolunteers", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": false},
  {"name": "Keywords", "type": "Collection(Edm.String)", "searchable": true, "retrievable": true, "facetable": true, "filterable": false, "sortable": false},
  {"name": "metadata_storage_last_modified", "type":"Edm.DateTimeOffset", "searchable": false, "retrievable": true, "filterable": true, "sortable": false},
  {"name": "metadata_storage_size", "type":"Edm.String", "searchable": false, "retrievable": true, "filterable": true, "sortable": false},
  {"name": "metadata_content_type", "type":"Edm.String", "searchable": true, "retrievable": true, "filterable": true, "sortable": false}
  ],
  "suggesters": [
  {
    "name": "sg",
    "searchMode": "analyzingInfixMatching",
    "sourceFields": ["Title"]
  }
  ]
}
```

Das Ergebnis sollte in etwa wie folgt aussehen:

```json
{
    "@odata.context": "https://exampleurl.search.windows.net/$metadata#indexes/$entity",
    "@odata.etag": "\"0x8D505FC00EDD5FA\"",
    "name": "clinical-trials-json-index",
    "fields": [
        {
            "name": "FileName",
            "type": "Edm.String",
            "searchable": false,
            "filterable": false,
            "retrievable": true,
            "sortable": true,
            "facetable": false,
            "key": false,
            "indexAnalyzer": null,
            "searchAnalyzer": null,
            "analyzer": null,
            "synonymMaps": []
        },
        {
            "name": "Description",
            "type": "Edm.String",
            "searchable": true,
            "filterable": false,
            "retrievable": false,
            "sortable": false,
            "facetable": false,
            "key": false,
            "indexAnalyzer": null,
            "searchAnalyzer": null,
            "analyzer": null,
            "synonymMaps": []
        },
        ...
          "scoringProfiles": [],
    "defaultScoringProfile": null,
    "corsOptions": null,
    "suggesters": [],
    "analyzers": [],
    "tokenizers": [],
    "tokenFilters": [],
    "charFilters": []
}
```

### <a name="create-an-indexer"></a>Erstellen eines Indexers

Ein Indexer verbindet die Datenquelle mit dem Zielsuchindex und stellt optional einen Zeitplan für die Automatisierung der Datenaktualisierung bereit.

Die URL für diesen Aufruf lautet `https://[service name].search.windows.net/indexers?api-version=2016-09-01-Preview`. Ersetzen Sie `[service name]` durch den Namen Ihres Suchdiensts.

Ersetzen Sie zunächst die URL. Fügen Sie den folgenden Code in den Text ein, und führen Sie die Abfrage aus.

```json
{
  "name" : "clinical-trials-json-indexer",
  "dataSourceName" : "clinical-trials-json",
  "targetIndexName" : "clinical-trials-json-index",
  "parameters" : { "configuration" : { "parsingMode" : "jsonArray" } }
}
```

Die Antwort sollte in etwa wie folgt aussehen:

```json
{
    "@odata.context": "https://exampleurl.search.windows.net/$metadata#indexers/$entity",
    "@odata.etag": "\"0x8D505FDE143D164\"",
    "name": "clinical-trials-json-indexer",
    "description": null,
    "dataSourceName": "clinical-trials-json",
    "targetIndexName": "clinical-trials-json-index",
    "schedule": null,
    "parameters": {
        "batchSize": null,
        "maxFailedItems": null,
        "maxFailedItemsPerBatch": null,
        "base64EncodeKeys": null,
        "configuration": {
            "parsingMode": "jsonArray"
        }
    },
    "fieldMappings": [],
    "enrichers": [],
    "disabled": null
}
```

## <a name="search-your-json-files"></a>Durchsuchen von JSON-Dateien

Nachdem Ihr Suchdienst nun mit Ihrem Datencontainer verbunden wurde, können Sie mit dem Durchsuchen Ihrer Dateien beginnen.

Öffnen Sie das Azure-Portal, und navigieren Sie zurück zu Ihrem Suchdienst. Dies haben Sie auch im vorherigen Tutorial so durchgeführt.

  ![Unstrukturierte Suche](media/search-semi-structured-data/indexespane.png)

### <a name="user-defined-metadata-search"></a>Suche nach benutzerdefinierten Metadaten

Die Daten können auch hier wieder auf verschiedene Weise abgefragt werden: Volltextsuche, Systemeigenschaften oder benutzerdefinierte Metadaten. Sowohl Systemeigenschaften als auch benutzerdefinierte Metadaten können nur dann mit dem Parameter `$select` durchsucht werden, wenn sie beim Erstellen des Zielindex als **retrievable** (abrufbar) gekennzeichnet wurden. Parameter im Index können nach ihrer Erstellung nicht mehr geändert werden. Es können aber zusätzliche Parameter hinzugefügt werden.

Ein Beispiel für eine grundlegende Abfrage ist `$select=Gender,metadata_storage_size`, mit der die Rückgabe auf diese beiden Parameter beschränkt wird.

  ![Teilweise strukturierte Suche](media/search-semi-structured-data/lastquery.png)

Ein Beispiel für eine komplexere Abfrage ist `$filter=MinimumAge ge 30 and MaximumAge lt 75`. Hiermit werden nur Ergebnisse zurückgegeben, bei denen der Parameter MinimumAge größer oder gleich 30 und der Parameter MaximumAge kleiner als 75 ist.

  ![Teilweise strukturierte Suche](media/search-semi-structured-data/metadatashort.png)

Wenn Sie experimentieren und einige weitere Abfragen selbst ausprobieren möchten, können Sie dies tun. Denken Sie daran, dass Sie logische Operatoren (and, or, not) und Vergleichsoperatoren (eq, ne, gt, lt, ge, le) nutzen können. Bei Zeichenfolgenvergleichen wird die Groß-/Kleinschreibung beachtet.

Der Parameter `$filter` funktioniert nur mit Metadaten, die beim Erstellen des Index als filterbar gekennzeichnet wurden.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Informationen zum Suchen von teilweise strukturierten Daten mit Azure Search erhalten, z.B.:

> [!div class="checklist"]
> * Erstellen eines Azure Search-Diensts mit der REST-API
> * Verwenden des Azure Search-Diensts zum Durchsuchen Ihres Containers

Unter diesem Link erhalten Sie weitere Informationen zur Suche.

> [!div class="nextstepaction"]
> [Indizieren von Dokumenten in Azure Blob Storage mit Azure Search](search-howto-indexing-azure-blob-storage.md)