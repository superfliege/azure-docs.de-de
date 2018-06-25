---
title: Neuerstellen eines Azure Search-Indexes oder Aktualisieren von durchsuchbarem Inhalt | Microsoft-Dokumentation
description: Fügen Sie neue Elemente zu einer Neuerstellung oder einer inkrementellen Teilindizierung hinzu, aktualisieren Sie bereits vorhandene Elemente oder Dokumente, oder löschen Sie veraltete Dokumente, um einen Azure Search-Index zu aktualisieren.
services: search
author: HeidiSteen
manager: cgronlun
ms.service: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: heidist
ms.openlocfilehash: cb99096c1217fca1527b17946dc12390ddf3f62c
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34659609"
---
# <a name="how-to-scale-out-indexing-in-azure-seearch"></a>Horizontales Skalieren der Indizierung in Azure Seearch

Wenn das Datenvolumen zunimmt oder die Verarbeitung geändert werden muss, stellen Sie möglicherweise fest, dass einfache [Neuerstellungen und Aufträge für die erneute Indizierung](search-howto-reindex.md) nicht ausreichend sind. 

Wenn Sie dem höheren Bedarf nachkommen wollen, empfehlen wir als ersten Schritt eine Erweiterung der [Staffelung und Kapazität](search-capacity-planning.md) innerhalb der Grenzen des vorhanden Diensts. 

In einem zweiten Schritt werden Mechanismen für die skalierbare Indizierung hinzugefügt (vorausgesetzt, Sie können [Indexer](search-indexer-overview.md) verwenden). Indexer umfassen einen integrierten Scheduler, mit dem Sie die Indizierung in regelmäßigen Intervallen gliedern oder die Verarbeitung über den 24-Stunden-Rhythmus hinaus erweitern können. Außerdem können Indexer verwendet werden, um ein gewisses Maß an Parallelismus zu erreichen, wenn sie an Definitionen von Datenquellen gekoppelt werden, indem Daten partitioniert und Zeitpläne verwendet werden, um eine parallele Ausführung zu ermöglichen.

### <a name="scheduled-indexing-for-large-data-sets"></a>Geplante Indizierung für große Datasets

Zeitpläne stellen einen wichtigen Mechanismus zum Verarbeiten von großen Datasets und zum langsamen Ausführen von Analysen wie der Bildanalyse in einer Pipeline für die kognitive Suche dar. Die Indexerverarbeitung wird innerhalb von 24 Stunden ausgeführt. Wenn die Verarbeitung nicht innerhalb von 24 Stunden abgeschlossen ist, können Sie die Verhaltensweisen für die Indexerzeitplanung zu Ihrem Vorteil nutzen. 

Standardmäßig startet die geplante Indizierung zu bestimmten Zeitintervallen. Dabei wird ein Vorgang in der Regel abgeschlossen, bevor die Indizierung zum nächsten geplanten Intervall fortgesetzt wird. Wenn der Vorgang jedoch nicht innerhalb des Intervalls abgeschlossen wird, wird der Indexer beendet, da ein Timeout auftritt. Beim nächsten Intervall wird die Verarbeitung an der Stelle fortgesetzt, an der sie während des letzten Intervalls unterbrochen wurde. Dabei verfolgt das System nach, an welcher Stelle dies der Fall ist. 

