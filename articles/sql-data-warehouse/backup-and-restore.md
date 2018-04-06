---
title: Azure SQL Data Warehouse-Sicherung und -Wiederherstellung – Momentaufnahmen (georedundant) | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sicherungen und Wiederherstellungen in Azure SQL Data Warehouse funktionieren. Verwenden Sie Data Warehouse-Sicherungen, um Ihr Data Warehouse auf einen Wiederherstellungspunkt in der primären Region wiederherzustellen, oder verwenden Sie georedundante Sicherungen für eine Wiederherstellung in einer anderen geografischen Region.
services: sql-data-warehouse
documentationcenter: ''
author: barbkess
manager: jhubbard
editor: ''
ms.assetid: b5aff094-05b2-4578-acf3-ec456656febd
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.custom: backup-restore
ms.date: 03/22/2018
ms.author: jrj;barbkess
ms.openlocfilehash: e909cb6f31d8bc677d9dfd267dab242eb99f42df
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="backup-and-restore-in-azure-sql-data-warehouse"></a>Sicherung und Wiederherstellung in Azure SQL Data Warehouse
Es wird beschrieben, wie Sicherungen und Wiederherstellungen in Azure SQL Data Warehouse funktionieren. Verwenden Sie Data Warehouse-Sicherungen, um Ihr Data Warehouse auf einen Wiederherstellungspunkt in der primären Region wiederherzustellen, oder verwenden Sie georedundante Sicherungen für eine Wiederherstellung in einer anderen geografischen Region. 

## <a name="what-is-backup-and-restore"></a>Was ist die Sicherung und Wiederherstellung?
Bei einer *Data Warehouse-Sicherung* handelt es sich um eine Kopie Ihrer Datenbank, die Sie verwenden können, um ein Data Warehouse wiederherzustellen.  Da SQL Data Warehouse ein verteiltes System ist, besteht eine Data Warehouse-Sicherung aus zahlreichen Dateien, die in Azure Storage gespeichert sind. Eine Data Warehouse-Sicherung umfasst die lokalen Datenbankmomentaufnahmen und Geosicherungen aller Datenbanken und Dateien, die einem Data Warehouse zugeordnet sind. 

Eine *Data Warehouse-Wiederherstellung* ist ein neues Data Warehouse, das aus einer Sicherung eines vorhandenen oder gelöschten Data Warehouse erstellt wird. Das wiederhergestellte Data Warehouse erstellt das gesicherte Data Warehouse zu einem bestimmten Zeitpunkt neu. Die Wiederherstellung Ihrer Data Warehouse-Instanz ist ein wesentlicher Bestandteil jeder Strategie für Geschäftskontinuität und Notfallwiederherstellung, da Ihre Daten nach versehentlichen Beschädigungen oder Löschungen neu erstellt werden.

Sowohl lokale als auch geografische Wiederherstellungen sind Teil der Funktionen für die Notfallwiederherstellung von SQL Data Warehouse. 

## <a name="local-snapshot-backups"></a>Sicherungen einer lokalen Momentaufnahme
Sicherungen einer lokalen Momentaufnahme sind ein integriertes Feature des Diensts.  Sie können das Feature aber auch deaktivieren. 

SQL Data Warehouse erstellt über den Tag verteilte Momentaufnahmen des Data Warehouse. Momentaufnahmen stehen sieben Tage lang zur Verfügung. SQL Data Warehouse unterstützt eine RPO (Recovery Point Objective) von acht Stunden. Sie können das Data Warehouse in der primären Region anhand einer beliebigen Momentaufnahme wiederherstellen, die in den vergangenen sieben Tagen erstellt wurde.

Führen Sie die folgende Abfrage in Ihrer SQL Data Warehouse-Online-Instanz aus, um zu ermitteln, wann die letzte Momentaufnahme gestartet wurde. 

```sql
select   top 1 *
from     sys.pdw_loader_backup_runs 
order by run_id desc
;
```

### <a name="snapshot-retention-when-a-data-warehouse-is-paused"></a>Momentaufnahmenbeibehaltung, wenn ein Datawarehouse angehalten wird
Während ein Data Warehouse angehalten ist, erstellt SQL Data Warehouse keine Momentaufnahmen, und Momentaufnahmen laufen nicht ab. Das Alter der Momentaufnahmen ändert sich nicht, während das Data Warehouse angehalten ist. Die Aufbewahrung einer Momentaufnahme basiert auf der Anzahl von Tagen, die das Data Warehouse online ist, nicht auf Kalendertagen.

