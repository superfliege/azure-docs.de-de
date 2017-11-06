---
title: "Azure SQL Data Warehouse-Sicherungen – Momentaufnahmen (georedundant) | Microsoft-Dokumentation"
description: "Erfahren Sie, wie integrierte SQL Data Warehouse-Datenbanksicherungen Ihnen ermöglichen, ein Azure SQL Data Warehouse auf einen Wiederherstellungspunkt in einer anderen geografischen Region wiederherzustellen."
services: sql-data-warehouse
documentationcenter: 
author: barbkess
manager: jhubbard
editor: 
ms.assetid: b5aff094-05b2-4578-acf3-ec456656febd
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.custom: backup-restore
ms.date: 10/23/2017
ms.author: jrj;barbkess
ms.openlocfilehash: e76349ef7a2afa02d4f9e5295f299bb8084d1e08
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2017
---
# <a name="backup-and-restore-in-sql-data-warehouse"></a>Sicherung und Wiederherstellung in SQL Data Warehouse
Dieser Artikel erläutert die Merkmale von Sicherungen in SQL Data Warehouse. Verwenden von Data Warehouse Sicherungen zum Wiederherstellen einer Datenbankmomentaufnahme der primären Region oder Wiederherstellen einer Geosicherung für Ihre geografisch gepaarte Region. 

## <a name="what-is-a-data-warehouse-backup"></a>Was ist eine Data Warehouse-Sicherung?
Bei einer Data Warehouse-Sicherung handelt es sich eine Kopie Ihrer Datenbank, die Sie verwenden können, um ein Data Warehouse wiederherzustellen.  Da SQL Data Warehouse ein verteiltes System ist, besteht eine Data Warehouse-Sicherung aus zahlreichen Dateien, die in Azure Storage gespeichert sind. Eine Data Warehouse-Sicherung umfasst die lokalen Datenbankmomentaufnahmen und Geosicherungen aller Datenbanken und Dateien, die einem Data Warehouse zugeordnet sind. 

## <a name="local-snapshot-backups"></a>Sicherungen einer lokalen Momentaufnahme
SQL Data Warehouse erstellt über den Tag verteilte Momentaufnahmen des Data Warehouse. Momentaufnahmen stehen sieben Tage lang zur Verfügung. SQL Data Warehouse unterstützt eine RPO (Recovery Point Objective) von acht Stunden. Sie können das Data Warehouse in der primären Region anhand einer beliebigen Momentaufnahme wiederherstellen, die in den vergangenen sieben Tagen erstellt wurde.

Führen Sie die folgende Abfrage in Ihrer SQL Data Warehouse-Online-Instanz aus, um zu ermitteln, wann die letzte Momentaufnahme gestartet wurde. 

```sql
select   top 1 *
from     sys.pdw_loader_backup_runs 
order by run_id desc
;
```

## <a name="geo-backups"></a>Geosicherungen
SQL Data Warehouse führt ein Mal pro Tag für ein [geografisch gepaartes Datencenter](../best-practices-availability-paired-regions.md) eine Geosicherung aus. Die RPO für eine Geowiederherstellung beträgt 24 Stunden. Sie können die Geosicherung auf dem Server in der geografisch gepaarten Region wiederherstellen. Die Geosicherung stellt sicher, dass Sie ein Data Warehouse wiederherstellen können, wenn Sie nicht auf die Momentaufnahmen in Ihrer primären Region zugreifen können.

