---
title: Wiederherstellen einer Azure SQL Data Warehouse-Instanz (REST-API) | Microsoft-Dokumentation
description: Stellen Sie eine Azure SQL Data Warehouse-Instanz mithilfe von REST-APIs wieder her.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 71236afd3f72497887f42667f971539ed294bef1
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2018
ms.locfileid: "43307535"
---
# <a name="restore-an-azure-sql-data-warehouse-with-rest-apis"></a>Wiederherstellen einer Azure SQL Data Warehouse-Instanz mit REST-APIs
Stellen Sie eine Azure SQL Data Warehouse-Instanz mithilfe von REST-APIs wieder her.

## <a name="before-you-begin"></a>Voraussetzungen
**Überprüfen Sie Ihre DTU-Kapazität.** Jedes SQL Data Warehouse wird von einer logischen SQL Server-Instanz gehostet (z.B. myserver.database.windows.net), die über ein Standard-[DTU-Kontingent](../sql-database/sql-database-what-is-a-dtu.md) verfügt.  Bevor Sie ein SQL Data Warehouse wiederherstellen können, überprüfen Sie, ob Ihre SQL Server-Instanz über genügend verbleibendes DTU-Kontingent für die Datenbank-Wiederherstellung verfügt. Um weitere DTUs anzufordern, können Sie [ein Supportticket erstellen](sql-data-warehouse-get-started-create-support-ticket.md).

## <a name="restore-an-active-or-paused-data-warehouse"></a>Wiederherstellen eines aktiven oder angehaltenen Data Warehouse
So stellen Sie ein Data Warehouse wieder her

1. Rufen Sie die Liste mit den Wiederherstellungspunkten für die Datenbank ab, indem Sie den Get Database Restore Points-Vorgang verwenden.
2. Beginnen Sie die Wiederherstellung mit dem Vorgang [Datenbankwiederherstellungsanforderung erstellen](https://msdn.microsoft.com/library/azure/dn509571.aspx) .
3. Verfolgen Sie den Status der Wiederherstellung mithilfe des Vorgangs [Datenbank-Betriebsstatus](https://msdn.microsoft.com/library/azure/dn720371.aspx) nach.

> [!NOTE]
> Nach Abschluss der Wiederherstellung können Sie Ihre wiederhergestellte Data Warehouse-Instanz konfigurieren. Befolgen Sie hierzu die Anleitung [Konfigurieren der Datenbank nach der Wiederherstellung](../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery).
> 
> 

## <a name="restore-a-deleted-data-warehouse"></a>Wiederherstellen einer gelöschten Data Warehouse-Instanz
So stellen Sie eine gelöschte Data Warehouse-Instanz wieder her

1. Listen Sie alle wiederherstellbaren gelöschten Data Warehouses mithilfe des Vorgangs [Auflisten wiederherstellbarer gelöschter Datenbanken](https://msdn.microsoft.com/library/azure/dn509562.aspx) auf.
2. Rufen Sie die Details des gelöschten Data Warehouse, das wiederhergestellt werden soll, mithilfe des Vorgangs [Wiederherstellbare gelöschte Datenbank abrufen][Wiederherstellbare gelöschte Datenbank abrufen] ab.
3. Beginnen Sie die Wiederherstellung mit dem Vorgang [Datenbankwiederherstellungsanforderung erstellen](https://msdn.microsoft.com/library/azure/dn509571.aspx) .
4. Verfolgen Sie den Status der Wiederherstellung mithilfe des Vorgangs [Datenbank-Betriebsstatus](https://msdn.microsoft.com/library/azure/dn720371.aspx) nach.

> [!NOTE]
> Informationen zum Konfigurieren des Data Warehouse nach Abschluss der Wiederherstellung finden Sie unter [Konfigurieren der Datenbank nach der Wiederherstellung](../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery).
> 
> 

## <a name="next-steps"></a>Nächste Schritte
Informationen zu den Geschäftskontinuitätsfunktionen von Azure SQL-Datenbank-Editionen finden Sie in der [Azure SQL-Datenbank-Übersicht zur Geschäftskontinuität](../sql-database/sql-database-business-continuity.md).