Wenn eine Momentaufnahme z.B. am 1. Oktober um 16 Uhr gestartet wurde und das Data Warehouse am 3. Oktober um 16 Uhr angehalten wird, sind die Momentaufnahmen bis zu zwei Tage alt. Wenn das Data Warehouse erneut online geschaltet wird, ist die Momentaufnahme zwei Tage alt. Wenn das Data Warehouse am 5. Oktober um 16 Uhr wieder online geschaltet wird, ist die Momentaufnahme zwei Tage alt und wird weitere fünf Tage lang aufbewahrt.

Wenn das Data Warehouse wieder online geschaltet wird, setzt SQL Data Warehouse die Erstellung neuer Momentaufnahmen fort, und Momentaufnahmen mit Daten, die älter sind als sieben Tage, laufen ab.

### <a name="snapshot-retention-when-a-data-warehouse-is-dropped"></a>Aufbewahrung von Momentaufnahmen, wenn ein Data Warehouse gelöscht wird
Wenn Sie ein Data Warehouse trennen, erstellt SQL Data Warehouse eine letzte Momentaufnahme und speichert diese sieben Tage lang. Sie können das Data Warehouse anhand des letzten Wiederherstellungspunkts wiederherstellen, der zum Zeitpunkt der Löschung erstellt wurde. 

> [!IMPORTANT]
> Wenn Sie eine logische SQL Server-Instanz löschen, werden auch alle Datenbanken der Instanz gelöscht und können nicht wiederhergestellt werden. Es ist nicht möglich, einen gelöschten Server wiederherzustellen.
> 

## <a name="geo-backups"></a>Geosicherungen
SQL Data Warehouse führt ein Mal pro Tag für ein [geografisch gepaartes Datencenter](../best-practices-availability-paired-regions.md) eine Geosicherung aus. Die RPO für eine Geowiederherstellung beträgt 24 Stunden. Sie können die Geosicherung auf dem Server in der geografisch gepaarten Region wiederherstellen. Mit einer Geosicherung wird sichergestellt, dass Sie ein Data Warehouse wiederherstellen können, wenn Sie nicht auf die Momentaufnahmen in Ihrer primären Region zugreifen können.

Geosicherungen sind standardmäßig aktiviert. Wenn Ihr Data Warehouse für Elastizität optimiert ist, können Sie diese Option [abwählen](/powershell/module/azurerm.sql/set-azurermsqldatabasegeobackuppolicy), wenn Sie dies wünschen. Sie können Geosicherungen nicht abwählen, wenn Sie die Leistungsebene „Optimiert für Compute“ verwenden.

## <a name="backup-costs"></a>Sicherungskosten
Beachten Sie, dass die Azure-Rechnung einen Eintrag für Azure Storage Premium und einen Eintrag für georedundanten Speicher aufweist. Die Storage Premium-Gebühren sind die Gesamtkosten für das Speichern Ihrer Daten in der primären Region. Dies schließt Momentaufnahmen ein.  Die Gebühr für georedundanten Speicher umfasst die Kosten für das Speichern von Geosicherungen.  

Die Gesamtkosten für Ihr primäres Data Warehouse und sieben Tage Azure Blob-Momentaufnahmen werden auf die nächsten TB aufgerundet. Wenn Ihr Data Warehouse z.B. 1,5 TB umfasst und die Momentaufnahmen 100 GB verwenden, werden Ihnen 2 TB Daten zu Azure Storage Premium-Sätzen in Rechnung gestellt. 

> [!NOTE]
> Jede Momentaufnahme ist anfangs leer und wächst, wenn Sie Änderungen am primären Data Warehouse vornehmen. Alle Momentaufnahmen werden größer, wenn das Data Warehouse geändert wird. Daher steigen auch die Speicherkosten für Momentaufnahmen entsprechend der Änderungsrate.
> 
> 

Bei Verwendung von georedundantem Speicher wird Ihnen dieser separat in Rechnung gestellt. Der georedundante Speicher wird zum Standardsatz für georedundanten Speicher mit Lesezugriff (Read-Access Geographically Redundant Storage, RA-GRS) berechnet.

