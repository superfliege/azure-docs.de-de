---
title: Azure SQL Data Warehouse – Versionshinweise Juni 2018 | Microsoft-Dokumentation
description: Versionshinweise zu Azure SQL Data Warehouse
services: sql-data-warehouse
author: twounder
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 07/23/2018
ms.author: twounder
ms.reviewer: twounder
ms.openlocfilehash: 5f4d39c6aa1a5c2c30e84fbf26535fe3ee7799a6
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2018
ms.locfileid: "39216708"
---
# <a name="whats-new-in-azure-sql-data-warehouse-july-2018"></a>Neuerungen in Azure SQL Data Warehouse Juli 2018
Azure SQL Data Warehouse wird fortlaufend verbessert. In diesem Artikel informieren wir Sie über neue Features und Änderungen, die im Juli 2018 eingeführt wurden.


## <a name="finer-granularity-for-cross-region-and-server-restores"></a>Feinere Granularität für regionen- und serverübergreifende Wiederherstellungen
Sie können jetzt regionen- und serverübergreifende Wiederherstellungen über jeden beliebigen Wiederherstellungspunkt durchführen anstatt georedundante Sicherungen auszuwählen, die alle 24 Stunden ausgeführt werden. Regionen- und serverübergreifende Wiederherstellungen werden sowohl für benutzerdefinierte als auch für automatische Wiederherstellungspunkte unterstützt, die eine feinere Granularität ermöglichen und so zusätzlichen Datenschutz bieten. Mit mehreren verfügbaren Wiederherstellungspunkten können Sie sicher sein, dass Ihr Data Warehouse bei der regionsübergreifenden Wiederherstellung logisch konsistent ist.

![Wiederherstellungsbefehl – Azure SQL Data Warehouse](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/6ac23972-9ec0-4502-ab10-7b6bc1a3d947.png)
![Wiederherstellungsoptionen – Azure SQL Data Warehouse](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/6c63bd0e-9c52-414d-b4be-d3bd3774ee08.png)

Weitere Informationen finden Sie im Blogbeitrag [Accelerated and Flexible Restore Points (Beschleunigte und flexible Wiederherstellungspunkte)](https://azure.microsoft.com/blog/accelerated-and-flexible-restore-points-with-sql-data-warehouse/).

## <a name="20-minute-restorations"></a>20-minütige Wiederherstellungen
SQL Data Warehouse ermöglicht jetzt die Wiederherstellung für jedes Data Warehouse in **weniger als 20 Minuten** innerhalb der gleichen Region unabhängig von der Datenbankgröße. Die Wiederherstellungsdauer gilt für Wiederherstellungen auf demselben oder einem anderen logischen Server in derselben Region. Darüber hinaus wurde der Prozess für Momentaufnahmen verbessert, um die Zeitspanne zum Erstellen eines Wiederherstellungspunkts zu verkürzen. In niedrigeren Leistungsstufen (niedrigere DWU-Einstellungen) besteht die Verbesserung in einer *2-fachen Verringerung* der Zeit zum Erstellen einer Momentaufnahme.

Weitere Informationen finden Sie im Blogbeitrag [Accelerated and Flexible Restore Points (Beschleunigte und flexible Wiederherstellungspunkte)](https://azure.microsoft.com/blog/accelerated-and-flexible-restore-points-with-sql-data-warehouse/).

## <a name="custom-restoration-configurations"></a>Benutzerdefinierte Wiederherstellungskonfigurationen
Sie können jetzt bei Wiederherstellungen im Azure-Portal die Leistungsstufe (DWU) ändern. Außerdem ist es möglich, die Wiederherstellung auf ein auf Gen2 aktualisiertes Data Warehouse auszuführen. Durch die Wiederherstellung auf eine Gen2-Instanz können Sie nun die Auswirkungen von Gen2 auswerten bevor Sie [ein Upgrade Ihres Gen1-Data Warehouse](https://docs.microsoft.com/azure/sql-data-warehouse/upgrade-to-latest-generation) durchführen.

![Benutzerdefinierte Wiederherstellungskonfiguration – Azure SQL Data Warehouse](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/f4c410c7-8515-409c-a983-0976792b8628.png)

## <a name="spdescribeundeclaredparameters"></a>SP_DESCRIBE_UNDECLARED_PARAMETERS
Die gespeicherte Prozedur [Sp_describe_undeclared_parameters](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-describe-undeclared-parameters-transact-sql) wird häufig von Tools verwendet, um Metadaten zu Parametern in einem Transact-SQL-Batch abzurufen. Die Prozedur gibt für jeden Parameter im Batch eine Zeile mit den abgeleiteten Typinformationen für den entsprechenden Parameter an. 

```sql
EXEC sp_describe_undeclared_parameters
    @tsql = 
    N'SELECT
        object_id, name, type_desc
      FROM
        sys.indexes
      WHERE
        object_id = @id;'
```

Das Resultset enthält Metadaten zum Parameter `@id` (Teilergebnisse werden angezeigt)
```
parameter_ordinal | name | suggested_system_type_id | suggested_system_type_name
--------------------------------------------------------------------------------
1                 | @id  | 56                       | int
```