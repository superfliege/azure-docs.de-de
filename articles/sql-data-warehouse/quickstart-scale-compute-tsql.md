---
title: 'Schnellstart: Horizontales Hochskalieren von Computeressourcen in Azure SQL Data Warehouse – T-SQL | Microsoft-Dokumentation'
description: Skalieren von Computeressourcen durch Anpassen der DWUs mithilfe von T-SQL-Befehlen
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: ''
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 03/16/2018
ms.author: elbutter;barbkess
ms.openlocfilehash: 1591192c72f5bf201dbbef80acc5895c8324fca4
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="quickstart-scale-compute-in-azure-sql-data-warehouse-using-t-sql"></a>Schnellstart: Skalieren von Computeressourcen in Azure SQL Data Warehouse mithilfe von T-SQL

Skalieren Sie Computeressourcen in Azure SQL Data Warehouse mithilfe von T-SQL und SQL Server Management Studio (SSMS). [Skalieren Sie Computeressourcen zentral hoch](sql-data-warehouse-manage-compute-overview.md), um eine bessere Leistung zu erzielen, oder skalieren Sie sie zentral herunter, um Kosten einzusparen. 

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="before-you-begin"></a>Voraussetzungen

Laden Sie die aktuelle Version von [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms.md) (SSMS) herunter, und installieren Sie sie.

Dabei wird vorausgesetzt, dass Sie den [Schnellstart: Erstellen und Abfragen einer Azure SQL Data Warehouse-Instanz im Azure-Portal](create-data-warehouse-portal.md) abgeschlossen haben. Nachdem Sie den Schnellstart „Erstellen und Abfragen einer Azure SQL Data Warehouse-Instanz im Azure-Portal“ absolviert haben, wissen Sie, wie Sie eine Verbindung herstellen, ein Data Warehouse mit dem Namen **mySampleDataWarehouse** erstellen sowie eine Firewallregel erstellen, die unserem Client den Zugriff auf den installierten Server gestattet.
 
## <a name="create-a-data-warehouse"></a>Erstellen eines Data Warehouse

Verwenden Sie den [Schnellstart: Erstellen und Abfragen einer Azure SQL Data Warehouse-Instanz im Azure-Portal](create-data-warehouse-portal.md), um ein Data Warehouse namens **mySampleDataWarehouse** zu erstellen. Schließen Sie den Schnellstart ab, um sicherzustellen, dass Sie über eine Firewallregel verfügen und mit Ihrem Data Warehouse in SQL Server Management Studio eine Verbindung herstellen können.

## <a name="connect-to-the-server-as-server-admin"></a>Herstellen einer Verbindung mit dem Server als Serveradministrator

In diesem Abschnitt wird [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms.md) zum Herstellen einer Verbindung mit Ihrem Azure SQL-Server verwendet.

1. Öffnen Sie SQL Server Management Studio.

2. Geben Sie im Dialogfeld **Mit Server verbinden** die folgenden Informationen ein:

   | Einstellung       | Empfohlener Wert | BESCHREIBUNG | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | Servertyp | Datenbank-Engine | Dieser Wert ist erforderlich. |
   | Servername | Der vollqualifizierte Servername | Beispiel: **mynewserver-20171113.database.windows.net**. |
   | Authentifizierung | SQL Server-Authentifizierung | In diesem Tutorial ist die SQL-Authentifizierung der einzige konfigurierte Authentifizierungstyp. |
   | Anmeldung | Das Serveradministratorkonto | Hierbei handelt es sich um das Konto, das Sie beim Erstellen des Servers angegeben haben. |
   | Password | Das Kennwort für das Serveradministratorkonto | Hierbei handelt es sich um das Kennwort, das Sie beim Erstellen des Servers angegeben haben. |

    ![Verbindung mit dem Server herstellen](media/load-data-from-azure-blob-storage-using-polybase/connect-to-server.png)

4. Klicken Sie auf **Verbinden**. Die Objekt-Explorer-Fenster wird in SSMS geöffnet. 

5. Erweitern Sie im Objekt-Explorer die Option **Datenbanken**. Erweitern Sie dann **mySampleDatabase**, um die Objekte in der neuen Datenbank anzuzeigen.

    ![Datenbankobjekte](media/create-data-warehouse-portal/connected.png) 