Weitere Informationen zu den Preisen für SQL Data Warehouse finden Sie in der [Preisübersicht für SQL Data Warehouse](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

## <a name="restoring-from-restore-points"></a>Wiederherstellen anhand von Wiederherstellungspunkten
Jede Momentaufnahme ist ein Wiederherstellungspunkt, der den Zeitpunkt zu Beginn der Momentaufnahme darstellt. Um ein Data Warehouse wiederherzustellen, wählen Sie einen Wiederherstellungspunkt aus und geben einen Wiederherstellungsbefehl aus.  

SQL Data Warehouse stellt die Sicherung stets in einem neuen Data Warehouse wieder her. Sie können entweder das wiederhergestellte Data Warehouse und das aktuelle beibehalten oder eines davon löschen. Wenn Sie die aktuelle Data Warehouse-Instanz durch die wiederhergestellte Data Warehouse-Instanz ersetzen möchten, können Sie sie über [ALTER DATABASE (Azure SQL Data Warehouse)](/sql/t-sql/statements/alter-database-azure-sql-data-warehouse) mit der Option MODIFY NAME umbenennen. 

Informationen zur Wiederherstellung einer Data Warehouse-Instanz finden Sie unter [Wiederherstellen einer Azure SQL Data Warehouse-Instanz (Portal)](sql-data-warehouse-restore-database-portal.md), [Wiederherstellen einer Azure SQL Data Warehouse-Instanz (PowerShell)](sql-data-warehouse-restore-database-powershell.md) oder [Wiederherstellen einer Azure SQL Data Warehouse-Instanz (REST-API)](sql-data-warehouse-restore-database-rest-api.md).

Zum Wiederherstellen einer gelöschten oder angehaltenen Data Warehouse-Instanz können Sie [ein Supportticket erstellen](sql-data-warehouse-get-started-create-support-ticket.md). 


## <a name="geo-redundant-restore"></a>Georedundante Wiederherstellung
Sie können Ihr Data Warehouse in einer beliebigen Region wiederherstellen, die Azure SQL Data Warehouse auf der gewählten Leistungsebene unterstützt. 

> [!NOTE]
> Zum Ausführen einer georedundanten Wiederherstellung müssen Sie dieses Feature nicht deaktiviert haben.
> 
> 

## <a name="restore-timeline"></a>Zeitachse der Wiederherstellung
Sie können eine Datenbank auf jeden verfügbaren Wiederherstellungspunkt innerhalb der letzten sieben Tage wiederherstellen. Momentaufnahmen werden alle vier bis acht Stunden gestartet und sind sieben Tage lang verfügbar. Wenn eine Momentaufnahme älter als sieben Tage ist, läuft sie ab, und ihr Wiederherstellungspunkt ist nicht mehr verfügbar. 

Für eine angehaltene Data Warehouse-Instanz werden keine Sicherungsvorgänge durchgeführt. Wenn Ihre Data Warehouse-Instanz länger als sieben Tage angehalten wurde, sind keine Wiederherstellungspunkte mehr vorhanden. 

## <a name="restore-costs"></a>Kosten für die Wiederherstellung
Die Speichergebühren für das wiederhergestellte Data Warehouse werden zum Azure Storage Premium-Tarif abgerechnet. 

Wenn Sie ein wiederhergestelltes Data Warehouse anhalten, wird Ihnen der Speicher zum Azure Storage Premium-Tarif in Rechnung gestellt. Der Vorteil des Anhaltens besteht darin, dass Ihnen die Computingressourcen nicht berechnet werden.

Weitere Informationen zu den Preisen für SQL Data Warehouse finden Sie in der [Preisübersicht für SQL Data Warehouse](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

## <a name="restore-use-cases"></a>Anwendungsfälle für die Wiederherstellung
Der vorrangige Einsatzbereich für die Data Warehouse-Wiederherstellung ist das Wiederherstellen von Daten nach versehentlichen Datenverlusten oder -beschädigungen. Mit der Data Warehouse-Wiederherstellung können Sie auch eine Sicherung für mehr als sieben Tage beibehalten. Nachdem die Sicherung wiederhergestellt wurde, ist das Data Warehouse online, und Sie können es unbegrenzt anhalten, um Computekosten zu sparen. Für die angehaltene Datenbank fallen Speichergebühren nach dem Azure Storage Premium-Tarif an. 

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Planen der Notfallwiederherstellung finden Sie unter [Übersicht über die Geschäftskontinuität mit Azure SQL-Datenbank](../sql-database/sql-database-business-continuity.md).
