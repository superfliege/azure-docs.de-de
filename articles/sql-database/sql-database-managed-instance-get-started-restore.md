---
title: Wiederherstellen einer Sicherung in einer verwalteten Azure SQL-Datenbank-Instanz | Microsoft-Dokumentation
description: Stellen Sie mithilfe von SSMS eine Datenbanksicherung in einer verwalteten Azure SQL-Datenbank-Instanz wieder her.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: carlrab, bonova
manager: craigg
ms.date: 11/01/2018
ms.openlocfilehash: bc27ece2eddc842a81698aaa685cbe6d63c6a1df
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2018
ms.locfileid: "50912253"
---
# <a name="quickstart-restore-a-database-backup-to-an-azure-sql-database-managed-instance"></a>Schnellstart: Wiederherstellen einer Datenbanksicherung in einer verwalteten Azure SQL-Datenbank-Instanz

In dieser Schnellstartanleitung erfahren Sie mithilfe der Sicherungsdatei „Wide World Importers – Standard“, wie Sie eine in Azure Blob Storage gespeicherte Sicherung einer Datenbank in der verwalteten Instanz wiederherstellen. Die Vorgehensweise ist mit einer gewissen Ausfallzeit verbunden. 

> [!VIDEO https://www.youtube.com/embed/RxWYojo_Y3Q]

Ein Tutorial mit Verwendung des Azure Database Migration Service (DMS) für die Migration finden Sie unter [Migrieren von SQL Server zu einer verwalteten Azure SQL-Datenbank-Instanz](../dms/tutorial-sql-server-to-managed-instance.md). Die verschiedenen Migrationsmethoden werden unter [Migration einer SQL Server-Instanz zu einer verwalteten Azure SQL-Datenbank-Instanz](sql-database-managed-instance-migrate.md) behandelt.

## <a name="prerequisites"></a>Voraussetzungen

In dieser Schnellstartanleitung gilt Folgendes:
- Als Ausgangspunkt werden die Ressourcen verwendet, die in der Schnellstartanleitung [Erstellen einer verwalteten Instanz](sql-database-managed-instance-get-started.md) erstellt wurden.
- Erforderlich ist die neueste Version von [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS) auf Ihrem lokalen Clientcomputer.
- Erforderlich ist eine Verbindung zu Ihrer verwalteten Instanz über SQL Server Management Studio (SSMS). Die Verbindungsoptionen finden Sie in den folgenden Schnellstartanleitungen:
  - [Verbinden mit einer verwalteten Azure SQL-Datenbank-Instanz von einem virtuellen Azure-Computer](sql-database-managed-instance-configure-vm.md)
  - [Verbinden mit einer verwalteten Azure SQL-Datenbank-Instanz von einem lokalen Computer über eine Point-to-Site-Verbindung](sql-database-managed-instance-configure-p2s.md)
- Verwendet wird ein vorkonfiguriertes Azure Blob Storage-Konto mit der Sicherungsdatei „Wide World Importers – Standard“ (heruntergeladen von https://github.com/Microsoft/sql-server-samples/releases/download/wide-world-importers-v1.0/WideWorldImporters-Standard.bak)).

> [!NOTE]
> Weitere Informationen zum Sichern und Wiederherstellen einer SQL Server-Datenbank mithilfe von Azure Blob Storage und SAS (Shared Access Signature) finden Sie unter [SQL Server-Sicherung über URLs](sql-database-managed-instance-get-started-restore.md).

## <a name="restore-the-wide-world-importers-database-from-a-backup-file"></a>Wiederherstellen der Wide World Importers-Datenbank aus einer Sicherungsdatei

Verwenden Sie in SSMS die folgenden Schritte, um die Wide World Importers-Datenbank aus der Sicherungsdatei für Ihre verwaltete Instanz wiederherzustellen.

1. Öffnen Sie SQL Server Management Studio (SSMS), und stellen Sie eine Verbindung zu Ihrer verwalteten Instanz her.
2. Öffnen Sie in SSMS ein neues Abfragefenster.
3. Verwenden Sie das folgende Skript, um mit dem vorkonfigurierten Speicherkonto und dem SAS-Schlüssel Anmeldeinformationen in der verwalteten Instanz zu erstellen.

   ```sql
   CREATE CREDENTIAL [https://mitutorials.blob.core.windows.net/databases] 
   WITH IDENTITY = 'SHARED ACCESS SIGNATURE'
   , SECRET = 'sv=2017-11-09&ss=bfqt&srt=sco&sp=rwdlacup&se=2028-09-06T02:52:55Z&st=2018-09-04T18:52:55Z&spr=https&sig=WOTiM%2FS4GVF%2FEEs9DGQR9Im0W%2BwndxW2CQ7%2B5fHd7Is%3D' 
   ```

    ![erstellen von anmeldeinformationen](./media/sql-database-managed-instance-get-started-restore/credential.png)

    > [!NOTE]
    > Entfernen Sie immer das führende Fragezeichen (**?**) aus dem generierten SAS-Schlüssel.
  
3. Verwenden Sie das folgende Skript, um die Gültigkeit der SAS-Anmeldeinformationen und der Sicherung zu prüfen. Geben Sie dazu die URL für den Container mit der Sicherungsdatei an:

   ```sql
   RESTORE FILELISTONLY FROM URL = 
      'https://mitutorials.blob.core.windows.net/databases/WideWorldImporters-Standard.bak'
   ```

    ![Dateiliste](./media/sql-database-managed-instance-get-started-restore/file-list.png)

4. Verwenden Sie das folgende Skript, um die Wide World Importers-Datenbank aus einer Sicherungsdatei wiederherzustellen. Geben Sie die URL für den Container mit der Sicherungsdatei an:

   ```sql
   RESTORE DATABASE [Wide World Importers] FROM URL =
     'https://mitutorials.blob.core.windows.net/databases/WideWorldImporters-Standard.bak'
   ```

    ![Wiederherstellen](./media/sql-database-managed-instance-get-started-restore/restore.png)

5. Führen Sie die folgende Abfrage in einer neuen Abfragesitzung aus, um den Status Ihrer Wiederherstellung nachzuverfolgen:

   ```sql
   SELECT session_id as SPID, command, a.text AS Query, start_time, percent_complete
      , dateadd(second,estimated_completion_time/1000, getdate()) as estimated_completion_time 
   FROM sys.dm_exec_requests r 
   CROSS APPLY sys.dm_exec_sql_text(r.sql_handle) a 
   WHERE r.command in ('BACKUP DATABASE','RESTORE DATABASE')`
   ```

6. Nachdem die Wiederherstellung abgeschlossen wurde, können Sie sie im Objekt-Explorer anzeigen. 

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Beheben von Problemen bei Sicherungen finden Sie unter [SQL Server-Sicherung über URLs – bewährte Methoden und Problembehandlung](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url-best-practices-and-troubleshooting).
- Eine Übersicht über die Verbindungsoptionen für Anwendungen finden Sie unter [Herstellen einer Verbindung zwischen einer Anwendung und einer verwalteten Azure SQL-Datenbank-Instanz](sql-database-managed-instance-connect-app.md).
- Informationen zur Abfrage mit einem Ihrer bevorzugten Tools oder Sprachen finden Sie unter [Herstellen einer Verbindung und Abfragen](sql-database-connect-query.md).
