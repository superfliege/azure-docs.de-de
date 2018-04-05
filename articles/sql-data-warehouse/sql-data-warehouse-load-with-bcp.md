---
title: Verwenden von bcp zum Laden von Daten in SQL Data Warehouse | Microsoft Docs
description: Erfahren Sie, was bcp ist und wie es in Data Warehouse-Szenarios verwendet wird.
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: barbkess
editor: ''
ms.assetid: f9467d11-fcd6-4131-a65a-2022d2c32d24
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 01/22/2018
ms.author: cakarst;barbkess
ms.openlocfilehash: 752a2f0d7b49f9e053788b26e3df389246c72a73
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="load-data-with-bcp"></a>Laden von Daten mit bcp

**[bcp](/sql/tools/bcp-utility.md)** ist ein Befehlszeilen-Dienstprogramm zum Massenladen, das es Ihnen ermöglicht, Daten zwischen SQL Server, Datendateien und SQL Data Warehouse zu kopieren. Verwenden sie bcp zum Importieren großer Mengen an neuen Zeilen in SQL Data Warehouse-Tabellen oder zum Exportieren von Daten aus SQL Server-Tabellen in Datendateien. bcp erfordert außer bei Verwendung mit der queryout-Option keine Kenntnisse von Transact-SQL.

bcp bietet eine schnelle und einfache Möglichkeit, um kleinere DataSets in und aus einer SQL Data Warehouse-Datenbank zu verschieben. Der genaue Betrag der Daten, der für das Laden/Extrahieren per bcp empfohlen wird, richtet sich nach der Netzwerkverbindung mit Azure.  Kleine Dimensionstabellen können mit bcp problemlos geladen und extrahiert werden. Nicht bcp, sondern PolyBase, ist aber das empfohlene Tool zum Laden und Extrahieren großer Datenvolumen. PolyBase ist für die Architektur von SQL Data Warehouse mit paralleler Massenverarbeitung (Massively Parallel Processing, MPP) konzipiert.

Mit bcp haben Sie folgende Möglichkeiten:

* Verwenden eines Befehlszeilenprogramms zum Laden von Daten in SQL Data Warehouse
* Verwenden eines Befehlszeilenprogramms zum Extrahieren von Daten aus SQL Data Warehouse

In diesem Tutorial wird Folgendes durchgeführt:

* Importieren von Daten in eine Tabelle mithilfe des Befehls „bcp in“
* Exportieren von Daten aus einer Tabelle mithilfe des Befehls „bcp out“

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Loading-data-into-Azure-SQL-Data-Warehouse-with-BCP/player]
> 
> 

## <a name="prerequisites"></a>Voraussetzungen
Für dieses Lernprogramm ist Folgendes erforderlich:

* Eine SQL Data Warehouse-Datenbank
* Befehlszeilenprogramme bcp und sqlcmd Sie können diese Programme aus dem [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=36433) herunterladen. 

### <a name="data-in-ascii-or-utf-16-format"></a>Daten im ASCII- oder UTF-16-Format
Wenn Sie in diesem Tutorial Ihre eigenen Daten verwenden möchten, müssen die Daten in ASCII- oder UTF-16-Codierung vorlegen, da UTF-8 von bcp nicht unterstützt wird. 

PolyBase unterstützt zwar UTF-8, aber noch kein UTF-16. Zum Verwenden von bcp für den Datenexport und von PolyBase zum Laden von Daten müssen Sie die Daten in das Format UTF-8 transformieren, nachdem sie aus SQL Server exportiert wurden. 

## <a name="import-data-into-sql-data-warehouse"></a>Importieren von Daten in SQL Data Warehouse
In diesem Lernprogramm erstellen Sie eine Tabelle in Azure SQL Data Warehouse und importieren Daten in die Tabelle.

### <a name="step-1-create-a-table-in-azure-sql-data-warehouse"></a>Schritt 1: Erstellen einer Tabelle in Azure SQL Data Warehouse
Verwenden Sie sqlcmd von einer Befehlszeile aus zum Ausführen der folgenden Abfrage, um eine Tabelle für Ihre Instanz zu erstellen:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    CREATE TABLE DimDate2
    (
        DateId INT NOT NULL,
        CalendarQuarter TINYINT NOT NULL,
        FiscalQuarter TINYINT NOT NULL
    )
    WITH
    (
        CLUSTERED COLUMNSTORE INDEX,
        DISTRIBUTION = ROUND_ROBIN
    );