Das heißt in der Praxis, dass Sie für Indexladungen, die sich über mehrere Tage erstrecken, einen 24-Stunden-Zeitplan für den Indexer festlegen können. Wenn eine Indizierung für den nächsten 24-Stunden-Zeitraum fortgesetzt wird, startet diese bei dem letzten erfolgreich verarbeiteten Dokument neu. Auf diese Weise kann sich ein Indexer über mehrere Tage hinweg durch das Dokumentbacklog durcharbeiten, bis alle nicht verarbeitete Dokumente verarbeitet wurden. Weitere Informationen zu diesem Ansatz finden Sie unter [Indizieren großer Datasets](search-howto-indexing-azure-blob-storage.md#indexing-large-datasets).

<a name="parallel-indexing"></a>

## <a name="parallel-indexing"></a>Parallele Indizierung

Sie können stattdessen auch eine Strategie zur parallelen Indizierung einrichten. Für außergewöhnliche, rechenintensive Anforderungen für die Indizierung (z.B. OCR auf gescannten Dokumenten in einer Pipeline für die kognitive Suche) eignet sich ggf. die Strategie der parallelen Indizierung für dieses spezifische Ziel. In einer Pipeline für die Anreicherung der kognitiven Suche wird die Verarbeitung der Bildanalyse und der natürlichen Sprache nicht mehr ausgeführt. Die parallele Indizierung für einen Dienst, der zum selben Zeitpunkt keine Abfrageanforderungen verarbeitet, könnte sich besonders die Option für die Arbeit durch große Textmengen von Inhalt eignen, der langsam verarbeitet wird. 

Eine Strategie für die parallele Verarbeitung verfügt über die folgenden Elemente:

+ Verteilen Sie Ihre Quelldaten auf mehrere Container oder mehrere virtuelle Ordner innerhalb desselben Containers. 
+ Ordnen Sie jedes noch so kleine Dataset einer [Datenquelle](https://docs.microsoft.com/rest/api/searchservice/create-data-source) zu, die an einen eignen [Indexer](https://docs.microsoft.com/rest/api/searchservice/create-indexer) gekoppelt ist.
+ Verweisen Sie für die kognitive Suche in jeder Indexerdefinition auf dieselbe [Fähigkeitsgruppe](https://docs.microsoft.com/rest/api/searchservice/create-skillset).
+ Schreiben Sie in denselben Suchindex für das Ziel. 
+ Legen Sie für die Ausführung aller Indexer denselben Zeitpunkt fest.

> [!Note]
> Azure Search unterstützt das Dedizieren von Replikaten oder Partitionen auf bestimmte Workloads nicht. Es besteht ein erhöhtes Risiko, dass die gleichzeitige Indizierung Ihr System so belastet, dass die Abfrageleistung beeinträchtigt wird. Wenn Sie über eine Testumgebung verfügen, implementieren Sie die parallele Indizierung dort zuerst, um die Vor- und Nachteile nachvollziehen zu können.

## <a name="configure-parallel-indexing"></a>Konfigurieren der parallelen Indizierung

Für Indexer basiert die Verarbeitung der Kapazität grob auf einem Indexersubsystem für jede Diensteinheit, die von Ihrem Suchdienst verwendet wird. Für Azure Search-Dienste, die für die Tarife Basic und Standard verfügbar sind, sind mehrere gleichzeitige Indexer möglich, die mindestens zwei Replikate aufweisen. 

1. Prüfen Sie im [Azure-Portal](https://portal.azure.com) auf der Dashboardseite **Übersicht** des Search-Diensts den **Tarif**, um festzustellen, ob die parallele Indizierung notwendig ist. Sowohl der Basic- als auch der Standard-Tarif umfassen mehrere Replikate.

2. Erhöhen Sie die [Anzahl der Replikate](search-capacity-planning.md) unter **Einstellungen** > **Staffelung** um ein zusätzliches Replikat für jede Indexerworkload. Geben Sie eine ausreichende Zahl für bestehende Abfragevolumen an. Es sollten keine Abfrageworkloads für die Indizierung geopfert werden.

3. Teilen Sie Daten auf mehrere Container auf. Tun Sie dies auf einer Ebene, die von den Azure Search-Indexern erreicht werden kann. Also z.B. auf mehrere Tabellen in Azure SQL-Datenbank, auf mehrere Container im Azure Blob-Speicher oder auf mehrere Sammlungen. Definieren Sie ein Datenquellobjekt pro Tabelle oder Container.

4. Erstellen Sie mehrere Indexer, und planen Sie eine parallele Ausführung:

   + Gehen Sie von einem Dienst mit sechs Replikaten aus. Konfigurieren Sie sechs Indizes, wobei jeder einer Datenquelle zugeordnet ist, die ein Sechstel des Datasets enthält, sodass Sie das gesamte Dataset auf sechs Teile aufteilen können. 

   + Richten Sie alle Indexer auf einen Index aus. Richten Sie für kognitive Suchworkloads alle Indexer auf eine Fähigkeitsgruppe aus.

   + Legen Sie innerhalb sämtlicher Indexerdefinitionen denselben Zeitplan für das Ausführungsmuster für die Runtime fest. Beispielsweise erstellt `"schedule" : { "interval" : "PT8H", "startTime" : "2018-05-15T00:00:00Z" }` für den 15.5.2018 einen Zeitplan für alle Indexer, die in Intervallen von acht Stunden ausgeführt werden.

Zum festgelegten Zeitpunkt beginnen sämtliche Indexer mit der Ausführung, dem Laden von Daten, der Anwendung von Anreicherungen (wenn Sie eine Pipeline für die kognitive Suche konfiguriert haben) und dem Schreiben in den Index. Azure Search sperrt den Index für Updates nicht. Gleichzeitige Schreibvorgänge werden verwaltet und wiederholt, wenn ein bestimmter Schreibvorgang beim ersten Mal nicht erfolgreich ausgeführt werden kann.

> [!Note]
> Wenn Sie die Anzahl der Replikate erhöhen, sollten Sie auch die Anzahl der Partitionen erhöhen, wenn zu erwarten ist, dass der Index deutlich vergrößert wird. Partitionen speichern einzelne Segmente des indizierten Inhalts. Je mehr Partitionen Sie besitzen, desto kleiner sind die einzelnen Segmente, die darin gespeichert werden müssen.

## <a name="see-also"></a>Weitere Informationen

+ [Indexer (Übersicht)](search-indexer-overview.md)
+ [Indizieren im Portal](search-import-data-portal.md)
+ [Azure SQL-Datenbank-Indexer](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
+ [Azure Cosmos DB-Indexer](search-howto-index-cosmosdb.md)
+ [Azure Blob Storage-Indexer](search-howto-indexing-azure-blob-storage.md)
+ [Azure Table Storage-Indexer](search-howto-indexing-azure-tables.md)
+ [Sicherheit in Azure Search](search-security-overview.md)