## <a name="view-service-objective"></a>Anzeigen des Dienstziels
Die Einstellung des Dienstziels enthält die Anzahl der Data Warehouse-Einheiten für das Data Warehouse. 

So zeigen Sie die aktuellen Data Warehouse-Einheiten für Ihr Data Warehouse an

1. Erweitern Sie unterhalb der Verbindung mit **mynewserver 20171113.database.windows.net** den Knoten **Systemdatenbanken**.
2. Klicken Sie mit der rechten Maustaste auf **master** und dann auf **Neue Abfrage**. Ein neues Abfragefenster wird geöffnet.
3. Führen Sie die folgende Abfrage aus, um eine Auswahl aus der dynamischen Verwaltungssicht „sys.database_service_objectives“ zu treffen. 

    ```sql
    SELECT
        db.name [Database]
    ,   ds.edition [Edition]
    ,   ds.service_objective [Service Objective]
    FROM
        sys.database_service_objectives ds
    JOIN
        sys.databases db ON ds.database_id = db.database_id
    WHERE 
        db.name = 'mySampleDataWarehouse'
    ```

4. Die folgenden Ergebnisse zeigen, dass für **mySampleDataWarehouse** ein Dienstziel von DW400 besteht. 

    ![Anzeigen der aktuellen DWUs](media/quickstart-scale-compute-tsql/view-current-dwu.png)


## <a name="scale-compute"></a>Skalieren von Computeressourcen
Sie können in SQL Data Warehouse Computeressourcen durch Anpassen der Data Warehouse-Einheiten (DWUs) erhöhen oder verringern. Mit dem [Schnellstart: Erstellen und Abfragen einer Azure SQL Data Warehouse-Instanz im Azure-Portal](create-data-warehouse-portal.md) wurde **mySampleDataWarehouse** erstellt und mit 400 DWUs initialisiert. In den folgenden Schritten werden die DWUs für **mySampleDataWarehouse** angepasst.

So ändern Sie Data Warehouse-Einheiten

1. Klicken Sie mit der rechten Maustaste auf **master** und dann auf **Neue Abfrage**.
2. Mit der T-SQL-Anweisung [ALTER DATABASE](/sql/t-sql/statements/alter-database-azure-sql-database) ändern Sie das Dienstziel. Führen Sie die folgende Abfrage aus, um das Dienstziel in DW300 zu ändern. 

```Sql
ALTER DATABASE mySampleDataWarehouse
MODIFY (SERVICE_OBJECTIVE = 'DW300')
;
```

## <a name="check-data-warehouse-state"></a>Überprüfen des Data Warehouse-Zustands

Mit einem angehaltenen Data Warehouse kann über T-SQL keine Verbindung hergestellt werden. Den aktuellen Zustand des Data Warehouse können Sie mithilfe eines PowerShell-Cmdlets anzeigen. Ein entsprechendes Beispiel finden Sie unter [Schnellstart: Skalieren von Computeressourcen in Azure SQL Data Warehouse mithilfe von PowerShell](quickstart-scale-compute-powershell.md#check-data-warehouse-state). 

## <a name="check-operation-status"></a>Überprüfen des Vorgangsstatus

Um Informationen über verschiedene Verwaltungsvorgänge für SQL Data Warehouse zurückzugeben, führen Sie die folgende Abfrage in der DMV [dm_operation_status](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) aus. Beispielsweise werden der Vorgang und der Status des Vorgangs zurückgegeben, nämlich entweder IN_PROGRESS oder COMPLETED.

```sql
SELECT *
FROM
    sys.dm_operation_status
WHERE
    resource_type_desc = 'Database'
AND 
    major_resource_id = 'MySQLDW'
```


## <a name="next-steps"></a>Nächste Schritte
Sie haben nun erfahren, wie die Computeressourcen für Ihr Data Warehouse skaliert werden. Weitere Informationen zu Azure SQL Data Warehouse erhalten Sie im Tutorial zum Laden von Daten.

> [!div class="nextstepaction"]
>[Laden von Daten in ein SQL-Data Warehouse](load-data-from-azure-blob-storage-using-polybase.md)