"
```

Weitere Informationen zur Erstellung einer Tabelle finden Sie unter [Tabellenübersicht](sql-data-warehouse-tables-overview.md) oder in der [CREATE TABLE](/sql/t-sql/statements/create-table-azure-sql-data-warehouse.md)-Syntax.
 

### <a name="step-2-create-a-source-data-file"></a>Schritt 2: Erstellen einer Quelldatendatei
Öffnen Sie Editor, kopieren Sie die folgenden Datenzeilen in eine neue Textdatei, und speichern Sie diese Datei im lokalen temporären Verzeichnis C:\Temp\DimDate2.txt.

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

> [!NOTE]
> Es ist wichtig, daran zu denken, dass bcp.exe die UTF-8-Codierung nicht unterstützt. Verwenden Sie ASCII-Dateien oder UTF-16-codierte Dateien, wenn Sie „bcp.exe“ nutzen.
> 
> 

### <a name="step-3-connect-and-import-the-data"></a>Schritt 3: Herstellen einer Verbindung und Importieren von Daten
Sie können mit bcp eine Verbindung herstellen und die Daten mit dem folgenden Befehl importieren (ersetzen Sie die Werte nach Bedarf):

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t  ','
```

Sie können prüfen, ob die Daten geladen wurden, indem Sie mithilfe von sqlcmd die folgende Abfrage ausführen:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

Für diese Abfrage sollten die folgenden Ergebnisse zurückgegeben werden:

| DateId | CalendarQuarter | FiscalQuarter |
| --- | --- | --- |
| 20150101 |1 |3 |
| 20150201 |1 |3 |
| 20150301 |1 |3 |
| 20150401 |2 |4 |
| 20150501 |2 |4 |
| 20150601 |2 |4 |
| 20150701 |3 |1 |
| 20150801 |3 |1 |
| 20150801 |3 |1 |
| 20151001 |4 |2 |
| 20151101 |4 |2 |
| 20151201 |4 |2 |

### <a name="step-4-create-statistics-on-your-newly-loaded-data"></a>Schritt 4: Erstellen von Statistiken für die neu geladenen Daten
Nach dem Laden der Daten werden im letzten Schritt Statistiken erstellt bzw. aktualisiert. Hierdurch wird die Abfrageleistung verbessert. Weitere Informationen finden Sie unter [Verwalten von Statistiken für Tabellen in SQL Data Warehouse](sql-data-warehouse-tables-statistics.md). Im folgenden sqlcmd-Beispiel werden Statistiken für die Tabelle erstellt, in der die neu geladenen Daten enthalten sind.


```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="export-data-from-sql-data-warehouse"></a>Exportieren von Daten aus SQL Data Warehouse
In diesem Tutorial wird eine Datendatei aus einer Tabelle in SQL Data Warehouse erstellt. Hierbei werden die Daten exportiert, die Sie im vorherigen Abschnitt importiert haben. Die Ergebnisse werden in der Datei „DimDate2_export.txt“ gespeichert.

### <a name="step-1-export-the-data"></a>Schritt 1: Exportieren der Daten
Sie können mit dem Dienstprogramm bcp eine Verbindung herstellen und die Daten mit dem folgenden Befehl exportieren (ersetzen Sie die Werte nach Bedarf):

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
Sie können überprüfen, ob die Daten ordnungsgemäß exportiert wurden, indem Sie die neue Datei öffnen. Die Daten in der Datei sollten mit dem folgenden Text übereinstimmen:

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

> [!NOTE]
> Aufgrund der Struktur verteilter Systeme unterscheidet sich die Reihenfolge der Daten in den SQL Data Warehouse-Datenbanken möglicherweise. Eine andere Möglichkeit ist die Verwendung der Funktion **queryout** von bcp, um einen Auszug der Abfrage zu schreiben, und nicht die gesamte Tabelle zu exportieren.
> 
> 

## <a name="next-steps"></a>Nächste Schritte
Anweisungen zum Entwerfen Ihres Ladeprozesses finden Sie unter [Bewährte Methoden zum Laden von Daten in Azure SQL Data Warehouse](guidance-for-loading-data.md).  



<!--MSDN references-->



<!--Other Web references-->
[Microsoft Download Center]: https://www.microsoft.com/download/details.aspx?id=36433
