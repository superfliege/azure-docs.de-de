---
title: Entwerfen von ELT- anstelle von ETL-Prozessen für Azure SQL Data Warehouse | Microsoft-Dokumentation
description: Entwerfen Sie einen Extrahieren, Transformieren und Laden (ETL)- anstelle eines ETL-Prozesses zum Herunterladen von Daten oder für Azure SQL Data Warehouse.
services: sql-data-warehouse
author: ckarst
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: design
ms.date: 04/17/2018
ms.author: cakarst
ms.reviewer: igorstan
ms.openlocfilehash: d004ad1f24448da0c7404761ca0865826b3000b3
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51261280"
---
# <a name="designing-extract-load-and-transform-elt-for-azure-sql-data-warehouse"></a>Entwerfen von ELT-Prozessen für Azure SQL Data Warehouse

Entwerfen Sie einen Extrahieren, Transformieren und Laden (ETL)- anstelle eines Extrahieren, Transformieren und Laden (ETL)-Prozesses zum Herunterladen von Daten in Azure SQL Data Warehouse. In diesem Artikel werden Möglichkeiten zum Entwerfen eines ELT-Prozesses beschrieben, der Daten in ein Azure Data Warehouse verschiebt.

> [!VIDEO https://www.youtube.com/embed/l9-wP7OdhDk]

## <a name="what-is-elt"></a>Was ist ELT?

ELT (Extrahieren, Laden und Transformieren) ist ein Prozess, mit dem Daten aus einem Quellsystem in ein Ziel-Data Warehouse verschoben werden. Dieser Vorgang erfolgt in regelmäßigen Abständen, z. B. stündlich oder täglich, damit neu generierte Daten in das Data Warehouse gelangen. Der ideale Weg, um Daten von der Quelle in das Data Warehouse zu übermitteln, ist die Entwicklung eines ELT-Prozesses, der mithilfe von PolyBase die Daten in SQL Data Warehouse lädt.

Beim ELT-Prozess werden die Daten zunächst geladen und dann transformiert, während beim ETL-Prozess (Extrahieren, Transformieren und Laden) die Daten vor dem Laden transformiert werden. Die Durchführung von ELT anstelle von ETL spart Kosten für die Bereitstellung eigener Ressourcen zum Transformieren der Daten vor dem Laden. Bei der Verwendung von SQL Data Warehouse nutzt ELT das MPP-System, um die Transformationen durchzuführen.

Obwohl es viele Varianten für die Implementierung von ELT für SQL Data Warehouse gibt, sind dies die grundlegenden Schritte:  

1. Extrahieren Sie die Quelldaten in Textdateien.
2. Legen Sie die Daten in Azure Blob Storage oder Azure Data Lake Store ab.
3. Bereiten Sie die Daten für das Laden vor.
2. Laden Sie die Daten mit PolyBase in SQL Data Warehouse-Stagingtabellen.
3. Transformieren Sie die Daten.
4. Fügen Sie die Daten in Produktionstabellen ein.


Ein Tutorial zum Ladevorgang finden Sie unter [Verwenden von PolyBase zum Laden von Daten aus Azure Blob Storage in Azure SQL Data Warehouse](load-data-from-azure-blob-storage-using-polybase.md).

Weitere Informationen finden Sie im Blog [Lademuster](https://blogs.msdn.microsoft.com/sqlcat/2017/05/17/azure-sql-data-warehouse-loading-patterns-and-strategies/). 

## <a name="options-for-loading-with-polybase"></a>Optionen beim Laden mit PolyBase

PolyBase ist eine Technologie, die über die T-SQL-Sprache auf Daten außerhalb der Datenbank zugreift. Dies ist der beste Weg, um Daten in SQL Data Warehouse zu laden. Bei PolyBase werden die Daten parallel von der Datenquelle direkt in die Computeknoten geladen. 

Um Daten mit PolyBase zu laden, können Sie jede dieser Ladeoptionen nutzen.

- [PolyBase mit T-SQL](load-data-from-azure-blob-storage-using-polybase.md) funktioniert gut, wenn sich Ihre Daten in Azure Blob Storage oder Azure Data Lake Store befinden. Es bietet Ihnen die größtmögliche Kontrolle über den Ladevorgang, erfordert aber auch die Definition externer Datenobjekte. Die anderen Methoden definieren diese Objekte im Hintergrund, wenn Sie Quelltabellen zu Zieltabellen zuordnen.  Zum Orchestrieren von T-SQL-Ladevorgängen können Sie Azure Data Factory-, SSIS- oder Azure-Funktionen verwenden. 
- [PolyBase mit SSIS](/sql/integration-services/load-data-to-sql-data-warehouse) funktioniert gut, wenn sich die Quelldaten in SQL Server befinden (entweder lokal in SQL Server oder in der Cloud). SSIS definiert die Zuordnung von Quell- zu Zieltabellen und orchestriert zudem die Workload. Wenn Sie bereits über SSIS-Pakete verfügen, können Sie die Pakete so ändern, dass sie mit dem neuen Data Warehouse-Ziel funktionieren. 
- [PolyBase mit Azure Data Factory (ADF)](sql-data-warehouse-load-with-data-factory.md) ist ein weiteres Orchestrierungstool.  Es definiert eine Pipeline und plant Aufträge. 
- [PolyBase mit Azure DataBricks](../azure-databricks/databricks-extract-load-sql-data-warehouse.md) überträgt Daten von einer SQL Data Warehouse-Tabelle in einen Databricks-Datenrahmen und/oder schreibt Daten aus einem Databricks-Datenrahmen in eine SQL Data Warehouse-Tabelle.

### <a name="polybase-external-file-formats"></a>Externe PolyBase-Dateiformate

PolyBase lädt Daten aus UTF-8- und UTF-16-codierten, durch Trennzeichen getrennten Textdateien. Zusätzlich zu den durch Trennzeichen getrennten Textdateien lädt es aus den Hadoop-Dateiformaten „RC File“, „ORC“ und „Parquet“. PolyBase kann Daten aus Dateien laden, die mit Gzip und Snappy komprimiert wurden. PolyBase unterstützt derzeit kein erweitertes ASCII, keine Formate mit fester Breite und keine geschachtelten Formate wie WinZip, JSON und XML.

### <a name="non-polybase-loading-options"></a>Nicht von PolyBase stammende Ladeoptionen
Wenn Ihre Daten nicht mit PolyBase kompatibel sind, können Sie [bcp](/sql/tools/bcp-utility) oder die [SQLBulkCopy API](https://msdn.microsoft.com/library/system.data.sqlclient.sqlbulkcopy.aspx) verwenden. Mit „bcp“ werden Daten direkt in SQL Data Warehouse geladen, ohne Azure Blob Storage zu durchlaufen. Daher ist es nur für kleine Workloads konzipiert. Beachten Sie, dass die Leistung dieser Optionen beim Laden wesentlich langsamer ist als bei PolyBase. 


## <a name="extract-source-data"></a>Extrahieren von Quelldaten

Das Abrufen von Daten aus dem Quellsystem hängt von der Quelle ab.  Ziel ist es, die Daten in durch Trennzeichen getrennte Textdateien zu verschieben. Wenn Sie SQL Server verwenden, können Sie die Daten mit dem [Befehlszeilentool „bcp“](/sql/tools/bcp-utility) exportieren.  

## <a name="land-data-to-azure-storage"></a>Verschieben von Daten in Azure Storage

Um die Daten in Azure Storage zu verschieben, können Sie sie in [Azure Blob Storage](../storage/blobs/storage-blobs-introduction.md) oder [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md) verschieben. In beiden Fällen sollten die Daten in Textdateien gespeichert werden. PolyBase kann die Daten von beiden Speicherorten laden.

Dies sind Tools und Dienste, mit denen Sie Daten in Azure Storage verschieben können.

- Der [Azure ExpressRoute](../expressroute/expressroute-introduction.md)-Dienst verbessert Netzwerkdurchsatz, Leistung und Vorhersagbarkeit. ExpressRoute ist ein Dienst, der Ihre Daten über eine dedizierte private Verbindung zu Azure weiterleitet. Bei ExpressRoute-Verbindungen werden Daten nicht über das öffentliche Internet weitergeleitet. Die Verbindungen bieten mehr Zuverlässigkeit, eine höhere Geschwindigkeit, niedrigere Latenzzeiten und mehr Sicherheit als herkömmliche Verbindungen über das öffentliche Internet.
- Das Hilfsprogramm [AZCopy](../storage/common/storage-moving-data.md) verschiebt Daten über das öffentliche Internet in Azure Storage. Dies funktioniert, wenn Ihre Datenmengen weniger als 10 TB umfassen. Wenn Sie Ladevorgänge in regelmäßigen Abständen mit AZCopy ausführen möchten, testen Sie die Netzwerkgeschwindigkeit, um festzustellen, ob sie geeignet ist. 
- [Azure Data Factory (ADF)](../data-factory/introduction.md) verfügt über ein Gateway, das Sie auf dem lokalen Server installieren können. Anschließend können Sie eine Pipeline erstellen, um Daten vom lokalen Server in Azure Storage zu verschieben. Informationen zur Verwendung der Data Factory mit SQL Data Warehouse finden Sie unter [Laden von Daten in Azure SQL Data Warehouse mit Azure Data Factory](/azure/data-factory/load-azure-sql-data-warehouse).

## <a name="prepare-data"></a>Vorbereiten von Daten

Möglicherweise müssen Sie die Daten in Ihrem Speicherkonto vorbereiten und bereinigen, bevor Sie sie in SQL Data Warehouse laden. Die Datenvorbereitung kann durchgeführt werden, während sich Ihre Daten in der Quelle befinden, während Sie die Daten in Textdateien exportieren oder nachdem sich die Daten in Azure Storage befinden.  Am einfachsten ist es, so früh wie möglich im Prozess mit den Daten zu arbeiten.  

### <a name="define-external-tables"></a>Definieren externer Tabellen
Bevor Sie Daten laden können, müssen Sie externe Tabellen im Data Warehouse definieren. PolyBase verwendet externe Tabellen, um Daten in Azure Storage zu definieren und auf diese zuzugreifen. Die externe Tabelle ist vergleichbar mit einer regulären Tabelle. Der Hauptunterschied besteht darin, dass die externe Tabelle auf Daten verweist, die außerhalb des Data Warehouse gespeichert sind. 

Die Definition externer Tabellen umfasst die Angabe der Datenquelle, des Formats der Textdateien und der Tabellendefinitionen. Dies sind die Themen der T-SQL-Syntax, die Sie benötigen:
- [CREATE EXTERNAL DATA SOURCE](/sql/t-sql/statements/create-external-data-source-transact-sql)
- [CREATE EXTERNAL FILE FORMAT](/sql/t-sql/statements/create-external-file-format-transact-sql)
- [CREATE EXTERNAL TABLE](/sql/t-sql/statements/create-external-table-transact-sql)

Ein Beispiel für die Erstellung externer Objekte finden Sie im Schritt [Erstellen externer Tabellen](load-data-from-azure-blob-storage-using-polybase.md#create-external-tables-for-the-sample-data) im Tutorial zu Ladevorgängen.

### <a name="format-text-files"></a>Formatieren von Textdateien

Nachdem die externen Objekte definiert sind, müssen Sie die Zeilen der Textdateien mit der Definition der externen Tabelle und des Dateiformats abgleichen. Die Daten in den einzelnen Zeilen der Textdatei müssen mit der Tabellendefinition übereinstimmen.

So formatieren Sie die Textdateien

- Wenn Ihre Daten aus einer nicht relationalen Quelle stammen, müssen Sie sie in Zeilen und Spalten transformieren. Unabhängig davon, ob die Daten aus einer relationalen oder nicht relationalen Quelle stammen, müssen die Daten so transformiert werden, dass sie mit den Spaltendefinitionen der Tabelle übereinstimmen, in die die Daten geladen werden sollen. 
- Formatieren Sie die Daten in der Textdatei so, dass sie mit den Spalten und Datentypen in der SQL Data Warehouse-Zieltabelle übereinstimmen. Eine Nichtübereinstimmung zwischen den Datentypen in den externen Textdateien und der Data Warehouse-Tabelle führt dazu, dass Zeilen beim Laden zurückgewiesen werden.
- Trennen Sie Felder in der Textdatei mit einem Abschlusszeichen.  Stellen Sie sicher, dass Sie ein Zeichen oder eine Zeichenfolge verwenden, die nicht in Ihren Quelldaten enthalten ist. Verwenden Sie das durch [CREATE EXTERNAL FILE FORMAT](/sql/t-sql/statements/create-external-file-format-transact-sql) angegebene Abschlusszeichen.

## <a name="load-to-a-staging-table"></a>Laden in eine Stagingtabelle
Um Daten in das Data Warehouse zu übertragen, ist es hilfreich, die Daten zunächst in eine Stagingtabelle zu laden. Durch die Verwendung einer Stagingtabelle können Sie Fehler behandeln, ohne die Produktionstabellen zu stören. Zudem vermeiden Sie Rollbackvorgänge für die Produktionstabelle. Eine Stagingtabelle bietet Ihnen außerdem die Möglichkeit, Transformationen mit SQL Data Warehouse auszuführen, bevor Daten in Produktionstabellen eingefügt werden.

Führen Sie die T-SQL-Anweisung [CREATE TABLE AS SELECT (CTAS)](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) aus, um mit T-SQL zu laden. Dieser Befehl fügt die Ergebnisse einer SELECT-Anweisung in eine neue Tabelle ein. Wenn die Auswahl über eine externe Tabelle erfolgt, werden die externen Daten importiert. 

Im folgenden Beispiel ist „ext.Date“ eine externe Tabelle. Alle Zeilen werden in eine neue Tabelle namens „dbo.Date“ importiert.

```sql
CREATE TABLE [dbo].[Date]
WITH
( 
    CLUSTERED COLUMNSTORE INDEX
)
AS SELECT * FROM [ext].[Date]
;
```

## <a name="transform-the-data"></a>Transformieren der Daten
Führen Sie während der Bereitstellung der Daten in der Stagingtabelle entsprechende Transformationen durch, die für Ihre Workload erforderlich sind. Anschließend verschieben Sie die Daten in eine Produktionstabelle.

## <a name="insert-data-into-production-table"></a>Einfügen von Daten in die Produktionstabelle

Die SELECT-Anweisung „INSERT INTO...“ verschiebt die Daten aus der Stagingtabelle in die permanente Tabelle. 

Versuchen Sie beim Entwerfen eines ETL-Prozesses, den Prozess für ein kleines Testbeispiel auszuführen. Versuchen Sie, 1000 Zeilen aus der Tabelle in eine Datei zu extrahieren, sie nach Azure zu verschieben und sie dann in eine Stagingtabelle zu laden. 

## <a name="partner-loading-solutions"></a>Ladelösungen von Partnern
Viele unserer Partner stellen Ladelösungen bereit. Weitere Informationen finden Sie in der Liste mit unseren [Lösungspartnern](sql-data-warehouse-partner-business-intelligence.md). 

## <a name="next-steps"></a>Nächste Schritte
Eine Anleitung zum Laden finden Sie unter [Anleitung zum Laden von Daten](guidance-for-loading-data.md).


