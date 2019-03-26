---
title: 'Tutorial zum Durchsuchen von JSON in Azure Blob Storage: Azure Search'
description: In diesem Tutorial erfahren Sie, wie teilweise strukturierte Azure-Blobbaten mithilfe von Azure Search durchsucht werden.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: tutorial
ms.date: 03/18/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 1c8ce14dd3961eff33a54a14c2bd0b27650d8a50
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58201346"
---
# <a name="tutorial-search-semi-structured-data-in-azure-cloud-storage"></a>Tutorial: Durchsuchen von teilweise strukturierten Daten in Azure-Cloudspeicher

Azure Search kann JSON-Dokumente und -Arrays in Azure Blob Storage mithilfe eines [Indexers](search-indexer-overview.md) indizieren, der teilweise strukturierte Daten lesen kann. Teilweise strukturierte Daten enthalten Tags oder Markierungen, mit denen Inhalte in den Daten voneinander getrennt werden. Sie bilden den Unterschied zwischen unstrukturierten Daten, die vollständig indiziert werden müssen, und formal strukturierten Daten, die einem Datenmodell entsprechen (wie z. B. einem Schema einer relationalen Datenbank), das pro Feld indiziert werden kann.

In diesem Tutorial verwenden Sie die [Azure Search-REST-APIs](https://docs.microsoft.com/rest/api/searchservice/) und einen REST-Client, um die folgenden Aufgaben auszuführen:

> [!div class="checklist"]
> * Konfigurieren einer Azure Search-Datenquelle für einen Azure-Blobcontainer
> * Erstellen eines Azure Search-Index mit durchsuchbarem Inhalt
> * Konfigurieren und Ausführen eines Indexers zum Lesen des Containers und zum Extrahieren von durchsuchbaren Inhalten aus Azure Blob Storage
> * Durchsuchen des soeben erstellten Index

> [!NOTE]
> Dieses Tutorial basiert auf JSON-Array-Unterstützung, die derzeit als Vorschaufeature in Azure Search vorhanden ist. Sie ist nicht im Portal verfügbar. Aus diesem Grund verwenden wir die Vorschauversion der REST-API, in der dieses Feature enthalten ist, und ein REST-Clienttool zum Aufrufen der API.

## <a name="prerequisites"></a>Voraussetzungen

[Erstellen Sie einen Azure Search-Dienst](search-create-service-portal.md), oder suchen Sie in Ihrem aktuellen Abonnement [nach einem vorhandenen Dienst](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices). In diesem Tutorial können Sie einen kostenlosen Dienst verwenden.

[Erstellen Sie ein Azure-Speicherkonto](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account), das Beispieldaten enthält.

[Verwenden Sie Postman](https://www.getpostman.com/) oder einen anderen REST-Client, um Ihre Anforderungen zu senden. Anweisungen zum Einrichten einer HTTP-Anforderung in Postman finden Sie im nächsten Abschnitt.

## <a name="set-up-postman"></a>Einrichten von Postman

Starten Sie Postman, und richten Sie eine HTTP-Anforderung ein. Wenn Sie mit diesem Tool nicht vertraut sind, lesen Sie [Untersuchen von Azure Search-REST-APIs mit Postman oder Fiddler](search-fiddler.md).

Die Anforderungsmethode für jeden Aufruf in diesem Tutorial ist „POST“. Die Headerschlüssel sind „Content-type“ und „api-key“. Die Werte der Headerschlüssel sind „application/json“ bzw. Ihr „Administratorschlüssel“ (der Administratorschlüssel ist ein Platzhalter für Ihren Primärschlüssel für die Suche). Im Text ordnen Sie den eigentlichen Inhalt Ihres Aufrufs an. Je nach verwendetem Client kann der Aufbau Ihrer Abfrage leicht abweichen, aber hier erhalten Sie die grundlegenden Informationen.

  ![Teilweise strukturierte Suche](media/search-semi-structured-data/postmanoverview.png)

Für die in diesem Tutorial beschriebenen REST-Aufrufe ist Ihr API-Schlüssel für die Suche erforderlich. Sie finden Ihren API-Schlüssel unter **Schlüssel** in Ihrem Suchdienst. Dieser API-Schlüssel muss im Header jedes API-Aufrufs enthalten sein (ersetzen Sie entsprechend „admin key“ im obigen Screenshot), den Sie für dieses Tutorial durchführen. Halten Sie den Schlüssel immer bereit, da Sie ihn für jeden Aufruf benötigen.

  ![Teilweise strukturierte Suche](media/search-semi-structured-data/keys.png)

## <a name="prepare-sample-data"></a>Vorbereiten der Beispieldaten

1. **Laden Sie [clinical-trials-json.zip](https://github.com/Azure-Samples/storage-blob-integration-with-cdn-search-hdi/raw/master/clinical-trials-json.zip)** herunter, und entzippen Sie die Datei in einem eigenen Ordner. Die Daten stammen aus [ClinicalTrials.gov](https://clinicaltrials.gov/ct2/results) und werden für dieses Tutorial in JSON konvertiert.

2. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, navigieren Sie zu Ihrem Azure-Speicherkonto, öffnen Sie den Container **Daten**, und klicken Sie dann auf **Hochladen**.

3. Klicken Sie auf **Erweitert**, geben Sie „clinical-trials-json“ ein, und laden Sie dann alle JSON-Dateien hoch, die Sie heruntergeladen haben.

  ![Teilweise strukturierte Suche](media/search-semi-structured-data/clinicalupload.png)

Nach Abschluss des Uploads sollten die Dateien im Datencontainer in einem eigenen Unterordner angezeigt werden.

## <a name="connect-your-search-service-to-your-container"></a>Verbinden des Azure Search-Diensts mit Ihrem Container

Wir nutzen Postman zum Senden von drei API-Aufrufen an Ihren Suchdienst, um eine Datenquelle, einen Index und einen Indexer zu erstellen. Die Datenquelle enthält einen Zeiger auf Ihr Speicherkonto und Ihre JSON-Daten. Ihr Suchdienst stellt die Verbindung beim Laden der Daten her.

Die Abfragezeichenfolge muss eine Vorschau-API (z. B. **api-version=2017-11-11-Preview**) enthalten, und für jeden Aufruf sollte **201 Created** zurückgegeben werden. Die allgemein verfügbare API-Version verfügt noch nicht über die Funktion zum Behandeln von json als jsonArray. Dies ist derzeit nur für die API-Vorschauversion möglich.

Führen Sie die folgenden drei API-Aufrufe über Ihren REST-Client aus.

## <a name="create-a-data-source"></a>Erstellen einer Datenquelle

Eine Datenquelle ist ein Azure Search-Objekt, das angibt, welche Daten indiziert werden sollen.

Der Endpunkt dieses Aufrufs lautet `https://[service name].search.windows.net/datasources?api-version=2016-09-01-Preview`. Ersetzen Sie `[service name]` durch den Namen Ihres Suchdiensts. Für diesen Aufruf benötigen Sie den Namen Ihres Speicherkontos und Ihren Speicherkontoschlüssel. Sie finden den Speicherkontoschlüssel im Azure-Portal unter den **Zugriffsschlüsseln** Ihres Speicherkontos. Der Speicherort ist in der folgenden Abbildung angegeben:

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

Die Antwort sollte in etwa wie folgt aussehen:

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

## <a name="create-an-index"></a>Erstellen eines Index
    
Mit dem zweiten API-Aufruf wird ein Azure Search-Index erstellt. Ein Index gibt alle Parameter und die dazugehörigen Attribute an.

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

Die Antwort sollte in etwa wie folgt aussehen:

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

## <a name="create-and-run-an-indexer"></a>Erstellen und Ausführen eines Indexers

Ein Indexer verbindet die Datenquelle, importiert Daten in den Zielsuchindex und stellt optional einen Zeitplan für die Automatisierung der Datenaktualisierung bereit.

Die URL für diesen Aufruf lautet `https://[service name].search.windows.net/indexers?api-version=2016-09-01-Preview`. Ersetzen Sie `[service name]` durch den Namen Ihres Suchdiensts.

Ersetzen Sie zunächst die URL. Kopieren Sie dann den folgenden Code, fügen Sie ihn in den Text ein, und senden Sie die Anforderung. Die Anforderung wird sofort verarbeitet. Wenn die Antwort eintrifft, verfügen Sie über einen Index, der für die Volltextsuche verwendet werden kann.

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

Sie können jetzt Abfragen für den Index ausgeben. Verwenden Sie für diese Aufgabe den [**Suchexplorer**](search-explorer.md) im Portal.

  ![Unstrukturierte Suche](media/search-semi-structured-data/indexespane.png)

### <a name="user-defined-metadata-search"></a>Suche nach benutzerdefinierten Metadaten

Die Daten können auch hier wieder auf verschiedene Weise abgefragt werden: Volltextsuche, Systemeigenschaften oder benutzerdefinierte Metadaten. Sowohl Systemeigenschaften als auch benutzerdefinierte Metadaten können nur dann mit dem Parameter `$select` durchsucht werden, wenn sie beim Erstellen des Zielindex als **retrievable** (abrufbar) gekennzeichnet wurden. Parameter im Index können nach ihrer Erstellung nicht mehr geändert werden. Es können aber zusätzliche Parameter hinzugefügt werden.

Ein Beispiel für eine grundlegende Abfrage ist `$select=Gender,metadata_storage_size`, mit der die Rückgabe auf diese beiden Parameter beschränkt wird.

  ![Teilweise strukturierte Suche](media/search-semi-structured-data/lastquery.png)

Ein Beispiel für eine komplexere Abfrage ist `$filter=MinimumAge ge 30 and MaximumAge lt 75`. Hiermit werden nur Ergebnisse zurückgegeben, bei denen der Parameter MinimumAge größer oder gleich 30 und der Parameter MaximumAge kleiner als 75 ist.

  ![Teilweise strukturierte Suche](media/search-semi-structured-data/metadatashort.png)

Wenn Sie experimentieren und einige weitere Abfragen selbst ausprobieren möchten, können Sie dies tun. Denken Sie daran, dass Sie logische Operatoren (and, or, not) und Vergleichsoperatoren (eq, ne, gt, lt, ge, le) nutzen können. Bei Zeichenfolgenvergleichen wird die Groß-/Kleinschreibung beachtet.

Der Parameter `$filter` funktioniert nur mit Metadaten, die beim Erstellen des Index als filterbar gekennzeichnet wurden.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Die schnellste Möglichkeit, das System nach einem Tutorial zu bereinigen, besteht im Löschen der Ressourcengruppe, die den Azure Search-Dienst enthält. Sie können dann die Ressourcengruppe löschen, um alle darin enthaltenen Daten endgültig zu löschen. Der Name der Ressourcengruppe befindet sich im Portal auf der Seite „Übersicht“ des Azure Search-Diensts.

## <a name="next-steps"></a>Nächste Schritte

Sie können KI-basierte Algorithmen an eine Indexer-Pipeline anfügen. Als Nächstes fahren Sie mit dem folgenden Tutorial fort.

> [!div class="nextstepaction"]
> [Indizieren von Dokumenten in Azure Blob Storage mit Azure Search](search-howto-indexing-azure-blob-storage.md)