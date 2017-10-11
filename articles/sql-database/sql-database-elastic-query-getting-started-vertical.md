---
title: "Erste Schritte mit datenbankübergreifende Abfragen, die (vertikale Partitionierung) | Microsoft Docs"
description: Verwendung von elastischen Datenbankabfrage mit vertikal partitionierten Datenbanken
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
ms.assetid: e5b44b10-c432-4f96-b20e-08615ff4d5dd
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2016
ms.author: torsteng
ms.openlocfilehash: 17158c4960e9ba9251524659c90af9aec1316774
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-cross-database-queries-vertical-partitioning-preview"></a>Erste Schritte mit datenbankübergreifende Abfragen, die (vertikale Partitionierung) (Vorschau)
Elastische Datenbankabfrage (Vorschau) für Azure SQL-Datenbank können Sie T-SQL-Abfragen ausführen, die mehrere Datenbanken, die mit einem einzelnen Verbindungspunkt umfassen. Dieses Thema gilt für [vertikal partitioniert Datenbanken](sql-database-elastic-query-vertical-partitioning.md).  

Abschließend wird: Informationen zum Konfigurieren und Verwenden einer Azure SQL-Datenbank zum Ausführen von Abfragen, die mehrere verbundene Datenbanken umfassen. 

Weitere Informationen zur Funktion für elastische Datenbanken Abfrage finden Sie unter [Übersicht zu Azure SQL-Datenbank elastischen Datenbank Abfragen](sql-database-elastic-query-overview.md). 

## <a name="prerequisites"></a>Voraussetzungen

Sie müssen die ALTER ANY EXTERNAL DATA SOURCE-Berechtigung besitzen. Diese Berechtigung ist im Lieferumfang der ALTER DATABASE-Berechtigung. ALTER ANY EXTERNAL DATA SOURCE-Berechtigungen sind erforderlich, um auf der zugrunde liegenden Datenquelle zu verweisen.

## <a name="create-the-sample-databases"></a>Erstellen Sie die Beispieldatenbanken
Zum Einstieg müssen zum Erstellen von zwei Datenbanken **Kunden** und **Aufträge**, entweder in die gleichen oder einem anderen logischen Server.   

Führen Sie die folgenden Abfragen auf die **Aufträge** Datenbank zum Erstellen der **Bestellinformationen** Tabelle, und geben Sie die Beispieldaten. 

    CREATE TABLE [dbo].[OrderInformation]( 
        [OrderID] [int] NOT NULL, 
        [CustomerID] [int] NOT NULL 
        ) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (123, 1) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (149, 2) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (857, 2) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (321, 1) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (564, 8) 

Jetzt, führen Sie folgende Abfrage für die **Kunden** Datenbank zum Erstellen der **CustomerInformation** Tabelle, und geben Sie die Beispieldaten. 

    CREATE TABLE [dbo].[CustomerInformation]( 
        [CustomerID] [int] NOT NULL, 
        [CustomerName] [varchar](50) NULL, 
        [Company] [varchar](50) NULL 
        CONSTRAINT [CustID] PRIMARY KEY CLUSTERED ([CustomerID] ASC) 
    ) 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (1, 'Jack', 'ABC') 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (2, 'Steve', 'XYZ') 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (3, 'Lylla', 'MNO') 

## <a name="create-database-objects"></a>Erstellen von Datenbankobjekten
### <a name="database-scoped-master-key-and-credentials"></a>Datenbankweit gültige Hauptschlüssel und Anmeldeinformationen
1. Öffnen Sie SQL Server Management Studio oder SQL Server Datatools in Visual Studio.
2. Herstellen einer Verbindung mit der Orders-Datenbank, und führen Sie die folgenden T-SQL-Befehle:
   
        CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>'; 
        CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred 
        WITH IDENTITY = '<username>', 
        SECRET = '<password>';  
   
    "Benutzername" und "Kennwort" Benutzername und Kennwort verwendet werden soll, die Anmeldung in der Customers-Datenbank.
    Authentifizierung mit Azure Active Directory mit flexible Abfragen wird derzeit nicht unterstützt.

### <a name="external-data-sources"></a>Externe Datenquellen
Um eine externe Datenquelle zu erstellen, führen Sie den folgenden Befehl für die Orders-Datenbank: 

    CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc WITH 
        (TYPE = RDBMS, 
        LOCATION = '<server_name>.database.windows.net', 
        DATABASE_NAME = 'Customers', 
        CREDENTIAL = ElasticDBQueryCred, 
    ) ;

### <a name="external-tables"></a>Externe Tabellen
Erstellen Sie eine externe Tabelle, für die Orders-Datenbank, die die Definition der Tabelle CustomerInformation entspricht:

    CREATE EXTERNAL TABLE [dbo].[CustomerInformation] 
    ( [CustomerID] [int] NOT NULL, 
      [CustomerName] [varchar](50) NOT NULL, 
      [Company] [varchar](50) NOT NULL) 
    WITH 
    ( DATA_SOURCE = MyElasticDBQueryDataSrc) 

## <a name="execute-a-sample-elastic-database-t-sql-query"></a>Führen Sie eine Beispielabfrage für elastische Datenbanken T-SQL
Nachdem Sie Ihre externen Tabellen und der externen Datenquelle definiert haben jetzt können T-SQL Sie die externe Tabellen abzufragen. Führen Sie diese Abfrage für die Orders-Datenbank: 

    SELECT OrderInformation.CustomerID, OrderInformation.OrderId, CustomerInformation.CustomerName, CustomerInformation.Company 
    FROM OrderInformation 
    INNER JOIN CustomerInformation 
    ON CustomerInformation.CustomerID = OrderInformation.CustomerID 

## <a name="cost"></a>Kosten
Derzeit ist die Funktion der elastischen Datenbank Abfrage in die Kosten der Azure SQL-Datenbank enthalten.  

Weitere Informationen zur Preisgestaltung finden Sie unter [SQL-Datenbank – Preise](https://azure.microsoft.com/pricing/details/sql-database). 

## <a name="next-steps"></a>Nächste Schritte

* Einen Überblick über die flexible Abfrage finden Sie unter [elastische Abfrageübersicht](sql-database-elastic-query-overview.md).
* Syntax und Beispiel-Abfragen für vertikal partitionierten Daten, finden Sie unter [vertikal Abfragen partitionierte Daten)](sql-database-elastic-query-vertical-partitioning.md)
* Ein Lernprogramm horizontale Partitionierung (Sharding) finden Sie unter [erste Schritte mit flexible Abfrage für die horizontale Partitionierung (Sharding)](sql-database-elastic-query-getting-started.md).
* Syntax und Beispiel Abfragen von horizontal partitionierten Daten, finden Sie unter [Abfragen von horizontal partitionierte Daten)](sql-database-elastic-query-horizontal-partitioning.md)
* Finden Sie unter [sp\_ausführen \_remote](https://msdn.microsoft.com/library/mt703714) für eine gespeicherte Prozedur, die auf einem einzelnen Azure SQL-Remotedatenbank oder eine Gruppe von Datenbanken, die als Shards in einem horizontalen Partitionierungsschema für eine Transact-SQL-Anweisung ausgeführt wird.