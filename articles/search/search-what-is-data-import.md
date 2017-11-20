---
title: Importieren von Daten in Azure Search | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie Daten in einen Index in Azure Search hochladen können."
services: search
documentationcenter: 
author: ashmaka
manager: jhubbard
editor: 
tags: 
ms.assetid: aa8d47c1-4ae6-4209-a8ce-48d5a9474707
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 11/01/2017
ms.author: ashmaka
ms.openlocfilehash: ebf7319f0017b4adef25fe5840864e002c88fea7
ms.sourcegitcommit: 659cc0ace5d3b996e7e8608cfa4991dcac3ea129
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2017
---
# <a name="data-import-in-azure-search"></a>Importieren von Daten in Azure Search
> [!div class="op_single_selector"]
> * [Übersicht](search-what-is-data-import.md)
> * [.NET](search-import-data-dotnet.md)
> * [REST](search-import-data-rest-api.md)
> 
> 

In Azure Search werden Abfragen für Ihren Inhalt ausgeführt, der in einen [Suchindex](search-what-is-an-index.md) geladen wurde. In diesem Artikel werden die zwei grundlegenden Ansätze zum Laden von Inhalt in einen Index untersucht: Sie können Ihre Daten per *Pushvorgang* programmgesteuert in den Index übertragen, oder Sie können für einen [Azure Search-Indexer](search-indexer-overview.md) auf eine unterstützte Datenquelle verweisen, um die Daten per *Pullvorgang* zu beziehen.

## <a name="pushing-data-to-an-index"></a>Übertragen von Daten per Pushvorgang in einen Index
Das Pushmodell, das zum programmgesteuerten Senden Ihrer Daten an Azure Search verwendet wird, ist der flexibelste Ansatz. Erstens gelten keine Einschränkungen für den Datenquellentyp. Alle Datasets, die aus JSON-Dokumenten bestehen, können per Pushvorgang in einen Azure Search-Index übertragen werden, solange jedes Dokument im Dataset über Felder verfügt, die den in Ihrem Indexschema definierten Feldern zugeordnet sind. Zweitens gelten keine Einschränkungen für die Ausführungshäufigkeit. Sie können Änderungen beliebig oft per Pushvorgang in einen Index übertragen. Bei Anwendungen mit sehr niedrigen Latenzanforderungen (wenn z.B. Suchvorgänge mit dynamischen Inventardatenbanken synchronisiert werden müssen) ist das Push-Modell Ihre einzige Option.

Dieser Ansatz ist flexibler als ein Pull-Modell, da Sie Dokumente einzeln oder in Batches hochladen können (bis zu 1000 pro Batch oder 16 MB, je nachdem, welches Limit zuerst erreicht wird). Das Push-Modell ermöglicht Ihnen auch das Hochladen von Dokumenten in Azure Search, unabhängig davon, wo sich Ihre Daten befinden.

### <a name="how-to-push-data-to-an-azure-search-index"></a>Übertragen von Daten in einen Azure Search-Index per Pushvorgang

Sie können die folgenden APIs verwenden, um ein oder mehrere Dokumente in einen Index zu laden:

