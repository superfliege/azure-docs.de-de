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
ms.openlocfilehash: f38054eaf2829149a496f840366b6f2f9e03e12b
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
ms.locfileid: "33942102"
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
| Ändern eines Feldnamens, Datentyps oder der [Indexattribute](https://docs.microsoft.com/rest/api/searchservice/create-index) | Wenn eine Felddefinition geändert wird, entstehen in der Regel Einbußen bei der Neuerstellung, mit Ausnahme der folgenden [Indexattribute](https://docs.microsoft.com/rest/api/searchservice/create-index): Retrievable, SearchAnalyzer, SynonymMaps. Sie können die Retrievable-, SearchAnalyzer- und SynonymMaps-Attribute zu einem bereits vorhandenen Feld hinzufügen und müssen keinen Index neu erstellen.|
| Hinzufügen eines Felds | Es ist nicht zwingend eine Neuerstellung erforderlich. Bereits vorhandenen indizierten Dokumenten wird ein NULL-Wert für das neue Feld hinzugefügt. Bei einer späteren Neuindizierung ersetzen Werte aus Quelldaten die von Azure Search hinzugefügten NULL-Werte. |
| Löschen eines Felds | Sie können ein Feld nicht direkt über einen Azure Search-Index löschen. Stattdessen sollte Ihre Anwendung das Feld „Gelöscht“ ignorieren, damit es nicht mehr verwendet wird. Die Felddefinition und die Inhalte bleiben im Index, bis Sie Ihren Index das nächste Mal unter Verwendung eines Schemas neu erstellen, das das betreffende Feld auslässt.|

> [!Note]
> Außerdem ist eine Neuerstellung erforderlich, wenn Sie die Schicht wechseln. Wenn Sie sich gleichzeitig für mehr Kapazitäten entscheiden, gibt es kein direktes Upgrade. Es muss am neuen Kapazitätspunkt ein neuer Dienst erstellt werden, und Indizes müssen von Grund auf für den neuen Dienst erstellt werden. 

## <a name="partial-or-incremental-indexing"></a>Teilindizierung oder inkrementelle Indizierung

Sobald ein Index hergestellt wird, ändert sich der Fokus auf die inkrementelle Indizierung. Dabei entstehen in der Regel keine wahrnehmbaren Dienstausfälle. Bei der Teilindizierung oder der inkrementellen Indizierung handelt es sich um eine Workload, die sich nur auf den Inhalt bezieht und den Inhalt des Suchindex synchronisiert, um den Zustand des Inhalts in einer zugehörigen Datenquelle zu reflektieren. Ein Dokument, das der Quelle hinzugefügt oder aus ihr entfernt wird, wird auch dem Index hinzugefügt bzw. aus ihm entfernt. Rufen Sie im Code den Vorgang [Hinzufügen, Aktualisieren oder Löschen von Dokumenten](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) oder das entsprechende .NET-Äquivalent auf.

> [!Note]
> Wenn Sie Indexer verwenden, die externe Datenquellen auffüllen, werden Mechanismen für die Änderungsnachverfolgung in Quellsystemen für die inkrementelle Indizierung genutzt. Für den [Azure Blob-Speicher](search-howto-indexing-azure-blob-storage.md#incremental-indexing-and-deletion-detection) wird ein `lastModified`-Feld verwendet. Im [Azure-Tabellenspeicher](search-howto-indexing-azure-tables.md#incremental-indexing-and-deletion-detection) dient `timestamp` demselben Zweck. Genauso weisen sowohl der [Azure SQL-Datenbank-Indexer](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md#capture-new-changed-and-deleted-rows) als auch der [Azure Cosmos DB-Indexer](search-howto-index-cosmosdb.md#indexing-changed-documents) Felder zum Kennzeichnen von Zeilenupdates auf. Weitere Informationen zu Indexern finden Sie in der [Indexer-Übersicht](search-indexer-overview.md).

## <a name="scale-out-indexing"></a>Indizierung für die horizontale Skalierung

Wenn die Datenvolumen wachsen oder die Verarbeitung geändert werden muss, stellen Sie möglicherweise fest, dass die einfache Neuerstellung und die erneute Indizierung von Vorgängen nicht ausreichend sind. Wenn Sie dem höheren Bedarf nachkommen wollen, empfehlen wir als ersten Schritt eine Erweiterung der [Staffelung und Kapazität](search-capacity-planning.md) innerhalb der Grenzen des vorhanden Diensts. 

Wenn Sie [Indexer](search-indexer-overview.md) verwenden können, werden zusätzliche Mechanismen zur horizontalen Skalierung verfügbar gemacht. Indexer umfassen einen integrierten Scheduler, mit dem Sie die Indizierung in regelmäßigen Intervallen gliedern oder die Verarbeitung über den 24-Stunden-Rhythmus hinaus erweitern können. Außerdem können Indexer verwendet werden, um ein gewisses Maß an Parallelismus zu erreichen, wenn sie an Definitionen von Datenquellen gekoppelt werden, indem Daten partitioniert und Zeitpläne verwendet werden, um eine parallele Ausführung zu ermöglichen.

### <a name="scheduled-indexing-for-large-data-sets"></a>Geplante Indizierung für große Datasets

Zeitpläne stellen einen wichtigen Mechanismus zum Verarbeiten von großen Datasets und zum langsamen Ausführen von Analysen wie der Bildanalyse in einer Pipeline für die kognitive Suche dar. Die Indexerverarbeitung wird innerhalb von 24 Stunden ausgeführt. Wenn die Verarbeitung nicht innerhalb von 24 Stunden abgeschlossen ist, können Sie die Verhaltensweisen für die Indexerzeitplanung zu Ihrem Vorteil nutzen. 

Standardmäßig startet die geplante Indizierung zu bestimmten Zeitintervallen. Dabei wird ein Vorgang in der Regel abgeschlossen, bevor die Indizierung zum nächsten geplanten Intervall fortgesetzt wird. Wenn der Vorgang jedoch nicht innerhalb des Intervalls abgeschlossen wird, wird der Indexer beendet, da ein Timeout auftritt. Beim nächsten Intervall wird die Verarbeitung an der Stelle fortgesetzt, an der sie während des letzten Intervalls unterbrochen wurde. Dabei verfolgt das System nach, an welcher Stelle dies der Fall ist. 

Das heißt in der Praxis, dass Sie für Indexladungen, die sich über mehrere Tage erstrecken, einen 24-Stunden-Zeitplan für den Indexer festlegen können. Wenn eine Indizierung für den nächsten 24-Stunden-Zeitraum fortgesetzt wird, startet diese bei dem letzten erfolgreich verarbeiteten Dokument neu. Auf diese Weise kann sich ein Indexer über mehrere Tage hinweg durch das Dokumentbacklog durcharbeiten, bis alle nicht verarbeitete Dokumente verarbeitet wurden. Weitere Informationen zu diesem Ansatz finden Sie unter [Indizieren großer Datasets](search-howto-indexing-azure-blob-storage.md#indexing-large-datasets).

<a name="parallel-indexing"></a>

### <a name="parallel-indexing"></a>Parallele Indizierung

Sie können stattdessen auch eine Strategie zur parallelen Indizierung einrichten. Für außergewöhnliche, rechenintensive Anforderungen für die Indizierung (z.B. OCR auf gescannten Dokumenten in einer Pipeline für die kognitive Suche) eignet sich ggf. die Strategie der parallelen Indizierung für dieses spezifische Ziel. In einer Pipeline für die Anreicherung der kognitiven Suche wird die Verarbeitung der Bildanalyse und der natürlichen Sprache nicht mehr ausgeführt. Die parallele Indizierung für einen Dienst, der zum selben Zeitpunkt keine Abfrageanforderungen verarbeitet, könnte sich besonders die Option für die Arbeit durch große Textmengen von Inhalt eignen, der langsam verarbeitet wird. 

Eine Strategie für die parallele Verarbeitung verfügt über die folgenden Elemente:

+ Verteilen Sie Ihre Quelldaten auf mehrere Container oder mehrere virtuelle Ordner innerhalb desselben Containers. 
+ Ordnen Sie jedes noch so kleine Dataset einer [Datenquelle](https://docs.microsoft.com/rest/api/searchservice/create-data-source) zu, die an einen eignen [Indexer](https://docs.microsoft.com/rest/api/searchservice/create-indexer) gekoppelt ist.
+ Verweisen Sie für die kognitive Suche in jeder Indexerdefinition auf dieselbe [Fähigkeitsgruppe](ref-create-skillset.md).
+ Schreiben Sie in denselben Suchindex für das Ziel. 
+ Legen Sie für die Ausführung aller Indexer denselben Zeitpunkt fest.

> [!Note]
> Azure Search unterstützt das Dedizieren von Replikaten oder Partitionen auf bestimmte Workloads nicht. Es besteht ein erhöhtes Risiko, dass die gleichzeitige Indizierung Ihr System so belastet, dass die Abfrageleistung beeinträchtigt wird. Wenn Sie über eine Testumgebung verfügen, implementieren Sie die parallele Indizierung dort zuerst, um die Vor- und Nachteile nachvollziehen zu können.

### <a name="configure-parallel-indexing"></a>Konfigurieren der parallelen Indizierung

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