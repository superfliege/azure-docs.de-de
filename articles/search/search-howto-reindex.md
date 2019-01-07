---
title: 'Neuerstellen eines Azure Search-Indexes oder Aktualisieren von durchsuchbarem Inhalt: Azure Search'
description: Fügen Sie neue Elemente zu einer Neuerstellung oder einer inkrementellen Teilindizierung hinzu, aktualisieren Sie bereits vorhandene Elemente oder Dokumente, oder löschen Sie veraltete Dokumente, um einen Azure Search-Index zu aktualisieren.
services: search
author: HeidiSteen
manager: cgronlun
ms.service: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 9c9af69e45af6a70c5327393a1c10385ba2c2aed
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/12/2018
ms.locfileid: "53316895"
---
# <a name="how-to-rebuild-an-azure-search-index"></a>Neuerstellen eines Azure Search-Indexes

Wenn ein Index neu erstellt wird, wird dessen Struktur geändert. Dadurch wird wiederum der physische Ausdruck des Indexes in Ihrem Azure Search-Dienst verändert. Im Gegensatz dazu handelt es sich bei der Aktualisierung des Indexes um ein Update, das nur den Inhalt und insbesondere die neusten Änderungen betrifft, die von einer zugehörigen externen Datenquelle ausgehen. In diesem Artikel erhalten Sie Anweisungen zum grundlegenden und strukturellen Aktualisieren von Indizes.

Für Indexupdates sind Lese-/Schreibberechtigungen auf Dienstebene erforderlich. Sie können programmgesteuert REST- oder .NET-APIs für eine vollständige Neuerstellung oder eine inkrementelle Indizierung von Inhalt mit Parametern aufrufen, die Updateoptionen angeben. 

In der Regel sind Updates eines Indexes bedarfsgesteuert. Sie können aber für Indizes, die mit quellenspezifischen [Indexern](search-indexer-overview.md) aufgefüllt werden, auch einen integrierten Scheduler verwenden. Der Scheduler unterstützt Aktualisierungen des Dokuments im 15-Minuten-Takt. Sie können aber je nach Bedarf auch ein anderes Intervall und Muster anwenden. Wenn Sie häufiger Aktualisierungen durchführen möchten, müssen Sie manuelle Indexupdates vornehmen, indem Sie z.B. Transaktionen doppelt schreiben, indem Sie die externe Datenquelle und den Azure Search-Index gleichzeitig aktualisieren.

## <a name="full-rebuilds"></a>Vollständige Neuerstellungen

