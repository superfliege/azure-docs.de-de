---
title: Indizierung von CSV-Blobs mit Azure Search-Blobindexer – Azure Search
description: Durchforsten Sie CSV-Blobs im Azure Blob Storage für die Volltextsuche mithilfe eines Azure Search-Index. Indexer automatisieren die Datenerfassung für ausgewählte Datenquellen wie Azure Blob Storage.
ms.date: 10/17/2018
author: mgottein
manager: cgronlun
ms.author: magottei
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seodec2018
ms.openlocfilehash: eac0c1f2f7ded35cf09eec12f0406c754621f49c
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54465461"
---
# <a name="indexing-csv-blobs-with-azure-search-blob-indexer"></a>Indizierung von CSV-Blobs mit Azure Search-Blobindexer
In der Standardeinstellung analysiert der [Azure Search-Blobindexer](search-howto-indexing-azure-blob-storage.md) durch Trennzeichen getrennte Blobs als ein einzelnes Textsegment. Bei Blobs mit CSV-Daten sollen die einzelnen Zeilen im Blob jedoch häufig als separates Dokument behandelt werden. Angenommen, Sie möchten den folgenden durch Trennzeichen getrennten Text in zwei Dokumente analysieren, die jeweils die Felder „Id“, „DatePublished“ und „Tags“ enthalten: 

    id, datePublished, tags
    1, 2016-01-12, "azure-search,azure,cloud" 
    2, 2016-07-07, "cloud,mobile" 

In diesem Artikel erfahren Sie, wie Sie CSV-Blobs mit einem Azure Search-Blobindexer analysieren. 

> [!IMPORTANT]
> CSV-Blobindizierung ist derzeit als öffentliche Vorschau verfügbar und sollte nicht in Produktionsumgebungen verwendet werden. Weitere Informationen finden Sie unter [REST api-version=2017-11-11-Preview](search-api-2017-11-11-preview.md). 
> 

## <a name="setting-up-csv-indexing"></a>Einrichten der CSV-Indizierung
Erstellen oder aktualisieren Sie zum Indizieren von CSV-Blobs eine Indexerdefinition mit dem `delimitedText` -Analysemodus:  

    {
      "name" : "my-csv-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "firstLineContainsHeaders" : true } }
    }

Weitere Informationen zur API zum Erstellen eines Indexers finden Sie unter [Erstellen eines Indexers](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

`firstLineContainsHeaders` gibt an, dass die erste (nicht leere) Zeile der einzelnen Blobs Header enthält.
Wenn Blobs am Anfang keine Headerzeile enthalten, sollten die Header in der Indexerkonfiguration angegeben werden: 

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } } 

Sie können das Trennzeichen mithilfe der Konfigurationseinstellung `delimitedTextDelimiter` anpassen. Beispiel: 

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextDelimiter" : "|" } }

> [!NOTE]
> Derzeit wird nur die UTF-8-Codierung unterstützt. Wenn Sie Unterstützung für andere Codierungen benötigen, stimmen Sie unter [UserVoice](https://feedback.azure.com/forums/263029-azure-search) dafür ab.

> [!IMPORTANT]
> Bei Verwendung des Analysemodus für durch Trennzeichen getrennten Text betrachtet Azure Search alle Blobs in Ihrer Datenquelle als CSV-Blobs. Wenn Sie eine Mischung aus CSV- und Nicht-CSV-Blobs in der gleichen Datenquelle unterstützen müssen, stimmen Sie unter [UserVoice](https://feedback.azure.com/forums/263029-azure-search) dafür ab.
> 
> 

## <a name="request-examples"></a>Beispiele für Anforderungen
Dies alles wird an vollständigen Nutzlastbeispielen demonstriert. 

Datenquelle: 

    POST https://[service name].search.windows.net/datasources?api-version=2017-11-11-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "<optional, my-folder>" }
    }   

Indexer:

    POST https://[service name].search.windows.net/indexers?api-version=2017-11-11-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-csv-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } }
    }

## <a name="help-us-make-azure-search-better"></a>Helfen Sie uns bei der Verbesserung von Azure Search
Wenn Sie sich Features wünschen oder Verbesserungsvorschläge haben, stimmen Sie unter [UserVoice](https://feedback.azure.com/forums/263029-azure-search/) dafür ab.

