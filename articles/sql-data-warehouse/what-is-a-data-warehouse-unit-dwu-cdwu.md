---
title: Was sind Data Warehouse-Einheiten (DWUs, cDWUs) in Azure SQL Data Warehouse? | Microsoft Docs
description: "Funktionen zur horizontalen Leistungshochskalierung in Azure SQL Data Warehouse. Führen Sie eine horizontale Skalierung durch, indem Sie DWUs/cDWUs anpassen oder Computeressourcen anhalten und fortsetzen, um Kosten zu sparen."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.assetid: e13a82b0-abfe-429f-ac3c-f2b6789a70c6
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 11/10/2017
ms.author: jrj;barbkess
ms.openlocfilehash: 02998c48dcab5d3ed191b168665c9e47bbfbd232
ms.sourcegitcommit: 659cc0ace5d3b996e7e8608cfa4991dcac3ea129
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2017
---
# <a name="data-warehouse-units-dwus-and-compute-data-warehouse-units-cdwus"></a>Data Warehouse-Einheiten (DWUs) und Compute Data Warehouse-Einheiten (cDWUs)
Erläutert Data Warehouse-Einheiten (DWUs) und Compute Data Warehouse-Einheiten (cDWUS) für Azure SQL Data Warehouse. Enthält Empfehlungen zum Auswählen der idealen Anzahl von Data Warehouse-Einheiten sowie zum Ändern der Anzahl. 

## <a name="what-are-data-warehouse-units"></a>Was sind Data Warehouse-Einheiten?
Durch SQL Data Warehouse werden CPU, Arbeitsspeicher und E/A in Einheiten von Computeskalierungen gebündelt, die als Data Warehouse-Einheiten (DWUs) bezeichnet werden. Eine DWU stellt ein abstraktes, normalisiertes Maß für Computeressourcen und -leistung dar. Durch Ändern der Dienstebene ändern Sie die Anzahl der DWUs, die dem System zugewiesen werden. Dies wiederum passt die Leistung und die Kosten Ihres Systems an. 

Wenn Sie für höhere Leistung bezahlen möchten, können Sie die Anzahl der Data Warehouse-Einheiten erhöhen. Um für weniger Leistung zu bezahlen, verringern Sie die Anzahl der Data Warehouse-Einheiten. Speicher- und Computekosten werden separat in Rechnung gestellt. Das Ändern der Data Warehouse-Einheiten wirkt sich daher nicht auf die Speicherkosten aus.

Die Leistung für Data Warehouse-Einheiten basiert auf den folgenden Data Warehouse-Workloadmetriken:

- Wie schnell kann eine Data Warehousing-Standardabfrage eine große Anzahl von Zeilen überprüfen und anschließend eine komplexe Aggregation ausführen? Dies ist ein Vorgang mit hohem E/A- und CPU-Aufwand.
- Wie schnell kann das Data Warehouse Daten aus Azure Storage Blob oder Azure Data Lake aufnehmen? Dies ist ein Vorgang mit hohem Netzwerk- und CPU-Aufwand. 
- Wie schnell kann der T-SQL-Befehl [CREATE TABLE AS SELECT](https://docs.microsoft.com/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) eine Tabelle kopieren? Dieser Vorgang umfasst das Lesen von Daten aus dem Speicher, das Verteilen auf die Knoten der Anwendung und das erneute Schreiben in den Speicher. Dies ist ein Vorgang mit hohem CPU-, E/A- und Netzwerkaufwand.

Erhöhen der Anzahl der DWUs:
- Lineare Änderung der Systemleistung bei Scans, Aggregationen und CTAS-Anweisungen
- Erhöhen der Anzahl von Readern und Writern für PolyBase-Ladevorgänge
- Erhöht die maximale Anzahl von gleichzeitigen Abfragen und Parallelitätsslots.

## <a name="performance-tiers-and-data-warehouse-units"></a>Leistungsstufen und Data Warehouse-Einheiten

Jede Leistungsstufe verwendet eine etwas andere Maßeinheit für ihre Data Warehouse-Einheiten. Dieser Unterschied wird in der Rechnung berücksichtigt, weil die Skalierungseinheit direkt in eine Abrechnung übersetzt wird.

- Die für die Elastizitätsleistung optimierte Stufe wird in Data Warehouse-Einheiten (DWUs) gemessen.
- Die für die Computeleistung optimierte Stufe wird in Compute Data Warehouse-Einheiten (cDWUs) gemessen. 

Sowohl DWUs als auch cDWUs unterstützen das zentrale Herunterskalieren und Hochskalieren sowie Computepausen, wenn Sie das Data Warehouse nicht nutzen müssen. Diese Vorgänge werden alle bei Bedarf ausgeführt. Die für die Computeleistung optimierte Stufe verwendet auch einen lokalen datenträgerbasierten Cache auf den Computeknoten, um die Leistung zu verbessern. Wenn Sie das System skalieren oder anhalten, wird der Cache für ungültig erklärt. Daher ist eine „Aufwärmphase“ des Caches erforderlich, bevor eine optimale Leistung erzielt wird.  

Wenn Sie Data Warehouse-Einheiten erhöhen, erhöhen Sie die Computingressourcen linear. Die für die Computeleistung optimierte Stufe bietet die beste Abfrageleistung und die höchste Skalierung, weist jedoch einen höheren Einstiegspreis auf. Sie ist für Unternehmen gedacht, die eine konstante Leistungsanforderung haben. Diese Systeme nutzen den Cache am meisten. 

### <a name="capacity-limits"></a>Kapazitätsgrenzen
Jeder SQL-Server (z.B. myserver.database.windows.net) weist ein Kontingent für [DTUs (Database Transaction Unit, Datenübertragungseinheiten)](../sql-database/sql-database-what-is-a-dtu.md) auf, das eine bestimmte Anzahl von Data Warehouse-Einheiten zulässt. Weitere Informationen finden Sie in den [Kapazitätsgrenzen für die Workloadverwaltung](sql-data-warehouse-service-capacity-limits.md#workload-management).


## <a name="how-many-data-warehouse-units-do-i-need"></a>Wie viele Data Warehouse-Einheiten benötige ich?
Die optimale Anzahl der Data Warehouse-Einheiten hängt in hohem Maß von Ihrer Arbeitsauslastung und der Datenmenge ab, die Sie in das System geladen haben.

Schritte zum Ermitteln der besten DWU-Anzahl für Ihre Arbeitsauslastung:

1. Beginnen Sie bei der Entwicklung zunächst mit der Auswahl einer kleineren DWU unter Verwendung der für Elastizität optimierten Leistungsstufe.  Da die Aufgabe in dieser Phase die funktionale Überprüfung ist, stellt die für Elastizität optimierte Leistungsstufe eine sinnvolle Option dar. Ein guter Ausgangspunkt ist DW200. 
2. Überwachen Sie die Anwendungsleistung, wenn Sie Datenmengen im System testen, und beobachten Sie dabei die Anzahl der ausgewählten DWUs im Vergleich zur beobachteten Leistung.
3. Identifizieren Sie zusätzliche Anforderungen für periodische Zeiten mit Spitzenauslastung. Wenn die Arbeitsauslastung starke Aktivitätsschwankungen zeigt und es einen guten Grund gibt, häufige Skalierungen auszuführen, sollten Sie sich für die für Elastizität optimierte Leistungsstufe entscheiden.
4. Wenn Sie mehr als 1.000 DWUs benötigen, sollten Sie die für Computeleistung optimierte Leistungsstufe bevorzugen, da diese die beste Leistung bereitstellt.

SQL Data Warehouse ist ein horizontal hochskalierbares System, das große Mengen von Compute- und Abfragedaten bereitstellen kann. Wenn Sie seine wahren Skalierungsfunktionen (insbesondere bei größeren DWUs) in Aktion sehen möchten, wird empfohlen, bei der Skalierung das Dataset zu skalieren, um sicherzustellen, dass genügend Daten zum Übertragen an die CPUs verfügbar sind. Es wird empfohlen, für die Skalierungstests mindestens 1 TB zu verwenden.

> [!NOTE]
>
> Die Abfrageleistung steigt bei höherem Parallelisierungsgrad nur, wenn die Verarbeitung auf mehrere Computeknoten aufgeteilt werden kann. Wenn Sie feststellen, dass die Skalierung Ihre Leistung nicht ändert, müssen Sie möglicherweise Ihr Tabellendesign und/oder Ihre Abfragen anpassen. Anleitungen zur Abfrageoptimierung finden Sie in den folgenden Artikeln zur [Leistung](sql-data-warehouse-overview-manage-user-queries.md). 

## <a name="permissions"></a>Berechtigungen

Zum Ändern der Data Warehouse-Einheiten sind die unter [ALTER DATABASE][ALTER DATABASE] beschriebenen Berechtigungen erforderlich. 

## <a name="view-current-dwu-settings"></a>Anzeigen der aktuellen DWU-Einstellungen

So zeigen Sie die aktuellen DWU-Einstellungen an:

1. Öffnen Sie den Objekt-Explorer von SQL Server in Visual Studio.
2. Stellen Sie eine Verbindung mit der Masterdatenbank mit dem logischen SQL-Datenbankserver her.
3. Treffen Sie eine Auswahl aus der dynamischen Verwaltungssicht „sys.database_service_objectives“. Beispiel: 

```sql
SELECT  db.name [Database]
,       ds.edition [Edition]
,       ds.service_objective [Service Objective]
FROM    sys.database_service_objectives   AS ds
JOIN    sys.databases                     AS db ON ds.database_id = db.database_id
;
```

## <a name="change-data-warehouse-units"></a>Ändern von Data Warehouse-Einheiten

### <a name="azure-portal"></a>Azure-Portal
So ändern Sie DWUs oder cDWUs:

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com), öffnen Sie Ihre Datenbank, und klicken Sie dann auf **Skalieren**.

2. Verschieben Sie unter **Skalieren** den Schieberegler nach links oder rechts, um die DWU-Einstellung zu ändern.

3. Klicken Sie auf **Speichern**. Eine Bestätigungsmeldung wird angezeigt. Klicken Sie zur Bestätigung auf **Ja** oder zum Abbrechen auf **Nein**.

### <a name="powershell"></a>PowerShell
Um die DWUs oder cDWUs zu ändern, verwenden Sie das PowerShell-Cmdlet [Set-AzureRmSqlDatabase][Set-AzureRmSqlDatabase]. Im folgenden Beispiel wird das Servicelevelziel für die Datenbank „MySQLDW“, die auf dem Server „MyServer“ gehostet wird, auf „DW1000“ festgelegt.

```Powershell
Set-AzureRmSqlDatabase -DatabaseName "MySQLDW" -ServerName "MyServer" -RequestedServiceObjectiveName "DW1000"
```

### <a name="t-sql"></a>T-SQL
Mit T-SQL können Sie die aktuellen DWU- oder cDWU-Einstellungen anzeigen, die Einstellungen ändern und den Fortschritt überprüfen. 

So ändern Sie DWUs oder cDWUs:

1. Stellen Sie eine Verbindung mit der Masterdatenbank mit Ihrem logischen SQL-Datenbankserver her.
2. Verwenden Sie die T-SQL-Anweisung [ALTER DATABASE][ALTER DATABASE]. Im folgenden Beispiel wird das Servicelevelziel für die Datenbank „MySQLDW“ auf „DW1000“ gesetzt. 

```Sql
ALTER DATABASE MySQLDW
MODIFY (SERVICE_OBJECTIVE = 'DW1000')
;
```

### <a name="rest-apis"></a>REST-APIs

Um die DWUs zu ändern, verwenden Sie die REST-API zum [Erstellen oder Aktualisieren einer Datenbank][Erstellen oder Aktualisieren einer Datenbank]. Im folgenden Beispiel wird der Servicelevel-Zielpunkt für die Datenbank „MySQLDW“, die auf dem Server „MyServer“ gehostet wird, auf „DW1000“ gesetzt. Der Server befindet sich in einer Azure-Ressourcengruppe namens „ResourceGroup1“.

```
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}?api-version=2014-04-01-preview HTTP/1.1
Content-Type: application/json; charset=UTF-8

{
    "properties": {
        "requestedServiceObjectiveName": DW1000
    }
}
```


## <a name="check-status-of-dwu-changes"></a>Überprüfen des Status von DWU-Änderungen

Es kann mehrere Minuten in Anspruch nehmen, bis DWU-Änderungen abgeschlossen werden. Wenn Sie die Skalierung automatisch ausführen, empfiehlt es sich, eine Logik zu implementieren, die sicherstellt, dass bestimmte Vorgänge abgeschlossen wurden, bevor mit einer andern Aktion fortgefahren wird. 

Überprüfen Sie den Datenbankzustand über verschiedene Endpunkte, um sicherzugehen, dass die Automatisierung ordnungsgemäß implementiert wird. Über das Portal erhalten Sie bei Abschluss eines Vorgangs eine Benachrichtigung sowie Informationen zum aktuellen Zustand der Datenbanken. Das Portal ermöglicht aber keine programmgesteuerte Überprüfung des Zustands. 

Der Zustand der Datenbank für horizontale Hochskalierungsvorgänge kann nicht mit dem Azure-Portal überprüft werden.

So überprüfen Sie den Zustand von DWU-Änderungen:

1. Stellen Sie eine Verbindung mit der Masterdatenbank mit Ihrem logischen SQL-Datenbankserver her.
2. Übermitteln Sie die folgende Abfrage, um den Datenbankzustand zu überprüfen.


```sql
SELECT    *
FROM      sys.databases
;
```

3. Übermitteln der folgenden Abfrage zum Überprüfen des Betriebszustands

```sql
SELECT    *
FROM      sys.dm_operation_status
WHERE     resource_type_desc = 'Database'
AND       major_resource_id = 'MySQLDW'
;
```

Diese DMV gibt Informationen zu verschiedenen Verwaltungsvorgängen in Ihrer SQL Data Warehouse-Instanz zurück. Dazu gehören etwa der Vorgang und der Status des Vorgangs, der IN_PROGRESS oder COMPLETED lauten kann.

## <a name="the-scaling-workflow"></a>Der Skalierungsworkflow

Wenn Sie einen Skalierungsvorgang einleiten, beendet das System zuerst alle geöffneten Sitzungen und führt ein Rollback aller offenen Transaktionen aus, um einen konsistenten Zustand zu gewährleisten. Bei Skalierungsvorgängen erfolgt die Skalierung nur ein Mal, nachdem das Rollback der Transaktionen abgeschlossen ist.  

- Für eine zentrale Hochskalierung stellt das System die zusätzliche Computeressourcen bereit und führt dann erneut eine Anfügung an die Speicherebene aus. 
- Für eine zentrale Herunterskalierung werden die nicht benötigten Knoten vom Speicher getrennt und an die verbleibenden Knoten erneut angefügt.

## <a name="next-steps"></a>Nächste Schritte
Informationen zu zusätzlichen zentralen Leistungskonzepten finden Sie in den folgenden Artikeln:

* [Workload- und Parallelitätsverwaltung][Workload and concurrency management]
* [Tabellenentwurf – Übersicht][Table design overview]
* [Tabellenverteilung][Table distribution]
* [Tabellenindizierung][Table indexing]
* [Tabellenpartitionierung][Table partitioning]
* [Tabellenstatistiken][Table statistics]
* [Best Practices][Best practices]

<!--Image reference-->

<!--Article references-->

[capacity limits]: ./sql-data-warehouse-service-capacity-limits.md


[Check database state with T-SQL]: ./sql-data-warehouse-manage-compute-tsql.md#check-database-state-and-operation-progress
[Check database state with PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#check-database-state
[Check database state with REST APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#check-database-state

[Workload and concurrency management]: ./sql-data-warehouse-develop-concurrency.md
[Table design overview]: ./sql-data-warehouse-tables-overview.md
[Table distribution]: ./sql-data-warehouse-tables-distribute.md
[Table indexing]: ./sql-data-warehouse-tables-index.md
[Table partitioning]: ./sql-data-warehouse-tables-partition.md
[Table statistics]: ./sql-data-warehouse-tables-statistics.md
[Best practices]: ./sql-data-warehouse-best-practices.md
[development overview]: ./sql-data-warehouse-overview-develop.md

[SQL DB Contributor]: ../active-directory/role-based-access-built-in-roles.md#sql-db-contributor

<!--MSDN references-->
[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx

<!--Other Web references-->
[Azure portal]: http://portal.azure.com/