Für viele Updatetypen ist eine vollständige Neuerstellung erforderlich. Eine vollständige Neuerstellung umfasst das Löschen eines Indexes, von Daten und Metadaten und die anschließende Wiederauffüllung des Indexes über externe Datenquellen. [Löschen](https://docs.microsoft.com/rest/api/searchservice/delete-index) und [Erstellen](https://docs.microsoft.com/rest/api/searchservice/create-index) Sie den Index programmgesteuert, und [laden](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) Sie ihn erneut, um diesen anschließend neu zu erstellen. 

Beachten Sie nach der Neuerstellung, dass abweichende Abfrageergebnisse zu erwarten sind, wenn Sie Abfragemuster und Bewertungsprofile getestet haben und sich der zugrundeliegende Inhalt geändert hat.

## <a name="when-to-rebuild"></a>Zeitpunkt einer Neuerstellung

Planen Sie häufige, vollständige Neuerstellungen für die aktive Entwicklung ein, wenn sich Indexschemas im Flux-Zustand befinden.

| Änderung | Neuerstellungsstatus|
|--------------|---------------|
| Ändern eines Feldnamens, Datentyps oder der [Indexattribute](https://docs.microsoft.com/rest/api/searchservice/create-index) | Wenn eine Felddefinition geändert wird, entstehen mit Ausnahme der folgenden [Indexattribute](https://docs.microsoft.com/rest/api/searchservice/create-index) in der Regel Einbußen bei der Neuerstellung: „Retrievable“, „SearchAnalyzer“ und „SynonymMaps“. Sie können die Retrievable-, SearchAnalyzer- und SynonymMaps-Attribute zu einem bereits vorhandenen Feld hinzufügen und müssen keinen Index neu erstellen.|
| Hinzufügen eines Felds | Es ist nicht zwingend eine Neuerstellung erforderlich. Bereits vorhandenen indizierten Dokumenten wird ein NULL-Wert für das neue Feld hinzugefügt. Bei einer späteren Neuindizierung ersetzen Werte aus Quelldaten die von Azure Search hinzugefügten NULL-Werte. |
| Löschen eines Felds | Sie können ein Feld nicht direkt über einen Azure Search-Index löschen. Stattdessen sollte Ihre Anwendung das Feld „Gelöscht“ ignorieren, damit es nicht mehr verwendet wird. Die Felddefinition und die Inhalte bleiben im Index, bis Sie Ihren Index das nächste Mal unter Verwendung eines Schemas neu erstellen, das das betreffende Feld auslässt.|

> [!Note]
> Außerdem ist eine Neuerstellung erforderlich, wenn Sie die Schicht wechseln. Wenn Sie sich gleichzeitig für mehr Kapazitäten entscheiden, gibt es kein direktes Upgrade. Es muss am neuen Kapazitätspunkt ein neuer Dienst erstellt werden, und Indizes müssen von Grund auf für den neuen Dienst erstellt werden. 

## <a name="partial-or-incremental-indexing"></a>Teilindizierung oder inkrementelle Indizierung

Sobald ein Index hergestellt wird, ändert sich der Fokus auf die inkrementelle Indizierung. Dabei entstehen in der Regel keine wahrnehmbaren Dienstausfälle. Bei der Teilindizierung oder der inkrementellen Indizierung handelt es sich um eine Workload, die sich nur auf den Inhalt bezieht und den Inhalt des Suchindex synchronisiert, um den Zustand des Inhalts in einer zugehörigen Datenquelle zu reflektieren. Ein Dokument, das der Quelle hinzugefügt oder aus ihr entfernt wird, wird auch dem Index hinzugefügt bzw. aus ihm entfernt. Rufen Sie im Code den Vorgang [Hinzufügen, Aktualisieren oder Löschen von Dokumenten](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) oder das entsprechende .NET-Äquivalent auf.

> [!Note]
> Wenn Sie Indexer verwenden, die externe Datenquellen auffüllen, werden Mechanismen für die Änderungsnachverfolgung in Quellsystemen für die inkrementelle Indizierung genutzt. Für den [Azure Blob-Speicher](search-howto-indexing-azure-blob-storage.md#incremental-indexing-and-deletion-detection) wird ein `lastModified`-Feld verwendet. Im [Azure-Tabellenspeicher](search-howto-indexing-azure-tables.md#incremental-indexing-and-deletion-detection) dient `timestamp` demselben Zweck. Genauso weisen sowohl der [Azure SQL-Datenbank-Indexer](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md#capture-new-changed-and-deleted-rows) als auch der [Azure Cosmos DB-Indexer](search-howto-index-cosmosdb.md#indexing-changed-documents) Felder zum Kennzeichnen von Zeilenupdates auf. Weitere Informationen zu Indexern finden Sie in der [Indexer-Übersicht](search-indexer-overview.md).


## <a name="see-also"></a>Weitere Informationen

+ [Indexer (Übersicht)](search-indexer-overview.md)
+ [Bedarfsorientiertes Indizieren großer Datasets](search-howto-large-index.md)
+ [Indizieren im Portal](search-import-data-portal.md)
+ [Azure SQL-Datenbank-Indexer](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
+ [Azure Cosmos DB-Indexer](search-howto-index-cosmosdb.md)
+ [Azure Blob Storage-Indexer](search-howto-indexing-azure-blob-storage.md)
+ [Azure Table Storage-Indexer](search-howto-indexing-azure-tables.md)
+ [Sicherheit in Azure Search](search-security-overview.md)