+ [Hinzufügen, Aktualisieren oder Löschen von Dokumenten (REST-API)](https://docs.microsoft.com/rest/api/searchservice/AddUpdate-or-Delete-Documents)
+ [indexAction-Klasse](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexaction?view=azure-dotnet) oder [indexBatch-Klasse](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexbatch?view=azure-dotnet) 

Es gibt derzeit keine Toolunterstützung für die Push-Übertragung von Daten über das Portal.

Eine Einführung in die einzelnen Methodiken finden Sie unter [Hochladen von Daten in Azure Search über die REST-API](search-import-data-rest-api.md) bzw. [Hochladen von Daten in Azure Search mit dem .NET SDK](search-import-data-dotnet.md).


## <a name="pulling-data-into-an-index"></a>Übertragen von Daten per Pullvorgang in einen Index
Beim Pull-Modell wird eine unterstützte Datenquelle durchsucht, und die Daten werden für Sie automatisch in Ihren Index hochgeladen. In Azure Search wird diese Funktion mit *Indexern* implementiert, die derzeit für diese Plattformen verfügbar sind:

+ [Blob Storage](search-howto-indexing-azure-blob-storage.md)
+ [Table Storage](search-howto-indexing-azure-tables.md)
+ [Azure Cosmos DB](http://aka.ms/documentdb-search-indexer)
+ [Azure SQL-Datenbank und SQL Server auf Azure-VMs](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)

Mit Indexern wird für einen Index eine Verbindung mit einer Datenquelle (normalerweise eine Tabelle, Sicht oder ähnliche Struktur) hergestellt, und Quellfelder werden gleichwertigen Feldern im Index zugeordnet. Während der Ausführung wird das Rowset automatisch in JSON transformiert und in den angegebenen Index geladen. Alle Indexer unterstützen die Verwendung von Zeitplänen, sodass Sie angeben können, wie häufig die Daten aktualisiert werden sollen. Die meisten Indexer verfügen über eine Änderungsnachverfolgung, wenn dies von der Datenquelle unterstützt wird. Da Änderungen und Löschvorgänge in vorhandenen Dokumenten nachverfolgt und neue Dokumente erkannt werden, ist es mithilfe von Indexern nicht mehr erforderlich, die Daten in Ihrem Index aktiv zu verwalten. 


### <a name="how-to-pull-data-into-an-azure-search-index"></a>Übertragen von Daten per Pullvorgang in einen Azure Search-Index

Die Indexer-Funktion steht im [Azure-Portal](search-import-data-portal.md) und für die [REST-API](/rest/api/searchservice/Indexer-operations) und das [.NET SDK](/dotnet/api/microsoft.azure.search.indexersoperations) zur Verfügung. 

Ein Vorteil der Verwendung des Portals ist, dass Azure Search normalerweise ein Indexstandardschema für Sie generieren kann, indem die Metadaten des Quelldatasets gelesen werden. Sie können den generierten Index ändern, bis er verarbeitet wird. Nach der Verarbeitung sind nur noch Schemabearbeitungen zulässig, bei denen keine erneute Indizierung erforderlich ist. Wenn die gewünschten Änderungen eine direkte Auswirkung auf das Schema haben, müssen Sie den Index neu erstellen. 

## <a name="verify-data-import-with-search-explorer"></a>Überprüfen des Datenimports mit dem Suchexplorer

Eine schnelle Möglichkeit zum Durchführen einer vorläufigen Prüfung für den Dokumentupload ist die Verwendung des **Suchexplorers** im Portal. Mit dem Explorer können Sie einen Index abfragen, ohne dass Sie dafür Code schreiben müssen. Die Suche basiert auf den Standardeinstellungen, z.B. der [einfachen Syntax](/rest/api/searchservice/simple-query-syntax-in-azure-search) und dem [searchMode-Abfrageparameter](/rest/api/searchservice/search-documents), der standardmäßig verwendet wird. Ergebnisse werden im JSON-Format zurückgegeben, damit Sie das gesamte Dokument untersuchen können.

> [!TIP]
> Viele [Azure Search-Codebeispiele](https://github.com/Azure-Samples/?utf8=%E2%9C%93&query=search) enthalten eingebettete bzw. sofort verfügbare Datasets, damit Sie schnell starten können. Außerdem enthält das Portal auch einen Beispielindexer und eine Datenquelle mit einem kleinen Immobiliendataset („realestate-us-sample“). Wenn Sie den vorkonfigurierten Indexer in der Beispieldatenquelle ausführen, wird ein Index erstellt und mit Dokumenten geladen, die dann im Suchexplorer oder mit von Ihnen geschriebenem Code abgefragt werden können.

## <a name="see-also"></a>Weitere Informationen

+ [Indexer (Übersicht)](search-indexer-overview.md)
+ [Tutorial: Erstellen Ihres ersten Azure Search-Index im Portal](search-get-started-portal.md)