Geosicherungen sind standardmäßig aktiviert. Wenn Ihr Data Warehouse für Elastizität optimiert ist, können Sie diese Option [abwählen](https://docs.microsoft.com/powershell/resourcemanager/Azurerm.sql/v2.1.0/Set-AzureRmSqlDatabaseGeoBackupPolicyredirectedfrom=msdn), wenn Sie dies wünschen. Sie können Geosicherungen nicht abwählen, wenn Sie die Leistungsebene „Optimiert für Compute“ verwenden.

## <a name="backup-costs"></a>Sicherungskosten
Beachten Sie, dass die Azure-Rechnung einen Eintrag für Azure Storage Premium und einen Eintrag für georedundanten Speicher aufweist. Die Storage Premium-Gebühren sind die Gesamtkosten für das Speichern Ihrer Daten in der primären Region. Dies schließt Momentaufnahmen ein.  Die Gebühr für georedundanten Speicher umfasst die Kosten für das Speichern von Geosicherungen.  

Die Gesamtkosten für Ihr primäres Data Warehouse und sieben Tage Azure Blob-Momentaufnahmen werden auf die nächsten TB aufgerundet. Wenn Ihr Data Warehouse z.B. 1,5 TB umfasst und die Momentaufnahmen 100 GB verwenden, werden Ihnen 2 TB Daten zu Azure Storage Premium-Sätzen in Rechnung gestellt. 

> [!NOTE]
> Jede Momentaufnahme ist anfangs leer und wächst, wenn Sie Änderungen am primären Data Warehouse vornehmen. Alle Momentaufnahmen werden größer, wenn das Data Warehouse geändert wird. Daher steigen auch die Speicherkosten für Momentaufnahmen entsprechend der Änderungsrate.
> 
> 

Bei Verwendung von georedundantem Speicher wird Ihnen dieser separat in Rechnung gestellt. Der georedundante Speicher wird zum Standardsatz für georedundanten Speicher mit Lesezugriff (Read-Access Geographically Redundant Storage, RA-GRS) berechnet.

Weitere Informationen zu den Preisen für SQL Data Warehouse finden Sie in der [Preisübersicht für SQL Data Warehouse](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

## <a name="snapshot-retention-when-a-data-warehouse-is-paused"></a>Momentaufnahmenbeibehaltung, wenn ein Datawarehouse angehalten wird
Während ein Data Warehouse angehalten ist, erstellt SQL Data Warehouse keine Momentaufnahmen, und Momentaufnahmen laufen nicht ab. Das Alter der Momentaufnahmen ändert sich nicht, während das Data Warehouse angehalten ist. Die Aufbewahrung einer Momentaufnahme basiert auf der Anzahl von Tagen, die das Data Warehouse online ist, nicht auf Kalendertagen.

Wenn eine Momentaufnahme z.B. am 1. Oktober um 16 Uhr gestartet wurde und das Data Warehouse am 3. Oktober um 16 Uhr angehalten wird, sind die Momentaufnahmen bis zu zwei Tage alt. Wenn das Data Warehouse erneut online geschaltet wird, ist die Momentaufnahme zwei Tage alt. Wenn das Data Warehouse am 5. Oktober um 16 Uhr wieder online geschaltet wird, ist die Momentaufnahme zwei Tage alt und wird weitere fünf Tage lang aufbewahrt.

Wenn das Data Warehouse wieder online geschaltet wird, setzt SQL Data Warehouse die Erstellung neuer Momentaufnahmen fort, und Momentaufnahmen mit Daten, die älter sind als sieben Tage, laufen ab.

## <a name="can-i-restore-a-dropped-data-warehouse"></a>Kann ich ein getrenntes Data Warehouse wiederherstellen?
Wenn Sie ein Data Warehouse trennen, erstellt SQL Data Warehouse eine letzte Momentaufnahme und speichert diese sieben Tage lang. Sie können das Data Warehouse anhand des letzten Wiederherstellungspunkts wiederherstellen, der zum Zeitpunkt der Löschung erstellt wurde. 

> [!IMPORTANT]
> Wenn Sie eine logische SQL Server-Instanz löschen, werden auch alle Datenbanken der Instanz gelöscht und können nicht wiederhergestellt werden. Es ist nicht möglich, einen gelöschten Server wiederherzustellen.
> 

## <a name="next-steps"></a>Nächste Schritte
Informationen zum Wiederherstellen von SQL Data Warehouse finden Sie unter [Wiederherstellen einer SQL Data Warehouse-Instanz](sql-data-warehouse-restore-database-overview.md).

Eine Übersicht zum Thema Geschäftskontinuität finden Sie unter [Übersicht über die Geschäftskontinuität](../sql-database/sql-database-business-continuity.md).
